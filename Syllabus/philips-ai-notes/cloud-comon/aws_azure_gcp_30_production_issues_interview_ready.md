# 30 Production Issues and Troubleshooting Steps for AWS, Azure, and GCP — Interview-Ready Answers

This file is designed for **interview preparation** and **production troubleshooting thinking**.

It contains **30 concrete cloud production issues**:
- **10 AWS**
- **10 GCP**
- **10 Azure**

Each item is written as:

1. **Interview question**
2. **What I would say**
3. **Failure boundary**
4. **Troubleshooting steps**
5. **Redacted log / trace evidence**
6. **Fix**
7. **Prevention**
8. **Open-source references**

> The traces and logs are **realistic redacted examples** based on common production failure patterns. They are included to show evidence-led troubleshooting, not as vendor incident transcripts.

---

# AWS

## 1) ALB returns 502/503 even though EC2 or EKS pods look healthy

### Interview question
**Users get 502/503 from an AWS Application Load Balancer, but the service looks healthy internally. How would you troubleshoot it?**

### What I would say
If the ALB is returning `502` or `503`, I would assume the failure is at the **load balancer to backend path** before I blame the application. In production, this is often a health check path mismatch, wrong target port, TLS termination mismatch, or target registration issue. I would trace the request from ALB listener to target group health to backend listener and then compare that with what the app actually exposes.

### Failure boundary
Typical causes:
- health check path mismatch
- target group port mismatch
- backend not listening on the expected interface or port
- security group path broken
- TLS/host-header mismatch

### Troubleshooting steps
1. Check target group health and target registration.
2. Compare health check path and port to the application.
3. Verify security group rules between ALB and backend.
4. Confirm whether the backend is actually listening on the expected port.
5. Review ALB access logs and backend logs around the same timestamp.

### Redacted log / trace evidence
```text
ALB access log:
https 2026-10-04T08:22:11.412Z app/prod-api-alb 203.0.113.10:443 10.0.21.18:32044 0.001 0.002 0.000 503 503 0 57 "GET https://api.example.com:443/health HTTP/1.1" "ELB-HealthChecker/2.0"

Target health:
Target.ResponseCodeMismatch
expected=200 actual=404

Backend log:
GET /readyz 200
GET /health 404
```

### Fix
Align the target group health check path and backend port with the actual application endpoint and listener.

### Prevention
Standardize health endpoints and validate ALB target health in release verification.

### Open-source references
- AWS Load Balancer Controller docs: https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/
- Ingress NGINX concepts: https://kubernetes.github.io/ingress-nginx/

---

## 2) RDS is available, but application latency spikes

### Interview question
**RDS looks healthy from AWS metrics, but app latency and timeout errors increase. What would you do?**

### What I would say
I would treat this as an **application-to-database interaction problem**, not a simple RDS availability issue. In production, the database can be up while the app still fails because of pool exhaustion, query regression, retry storms, or connection spikes after a deploy. I would inspect connection acquisition time, slow queries, and the release timeline.

### Failure boundary
Typical causes:
- connection pool exhaustion
- slow query or bad query plan
- retry amplification
- missing index
- higher app concurrency after release

### Troubleshooting steps
1. Check app-side connection wait time.
2. Review RDS connections, latency, and memory.
3. Inspect slow query logs or query insights.
4. Compare traffic and release timing.
5. Check whether a retry loop is amplifying load.

### Redacted log / trace evidence
```text
trace_id=7f0e2f3f4f5a4a1d
span=POST /checkout duration_ms=8421
  -> span=db.acquire_connection wait_ms=5100
  -> span=db.query duration_ms=3012
error=timeout waiting for connection from pool

RDS metrics:
DatabaseConnections=1498
CPUUtilization=38%
FreeableMemory=low
```

### Fix
Reduce retry amplification, fix the slow query or pool sizing issue, and roll back the release if it introduced the regression.

### Prevention
Monitor pool wait time, not just DB CPU, and test high-concurrency release behavior before production rollout.

### Open-source references
- OpenTelemetry docs: https://opentelemetry.io/docs/
- Prometheus docs: https://prometheus.io/docs/introduction/overview/

---

## 3) S3 access fails after policy or KMS change

### Interview question
**An application suddenly cannot read or write S3 objects. How would you troubleshoot it?**

### What I would say
I would trace the full **authorization chain**. In S3 failures, it is usually not enough to check the IAM role alone. I would inspect IAM policy, bucket policy, KMS decrypt permission, and any VPC endpoint policy if private access is involved.

### Failure boundary
Typical causes:
- bucket policy deny
- KMS decrypt denied
- VPC endpoint policy too narrow
- object prefix restriction mismatch
- wrong role assumed

### Troubleshooting steps
1. Identify the exact principal making the call.
2. Check S3 bucket policy and object prefix rules.
3. Review KMS key policy and grant path.
4. Verify VPC endpoint policy if used.
5. Match CloudTrail or audit timestamps to app errors.

### Redacted log / trace evidence
```text
AccessDenied: Access Denied
status code: 403

CloudTrail:
eventSource=s3.amazonaws.com
eventName=GetObject
errorCode=AccessDenied
resource=arn:aws:s3:::prod-uploads/customer/123/report.pdf

KMS:
eventName=Decrypt
errorCode=AccessDenied
```

