# 10 Kubernetes Production Issues and Troubleshooting Steps — Interview-Ready Answers

This file is written for **interview revision**.  
Each issue is structured as:

1. **Interview question**
2. **What I would say**
3. **Failure boundary**
4. **Troubleshooting steps**
5. **Redacted log / trace evidence**
6. **Fix**
7. **Prevention**
8. **Open-source references**

> The log and trace snippets are **realistic redacted examples** based on common production failure modes. They are included to show how an engineer should reason from evidence.

---

## 1) Pods are Running, but the Service is still down

### Interview question
**A Deployment is healthy and pods are Running, but customers still cannot access the service. How would you troubleshoot it?**

### What I would say
If pods are Running but the service is still down, I would not assume the application is healthy. In Kubernetes, **Running only means the container process started**. I would trace the request path in order: Ingress or LoadBalancer, Service, Endpoints or EndpointSlices, pod readiness, then the application listener inside the container. In production, this is very often a mismatch between Service selectors, ports, readiness state, or ingress routing.

### Failure boundary
The break is usually one of these:
- Service selector does not match pod labels
- Service `targetPort` does not match container port
- readiness probe is failing, so no endpoints are published
- Ingress points to the wrong Service or path
- app listens on localhost instead of `0.0.0.0`

### Troubleshooting steps
1. Check whether the Service has endpoints.
2. Verify pod labels match the Service selector.
3. Compare Service `port` and `targetPort` with the container port.
4. Confirm the pod is Ready, not just Running.
5. Exec into the pod or use an ephemeral debug container to verify the process is listening on the expected port.
6. If Ingress is involved, verify the request is reaching the correct backend Service.

### Redacted log / trace evidence
```text
kubectl get svc api -o wide
NAME   TYPE        CLUSTER-IP      PORT(S)   AGE
api    ClusterIP   10.96.121.44    80/TCP    10d

kubectl get endpoints api
NAME   ENDPOINTS   AGE
api    <none>      10d

kubectl describe pod api-6c5f87d7d8-z9nkp
Labels: app=api-v2
Status: Running
Ready: False

kubectl logs api-6c5f87d7d8-z9nkp
INFO server starting on 127.0.0.1:8080
```

### Fix
Correct the Service selector, fix the exposed port mapping, and make sure the application listens on `0.0.0.0` so Kubernetes networking can reach it.

### Prevention
Use standard label conventions, validate Service-to-pod wiring in CI, and gate production rollout on endpoint availability plus real HTTP checks.

### Open-source references
- Kubernetes Services: https://kubernetes.io/docs/concepts/services-networking/service/
- EndpointSlices: https://kubernetes.io/docs/concepts/services-networking/endpoint-slices/
- Debugging Services: https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/

---

## 2) CrashLoopBackOff after a new release

### Interview question
**A workload enters CrashLoopBackOff immediately after deployment. How would you diagnose it?**

### What I would say
If I see `CrashLoopBackOff`, I first want to know whether the container is crashing because of **bad startup logic, missing config, failed dependency initialization, or an aggressive probe**. I would inspect `kubectl describe pod`, the previous container logs, and the startup sequence. In production, the fastest mistake is to keep redeploying while the real signal is already in the last terminated container state.

### Failure boundary
Typical causes:
- missing environment variable or secret
- startup command exits immediately
- migration/init logic fails
- liveness probe kills the container before startup completes
- runtime dependency such as database or broker is unavailable

### Troubleshooting steps
1. Run `kubectl describe pod` and inspect the last termination reason.
2. Pull previous logs using `kubectl logs --previous`.
3. Check whether a Secret, ConfigMap, or mounted file is missing.
4. Review probe configuration, especially if the app has slow startup.
5. Confirm init containers or startup tasks completed successfully.
6. Verify external dependency reachability only after local config is confirmed.

