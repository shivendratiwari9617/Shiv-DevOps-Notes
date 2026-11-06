# Kubernetes Top 100 Production Troubleshooting Commands
**10/10 senior-level runbook pack for Kubernetes Platform Engineers, DevOps Engineers, and SREs in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic kubectl reference.

The commands are grouped by the failure domains senior responders usually work through first:

- Pods
- Deployments
- Services
- Ingress
- Nodes
- Networking
- Storage

Each command includes:

- the command itself
- what it technically checks
- when to use it during a real incident

## Recommended Incident Flow

When an incident starts, avoid random command execution. Work in this order:

1. confirm user impact and current blast radius
2. identify the likely failure domain
3. gather evidence before risky mutation
4. pause rollout, cordon, or isolate only when justified
5. restore service safely
6. verify recovery through traffic, health, and stability checks
7. convert findings into a permanent guardrail

## Fast “First 5 Commands” by Incident Type

### Traffic failures
1. `kubectl get pods -n <ns>`
2. `kubectl describe svc <name> -n <ns>`
3. `kubectl get endpoints <name> -n <ns>`
4. `kubectl describe ingress <name> -n <ns>`
5. `kubectl logs <pod> -n <ns>`

### Rollout failures
1. `kubectl get deploy <name> -n <ns>`
2. `kubectl rollout status deploy/<name> -n <ns>`
3. `kubectl describe deploy <name> -n <ns>`
4. `kubectl get rs -n <ns>`
5. `kubectl rollout history deploy/<name> -n <ns>`

### Pending pods
1. `kubectl get pods -o wide -n <ns>`
2. `kubectl describe pod <pod> -n <ns>`
3. `kubectl describe node <node>`
4. `kubectl get node <node> -o jsonpath='{.spec.taints}'`
5. `kubectl get events -A --sort-by=.metadata.creationTimestamp`

### DNS or networking failures
1. `kubectl exec -it <pod> -n <ns> -- nslookup kubernetes.default`
2. `kubectl logs -n kube-system deploy/coredns`
3. `kubectl get svc -n kube-system kube-dns`
4. `kubectl describe networkpolicy <name> -n <ns>`
5. `kubectl exec -it <pod> -n <ns> -- nc -vz <svc>.<ns>.svc.cluster.local <port>`

### Storage failures
1. `kubectl describe pod <pod> -n <ns>`
2. `kubectl describe pvc <pvc> -n <ns>`
3. `kubectl get pv`
4. `kubectl get volumeattachments`
5. `kubectl get events -A --field-selector reason=FailedAttachVolume`

---

# Pods


## 1. `kubectl get pods -A`


```bash

kubectl get pods -A

```

**Technical explanation:** Quick triage view of all pods across the cluster so you can immediately estimate incident breadth.

**Use when:** Use when you need the fastest cluster-wide snapshot of workload health after an alert fires.


## 2. `kubectl get pods -n <ns>`


```bash

kubectl get pods -n <ns>

```

**Technical explanation:** Namespace-scoped pod listing that reduces noise when the incident is already isolated to one team or service.

**Use when:** Use when blast radius appears limited to a single application namespace.


## 3. `kubectl get pods -o wide -n <ns>`


```bash

kubectl get pods -o wide -n <ns>

```

**Technical explanation:** Adds pod IPs, node placement, and scheduling locality so you can correlate failures to specific nodes or zones.

**Use when:** Use when only some replicas fail and you suspect node-level or zonal skew.


## 4. `kubectl describe pod <pod> -n <ns>`


```bash

kubectl describe pod <pod> -n <ns>

```

**Technical explanation:** Shows lifecycle events, probe failures, restarts, mounts, image pull issues, and scheduler decisions in one place.

**Use when:** Use when one pod is clearly unhealthy and you need the first deep inspection command.


## 5. `kubectl logs <pod> -n <ns>`


```bash

kubectl logs <pod> -n <ns>

```

**Technical explanation:** Fetches the current container log stream for the active failure window.

**Use when:** Use when the pod is running or recently failed and you need application-side evidence.


## 6. `kubectl logs <pod> -n <ns> --previous`


```bash

kubectl logs <pod> -n <ns> --previous

```

**Technical explanation:** Retrieves logs from the last crashed container instance, which is often the only source of startup failure evidence.

**Use when:** Use when the pod is in CrashLoopBackOff or restarted before you could capture logs.