### Fix
Correct the full permissions chain so IAM, bucket policy, KMS policy, and endpoint policy all allow the intended object path.

### Prevention
Use policy-as-code review and add upload/download integration tests.

### Open-source references
- Terraform AWS provider docs: https://registry.terraform.io/providers/hashicorp/aws/latest/docs
- External Secrets Operator docs: https://external-secrets.io/latest/

---

## 4) ECS task keeps restarting after deployment

### Interview question
**An ECS service keeps replacing tasks after a new deployment. How would you debug it?**

### What I would say
I would determine whether the restart is caused by **application exit, health check failure, missing secret/config, or downstream dependency failure**. The fastest signals are task exit code, service events, and application startup logs.

### Failure boundary
Typical causes:
- invalid container command
- missing env var or secret
- container health check fails
- app cannot connect to a dependency
- CPU/memory pressure causes repeated exits

### Troubleshooting steps
1. Check ECS service events.
2. Inspect task stopped reason and exit code.
3. Review container startup logs.
4. Verify health check and container command.
5. Check task role, secrets, and dependency connectivity.

### Redacted log / trace evidence
```text
ECS service events:
(service payments) (task 8d7f...) failed container health checks

Task stopped reason:
Essential container in task exited

Container log:
FATAL: required env REDIS_URL is not set
```

### Fix
Restore the missing configuration or fix the health check and redeploy.

### Prevention
Validate required env/config before deployment and make startup failures obvious in logs.

### Open-source references
- OpenTelemetry docs: https://opentelemetry.io/docs/
- Prometheus docs: https://prometheus.io/docs/introduction/overview/

---

## 5) EKS rollout is green, but traffic still fails

### Interview question
**Kubernetes says the Deployment rolled out successfully in EKS, but users still get errors. What would you do?**

### What I would say
A green rollout only proves Kubernetes updated the objects. It does not prove the **customer path** works. I would inspect ingress health, service endpoints, readiness state, dependency health, and user-path telemetry before treating the rollout as successful.

### Failure boundary
Typical causes:
- readiness path mismatch
- Service endpoints missing
- ALB ingress health mismatch
- dependency failure after rollout
- prod-only config regression

### Troubleshooting steps
1. Verify Service endpoints exist.
2. Check readiness and ingress health.
3. Compare image digest and config revision.
4. Review error rate and latency around rollout time.
5. Roll back if the new revision is clearly causal.

### Redacted log / trace evidence
```text
kubectl rollout status deployment/api
deployment "api" successfully rolled out

ALB target health:
unhealthy reason=HTTP 404

App trace:
GET /checkout -> redis timeout -> 502
```

### Fix
Correct the broken readiness or dependency path and roll back if needed.

### Prevention
Use post-deploy checks against the real public endpoint, not only rollout status.

### Open-source references
- Kubernetes Deployments: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- AWS Load Balancer Controller docs: https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/

---

## 6) Lambda errors spike after a dependency or configuration change

### Interview question
**A Lambda function suddenly starts timing out or failing after a release. How would you troubleshoot it?**

### What I would say
I would separate **cold start behavior, dependency latency, permission errors, and code exceptions**. Lambda incidents are often blamed on cold starts, but in production the bigger problems are usually downstream timeouts, bad secret/config changes, or concurrency patterns.

### Failure boundary
Typical causes:
- downstream API latency
- secret/config change
- IAM permission denied
- timeout too low
- memory too low causing slow execution

### Troubleshooting steps
1. Review invocation errors, duration, and timeout patterns.
2. Inspect function logs and traces.
3. Compare config, secret, and dependency changes.
4. Check IAM permission failures.
5. Look at concurrent executions and downstream saturation.

### Redacted log / trace evidence
```text
REPORT RequestId: 1f1c... Duration: 3003.45 ms Billed Duration: 3000 ms
Task timed out after 3.00 seconds

Trace:
POST /invoice -> secret fetch ok -> tax-api timeout after 2500ms
```

### Fix
Increase timeout only if necessary, fix the slow dependency, and restore broken config or IAM permissions.

### Prevention
Trace downstream calls and keep Lambda timeout and memory aligned with real workload behavior.

### Open-source references
- OpenTelemetry Lambda docs: https://opentelemetry.io/docs/platforms/faas/
- Prometheus concepts: https://prometheus.io/docs/introduction/overview/

---

## 7) CloudWatch alarms fire, but on-call cannot isolate the failure quickly

### Interview question
**An alarm triggered, but the team struggled to isolate root cause. How would you improve troubleshooting?**

### What I would say
I would say the issue is not just alerting, it is **observability design**. A useful production signal should separate edge failure, application failure, and dependency failure. I would connect logs, metrics, and traces around a shared release marker or trace ID so the on-call engineer can follow a request path quickly.

### Failure boundary
Typical causes:
- too many symptom-only alerts
- no release annotations
- logs not correlated with trace IDs
- no dependency breakdown
- dashboards mix unrelated services

### Troubleshooting steps
1. Correlate the alert timestamp with recent deploys.
2. Check whether the error is edge, app, or dependency.
3. Follow one failing request trace end to end.
4. Compare structured logs with metric spikes.
5. Identify which signal should have been clearer.

### Redacted log / trace evidence
```text
Alert: prod-api 5xx_rate > 5%

Trace:
GET /checkout
  -> auth-service 200
  -> payments-db acquire_connection wait_ms=4100
  -> request failed 502
```

