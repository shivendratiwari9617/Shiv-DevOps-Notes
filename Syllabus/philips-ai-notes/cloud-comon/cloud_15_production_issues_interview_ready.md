# 15 Production Issues Across AWS, GCP, and Azure — Interview-Ready Answers

This file rewrites the cloud production issues into **final interview-style answers**.  
Each example is structured so you can speak it in an interview:

1. **What I would say**
2. **Real failure boundary**
3. **Redacted log / trace evidence**
4. **How I would fix it**
5. **How I would prevent it**

> **Source policy used here:** the explanations are based on widely seen production patterns, and the references section points to **open-source documentation or open-source project docs** only.

---

## 1) AWS — EKS service looks healthy, but ALB returns 502/503

### What I would say
If pods look healthy but the ALB returns `502` or `503`, I would assume the break is at the **ingress-to-service handoff**, not in business logic. My first checks are the ALB target group health, Ingress annotations, Service port mapping, readiness endpoint, and whether the ALB is probing the same path the app actually exposes. In production, this is a classic false-positive case where Kubernetes says the workload is fine, but the real user path is broken.

### Real failure boundary
The failure is usually one of these:
- ALB health check hits `/health`, but the app only exposes `/readyz`
- Ingress target type or Service port is wrong
- backend pod is ready internally, but not reachable through Service/Ingress
- security group path is broken between ALB and backend
- TLS or host-header behavior changed at the edge

### Redacted log / trace evidence
```text
# ALB access log
https 2026-10-04T08:22:11.412Z app/prod-api-alb 203.0.113.10:443 10.0.21.18:32044 0.001 0.002 0.000 503 503 0 57 "GET https://api.example.com:443/health HTTP/1.1" "ELB-HealthChecker/2.0"

# aws-load-balancer-controller
{"level":"error","msg":"targetGroup health check failed","targetGroupArn":"arn:aws:elasticloadbalancing:...:targetgroup/prod-api","reason":"Health checks failed with these codes: [404]"}

# Pod log
GET /readyz 200
GET /health 404
```

### How I would fix it
I would align the ALB health check with the endpoint the app actually exposes, verify the Service `port` and `targetPort`, and confirm whether the Ingress is using `ip` or `instance` mode correctly.

### How I would prevent it
I would standardize one health endpoint convention, keep ingress templates platform-owned, and gate rollouts on the **real external endpoint**, not just `kubectl rollout status`.

---

## 2) AWS — CI/CD can no longer assume the deploy role after OIDC trust-policy change

### What I would say
If deployments suddenly start failing on `AssumeRoleWithWebIdentity`, I would not debug Terraform or Helm first. I would reconstruct the **identity chain**: CI issuer, token claims, trust policy conditions, target role, and any SCP or permission boundary. In production, this kind of failure often comes from a repo rename, branch restriction change, or OIDC condition mismatch rather than a cloud outage.

### Real failure boundary
The actual break is usually:
- `sub` claim no longer matches the repo or branch
- `aud` claim mismatch
- trust policy tightened but CI token shape changed
- SCP blocks the role assumption
- pipeline session tags required but not passed

### Redacted log / trace evidence
```text
An error occurred (AccessDenied) when calling the AssumeRoleWithWebIdentity operation:
Not authorized to perform sts:AssumeRoleWithWebIdentity

CloudTrail:
eventSource=sts.amazonaws.com
eventName=AssumeRoleWithWebIdentity
errorCode=AccessDenied
requestParameters.roleArn=arn:aws:iam::123456789012:role/prod-deploy-role
additionalEventData.sub=repo:org/api-service:ref:refs/heads/main
```

### How I would fix it
I would compare the live OIDC token claims with the trust policy conditions, correct the repo or branch match, and verify no SCP is silently denying the call.

### How I would prevent it
I would version-control trust policies, keep repo-to-role mapping documented, and add a non-prod preflight stage that validates role assumption before the real deployment starts.

---

## 3) AWS — Terraform import succeeded, but the next plan wants to replace production resources

### What I would say
If `terraform import` succeeded but the next `plan` wants to destroy and recreate production resources, I would stop immediately and treat it as a **state-versus-reality mismatch**. My first goal is not cleanup or refactor; it is getting to a no-op or tightly explained plan. I would compare the resource address, provider alias, immutable attributes, imported state, and live cloud object before applying anything.