## 7. `kubectl logs <pod> -c <container> -n <ns>`


```bash

kubectl logs <pod> -c <container> -n <ns>

```

**Technical explanation:** Targets a single container inside a multi-container pod to separate app failures from sidecar or init behavior.

**Use when:** Use when a pod contains proxies, agents, or service-mesh sidecars.


## 8. `kubectl get pod <pod> -n <ns> -o yaml`


```bash

kubectl get pod <pod> -n <ns> -o yaml

```

**Technical explanation:** Returns the full live pod object, including status, resolved spec, volume mounts, probes, and container states.

**Use when:** Use when you must compare actual runtime state to the intended deployment template.


## 9. `kubectl get pod <pod> -n <ns> -o jsonpath='{.status.phase}'`


```bash

kubectl get pod <pod> -n <ns> -o jsonpath='{.status.phase}'

```

**Technical explanation:** Extracts just the phase field for fast scriptable checks and incident automation.

**Use when:** Use when building quick triage scripts or validating state in shell loops.


## 10. `kubectl get pod <pod> -n <ns> -o jsonpath='{.status.containerStatuses[*].restartCount}'`


```bash

kubectl get pod <pod> -n <ns> -o jsonpath='{.status.containerStatuses[*].restartCount}'

```

**Technical explanation:** Shows restart counts directly so you can quantify instability without reading the full pod YAML.

**Use when:** Use when comparing several pods for crash severity.


## 11. `kubectl exec -it <pod> -n <ns> -- sh`


```bash

kubectl exec -it <pod> -n <ns> -- sh

```

**Technical explanation:** Opens an interactive shell inside the pod so you can test DNS, environment, local ports, and dependency reachability from the workload context.

**Use when:** Use when cluster objects look healthy but the application still behaves incorrectly.


## 12. `kubectl exec <pod> -n <ns> -- env`


```bash

kubectl exec <pod> -n <ns> -- env

```

**Technical explanation:** Prints live environment variables to verify secret/config injection and startup parameters.

**Use when:** Use when a release changed config, secret keys, or feature flags.


## 13. `kubectl exec <pod> -n <ns> -- cat /etc/resolv.conf`


```bash

kubectl exec <pod> -n <ns> -- cat /etc/resolv.conf

```

**Technical explanation:** Displays pod DNS resolver settings so you can confirm the runtime is using expected cluster DNS.

**Use when:** Use when service discovery or external name resolution begins failing.


## 14. `kubectl top pod <pod> -n <ns>`


```bash

kubectl top pod <pod> -n <ns>

```

**Technical explanation:** Shows current CPU and memory consumption for a single pod to identify pressure, leaks, or unexpected spikes.

**Use when:** Use when latency, restarts, or throttling suspicion points to resource stress.


# Deployments


## 15. `kubectl get deployments -A`


```bash

kubectl get deployments -A

```

**Technical explanation:** Lists deployment status cluster-wide so you can spot parallel rollout failures or multi-namespace impact.

**Use when:** Use when the incident may be tied to a platform release or shared pipeline event.


## 16. `kubectl get deploy <name> -n <ns>`


```bash

kubectl get deploy <name> -n <ns>

```

**Technical explanation:** Provides desired, updated, available, and unavailable replica counts for one deployment.

**Use when:** Use when validating whether a rollout is stalled or partially healthy.


## 17. `kubectl describe deploy <name> -n <ns>`


```bash

kubectl describe deploy <name> -n <ns>

```

**Technical explanation:** Shows rollout conditions, revision transitions, selectors, and warning events that explain why a deployment is not converging.

**Use when:** Use when a deployment reports unavailable replicas or timeout conditions.


## 18. `kubectl rollout status deploy/<name> -n <ns>`


```bash

kubectl rollout status deploy/<name> -n <ns>

```

**Technical explanation:** Watches rollout progression and tells you whether the controller considers the deployment complete.

**Use when:** Use when verifying if the incident started during an active rollout.


## 19. `kubectl rollout history deploy/<name> -n <ns>`


```bash

kubectl rollout history deploy/<name> -n <ns>

```

**Technical explanation:** Lists rollout revisions so you can tie symptoms to recent changes quickly.

**Use when:** Use when you need the change timeline before deciding on rollback.


## 20. `kubectl rollout history deploy/<name> -n <ns> --revision=<n>`


```bash

kubectl rollout history deploy/<name> -n <ns> --revision=<n>

```