### Fix
Add release markers, end-to-end traces, and dashboards that separate upstream, app, and dependency health.

### Prevention
Design alerts around SLOs and failure boundaries, not just generic CPU or 5xx spikes.

### Open-source references
- OpenTelemetry docs: https://opentelemetry.io/docs/
- Prometheus alerting docs: https://prometheus.io/docs/alerting/latest/overview/

---

## 8) Auto Scaling Group replaces instances unexpectedly

### Interview question
**Instances in an Auto Scaling Group keep getting replaced. How would you diagnose it?**

### What I would say
I would identify whether the replacements are caused by **EC2 health, load balancer health, launch template issues, or application health checks**. In production, frequent replacement often hides a startup or registration problem rather than an EC2 hardware failure.

### Failure boundary
Typical causes:
- failed EC2 status checks
- failed ALB health checks
- bad launch template or user-data
- app never becomes healthy
- instance cannot reach required dependency during boot

### Troubleshooting steps
1. Check ASG activity history.
2. Review EC2 and ELB health status.
3. Inspect boot logs or cloud-init/user-data output.
4. Verify instance profile and startup dependency path.
5. Compare launch template version to last known good.

### Redacted log / trace evidence
```text
ASG activity:
Terminating EC2 instance: instance failed ELB health checks

/var/log/cloud-init-output.log
Starting api service...
api.service failed: missing /etc/app/config.yaml
```

### Fix
Correct the launch configuration, startup dependency, or health check expectations.

### Prevention
Version launch templates carefully and validate instance boot success before scaling out broadly.

### Open-source references
- cloud-init docs: https://cloudinit.readthedocs.io/
- Prometheus node exporter: https://github.com/prometheus/node_exporter

---

## 9) Route 53 DNS is correct, but users still hit the wrong environment

### Interview question
**DNS looks correct in Route 53, but some users still reach the wrong environment. How would you investigate it?**

### What I would say
I would treat this as a **DNS propagation, caching, split-horizon, or traffic policy issue**. Route 53 may be correct centrally, but clients or intermediate resolvers may still cache old answers or use a different view.

### Failure boundary
Typical causes:
- DNS TTL and resolver caching
- split-horizon/private-public record confusion
- weighted/latency routing misconfiguration
- stale CNAME chain
- client resolver caching

### Troubleshooting steps
1. Query authoritative and recursive resolvers separately.
2. Check TTL and record type chain.
3. Compare public and private hosted zones.
4. Confirm traffic policy or weighted routing behavior.
5. Inspect endpoint logs to see which hostname users actually hit.

### Redacted log / trace evidence
```text
dig api.example.com @ns-123.awsdns-45.net
api.example.com. 60 IN A 198.51.100.44

dig api.example.com @8.8.8.8
api.example.com. 300 IN A 203.0.113.18
```

### Fix
Correct the record set or routing policy and reduce TTL before cutovers.

### Prevention
Use lower TTL for planned changes and validate both authoritative and recursive resolution during incident response.

### Open-source references
- CoreDNS docs: https://coredns.io/manual/toc/
- ExternalDNS docs: https://kubernetes-sigs.github.io/external-dns/latest/

---

## 10) CloudFront serves stale or broken content after deployment

### Interview question
**A new frontend release is deployed, but users still get stale assets or broken pages through CloudFront. How would you troubleshoot it?**

### What I would say
I would suspect a **cache invalidation or immutable asset versioning issue**. In production, CDN incidents often happen because HTML and static assets are not versioned consistently, so clients receive a new HTML page that references old or missing files.

### Failure boundary
Typical causes:
- missing cache invalidation
- stale HTML referencing new asset names
- old assets evicted too early
- inconsistent Cache-Control headers
- deployment order problem between origin and CDN

### Troubleshooting steps
1. Check response headers and cache status.
2. Compare HTML references to actual asset object keys.
3. Verify invalidation behavior or asset versioning.
4. Inspect origin bucket or backend object availability.
5. Compare the deployment order of HTML vs assets.

### Redacted log / trace evidence
```text
curl -I https://app.example.com/
x-cache: Hit from cloudfront
cache-control: max-age=300

Browser console:
GET https://app.example.com/static/app.91ab.js 404
```

### Fix
Use immutable asset names, deploy assets before HTML, and invalidate only the mutable entry points.

### Prevention
Adopt content-hash asset versioning and validate release through the CDN path.

### Open-source references
- Vite deployment docs: https://vitejs.dev/guide/static-deploy.html
- NGINX caching docs: https://nginx.org/en/docs/

---

# GCP

## 11) GKE rollout succeeded, but external traffic still fails

### Interview question
**GKE says the rollout succeeded, but users still get errors. What would you troubleshoot first?**

### What I would say
I would assume the break is between the **Google load balancer and the backend service**, not in the deployment itself. I would verify backend health, NEG membership, Service port mapping, and the health check path before blaming the app.

### Failure boundary
Typical causes:
- wrong health check path
- NEG membership issue
- Service port mismatch
- firewall path problem
- app returns a non-200 on the probe path

### Troubleshooting steps
1. Check backend service health.
2. Verify NEG registration.
3. Compare health check path with the app.
4. Confirm Service `port`/`targetPort`.
5. Inspect app logs for probe requests.