### Redacted log / trace evidence
```text
kubectl describe pod payments-6f8bbf7f7c-jhx2w
Last State:     Terminated
  Reason:       Error
  Exit Code:    1

kubectl logs payments-6f8bbf7f7c-jhx2w --previous
FATAL: required env DB_URL is not set

Events:
Warning  BackOff    kubelet  Back-off restarting failed container
```

### Fix
Restore the missing configuration, correct the container command or entrypoint, and switch to a `startupProbe` if the app needs more time before liveness should begin.

### Prevention
Validate required config during deployment, template-check Secrets and ConfigMaps, and use `startupProbe` for slow boot applications.

### Open-source references
- Pod lifecycle: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/
- Liveness, Readiness, Startup Probes: https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/
- Debug Running Pods: https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/

---

## 3) Readiness probe keeps failing, but the app logs look normal

### Interview question
**How would you troubleshoot a pod that stays unready even though the application seems up?**

### What I would say
If the pod never becomes Ready, I treat that as a **traffic admission problem**, not just a pod problem. Kubernetes is telling me it is not safe to send traffic yet. I would compare the readiness probe definition with the app’s actual behavior: correct path, port, protocol, host binding, auth expectations, and startup time. A lot of outages come from the app exposing `/readyz` while the probe still calls `/health`.

### Failure boundary
Common causes:
- wrong probe path or port
- app binds only to localhost
- readiness endpoint depends on a downstream system that is currently slow
- TLS or auth is required, but probe is plain HTTP
- initial delay is too short

### Troubleshooting steps
1. Inspect probe settings in the Pod spec.
2. Exec into the pod and curl the exact readiness path and port locally.
3. Verify whether the process binds to `0.0.0.0`.
4. Check whether the readiness endpoint incorrectly depends on a slow downstream.
5. Review event history for repeated probe failures.
6. Confirm whether a `startupProbe` should protect slow bootstrap.

### Redacted log / trace evidence
```text
kubectl describe pod api-54f84f8878-p9q2r
Readiness: http-get http://:8080/health delay=5s timeout=1s period=10s #success=1 #failure=3
Warning  Unhealthy  kubelet  Readiness probe failed: HTTP probe failed with statuscode: 404

kubectl exec api-54f84f8878-p9q2r -- curl -s -o /dev/null -w "%{http_code}\n" localhost:8080/readyz
200
```

### Fix
Point the readiness probe at the correct endpoint, relax initial timing if startup is slow, and keep readiness tied to “can receive traffic safely,” not to every optional dependency.

### Prevention
Standardize readiness endpoint naming and make probe definitions reusable through Helm or Kustomize templates.

### Open-source references
- Probes: https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/
- Configure probes task: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/

---

## 4) OOMKilled under real traffic

### Interview question
**A pod is stable in staging but gets OOMKilled in production. How would you handle it?**

### What I would say
If a pod is being OOMKilled, I would not just raise the memory limit blindly. I want to know whether the issue is **a memory leak, wrong JVM/runtime sizing, burst traffic, cache growth, or an unrealistic request/limit policy**. In production, OOMs are often hidden capacity-design problems, not just low limits.

### Failure boundary
Typical causes:
- memory limit lower than real runtime need
- application memory leak
- runtime heap not aligned to cgroup limit
- request too low, causing node overpacking
- burst traffic causes in-memory queue or cache growth

### Troubleshooting steps
1. Confirm `OOMKilled` in pod status and events.
2. Compare memory request and limit.
3. Review container memory usage over time.
4. Check runtime-specific settings such as JVM heap or Node.js memory limits.
5. Correlate OOM timing with traffic spikes or queue buildup.
6. Inspect whether the pod is caching unbounded data.

### Redacted log / trace evidence
```text
kubectl describe pod worker-7d84cf9b7c-8q5p4
Last State:     Terminated
  Reason:       OOMKilled
  Exit Code:    137

kubectl top pod worker-7d84cf9b7c-8q5p4
NAME                     CPU(cores)   MEMORY(bytes)
worker-7d84cf9b7c-8q5p4  480m         1018Mi

Container spec:
requests:
  memory: "256Mi"
limits:
  memory: "512Mi"
```

