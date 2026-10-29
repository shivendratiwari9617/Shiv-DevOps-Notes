# Senior Kubernetes Platform, DevOps, and SRE Interview Answers

## Architecture and tenancy

### 1. What happens when `kubectl apply` creates a Deployment, from authentication and admission through persistence, reconciliation, scheduling, and pod startup?

`kubectl` sends a request to the API server; authentication, authorization, schema checks, and admission run before the desired object is stored in etcd. Controllers reconcile Deployment to ReplicaSet and Pods; the scheduler binds unscheduled Pods to eligible nodes. Kubelet asks the runtime to pull images, mount volumes, configure networking, and start containers; readiness gates traffic. Each stage is asynchronous. Diagnose by observing object events and status at the failed boundary, not by treating successful `apply` as a healthy release.

### 2. How would you design cluster and namespace boundaries for teams with different reliability, security, and compliance requirements? What isolation do namespaces not provide?

Namespaces provide API naming and policy scope, not separate kernels, etcd, control planes, or guaranteed noisy-neighbor isolation. Use namespaces with quotas, RBAC, network policies, admission and workload identity for compatible tenants; separate clusters or nodes for stronger trust, compliance or failure-domain needs. Test effective access and zone-failure impact. Set ownership and cost boundaries without multiplying clusters needlessly. An exception for privileged workloads must not silently expose unrelated product teams.

### 3. When would a capability belong in a controller, operator, admission policy, or external service? How would you limit the impact of its failure?

Use admission for fast deterministic validation or mutation, controllers for asynchronous desired-state reconciliation, and operators only when a domain-specific resource lifecycle warrants one. Keep external policy or dependency calls off synchronous API requests when possible. Scope watched resources, rate-limit retries, use timeouts and health signals, and decide fail-open versus fail-closed explicitly by risk. Test dependency failure and controller restarts; a broken extension should not block unrelated customer fixes.

### 4. API server latency rises after a new controller is deployed. How would you investigate request volume, watches, admission dependencies, and control-plane saturation?

Correlate the rollout with API request rates by verb/user agent, watch churn, 429s, admission webhook latency, etcd latency and control-plane CPU. Inspect the controller's list/watch and reconcile loops for broad scans or hot retries. Scale or temporarily pause the offending controller if it is safe, preserving evidence and avoiding mass restarts. Add cache, backoff and narrow watches, then measure API p99 and deployment capability. Verify existing workloads and customer paths remain healthy.

### 5. How would you choose managed versus self-managed Kubernetes when operational ownership, customization, recovery, and compliance requirements differ?

Compare control-plane support and SLA, upgrade responsibility, access to etcd, network and storage customization, residency, and total staffing cost. Managed clusters reduce control-plane operations but do not manage workloads, add-ons or application recovery for you; self-management allows deeper control but requires tested quorum, backup and upgrade skills. Pilot the hardest requirement and a region failure. Choose based on customer RTO/RPO and risk rather than preference for a particular cloud.

## Scheduling and capacity

### 6. A pod remains Pending even though cluster-wide CPU is available. How would you inspect requests, node allocatable resources, taints, affinity, topology constraints, and quota?

Read `kubectl describe pod` scheduling events and compare per-node allocatable minus requested CPU/memory, not fleet-wide idle usage. Check required affinity, topology spread, taints/tolerations, PVC zone and namespace quota or limit ranges. A pod's full request must fit on one eligible node. Change capacity or constraints only after verifying which one blocks placement; then confirm it schedules without evicting customer-critical pods.

### 7. How do requests, limits, QoS, eviction, and CPU throttling influence workload behavior under node pressure?

Requests govern scheduling and influence eviction priority; CPU limits enforce quota and can throttle even on an otherwise idle node, while exceeding memory limits can trigger OOM kills. QoS and actual usage affect eviction decisions under node pressure but do not guarantee uninterrupted service. Compare cgroup throttling, OOM events, node pressure and customer latency before tuning. Set requests from observed peak and resilience needs and keep headroom for failover.

### 8. Traffic rises but the HPA does not add replicas. How would you investigate metrics, target configuration, resource requests, and downstream capacity?