**Technical explanation:** Inspects a specific past revision to compare bad and known-good rollout definitions.

**Use when:** Use when you suspect a small manifest change introduced the incident.


## 21. `kubectl rollout undo deploy/<name> -n <ns>`


```bash

kubectl rollout undo deploy/<name> -n <ns>

```

**Technical explanation:** Reverts to the previous rollout revision, making it one of the safest fast mitigations for a bad deploy.

**Use when:** Use when customer impact is active and the current revision is the obvious trigger.


## 22. `kubectl rollout pause deploy/<name> -n <ns>`


```bash

kubectl rollout pause deploy/<name> -n <ns>

```

**Technical explanation:** Stops rollout progression so additional broken pods are not created while investigation continues.

**Use when:** Use when canary or rolling update behavior is expanding blast radius.


## 23. `kubectl rollout resume deploy/<name> -n <ns>`


```bash

kubectl rollout resume deploy/<name> -n <ns>

```

**Technical explanation:** Restarts a paused rollout after you verify the issue is understood or corrected.

**Use when:** Use when recovery has been staged and further rollout is safe.


## 24. `kubectl scale deploy/<name> --replicas=<n> -n <ns>`


```bash

kubectl scale deploy/<name> --replicas=<n> -n <ns>

```

**Technical explanation:** Temporarily changes replica count to preserve capacity, isolate impact, or test scaling assumptions during the incident.

**Use when:** Use when availability depends on short-term manual capacity control.


## 25. `kubectl get rs -n <ns>`


```bash

kubectl get rs -n <ns>

```

**Technical explanation:** Lists replica sets so you can see which revision is active, scaled down, or partially serving traffic.

**Use when:** Use when deployment state is confusing and you need the underlying rollout objects.


## 26. `kubectl describe rs <rs-name> -n <ns>`


```bash

kubectl describe rs <rs-name> -n <ns>

```

**Technical explanation:** Shows pod template details and events for the specific replica set producing broken pods.

**Use when:** Use when only one revision behaves incorrectly.


## 27. `kubectl get deploy <name> -n <ns> -o yaml`


```bash

kubectl get deploy <name> -n <ns> -o yaml

```

**Technical explanation:** Returns the full live deployment object for exact inspection of image tags, probes, resources, selectors, and strategy.

**Use when:** Use when you need to confirm what is actually applied in the cluster.


## 28. `kubectl set image deploy/<name> <container>=<image> -n <ns>`


```bash

kubectl set image deploy/<name> <container>=<image> -n <ns>

```

**Technical explanation:** Changes the running image reference for controlled emergency rollback or hotfix scenarios.

**Use when:** Use when a precise image correction is safer than broader manifest edits.


## 29. `kubectl rollout restart deploy/<name> -n <ns>`


```bash

kubectl rollout restart deploy/<name> -n <ns>

```

**Technical explanation:** Restarts pods without changing the deployment spec, which is useful after secret reload, config drift, or transient runtime issues.

**Use when:** Use when the spec is correct but pods need recreation to recover.


# Services


## 30. `kubectl get svc -A`


```bash

kubectl get svc -A

```

**Technical explanation:** Shows every service object so you can map traffic entry points and spot cross-namespace routing impact.

**Use when:** Use when the incident involves service discovery, backend exposure, or unexpected traffic flow.


## 31. `kubectl get svc -n <ns>`


```bash

kubectl get svc -n <ns>

```

**Technical explanation:** Displays services in one namespace to narrow the problem space quickly.

**Use when:** Use when one product area is affected and cluster-wide output is too noisy.


## 32. `kubectl describe svc <name> -n <ns>`


```bash

kubectl describe svc <name> -n <ns>

```

**Technical explanation:** Shows ports, selectors, endpoints, annotations, and service type so you can validate the traffic contract.

**Use when:** Use when requests are failing even though pods appear healthy.


## 33. `kubectl get endpoints <name> -n <ns>`


```bash

kubectl get endpoints <name> -n <ns>

```

**Technical explanation:** Lists the pod IPs currently registered behind the service, which tells you where traffic is really going.

**Use when:** Use when debugging partial traffic failures or zero-endpoint outages.


## 34. `kubectl get endpointslice -n <ns>`


```bash

kubectl get endpointslice -n <ns>

```

**Technical explanation:** Shows endpoint distribution using the modern EndpointSlice model, which is especially useful for larger services.

**Use when:** Use when endpoint behavior looks inconsistent or services have many backends.