### Real failure boundary
This usually means:
- import went to the wrong module path
- wrong provider alias or wrong account/region
- code contains immutable settings that differ from live infra
- live defaults or tags are missing from config
- code models future desired state rather than current real state

### Redacted log / trace evidence
```text
# terraform plan
-/+ resource "aws_db_instance" "prod" {
      identifier              = "payments-prod-db"
    ~ instance_class          = "db.r6g.large" -> "db.r6g.xlarge"
    ~ parameter_group_name    = "custom-pg-1" -> "default.postgres15"
    ~ storage_encrypted       = true -> false # forces replacement
  }

Plan: 1 to add, 0 to change, 1 to destroy.
```

### How I would fix it
I would use `terraform state show`, compare it to the live AWS API output, correct the provider context, and remove or defer any immutable field changes until the imported object is stable in code.

### How I would prevent it
Import by logical stack, freeze manual changes during import, and require the first post-import plan to be no-op before refactoring module structure.

---

## 4) AWS — RDS is healthy, but application latency spikes after a deploy

### What I would say
If RDS looks healthy but the app gets slower after a release, I would assume the issue is **application connection behavior or query shape**, not “RDS is down.” I would inspect connection-pool wait time, slow queries, retry behavior, and whether the new release changed concurrency or query plans.

### Real failure boundary
Typical root causes:
- pool exhaustion after concurrency increase
- slow query or missing index
- retry storm amplifying DB load
- ORM change causing N+1 queries
- auth/TLS or SG path changed during deploy

### Redacted log / trace evidence
```text
trace_id=7f0e2f3f4f5a4a1d
span=POST /checkout duration_ms=8421
  -> span=db.acquire_connection wait_ms=5100
  -> span=db.query "select ... from orders ..." duration_ms=3012
error=timeout waiting for connection from pool

RDS metrics:
DatabaseConnections=1498
FreeableMemory=low
CPUUtilization=38%
```

### How I would fix it
I would stop retry amplification, identify the slow or regressed query, tune pool settings alongside application concurrency, and roll back if the release clearly changed query behavior.

### How I would prevent it
Track pool wait time, not just DB CPU. Also review query plans on critical endpoints and load-test with production-like connection settings.

---

## 5) AWS — S3 access fails after bucket policy or KMS change

### What I would say
If a service suddenly cannot read or write S3 objects, I would not stop at the IAM role. I would trace the **full authorization path**: IAM policy, bucket policy, KMS permissions, and any VPC endpoint policy. In production, storage outages are often policy-composition problems, not application bugs.

### Real failure boundary
The break is often:
- bucket policy denies the app role
- KMS key policy or grant blocks decrypt
- VPC endpoint policy is narrower than expected
- prefix restriction no longer matches object path

### Redacted log / trace evidence
```text
AccessDenied: Access Denied
status code: 403, request id: 5L9R2A..., host id: i0d3...

CloudTrail:
eventSource=s3.amazonaws.com
eventName=GetObject
errorCode=AccessDenied
resources[0].ARN=arn:aws:s3:::prod-uploads/customer/123/report.pdf

KMS:
eventSource=kms.amazonaws.com
eventName=Decrypt
errorCode=AccessDenied
```

### How I would fix it
I would identify the exact denied action and exact principal, then align IAM, bucket, KMS, and endpoint policy so the whole path is allowed.

### How I would prevent it
Add an integration test for upload/download paths and alert on denied decrypt operations for critical services.

---

## 6) GCP — GKE rollout is green, but public traffic still fails

### What I would say
If GKE says the rollout succeeded but users still fail, I would treat the problem as **load-balancer path health**, not “Kubernetes deployment success.” I would check the backend service, NEG membership, health-check path, Service port mapping, and firewall requirements for the load balancer path.

### Real failure boundary
Typical root causes:
- backend service health check hits the wrong path
- NEG is not populated as expected
- Service `port` and `targetPort` mismatch
- health check expects `200`, app returns `404`
- firewall path for health checks is incomplete