### Redacted log / trace evidence
```text
deployment "api" successfully rolled out

Backend service:
status=UNHEALTHY
reason=Health check failed, expected HTTP 200 got 404

App logs:
GET /readyz 200
GET /healthz 404
```

### Fix
Align the health check path and backend wiring with the app’s actual endpoint.

### Prevention
Use a standard readiness/health path and verify the public endpoint after each rollout.

### Open-source references
- Kubernetes Services: https://kubernetes.io/docs/concepts/services-networking/service/
- Ingress concepts: https://kubernetes.io/docs/concepts/services-networking/ingress/

---

## 12) Cloud SQL works from a bastion, but not from the workload

### Interview question
**The database is reachable from an admin host, but the app still times out. How would you debug it?**

### What I would say
I would focus on **workload identity, proxy path, private IP routing, and DNS**, not the database engine first. In cloud environments, admin reachability proves very little about the actual workload path.

### Failure boundary
Typical causes:
- Workload Identity binding wrong
- Cloud SQL Auth Proxy auth fails
- private network path missing
- app points to the wrong DB host
- firewall or DNS issue

### Troubleshooting steps
1. Confirm whether the app uses private IP or Auth Proxy.
2. Check workload identity or service account bindings.
3. Verify connectivity from the workload namespace or pod.
4. Inspect proxy logs if present.
5. Check DNS resolution and target IP.

### Redacted log / trace evidence
```text
dial tcp 10.54.0.12:5432: i/o timeout

Cloud SQL Auth Proxy:
failed to refresh instance metadata: Error 403

principal=serviceAccount:proj.svc.id.goog[payments/api]
```

### Fix
Repair the workload identity or network path and verify the app uses the intended access method.

### Prevention
Use one standard database connectivity pattern per environment and test it in rollout verification.

### Open-source references
- Cloud SQL Auth Proxy repo: https://github.com/GoogleCloudPlatform/cloud-sql-proxy
- Kubernetes debugging pods: https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/

---

## 13) Pub/Sub lag explodes after a new release

### Interview question
**A Pub/Sub subscriber falls behind badly after deployment. How would you troubleshoot it?**

### What I would say
I would investigate **message processing time, ack behavior, and downstream dependencies** before increasing worker count. In production, lag often grows because the handler got slower or a dependency got worse, and adding workers just amplifies the pressure.

### Failure boundary
Typical causes:
- slower message handler
- downstream timeout
- too-short ack deadline
- retry storm
- concurrency increase overloaded a dependency

### Troubleshooting steps
1. Check oldest unacked message age.
2. Measure handler duration.
3. Inspect redelivery and retry rate.
4. Trace downstream calls from the subscriber.
5. Adjust concurrency only after understanding the bottleneck.

### Redacted log / trace evidence
```text
subscription=invoice-worker-sub
oldest_unacked_message_age=1840s
num_undelivered_messages=128400

worker:
processing message_id=913778 took 47.2s
error=deadline exceeded calling tax-service
message redelivered=true
```

### Fix
Reduce handler latency, tune ack behavior, and remove retry amplification.

### Prevention
Track queue lag and downstream dependency latency together.

### Open-source references
- OpenTelemetry docs: https://opentelemetry.io/docs/
- Prometheus docs: https://prometheus.io/docs/introduction/overview/

---

## 14) Service account impersonation breaks deployments

### Interview question
**Deployments fail because service account impersonation stopped working. How would you handle it?**

### What I would say
I would reconstruct the **caller-to-target identity chain**. In GCP, if impersonation breaks, the key question is which identity is calling, which service account it is trying to impersonate, and whether `serviceAccountTokenCreator` still exists at the right scope.

### Failure boundary
Typical causes:
- missing impersonation role
- IAM cleanup removed inherited binding
- conditional binding no longer matches
- wrong target service account

### Troubleshooting steps
1. Identify the caller identity.
2. Confirm the target service account.
3. Check impersonation roles and conditional bindings.
4. Compare working vs failing environment.
5. Re-test token generation before rerunning deploy.

### Redacted log / trace evidence
```text
PERMISSION_DENIED:
Permission 'iam.serviceAccounts.getAccessToken' denied on resource
'projects/-/serviceAccounts/prod-deploy@myproj.iam.gserviceaccount.com'
```

### Fix
Restore the minimum impersonation permission at the right scope for the correct caller.

### Prevention
Document impersonation chains and validate them in CI.

### Open-source references
- Terraform GCP provider docs: https://registry.terraform.io/providers/hashicorp/google/latest/docs
- OpenTelemetry docs: https://opentelemetry.io/docs/

---

## 15) GCS access works in staging but fails in prod

### Interview question
**The same application can access GCS in staging but gets 403 in production. How would you debug it?**

### What I would say
I would compare the **exact runtime principal and exact bucket authorization model** between staging and prod. Many environment-specific storage bugs happen because staging inherits broad project permissions while production requires explicit bucket-level permissions.

### Failure boundary
Typical causes:
- wrong service account in prod
- bucket IAM differs from expectation
- VPC Service Controls restriction
- conditional IAM mismatch
- uniform bucket-level access changed behavior

### Troubleshooting steps
1. Identify the service account actually running in prod.
2. Check bucket-level IAM and project-level assumptions.
3. Compare staging and prod bindings.
4. Verify perimeter or network restrictions.
5. Match audit logs to app errors.