Inspect HPA conditions, current versus desired metrics, metrics-server or adapter freshness, CPU requests, min/max replicas and scaling behavior windows. CPU utilization cannot be computed reliably for a container without its request. Fix missing metrics or requests and simulate load to verify scaling, then confirm nodes can provision and downstream databases tolerate added concurrency. More replicas may amplify a shared bottleneck and worsen customer latency.

### 9. A cluster autoscaler cannot provision nodes during a spike. How would you distinguish workload constraints, cloud quotas, node group configuration, and unavailable capacity?

Read pending-pod events and autoscaler logs for unschedulable predicates and failed scale-up reasons. Check node-group max and instance type fit, zone/PVC constraints, cloud quota, capacity and provisioning errors. If the chosen group cannot ever fit the pod, raising its maximum does nothing. Mitigate with a compatible group or reserved capacity and enforce admission for impossible requests. Verify pods become Ready and traffic recovers before declaring scale-up fixed.

### 10. How would you combine replica scaling, node scaling, disruption budgets, and capacity headroom for a service with bursty traffic?

Size replicas for customer p99 and dependency limits, nodes for cold-start plus zone-loss headroom, and PDBs for the minimum safe availability during voluntary disruption. HPA reacts to metrics, autoscaler to unschedulable Pods; neither provides instant capacity. Pre-provision for predictable peaks and test rollout and node drain during a burst. Monitor saturation, scale-up delay and pending age. Do not set a PDB that makes necessary recovery or maintenance permanently impossible.

## Networking and storage

### 11. Trace a request from ingress to a pod through service discovery and networking. Where would you collect evidence if only some requests fail?

Follow DNS to load balancer/ingress, its route and TLS config, Service endpoints/EndpointSlices, kube-proxy or CNI forwarding, then Pod listener and application. Segment failures by source, zone, endpoint, version, and connection reuse; compare ingress logs, network policy and pod traces. A Ready endpoint may still fail the real customer request. Remove a bad endpoint or roll back a correlated change and verify the full path, including response, before closing.

### 12. Pods can reach IP addresses but cannot resolve service names. How would you investigate DNS configuration, CoreDNS, network policies, and upstream resolution?

From an affected Pod check `/etc/resolv.conf`, namespace-qualified service names, DNS query errors, and CoreDNS availability and latency. Compare unaffected namespaces and node pools, then inspect policy for UDP/TCP 53 to DNS and CoreDNS-to-upstream paths. Cluster service names should not require external upstream resolution; separate those failure modes. Restore the failing hop and test both internal names and customer flows. Avoid masking a cluster DNS outage with per-Pod host entries.

### 13. How would you design and test network policies for a multi-tenant cluster without interrupting critical dependencies?

Inventory ingress/egress flows including DNS, control-plane or metadata endpoints, observability and payment dependencies before default-deny. Apply scoped policies to a canary namespace, test allowed and blocked flows from actual workloads, and inspect CNI enforcement; a policy object alone does not ensure support. Version changes and define an emergency revert. Measure denied flows and customer errors after rollout. Treat egress and return-path semantics according to the installed CNI.

### 14. A stateful workload cannot mount its volume after rescheduling. How would you investigate the PVC, storage class, CSI driver, topology, attach state, and filesystem?

Inspect Pod, PVC, PV and VolumeAttachment events plus CSI controller/node logs. Compare PV zone with scheduled node, access mode, old node attachment and cloud storage health. Protect data: do not force detach while an old writer may still be active. Confirm fencing, then repair attachment or scheduling and check filesystem and app consistency. A mounted volume is not proof the database recovered correctly; verify writes and replication.

### 15. How would you validate backup and restore for a stateful application whose Kubernetes objects and persistent data must recover consistently?

Back up manifests and secrets/config plus application-consistent data and encryption keys with timestamped dependency versions. Restore into an isolated cluster, preserve storage topology and one-writer guarantees, and replay logs only to a tested point. Measure RPO at the last durable transaction and RTO when customers can complete the workload, not when Pods are Running. Test restore after a zone failure and document unsupported schema rollbacks.

## Security and delivery

### 16. How would you design service-account permissions, workload identity, secrets access, and RBAC for applications and operators in a shared cluster?

Give each workload a dedicated service account and minimal Kubernetes permissions; disable automount where API access is unnecessary. Use workload federation or short-lived identities for cloud resources, scope secret reads, and separate operator cluster-wide roles from tenant roles. Test effective access by impersonation and a compromised Pod scenario. Audit grants and rotate credentials. Namespace membership alone does not constrain a cluster-scoped binding or mounted cloud token.

