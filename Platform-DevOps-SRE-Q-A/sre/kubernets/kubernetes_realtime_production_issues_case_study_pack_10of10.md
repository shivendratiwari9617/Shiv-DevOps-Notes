# Kubernetes Real-Time Deployment and Production Issues
**10/10 senior-level runbook and case study pack for Platform Engineering, DevOps, and SRE interviews**

## Positioning

This document is designed to read like a **real senior-level incident and troubleshooting pack**, not a blog post. It is built for:

- senior Kubernetes / platform / DevOps / SRE interviews
- portfolio case studies
- internal runbooks
- on-call enablement
- architecture and operational maturity discussions

Each issue includes:

- production context
- severity and blast radius
- ownership and escalation guidance
- symptoms
- likely root causes
- investigation flow
- safe fixes
- unsafe fixes to avoid
- how to verify recovery after the fix
- prevention and follow-up improvements
- senior interview talking points

---

# Fast Triage Model

Before touching anything, classify the incident across four dimensions:

## 1. User Impact
- Is this a customer outage, degraded service, or delivery-only issue?
- Are internal-only systems affected, or customer-facing paths?

## 2. Failure Domain
- one pod
- one workload
- one namespace
- one node / node pool
- ingress / DNS / cluster-wide
- downstream dependency outside Kubernetes

## 3. Recent Change
- deployment
- config / secret rotation
- ingress or DNS change
- autoscaling event
- cluster upgrade / node replacement
- certificate renewal
- network policy / security change

## 4. Safe First Move
- pause rollout
- isolate unhealthy endpoints
- rollback
- preserve evidence and avoid mutation
- engage platform/network/security owner

---

# Evidence to Capture Before Mutation

Always capture evidence before large changes:

- incident start time
- current rollout version and previous version
- pod restart counts
- recent events
- current HPA status
- node conditions
- ingress / service endpoints
- logs from failing and healthy pods
- metrics around latency, error rate, saturation
- recent secret, config, or policy changes
- cloud-side logs if load balancer, IAM, or storage is involved

---

# 1) Pods Are Running but the Service Is Still Failing Requests

## Severity / Blast Radius
**SEV-2 to SEV-1** if users are getting 5xx or timeouts while Kubernetes appears healthy.

## Primary Owner
**Application team**

## Consulted
- Platform
- SRE
- Networking if ingress or service mesh path is involved

## Escalate When
- more than one service is affected
- ingress or service routing appears inconsistent
- rollback does not restore customer health
- healthy pods in Kubernetes still correlate with widespread user failure

## Production Context
This is one of the most misleading Kubernetes incidents. Pods can be `Running`, deployments can show available replicas, and yet customers still see failures because Kubernetes lifecycle health is not the same as application serve-readiness.

## Symptoms

- pods are `Running`
- deployment shows desired replicas available
- ingress or load balancer returns `502`, `503`, or timeout
- p95/p99 latency rises sharply
- error rate rises after deploy
- liveness passes but user traffic fails
- app logs show dependency, startup, or port-binding errors

## Likely Root Causes

- readiness probe is too weak or too early
- service points to pods that are not truly traffic-ready
- service port / targetPort mismatch
- app listens on wrong interface or port
- startup warm-up incomplete when traffic begins
- sidecar is ready before main app
- service selector matches wrong pods
- old and new versions behave differently behind same service

## Investigation Flow

1. verify whether failures started during or after rollout
2. inspect service endpoints and confirm which pods are registered
3. compare readiness status with actual response success from inside cluster
4. inspect pod logs during startup and first traffic window
5. verify port mappings:
   - container port
   - service port
   - targetPort
   - ingress backend
6. compare current replica set to last healthy one
7. test service directly from another pod
8. determine whether traffic should be rolled back or drained from bad endpoints
9. validate whether dependency warm-up is part of startup
10. fix readiness model, not just symptoms

## Safe Fixes

- rollback if customer traffic is impacted
- remove unhealthy pods from endpoint path
- correct service/port mapping
- add or strengthen startup and readiness probes
- delay readiness until app is truly able to serve traffic