## 35. `kubectl describe endpoints <name> -n <ns>`


```bash

kubectl describe endpoints <name> -n <ns>

```

**Technical explanation:** Provides human-readable endpoint registration details to confirm whether ready pods are actually reachable through the service.

**Use when:** Use when selectors look right but service traffic still fails.


## 36. `kubectl get svc <name> -n <ns> -o yaml`


```bash

kubectl get svc <name> -n <ns> -o yaml

```

**Technical explanation:** Returns the full service object so you can validate selectors, ports, annotations, and exposure type exactly.

**Use when:** Use when you suspect a subtle manifest or annotation issue.


## 37. `kubectl get pods -n <ns> --show-labels`


```bash

kubectl get pods -n <ns> --show-labels

```

**Technical explanation:** Displays pod labels so you can compare them against service selectors and catch routing mismatches.

**Use when:** Use when a deployment succeeded but the service still has no or wrong backends.


## 38. `kubectl run tmp-shell --rm -it --restart=Never --image=busybox -- nslookup <svc>.<ns>.svc.cluster.local`


```bash

kubectl run tmp-shell --rm -it --restart=Never --image=busybox -- nslookup <svc>.<ns>.svc.cluster.local

```

**Technical explanation:** Launches a disposable debug pod to test in-cluster service DNS without modifying production workloads.

**Use when:** Use when application pods are too locked down for direct debugging.


## 39. `kubectl run tmp-shell --rm -it --restart=Never --image=busybox -- wget -qO- http://<svc>.<ns>.svc.cluster.local:<port>`


```bash

kubectl run tmp-shell --rm -it --restart=Never --image=busybox -- wget -qO- http://<svc>.<ns>.svc.cluster.local:<port>

```

**Technical explanation:** Performs a direct in-cluster HTTP request to the service so you can separate service/backend failure from ingress failure.

**Use when:** Use when users see errors and you need to know whether the problem starts before ingress.


## 40. `kubectl port-forward svc/<name> <local-port>:<svc-port> -n <ns>`


```bash

kubectl port-forward svc/<name> <local-port>:<svc-port> -n <ns>

```

**Technical explanation:** Creates a local tunnel to the service for targeted verification without changing cluster routing or firewall state.

**Use when:** Use when you need controlled debugging from your workstation.


## 41. `kubectl get svc <name> -n <ns> -o jsonpath='{.spec.selector}'`


```bash

kubectl get svc <name> -n <ns> -o jsonpath='{.spec.selector}'

```

**Technical explanation:** Extracts the live service selector for quick comparison against pod labels.

**Use when:** Use when suspecting a selector mismatch after a release.


## 42. `kubectl get svc <name> -n <ns> -o jsonpath='{.spec.ports[*].targetPort}'`


```bash

kubectl get svc <name> -n <ns> -o jsonpath='{.spec.ports[*].targetPort}'

```

**Technical explanation:** Shows the target container port mapping to catch service-to-pod port mistakes.

**Use when:** Use when the app listens on one port but the service may forward to another.


## 43. `kubectl get endpoints <name> -n <ns> -o wide`


```bash

kubectl get endpoints <name> -n <ns> -o wide

```

**Technical explanation:** Adds more endpoint detail so you can correlate failing backends to specific nodes or IP ranges.

**Use when:** Use when only a subset of requests fail and you suspect locality issues.


# Ingress


## 44. `kubectl get ingress -A`


```bash

kubectl get ingress -A

```

**Technical explanation:** Lists ingress resources cluster-wide so you can quickly see which external routes may be impacted.

**Use when:** Use when customers report public traffic failures and you need the full ingress scope.


## 45. `kubectl get ingress -n <ns>`


```bash

kubectl get ingress -n <ns>

```

**Technical explanation:** Shows ingress objects for one namespace to narrow a product-specific edge-routing incident.

**Use when:** Use when the alert already identifies the failing application area.


## 46. `kubectl describe ingress <name> -n <ns>`


```bash

kubectl describe ingress <name> -n <ns>

```

**Technical explanation:** Displays hosts, paths, backend mappings, annotations, and recent events for ingress debugging.

**Use when:** Use when requests fail at the edge or route to the wrong backend.


## 47. `kubectl get ingress <name> -n <ns> -o yaml`


```bash

kubectl get ingress <name> -n <ns> -o yaml

```

