# 10 Real-Time Production Issues in GCP Deployments
## 10/10 Incident Runbook Notes for GKE, Cloud Run, Networking, Databases, and Eventing

These notes are written as an incident-oriented Markdown runbook for real-time production systems on Google Cloud. Each issue includes:

- symptoms
- likely root causes
- blast radius
- verification commands
- decision points
- remediation actions
- prevention guidance
- key metrics and alerts
- escalation cues

The examples cover **GKE**, **Cloud Run**, **Cloud Load Balancing**, **Cloud SQL / AlloyDB**, **Memorystore**, **Pub/Sub**, IAM, and multi-region recovery patterns.

---

# 1. Fast Incident Summary Matrix

| # | Issue | First Check | Main Metric / Signal | Fastest Safe Mitigation |
|---|---|---|---|---|
| 1 | GKE pods stuck in `Pending` | `kubectl describe pod` | unschedulable events | scale node pool or reduce pod requests |
| 2 | GKE `CrashLoopBackOff` | `kubectl logs --previous` | restart count / probe failures | rollback bad deployment |
| 3 | Cloud Run cold start latency | service config + logs | p95/p99 latency after idle | increase min instances |
| 4 | App cannot reach Cloud SQL / AlloyDB | network path + auth | DB timeouts / auth failures | fix connectivity and credentials |
| 5 | Pub/Sub backlog growth | subscription metrics | undelivered messages / oldest unacked age | scale consumers |
| 6 | Load balancer 502 / 503 | backend health | unhealthy backends | fix readiness / port mapping / backend health |
| 7 | Redis saturation | Redis CPU/memory | cache latency / hit drop | scale Redis or reduce hot keys |
| 8 | HPA not scaling | `kubectl describe hpa` | utilization missing / maxed out | fix metrics or raise limits |
| 9 | Secret / IAM permission denied | runtime identity | 403 / permission errors | correct SA and IAM bindings |
| 10 | Multi-region failover failed | health routing + DB readiness | traffic not shifting / DR lag | execute validated failover sequence |

---

# 2. Standard Production Troubleshooting Workflow

Use this sequence during incidents before diving into service-specific debugging.

## 2.1 Confirm Blast Radius
Identify whether the issue affects:
- one pod
- one deployment
- one namespace
- one cluster
- one region
- all customer traffic
- only reads
- only writes
- only async processing
- only one integration partner

## 2.2 Check Recent Change Window
Look for:
- new deployment or revision
- config or secret rotation
- IAM change
- Terraform apply
- DB maintenance
- quota or scaling changes
- DNS, LB, or certificate updates

## 2.3 Validate Edge Health
```bash
kubectl get ingress -A
gcloud compute backend-services list
gcloud compute backend-services get-health <BACKEND_SERVICE_NAME> --global
gcloud compute url-maps list
```

## 2.4 Validate Runtime Health
```bash
kubectl get pods -A
kubectl top pods -A
kubectl top nodes
gcloud run services list --region <REGION>
gcloud run revisions list --region <REGION> --service <SERVICE_NAME>
```

## 2.5 Validate Dependencies
- database connectivity
- Redis health
- Pub/Sub backlog
- VPC connector status
- NAT egress path
- DNS resolution
- Secret Manager access
- KMS access
- external dependency latency

## 2.6 Mitigate Before Perfectly Explaining
Preferred early mitigation options:
- rollback
- scale out
- disable a bad revision
- drain or replay queue carefully
- route traffic away from unhealthy region
- fail over if documented and required

## 2.7 Preserve Evidence
Capture:
- logs
- failing revision or image digest
- deployment timestamps
- metrics screenshots
- event samples
- exact error messages

---

# 3. Issue 1: GKE Pods Stuck in `Pending`

## Symptoms
- deployment is created but pods never schedule
- service stays under-replicated
- release rollout pauses or fails
- latency rises because old replicas carry all traffic
- HPA may request more pods but none start

## Blast Radius
- typically one deployment or namespace first
- can become cluster-wide during resource exhaustion or quota shortage

## Common Root Causes
- insufficient CPU or memory in the node pool
- taints/tolerations mismatch
- node selector or affinity mismatch
- quota exhaustion
- unbound PVC
- region/zone shortage
- oversized pod requests
- max pods per node reached