## Unsafe Fixes to Avoid

- trusting `Running` as proof of health
- restarting all pods before understanding readiness failure
- masking real app issue by only increasing timeouts
- forcing rollout forward while customers are failing

## Verify Recovery After Fix

Confirm all of the following:

- error rate returns to baseline
- latency normalizes
- endpoints contain only healthy pods
- readiness remains stable for at least one full traffic cycle
- no recurrence after one or two scale events
- user-facing synthetic checks pass

## Prevention / Permanent Fix

- design readiness around true traffic readiness
- use startup probes for slow-boot apps
- add version-aware dashboards during rollout
- standardize probe patterns by workload type
- require service contract validation in pre-prod

## Senior Interview Talking Point

This issue proves that **Kubernetes health and customer health are different signals**. A senior engineer closes that gap quickly and safely.

---

# 2) CrashLoopBackOff After Deployment

## Severity / Blast Radius
**SEV-2**, potentially **SEV-1** for critical APIs or shared infrastructure components.

## Primary Owner
**Application team**

## Consulted
- Platform
- SRE
- Security if secret/config/IAM change is involved

## Escalate When
- rollout impacts a tier-1 service
- multiple workloads crash at once
- root cause appears tied to shared config, secret, runtime base image, or cluster policy

## Production Context
A new deployment rolls out and pods begin crashing repeatedly. This often looks simple, but senior handling requires fast differentiation between bad code, missing config, resource pressure, startup dependency failure, and probe-induced restarts.

## Symptoms

- pod enters `CrashLoopBackOff`
- restart count increases rapidly
- rollout stalls
- deployment never reaches steady state
- logs show panic, config parse failure, or dependency error
- new replica set is unhealthy while old one was fine

## Likely Root Causes

- bad config or secret reference
- wrong env var key or mount path
- broken entrypoint / command
- runtime incompatibility in the container image
- missing dependency during startup
- low memory limit causing OOM then restart
- aggressive liveness/startup configuration
- image was built incorrectly

## Investigation Flow

1. inspect exit code and restart reason
2. check previous logs, not just current logs
3. compare deployment spec against last healthy version
4. verify secrets, config maps, and mount paths
5. inspect memory and CPU limits
6. confirm image tag and digest match expected release
7. verify startup dependencies are reachable
8. determine whether liveness or startup probe is causing kill loop
9. pause rollout or rollback if service is customer-facing
10. fix root cause before scaling or retrying

## Safe Fixes

- rollback to last healthy version
- restore missing config or secret
- correct command / entrypoint / runtime
- fix resource sizing if OOM-related
- add startup probe or tune startup timing based on evidence

## Unsafe Fixes to Avoid

- deleting pods repeatedly with no diagnosis
- scaling out a crashing workload
- patching production config ad hoc without tracking
- disabling liveness permanently without understanding app behavior

## Verify Recovery After Fix

- restart count stabilizes
- rollout completes without repeated failure
- new pods survive beyond prior crash window
- request success and latency normalize
- no hidden dependency errors remain in logs

## Prevention / Permanent Fix

- validate required config/secret references in CI
- enforce image immutability
- keep startup/liveness/readiness defaults by runtime profile
- use canary or staged rollout for critical services

## Senior Interview Talking Point

The senior signal is not recognizing `CrashLoopBackOff`; it is **rapidly classifying the crash into config, runtime, dependency, or resource root cause** and protecting production early.

---

# 3) ImagePullBackOff During Production Release

## Severity / Blast Radius
**SEV-3 to SEV-2**, but **SEV-1** if it blocks a critical hotfix during an incident.

## Primary Owner
**Platform / Release Engineering**

## Consulted
- Application team
- Security if policy or signing rejection is involved

## Escalate When
- hotfix rollout is blocked
- multiple namespaces or node pools cannot pull images
- registry auth failure affects more than one service

## Production Context
This issue can block new releases, rollback, autoscaling, and emergency mitigation. It is a release-integrity problem, not just a Kubernetes error.

## Symptoms