**Technical explanation:** Returns the full ingress manifest for exact validation of class, annotations, TLS, and path rules.

**Use when:** Use when annotation behavior or controller interpretation is in doubt.


## 48. `kubectl get ingressclass`


```bash

kubectl get ingressclass

```

**Technical explanation:** Shows available ingress classes so you can confirm the ingress is attached to the intended controller.

**Use when:** Use when an ingress object exists but is not being reconciled.


## 49. `kubectl get pods -n ingress-nginx`


```bash

kubectl get pods -n ingress-nginx

```

**Technical explanation:** Checks ingress controller pod health in clusters using ingress-nginx.

**Use when:** Use when edge traffic is failing and the controller itself may be unhealthy.


## 50. `kubectl logs deploy/ingress-nginx-controller -n ingress-nginx`


```bash

kubectl logs deploy/ingress-nginx-controller -n ingress-nginx

```

**Technical explanation:** Retrieves ingress controller logs to inspect backend failures, rejected configs, and timeout patterns.

**Use when:** Use when route config looks correct but the controller behaves unexpectedly.


## 51. `kubectl get svc -n ingress-nginx`


```bash

kubectl get svc -n ingress-nginx

```

**Technical explanation:** Shows ingress controller service exposure and load balancer mapping.

**Use when:** Use when external IP or load balancer routing is part of the incident.


## 52. `kubectl describe svc ingress-nginx-controller -n ingress-nginx`


```bash

kubectl describe svc ingress-nginx-controller -n ingress-nginx

```

**Technical explanation:** Surfaces service type, ports, annotations, and event history for the ingress controller service.

**Use when:** Use when the external load balancer is not programming correctly.


## 53. `kubectl get endpoints -n ingress-nginx`


```bash

kubectl get endpoints -n ingress-nginx

```

**Technical explanation:** Checks whether ingress controller pods are properly registered behind their service.

**Use when:** Use when the load balancer exists but traffic still drops at the controller layer.


## 54. `kubectl get secret -n <ns>`


```bash

kubectl get secret -n <ns>

```

**Technical explanation:** Lists namespace secrets so you can verify a referenced TLS secret exists where ingress expects it.

**Use when:** Use when HTTPS handshakes fail or cert references look broken.


## 55. `kubectl describe secret <tls-secret> -n <ns>`


```bash

kubectl describe secret <tls-secret> -n <ns>

```

**Technical explanation:** Confirms secret type and metadata, which helps detect wrong secret format for TLS termination.

**Use when:** Use when the ingress references a secret but certificates still fail.


## 56. `kubectl get events -n <ns> --sort-by=.metadata.creationTimestamp`


```bash

kubectl get events -n <ns> --sort-by=.metadata.creationTimestamp

```

**Technical explanation:** Shows ingress-related warnings like missing backends, invalid annotations, or secret issues in time order.

**Use when:** Use when a recent config change may have broken route reconciliation.


## 57. `kubectl port-forward deploy/ingress-nginx-controller 8080:80 -n ingress-nginx`


```bash

kubectl port-forward deploy/ingress-nginx-controller 8080:80 -n ingress-nginx

```

**Technical explanation:** Lets you test controller behavior locally even if external load balancer access is degraded.

**Use when:** Use when external path debugging is blocked by upstream network issues.


# Nodes


## 58. `kubectl get nodes`


```bash

kubectl get nodes

```

**Technical explanation:** Gives the fastest cluster-wide node readiness view for estimating infrastructure-level blast radius.

**Use when:** Use when multiple workloads fail at once or pods become pending unexpectedly.


## 59. `kubectl get nodes -o wide`


```bash

kubectl get nodes -o wide

```

**Technical explanation:** Adds internal IPs, OS, and runtime details to help correlate incident patterns with node classes or pools.

**Use when:** Use when failures appear limited to a subset of worker nodes.


## 60. `kubectl describe node <node>`


```bash

kubectl describe node <node>

```

**Technical explanation:** Shows node conditions, taints, allocatable resources, addresses, and recent events for deep triage.

**Use when:** Use when a node seems responsible for crashes, scheduling failures, or network issues.


## 61. `kubectl top nodes`


```bash

kubectl top nodes

```

**Technical explanation:** Displays real-time CPU and memory usage across nodes so you can detect hotspots, pressure, or imbalance.

**Use when:** Use when cluster capacity or noisy-neighbor behavior is suspected.


## 62. `kubectl get pods -A -o wide --field-selector spec.nodeName=<node>`