### Redacted log / trace evidence
```text
googleapi: Error 403: Access denied., forbidden

Audit:
methodName=storage.objects.get
principalEmail=api-prod@prod-project.iam.gserviceaccount.com
status.code=7
```

### Fix
Grant the right bucket-level access to the exact prod principal and correct any perimeter restriction.

### Prevention
Use explicit per-environment principals and test real object operations during rollout.

### Open-source references
- Terraform GCP provider docs: https://registry.terraform.io/providers/hashicorp/google/latest/docs
- Prometheus docs: https://prometheus.io/docs/introduction/overview/

---

## 16) GKE pods are healthy, but DNS resolution fails intermittently

### Interview question
**Application calls sometimes fail because service names do not resolve inside GKE. How would you troubleshoot it?**

### What I would say
I would treat intermittent DNS failures as a **shared cluster dependency issue**. I would test DNS from affected pods, inspect CoreDNS behavior, and check whether query load, upstream DNS latency, or network policies are interfering.

### Failure boundary
Typical causes:
- CoreDNS overload
- upstream DNS timeouts
- NetworkPolicy blocks DNS
- excessive search-domain lookups
- node networking issue

### Troubleshooting steps
1. Reproduce lookup from an affected pod.
2. Check CoreDNS restarts and logs.
3. Inspect network policies for UDP/TCP 53.
4. Compare affected and healthy namespaces.
5. Check whether application resolver behavior caused query spikes.

### Redacted log / trace evidence
```text
nslookup redis.default.svc.cluster.local
;; connection timed out; no servers could be reached

coredns:
[ERROR] plugin/errors: 2 redis.default.svc.cluster.local. A: read udp 10.244.0.23:54213->10.96.0.10:53: i/o timeout
```

### Fix
Restore DNS reachability, scale CoreDNS if needed, and reduce query amplification.

### Prevention
Monitor DNS latency and error rate and ensure NetworkPolicies always allow DNS.

### Open-source references
- DNS for Services and Pods: https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/
- CoreDNS docs: https://coredns.io/manual/toc/

---

## 17) Cloud Run service latency spikes unexpectedly

### Interview question
**A Cloud Run service starts timing out or showing high latency even though deployments are normal. How would you investigate it?**

### What I would say
I would separate **cold starts, dependency latency, concurrency pressure, and configuration drift**. Cloud Run can look healthy from a deployment perspective while user-facing latency rises because too much concurrency hits a slow downstream or the service is underprovisioned.

### Failure boundary
Typical causes:
- too much concurrency per instance
- downstream API latency
- cold start amplification
- memory/CPU under-sizing
- bad config or secret rollout

### Troubleshooting steps
1. Compare latency increase with release timing.
2. Check request concurrency and instance scaling behavior.
3. Trace downstream calls.
4. Review timeout and resource settings.
5. Compare old and new config revisions.

### Redacted log / trace evidence
```text
request_id=ab17...
latency_ms=5921
revision=api-00042-xdf

trace:
GET /quote -> config load ok -> pricing-api timeout at 4800ms
```

### Fix
Tune concurrency and timeout settings, fix the slow downstream, and roll back bad config if necessary.

### Prevention
Set concurrency deliberately and validate downstream performance under burst traffic.

### Open-source references
- OpenTelemetry docs: https://opentelemetry.io/docs/
- Prometheus docs: https://prometheus.io/docs/introduction/overview/

---

## 18) GKE ingress serves 404 after an ingress rule change

### Interview question
**Users get 404 after an ingress change in GKE. What would you do first?**

### What I would say
I would debug from the **edge routing rule inward**. A 404 often means the host or path no longer matches what the ingress expects. I would inspect host rules, backend service mapping, path matching, and the actual request path seen in ingress logs.

### Failure boundary
Typical causes:
- wrong host rule
- path mismatch
- wrong backend service
- missing rewrite
- backend service has no endpoints

### Troubleshooting steps
1. Check ingress rules and backend services.
2. Compare request host/path with the manifest.
3. Review ingress or controller logs for unmatched routes.
4. Verify backend endpoints.
5. Check if multiple ingresses conflict.

### Redacted log / trace evidence
```text
ingress:
host=api.example.com path=/v1 backend=api-service:80

controller log:
GET /health host=api.example.com -> 404
reason=no route matched
```

### Fix
Correct the ingress rule or backend mapping and confirm endpoint availability.

### Prevention
Use validated ingress templates and smoke-test public routes after changes.

### Open-source references
- Kubernetes Ingress docs: https://kubernetes.io/docs/concepts/services-networking/ingress/
- ingress-nginx docs: https://kubernetes.github.io/ingress-nginx/

---

## 19) BigQuery job errors surge after schema or query change

### Interview question
**A production BigQuery pipeline starts failing after a release. How would you troubleshoot it?**

### What I would say
I would determine whether the break is **schema mismatch, query logic change, permissions, or quota/resource behavior**. In production data pipelines, many failures are not platform outages but compatibility mistakes between upstream schema and downstream query expectations.

### Failure boundary
Typical causes:
- schema mismatch
- field type change
- permission denied on dataset/table
- query exceeded resources
- partition filter missing