- pods stuck in `ImagePullBackOff` or `ErrImagePull`
- rollout stalls
- new nodes also fail to pull image
- events show auth denied, repo missing, tag missing, or network timeout
- autoscaling cannot recover capacity because replacement pods fail to start

## Likely Root Causes

- manifest points to nonexistent tag
- image was not published successfully
- registry secret expired or missing
- node / workload lacks permission to pull
- registry network path broken
- admission control rejects unsigned or disallowed image
- rate limiting or registry outage

## Investigation Flow

1. inspect pod events carefully
2. verify the exact image reference exists
3. confirm CI actually published the artifact
4. validate pull secret or node identity
5. compare working vs failing image references
6. inspect admission or policy engine logs
7. verify registry DNS/network path from nodes
8. decide whether rollback to previous image is safer
9. restore release path integrity
10. fix CI/CD gap that allowed bad reference promotion

## Safe Fixes

- correct manifest image reference
- republish missing image
- restore valid pull auth
- rollback to known-good image
- fix policy or signing configuration only if the intended image is genuinely valid

## Unsafe Fixes to Avoid

- changing image to `latest`
- bypassing security controls permanently
- overwriting an existing immutable release tag
- assuming old running pods prove registry path is healthy

## Verify Recovery After Fix

- new pods pull image successfully
- rollout continues normally
- replacement or autoscaled pods start without pull failure
- hotfix/rollback path is confirmed operational
- no residual auth or policy errors remain in events

## Prevention / Permanent Fix

- manifest promotion check must validate image existence
- prefer digest pinning
- monitor pull failures by namespace/service
- routinely test rollback artifact availability

## Senior Interview Talking Point

A senior answer frames this as a **release-path reliability problem**. If you cannot pull images in production, your recovery path is impaired.

---

# 4) Readiness Probe Works in Dev but Fails in Production

## Severity / Blast Radius
**SEV-2**, common during first real production scale or dependency load.

## Primary Owner
**Application team**

## Consulted
- Platform
- SRE

## Escalate When
- rollout repeatedly fails in prod only
- multiple services share the same flawed probe pattern
- probe failure blocks high-priority production release

## Production Context
Lower environments often hide startup complexity. Production adds real dependencies, bigger datasets, stricter network policy, slower warm-up, and actual load. Probes that looked fine in dev become availability liabilities in prod.

## Symptoms

- readiness flaps in production
- pods alternate between ready and unready
- rollout stalls or takes too long
- intermittent 503 during deploy
- app appears healthy locally and in stage but not under production conditions

## Likely Root Causes

- startup probe missing
- readiness checks too much dependency state
- production-only cold-start behavior
- CPU throttling delays initialization
- real network policy or DNS path differs
- app caches or connections not warmed

## Investigation Flow

1. compare prod vs lower-environment startup behavior
2. inspect time from container start to true serve-readiness
3. review readiness implementation
4. inspect throttling, memory pressure, and slow init tasks
5. verify prod-only dependencies and policies
6. test whether startup probe should gate liveness/readiness
7. slow rollout or rollback if user traffic is affected
8. redesign probe based on actual production startup path

## Safe Fixes

- introduce startup probe
- simplify readiness to serve-readiness
- right-size resources for startup
- stage rollout more gradually
- rollback until probe design is corrected

## Unsafe Fixes to Avoid

- blindly increasing all probe delays
- removing readiness entirely
- treating lower-environment success as proof
- making readiness a deep dependency health checker for every downstream system

## Verify Recovery After Fix

- readiness becomes stable through repeated restarts
- rollout completes with no 503 spike
- startup time is within expected window
- synthetic checks remain green after scale-out
- no flapping over sustained traffic period

## Prevention / Permanent Fix

- benchmark startup under production-like conditions
- keep workload-specific probe templates
- include startup-time metrics in dashboards
- validate readiness under realistic pre-prod traffic

## Senior Interview Talking Point

This is a strong example of senior thinking because probes are part of **availability design**, not just deployment YAML.

---

# 5) HPA Scales Out but Latency Still Keeps Rising