### 17. An admission webhook becomes unavailable and deployments fail. How would you evaluate failure policy, restore delivery safely, and reduce future blast radius?

Check webhook service endpoints, TLS, timeout and `failurePolicy`, plus API admission latency and which operations are blocked. Restore the webhook or temporarily narrow scope with an approved policy exception; fail-open may admit unsafe workloads, while fail-closed may block urgent customer fixes. Preserve a documented break-glass action with audit and expiry. Run webhook replicas across failure domains, bound timeouts, and rehearse loss. Verify both releases and security policy after restoration.

### 18. How would you enforce workload security and image provenance while providing a documented, time-bound exception process?

Require trusted signed image digests, restricted Pod security, non-root and capability limits where supported, and CI tests for policy compliance. Verify signatures and provenance at admission, while recognizing that a signed artifact can still be vulnerable. For privileged exceptions require owner, justification, isolation, compensating controls, expiry and review. Track exception age and patch coverage. Canary policy updates to avoid blocking production fixes through a faulty rule.

### 19. A deployment succeeds but user errors rise. How would you use rollout status, readiness, application signals, and traffic management to decide whether to pause or roll back?

Pause the rollout, compare errors and p99 by version, zone and route, and inspect ReplicaSet availability and readiness. Readiness only indicates the configured check passes; compare real transactions and dependency health. Roll back to a known-compatible digest if data changes permit, or use a forward fix and traffic isolation otherwise. Confirm errors and incomplete orders recover and preserve the failed version's evidence. An API success or six Ready Pods is not a release success.

### 20. How do readiness, liveness, and startup probes serve different purposes? What failure can result from using the same check for all three?

Readiness determines whether to receive traffic, liveness triggers restart when a process cannot recover, and startup grants slow initialization time before liveness applies. If all use a remote database check, a shared DB outage may restart every healthy application Pod and amplify the incident. Make liveness local and conservative, readiness reflective of ability to serve, and startup sized for cold boot. Test dependency loss and slow start under load.

### 21. How would you design GitOps ownership and environment promotion while managing emergency changes and avoiding competing controllers?

Assign one authoritative reconciler and repo per resource, use reviewed environment-specific configuration, and promote the same artifact digest. Detect drift and record which controller applied each change; two controllers fighting over fields can continuously revert fixes. For emergencies, pause reconciliation or commit the approved change through a break-glass path with audit and follow-up reconciliation. Test rollback and secret handling. Measure time to safe production changes and drift resolution.

### 22. How would you plan a production cluster upgrade covering version compatibility, add-ons, node replacement, workload disruption, verification, and recovery limitations?

Check version skew for API clients, controllers, CRDs, CNI, CSI and webhook APIs; test representative workloads on staging. Upgrade control plane and nodes in supported order with zone-by-zone drains, PDB and capacity headroom checks. Watch API latency, storage mounts, DNS, customer SLIs and rollout ability at each gate. Some control-plane or etcd migrations cannot simply be downgraded; document a forward recovery strategy. Rehearse a node failure during upgrade.

## SRE, cost, and leadership

### 23. How would you define platform and workload SLIs that distinguish cluster health from customer experience? How would error budgets affect change decisions?

Platform SLIs include API availability, scheduling delay, DNS success, node provisioning and volume attach; product SLIs measure real checkout success and latency. Correlate them so a green control plane cannot hide a failing namespace. Use error-budget burn to decide whether to pause high-risk platform changes and prioritize reliability work with product owners. Avoid treating every Pod restart as a customer outage; test end-to-end impact.

### 24. A cluster shows healthy control-plane metrics but one namespace experiences widespread errors. How would you narrow the issue using workload, node, network, and dependency evidence?

Segment affected deployments by node, zone, version and ingress path; compare events, resource limits, probes, Service endpoints, policies and namespace quota. Check shared dependencies, secret rotation and DNS from an affected Pod against an unaffected namespace. Preserve a customer error timeline and mitigate the narrowest common cause, such as a bad policy or tenant-specific credential. Verify transaction success, not only cluster metrics, after correction.

### 25. A pod repeatedly restarts with `OOMKilled`, but memory graphs show no sustained peak. What evidence and measurement limitations would you investigate?