```bash

kubectl get pods -A -o wide --field-selector spec.nodeName=<node>

```

**Technical explanation:** Lists all pods on a node so you can estimate workload blast radius before isolating or draining it.

**Use when:** Use when deciding whether a single-node issue explains many symptoms.


## 63. `kubectl cordon <node>`


```bash

kubectl cordon <node>

```

**Technical explanation:** Marks a node unschedulable to prevent new workload placement while you investigate.

**Use when:** Use when the node is unstable but existing workloads have not yet been evacuated.


## 64. `kubectl uncordon <node>`


```bash

kubectl uncordon <node>

```

**Technical explanation:** Restores a node to scheduling after health is verified.

**Use when:** Use when maintenance or incident isolation is complete.


## 65. `kubectl drain <node> --ignore-daemonsets --delete-emptydir-data`


```bash

kubectl drain <node> --ignore-daemonsets --delete-emptydir-data

```

**Technical explanation:** Evicts workloads so you can safely remove a bad node from service.

**Use when:** Use when node-level failure is confirmed and workload migration is safer than local recovery.


## 66. `kubectl get events --field-selector involvedObject.kind=Node --sort-by=.metadata.creationTimestamp`


```bash

kubectl get events --field-selector involvedObject.kind=Node --sort-by=.metadata.creationTimestamp

```

**Technical explanation:** Pulls node-related events such as pressure, reboot, network errors, and eviction signals in time order.

**Use when:** Use when you need a concise event timeline for one or more troubled nodes.


## 67. `kubectl get node <node> -o jsonpath='{.status.conditions[*].type}'`


```bash

kubectl get node <node> -o jsonpath='{.status.conditions[*].type}'

```

**Technical explanation:** Extracts the condition types for quick scriptable node-health validation.

**Use when:** Use when automating checks across several nodes during an incident.


## 68. `kubectl get node <node> -o jsonpath='{.spec.taints}'`


```bash

kubectl get node <node> -o jsonpath='{.spec.taints}'

```

**Technical explanation:** Shows live taints that may block scheduling or redirect workloads unexpectedly.

**Use when:** Use when pods remain pending despite apparent capacity.


## 69. `kubectl describe pod <pod> -n <ns> | grep -i 'Node:'`


```bash

kubectl describe pod <pod> -n <ns> | grep -i 'Node:'

```

**Technical explanation:** Confirms the node hosting a given pod when you need fast placement correlation.

**Use when:** Use when tracing whether failing pods cluster on one host.


## 70. `kubectl get leases -n kube-node-lease`


```bash

kubectl get leases -n kube-node-lease

```

**Technical explanation:** Displays node heartbeat lease objects that can reveal delayed kubelet heartbeats or stale nodes.

**Use when:** Use when node readiness appears flaky or delayed.


## 71. `kubectl get pods -n kube-system -o wide`


```bash

kubectl get pods -n kube-system -o wide

```

**Technical explanation:** Shows where system components run so you can tell whether control-plane-adjacent services are also affected.

**Use when:** Use when network, DNS, or CNI symptoms may be node-local.


## 72. `kubectl get events -A --sort-by=.metadata.creationTimestamp`


```bash

kubectl get events -A --sort-by=.metadata.creationTimestamp

```

**Technical explanation:** Provides a cluster-wide chronological event stream that often exposes node failures faster than dashboards.

**Use when:** Use when the incident is broad and you need the shared story across namespaces.


# Networking


## 73. `kubectl get networkpolicy -A`


```bash

kubectl get networkpolicy -A

```

**Technical explanation:** Lists all network policies so you can quickly determine whether traffic may be intentionally blocked.

**Use when:** Use when connectivity fails between pods, namespaces, or outbound destinations.


## 74. `kubectl describe networkpolicy <name> -n <ns>`


```bash

kubectl describe networkpolicy <name> -n <ns>

```

**Technical explanation:** Shows ingress and egress rules in readable form to validate whether a path should be allowed.

**Use when:** Use when a release or policy change may have introduced isolation.


## 75. `kubectl get networkpolicy -n <ns> -o yaml`


```bash

kubectl get networkpolicy -n <ns> -o yaml

```

**Technical explanation:** Returns the exact policy definitions for deep inspection and diffing.

**Use when:** Use when human-readable describe output is not enough to verify selectors and ports.


## 76. `kubectl exec -it <pod> -n <ns> -- nslookup kubernetes.default`