## Severity / Blast Radius
**SEV-2 to SEV-1** for customer-facing services.

## Primary Owner
**Application team + Platform/SRE shared**

## Consulted
- Database team
- Networking if ingress saturation is suspected

## Escalate When
- tier-1 service continues to degrade despite scaling
- cluster autoscaler cannot keep up
- downstream dependency is saturating

## Production Context
Autoscaling is often assumed to be a universal fix. In reality, rising pod count with rising latency usually means the bottleneck is elsewhere or the scaling signal is wrong.

## Symptoms

- HPA increases replicas
- latency keeps increasing
- 5xx also rise
- pending pods appear or new pods stay unready
- downstream dependency latency rises
- CPU is not obviously maxed on all pods

## Likely Root Causes

- HPA metric does not represent real saturation
- database/cache/external dependency is bottleneck
- node autoscaling lags pod scale demand
- connection pools saturate
- request/limit sizing causes throttling
- pods start too slowly for spike
- max replicas too low
- traffic imbalance across pods or zones

## Investigation Flow

1. inspect what metric HPA uses
2. compare HPA actions to latency and error timeline
3. inspect pending pods and node headroom
4. check DB/cache/external dependency latency
5. inspect CPU throttling, memory pressure, queue depth, thread pools
6. verify whether new pods are getting traffic evenly
7. decide whether rate limiting or traffic protection is needed now
8. tune scaling signal and capacity design later

## Safe Fixes

- tune HPA to better metric
- raise minimum replicas for predictable peaks
- fix node autoscaling lag or reserved headroom
- protect downstream systems with backpressure
- optimize startup and readiness time
- right-size resource requests

## Unsafe Fixes to Avoid

- just raising max replicas blindly
- assuming CPU HPA works for all workloads
- ignoring downstream saturation
- scaling broken or unready pods faster

## Verify Recovery After Fix

- latency stabilizes as capacity grows
- 5xx falls back to baseline
- pending pod count returns to zero
- dependency latency normalizes
- HPA and cluster autoscaler converge without oscillation

## Prevention / Permanent Fix

- use meaningful autoscaling signals
- maintain autoscaling headroom
- run production-like load tests
- document bottleneck-specific scaling runbooks

## Senior Interview Talking Point

Senior engineers know the right answer is often: **the system is not CPU-bound**. Scaling only helps if it matches the real bottleneck.

---

# 6) Pods Stay Pending Even Though the Cluster Looks Like It Has Capacity

## Severity / Blast Radius
**SEV-2**, especially during scale events or urgent releases.

## Primary Owner
**Platform**

## Consulted
- Application team for request sizing
- Storage team if PVC zone/attach constraints are involved

## Escalate When
- critical workload cannot schedule
- autoscaler cannot add compatible nodes
- topology or storage constraints block HA workloads

## Production Context
This is a classic schedulability trap. Aggregate cluster metrics suggest enough capacity, but pods still cannot be placed because Kubernetes schedules against actual constraints, fragmentation, reservations, taints, topology, and zone alignment.

## Symptoms

- pods remain `Pending`
- HPA increases desired replica count but no pods start
- scheduler events mention insufficient resources, taints, affinity, topology, or PVC mismatch
- some nodes appear underutilized
- deploy or failover stalls

## Likely Root Causes

- requests too large for remaining node fragments
- node selectors or affinity too strict
- taints not tolerated
- topology spread impossible to satisfy
- PVC zone mismatch
- autoscaler cannot create right node type
- daemonset/system overhead reduces real allocatable capacity

## Investigation Flow

1. inspect scheduler events on pending pods
2. compare allocatable vs requested, not just totals
3. inspect selectors, tolerations, affinity, spread rules
4. inspect PVC/PV zone and storage class behavior
5. verify autoscaler can provision compatible nodes
6. identify whether issue is fragmentation or true exhaustion
7. choose between scaling, relaxing constraints, or rebalancing workload class

## Safe Fixes

- add compatible node capacity
- correct tolerations/selectors
- relax unjustified placement constraints
- right-size requests to realistic values
- separate specialized workloads into proper node pools