## Verification Commands
```bash
kubectl get pods -A
kubectl describe pod <POD_NAME> -n <NAMESPACE>
kubectl get nodes -o wide
kubectl top nodes
kubectl get events -A --sort-by=.lastTimestamp
gcloud container clusters describe <CLUSTER_NAME> --region <REGION>
gcloud compute project-info describe --project <PROJECT_ID>
```

## What to Look For
- `0/X nodes are available`
- `Insufficient cpu`
- `Insufficient memory`
- `didn't tolerate`
- `node affinity`
- `pod has unbound immediate PersistentVolumeClaims`

## Decision Path
- If **insufficient CPU/memory**, scale nodes or reduce requests.
- If **taint/selector mismatch**, correct scheduling rules.
- If **PVC issue**, fix storage class or claim binding.
- If **quota exhausted**, increase project quota.
- If **regional capacity shortage**, consider alternate zone/region strategy.

## Remediation Actions
- scale node pool
- reduce resource requests
- split workloads across dedicated node pools
- correct taints/tolerations or affinity
- fix PVC/storage class
- raise quotas
- pre-provision capacity before traffic peaks

## Verification After Fix
```bash
kubectl get pods -n <NAMESPACE> -w
kubectl rollout status deployment/<DEPLOYMENT_NAME> -n <NAMESPACE>
```

## Prevention
- define realistic requests/limits
- maintain buffer capacity for tier0 workloads
- alert on unschedulable pods
- separate noisy workloads from critical services
- pre-scale before campaigns or releases

## Key Metrics / Alerts
- pending pod count
- node CPU allocatable headroom
- node memory allocatable headroom
- cluster autoscaler events
- quota utilization

## Escalate When
- tier0 service cannot restore capacity quickly
- quota increase requires platform/cloud governance action
- regional shortage affects production deadline recovery

---

# 4. Issue 2: GKE `CrashLoopBackOff` After Deployment

## Symptoms
- pod starts and restarts repeatedly
- high restart count
- rollout stuck
- service returns 5xx
- probes fail even though image pull succeeds

## Blast Radius
- often limited to new version
- can become full outage if all replicas updated at once

## Common Root Causes
- bad environment variable
- missing secret or configmap
- wrong startup command
- app bug
- port mismatch
- liveness/readiness probe misconfiguration
- startup dependency unavailable
- image built incorrectly

## Verification Commands
```bash
kubectl get pods -n <NAMESPACE>
kubectl describe pod <POD_NAME> -n <NAMESPACE>
kubectl logs <POD_NAME> -n <NAMESPACE> --previous
kubectl logs <POD_NAME> -n <NAMESPACE>
kubectl get deploy <DEPLOYMENT_NAME> -n <NAMESPACE> -o yaml
kubectl get configmap,secret -n <NAMESPACE>
```

## What to Look For
- stack trace or panic
- `connection refused`
- `secret not found`
- `permission denied`
- probe failures
- container listening on wrong port

## Decision Path
- If **app bug**, roll back immediately.
- If **config error**, patch config and restart.
- If **secret missing**, restore secret/version and redeploy.
- If **probe too strict**, tune probe thresholds.
- If **dependency unavailable**, consider startup retry logic.

## Remediation Actions
- rollback to last good revision
- fix config or secret references
- patch image
- correct probe paths/ports
- widen startup probe timing for heavy boot apps
- restore partial traffic only after readiness proves healthy

## Example Commands
```bash
kubectl rollout undo deployment/<DEPLOYMENT_NAME> -n <NAMESPACE>
kubectl rollout status deployment/<DEPLOYMENT_NAME> -n <NAMESPACE>
```

## Prevention
- canary rollout for tier0 services
- startup probe for heavy apps
- config validation in CI
- mandatory secret existence checks before promotion
- synthetic smoke tests after deployment

## Key Metrics / Alerts
- restart count
- probe failure count
- 5xx rate after deploy
- rollout availability percentage

## Escalate When
- rollback also fails
- issue affects multiple services or shared base image
- startup failure points to platform dependency outage