### Redacted log / trace evidence
```text
kubectl rollout status deployment/api
deployment "api" successfully rolled out

Load balancer backend health:
backend_service=prod-api-bs
status=UNHEALTHY
reason=Health check failed, expected HTTP 200 got 404

App logs:
GET /readyz 200
GET /healthz 404
```

### How I would fix it
I would align health-check path and port wiring, verify NEG registration, and confirm the backend service is probing the correct endpoint.

### How I would prevent it
Release gates should validate the **real public endpoint**, not just Kubernetes object health.

---

## 7) GCP — Cloud SQL works from an admin host, but not from the GKE workload

### What I would say
If Cloud SQL is reachable from a bastion or debug host but not from the workload, I would assume the problem is **identity, proxy, or private network path**, not the database itself. I would confirm whether the app uses private IP or Cloud SQL Auth Proxy, then verify Workload Identity, service-account roles, DNS, and routing.

### Real failure boundary
Common causes:
- Workload Identity binding broken
- Cloud SQL Auth Proxy cannot mint credentials
- app expects private IP, but VPC path is wrong
- service account lacks Cloud SQL client capability
- DNS or instance target is wrong

### Redacted log / trace evidence
```text
dial tcp 10.54.0.12:5432: i/o timeout

Cloud SQL Auth Proxy:
failed to refresh instance metadata: googleapi: Error 403:
The client is not authorized to make this request.

GKE identity:
principal=serviceAccount:proj.svc.id.goog[payments/api]
```

### How I would fix it
I would restore the right Workload Identity binding or Cloud SQL access role and verify the selected network path is the one the app is actually using.

### How I would prevent it
Use one standard connectivity model per environment and add a rollout precheck that verifies database connectivity from the workload identity.

---

## 8) GCP — Service account impersonation breaks after IAM cleanup

### What I would say
If deploy tooling or support automation suddenly starts failing after IAM cleanup, I would immediately check whether impersonation rights were removed. In GCP, a lot of production auth paths depend on `iam.serviceAccounts.getAccessToken` through service account impersonation.

### Real failure boundary
The break usually is:
- missing `roles/iam.serviceAccountTokenCreator`
- binding removed at project or folder level
- condition on the binding no longer matches
- caller identity changed after pipeline/workload refactor

### Redacted log / trace evidence
```text
ERROR: (gcloud.auth.print-access-token) PERMISSION_DENIED:
Permission 'iam.serviceAccounts.getAccessToken' denied on resource
'projects/-/serviceAccounts/prod-deploy@myproj.iam.gserviceaccount.com'
```

### How I would fix it
I would identify the caller, confirm the target service account, and restore the minimum impersonation permission on the correct scope rather than granting broad editor rights.

### How I would prevent it
Keep critical impersonation chains documented and validate auth paths in CI before deploy stages execute.

---

## 9) GCP — Pub/Sub subscriber lag explodes after a release

### What I would say
If Pub/Sub lag grows after a release, I would look at **per-message processing time and redelivery behavior** before adding workers blindly. Queue incidents usually come from slow handlers, downstream dependency latency, ack-deadline issues, or retries that cause replay storms.

### Real failure boundary
Typical causes:
- handler time increased
- downstream dependency got slower
- ack deadline too short
- concurrency increase overloaded a dependency
- failed messages are redelivered repeatedly

### Redacted log / trace evidence
```text
subscription=invoice-worker-sub
oldest_unacked_message_age=1840s
num_undelivered_messages=128400

worker logs:
trace_id=2ab4c...
processing message_id=913778 took 47.2s
error=deadline exceeded calling tax-service
message redelivered=true
```

### How I would fix it
I would reduce handler latency, correct retry behavior, tune concurrency carefully, and extend ack handling only if the processing pattern genuinely requires it.

### How I would prevent it
Track oldest unacked age, trace consumer spans into dependencies, and prefer backpressure over blind concurrency increases.

---

## 10) GCP — GCS access fails only in one environment

### What I would say
If the same application can access GCS in staging but fails in prod, I would compare the **exact runtime principal and exact bucket permission model** in each environment. These bugs are often caused by hidden inherited permissions in one environment and missing explicit permissions in another.

### Real failure boundary
Likely causes:
- wrong service account in prod
- bucket-level IAM differs from project-level expectation
- VPC Service Controls perimeter blocks access
- conditional binding excludes prod
- uniform bucket-level access changed the model