## Unsafe Fixes to Avoid

- lowering requests unrealistically just to force scheduling
- deleting random pods to make room
- breaking HA spread rules casually for critical services
- relying on cluster-wide averages as proof of schedulability

## Verify Recovery After Fix

- pending pods schedule successfully
- rollout/scale event completes
- autoscaler provisions suitable nodes if needed
- critical workloads remain spread as intended
- no secondary capacity crunch appears in adjacent namespaces

## Prevention / Permanent Fix

- alert on scheduling failures and fragmentation
- keep reserved headroom for critical workloads
- design node pools around workload classes
- review placement rules during platform design

## Senior Interview Talking Point

A senior engineer knows **capacity is not the same as schedulable capacity**. This is often a platform design issue, not merely a resource shortage.

---

# 7) DNS / Service Discovery Failure Inside the Cluster

## Severity / Blast Radius
**SEV-2 to SEV-1** depending on breadth.

## Primary Owner
**Platform**

## Consulted
- Networking
- Application team if one noisy app is flooding DNS

## Escalate When
- multiple namespaces are affected
- CoreDNS is overloaded or repeatedly failing
- DNS failure is causing cascading cross-service outage

## Production Context
When service discovery fails, many application teams initially think their service broke. In reality, the cluster’s name-resolution layer may be the real fault domain.

## Symptoms

- lookup timeouts or `NXDOMAIN`
- apps cannot reach peers, DBs, or caches
- intermittent cross-service failures
- only some nodes/pods impacted
- CoreDNS pods show high CPU, memory, or restart count

## Likely Root Causes

- CoreDNS overload
- node DNS path issue
- network policy blocking UDP/TCP DNS
- upstream resolver issue
- bad CoreDNS config
- excessive query storm from an app
- wrong namespace/service name assumptions

## Investigation Flow

1. determine whether cluster-internal, external, or both DNS paths fail
2. test from affected and unaffected pods
3. inspect CoreDNS health and logs
4. inspect recent CoreDNS config or node DNS changes
5. verify DNS traffic allowed by policy
6. identify query storms or retry loops from apps
7. compare failures by node or zone
8. restore stable DNS service first, then fix noisy callers

## Safe Fixes

- scale or tune CoreDNS
- restore last known good CoreDNS config
- fix network policy for DNS
- reduce noisy app query rate
- correct bad service naming assumptions

## Unsafe Fixes to Avoid

- restarting CoreDNS repeatedly with no diagnosis
- assuming this is app-only if several namespaces fail
- changing cluster DNS config in live incident without rollback path

## Verify Recovery After Fix

- resolution succeeds from previously failing pods
- CoreDNS latency and error rate normalize
- affected services reconnect successfully
- DNS failure alerts stop firing
- no recurrence during peak query load

## Prevention / Permanent Fix

- monitor CoreDNS saturation
- alert on DNS latency/error spikes
- document DNS-allowing policy baseline
- review app query patterns that generate excessive DNS churn

## Senior Interview Talking Point

This is an excellent case to demonstrate **cross-layer incident thinking**: app symptoms, network policy, cluster DNS, node behavior, and noisy clients all have to be considered.

---

# 8) Ingress Looks Healthy but Some Users Get 502 / 504 Errors

## Severity / Blast Radius
**SEV-2**, potentially **SEV-1** during launches or peak events.

## Primary Owner
**Platform / Networking shared**

## Consulted
- Application team
- SRE

## Escalate When
- errors correlate to one AZ, one node pool, or one version
- ingress/controller behavior is inconsistent across backends
- aggregate success rate hides meaningful user impact

## Production Context
Partial ingress failures are especially dangerous because averages can look normal while a subset of users suffers badly.

## Symptoms

- intermittent 502/504
- errors correlate to path, AZ, node, or backend subset
- ingress/controller appears mostly healthy
- some pods respond normally while others do not
- error spikes during rollout or load surge

## Likely Root Causes

- readiness flapping causes endpoint churn
- subset of pods are slow or broken
- one node/AZ path is unhealthy
- ingress timeout lower than backend latency
- draining/termination mismatch during rollout
- stale or inconsistent endpoints
- ingress-controller or LB config drift