---

# 5. Issue 3: Cloud Run High Latency Due to Cold Starts

## Symptoms
- intermittent p95/p99 spikes
- first requests after idle are slow
- webhook acknowledgments breach SLA
- public API appears healthy on average but unstable at tail latency

## Blast Radius
- usually one service or one region
- more visible on low-traffic or bursty services

## Common Root Causes
- scale to zero with latency-sensitive traffic
- large image or heavy startup
- expensive dependency initialization
- synchronous secret or config loading
- VPC connector path overhead
- concurrency misfit

## Verification Commands
```bash
gcloud run services describe <SERVICE_NAME> --region <REGION>
gcloud run revisions list --service <SERVICE_NAME> --region <REGION>
gcloud logging read \
  'resource.type="cloud_run_revision" AND resource.labels.service_name="<SERVICE_NAME>"' \
  --limit 100
```

## What to Look For
- min instances at 0
- long initialization messages in logs
- spikes after quiet periods
- latency isolated to first requests on new instances

## Decision Path
- If **customer-facing low-latency API**, raise min instances.
- If **startup too heavy**, optimize image and app init.
- If **traffic burst + high concurrency mismatch**, tune concurrency.
- If **webhook intake is slow**, acknowledge quickly and process async.

## Remediation Actions
- set min instances
- reduce startup time
- separate warm path from heavy path
- tune concurrency
- use lazy initialization
- isolate background work behind Pub/Sub or Tasks

## Example Fix
```bash
gcloud run services update <SERVICE_NAME> \
  --region <REGION> \
  --min-instances 2 \
  --concurrency 40
```

## Prevention
- classify APIs by latency sensitivity
- benchmark cold and warm starts
- keep images small
- avoid unnecessary frameworks at edge
- use synthetic checks that detect tail latency, not just uptime

## Key Metrics / Alerts
- p95/p99 latency
- instance count
- request count after idle
- cold start rate inference from logs/revision behavior

## Escalate When
- tail latency affects revenue or contractual SLAs
- VPC or dependency path causes systemic latency across services

---

# 6. Issue 4: Cloud Run or GKE Service Cannot Reach Cloud SQL / AlloyDB

## Symptoms
- connection timeout
- authentication failure
- intermittent DB errors under load
- deployment healthy, runtime broken
- only private environments affected

## Blast Radius
- can affect one service, one namespace, or all workloads sharing a connector/network path

## Common Root Causes
- wrong hostname/connection string
- Serverless VPC Access misconfiguration
- missing private IP routing
- firewall or route issue
- wrong service account / IAM auth config
- invalid secret
- DB connection pool exhaustion
- maintenance or failover event

## Verification Commands
```bash
gcloud sql instances describe <INSTANCE_NAME>
gcloud beta alloydb instances describe <INSTANCE_NAME> --cluster <CLUSTER_NAME> --region <REGION>
gcloud compute networks vpc-access connectors describe <CONNECTOR_NAME> --region <REGION>
kubectl exec -it <POD_NAME> -n <NAMESPACE> -- /bin/sh
nslookup <DB_HOST>
nc -zv <DB_HOST> <DB_PORT>
gcloud logging read 'textPayload:"connection" AND resource.type=("k8s_container" OR "cloud_run_revision")' --limit 100
```

## What to Look For
- host not resolving
- TCP timeout
- `password authentication failed`
- connector detached or unhealthy
- max connections reached
- failover event in DB logs

## Decision Path
- If **DNS/network path fails**, fix routing/connectors/firewall first.
- If **auth fails**, validate secret and principal.
- If **connections exhausted**, pool more carefully and reduce app parallelism.
- If **issue started during maintenance/failover**, confirm DB state and client retry behavior.

## Remediation Actions
- attach or fix VPC connector
- verify private IP path
- rotate/fix secret
- restart or roll out with corrected DB settings
- reduce concurrency
- introduce pooling/proxy model
- validate DB HA/failover settings

## Example Command
```bash
gcloud run services update <SERVICE_NAME> \
  --region <REGION> \
  --vpc-connector <CONNECTOR_NAME> \
  --set-secrets DB_PASSWORD=db-password:latest
```