```bash

kubectl exec -it <pod> -n <ns> -- nslookup kubernetes.default

```

**Technical explanation:** Tests whether the pod can resolve a well-known cluster service, giving a fast DNS sanity check.

**Use when:** Use when DNS failures are suspected but the scope is not yet clear.


## 77. `kubectl exec -it <pod> -n <ns> -- wget -qO- http://<svc>.<ns>.svc.cluster.local:<port>`


```bash

kubectl exec -it <pod> -n <ns> -- wget -qO- http://<svc>.<ns>.svc.cluster.local:<port>

```

**Technical explanation:** Checks actual pod-to-service HTTP connectivity from the workload network namespace.

**Use when:** Use when app traffic fails and you need to separate DNS, routing, and application problems.


## 78. `kubectl exec -it <pod> -n <ns> -- ping <pod-ip>`


```bash

kubectl exec -it <pod> -n <ns> -- ping <pod-ip>

```

**Technical explanation:** Performs basic IP reachability testing where ICMP is allowed by the environment.

**Use when:** Use when debugging low-level routing or overlay network behavior in permissive clusters.


## 79. `kubectl exec -it <pod> -n <ns> -- nc -vz <svc>.<ns>.svc.cluster.local <port>`


```bash

kubectl exec -it <pod> -n <ns> -- nc -vz <svc>.<ns>.svc.cluster.local <port>

```

**Technical explanation:** Tests raw TCP connectivity to a service port without needing an application-specific client.

**Use when:** Use when HTTP tooling is unavailable or you need port-level validation.


## 80. `kubectl get pods -n kube-system -l k8s-app=kube-dns`


```bash

kubectl get pods -n kube-system -l k8s-app=kube-dns

```

**Technical explanation:** Shows CoreDNS pod health in clusters using the default kube-dns labeling convention.

**Use when:** Use when multiple services report DNS resolution failures.


## 81. `kubectl logs -n kube-system deploy/coredns`


```bash

kubectl logs -n kube-system deploy/coredns

```

**Technical explanation:** Retrieves CoreDNS logs to spot upstream resolver issues, timeouts, or query saturation.

**Use when:** Use when cluster DNS is flaky or returning intermittent failures.


## 82. `kubectl get svc -n kube-system kube-dns`


```bash

kubectl get svc -n kube-system kube-dns

```

**Technical explanation:** Verifies the cluster DNS service object and assigned cluster IP.

**Use when:** Use when pods cannot resolve names and you need to confirm the DNS service still exists.


## 83. `kubectl exec -it <pod> -n <ns> -- cat /etc/hosts`


```bash

kubectl exec -it <pod> -n <ns> -- cat /etc/hosts

```

**Technical explanation:** Checks whether static host mappings inside the pod are masking or overriding normal name resolution.

**Use when:** Use when one pod behaves differently from otherwise identical replicas.


## 84. `kubectl exec -it <pod> -n <ns> -- ip route`


```bash

kubectl exec -it <pod> -n <ns> -- ip route

```

**Technical explanation:** Displays the pod routing table for advanced debugging of nonstandard network behavior.

**Use when:** Use when overlay, sidecar, or custom CNI issues are suspected.


## 85. `kubectl get endpoints -n kube-system kube-dns`


```bash

kubectl get endpoints -n kube-system kube-dns

```

**Technical explanation:** Confirms DNS backend pods are actually registered behind the kube-dns service.

**Use when:** Use when the DNS service exists but queries still fail.


## 86. `kubectl get events -n kube-system --sort-by=.metadata.creationTimestamp`


```bash

kubectl get events -n kube-system --sort-by=.metadata.creationTimestamp

```

**Technical explanation:** Shows recent DNS, CNI, and system-networking events in chronological order.

**Use when:** Use when cluster networking broke after node changes, upgrades, or policy rollout.


# Storage


## 87. `kubectl get pvc -A`


```bash

kubectl get pvc -A

```

**Technical explanation:** Lists all persistent volume claims so you can identify whether storage issues are isolated or cluster-wide.

**Use when:** Use when stateful workloads fail to start or attach volumes.


## 88. `kubectl get pvc -n <ns>`


```bash

kubectl get pvc -n <ns>

```

**Technical explanation:** Scopes PVC troubleshooting to one namespace for faster incident focus.

**Use when:** Use when one application or team reports storage problems.


## 89. `kubectl describe pvc <pvc> -n <ns>`