Check `lastState`, exit reason, cgroup `memory.events`, kernel OOM evidence and container versus node limits. Short spikes, sampling gaps and memory used by page cache can be missed by dashboard averages; compare high-resolution samples and heap allocation. Distinguish eviction from limit OOM. Fix a leak or request/limit mismatch with node headroom in mind, then verify restart rate and customer p99 under comparable load.

### 26. How would you reduce Kubernetes cost without compromising availability? Which utilization, scheduling, and reliability signals would guide your changes?

Find over-requested resources by comparing requests with peak and p99 usage, throttling, OOMs and failover headroom. Right-size workloads and select suitable nodes or schedules, then test a node/zone loss and a traffic spike before shrinking reserves. Review idle persistent volumes, logs and network egress too. Track cost per customer transaction alongside SLO compliance and release disruption. Higher utilization that removes recovery capacity is not a saving.

### 27. An entire region becomes unavailable. How would you restore a Kubernetes-hosted service, including traffic, persistent data, external dependencies, and verification of RTO/RPO?

Confirm region failure and fence old writers, then activate pretested traffic failover to a healthy region with sufficient capacity. Restore or promote data with replication-lag and consistency checks, and verify identity, DNS, payment and secret dependencies in the new region. Measure RPO by last durable business event and RTO by successful customer transactions. Avoid split-brain during failback. Rehearse the full customer journey, not just cluster object recovery.

### 28. Tell me about a platform standard you introduced across application teams. How did you balance adoption, exceptions, and the operational cost of supporting it?

Use a real standard you worked on, its initial pain and team concerns, the pilot and migration path, and why exceptions were allowed. Show before-and-after lead time, policy failures, incident rate or support load, acknowledging what did not improve. Describe your own contribution and how you changed the standard after feedback. Do not fabricate personal experience; a hypothetical design answer is distinct from this behavioral question.

### 29. During a cluster-wide DNS incident, how would you coordinate mitigation, preserve diagnostic evidence, communicate impact, and decide when service is restored?

Identify customer-facing lookup failures by namespace and node, freeze unrelated changes and gather CoreDNS logs, query latency, network policy and upstream evidence. Restore the broken path or scale DNS with node and upstream headroom; avoid blindly restarting all workloads. Communicate affected customer journeys and uncertainty on a regular cadence. Verify internal and external DNS plus real service transactions across zones before closing. Record any telemetry gaps for the review.

### 30. How would you measure whether a Kubernetes platform improves developer delivery and reliability rather than merely increasing infrastructure utilization?

Baseline time from merged code to healthy production, deployment failure and recovery rates, on-call effort, and customer SLOs for representative teams. Compare after adoption while accounting for differences in product traffic and maturity. Include onboarding time, self-service success and platform cost per workload. High bin packing can lower costs but hurt burst or failure recovery; test both. Prioritize improvements that shorten safe releases and reduce customer harm.

## Evidence-based exercises

### 31. A pod requests `cpu: 1500m`; each eligible node has less than `1000m` of allocatable CPU remaining, even though cluster-wide unused CPU exceeds `3000m`. What scheduling result would you expect, how would you verify it, and which safe capacity or workload changes would you consider?

The Pod stays Pending because the full 1500m request must fit on one eligible node; unused CPU cannot be combined across nodes. Confirm `FailedScheduling` events and per-node allocated requests, affinity, taints and quota. Add an eligible node, safely evict/move other work, or reduce the request only if measured peaks justify it. Check node/zone failure headroom and Pod readiness after the fix.

### 32. An HPA targets 60% CPU utilization. Over several minutes, its status reports `<unknown>/60%`, and new pods do not have CPU requests configured. What additional evidence would you gather, how would you test the cause, and how would you verify scaling without overloading dependencies?

Inspect HPA conditions, metric adapter health and target Pod CPU requests; utilization needs a request for the relevant containers. Add justified requests after checking actual load and quotas, then generate controlled traffic and observe metric availability, desired replicas and Pod readiness. Verify node autoscaling and downstream connection saturation. `<unknown>` is missing metric evidence, not proof traffic is low; customer errors must remain the independent gate.

### 33. A Service has a healthy endpoint, but requests from one namespace time out while requests from another succeed. What evidence would you collect about DNS, network policies, CNI enforcement, and the return path before making a change?