### Redacted log / trace evidence
```text
googleapi: Error 403: Access denied., forbidden

Audit log:
serviceName=storage.googleapis.com
methodName=storage.objects.get
principalEmail=api-prod@prod-project.iam.gserviceaccount.com
status.code=7
```

### How I would fix it
I would verify the prod service account actually running in the workload, then align bucket IAM or perimeter rules so prod has the exact object access it needs.

### How I would prevent it
Use explicit per-environment service accounts and test object operations as part of deployment verification.

---

## 11) Azure — AKS pods are healthy, but Application Gateway returns 502

### What I would say
If AKS workloads look healthy but Application Gateway returns `502`, I would treat it as an **edge health and backend wiring problem**. I would check Application Gateway backend health, AGIC configuration, probe path, host header expectations, Service `port` and `targetPort`, and network access from gateway to backend.

### Real failure boundary
Typical causes:
- probe path mismatch
- incorrect backend pool or port mapping
- host header assumptions
- NSG or subnet routing issue
- TLS mismatch between gateway and backend

### Redacted log / trace evidence
```text
ApplicationGatewayAccessLog:
clientIP=198.51.100.15 requestUri=/api/health httpStatus=502 backendStatusCode=0

AGIC logs:
backend health probe failed for pool=prod-api-pool path=/health expected_status=200

Pod logs:
GET /ready 200
GET /health 404
```

### How I would fix it
I would align probe path and backend port, then verify gateway-to-backend network reach and any host-header expectations in the app.

### How I would prevent it
Use shared ingress templates and gate releases on backend-health checks from the gateway layer, not just pod readiness.

---

## 12) Azure — Managed Identity token acquisition fails after RBAC cleanup

### What I would say
If an app loses access to Key Vault, Storage, or Azure SQL after RBAC cleanup, I would confirm the **managed identity attachment and scope** before blaming the service. This class of issue often happens because the right identity is no longer attached, or the role assignment exists at the wrong scope.

### Real failure boundary
The usual break is:
- system-assigned vs user-assigned identity confusion
- identity detached from compute resource
- role assignment removed or moved
- data-plane access missing even though control-plane role exists
- propagation delay misread as application failure

### Redacted log / trace evidence
```text
ManagedIdentityCredential authentication unavailable.
The requested identity has not been assigned to this resource.

App error:
Azure.RequestFailedException: Status: 403 (Forbidden)
```

### How I would fix it
I would verify which managed identity the workload should use, confirm it is attached, and then check that the right data-plane role exists at the correct scope.

### How I would prevent it
Keep an explicit identity inventory and include token-acquisition plus target-service access in deployment smoke tests.

---

## 13) Azure — Key Vault secret retrieval breaks after network lockdown

### What I would say
If the app can authenticate but cannot fetch secrets from Key Vault after a network change, I would suspect **private endpoint routing or DNS** before I blame identity. A lot of Key Vault outages are really name-resolution or private-link path failures.

### Real failure boundary
Common causes:
- private DNS resolves incorrectly
- private endpoint not approved or not healthy
- NSG blocks the subnet path
- Key Vault firewall tightened
- app still resolves the public endpoint

### Redacted log / trace evidence
```text
GET https://prod-kv.vault.azure.net/secrets/db-password?api-version=7.4
-> timeout after 30000ms

nslookup prod-kv.vault.azure.net
10.22.4.9

App error:
Azure.Security.KeyVault.Secrets: Retry failed after 4 tries.
SocketException: Name or service not known
```

### How I would fix it
I would verify DNS resolution from the workload subnet, check the private endpoint and firewall path, and confirm the application is using the intended private route.

### How I would prevent it
Every network lockdown change should include DNS validation and a synthetic secret-read check from the workload path.

---

## 14) Azure — Azure SQL is available, but p95 and p99 latency spike after a deploy

### What I would say
If Azure SQL is up but application latency increases after a release, I would look for **pool saturation, query-plan regression, and retry amplification** rather than treating it as a raw database outage. Availability is not the same as healthy application performance.