### Troubleshooting steps
1. Inspect the exact failed job error.
2. Compare current schema with previous successful schema.
3. Review recent query changes.
4. Check dataset/table permissions.
5. Inspect resource or quota-related warnings.

### Redacted log / trace evidence
```text
BigQuery job failed:
No matching signature for operator = for argument types: INT64, STRING at [14:9]

Pipeline log:
revision=etl-20261004-1
step=aggregate_daily_orders
```

### Fix
Correct the query or schema contract and redeploy the pipeline.

### Prevention
Use schema validation and contract testing between producing and consuming jobs.

### Open-source references
- dbt docs: https://docs.getdbt.com/
- Apache Airflow docs: https://airflow.apache.org/docs/

---

## 20) Memorystore or Redis-backed service times out after scale increase

### Interview question
**A cache-backed service starts timing out after traffic increases. How would you reason about it?**

### What I would say
I would verify whether the issue is **cache saturation, connection handling, hot keys, or network path**, not just “Redis is slow.” In production, a traffic increase often exposes connection storms or uneven key distribution before it looks like a platform outage.

### Failure boundary
Typical causes:
- too many client connections
- hot key or uneven access
- low timeout settings
- retries amplify pressure
- network path problem between app and cache

### Troubleshooting steps
1. Compare latency with traffic and deploy timing.
2. Check connection count and cache CPU/memory.
3. Identify hot keys or burst patterns.
4. Inspect client retry and timeout settings.
5. Trace app requests that depend on the cache.

### Redacted log / trace evidence
```text
trace_id=91ab...
GET /checkout duration_ms=3812
  -> redis connect wait_ms=1100
  -> redis GET cart:4983 timeout after 2000ms
```

### Fix
Tune client pooling and timeout behavior, reduce hot-key pressure, and scale or shard the cache if needed.

### Prevention
Track cache connection pressure and key distribution, not only overall cache health.

### Open-source references
- Redis docs: https://redis.io/docs/
- OpenTelemetry docs: https://opentelemetry.io/docs/

---

# Azure

## 21) AKS pods are healthy, but Application Gateway returns 502

### Interview question
**AKS workloads look healthy, but Application Gateway returns 502. How would you troubleshoot it?**

### What I would say
I would treat this as an **edge-to-backend path issue**. Healthy pods do not guarantee healthy ingress. I would inspect Application Gateway backend health, probe path, Service mapping, and network connectivity from the gateway subnet to the backend.

### Failure boundary
Typical causes:
- wrong probe path
- wrong backend port
- host-header mismatch
- AGIC config issue
- NSG path blocked

### Troubleshooting steps
1. Check Application Gateway backend health.
2. Compare probe path with app endpoint.
3. Verify Service port and targetPort.
4. Inspect AGIC logs and ingress config.
5. Check subnet NSG and route path.

### Redacted log / trace evidence
```text
ApplicationGatewayAccessLog:
requestUri=/api/health httpStatus=502 backendStatusCode=0

AGIC:
backend health probe failed path=/health expected=200

Pod log:
GET /ready 200
GET /health 404
```

### Fix
Align probe path and backend port and restore network path from Application Gateway to the pods or nodes.

### Prevention
Use shared ingress templates and validate backend health in rollout verification.

### Open-source references
- AGIC docs: https://azure.github.io/application-gateway-kubernetes-ingress/
- Kubernetes Services docs: https://kubernetes.io/docs/concepts/services-networking/service/

---

## 22) Managed Identity stops working after RBAC cleanup

### Interview question
**An app suddenly cannot access Key Vault or Storage after an RBAC cleanup. How would you diagnose it?**

### What I would say
I would confirm the **intended managed identity, the actual attached identity, and the scope of the role assignment**. In Azure, these incidents often happen because the correct identity is no longer attached or only control-plane access exists while data-plane access is missing.

### Failure boundary
Typical causes:
- wrong identity attached
- user-assigned identity detached
- role assignment removed
- role assigned at wrong scope
- data-plane role missing

### Troubleshooting steps
1. Identify the identity the workload should use.
2. Confirm that identity is attached.
3. Inspect role assignments at the correct scope.
4. Distinguish control-plane and data-plane permissions.
5. Match app errors to activity/audit records.

### Redacted log / trace evidence
```text
ManagedIdentityCredential authentication unavailable.
The requested identity has not been assigned to this resource.

App:
Status: 403 (Forbidden)
```

### Fix
Attach the correct identity and assign the required data-plane role at the proper scope.

### Prevention
Maintain an identity inventory and test token acquisition plus target access during deploy checks.

### Open-source references
- Azure SDK auth examples: https://github.com/Azure/azure-sdk-for-python
- OpenTelemetry docs: https://opentelemetry.io/docs/

---

## 23) Key Vault secret retrieval fails after network lockdown

### Interview question
**The app can authenticate but still cannot fetch secrets from Key Vault after a network change. What would you check?**

### What I would say
I would suspect **private endpoint routing or DNS** before I blame identity. In Azure, Key Vault often breaks after network hardening because the app resolves the wrong endpoint or the private path is not fully working.

### Failure boundary
Typical causes:
- private endpoint issue
- private DNS wrong
- Key Vault firewall tightened
- NSG blocks path
- app resolves public endpoint