## Prevention
- standard connection pooling patterns
- readiness checks that validate dependency path carefully
- monitor connection count and query latency
- test failover behavior under load
- avoid opening one DB connection per request

## Key Metrics / Alerts
- DB connection count
- auth failure count
- query latency
- connector throughput/errors
- app timeout rates

## Escalate When
- shared DB outage affects multiple critical services
- suspected network control-plane or regional issue
- replica promotion or failover required

---

# 7. Issue 5: Pub/Sub Backlog Growing Rapidly

## Symptoms
- backlog count climbs continuously
- event completion SLA breached
- notifications, orders, or downstream updates arrive late
- oldest unacked message age increases
- workers appear healthy but throughput is insufficient

## Blast Radius
- one subscription first
- can spread to many downstream services if topic fan-out is broad

## Common Root Causes
- too few consumers
- slow business logic
- downstream DB/API bottleneck
- poison messages retrying repeatedly
- ordering constraints slowing throughput
- no dead-letter handling
- consumer concurrency too low

## Verification Commands
```bash
gcloud pubsub subscriptions describe <SUBSCRIPTION_NAME>
gcloud pubsub subscriptions pull <SUBSCRIPTION_NAME> --limit=5 --auto-ack
gcloud logging read 'resource.type=("cloud_run_revision" OR "k8s_container") AND textPayload:"Pub/Sub"' --limit 100
kubectl top pods -n <NAMESPACE>
gcloud monitoring time-series list --filter='metric.type="pubsub.googleapis.com/subscription/num_undelivered_messages"'
gcloud monitoring time-series list --filter='metric.type="pubsub.googleapis.com/subscription/oldest_unacked_message_age"'
```

## What to Look For
- undelivered messages increasing
- oldest message age increasing
- same failure signatures repeating
- consumer CPU or DB wait saturation
- DLQ absent or unused

## Decision Path
- If **consumers too few**, scale horizontally.
- If **poison messages repeating**, DLQ them.
- If **DB or external call is slow**, isolate and buffer slower stages.
- If **ordering key bottleneck**, redesign partitioning or ordering strategy.

## Remediation Actions
- scale consumers
- raise max instances / replicas
- move heavy steps to Tasks or separate topic
- configure DLQ
- optimize DB writes
- reduce ordered-key hotspots
- replay carefully once fixed

## Example Commands
```bash
kubectl scale deployment <CONSUMER_DEPLOYMENT> --replicas=20 -n <NAMESPACE>

gcloud run services update <SERVICE_NAME> \
  --region <REGION> \
  --max-instances 200
```

## Prevention
- backlog alerts with severity thresholds
- idempotent consumer design
- schema validation
- split heavy consumers by event class
- periodic replay tests

## Key Metrics / Alerts
- undelivered message count
- oldest unacked age
- DLQ publish rate
- per-message processing latency
- duplicate processing rate

## Escalate When
- backlog threatens financial or compliance deadlines
- replay design is unclear
- poison messages suggest data contract breach across teams

---

# 8. Issue 6: Load Balancer Returns 502 / 503

## Symptoms
- users get 502 or 503
- health checks fail
- rollout causes traffic loss
- service works internally but not through edge

## Blast Radius
- can be service-local or edge-global depending on LB pathing

## Common Root Causes
- pods not ready
- wrong port mapping
- service selector mismatch
- backend NEG mismatch
- no healthy backends
- Cloud Run auth/ingress mismatch behind LB
- certificate or URL map misroute

## Verification Commands
```bash
kubectl get ingress -A
kubectl describe ingress <INGRESS_NAME> -n <NAMESPACE>
kubectl get svc,endpoints -n <NAMESPACE>
kubectl describe pod <POD_NAME> -n <NAMESPACE>
gcloud compute backend-services list
gcloud compute backend-services get-health <BACKEND_SERVICE_NAME> --global
gcloud compute url-maps describe <URL_MAP_NAME>
```

## What to Look For
- endpoints empty
- unhealthy backend targets
- service exposing wrong targetPort
- readiness endpoint failing
- LB points to backend that is not serving expected path