Compare DNS resolution and direct endpoint/Service connections from both namespaces, recording source Pod labels, IP and protocol. Inspect ingress and egress policies on both sides, CNI flow logs, node/zone placement and return-path filtering. A healthy endpoint says its readiness probe passed, not that every source is allowed to reach it. Change the narrow rule or route causing the timeout, then test the affected customer's request and a denied path.

### 34. In a self-managed cluster, etcd is unavailable after a storage failure, and the most recent snapshot is 30 minutes old. How would you assess quorum, snapshot integrity, application impact, recovery point, and safe restoration before modifying the cluster?

Identify etcd member and quorum health, surviving data directories, disk errors, and API availability before choosing a restore; if quorum can be safely restored from current members, do not overwrite newer cluster state with a 30-minute-old snapshot. Validate snapshot integrity and revision, preserve failed-member data, and stop or fence the old control planes before a coordinated restore under the cluster's documented procedure. Rewinding etcd can lose object and secret changes and confuse watches or controllers, so inventory changes since the snapshot and verify controllers reconcile intentionally rather than recreating or deleting live resources unexpectedly. Kubernetes object-state age is not necessarily business-data RPO: check database volumes and their own recovery points separately. Test API, scheduling, storage attach and an actual customer write with one authoritative data writer before reporting RTO or data loss.

### 35. A rollout shows `3/6` available replicas and `ProgressDeadlineExceeded`; events show both image-pull failures and readiness failures. How would you prioritize the two failure paths, decide whether to pause or roll back, and verify customer recovery?

Pause rollout and check current customer errors and old Ready capacity. Separate Pods unable to pull the digest from Pods that start but fail readiness; verify registry credentials and image identity for the first, app logs and dependency checks for the second. Roll back if the new version is harming traffic and the previous version is data-compatible, or restore pull access and fix readiness under a safe canary. Confirm six required customer-capable replicas and transaction SLIs.

## Product-company practical exercises

### 36. A Deployment shows `replicas: 6` and `maxUnavailable: 0`; during a release, `kubectl get pods` shows six old Ready pods and two new pods stuck Pending. Customer traffic is still healthy. How would you investigate and complete or safely halt the rollout?

The old six protect traffic while surge Pods are unschedulable; check `describe pod` events, surge settings, per-node requests, affinity, topology, quota and storage constraints. Do not delete healthy old Pods to force capacity while `maxUnavailable: 0` preserves availability. Add compatible capacity or correct an unjustified constraint and watch new readiness and customer errors. If it cannot be fixed safely, pause or undo the rollout and record the pending cause.

### 37. A product team presents a pod configuration with `requests.cpu: 100m`, `limits.cpu: 100m`, and observed CPU throttling during traffic spikes. What further evidence would you collect before changing resource settings, and how would you measure customer impact and cost?

Correlate cgroup throttled periods with p99 latency, queue depth, traffic peaks and node headroom. A 100m CPU limit may throttle despite idle host CPU; compare app CPU demand and dependency time to avoid blaming every slow request on the cap. Canary a higher limit or remove it within node-budget policy while sizing request from sustained needs. Measure latency, neighbor contention and cost per transaction before fleet rollout.

### 38. A platform team supports 40 product teams and receives repeated requests for privileged workloads. How would you design a review and exception process, define ownership, and measure whether the platform is safe without blocking legitimate delivery?

Require the exact privileged operation, threat model, alternative capability and test of a least-privilege option. For genuine exceptions isolate nodes or clusters, assign an owner, expiry and compensating controls, and audit actual use. Publish a fast review SLA and templates so normal releases are not blocked by ad hoc judgment. Track privileged workloads, exception age, incidents and time to resolve requests. Reject blanket `privileged: true` when a narrower capability suffices.

### 39. One availability zone degrades during a release: customer errors rise, pods remain Pending, and a PodDisruptionBudget blocks node maintenance. How would you prioritize customer recovery, assess capacity and disruption risk, and verify the service before resuming the rollout?

Pause rollout, isolate the failing zone from traffic where safe, and determine whether healthy zones have capacity for required replicas and data access. Check pending-Pod scheduling reasons, PDB allowed disruptions, and storage topology; bypassing the PDB during degradation can drop below the minimum service capacity. Add eligible capacity or correct placement, then perform controlled drain if justified. Verify checkout success and latency across remaining zones before lifting the rollout pause.