### Troubleshooting steps
1. Check DNS resolution from the workload subnet.
2. Verify private endpoint state and approval.
3. Inspect Key Vault firewall settings.
4. Confirm NSG rules allow the path.
5. Test secret retrieval from the workload environment.

### Redacted log / trace evidence
```text
GET https://prod-kv.vault.azure.net/secrets/db-password?api-version=7.4
-> timeout after 30000ms

nslookup prod-kv.vault.azure.net
10.22.4.9

App error:
SocketException: Name or service not known
```

### Fix
Repair DNS and private endpoint routing and confirm the app uses the intended private path.

### Prevention
Validate DNS and secret-read behavior whenever network restrictions change.

### Open-source references
- Azure SDK repo: https://github.com/Azure/azure-sdk-for-python
- CoreDNS docs: https://coredns.io/manual/toc/

---

## 24) Azure SQL is up, but application latency becomes unacceptable

### Interview question
**Azure SQL shows as available, but app p95 latency grows sharply. How would you investigate it?**

### What I would say
I would look at **pool saturation, query regression, and retry amplification** before calling it a database outage. Production systems often fail because the app cannot get a connection quickly enough even while the database itself remains technically available.

### Failure boundary
Typical causes:
- pool exhaustion
- query regression
- session or worker saturation
- retry storm
- release increased concurrency

### Troubleshooting steps
1. Check connection acquisition time in traces.
2. Review Azure SQL worker/session pressure.
3. Compare recent release timing.
4. Inspect query regressions or plan changes.
5. Reduce retries and decide rollback vs fix-forward.

### Redacted log / trace evidence
```text
trace_id=9cd1f...
GET /invoice/4821 duration_ms=6210
  -> sql.connection.acquire wait_ms=4020
  -> sql.query duration_ms=1810
error=timeout prior to obtaining a connection from the pool

Azure SQL:
workers_percent=84
sessions_percent=92
```

### Fix
Tune pool and concurrency, fix query behavior, and roll back if the latest release caused the regression.

### Prevention
Monitor pool wait time and validate hot-path queries under production-like concurrency.

### Open-source references
- OpenTelemetry docs: https://opentelemetry.io/docs/
- Prometheus docs: https://prometheus.io/docs/introduction/overview/

---

## 25) AKS DNS resolution fails intermittently

### Interview question
**Applications in AKS intermittently fail to resolve service names. How would you troubleshoot it?**

### What I would say
I would treat DNS as a **cluster dependency** and verify whether the issue is CoreDNS health, upstream DNS latency, network policy, or node-level networking. Intermittent DNS errors often create misleading app-level symptoms.

### Failure boundary
Typical causes:
- CoreDNS overload
- upstream resolver issue
- network policy blocks DNS
- conntrack pressure
- node networking issue

### Troubleshooting steps
1. Reproduce DNS failure from an affected pod.
2. Check CoreDNS restarts and logs.
3. Confirm DNS traffic is allowed.
4. Compare affected node or namespace patterns.
5. Check whether query volume spiked recently.

### Redacted log / trace evidence
```text
nslookup postgres.db.svc.cluster.local
;; connection timed out; no servers could be reached

coredns:
[ERROR] plugin/errors: read udp 10.244.1.9:52731->10.0.0.10:53: i/o timeout
```

### Fix
Restore DNS path, scale CoreDNS, and reduce query amplification where needed.

### Prevention
Monitor DNS latency and error rate and keep DNS explicitly allowed in policies.

### Open-source references
- DNS docs: https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/
- CoreDNS docs: https://coredns.io/manual/toc/

---

## 26) Azure DevOps deployment is green, but the app is broken in prod

### Interview question
**The deployment pipeline succeeded, but production traffic fails immediately afterward. How would you respond?**

### What I would say
I would say the pipeline proved **artifact delivery**, not service health. I would correlate the deployment timestamp with customer-facing error rate, ingress errors, dependency health, and the exact image or config revision that went live. If the release is clearly causal, I would roll back quickly.

### Failure boundary
Typical causes:
- bad image or config
- prod-only secret or variable mismatch
- ingress/backend health degraded
- dependency regression under real traffic
- rollout checks too shallow

### Troubleshooting steps
1. Match deployment time with error and latency changes.
2. Verify exact artifact digest and config revision.
3. Check ingress/backend health and dependency traces.
4. Decide rollback vs fix-forward based on blast radius.
5. Confirm recovery using the customer path.

### Redacted log / trace evidence
```text
Pipeline:
Build succeeded
helm upgrade succeeded

Metrics:
5xx_rate=18%
p95_latency=4.8s

Ingress logs:
upstream connect error or disconnect/reset before headers
```

### Fix
Roll back the bad revision or apply the smallest safe correction, then verify with real user-path checks.

### Prevention
Use post-deploy verification against customer flows and progressive delivery.

### Open-source references
- Helm docs: https://helm.sh/docs/
- Argo Rollouts docs: https://argo-rollouts.readthedocs.io/

---

## 27) Storage account access works in test but fails in production

### Interview question
**The same code can read storage in test but gets authorization failures in prod. How would you debug it?**

### What I would say
I would compare the **exact identity and exact storage permission model** in both environments. In Azure, test often works because of inherited or broad rights, while prod requires explicit RBAC or SAS behavior that the app is not matching.

### Failure boundary
Typical causes:
- wrong managed identity in prod
- RBAC scope mismatch
- network restrictions only in prod
- SAS token or connection string wrong
- firewall/private endpoint issue