```bash

kubectl describe pvc <pvc> -n <ns>

```

**Technical explanation:** Shows binding state, storage class, events, access mode, and requested capacity for the claim.

**Use when:** Use when a pod is pending or container creation is blocked by volume issues.


## 90. `kubectl get pv`


```bash

kubectl get pv

```

**Technical explanation:** Lists persistent volumes to verify whether claims are bound, released, failed, or stuck.

**Use when:** Use when PVC state alone does not explain the incident.


## 91. `kubectl describe pv <pv>`


```bash

kubectl describe pv <pv>

```

**Technical explanation:** Displays backend volume details, reclaim policy, claim reference, and mount-related events.

**Use when:** Use when you need the cluster-side truth about one specific volume.


## 92. `kubectl get storageclass`


```bash

kubectl get storageclass

```

**Technical explanation:** Shows available storage classes and the default class, which is key when claims stay pending.

**Use when:** Use when a PVC does not bind and provisioner choice may be wrong.


## 93. `kubectl describe storageclass <name>`


```bash

kubectl describe storageclass <name>

```

**Technical explanation:** Explains provisioner, binding mode, reclaim behavior, and expansion settings for the class.

**Use when:** Use when storage behavior differs between environments or zones.


## 94. `kubectl get volumeattachments`


```bash

kubectl get volumeattachments

```

**Technical explanation:** Lists CSI volume attachment objects, which is essential for attach/detach incident analysis.

**Use when:** Use when a stateful pod is stuck moving between nodes.


## 95. `kubectl describe volumeattachment <name>`


```bash

kubectl describe volumeattachment <name>

```

**Technical explanation:** Shows whether a CSI attachment succeeded or failed and why.

**Use when:** Use when the storage backend appears to block pod startup.


## 96. `kubectl get pods -A | grep ContainerCreating`


```bash

kubectl get pods -A | grep ContainerCreating

```

**Technical explanation:** Quickly surfaces pods stalled in early startup phases, which often indicates mount or image issues.

**Use when:** Use when you need a fast cluster-wide scan for storage-related startup blockers.


## 97. `kubectl describe pod <pod> -n <ns>`


```bash

kubectl describe pod <pod> -n <ns>

```

**Technical explanation:** Shows mount, attach, device, and volume initialization errors directly on the pod timeline.

**Use when:** Use when one stateful workload fails and you need the most direct evidence.


## 98. `kubectl get events -A --field-selector reason=FailedMount`


```bash

kubectl get events -A --field-selector reason=FailedMount

```

**Technical explanation:** Extracts mount failures across the cluster so you can see whether the problem is isolated or systemic.

**Use when:** Use when multiple workloads are blocked waiting for storage.


## 99. `kubectl get events -A --field-selector reason=FailedAttachVolume`


```bash

kubectl get events -A --field-selector reason=FailedAttachVolume

```

**Technical explanation:** Shows attach failures, which are especially important for StatefulSets and node failovers.

**Use when:** Use when the volume exists but the workload cannot move or recover.


## 100. `kubectl get statefulsets -A`


```bash

kubectl get statefulsets -A

```

**Technical explanation:** Lists StatefulSets so you can map storage incidents to ordered, persistent workloads that have stricter recovery behavior.

**Use when:** Use when storage impact may involve replicas, ordinals, or ordered rollout logic.


# Recovery Verification Commands to Re-Run After a Fix

After mitigation, do not stop at “the object looks healthy.” Re-run the checks that prove customer recovery:

- `kubectl get pods -n <ns>`
- `kubectl describe svc <name> -n <ns>`
- `kubectl get endpoints <name> -n <ns>`
- `kubectl rollout status deploy/<name> -n <ns>`
- `kubectl top nodes`
- `kubectl top pod <pod> -n <ns>`
- `kubectl get events -A --sort-by=.metadata.creationTimestamp`

These help confirm:

- pods are stable, not just recreated
- traffic is reaching healthy backends
- rollout has converged cleanly
- no new node or capacity pressure is building
- the cluster is not quietly generating fresh warnings

# Troubleshooting Discipline

Strong Kubernetes incident response is not about knowing the most commands. It is about:

- choosing the right layer first
- collecting evidence before mutation
- reducing blast radius safely
- validating full recovery, not partial recovery
- turning incidents into better platform defaults

In senior-level product environments, the difference between average and excellent response is judgment, sequencing, and verification.