## Investigation Flow

1. isolate whether failure maps to path, version, zone, or backend subset
2. inspect ingress logs and backend health
3. inspect endpoint changes during failure window
4. compare healthy and unhealthy pods
5. inspect termination/draining behavior during rollout
6. test from multiple regions or paths if external
7. remove or drain bad backends
8. repair readiness or timeout mismatch

## Safe Fixes

- remove failing backend subset from rotation
- rollback bad replica set
- fix readiness/draining behavior
- tune timeout only if backend timing genuinely justifies it
- rebalance traffic away from unhealthy zone or node path

## Unsafe Fixes to Avoid

- increasing timeouts blindly
- restarting ingress controller first during customer outage
- treating partial failures as minor because averages look acceptable

## Verify Recovery After Fix

- 502/504 returns to baseline
- backend health is uniform across zones/nodes
- endpoint churn stops
- canary or rollout stabilizes
- user-facing synthetic probes pass from multiple vantage points

## Prevention / Permanent Fix

- correlate ingress logs with pod/version/node metadata
- test graceful shutdown and draining under load
- monitor per-endpoint and per-zone health
- keep rollout settings aligned with LB draining behavior

## Senior Interview Talking Point

This case shows whether you can debug **partial failure domains** instead of relying on cluster-wide averages.

---

# 9) Secret Rotated Successfully but the Application Still Fails Authentication

## Severity / Blast Radius
**SEV-2**, sometimes **SEV-1** for auth, payments, or critical data paths.

## Primary Owner
**Application team**

## Consulted
- Platform
- Security
- Database team if DB credential rotation is involved

## Escalate When
- auth failures affect customer transactions
- secret controller or external secret path is failing broadly
- rollback requires old credential reactivation under incident conditions

## Production Context
Teams often think rotation is done once the secret object changes. In reality, the application consumption path, reload behavior, and connection lifecycle matter just as much as the secret source.

## Symptoms

- auth failure spike after secret rotation
- pods appear healthy
- some pods work, some fail
- secret object shows updated value/version
- restart fixes some pods temporarily
- connection pools keep old credential state

## Likely Root Causes

- app only reads secret at startup
- env-var-based secret requires restart
- wrong key/version referenced
- secret sync delay from controller
- sidecar injection or mounted file not reloaded
- cached connections still use old credential
- multiple secret sources drifted

## Investigation Flow

1. confirm exact rotation timeline
2. compare source secret, Kubernetes secret, and app config
3. identify secret consumption model:
   - env var
   - mounted file
   - external sidecar / agent
4. compare working vs failing pods
5. inspect secret-sync controller status
6. inspect app logs for auth failure pattern
7. decide whether safe rollout/restart is required
8. restore compatibility path only as emergency measure

## Safe Fixes

- restart workloads that only read secret on boot
- correct key/version reference
- shorten secret sync gap
- update app to reload credentials safely
- temporarily support dual credentials if system design allows

## Unsafe Fixes to Avoid

- repeated blind rotations
- assuming secret object update means app consumed it
- keeping old secret active indefinitely
- hot-patching credentials in pods manually

## Verify Recovery After Fix

- auth failures return to baseline
- all active pods use new credential path
- no split-brain between old and new secret versions
- old credential can be retired safely after confirmation
- dashboards stay clean through one connection-recycle window

## Prevention / Permanent Fix

- document secret-consumption model per workload
- standardize rotation patterns by runtime
- test rotation in pre-prod
- alert on auth failure spikes immediately after rotation

## Senior Interview Talking Point

A senior engineer treats rotation as an **end-to-end lifecycle problem**, not a secret-object update.

---

# 10) Deployment Completes but Traffic Still Hits the Old Version

## Severity / Blast Radius
**SEV-3 to SEV-2**, but critical for release trust and incident mitigation.

## Primary Owner
**Platform / Release Engineering**

## Consulted
- Application team
- Networking / CDN team if external cache is involved