### Real failure boundary
Typical causes:
- connection-pool exhaustion
- query-plan regression after release or schema change
- retries multiplying pressure
- worker/session saturation
- deployment increased concurrency without retuning pool settings

### Redacted log / trace evidence
```text
trace_id=9cd1f...
span=GET /invoice/4821 duration_ms=6210
  -> sql.connection.acquire wait_ms=4020
  -> sql.query duration_ms=1810
error=timeout expired. The timeout period elapsed prior to obtaining a connection from the pool.

Azure SQL metrics:
cpu_percent=41
workers_percent=84
sessions_percent=92
```

### How I would fix it
I would inspect pool wait time, query regressions, and retry behavior, then tune application concurrency and roll back if the new release introduced the change.

### How I would prevent it
Track pool saturation in dashboards and review hot-path query behavior under production-like concurrency.

---

## 15) Cross-cloud — Pipeline is green, rollout is green, but production is broken

### What I would say
If the pipeline succeeded and Kubernetes says the rollout succeeded, but production is failing, I would say the release process validated **object state, not customer state**. I would reconstruct the change timeline from commit to deploy revision, then verify whether the same artifact digest was promoted, whether prod-only config or secrets changed, and whether ingress or dependency health degraded immediately after rollout.

### Real failure boundary
This usually means:
- tested artifact was not the same one promoted
- prod-only values changed behavior
- ingress/backend path broke after deploy
- dependency health degraded under real traffic
- deployment checks were too shallow

### Redacted log / trace evidence
```text
Pipeline:
Build succeeded
Image scan passed
helm upgrade --install api-prod succeeded

Kubernetes:
deployment.apps/api-prod successfully rolled out

Real traffic:
5xx_rate=18%
p95_latency=4.8s

Ingress logs:
upstream connect error or disconnect/reset before headers
```

### How I would fix it
I would compare commit SHA, image digest, Helm values, release revision, and telemetry around the rollout window. If the latest change is clearly causal and safe to reverse, I would roll back immediately.

### How I would prevent it
Use progressive delivery, verify the real customer path after each rollout, and treat “rollout success” as necessary but not sufficient.

---

# How to Speak These in an Interview

For each scenario, use this sequence:

1. **Start with the user symptom**
2. **Name the most likely failure boundary**
3. **List the first 3–5 checks in order**
4. **Use one log or trace to prove the direction**
5. **State the fix**
6. **End with one prevention improvement**

That answer shape sounds like someone who has actually run production.

---

# Open-Source References

These references are included to ground the concepts in **open-source documentation or open-source project docs**:

1. **Kubernetes documentation** — probes, Deployments, Services, Ingress, rollback behavior  
   - https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/  
   - https://kubernetes.io/docs/concepts/workloads/controllers/deployment/  
   - https://kubernetes.io/docs/concepts/services-networking/service/  
   - https://kubernetes.io/docs/concepts/services-networking/ingress/

2. **Kubernetes RBAC and NetworkPolicy**  
   - https://kubernetes.io/docs/reference/access-authn-authz/rbac/  
   - https://kubernetes.io/docs/concepts/services-networking/network-policies/

3. **Terraform language and import/state model**  
   - https://developer.hashicorp.com/terraform/language  
   - https://developer.hashicorp.com/terraform/cli/import  
   - https://developer.hashicorp.com/terraform/language/state

4. **AWS Load Balancer Controller documentation**  
   - https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/

5. **ingress-nginx documentation**  
   - https://kubernetes.github.io/ingress-nginx/

6. **Argo CD documentation**  
   - https://argo-cd.readthedocs.io/

7. **Prometheus documentation**  
   - https://prometheus.io/docs/introduction/overview/

8. **OpenTelemetry documentation**  
   - https://opentelemetry.io/docs/

9. **GitHub Actions OIDC hardening docs in open docs repository**  
   - https://docs.github.com/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect

10. **Cloud SQL Auth Proxy open-source repository**  
   - https://github.com/GoogleCloudPlatform/cloud-sql-proxy

11. **ExternalDNS documentation**  
   - https://kubernetes-sigs.github.io/external-dns/latest/

12. **Helm documentation**  
   - https://helm.sh/docs/

> The examples in this file are **representative redacted production patterns**, while the reference links point to open-source or open documentation for the underlying mechanisms.