### Troubleshooting steps
1. Identify the prod runtime identity.
2. Check role assignment scope on the storage account or container.
3. Compare test and prod network restrictions.
4. Validate secret or connection string source.
5. Match storage audit records to app failures.

### Redacted log / trace evidence
```text
Azure.RequestFailedException: Status: 403 (Forbidden)
ErrorCode: AuthorizationPermissionMismatch

principal=payments-api-prod-mi
resource=prod-storage/container/invoices
```

### Fix
Grant the right data access at the right scope and correct any prod-only network or secret mismatch.

### Prevention
Use environment-specific smoke tests for real storage operations.

### Open-source references
- Azure SDK repo: https://github.com/Azure/azure-sdk-for-python
- OpenTelemetry docs: https://opentelemetry.io/docs/

---

## 28) VM Scale Set instances boot, but app never becomes healthy

### Interview question
**Scale set instances come up, but the app never becomes healthy behind the load balancer. How would you investigate it?**

### What I would say
I would inspect the **instance bootstrap path**. The fact that a VM exists does not mean the application started correctly. I would check custom script or cloud-init output, config delivery, dependency reachability, and load balancer health probes.

### Failure boundary
Typical causes:
- bootstrap script failed
- config file missing
- dependency unreachable at boot
- app starts on wrong port
- probe path mismatch

### Troubleshooting steps
1. Review scale set instance model and recent image/config change.
2. Inspect bootstrap logs.
3. Verify app listener and health path.
4. Check load balancer probe expectations.
5. Compare a new instance to a last-known-good instance.

### Redacted log / trace evidence
```text
custom-script.log
Downloading app package...
Starting api.service...
api.service failed: /etc/api/config.yaml not found

Load balancer probe:
expected /health 200
received 503
```

### Fix
Correct bootstrap/config delivery or probe alignment and redeploy the scale set safely.

### Prevention
Version bootstrap scripts, validate instance startup, and canary new images before broad rollout.

### Open-source references
- cloud-init docs: https://cloudinit.readthedocs.io/
- NGINX docs: https://nginx.org/en/docs/

---

## 29) Event-driven Azure Function falls behind or times out

### Interview question
**An Azure Function processing queue messages starts falling behind. How would you troubleshoot it?**

### What I would say
I would measure **handler duration, downstream dependency latency, retry behavior, and concurrency**. Event-driven outages usually happen because work per message increased or a dependency degraded, not because the function platform suddenly broke.

### Failure boundary
Typical causes:
- slow handler logic
- downstream timeout
- retry storm
- low timeout setting
- concurrency too high or too low

### Troubleshooting steps
1. Measure message age and handler runtime.
2. Trace downstream calls.
3. Inspect retry count and dead-letter behavior.
4. Compare release timing with throughput drop.
5. Tune concurrency only after understanding the bottleneck.

### Redacted log / trace evidence
```text
Function execution started
message_id=7ab19 elapsed_ms=47211
dependency=tax-api timeout after 30000ms
delivery_count=4
```

### Fix
Reduce handler latency, correct retry behavior, and tune timeout/concurrency based on real processing needs.

### Prevention
Track message age, downstream latency, and retry patterns together.

### Open-source references
- OpenTelemetry docs: https://opentelemetry.io/docs/
- Prometheus docs: https://prometheus.io/docs/introduction/overview/

---

## 30) Cross-cloud identity federation breaks for one environment

### Interview question
**A deployment system works in one cloud environment but fails in another because federated identity stopped working. How would you approach it?**

### What I would say
I would model the problem as an **identity-chain mismatch**. I would compare the token issuer, audience, subject or claims, trust binding, target role or service account, and any org-level restrictions. Cross-cloud or multi-environment auth failures usually come from one claim or trust condition drifting, not from the deployment code itself.

### Failure boundary
Typical causes:
- issuer mismatch
- audience mismatch
- subject/claim mismatch
- wrong target role/service account
- org or policy restriction blocks token exchange

### Troubleshooting steps
1. Capture the exact auth error.
2. Decode token claims safely in a non-prod path.
3. Compare working and failing environment trust conditions.
4. Verify target identity and scope.
5. Retest the auth exchange before rerunning the full deploy.

### Redacted log / trace evidence
```text
AccessDenied / PERMISSION_DENIED / Unauthorized
subject=repo:org/platform:ref:refs/heads/main
aud=sts.amazonaws.com
target=prod-deploy-role

working env:
subject=repo:org/platform:ref:refs/heads/release
```

### Fix
Align trust conditions with the real token claims and narrow them intentionally rather than using broad wildcards.

### Prevention
Document federation mappings per environment and validate them continuously in CI.

### Open-source references
- GitHub Actions OIDC docs: https://docs.github.com/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect
- OpenTelemetry docs: https://opentelemetry.io/docs/

---

# How to answer cloud production incidents in interviews

Use this order:

1. **Start with the user symptom**
2. **Name the most likely failure boundary**
3. **Give the first troubleshooting steps in order**
4. **Use one log, trace, or audit snippet as evidence**
5. **State the fix**
6. **End with prevention**

That structure sounds strong in interviews because it shows:
- systems thinking
- evidence-led debugging
- production safety
- recovery awareness
- clear ownership of rollout and observability