## Decision Path
- If **no endpoints**, fix deployment/service selector.
- If **probe failing**, fix health endpoint or startup timing.
- If **Cloud Run path**, verify ingress and auth alignment.
- If **URL map wrong**, patch route rules.

## Remediation Actions
- restore healthy backend
- rollback faulty release
- correct ports/selectors
- patch readiness/health checks
- fix URL map or backend service references
- confirm cert and hostname routing where applicable

## Quick Validation
```bash
kubectl port-forward svc/<SERVICE_NAME> 8080:80 -n <NAMESPACE>
curl -I http://127.0.0.1:8080/health
```

## Prevention
- pre-prod health-check validation
- synthetic edge tests after deploy
- standard service/port conventions
- canary route before full traffic cutover

## Key Metrics / Alerts
- LB backend healthy count
- 502/503 rate
- readiness failure count
- endpoint count by service

## Escalate When
- edge issue impacts multiple services
- routing layer misconfiguration is shared across domains
- certificate / hostname issue blocks customer access globally

---

# 9. Issue 7: Redis / Memorystore Saturation Causing Latency Spikes

## Symptoms
- p95/p99 latency spikes
- cart/session reads slow down
- cache timeouts
- DB query volume surges due to cache misses
- critical user flows degrade during spikes

## Blast Radius
- all services sharing the cache
- can cascade into database overload

## Common Root Causes
- undersized Redis instance
- hot key concentration
- poor TTL strategy
- cache abuse as source of truth
- overly large payload values
- sudden burst beyond expected headroom

## Verification Commands
```bash
gcloud redis instances describe <REDIS_INSTANCE> --region <REGION>
gcloud monitoring time-series list --filter='metric.type="redis.googleapis.com/stats/cpu_utilization"'
gcloud monitoring time-series list --filter='metric.type="redis.googleapis.com/stats/memory/usage_ratio"'
gcloud logging read 'textPayload:"redis" AND resource.type=("k8s_container" OR "cloud_run_revision")' --limit 100
```

## What to Look For
- high CPU
- high memory usage ratio
- timeout errors
- hot user journeys aligning with spikes
- DB fallback traffic climbing simultaneously

## Decision Path
- If **capacity issue**, scale Redis quickly.
- If **hot key issue**, shard or redesign key model.
- If **cache stampede**, add jitter/locking/fallback control.
- If **payload too large**, reduce value size.

## Remediation Actions
- scale cache
- redesign key distribution
- tune TTLs
- reduce payload size
- add stampede protection
- rate-limit fallback to DB

## Example Command
```bash
gcloud redis instances update <REDIS_INSTANCE> \
  --region <REGION> \
  --size=10
```

## Prevention
- cache hit ratio dashboard
- keyspace design review
- TTL policy review
- game-day testing under flash traffic
- isolate non-critical cache use from checkout-critical cache paths

## Key Metrics / Alerts
- Redis CPU
- memory usage ratio
- cache timeout rate
- hit ratio
- fallback DB query rate

## Escalate When
- DB begins to fail due to cache collapse
- tier0 flows such as cart/checkout depend on the saturated cache

---

# 10. Issue 8: GKE HPA Not Scaling as Expected

## Symptoms
- traffic rises but pod count does not
- queue lag increases
- response times worsen under load
- HPA object exists but no scale event occurs

## Blast Radius
- one deployment, then possibly customer-facing capability

## Common Root Causes
- missing resource requests
- metrics pipeline issue
- HPA targeting wrong object
- max replicas too low
- autoscaling on wrong signal
- cluster autoscaler unable to add nodes fast enough

## Verification Commands
```bash
kubectl get hpa -A
kubectl describe hpa <HPA_NAME> -n <NAMESPACE>
kubectl top pods -n <NAMESPACE>
kubectl get deploy <DEPLOYMENT_NAME> -n <NAMESPACE> -o yaml
kubectl get apiservices
kubectl top nodes
```

## What to Look For
- `missing request for cpu`
- current metrics unavailable
- max replicas already reached
- pods pending due to no node capacity
- CPU low while queue lag high

## Decision Path
- If **metrics missing**, restore metrics path.
- If **CPU requests absent**, define them.
- If **wrong scaling signal**, add custom metric.
- If **node shortage**, scale nodes or pre-scale capacity.