### Fix
Set realistic requests and limits, align runtime memory settings to the container limit, and fix the memory growth pattern if it is an application issue.

### Prevention
Track p95 and p99 memory usage, size requests from real production data, and avoid unbounded in-memory buffers.

### Open-source references
- Resource Management: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
- Assign memory resources: https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/

---

## 5) Pending pods due to scheduling failure

### Interview question
**Pods remain Pending after deployment. What is your troubleshooting sequence?**

### What I would say
If pods stay Pending, I would focus on the **scheduler decision**, not the application. Kubernetes is telling me it cannot place the pod. I would inspect scheduling events and look for resource shortage, node selectors, taints and tolerations, topology rules, PVC binding delays, or quota restrictions. The fastest signal is usually in `kubectl describe pod`.

### Failure boundary
Usual causes:
- insufficient CPU or memory
- node selector or affinity mismatch
- taint not tolerated
- PVC not bound
- PodDisruptionBudget / topology spread / quota constraints

### Troubleshooting steps
1. Run `kubectl describe pod` and inspect scheduler events.
2. Check requested CPU and memory against available node capacity.
3. Review node selectors, affinity, and tolerations.
4. Confirm PVCs are bound if storage is required.
5. Check namespace quota and limit ranges.
6. Inspect whether topology spread or anti-affinity is too strict.

### Redacted log / trace evidence
```text
Events:
Warning  FailedScheduling  default-scheduler  0/6 nodes are available:
2 Insufficient memory,
2 node(s) didn't match node selector,
1 node(s) had untolerated taint {dedicated: batch},
1 node(s) had volume node affinity conflict
```

### Fix
Adjust resource requests, correct scheduling constraints, bind storage properly, or add capacity in the right node pool.

### Prevention
Review scheduler constraints during code review, keep pod requests realistic, and alert on Pending duration for critical workloads.

### Open-source references
- Scheduling: https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/
- Assign Pods to Nodes: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- Taints and Tolerations: https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/

---

## 6) DNS resolution intermittently fails inside the cluster

### Interview question
**Services sometimes fail to resolve other services by name. How would you investigate Kubernetes DNS issues?**

### What I would say
Intermittent DNS failures usually point to a **cluster-level dependency problem** such as CoreDNS overload, upstream DNS latency, network policy issues, or node-local DNS misbehavior. I would reproduce the issue from a pod, inspect CoreDNS health, check query volume, and confirm whether the failure is cluster-wide or namespace-specific.

### Failure boundary
Common causes:
- CoreDNS pods overloaded or restarting
- upstream resolver latency
- NetworkPolicy blocks DNS traffic to CoreDNS
- ndots/search behavior causing excessive lookups
- node networking or conntrack pressure

### Troubleshooting steps
1. Reproduce name lookup from an affected pod.
2. Check CoreDNS pod health and restarts.
3. Inspect CoreDNS logs for timeout or SERVFAIL patterns.
4. Confirm UDP/TCP 53 path is allowed by NetworkPolicy.
5. Check whether DNS load spiked after an application change.
6. Verify whether FQDN use would reduce lookup amplification.

### Redacted log / trace evidence
```text
kubectl exec web-7f6d99b8d8-zx2tk -- nslookup redis.default.svc.cluster.local
;; connection timed out; no servers could be reached

kubectl logs -n kube-system coredns-6d4b75cb6d-v4g5r
[ERROR] plugin/errors: 2 redis.default.svc.cluster.local. A: read udp 10.244.0.23:54213->10.96.0.10:53: i/o timeout
```

### Fix
Restore DNS path to CoreDNS, scale CoreDNS if overloaded, and reduce unnecessary query amplification from application resolvers.

### Prevention
Monitor DNS error rates and latency, standardize FQDN usage where helpful, and ensure NetworkPolicies always allow DNS.

### Open-source references
- DNS for Services and Pods: https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/
- Debug DNS Resolution: https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/