## Escalate When
- hotfix appears deployed but user issue persists
- rollout controller or traffic-split logic is inconsistent
- multiple traffic layers may still route old response path

## Production Context
This is a classic release-validation failure. Pods are healthy, rollout says complete, but real traffic still reaches the old backend or stale response layer.

## Symptoms

- new pods are ready
- deployment says successful
- logs show most traffic still on old version
- canary/stable traffic split looks wrong
- user cannot observe intended fix
- cache or ingress path appears stale

## Likely Root Causes

- service selector still targets old labels
- rollout controller paused or misconfigured
- ingress/backend still routes to stable service
- sticky sessions pin traffic
- CDN/cache still serving old content
- validation happened against wrong environment/domain

## Investigation Flow

1. confirm which version is receiving traffic
2. inspect service selector and endpoints
3. inspect rollout controller state
4. inspect ingress/backend mapping
5. inspect cache/session affinity behavior
6. verify environment/domain being tested
7. validate version in headers, logs, and metrics
8. fix traffic path before redeploying again

## Safe Fixes

- correct service selector
- repair or resume rollout traffic-shift logic
- fix ingress/service mapping
- invalidate stale cache where appropriate
- expose version metadata for response verification

## Unsafe Fixes to Avoid

- redeploying repeatedly without traffic validation
- assuming healthy new pods mean user traffic moved
- changing labels manually without understanding selector consequences

## Verify Recovery After Fix

- new version receives expected traffic share
- user-visible fix is observed end-to-end
- cache/session behavior no longer masks release
- rollout metrics and app logs agree on serving version
- incident validation succeeds from multiple test paths

## Prevention / Permanent Fix

- standardize label/selector strategy
- add post-deploy traffic validation checks
- expose serving version in response headers or metrics
- test cache/CDN interaction during release validation

## Senior Interview Talking Point

Senior engineers know **deployment success and traffic cutover are different events**. Release safety requires validating the real user path.

---

# 11) Pods Stuck Terminating and Blocking Rollout

## Severity / Blast Radius
**SEV-2**, sometimes **SEV-1** for high-traffic or singleton critical services.

## Primary Owner
**Application team + Platform shared**

## Consulted
- SRE
- Storage team if volume detach is involved

## Escalate When
- rollout is blocked for critical service
- termination takes longer than deployment safety window
- volume detach or finalizer issue affects multiple workloads

## Production Context
A rollout begins, but old pods never exit cleanly. New pods may be ready, yet rollout is blocked because connections, finalizers, sidecars, or storage detach paths hang.

## Symptoms

- old pods remain in `Terminating`
- rollout hangs at partial completion
- connection draining exceeds expected window
- old pods still receive traffic or hold resources
- forced delete is repeatedly considered

## Likely Root Causes

- app ignores SIGTERM
- preStop hook runs too long or hangs
- long-lived connections never drain
- sidecar prevents clean exit
- finalizer or volume detach blocks deletion
- pod has stuck process shutdown path

## Investigation Flow

1. inspect pod deletion timestamp and grace period
2. inspect preStop behavior and app termination logs
3. verify whether pod still serves traffic
4. inspect finalizers and attached volumes
5. compare termination timing against LB draining
6. decide whether forced deletion is safe
7. fix shutdown contract before next rollout

## Safe Fixes

- remove pod from traffic path first
- correct SIGTERM handling
- align preStop and terminationGracePeriodSeconds
- fix finalizer or volume detach issue with evidence
- use forced deletion only as controlled last resort

## Unsafe Fixes to Avoid

- force deleting immediately in every case
- shortening grace period below actual drain time
- treating termination hang as just a Kubernetes problem when app ignores shutdown

## Verify Recovery After Fix

- terminating pods exit within expected window
- rollout completes cleanly
- no connection reset spike occurs during shutdown
- replacement pods receive traffic normally
- subsequent deployment repeats cleanly

## Prevention / Permanent Fix

- test graceful shutdown under load
- align app, proxy, and LB draining behavior
- alert on excessive termination duration
- review finalizers and sidecar lifecycle behavior