## Remediation Actions
- set realistic requests
- fix HPA target
- raise max replicas
- use queue lag/request rate metrics where appropriate
- tune cluster autoscaler
- isolate workloads into dedicated pools

## Prevention
- HPA linting in CI
- mandatory requests/limits policy
- queue-based autoscaling for async workers
- load testing with scale validation

## Key Metrics / Alerts
- HPA desired vs current replicas
- pending pods
- queue lag
- CPU utilization
- node allocatable headroom

## Escalate When
- autoscaling logic is flawed for multiple services
- cluster-wide scale ceiling is hit

---

# 11. Issue 9: Secret / IAM Permission Denied at Runtime

## Symptoms
- runtime returns 403 / permission denied
- app cannot read secrets, Pub/Sub, GCS, BigQuery, or KMS
- deployment is healthy but business action fails
- issue starts after identity or policy change

## Blast Radius
- one service first
- can affect multiple services if shared identity or org policy changed

## Common Root Causes
- wrong runtime service account
- missing IAM role
- broken Workload Identity mapping
- resource-level permission missing
- project mismatch
- secret version access missing
- default service account accidentally used

## Verification Commands
```bash
kubectl get sa -n <NAMESPACE>
kubectl describe sa <K8S_SERVICE_ACCOUNT> -n <NAMESPACE>
kubectl describe pod <POD_NAME> -n <NAMESPACE>
gcloud projects get-iam-policy <PROJECT_ID>
gcloud secrets get-iam-policy <SECRET_NAME>
gcloud run services describe <SERVICE_NAME> --region <REGION>
gcloud logging read 'textPayload:("403" OR "permission denied") AND resource.type=("k8s_container" OR "cloud_run_revision")' --limit 100
```

## What to Look For
- wrong service account annotation
- missing `secretAccessor`
- missing Pub/Sub or storage access role
- revision deployed with unintended identity
- 403 concentrated after policy update

## Decision Path
- If **wrong identity attached**, fix deployment immediately.
- If **binding missing**, add least-privilege role.
- If **Workload Identity broken**, repair KSA-to-GSA mapping.
- If **org policy blocked behavior**, coordinate platform/security approval.

## Remediation Actions
- correct runtime service account
- add resource-scoped IAM binding
- fix Workload Identity annotations and bindings
- redeploy with intended identity
- verify secret/version permissions
- remove dependency on default service accounts

## Prevention
- one runtime identity per service
- IAM policy tests in CI
- no manual prod identity edits outside change process
- alerts on permission-denied spikes

## Key Metrics / Alerts
- permission-denied log count
- failed secret access
- failed publish/pull/storage operations
- revision change correlation

## Escalate When
- security policy blocks emergency recovery
- shared identity or org-level policy regression affects broad estate

---

# 12. Issue 10: Multi-Region Failover Did Not Work During Incident

## Symptoms
- secondary region exists but traffic stayed in failed region
- manual failover took too long
- data inconsistency after recovery
- queue replay order unclear
- secondary services missing secrets/config/access

## Blast Radius
- regional or global
- potentially full customer impact for tier0 systems

## Common Root Causes
- DR never tested
- health-based routing not configured correctly
- DB replica/failover target not ready
- secrets/certs/config not synchronized
- secondary consumers cold or unauthorized
- recovery runbook incomplete
- active/active expectations unclear

## Verification Commands
```bash
gcloud compute backend-services get-health <BACKEND_SERVICE_NAME> --global
gcloud dns record-sets list --zone=<DNS_ZONE>
gcloud sql instances describe <INSTANCE_NAME>
gcloud beta alloydb clusters describe <CLUSTER_NAME> --region <REGION>
gcloud container clusters list
gcloud run services list --region <REGION>
gcloud pubsub subscriptions describe <SUBSCRIPTION_NAME>
```

## What to Look For
- unhealthy secondary backends
- missing DB promotion path
- stale DNS or wrong traffic policy
- subscriptions not active
- secrets/certs absent in secondary region/project

