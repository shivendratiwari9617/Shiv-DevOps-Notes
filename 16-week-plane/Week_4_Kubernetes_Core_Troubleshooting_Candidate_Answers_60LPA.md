**Week 4 — Kubernetes Core Troubleshooting — Candidate Answers (60 LPA+)**

**Q1 \| Pods \| P1 \| Production Troubleshooting**

**Interview Question:**

> A production pod is stuck in Pending after a normal release while other workloads are running; how would you isolate whether the blocker is insufficient resources, scheduling constraints, PVC binding, taints, or quota limits?

**Candidate Answer:**

I would not start by deleting the pod or changing random constraints. I would first confirm impact and whether this is isolated to the new release or a wider scheduler issue by checking deployment status, pending count, namespace quota, and recent events.

My main source of truth would be \`kubectl describe pod\` and scheduler events. I would look for exact reasons such as \`Insufficient cpu\`, \`Insufficient memory\`, \`didn't match node selector\`, untolerated taints, PVC not bound, or exceeded quota. Then I would correlate with \`kubectl get quota\`, \`kubectl get pvc\`, \`kubectl describe node\`, taints, labels, and affinity rules. If total cluster capacity exists but not on eligible nodes, I would treat it as placement or topology fragmentation, not raw capacity.

For mitigation, I would either temporarily relax non-critical affinity, scale a safe node pool, fix quota, or revert the scheduling change. I would validate by watching scheduler events, pod readiness, endpoints, and traffic. Permanent prevention would be admission checks for invalid node selectors, capacity dashboards by eligible node pool, and pre-deployment checks for PVC and quota.

**Q2 \| Pods \| P1 \| Production Troubleshooting**

**Interview Question:**

> A newly deployed pod enters CrashLoopBackOff only in production, but the same image works in staging; how would you separate application crash, config mismatch, secret issue, resource limit, and node-level dependency causes?

**Candidate Answer:**

I would separate this into image correctness, runtime configuration, and production-only dependencies. First I would check \`kubectl logs --previous\`, container exit code, restart count, pod events, and whether the crash starts before or after the application initializes.

Because the image works in staging, I would compare production ConfigMaps, Secrets, environment variables, mounted volumes, service accounts, feature flags, and external dependency endpoints. If the exit code is 137 or the pod shows OOMKilled, I would focus on memory limits and traffic-specific startup behavior. If it is exit code 1 with missing config, auth, or certificate errors, I would inspect secret versions and mounted paths. I would also check node-level differences such as CSI mounts, DNS, proxy settings, or kernel/runtime issues on specific nodes.

For mitigation, I would pause rollout, keep healthy old replicas serving, and rollback if the new replicas cannot become ready. After recovery, I would add config parity checks, secret validation, startup smoke tests, and a progressive rollout gate that blocks production if the same image cannot boot with production config.

**Q3 \| Pods \| P1 \| Production Troubleshooting**

**Interview Question:**

> A pod restarts every few minutes during peak traffic without clear application logs; how would you determine whether the issue is liveness probe failure, OOM kill, node pressure eviction, or application deadlock?

**Candidate Answer:**

I would first build a restart timeline instead of assuming the application is crashing. I would check restart count, \`lastState\`, termination reason, exit code, pod events, \`kubectl logs --previous\`, node events, and metrics around the exact restart time.

If events show liveness probe failures, I would inspect probe timeout, failure threshold, path behavior under load, and whether the probe depends on downstream systems. If the reason is OOMKilled, I would compare peak memory, container limit, RSS growth, GC behavior, and request rate during peak rather than average memory. If events show eviction, I would inspect node memory/disk/PID pressure and noisy neighbors. If there are no clear events and requests hang before restart, I would suspect deadlock or blocked worker pools and use thread dumps, pprof/JFR, or application diagnostics.

Mitigation depends on proof: disable aggressive liveness temporarily, raise memory limit with rollback guardrails, move workload away from pressured nodes, or rollback the release. I would validate using restart rate, latency, error rate, and node pressure metrics, then add better probes, peak-memory alerts, and load-test coverage.

**Q4 \| Pods \| P1 \| SRE**

**Interview Question:**

> A critical pod is running but user traffic is failing intermittently; how would you prove whether the pod is actually ready to serve traffic or only passing a weak readiness condition?

**Candidate Answer:**

I would challenge the assumption that Running means serviceable. For a critical pod, I would validate actual readiness from the user path backward: load balancer target health, Service endpoints, EndpointSlice readiness, pod readiness condition, application health endpoint, and dependency readiness.

A weak readiness probe often checks only process liveness, so I would compare readiness success with real signals such as 5xx rate, p95/p99 latency, failed dependency calls, connection pool saturation, cache warm-up completion, and synthetic transaction results. I would also check whether traffic is reaching terminating pods, whether \`preStop\` and termination grace are configured, and whether endpoint updates are delayed during rollout.

For immediate mitigation, I would remove unhealthy pods from service by tightening readiness, pausing rollout, or scaling stable replicas. I would not restart all pods blindly because it can increase intermittent failure. Long term, readiness should represent “can serve this API safely,” not “process is alive.” I would add blackbox probes, dependency-aware readiness with sensible degradation rules, and release gates based on SLO and business metrics, not only Kubernetes status.

**Q5 \| Deployments \| P1 \| Production Troubleshooting**

**Interview Question:**

> A deployment rollout is stuck with old and new replicas both present, and the release window is closing; how would you identify the rollout blocker and decide whether to pause, rollback, or continue?

**Candidate Answer:**

I would first protect the release window by pausing the rollout and checking whether customer traffic is already impacted. Then I would inspect \`kubectl rollout status\`, ReplicaSet events, unavailable replicas, maxSurge/maxUnavailable, pod readiness failures, image pull errors, scheduling failures, and PDB constraints.

Old and new replicas present is normal during rolling update, but it becomes a blocker when new pods never become ready or old pods cannot terminate. I would identify whether the blocker is readiness probe failure, insufficient capacity for surge pods, a bad selector, stuck terminating pods, PVC attach issues, or dependency startup failure. I would also compare the new ReplicaSet template hash against the intended image, config, and secret versions.

The decision is risk-based. If new pods are failing readiness or business metrics degrade, I would rollback. If the issue is capacity only and old pods are healthy, I may temporarily increase node capacity or reduce maxSurge. If the cause is still unknown and release window is closing, I would pause and preserve stable state. RCA would add rollout budgets, preflight capacity checks, and canary validation.

**Q6 \| Deployments \| P1 \| Implementation**

**Interview Question:**

> A team wants zero-downtime rollout for a high-traffic API, but deployments still cause brief 5xx spikes; how would you redesign rollout strategy, readiness behavior, termination handling, and capacity planning?

**Candidate Answer:**

I would redesign this as a controlled rollout problem, not only a Deployment YAML issue. First I would ensure enough spare capacity for surge pods, readiness only turns true after warm-up, and termination drains existing requests before the container exits. I would also use progressive rollout or canary for very high-traffic APIs.

A concise baseline would look like this:

strategy:

type: RollingUpdate

rollingUpdate: { maxSurge: 25%, maxUnavailable: 0 }

minReadySeconds: 30

template:

spec:

terminationGracePeriodSeconds: 60

containers:

\- name: api

lifecycle:

preStop:

exec: { command: \["sh","-c","sleep 20"\] }

readinessProbe:

httpGet: { path: /ready, port: 8080 }

periodSeconds: 5

failureThreshold: 2

startupProbe:

httpGet: { path: /startup, port: 8080 }

failureThreshold: 30

periodSeconds: 5

I would validate no 5xx spike using ingress metrics, Service endpoint changes, latency, and application logs. Permanent controls would include PDBs, HPA headroom, canary gates, synthetic checks, and alerts on terminating-pod traffic.

**Q7 \| Deployments \| P1 \| Production Troubleshooting**

**Interview Question:**

> After a deployment, only a subset of replicas receive traffic and error rate increases; how would you investigate pod labels, selector mismatch, readiness gates, endpoints, and load distribution?

**Candidate Answer:**

I would start by proving where traffic distribution breaks: Service selector, endpoint population, ingress/load balancer behavior, or application-level routing. I would compare labels on pods with the Service selector and inspect EndpointSlices to see exactly which pod IPs are marked ready.

If only a subset receives traffic, common causes are label mismatch between old and new ReplicaSets, readiness gates excluding some pods, ports changed in the container but Service targetPort still points to the old name, or session affinity/topology-aware routing concentrating traffic. I would check \`kubectl get endpointslice\`, \`kubectl describe service\`, ingress controller upstream list, and per-pod request metrics. I would also verify that new pods are not passing readiness too early and then failing user requests.

Mitigation would be to rollback the label/selector or targetPort change, pause rollout, or temporarily scale healthy replicas. I would validate by confirming all ready endpoints receive traffic and error rate returns to baseline. Prevention includes immutable label conventions, Service selector tests in CI, deployment smoke tests, and dashboards showing requests per pod and endpoint readiness.

**Q8 \| Deployments \| P2 \| SRE**

**Interview Question:**

> A rollout succeeds from Kubernetes perspective but business metrics degrade within minutes; what production signals would you require before considering a Kubernetes deployment successful?

**Candidate Answer:**

I would not call a deployment successful only because Kubernetes shows rollout complete. Kubernetes confirms desired pods are ready, but it does not prove the business transaction is healthy.

Before declaring success, I would require technical and product signals for the release window: error rate, p95/p99 latency, saturation, pod restart rate, ingress 4xx/5xx split, dependency call failures, queue depth, database connection usage, and key business metrics such as checkout success, login success, payment authorization, or order creation depending on the service. I would compare these with pre-release baselines and canary versus stable traffic.

If business metrics degrade within minutes, I would treat it as a failed release even if probes pass. I would pause further rollout, compare traces and logs between old and new versions, and rollback or disable the feature flag if the blast radius is growing. Long term, I would wire deployment success gates to SLO burn rate, synthetic journeys, and application-specific KPIs. This is the difference between platform availability and actual customer availability.

**Q9 \| StatefulSets \| P1 \| Production Troubleshooting**

**Interview Question:**

> A StatefulSet upgrade is blocked because one replica remains unavailable and ordered rollout is not progressing; how would you debug identity, PVC, readiness, and quorum-related risks?

**Candidate Answer:**

For StatefulSet issues, I would be careful because identity, storage, and quorum are involved. I would first check which ordinal is unavailable, whether the rollout is blocked due to OrderedReady behavior, and whether the workload still has quorum or data safety.

I would inspect the failing pod events, PVC binding/attachment status, volume mount errors, readiness probe output, logs, and whether the pod identity still matches expected DNS and persistent volume. If the pod is blocked on readiness, I would distinguish between a real application failure and a readiness probe that is too strict for recovery. For database-like workloads, I would check leader/follower state, replication lag, and whether forcing the rollout could violate quorum.

Mitigation might be pausing the rollout, scaling only if the application supports it, fixing the PVC or node issue, or reverting the image/config for that ordinal. I would not delete PVCs or force-recreate pods without backup and data-owner approval. Prevention includes partitioned rollouts, backup verification, readiness tied to quorum health, and runbooks for ordinal-specific recovery.

**Q10 \| StatefulSets \| P1 \| Architecture**

**Interview Question:**

> A database-like workload is proposed as a Deployment instead of a StatefulSet; what production failure modes would you evaluate before approving that design?

**Candidate Answer:**

I would challenge a Deployment for database-like workloads because Deployments treat replicas as interchangeable, while many stateful systems need stable identity, ordered rollout, and persistent storage ownership.

The production failure modes I would evaluate are volume identity loss, multiple replicas writing to the same storage, DNS identity changes, unsafe parallel replacement, split-brain during rescheduling, backup/restore complexity, and whether the application can tolerate random pod names and unordered startup. I would also check storage access mode, replication model, leader election, quorum requirements, and how failover is handled during node drain or zone outage.

A StatefulSet is usually safer when each replica needs a stable network identity, stable PVC, and ordered lifecycle. However, I would not approve StatefulSet blindly either; managed databases may be a better choice if the team cannot operate backups, upgrades, and failover. My recommendation would be based on RTO/RPO, data criticality, operational maturity, and whether the team has tested recovery. For critical data workloads, the design must include backup validation, PDBs, anti-affinity, monitoring, and documented failover procedures.

**Q11 \| StatefulSets \| P2 \| Production Troubleshooting**

**Interview Question:**

> A StatefulSet pod is rescheduled to another node but cannot attach its volume; how would you reason through storage topology, access mode, cloud volume attachment limits, and recovery risk?

**Candidate Answer:**

I would treat this as a storage attachment and topology incident, not only a pod scheduling issue. First I would check the PVC, PV, StorageClass, volume attachment objects, pod events, CSI driver logs, and cloud provider volume status.

The key questions are whether the volume access mode allows attachment to the new node, whether the PV has node affinity or zonal topology, whether the pod was rescheduled into a different zone, and whether the previous node still has a stale attachment. For cloud disks, many volumes are single-writer and zonal, so a pod moved across zones may never attach. I would also inspect attach/detach controller events, CSI node plugin health, and cloud API quota or volume attach limits.

Mitigation would be to reschedule the pod into the volume’s zone, detach the stale volume safely, or cordon/drain only after confirming data consistency. I would avoid force-detach unless the application owner confirms the old node is not writing. Permanent prevention includes topology-aware scheduling, zone-aligned node pools, PDBs, CSI alerts, and documented recovery for stuck attachments.

**Q12 \| DaemonSets \| P1 \| Production Troubleshooting**

**Interview Question:**

> A logging DaemonSet is missing from several nodes after a node pool upgrade; how would you identify whether the cause is taints, tolerations, node selectors, resource pressure, or admission policy?

**Candidate Answer:**

I would start by listing nodes missing the DaemonSet pod and comparing them with nodes where it is running. That immediately shows whether the issue aligns with a new node pool, labels, taints, architecture, OS image, or resource profile.

I would inspect the DaemonSet desired/current/ready counts, pod events, node labels, taints, tolerations, nodeSelector, affinity, and any admission policy rejections. After a node pool upgrade, common causes are new taints not tolerated, missing labels used by nodeSelector, changed runtime/OS architecture, insufficient reserved resources, or Pod Security policy changes. I would also check whether the DaemonSet has priority class and tolerations for critical node components if it is logging infrastructure.

Mitigation depends on severity. For missing logs, I would deploy a corrected DaemonSet with the proper tolerations and labels, but avoid over-broad tolerations that place it on unsupported nodes. I would validate log ingestion from every node pool and add conformance checks after node upgrades. Long term, node pool upgrade runbooks should include DaemonSet coverage checks before production rollout.

**Q13 \| DaemonSets \| P2 \| SRE**

**Interview Question:**

> A node-level security agent DaemonSet causes CPU pressure across the cluster after an update; how would you mitigate impact while maintaining minimum security coverage?

**Candidate Answer:**

I would treat this as a reliability and security trade-off. The security agent is important, but if it causes CPU pressure across nodes it can degrade every workload and create a larger outage.

First I would quantify impact: node CPU saturation, pod throttling, eviction risk, application latency, and whether the issue is limited to a version, node type, or workload pattern. I would compare DaemonSet resource requests/limits, recent update diff, logs, and per-node CPU usage. Immediate mitigation could be pausing rollout, rolling back the agent version, lowering scan concurrency, adding CPU limits carefully, or canarying the agent to a subset of nodes while maintaining coverage on internet-facing and regulated workloads.

I would communicate the risk clearly with security and incident stakeholders: maintaining minimum security coverage while restoring cluster stability. Permanent prevention would include DaemonSet canary rollout, resource budgets, priority classes, node-level SLO alerts, pre-production performance tests, and exception handling for critical clusters. I would also require vendor/agent release validation before broad deployment.

**Q14 \| Services \| P1 \| Production Troubleshooting**

**Interview Question:**

> A Service exists and pods are healthy, but traffic is not reaching the backend; how would you investigate selector mismatch, endpoint population, target ports, readiness state, and network policy?

**Candidate Answer:**

I would debug the Service from selector to packet path. First I would confirm the Service selector exactly matches pod labels and that EndpointSlices contain the expected pod IPs and ports. Healthy pods do not matter if they are not selected.

Next I would check \`targetPort\` versus the actual container port name/number, readiness status, and whether only ready endpoints are published. I would test from inside the cluster using a temporary debug pod, resolving the service DNS name and connecting to the ClusterIP and directly to pod IPs. If direct pod access works but Service fails, I would inspect kube-proxy/ipvs/iptables health or CNI issues. If access works from some clients but not others, I would check NetworkPolicy, namespace labels, and egress rules.

For mitigation, I would fix selector/targetPort drift, rollback the Service manifest, or temporarily route traffic to a known-good service. I would validate endpoint population, request success, and ingress/backend health. Prevention includes CI checks for selector/label consistency, named ports, service tests, and endpoint alerts.

**Q15 \| Services \| P1 \| Production Troubleshooting**

**Interview Question:**

> A ClusterIP service works from some namespaces but fails from others; how would you isolate DNS, NetworkPolicy, service discovery, namespace scoping, and application configuration issues?

**Candidate Answer:**

I would first determine whether this is name resolution or network reachability. From a debug pod in both working and failing namespaces, I would test the full service FQDN, short name, DNS search behavior, ClusterIP connectivity, and direct pod IP connectivity.

If DNS fails only from some namespaces, I would check namespace DNS policies, CoreDNS logs, search suffix behavior, and whether the application is using a short name that resolves differently by namespace. If DNS works but connection fails, I would inspect NetworkPolicy egress from the client namespace and ingress into the server namespace, including namespace labels and pod selectors. I would also compare service account restrictions, sidecar policies, and application configuration that may point to a namespace-local service.

Mitigation would be to use the correct FQDN, fix namespace labels or NetworkPolicy, or revert the policy/config change. I would validate with synthetic checks from each namespace and traffic metrics. Long term, I would standardize service discovery naming, add namespace-to-service connectivity tests, and review default-deny rollout rules.

**Q16 \| Services \| P2 \| Architecture**

**Interview Question:**

> A team exposes an internal service through NodePort for convenience in production; what reliability and security risks would you challenge, and what safer exposure pattern would you recommend?

**Candidate Answer:**

I would challenge NodePort as a production shortcut because it expands the attack surface and makes traffic management harder. NodePort exposes every eligible node on a high port, which can bypass intended ingress controls, WAF, TLS policy, rate limiting, and centralized logging if not tightly governed.

Reliability risks include dependency on node IP reachability, uneven traffic during node churn, weak health checking, and cloud firewall or security group sprawl. Security risks include accidental public exposure, broad inbound rules, and unclear ownership of who can reach the port. Operationally, NodePort also complicates certificate management and observability.

For an internal production service, I would recommend ClusterIP behind an internal Ingress, Gateway API, service mesh, or cloud internal load balancer depending on use case. That gives controlled routing, TLS, auth, rate limits, logs, and health checks. If NodePort is temporarily required for migration, I would restrict it to private networks, document expiry, add firewall rules, and monitor access. The permanent pattern should be policy-driven exposure, not convenience-based exposure.

**Q17 \| Endpoints \| P1 \| Production Troubleshooting**

**Interview Question:**

> A service has no ready endpoints after deployment even though pods show Running; how would you debug labels, readiness probes, container ports, endpoint slices, and rollout sequencing?

**Candidate Answer:**

Running pods with no ready endpoints usually means the Service cannot publish them as traffic targets. I would first inspect pod labels versus Service selector, then check EndpointSlices to see whether endpoints are absent or present but marked not ready.

Next I would validate readiness probes, container port names, Service targetPort, and any readiness gates. A common production mistake is changing a named container port while the Service still targets the old name, or deploying pods with new labels not selected by the Service. I would also check rollout sequencing: if the new ReplicaSet is running but readiness depends on a database migration, cache warm-up, or external dependency, Kubernetes will correctly withhold endpoints.

Mitigation would be to restore selector/label consistency, correct targetPort, rollback readiness/config changes, or pause rollout while dependencies recover. I would validate with \`kubectl get endpointslice\`, ingress backend health, and live request success. Permanent prevention includes manifest unit tests, named-port conventions, readiness probe review, and deployment gates that fail fast when a Service has zero ready endpoints.

**Q18 \| Endpoints \| P1 \| SRE**

**Interview Question:**

> During an incident, traffic is routed to only one endpoint while several replicas are available; how would you investigate endpoint readiness, topology-aware routing, session affinity, and load balancer behavior?

**Candidate Answer:**

I would first confirm whether Kubernetes sees only one ready endpoint or whether the load balancer is choosing one despite multiple endpoints. That distinction changes the investigation completely.

I would inspect EndpointSlices, pod readiness conditions, per-pod request rates, session affinity on the Service, topology-aware hints, ingress controller upstreams, and external load balancer target health. If several replicas exist but only one is ready, I would focus on readiness probes, dependency availability, or resource pressure. If all endpoints are ready but traffic goes to one, I would check sticky sessions, client-side connection pooling, HTTP keep-alive, topology routing, and ingress/load balancer configuration.

During an incident, I would mitigate by scaling healthy replicas, disabling session affinity if safe, correcting readiness, or routing through a stable backend. I would validate by checking request distribution across pods and customer-facing latency/error rate. Long term, I would add alerts for endpoint imbalance, dashboards showing requests per pod, and review topology-aware routing and affinity settings before enabling them for critical services.

**Q19 \| Ingress \| P1 \| Production Troubleshooting**

**Interview Question:**

> An Ingress starts returning 502 after a backend deployment while direct service access works; how would you debug ingress controller routing, backend service mapping, health checks, TLS, and upstream timeouts?

**Candidate Answer:**

A 502 at Ingress with direct Service access working usually means the ingress controller cannot successfully proxy to the backend under its own routing, timeout, TLS, or health assumptions. I would compare the path through Ingress versus direct cluster access.

I would inspect ingress controller logs, generated upstream/backend configuration, Service name and port mapping, EndpointSlices, ingress class, annotations, backend protocol, and timeout settings. If the backend deployment changed ports or protocol, direct testing may work on one path while the ingress still points to an old named port or expects HTTP instead of HTTPS. I would also check TLS passthrough/termination settings and whether the application now needs longer startup or response timeout.

Mitigation would be to rollback the ingress annotation or backend change, point the ingress to the correct service port, or increase timeout only if the service is healthy but slow. I would validate with controller metrics, 5xx rate, upstream response codes, and synthetic checks through the public route. Prevention includes ingress conformance tests during deployment and controller config drift detection.

**Q20 \| Ingress \| P1 \| Production Troubleshooting**

**Interview Question:**

> An Ingress returns 404 for a valid route only in production; how would you investigate host rules, path matching, ingress class, rewrite behavior, and controller configuration drift?

**Candidate Answer:**

For a production-only 404, I would avoid assuming the application route is missing. I would first identify who returns the 404: ingress controller, API gateway, service mesh, or application. Response headers and ingress logs usually tell that quickly.

Then I would compare production and lower-environment Ingress objects: host rule, path type, path prefix/exact matching, rewrite annotations, ingress class, TLS host, and controller-specific configuration. Production often differs because of multiple ingress controllers, wildcard hosts, stricter path matching, or a rewrite rule applied by annotation. I would also check whether DNS points to the expected load balancer and whether another Ingress claims the same host/path.

Mitigation would be to correct host/path rules, remove conflicting routes, restore the previous ingress revision, or temporarily add a safe compatibility path. I would validate using curl with the exact Host header, ingress controller route dump, and synthetic checks. Permanent prevention would be route ownership rules, CI diff checks between environments, and automated tests for every externally exposed path.

**Q21 \| Ingress \| P2 \| Architecture**

**Interview Question:**

> A product team wants one shared Ingress controller for many business-critical services; how would you design isolation, blast-radius control, rate limiting, certificate management, and ownership boundaries?

**Candidate Answer:**

I would not reject a shared Ingress controller outright, but I would design it as a platform component with clear isolation and blast-radius controls. The main risk is that one bad annotation, noisy service, certificate issue, or controller overload can affect many business-critical services.

I would separate workloads by risk: public versus internal, critical versus non-critical, and regulated versus standard. For high-criticality services, I may use dedicated ingress classes or controller pools. I would enforce ownership boundaries through namespaces, allowed annotations, rate-limit policies, admission controls, and route review. Certificates should be managed centrally with cert-manager or cloud certificate integration, with renewal alerts and secret ownership clearly defined.

Operationally, I would require controller HPA, PDB, anti-affinity, resource limits, WAF/rate limiting, access logs, request/latency/error dashboards per route, and rollback procedures for controller upgrades. I would also restrict dangerous annotations that alter global behavior. The goal is shared efficiency without shared failure. If the blast radius is unacceptable, separate ingress controllers per business tier are justified despite extra cost.

**Q22 \| CoreDNS \| P1 \| Production Troubleshooting**

**Interview Question:**

> Applications across multiple namespaces suddenly show DNS lookup failures inside the cluster; how would you determine whether CoreDNS, node DNS configuration, upstream resolver, network policy, or cluster overload is responsible?

**Candidate Answer:**

I would determine whether DNS failures are cluster-wide, namespace-specific, node-specific, or workload-specific. I would run DNS tests from debug pods across namespaces and nodes, checking service FQDN, external DNS, and upstream resolution.

Then I would inspect CoreDNS pod health, restarts, CPU/memory throttling, logs, error rate, cache metrics, and ConfigMap changes. If CoreDNS is healthy but failures occur only on certain nodes, I would check node \`/etc/resolv.conf\`, kubelet DNS configuration, CNI/network policy path to CoreDNS, and node-local DNS cache if used. If internal names work but external names fail, I would investigate upstream resolver health, firewall/NAT, or cloud DNS/private zone configuration. If only some namespaces fail, NetworkPolicy or namespace DNS behavior becomes likely.

Mitigation could be scaling CoreDNS, rolling back CoreDNS config, removing a bad NetworkPolicy, or switching to a healthy upstream resolver. I would validate DNS success rate and service error recovery. Long term, I would add CoreDNS latency/error alerts, config review, and synthetic DNS probes from multiple namespaces.

**Q23 \| CoreDNS \| P1 \| SRE**

**Interview Question:**

> CoreDNS latency increases during peak traffic and causes service-to-service timeout spikes; how would you investigate DNS query volume, cache behavior, autoscaling, noisy workloads, and upstream dependency health?

**Candidate Answer:**

I would treat CoreDNS latency as a production dependency incident because slow DNS becomes service-to-service latency. First I would quantify CoreDNS p95/p99 latency, query volume, error rate, CPU throttling, memory usage, cache hit ratio, and whether failures align with peak traffic.

I would identify noisy workloads by query source if metrics/logging allow it, or sample traffic with CoreDNS logs temporarily. Common causes are applications doing DNS lookup per request, low TTL behavior, missing client-side caching, CoreDNS under-replicated, CPU limits too low, or slow upstream resolvers. I would also check autoscaling rules, node-local DNS cache, NetworkPolicy to upstream DNS, and recent CoreDNS ConfigMap changes.

Mitigation might be scaling CoreDNS, increasing CPU requests, enabling/adjusting cache, fixing noisy clients, or using node-local DNS cache for large clusters. I would avoid enabling verbose logging permanently because it can worsen load. Prevention includes CoreDNS HPA based on cluster size/query rate, alerts on latency and SERVFAIL/NXDOMAIN spikes, and application standards for DNS caching and connection reuse.

**Q24 \| Service Discovery \| P2 \| Production Troubleshooting**

**Interview Question:**

> A microservice resolves an old service endpoint after a migration and sends traffic to the wrong backend; how would you debug service discovery caching, DNS TTL, client behavior, and rollout ordering?

**Candidate Answer:**

I would investigate both Kubernetes service discovery and client-side caching. Kubernetes DNS changes are not always consumed immediately by applications, especially if the application, JVM, sidecar, or HTTP client caches resolved IPs longer than DNS TTL.

First I would verify the current Service, EndpointSlice, and DNS answer from a fresh debug pod. Then I would compare that with what the affected application is actually using through logs, traces, connection targets, or packet captures. I would check whether the migration reused service names, changed headless versus ClusterIP behavior, or relied on clients reconnecting automatically. I would also inspect CoreDNS cache settings and any service mesh discovery cache.

Mitigation would be to restart or drain affected pods if the client cannot refresh endpoints, temporarily keep the old backend compatible, or restore a compatibility service name. For future migrations, I would lower TTL where applicable, deploy clients with connection refresh behavior, use staged dual-write/dual-route periods, and validate traffic movement through telemetry before deleting old endpoints.

**Q25 \| ConfigMap \| P1 \| Production Troubleshooting**

**Interview Question:**

> A ConfigMap was updated during an incident, but pods continue using old values; how would you explain and troubleshoot Kubernetes config propagation, mounted files, environment variables, and rollout triggers?

**Candidate Answer:**

I would explain that ConfigMap propagation depends on how the data is consumed. Environment variables are fixed at container start and will not update until the pod restarts. Mounted ConfigMap volumes can update eventually, but applications must read the file again or support reload.

First I would identify whether the pod consumes the ConfigMap as env vars, volume mounts, command arguments, or through a sidecar. Then I would check the pod template hash, mounted file contents inside the container, kubelet update delay, and whether the application cached the value at startup. If the change was urgent and pods use env vars, I would trigger a controlled rollout rather than expecting live refresh.

Mitigation would be to rollback the ConfigMap, restart only affected workloads, or patch the deployment annotation to force a new ReplicaSet. I would validate with application behavior, config version logs, and endpoint health. Long term, I would use versioned ConfigMaps, checksum annotations in pod templates, config validation, and progressive rollout for high-risk config changes.

**Q26 \| ConfigMap \| P1 \| Implementation**

**Interview Question:**

> A bad configuration change caused a production outage across many pods; how would you design validation, rollout safety, versioning, rollback, and ownership for ConfigMap changes?

**Candidate Answer:**

I would treat ConfigMaps as release artifacts with ownership, review, and rollback, not as informal runtime edits. The design should include schema validation, versioning, progressive rollout, and automatic restart when config changes.

A practical pattern is checksum-based rollout with immutable/versioned config:

metadata:

annotations:

config/checksum: "{{ sha256sum .Values.config }}"

spec:

strategy:

rollingUpdate: { maxUnavailable: 0, maxSurge: 1 }

template:

metadata:

annotations:

config-version: "payments-v42"

spec:

containers:

\- name: api

envFrom:

\- configMapRef: { name: payments-config-v42 }

Before rollout, I would validate config schema in CI, run dry-run apply, and test critical paths in canary. During rollout, I would watch SLO burn rate, errors, and config-version adoption. If failure occurs, rollback means returning to the previous config version and ReplicaSet, not manually editing live data. Permanent controls include CODEOWNERS, GitOps review, audit trail, config diff visibility, and runbooks for emergency config rollback.

**Q27 \| Secrets \| P1 \| Security**

**Interview Question:**

> A secret rotation is completed successfully in the secret store, but applications start failing authentication; how would you debug Kubernetes secret sync, pod restart behavior, mounted secret refresh, and application reload logic?

**Candidate Answer:**

I would split this into secret-store rotation, Kubernetes sync, pod consumption, and application reload. First I would confirm which secret version the application is using and whether failures started exactly after rotation.

I would inspect the external secret operator or CSI driver sync status, Kubernetes Secret resource version, mounted secret file timestamp, pod environment variables, and application auth errors. If the app consumes secrets as env vars, pods need restart. If mounted files refresh but the app does not reload credentials or connection pools, authentication can still fail. I would also check whether both old and new credentials were valid during a grace period; rotating without overlap often causes production outages.

Mitigation would be to restore the previous secret version, enable dual-valid credentials temporarily, restart pods progressively, or rollback the application config. I would validate by checking auth success rate, dependency health, and secret version in logs without exposing values. Long term, I would implement secret rotation runbooks with overlap windows, canary restart, audit evidence, and alerts on secret sync failures.

**Q28 \| Secrets \| P1 \| Security**

**Interview Question:**

> A production incident reveals that secrets are exposed through pod environment dumps and logs; how would you redesign secret consumption and operational debugging to reduce leakage risk?

**Candidate Answer:**

I would treat this as both an incident and a design flaw. First I would contain exposure: restrict access to logs/dumps, rotate affected secrets, identify which systems received the leaked values, and preserve audit evidence.

Then I would redesign secret consumption. Environment variables are convenient but often appear in process dumps, debug endpoints, crash reports, and accidental logs. For sensitive secrets, I would prefer mounted files with tight permissions, CSI/External Secrets integration, short-lived credentials, workload identity, or dynamic secrets where possible. I would also ensure applications redact known secret patterns and never log full environment dumps in production.

Operational debugging should use metadata such as secret name, resource version, checksum, or last rotation time, never raw values. Access to \`kubectl exec\`, pod logs, and crash dumps should be RBAC-controlled and audited. Permanent prevention includes secret scanning in logs, admission policies blocking unsafe env exposure for high-risk secrets, mandatory redaction libraries, and rotation drills. The goal is to keep debugging possible without making secrets observable by default.

**Q29 \| Rollout behavior after config changes \| P1 \| SRE**

**Interview Question:**

> A platform team wants every config or secret change to roll out safely without manual restarts; how would you design automated restart triggers, canary validation, and rollback for config-driven deployments?

**Candidate Answer:**

I would design config and secret rollout as a controlled deployment workflow. Every config or secret change should produce a visible pod template change, canary first, validation gates, and a clean rollback path.

For Kubernetes Deployments, I would use checksum annotations on ConfigMaps/Secrets or a controller such as Reloader to trigger restarts when referenced resources change. For high-risk services, I would avoid restarting all replicas at once; I would canary one replica or one traffic slice, verify SLOs, authentication success, dependency health, and business metrics, then continue. Secrets should have overlap windows so old and new credentials work during rollout.

Rollback must restore the previous config/secret version and restart affected pods safely. I would store versioned configuration in Git, require ownership approval, and expose config version in metrics/logs for debugging. Permanent controls include GitOps audit trail, admission validation, schema checks, progressive delivery, and alerts when running pods reference stale config versions. Manual restarts should become the exception, not the operating model.

**Q30 \| PV and PVC \| P1 \| Production Troubleshooting**

**Interview Question:**

> A pod is stuck because its PVC remains Pending after a storage class change; how would you investigate binding mode, provisioner health, quota, topology constraints, and access mode compatibility?

**Candidate Answer:**

I would start with the PVC status and events because they usually state why binding failed. I would check the PVC, StorageClass, provisioner pods, CSI controller logs, namespace storage quota, requested access mode, and volume binding mode.

After a storage class change, common issues are wrong provisioner name, disabled dynamic provisioning, \`WaitForFirstConsumer\` requiring scheduling before binding, unsupported access mode, zonal topology constraints, or cloud quota exhaustion. If the pod is also Pending, I would correlate scheduler events with PVC events because storage topology can block scheduling. I would also verify whether the default StorageClass changed unintentionally and whether existing PVCs were expected to migrate; PVCs do not automatically move to a new class.

Mitigation could be restoring the previous StorageClass, creating a compatible PVC, fixing provisioner health, or scheduling into the correct zone. I would avoid deleting PVCs without data impact review. Prevention includes storage class change management, quota dashboards, CSI health alerts, and preflight checks for access mode and topology compatibility.

**Q31 \| PV and PVC \| P1 \| Production Troubleshooting**

**Interview Question:**

> A production pod fails to mount a volume after node drain; how would you isolate whether the problem is cloud volume attachment, stale mount, filesystem corruption, access mode, or CSI driver failure?

**Candidate Answer:**

I would first determine whether this is an attach problem, mount problem, or filesystem problem. Pod events, \`VolumeAttachment\` objects, kubelet logs, and CSI node plugin logs would guide that distinction.

After a node drain, a common issue is the volume still attached to the drained or unhealthy node, especially for single-writer cloud disks. I would check cloud provider attachment status, stale mounts, node readiness, CSI attach/detach controller events, and whether the volume access mode supports the new placement. If the volume attaches but mount fails, I would inspect filesystem errors, mount options, permissions, and node plugin health. If only one workload is affected, I would check PVC/PV topology and StorageClass.

Mitigation would be safe detach, reschedule to the correct zone, restart CSI node plugin on the affected node, or restore from backup if filesystem corruption is confirmed. I would never force detach while the old node could still write. Prevention includes drain runbooks, PDBs, storage topology rules, CSI monitoring, and periodic restore testing.

**Q32 \| StorageClass \| P2 \| Architecture**

**Interview Question:**

> A latency-sensitive workload is using the default StorageClass and shows high disk latency; how would you evaluate and redesign storage class choice, topology, IOPS, backup, and cost trade-offs?

**Candidate Answer:**

I would evaluate storage as part of the application SLO, not as a default cluster setting. For a latency-sensitive workload, the default StorageClass may optimize cost or general availability, not IOPS, throughput, or topology.

I would first measure disk latency from application metrics, node volume metrics, cloud disk metrics, queue depth, fsync behavior, and database/storage logs. Then I would map requirements: read/write pattern, IOPS, throughput, latency target, volume expansion, snapshot/backup needs, zone placement, and failover expectations. Options may include premium SSD, provisioned IOPS, regional persistent disks, local SSD for ephemeral workloads, or managed database storage depending on durability needs.

The redesign must consider topology and cost. A high-performance zonal disk may reduce latency but reduce failover flexibility; regional disks improve resilience but cost more and may add latency. I would create a dedicated StorageClass with explicit parameters, node affinity/topology, backup policy, and monitoring. Migration would be staged with backup, restore validation, and rollback plan. I would not switch storage blindly during an incident unless data risk is controlled.

**Q33 \| Volume mount failures \| P1 \| Production Troubleshooting**

**Interview Question:**

> A pod fails only on one node with volume mount timeout while the same workload runs elsewhere; how would you debug node-level storage plugin health, kubelet state, cloud provider limits, and safe remediation?

**Candidate Answer:**

Because the failure happens only on one node, I would focus on node-level storage path first. I would compare pod events on the bad node with successful nodes, then inspect kubelet logs, CSI node plugin pod health, volume mount directories, node disk pressure, and cloud attach limits.

A mount timeout can come from a stuck CSI plugin, stale mount, network path to storage, cloud API throttling, or node kernel/filesystem issue. I would check whether multiple volumes fail on the same node, whether only one volume is affected, and whether the node recently upgraded or drained. If the workload is critical, I would cordon the node and reschedule the pod elsewhere if the volume topology allows it.

Safe remediation could be restarting the CSI node plugin, draining the node, clearing stale mounts only with storage-owner approval, or replacing the node. I would avoid manual unmounts on active volumes without confirming no writer exists. Prevention includes CSI plugin SLOs, node health checks, alerts on mount latency, and automated quarantine of nodes with repeated storage failures.

**Q34 \| Readiness Probes \| P1 \| SRE**

**Interview Question:**

> A service sends traffic to pods before the application has warmed cache and loaded dependencies; how would you redesign readiness probes to prevent cold-start errors without delaying rollout unnecessarily?

**Candidate Answer:**

I would design readiness around the point at which the pod can safely receive real user traffic. For a service with cache warm-up and dependency loading, a simple \`/health\` endpoint is too weak.

The readiness probe should fail until the application has loaded mandatory config, established critical dependency connectivity, completed cache warm-up or reached a minimum warm state, and is able to serve representative requests. At the same time, I would avoid making readiness depend on every optional downstream service, because that can remove all pods during a dependency outage. The endpoint should distinguish critical versus degraded dependencies.

I would tune \`initialDelaySeconds\`, \`periodSeconds\`, \`timeoutSeconds\`, \`failureThreshold\`, and \`successThreshold\` based on startup and recovery behavior, then validate during rollout under load. To avoid delaying rollout unnecessarily, I would combine readiness with startupProbe and application-level warm-up metrics. Prevention includes synthetic checks, cache warm-up observability, and release gates that ensure new pods become endpoints only after they can meet latency and error SLOs.

**Q35 \| Liveness Probes \| P1 \| Production Troubleshooting**

**Interview Question:**

> A misconfigured liveness probe restarts healthy pods during a downstream dependency outage; how would you detect this pattern and redesign health checks to avoid cascading failure?

**Candidate Answer:**

I would detect this by correlating restarts with downstream outage timing and liveness probe failures. If application logs show dependency timeouts while pod events show liveness failures, the probe is likely killing pods that are still recoverable.

Liveness should answer: “Is this process permanently stuck and needs restart?” It should not fail simply because a database, cache, or external API is temporarily unavailable. Otherwise, a downstream incident becomes a restart storm, cold caches increase latency, and the system cascades. I would inspect probe path, timeout, failure threshold, thread pool behavior, and whether the probe shares resources with user traffic.

Mitigation would be to patch the liveness probe to a local process check, increase thresholds temporarily, or rollback the probe change. Readiness can reflect inability to serve traffic, but liveness should be conservative. Long term, I would separate \`/live\`, \`/ready\`, and \`/startup\`, add probe design standards, and include dependency outage tests. I would also alert on restart rate caused by probe failures.

**Q36 \| Startup Probes \| P2 \| Implementation**

**Interview Question:**

> A slow-starting JVM or ML inference service fails repeatedly during rollout because liveness checks start too early; how would you use startup behavior to protect the rollout?

**Candidate Answer:**

I would use startupProbe to give slow-starting services a protected initialization window before liveness begins. This is especially important for JVM services, model servers, or applications that load large caches/models.

A concise configuration would be:

startupProbe:

httpGet:

path: /startup

port: 8080

periodSeconds: 10

failureThreshold: 30

timeoutSeconds: 2

livenessProbe:

httpGet:

path: /live

port: 8080

periodSeconds: 10

failureThreshold: 3

readinessProbe:

httpGet:

path: /ready

port: 8080

periodSeconds: 5

failureThreshold: 2

This gives up to five minutes for startup before liveness can restart the container. Readiness remains separate and should become true only when the service can receive traffic. I would validate using rollout events, startup duration metrics, no premature restarts, and endpoint readiness timing. Permanent controls include startup-time SLOs, model/cache load metrics, and load testing during rollout.

**Q37 \| Probe misconfiguration \| P1 \| Production Troubleshooting**

**Interview Question:**

> A deployment passes readiness but customers still see errors because the probe checks only process health; what senior-level health check design would you expect for a critical API?

**Candidate Answer:**

For a critical API, I would not accept a readiness probe that only checks whether the process responds. Readiness should reflect the minimum conditions needed to serve customer requests safely.

I would expect the readiness endpoint to validate configuration loaded, required secrets available, database connection pool usable, critical downstream dependencies reachable or in approved degraded mode, migrations compatible, cache/model warm-up complete where needed, and worker queues not saturated. It should also fail if the application is intentionally draining during shutdown. However, it should not fail because of optional dependencies that can be degraded without customer impact.

I would also check whether readiness runs through the same lightweight path as production traffic without causing load. The probe should be fast, deterministic, and observable. During rollout, I would compare readiness with real metrics: endpoint inclusion, 5xx rate, p95 latency, and synthetic transaction success. Long term, I would require health-check contracts for tier-1 services, release validation against those contracts, and dashboards showing readiness state versus business errors.

**Q38 \| Scheduling \| P1 \| Production Troubleshooting**

**Interview Question:**

> A pod remains Pending with enough total cluster capacity available; how would you identify whether fragmented resources, node affinity, topology spread, taints, or PVC topology is blocking scheduling?

**Candidate Answer:**

I would start from scheduler events instead of aggregate cluster capacity. A pod can be Pending even when total CPU and memory look sufficient because no single eligible node satisfies all constraints.

I would inspect \`kubectl describe pod\`, scheduler events, requested resources, node allocatable, taints, tolerations, node affinity, topology spread constraints, and PVC topology. Fragmented resources are common when many nodes have small free slices but no node has enough contiguous CPU/memory for the pod. If affinity or topology rules exist, I would calculate eligible nodes after those filters. If PVC uses \`WaitForFirstConsumer\` or zonal PV binding, storage topology may further reduce eligible nodes.

Mitigation could be reducing oversized requests only if safe, scaling the correct node group, relaxing non-critical placement rules, or fixing taints/tolerations. I would validate by watching scheduling events and pod readiness. Permanent prevention includes capacity dashboards by node pool and topology, admission checks for impossible constraints, and right-sized requests based on usage and SLOs.

**Q39 \| nodeSelector \| P2 \| Implementation**

**Interview Question:**

> A workload is pinned to a deprecated node pool using nodeSelector and starts failing during node pool retirement; how would you migrate it safely without downtime?

**Candidate Answer:**

I would migrate away from hard pinning in stages. First I would identify why the nodeSelector existed: hardware, compliance, dependency, or historical accident. Then I would create the replacement node pool, labels, taints, capacity, and monitoring before touching production pods.

A safe migration can use preferred affinity and dual compatibility:

affinity:

nodeAffinity:

preferredDuringSchedulingIgnoredDuringExecution:

\- weight: 80

preference:

matchExpressions:

\- key: nodepool

operator: In

values: \["new-general"\]

requiredDuringSchedulingIgnoredDuringExecution:

nodeSelectorTerms:

\- matchExpressions:

\- key: workload-tier

operator: In

values: \["api"\]

topologySpreadConstraints:

\- maxSkew: 1

topologyKey: topology.kubernetes.io/zone

whenUnsatisfiable: ScheduleAnyway

I would canary a few replicas, verify latency, logs, node metrics, and dependency access, then gradually remove the deprecated selector. PDBs and maxUnavailable should protect availability. After migration, I would block new workloads from using deprecated labels through policy and clean up old node pool only after zero scheduled pods remain.

**Q40 \| Taints and Tolerations \| P1 \| Production Troubleshooting**

**Interview Question:**

> Critical workloads are not scheduled on dedicated nodes after a cluster upgrade; how would you debug taints, tolerations, node labels, admission changes, and autoscaler behavior?

**Candidate Answer:**

I would compare the desired dedicated-node contract with the actual cluster state after upgrade. First I would inspect node taints, labels, workload tolerations, node affinity, pod events, and autoscaler logs.

Cluster upgrades often introduce changed taints, missing labels on new nodes, or admission mutations that alter pod specs. If critical workloads tolerate the old taint key but the new node pool uses a different taint, the scheduler will reject them. If labels changed, node affinity or nodeSelector may exclude the new nodes. I would also check whether Cluster Autoscaler considers the dedicated node group expandable and whether max size or cloud quota blocks new nodes.

Mitigation would be to restore expected labels/taints, patch tolerations through a controlled rollout, or temporarily allow scheduling on a safe fallback node pool. I would validate pod placement, topology spread, and application SLOs. Prevention includes node pool conformance tests, upgrade prechecks for labels/taints, policy-managed node metadata, and runbooks that verify critical workload scheduling before completing upgrades.

**Q41 \| Affinity and Anti-Affinity \| P1 \| Architecture**

**Interview Question:**

> A high-availability service has all replicas placed on the same node or zone during a traffic spike; how would you redesign affinity, anti-affinity, and topology spread for better resilience?

**Candidate Answer:**

I would redesign placement using both anti-affinity and topology spread, because high availability requires protecting against node and zone failure, not just running multiple replicas.

First I would check current labels, zones, node pools, replica count, and whether topology labels are consistent. For hard requirements, I would use required pod anti-affinity only when enough capacity exists; otherwise it can block scheduling during incidents. For most services, I prefer topology spread constraints across zones and hostname with \`DoNotSchedule\` or \`ScheduleAnyway\` depending on criticality, plus preferred anti-affinity to avoid same-node placement.

I would also ensure PDBs, HPA, and Cluster Autoscaler understand the placement model. If all replicas landed in one zone during a spike, autoscaler may have scaled only one node group or constraints may have been too weak. I would validate by simulating node/zone loss and checking remaining capacity. Permanent prevention includes placement dashboards, admission policies for tier-1 services requiring topology spread, and capacity planning per zone. The trade-off is that stricter spreading improves resilience but needs more headroom and can increase cost.

**Q42 \| Topology Spread Constraints \| P2 \| SRE**

**Interview Question:**

> A zonal outage affects more replicas than expected because workloads were not evenly spread; how would you detect and prevent this placement risk before production failure?

**Candidate Answer:**

I would detect this risk before failure by continuously measuring replica distribution across zones and nodes for tier-1 workloads. It should not require a zonal outage to discover skew.

I would review topology labels, Deployment replicas, topologySpreadConstraints, pod anti-affinity, HPA behavior, and Cluster Autoscaler node group balance. If constraints are absent or set to \`ScheduleAnyway\`, Kubernetes may allow skew during pressure. If one zone has more capacity, autoscaler may keep placing pods there unless balancing is configured. I would build alerts for “too many replicas in one zone” and run periodic failure simulations.

To prevent recurrence, I would define platform standards: critical services need minimum replicas across at least two or three zones, PDBs, zone-aware node groups, and topology spread on \`topology.kubernetes.io/zone\` and optionally hostname. I would tune \`maxSkew\` based on replica count and availability goals. I would validate with scheduler events, workload placement reports, and chaos testing. The trade-off is stricter spread may leave pods Pending if zone capacity is insufficient, so capacity planning must match the policy.

**Q43 \| Resource Requests and Limits \| P1 \| SRE**

**Interview Question:**

> A team sets very low CPU requests to save cost, but latency becomes unstable under load; how would you explain and correct the relationship between requests, scheduling, throttling, and SLOs?

**Candidate Answer:**

I would explain that low CPU requests may look cost-efficient but they distort scheduling and autoscaling. Requests tell Kubernetes how much capacity to reserve and are used by HPA utilization calculations. If requests are unrealistically low, pods can be packed too densely and HPA percentages become misleading.

Latency instability can come from CPU contention on nodes, throttling if limits exist, or delayed scale-out because the wrong metric is being used. I would compare CPU request, limit, actual usage, throttling, run queue, p95/p99 latency, and HPA behavior during load. If the service has SLO-sensitive traffic, I would right-size requests based on peak or percentile usage, not only average. Limits may be removed or raised for latency-sensitive services if throttling hurts SLOs.

Mitigation could be increasing requests, tuning HPA targets, adding custom metrics like RPS or queue depth, and creating capacity headroom. Long term, I would use VPA recommendations in advisory mode, enforce request quality guardrails, and review cost using bin-packing plus SLO impact, not raw low requests.

**Q44 \| QoS Classes \| P2 \| SRE**

**Interview Question:**

> During node memory pressure, some pods are evicted before others and the application team claims Kubernetes behaved randomly; how would you explain QoS impact and redesign workload resource profiles?

**Candidate Answer:**

I would explain that eviction is not random; Kubernetes uses QoS and resource pressure signals. Pods with BestEffort QoS are the first candidates under memory pressure, then Burstable pods exceeding requests, while Guaranteed pods have the strongest protection.

I would inspect each affected pod’s requests and limits to classify QoS. If pods have no requests/limits, they are BestEffort and easy to evict. If memory requests are too low compared with actual usage, Burstable pods can be evicted even if the application team thinks they are important. I would correlate eviction events with node memory pressure, kubelet logs, and workload usage.

Mitigation during the incident may involve rescheduling critical pods, increasing node capacity, or reducing noisy neighbors. The permanent fix is to define resource profiles by criticality: tier-1 services should have realistic memory requests, limits where appropriate, PDBs, priority classes, and potentially Guaranteed QoS for critical components. I would also add admission policies that reject production workloads without requests and dashboards showing eviction risk by QoS class.

**Q45 \| OOMKilled \| P1 \| Production Troubleshooting**

**Interview Question:**

> A pod is repeatedly OOMKilled after a release, but average memory dashboards look normal; how would you investigate peak memory, limits, leaks, traffic patterns, and rollout correlation?

**Candidate Answer:**

I would not trust average memory dashboards for OOMKilled. OOM is usually caused by short peaks, leaks, or per-request spikes that averages hide.

First I would check pod \`lastState\`, exit code 137, container limit, restart timestamps, and memory usage at high resolution. I would compare release timing, traffic shape, endpoint mix, payload size, cache behavior, and GC or heap metrics. If the application uses JVM/Go/Python, I would inspect heap, native memory, connection pools, and whether the new release changed buffering or concurrency. I would also check whether the limit was recently reduced or whether HPA increased traffic per pod due to fewer replicas.

Mitigation depends on risk: rollback the release, increase memory limit temporarily, reduce concurrency, disable the feature causing spikes, or scale out to lower per-pod pressure. I would validate by watching restart count, peak memory, latency, and error rate. Permanent prevention includes high-resolution memory alerts, load tests with realistic payloads, memory profiling, and release gates for memory regression.

**Q46 \| CPU Throttling \| P1 \| Production Troubleshooting**

**Interview Question:**

> A service has enough CPU request but shows high latency and throttling after limits were introduced; how would you decide whether to tune limits, requests, HPA, or application concurrency?

**Candidate Answer:**

I would separate CPU request, CPU limit, and actual throttling. Requests influence scheduling and HPA utilization; limits can cap CPU and cause throttling even when the node has spare CPU. For latency-sensitive services, unnecessary CPU limits can create p99 latency spikes.

I would check container CPU throttled seconds, CFS throttling ratio, p95/p99 latency, request concurrency, CPU usage, HPA metrics, and node saturation. If throttling begins after limits were introduced and nodes are not saturated, I would consider removing limits or setting them higher while keeping realistic requests. If nodes are saturated, increasing requests and scaling capacity may be required. If HPA is slow, I would tune target utilization, stabilization windows, and possibly use RPS/queue depth custom metrics.

Mitigation could be raising/removing limits and scaling replicas. I would validate by observing throttling drop and latency recover without overloading dependencies. Permanent controls include CPU policy standards by workload type, load tests before introducing limits, and alerts on throttling ratio rather than only CPU utilization.

**Q47 \| Node Pressure \| P1 \| Production Troubleshooting**

**Interview Question:**

> Multiple unrelated pods are evicted from the same node during peak load; how would you investigate memory pressure, disk pressure, PID pressure, image garbage collection, and node-level noisy neighbors?

**Candidate Answer:**

I would treat multiple unrelated evictions from one node as a node health or noisy-neighbor incident. First I would inspect node conditions, kubelet events, eviction messages, memory/disk/PID pressure, image filesystem usage, and top pods by resource usage.

Memory pressure points to workload spikes or insufficient reservations. Disk pressure may come from container logs, image layers, emptyDir usage, or failed image garbage collection. PID pressure can be caused by fork storms or process leaks. I would also check system daemons, DaemonSets, and whether kube-reserved/system-reserved are configured. If one pod is consuming abnormal resources, I would isolate it; if the node itself is unhealthy, I would cordon and drain safely.

Mitigation may include deleting log-heavy pods, expanding disk, restarting kubelet only if safe, moving workloads, or replacing the node. I would validate that node pressure clears and evictions stop. Prevention includes node pressure alerts, log rotation policies, requests/limits, emptyDir size limits, image GC tuning, and noisy-neighbor detection dashboards.

**Q48 \| HPA \| P1 \| Production Troubleshooting**

**Interview Question:**

> HPA is configured but does not scale pods during a traffic surge; how would you isolate metrics availability, target utilization, missing requests, custom metrics, stabilization windows, and scaling limits?

**Candidate Answer:**

I would begin by checking whether HPA can read the metric. \`kubectl describe hpa\` usually shows missing metrics, invalid targets, or scale recommendations. Then I would verify metrics-server/custom metrics adapter health and whether the pods have CPU requests, because CPU utilization-based HPA needs requests.

If metrics are available, I would inspect target utilization, current utilization, min/max replicas, scale-up policies, stabilization windows, and whether the Deployment is blocked by PDBs or readiness delays. For custom metrics, I would verify metric names, labels, aggregation, and freshness. I would also check whether HPA scaled but new pods remained Pending due to capacity or failed readiness, which is a different bottleneck.

Mitigation could be fixing metrics-server, adding missing requests, raising maxReplicas, temporarily manual scaling, or using a better metric such as RPS, queue depth, or latency. I would validate by watching HPA events, replica count, ready pods, and SLO recovery. Permanent prevention includes HPA conformance checks, metric freshness alerts, and load tests proving scale behavior before production.

**Q49 \| HPA \| P1 \| SRE**

**Interview Question:**

> HPA scales aggressively and causes database saturation instead of improving latency; how would you redesign autoscaling around dependency capacity and end-to-end SLOs?

**Candidate Answer:**

I would treat this as a system capacity problem, not a Kubernetes success. HPA increasing replicas can increase database connections, queries, and lock contention, making latency worse.

First I would compare traffic, replica count, DB connection usage, query latency, saturation, error rate, and application connection pool settings. I would check whether each pod opens a fixed-size pool, so scaling from 10 to 50 pods multiplies DB connections. I would also inspect whether HPA uses CPU while the true bottleneck is DB latency, which can create runaway scaling.

Mitigation may be to cap HPA maxReplicas, reduce per-pod connection pool size, enable backpressure, use queueing/rate limiting, or rollback the traffic-causing release. Long term, autoscaling should include dependency capacity: DB connection budgets, circuit breakers, load shedding, custom metrics, and SLO-based scale policies. I would also define safe scaling envelopes with database owners. A senior answer is not “scale more”; it is “scale only within end-to-end capacity limits.”

**Q50 \| VPA \| P2 \| Architecture**

**Interview Question:**

> A team wants to enable VPA on a latency-sensitive production Deployment already using HPA; what risks, modes, and rollout controls would you evaluate before enabling it?

**Candidate Answer:**

I would evaluate VPA carefully because it can evict pods to apply recommendations, and that can hurt a latency-sensitive service. With HPA already active, I would also avoid both controllers fighting over CPU behavior without clear ownership.

First I would decide VPA mode. I would start in \`Off\` or \`Initial\`/recommendation mode to collect request recommendations without automatic eviction. I would compare recommendations with p95 usage, SLOs, HPA targets, node capacity, and cost. For CPU, HPA and VPA can conflict if both act on CPU; a safer pattern is HPA on external/custom metrics and VPA for memory recommendations, or VPA only for initial sizing.

Before enabling updates, I would require PDBs, canary namespace/workload, maintenance windows if needed, and rollout monitoring. I would validate that VPA recommendations do not trigger capacity fragmentation or excessive node scale-up. Permanent adoption should include policy limits, review workflow, and dashboards showing VPA recommendation versus actual usage and SLO impact. I would not enable auto mode blindly on critical services.

**Q51 \| Cluster Autoscaler \| P1 \| Production Troubleshooting**

**Interview Question:**

> Pods are Pending due to insufficient resources, but Cluster Autoscaler does not add nodes; how would you debug node group limits, cloud quota, taints, affinity, storage topology, and autoscaler events?

**Candidate Answer:**

I would start with Cluster Autoscaler logs and pod scheduling events together. Autoscaler only adds nodes if it believes a pending pod would schedule on a node group after scale-up.

I would check pending pod reasons, node group max size, cloud quota, instance type availability, taints, node labels, affinity, topology spread, PVC zone constraints, and whether the autoscaler discovers the node group. If pods require a label or taint that no node group template provides, autoscaler will not help. If PVC topology pins the pod to a zone where the node group cannot scale, it will also remain pending. Cloud API errors, subnet IP exhaustion, or quota limits can block scaling despite correct Kubernetes config.

Mitigation could be raising node group max, fixing labels/taints, scaling the right node group manually, increasing cloud quota, or relaxing constraints. I would validate by watching autoscaler events, new node registration, and pod scheduling. Permanent prevention includes autoscaler alerting, node group template audits, capacity headroom by zone, and preflight checks for scheduling constraints.

**Q52 \| Cluster Autoscaler \| P1 \| Cost Optimization**

**Interview Question:**

> Cluster Autoscaler keeps adding nodes for poorly requested workloads and cloud cost spikes; how would you identify the workload pattern and implement platform guardrails?

**Candidate Answer:**

I would first identify whether autoscaler is reacting to real demand or bad resource specifications. Poorly requested workloads can force new nodes even when actual usage is low, especially with oversized CPU/memory requests, strict affinity, or single-pod-per-node patterns.

I would analyze pending pods, requested versus actual usage, node bin-packing, HPA/VPA recommendations, namespace quotas, and topology constraints. Then I would categorize waste: oversized requests, too many replicas, fragmented node pools, over-strict spread rules, or missing scale-down due to PDBs/local storage. I would avoid simply reducing node count because that can break SLOs.

Guardrails would include request/limit policies, VPA recommendation review, namespace resource quotas, limit ranges, cost dashboards by team/workload, and admission checks for extreme request-to-usage ratios. For platform-level control, I would use right-sized node pools, Karpenter/autoscaler policies where applicable, and scheduled scale-down for non-prod. I would validate cost reduction against latency and error SLOs. The target is efficient capacity, not under-provisioned capacity.

**Q53 \| RBAC \| P1 \| Security**

**Interview Question:**

> A deployment pipeline suddenly fails with forbidden errors while applying Kubernetes resources; how would you debug service account permissions, role bindings, namespace scope, and least-privilege changes?

**Candidate Answer:**

I would debug RBAC by identifying the exact identity, verb, resource, API group, and namespace from the forbidden error. Then I would verify what service account the pipeline is using and whether it changed recently.

I would run access checks such as \`kubectl auth can-i apply deployments --as system:serviceaccount:ns:sa -n target-ns\` and inspect Role, ClusterRole, RoleBinding, and ClusterRoleBinding. Common causes are namespace-scoped RoleBinding missing in a new namespace, a ClusterRole changed for least privilege, service account token/identity changed, or the pipeline trying to apply cluster-scoped resources with only namespace permissions. I would also check GitOps or admission controllers if the error is policy-related but presented during apply.

Mitigation should follow least privilege. I would grant only required verbs/resources and separate deployment permissions from cluster-admin operations. If production is blocked, I may temporarily restore the previous binding with expiry and audit. Permanent prevention includes RBAC tests in pipeline, permission change review, service account ownership, and audit logs showing who changed access.

**Q54 \| RBAC \| P2 \| Platform Engineering**

**Interview Question:**

> Developers need enough access to debug production safely, but security rejects broad cluster-admin access; how would you design a senior-level RBAC model for operational support?

**Candidate Answer:**

I would design operational RBAC around job-to-be-done, not around cluster-admin convenience. Developers usually need read and diagnostic access, plus controlled break-glass, not unrestricted mutation across the cluster.

My model would include namespace-scoped read access to pods, logs, events, deployments, services, config metadata, and non-secret descriptions. For debugging, I would allow \`exec\` only where approved, with audit logging and possibly restricted to non-sensitive namespaces. Mutating actions like rollout restart, scale, or rollback should be granted through controlled roles, approval workflow, or ChatOps commands with audit trail. Secrets access should be tightly restricted or replaced with metadata-only visibility.

For production, I would create roles by support tier: viewer, incident responder, release operator, and platform admin. Break-glass access should be time-bound, ticket-linked, MFA-protected, and reviewed. I would validate least privilege using \`kubectl auth can-i\` tests and audit logs. This balances fast incident response with security. Permanent platform support includes documentation, self-service access requests, and policy-as-code preventing broad cluster-admin bindings.

**Q55 \| NetworkPolicy \| P1 \| Production Troubleshooting**

**Interview Question:**

> A service cannot connect to its database after a NetworkPolicy change, while other services still work; how would you isolate ingress, egress, namespace labels, pod selectors, DNS, and default-deny behavior?

**Candidate Answer:**

I would start by proving whether the failure is DNS, TCP reachability, or application authentication. From the affected pod, I would test database DNS resolution, connect to port, and compare with a service that still works.

After a NetworkPolicy change, I would inspect both sides: egress policy from the application namespace/pods and ingress policy on the database side. I would check pod selectors, namespace selectors, labels, ports, protocol, and whether DNS egress to CoreDNS is allowed. A common mistake is adding default-deny egress and allowing database traffic but forgetting DNS, or changing namespace labels that selectors depend on. I would also confirm whether the database is inside the cluster, behind a service, or external through private endpoint.

Mitigation would be to rollback the policy or add a minimal allow rule for DNS and database traffic. I would validate with connection tests and application error recovery. Long term, I would use policy simulation, staged rollout, golden connectivity tests, and policy ownership so one service’s policy change does not unexpectedly break another.

**Q56 \| NetworkPolicy \| P1 \| Security**

**Interview Question:**

> You are asked to introduce default-deny NetworkPolicy in a running production namespace; how would you roll it out safely without breaking service-to-service communication?

**Candidate Answer:**

I would roll out default-deny as a migration, not a single policy apply. First I would inventory real traffic using flow logs, service mesh telemetry, CNI policy logs, DNS logs, and application owners. Then I would build explicit ingress and egress policies for required paths.

The safe sequence is to start with non-critical namespaces or audit mode if the CNI supports it, then apply default-deny with allow policies for DNS, metrics, ingress controller, required databases, APIs, and service-to-service calls. I would canary one workload, validate synthetic tests, and monitor connection failures before expanding. Every policy should use stable labels and avoid overly broad namespace selectors unless intentional.

During rollout, I would have a fast rollback policy ready and clear ownership for each allow rule. I would validate with automated connectivity tests and application SLOs. Permanent prevention includes NetworkPolicy templates, CI validation, service dependency documentation, and alerts on denied traffic. The goal is zero-trust segmentation without surprise outages.

**Q57 \| Pod Security \| P1 \| Security**

**Interview Question:**

> A critical deployment is rejected after Pod Security enforcement is enabled; how would you identify the violating security context and fix it without granting unsafe exceptions?

**Candidate Answer:**

I would first read the admission rejection message because it usually names the violated Pod Security rule: privileged container, hostPath, hostNetwork, running as root, added capabilities, writable root filesystem, or missing seccomp profile.

Then I would inspect the deployment securityContext at pod and container level and determine whether the setting is truly required. For most application workloads, the fix is to run as non-root, drop capabilities, set \`allowPrivilegeEscalation: false\`, use a runtime default seccomp profile, and avoid hostPath. If the workload is infrastructure-level and genuinely needs elevated permissions, I would isolate it into a dedicated namespace with explicit labels, review, and compensating controls rather than weakening enforcement cluster-wide.

Mitigation during a production block could be a targeted exception with expiry if risk is accepted, but my preferred path is to fix the manifest and redeploy. I would validate admission success, runtime behavior, and no privilege regression. Long term, I would add policy checks in CI and provide secure base templates so teams do not learn Pod Security only at deployment time.

**Q58 \| Pod Security \| P2 \| Platform Engineering**

**Interview Question:**

> How would you create platform guardrails that prevent privileged pods, hostPath misuse, root containers, and unsafe capabilities while still allowing approved infrastructure workloads?

**Candidate Answer:**

I would build guardrails as a layered platform control: secure templates, CI policy checks, admission enforcement, and controlled exceptions. The aim is to prevent unsafe defaults while still supporting infrastructure workloads like CNI, CSI, and security agents.

For application namespaces, I would enforce non-root containers, no privileged mode, no hostPath, dropped Linux capabilities, \`allowPrivilegeEscalation: false\`, read-only root filesystem where feasible, and runtime default seccomp. I would implement this with Pod Security Admission, Kyverno or OPA Gatekeeper policies, and CI checks so failures are caught before production. Infrastructure namespaces would have stricter ownership and explicit exceptions tied to service accounts and labels.

Exception handling is important. I would require justification, expiry, risk owner, and audit trail for privileged workloads. I would publish golden Helm/Kustomize templates and developer docs. Metrics would track policy violations by team, exception count, and blocked risky deployments. This turns security from manual review into a paved road with guardrails and a clear escalation path for legitimate platform needs.

**Q59 \| ImagePullBackOff \| P1 \| Production Troubleshooting**

**Interview Question:**

> A pod enters ImagePullBackOff only in one cluster while the same image deploys elsewhere; how would you investigate registry auth, image digest availability, node network path, pull secrets, and cloud-specific registry integration?

**Candidate Answer:**

I would compare the failing cluster with a working cluster across image identity, registry access, and node path. First I would check pod events for auth failure, manifest unknown, TLS error, timeout, or rate limit.

Then I would verify the image reference uses an immutable digest or expected tag, and that the digest exists in the registry/region. I would inspect imagePullSecrets, service account references, registry credentials, node IAM/managed identity integration, and whether the failing cluster uses a different namespace or secret name. If auth is fine but pulls timeout, I would check node egress, private registry endpoint, DNS, proxy, firewall, and cloud-specific registry integration such as ECR/ACR/GAR permissions.

Mitigation could be restoring pull secret, replicating the image to the cluster’s registry region, fixing node egress, or pinning to a known-good digest. I would validate by pulling on a fresh node and watching pod start. Permanent prevention includes image replication checks, digest pinning, registry auth monitoring, and pre-deployment validation per cluster.

**Q60 \| CrashLoopBackOff \| P1 \| Production Troubleshooting**

**Interview Question:**

> A CrashLoopBackOff starts immediately after a secret change and the application logs are incomplete; how would you reconstruct the failure timeline and decide whether to rollback config, image, or secret version?

**Candidate Answer:**

I would reconstruct the timeline from Kubernetes and external systems because application logs are incomplete. I would collect deployment events, secret resource version changes, external secret sync logs, pod restart timestamps, \`logs --previous\`, and audit logs showing who changed what.

Because the failure starts after a secret change, I would verify whether the pod consumed the new value through env var or mounted file, whether it restarted, and whether the application supports live reload. I would compare auth errors against dependency logs without exposing secret values. I would also confirm whether the image changed at the same time; if both image and secret changed, I would identify the first failing ReplicaSet and config version.

Rollback decision should be evidence-based. If old pods with old secret still work, rollback secret/config first or restore dual-valid credentials. If the new image cannot handle the secret format, rollback image. If both are incompatible, return to the last known-good deployment and secret version together. Prevention includes change bundling discipline, secret version labels, canary rotation, and better startup logs with redaction.

**Q61 \| Pending Pods \| P1 \| Production Troubleshooting**

**Interview Question:**

> A batch of production pods remains Pending after a node pool scale-down; how would you determine whether capacity, pod disruption budgets, affinity, quotas, or autoscaler configuration caused the incident?

**Candidate Answer:**

I would start by checking whether the pods are unschedulable because capacity was removed faster than workloads could move. I would inspect pod events, scheduler reasons, node pool scale-down events, PDBs, quotas, affinity, topology spread, and autoscaler logs.

After scale-down, common causes are remaining nodes not satisfying affinity or taints, PDBs blocking safe movement, namespace quotas preventing replacement pods, or Cluster Autoscaler not configured to scale back up the required node group. If stateful or zonal workloads are involved, PVC topology can also block scheduling. I would compare requested resources with allocatable resources on eligible nodes, not the whole cluster.

Mitigation might be manually scaling the correct node pool, relaxing non-critical constraints, reverting the scale-down, or prioritizing critical workloads using priority classes. I would validate by watching pods schedule and service endpoints recover. Long term, I would add scale-down safeguards: disruption budgets, node pool min sizes, critical workload placement reports, autoscaler limits, and pre-scale-down simulations showing which pods would become unschedulable.

**Q62 \| Rollout Stuck \| P1 \| Production Troubleshooting**

**Interview Question:**

> A Deployment rollout is stuck because new pods are created but never become ready; how would you investigate probe design, dependency readiness, config drift, resource pressure, and progressive rollback?

**Candidate Answer:**

I would treat “created but never ready” as a rollout safety signal. First I would pause the rollout to protect old replicas, then inspect new pod events, logs, \`logs --previous\`, readiness failures, resource pressure, dependency availability, and config differences from the previous ReplicaSet.

The root cause could be a probe that checks a dependency too aggressively, a dependency not ready for the new version, bad config or secret, CPU/memory throttling during startup, or a migration that changed behavior. I would compare old and new pod templates: image digest, env vars, ConfigMap/Secret versions, resource limits, probes, service account, and labels. If new pods are healthy but readiness is too strict, I would validate with direct pod requests before changing the probe.

Mitigation would be rollback if customer risk is rising, or a targeted fix if cause is obvious and safe. I would validate endpoint readiness, rollout progress, and business SLOs. Permanent prevention includes canary deployments, readiness contract tests, config diff review, and progressive rollback automation when readiness stalls beyond threshold.

**Q63 \| Rollbacks \| P1 \| SRE**

**Interview Question:**

> A rollback is requested during a P1 incident, but the previous version depends on an older ConfigMap and database schema; how would you evaluate rollback safety before executing it?

**Candidate Answer:**

I would not execute rollback blindly during a P1 if the previous version has config and schema dependencies. A rollback can worsen the incident if the database has moved forward or old ConfigMaps are gone.

First I would identify the last known-good combination: image digest, ConfigMap/Secret version, migration state, feature flags, and database schema compatibility. I would check whether migrations were backward compatible, whether old code can read the current schema, and whether old config still exists. If not, I would evaluate safer mitigations: disable feature flag, route traffic away, scale stable subset, restore previous config, or apply a forward fix.

If rollback is safe, I would rollback image and config together, monitor DB errors, and validate critical journeys. If not safe, I would communicate that rollback has data risk and choose the lowest-risk mitigation while engineers prepare a compatible fix. Permanent prevention includes expand-contract database migrations, versioned config retention, rollback runbooks, and release gates proving N-1 compatibility before production.

**Q64 \| AKS \| P2 \| Production Troubleshooting**

**Interview Question:**

> An AKS workload loses access to Azure-managed services after node pool rotation; how would you investigate managed identity, workload identity, network security groups, private endpoints, and DNS integration?

**Candidate Answer:**

For AKS, I would focus on identity, networking, and private DNS changes introduced by node pool rotation. First I would confirm whether failures are authentication, DNS resolution, or network connectivity to Azure-managed services.

I would inspect workload identity or managed identity configuration, federated credential binding, service account annotations, pod token projection, and whether new nodes have the expected kubelet identity permissions. For networking, I would check NSGs, UDRs, private endpoint DNS zones, VNet links, and whether the new node subnet has required routes to Key Vault, Storage, ACR, or other services. I would also compare DNS resolution from old and new node pool pods.

Mitigation could be restoring identity bindings, linking private DNS to the new subnet/VNet, fixing NSG/route rules, or temporarily moving workloads back to the old pool. I would validate with token acquisition, Azure service access, DNS results, and application metrics. Long term, node pool rotation runbooks should include identity, DNS, NSG, and private endpoint conformance checks before draining old nodes.

**Q65 \| EKS \| P2 \| Production Troubleshooting**

**Interview Question:**

> An EKS workload fails to assume the expected AWS permissions after a service account change; how would you troubleshoot IAM role mapping, token projection, trust policy, namespace binding, and pod restart behavior?

**Candidate Answer:**

For EKS, I would troubleshoot IRSA or Pod Identity from the service account outward. First I would confirm the pod is using the expected Kubernetes service account and that the pod restarted after the service account annotation or binding changed.

Then I would inspect the IAM role ARN annotation or Pod Identity association, projected token audience, OIDC provider, trust policy conditions for namespace and service account, and AWS STS errors. A common issue is a mismatch in \`sub\` condition, wrong namespace, missing OIDC provider thumbprint, or reusing a service account without restarting pods. I would also check whether node role permissions are masking or conflicting with expected workload role behavior.

Mitigation would be to restore the previous service account annotation/binding, fix the trust policy, or redeploy pods to refresh projected tokens. I would validate inside the pod using caller identity and application access to AWS APIs. Permanent prevention includes IAM policy tests, namespace-bound trust templates, GitOps review for service account changes, and alerts on AccessDenied after deployment.

**Q66 \| GKE \| P2 \| Production Troubleshooting**

**Interview Question:**

> A GKE workload cannot access Google Cloud APIs after a cluster security upgrade; how would you debug workload identity, service account binding, node scopes, network egress, and policy changes?

**Candidate Answer:**

For GKE, I would first determine whether the failure is identity, network egress, or policy enforcement. After a security upgrade, Workload Identity or metadata server behavior often becomes visible.

I would inspect the Kubernetes service account, Google service account binding, IAM policy, annotation, Workload Identity pool/project, and whether the pod was restarted to pick up changes. I would test token acquisition and call a minimal Google API from a debug pod with the same service account. If identity works but API access fails, I would check IAM roles, VPC Service Controls, firewall egress, Cloud NAT/private Google access, DNS, and org policy changes. If node scopes are still relevant for older configurations, I would verify those too.

Mitigation could be restoring IAM binding, fixing service account annotation, enabling private Google access/NAT, or rolling back the security policy change if it caused broad outage. I would validate with API call success, application recovery, and audit logs. Long term, I would enforce Workload Identity templates, policy tests, and upgrade prechecks for identity and egress.

**Q67 \| Managed Kubernetes Differences \| P2 \| Architecture**

**Interview Question:**

> When supporting AKS, EKS, and GKE, what production differences in identity, load balancing, storage, node upgrades, logging, and autoscaling would influence your troubleshooting playbook?

**Candidate Answer:**

My troubleshooting playbook would keep a common Kubernetes core flow, but managed-service differences matter a lot in production. Identity is different: AKS uses managed identity/workload identity with Entra ID, EKS uses IRSA or Pod Identity with IAM/OIDC trust policies, and GKE uses Workload Identity with Google service accounts and IAM bindings.

Load balancing also differs. AKS integrates with Azure Load Balancer/Application Gateway, EKS with AWS NLB/ALB controllers and security groups, and GKE with Google Cloud Load Balancing and NEGs. Storage differs in CSI drivers, zonal/regional disk behavior, default StorageClasses, and attach limits. Node upgrades differ in surge settings, node image behavior, disruption controls, and integration with autoscalers. Logging and monitoring integrate differently with Azure Monitor, CloudWatch/Container Insights, and Cloud Logging/Monitoring.

For autoscaling, I would check cloud quota, node group/MIG/VMSS limits, subnet IP capacity, and provider-specific events. My platform standard would document provider-specific checks for identity, DNS/private endpoints, load balancer health, CSI, upgrades, and audit logs. This prevents treating all managed Kubernetes clusters as identical when the failure is in cloud integration.

**Q68 \| Kubernetes Incident Debugging \| P1 \| Leadership**

**Interview Question:**

> During a Kubernetes production outage affecting checkout traffic, how would you structure incident command, blast-radius analysis, mitigation, stakeholder updates, and technical investigation in parallel?

**Candidate Answer:**

I would run this as incident commander and keep investigation parallelized. First I would declare severity, identify customer impact on checkout, set a communication cadence, and assign roles: incident lead, Kubernetes investigator, application owner, networking/DNS owner, and communications owner.

Blast-radius analysis would start with which routes, namespaces, clusters, zones, and customer segments are affected. I would check recent changes immediately: deployment, config/secret, ingress, autoscaling, DNS, NetworkPolicy, node changes, and cloud incidents. In parallel, I would look at SLO burn rate, checkout success, ingress 5xx, pod readiness, endpoints, DNS errors, HPA/autoscaler events, and dependency saturation.

Mitigation should be the safest reversible action: rollback recent release, route traffic to stable version, scale healthy replicas, disable a feature flag, or revert a network/config change. I would avoid multiple uncoordinated fixes. Stakeholder updates would state impact, current mitigation, ETA confidence level, and next update time. After recovery, I would ensure validation through business metrics, then run a blameless RCA with timeline, detection gaps, prevention actions, owners, and due dates.

**Q69 \| Mitigation Strategy \| P1 \| SRE**

**Interview Question:**

> A Kubernetes incident has multiple possible causes including rollout, DNS, autoscaling, and network policy; how would you choose the safest short-term mitigation while investigation continues?

**Candidate Answer:**

When multiple causes are possible, I would choose mitigation based on reversibility, blast-radius reduction, and evidence strength. I would not wait for perfect RCA if customer impact is active, but I would avoid stacking risky changes.

First I would freeze new rollouts and capture current state. Then I would check the timeline: did errors start after rollout, DNS change, autoscaler event, or NetworkPolicy apply? If one change strongly correlates and rollback is safe, I would revert that first. If cause is unclear, I would choose low-risk mitigations such as increasing stable capacity, routing traffic to a known-good version, disabling a feature flag, or temporarily reverting the last network policy if it has clear blast radius. I would avoid cluster-wide restarts or broad policy deletion unless impact justifies it.

Every mitigation needs validation criteria: checkout success, 5xx, latency, ready endpoints, DNS success, and saturation. I would keep one change owner and timestamp each action. RCA comes after stabilization, but the mitigation path should preserve evidence and be reversible.

**Q70 \| RCA \| P1 \| Leadership**

**Interview Question:**

> After a Kubernetes outage caused by a bad readiness probe and aggressive rollout settings, what senior-level RCA questions would you ask to prevent recurrence across all teams?

**Candidate Answer:**

I would structure the RCA around why the system allowed a bad readiness probe and aggressive rollout to reach production, not around who wrote it. The first questions are timeline-based: when was the manifest changed, when did rollout start, when did endpoints include bad pods, when did alerts fire, and when did users feel impact?

Then I would ask technical prevention questions. Did readiness represent real application readiness or only process health? Were startup, readiness, and liveness responsibilities separated? Were rollout settings appropriate for service criticality? Did maxUnavailable, maxSurge, minReadySeconds, and PDB protect capacity? Did canary or synthetic checks catch the issue before 100% rollout? Were business metrics used as deployment gates?

I would also ask platform questions. Do templates enforce probe standards? Do CI/admission policies flag weak probes for tier-1 services? Is there a runbook for rollout-induced incidents? The action items should include probe contract standards, progressive delivery gates, manifest linting, rollout SLO checks, and team education. Success metric would be reduction in rollout-related incidents and faster detection before customer impact.

**Q71 \| Platform Ownership \| P2 \| Platform Engineering**

**Interview Question:**

> Several product teams repeatedly create Kubernetes incidents due to weak manifests and missing production standards; how would you build platform-level templates, policy checks, and education to reduce repeat failures?

**Candidate Answer:**

I would approach this as a platform product problem. Repeated Kubernetes incidents across teams usually mean the paved road is weak, not that every team is careless.

First I would analyze incident patterns: bad probes, missing resources, unsafe rollouts, weak PDBs, broad RBAC, missing NetworkPolicies, or poor config rollout. Then I would build golden templates for Deployments, Services, Ingress, HPA, PDB, probes, resource requests, securityContext, and observability annotations. These templates should be easy to adopt through Helm/Kustomize or internal service scaffolding, not just documented in a wiki.

Policy checks would enforce minimum production standards in CI and admission: no missing requests, no privileged pods, required readiness for tier-1 services, safe rollout defaults, and required labels/owners. I would pair this with education: examples, office hours, incident reviews, and migration support. Metrics would track policy violations, template adoption, Kubernetes incident count, MTTR, and deployment rollback frequency. The goal is fewer incidents through standardization and self-service, while still allowing justified exceptions with review.

**Q72 \| Resume Deep Dive \| P2 \| Resume Deep Dive**

**Interview Question:**

> From your own experience, describe a Kubernetes production incident you owned end to end, including what broke, how you isolated the issue, how you restored service, and what permanent controls you added afterward?

**Candidate Answer:**

One Kubernetes incident I would describe is a production rollout where new API pods were marked ready too early. Traffic started hitting pods before cache warm-up and dependency connections were complete, causing intermittent 5xx and latency spikes.

My responsibility was to lead triage and restore service. I first checked ingress 5xx, per-pod request distribution, EndpointSlices, readiness transitions, and deployment events. The application logs did not show a clean crash, so I compared old and new ReplicaSet templates and found the readiness endpoint only checked process health. New pods entered endpoints within seconds, but real traffic failed until warm-up finished. I paused the rollout, scaled stable replicas, and rolled back the new ReplicaSet to restore customer traffic.

After recovery, I worked with developers to split \`/live\`, \`/ready\`, and \`/startup\`, added warm-up checks to readiness, configured \`minReadySeconds\`, and changed rollout settings to \`maxUnavailable: 0\`. I also added a dashboard showing readiness transitions versus 5xx and a deployment gate using synthetic checks. The lasting improvement was fewer rollout-related errors and faster detection because Kubernetes readiness was aligned with actual service readiness.