## Senior Interview Talking Point

This issue reveals whether you understand **shutdown as part of availability engineering**, not just startup.

---

# 12) Persistent Volume Attach / Mount Failure for Stateful Workload

## Severity / Blast Radius
**SEV-2 to SEV-1** depending on service criticality.

## Primary Owner
**Platform / Storage shared**

## Consulted
- Application team
- Cloud infrastructure team

## Escalate When
- stateful service cannot recover
- attach/mount failure affects multiple nodes or zones
- data integrity risk exists

## Production Context
Stateful workloads often fail differently from stateless ones. Pods can stay pending or container-creating because storage is not attached, mounted, or zonally aligned.

## Symptoms

- pod stuck in `ContainerCreating` or `Pending`
- events show volume attach or mount timeout
- failover to another node does not complete
- rollout of statefulset stalls
- service degraded because only one replica has data access

## Likely Root Causes

- zonal mismatch between PVC and target node
- CSI driver issue
- stale volume attachment
- cloud API delay or error
- access mode conflict
- node permissions or device mount issue

## Investigation Flow

1. inspect pod events and PVC/PV status
2. verify storage class and zone alignment
3. inspect CSI controller/node logs
4. inspect cloud-side attachment state
5. determine whether old node still holds attachment
6. avoid manual mutation that risks data corruption
7. restore clean attach path before retrying failover

## Safe Fixes

- schedule workload to compatible zone
- clear stale attachment through approved procedure
- fix CSI or node permission issue
- restore healthy node/storage path
- fail back only after validating clean mount

## Unsafe Fixes to Avoid

- manual attach/detach with no storage-owner review
- forcing multi-attach on unsupported volume type
- deleting PVC/PV casually during incident

## Verify Recovery After Fix

- volume attaches and mounts cleanly
- pod transitions to ready state
- application can read/write correctly
- failover path tested once stable
- no storage integrity alarms remain

## Prevention / Permanent Fix

- align storage design with zone strategy
- monitor CSI attach/mount latency
- document storage incident runbooks
- rehearse failover for stateful workloads

## Senior Interview Talking Point

Senior engineers know stateful incidents require extra caution because **restoring service must not compromise data integrity**.

---

# Patterns Behind Repeated Kubernetes Production Incidents

Repeated incidents usually come from a small number of systemic weaknesses:

- probes that do not reflect real runtime behavior
- rollout safety designed around pod state instead of user traffic
- autoscaling based on weak or misleading signals
- platform and application ownership boundaries left vague
- secret rotation treated as object update instead of lifecycle change
- node-pool and scheduling design that ignores actual workload classes
- insufficient DNS, ingress, and storage observability
- shutdown behavior never tested under load
- release validation stopping at “pods are ready”

---

# Post-Incident Checklist

After stabilization, a senior-quality response includes:

1. Did we confirm full user recovery, not just pod recovery?
2. Was rollback or mitigation time acceptable?
3. What evidence was captured before mutation?
4. Which guardrail failed:
   - probe design
   - rollout design
   - autoscaling
   - policy
   - secret lifecycle
   - storage/network dependency
5. Does a template, policy, or platform default need to change?
6. Does the runbook need an update?
7. Should this incident create:
   - new alert
   - new dashboard
   - new test
   - new release gate
8. Is ownership clearer now than before the incident?

---

# How to Use These 12 Issues in Interviews

A strong answer usually follows this pattern:

1. explain the business and production context
2. describe the symptom visible to users
3. identify the most likely failure domains
4. show how you reduced blast radius first
5. explain the investigation order across layers
6. mention unsafe fixes you avoided
7. describe the recovery validation
8. end with the permanent fix and platform lesson

---

# Closing Summary

These 12 issues are useful because they test more than Kubernetes syntax. They test whether you can:

- debug across application, cluster, network, storage, and release layers
- protect customers before optimizing elegance
- validate full recovery after the fix
- avoid dangerous incident shortcuts
- turn repeated incidents into better platform design

That is the standard expected from a strong **Senior Kubernetes Platform / DevOps / SRE engineer**.