---

## 7) Ingress returns 404 or routes to the wrong backend

### Interview question
**An Ingress exists, but traffic returns 404 or hits the wrong service. How would you troubleshoot it?**

### What I would say
I would debug this from the edge inward. A 404 at ingress often means the request never matched the rule I expected. I would check host matching, path matching, ingress class, rewrite behavior, backend Service name, and endpoint health. In production, teams often look at the app too early when the routing rule is the real bug.

### Failure boundary
Typical causes:
- wrong host or path rule
- wrong ingress class
- path rewrite mismatch
- backend Service name typo
- backend Service has no endpoints

### Troubleshooting steps
1. Inspect the Ingress rules and class.
2. Compare the real request host/path with the manifest.
3. Check ingress-controller logs for route selection.
4. Verify the backend Service exists and has endpoints.
5. Confirm any rewrite annotations match the app’s expected URL structure.
6. If multiple ingresses share a hostname, check for conflicting rules.

### Redacted log / trace evidence
```text
kubectl describe ingress api
Rules:
  Host             Path  Backends
  api.example.com
                   /v1   api-service:80

ingress-nginx logs:
2026/10/04 09:18:22 [info] 10.0.0.12 - "GET /health HTTP/1.1" 404 153 "-" "curl/8.1.0" host="api.example.com"
2026/10/04 09:18:22 [info] no ingress rule matched
```

### Fix
Correct the host/path rule, fix the ingress class or rewrite annotations, and verify backend endpoint availability.

### Prevention
Use validated ingress templates and keep public endpoint smoke tests in deployment gates.

### Open-source references
- Ingress: https://kubernetes.io/docs/concepts/services-networking/ingress/
- ingress-nginx docs: https://kubernetes.github.io/ingress-nginx/

---

## 8) Node goes NotReady and workloads start failing

### Interview question
**One node becomes NotReady and several workloads are impacted. How would you respond?**

### What I would say
If a node goes `NotReady`, I would treat it as an **infrastructure boundary issue first**. I would identify whether the node problem is kubelet failure, container runtime failure, disk pressure, network partition, or cloud-instance health. My first priority is to contain blast radius, see which workloads were on that node, and decide whether to cordon, drain, or replace it.

### Failure boundary
Typical causes:
- kubelet stopped reporting
- container runtime failure
- disk pressure or inode exhaustion
- node network partition
- underlying VM or cloud instance issue

### Troubleshooting steps
1. Check node conditions and events.
2. Identify which critical pods were scheduled there.
3. Inspect kubelet and container runtime logs if node access exists.
4. Check disk pressure, memory pressure, and filesystem usage.
5. Cordon and drain if the node is unstable and workloads can move safely.
6. Replace the node if health does not recover quickly.

### Redacted log / trace evidence
```text
kubectl describe node ip-10-0-12-44
Conditions:
  Ready              False
  DiskPressure       True
  MemoryPressure     False

Events:
Warning  NodeNotReady   kubelet stopped posting node status
Warning  EvictionThresholdMet  Attempting to reclaim ephemeral-storage

journalctl -u kubelet
failed to garbage collect required amount of images. Wanted to free 2147483648 bytes, but freed 0 bytes
```

### Fix
Recover or replace the node, clean up disk pressure, and reschedule workloads to healthy nodes.

### Prevention
Monitor node conditions, ephemeral storage, and image garbage collection behavior. Keep system and workload disks sized realistically.

### Open-source references
- Node Status: https://kubernetes.io/docs/reference/node/node-status/
- Node-pressure eviction: https://kubernetes.io/docs/concepts/scheduling-eviction/node-pressure-eviction/

---

## 9) NetworkPolicy blocks application traffic unexpectedly

### Interview question
**A recent NetworkPolicy change caused one service to stop talking to another. How would you debug it?**