## Decision Path
- If **routing unhealthy**, shift traffic only after backend health is verified.
- If **DB not ready**, do not reopen writes prematurely.
- If **secondary app missing config**, restore configs before cutover.
- If **queue replay unclear**, keep consumers paused until sequence is understood.

## Remediation Actions
- execute documented failover sequence
- promote or redirect database safely
- validate secret/cert parity
- enable and verify secondary consumers
- replay events only with idempotency checks
- reopen traffic gradually

## Recommended Recovery Sequence
1. confirm primary region impairment
2. stop unsafe writes if split-brain risk exists
3. promote or switch database
4. verify secondary app health
5. shift traffic by LB or DNS policy
6. enable async workers and validate event flow
7. reopen traffic progressively
8. monitor error budget, duplicate events, and data integrity

## Prevention
- quarterly DR tests for tier0
- semiannual execution drills
- regional readiness dashboards
- config and secret parity checks
- documented active/active vs active/passive strategy
- event replay rehearsals

## Key Metrics / Alerts
- backend health by region
- replication lag
- failover readiness
- queue consumer status
- region traffic distribution

## Escalate When
- data integrity is in doubt
- region recovery requires executive/business decision
- compliance or financial transaction risk exists

---

# 13. Additional GCP-Native Edge Cases Worth Tracking

These are not expanded into full sections above, but they should exist in mature runbooks:

- **Serverless VPC Access connector saturation**
- **Cloud NAT ephemeral port exhaustion**
- **NEG attachment mismatch after rollout**
- **Ingress vs Gateway misconfiguration in GKE**
- **Apigee policy misroute or quota rejection**
- **Cloud Armor false positives**
- **Private Service Connect endpoint routing issues**
- **BigQuery cost spike due to runaway analytical queries**
- **Artifact signing gate blocking production promotion**
- **Certificate expiration causing edge outage**

---

# 14. Minimal On-Call Command Pack

Keep a standardized command pack in team notes.

```bash
# Kubernetes
kubectl get pods -A
kubectl describe pod <POD_NAME> -n <NAMESPACE>
kubectl logs <POD_NAME> -n <NAMESPACE> --previous
kubectl top pods -A
kubectl top nodes
kubectl get events -A --sort-by=.lastTimestamp
kubectl get svc,endpoints -A
kubectl get ingress -A
kubectl describe hpa <HPA_NAME> -n <NAMESPACE>

# Cloud Run
gcloud run services list --region <REGION>
gcloud run services describe <SERVICE_NAME> --region <REGION>
gcloud run revisions list --service <SERVICE_NAME> --region <REGION>

# Load Balancing
gcloud compute backend-services list
gcloud compute backend-services get-health <BACKEND_SERVICE_NAME> --global
gcloud compute url-maps list

# Databases
gcloud sql instances describe <INSTANCE_NAME>
gcloud beta alloydb clusters describe <CLUSTER_NAME> --region <REGION>

# Pub/Sub
gcloud pubsub subscriptions describe <SUBSCRIPTION_NAME>
gcloud pubsub subscriptions pull <SUBSCRIPTION_NAME> --limit=5 --auto-ack

# IAM / Secrets
gcloud projects get-iam-policy <PROJECT_ID>
gcloud secrets get-iam-policy <SECRET_NAME>

# Logging
gcloud logging read 'resource.type=("k8s_container" OR "cloud_run_revision")' --limit 100
```

---

# 15. Final Senior Architect Recommendations

1. **Every incident runbook should start with blast radius, recent changes, and fastest safe mitigation.**
2. **Troubleshooting quality improves dramatically when logs, traces, and event IDs share the same correlation model.**
3. **Real-time systems fail at dependency boundaries more often than at pure compute boundaries.**
4. **A rollback path is mandatory for every tier0 deployment.**
5. **Async systems without DLQ, replay design, and idempotency are unfinished systems.**
6. **HA without tested failover is confidence theater.**
7. **Cold-start-sensitive APIs should be explicitly classified and configured differently from background workers.**
8. **Cache saturation can become a hidden database outage amplifier.**
9. **Least-privilege IAM should still be operationally testable during rollout, not discovered during an outage.**
10. **The best production teams do not improvise commands during incidents; they standardize them before the outage.**