### What I would say
I would assume the policy change narrowed traffic more than intended. With NetworkPolicy incidents, I want to prove three things in order: whether the source pod is selected, whether the destination pod is selected, and whether the required ingress or egress rule actually allows the flow. In production, these issues are often label mismatches or missing DNS egress rather than a total network failure.

### Failure boundary
Common causes:
- policy selects more pods than expected
- required egress rule missing
- required ingress rule missing
- namespace selector or pod selector label mismatch
- DNS traffic blocked, making service discovery fail first

### Troubleshooting steps
1. Identify source pod, destination pod, and required protocol/port.
2. List all policies selecting each side.
3. Verify pod labels and namespace labels.
4. Test direct connectivity and DNS separately.
5. Check whether there is a default-deny policy in effect.
6. Confirm whether the CNI actually enforces NetworkPolicy in the cluster.

### Redacted log / trace evidence
```text
kubectl get networkpolicy -n payments
NAME                POD-SELECTOR       AGE
default-deny        <none>             14d
allow-api-egress    app=api            2h

kubectl exec api-6b8d9f6f8f-b7m2w -- nc -zv postgres.db.svc.cluster.local 5432
nc: connect to postgres.db.svc.cluster.local port 5432 (tcp) failed: Operation timed out

kubectl exec api-6b8d9f6f8f-b7m2w -- nslookup postgres.db.svc.cluster.local
;; connection timed out; no servers could be reached
```

### Fix
Correct the selectors, allow DNS explicitly if needed, and add the exact ingress or egress rule required for the dependency.

### Prevention
Treat NetworkPolicy like code with peer review, test source-to-destination paths before rollout, and keep label conventions strict.

### Open-source references
- Network Policies: https://kubernetes.io/docs/concepts/services-networking/network-policies/
- Debug Services and networking basics: https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/

---

## 10) Rolling update succeeded, but user traffic got worse

### Interview question
**The Deployment rollout succeeded, but after release the 5xx rate and latency increased. How would you handle that?**

### What I would say
If Kubernetes says the rollout succeeded but user traffic got worse, I would say the deployment validated **object state, not customer state**. I would compare the rollout timestamp with application metrics, ingress errors, dependency latency, and the exact image digest or config revision that went live. In production, many incidents happen because teams equate `rollout status` with a successful release.

### Failure boundary
Typical causes:
- bad image or config revision
- hidden prod-only config change
- probe is too shallow to detect user-path failure
- dependency degradation under real traffic
- canary or rollout gate missing

### Troubleshooting steps
1. Correlate release timestamp with user-facing error and latency changes.
2. Verify the exact image digest and config revision deployed.
3. Compare logs and metrics before and after rollout.
4. Check ingress/backend errors and dependency latency.
5. Decide rollback versus fix-forward based on blast radius and safety.
6. Verify recovery using the real customer path.

### Redacted log / trace evidence
```text
kubectl rollout status deployment/api
deployment "api" successfully rolled out

Release annotation:
commit=8f1cae2
image=registry.example.com/api@sha256:91b7...

Metrics:
5xx_rate=17%
p95_latency=4.2s

Ingress logs:
upstream connect error or disconnect/reset before headers

App trace:
trace_id=ab82f1
POST /checkout -> redis timeout -> request failed 502
```

### Fix
If the new revision is clearly responsible and safe to reverse, roll back immediately. If rollback is unsafe, fix forward with the smallest targeted change and watch the real endpoint recover.

### Prevention
Use progressive delivery, canary analysis, and post-deploy checks that validate the actual customer path, not only pod readiness.

### Open-source references
- Deployments and rollout: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- Rollback: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment
- Argo Rollouts docs: https://argo-rollouts.readthedocs.io/

---

# How to answer Kubernetes incidents in interviews

Use this order every time:

1. **Start with the user symptom**
2. **Name the likely failure boundary**
3. **Give the first troubleshooting steps in order**
4. **Use one log, event, or trace to prove direction**
5. **State the fix**
6. **End with prevention**

That answer pattern sounds experienced because it is evidence-led, operational, and recovery-aware.
