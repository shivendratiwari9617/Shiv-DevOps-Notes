# Kubernetes Technical Notes

## 1. Cluster Architecture

Kubernetes is a declarative orchestration system that manages containerized workloads through a versioned API, controller reconciliation loops, and node-local execution agents.
Its architecture separates a control plane that stores and evaluates desired state from a node data plane that runs Pods, implements networking, and mounts storage.

### 1.1 Control Plane

The control plane is the authoritative management layer for scheduling, admission, reconciliation, and object persistence.
All desired state flows through the API server, is persisted in etcd, and is acted on by controllers and schedulers.

#### kube-apiserver

The API server is the front door for all Kubernetes operations and the only core component that writes directly to etcd.
It authenticates, authorizes, admits, validates, defaults, converts, and serves Kubernetes resources over a versioned REST interface.

**Commands**

```sh
kubectl cluster-info
kubectl api-resources
kubectl api-versions
kubectl explain pod
kubectl explain deployment.spec.template.spec
```

#### etcd

etcd is the strongly consistent, distributed key-value store that persists Kubernetes cluster state.
It uses the Raft consensus algorithm, so write availability depends on quorum and network stability between members.

#### kube-scheduler

The scheduler assigns unscheduled Pods to Nodes.
It runs a placement pipeline of filtering and scoring plugins, then issues a binding decision for the selected node.

#### kube-controller-manager

The controller manager runs built-in reconciliation loops for core resources such as Deployments, Jobs, Nodes, and EndpointSlices.
Each controller watches API state and issues create, update, or delete operations until observed state converges toward desired state.

#### cloud-controller-manager

The cloud controller manager integrates Kubernetes with an external cloud provider API.
It usually manages node metadata, routes, and LoadBalancer provisioning without embedding provider-specific code into core binaries.

### 1.2 Node Components

Node components form the execution layer that runs workloads and realizes networking and storage on worker machines.
They consume desired state from the control plane and materialize containers, mounts, addresses, and health reports locally.

#### kubelet

The kubelet is the node agent responsible for ensuring that PodSpecs assigned to the node are running.
It pulls images, mounts volumes, starts containers through the runtime, executes probes, and reports Pod and node status back to the API server.

#### container runtime

The container runtime is the software responsible for running containers and Pod sandboxes on a node.
It implements the Container Runtime Interface (CRI) and manages image pulls, container lifecycle, and runtime isolation behavior.

#### kube-proxy

kube-proxy implements Service virtual IP forwarding behavior on nodes.
It typically programs iptables or IPVS rules so traffic addressed to a Service is forwarded to a healthy backend Pod.

#### CNI

CNI is the plugin contract used to configure Pod networking.
A CNI implementation allocates Pod IP addresses, wires interfaces, configures routes, and may enforce NetworkPolicy depending on feature support.

#### CSI

CSI is the plugin contract used to integrate external storage systems with Kubernetes.
A CSI driver provides controller and node-side logic for provisioning, publishing, mounting, snapshotting, and resizing volumes.

### 1.3 High Availability

High availability requires eliminating single points of failure in the control plane and maintaining datastore quorum.
Production clusters usually run replicated API servers, multiple controller-manager and scheduler instances, and an odd-number etcd member set.

#### etcd quorum

etcd quorum is the majority of members required to commit a write.
If quorum is lost, reads may still be possible in limited forms, but durable state mutation cannot proceed safely.

#### leader election

Leader election allows only one active instance of certain replicated control-plane components to perform specific actions at a time.
It prevents duplicate reconciliation behavior while allowing standby instances to take over after failure.

#### failure domains

Failure domains are infrastructure boundaries across which failures can correlate.
Important domains include node, rack, zone, network segment, storage backend, and control-plane quorum boundaries.

---

## 2. Kubernetes API and Object Model

Kubernetes is driven by API objects that describe desired state rather than imperative process execution.
Controllers observe those objects and continuously reconcile actual cluster state toward the declared configuration.

### 2.1 Declarative Model

In the declarative model, the user describes the desired end state and the system determines the actions needed to reach it.
This enables idempotent automation, retries, drift correction, and eventual consistency under failure.

#### spec and status

`spec` describes desired state and is usually written by users or automation.
`status` describes observed state and is typically written by controllers, schedulers, or kubelets.

#### reconciliation loop

A reconciliation loop repeatedly compares desired state with observed state.
It acts until convergence is reached or blocked by validation, dependency, or runtime errors.

#### resourceVersion

`resourceVersion` is an opaque concurrency token associated with an object instance in storage.
It is used for optimistic concurrency control and for watch stream progression.

### 2.2 API Resources and Schema

API resources are typed objects served under versioned API groups.
Each resource defines schema, validation rules, scope, lifecycle semantics, and version compatibility expectations.

#### kind

`kind` identifies the object type represented in a manifest or API response.
It is interpreted together with `apiVersion` to resolve the actual resource schema.

#### apiVersion

`apiVersion` identifies the API group and version used to interpret the resource.
It determines schema, conversion behavior, and compatibility guarantees.

#### metadata

`metadata` contains common identity and lifecycle fields such as `name`, `namespace`, `uid`, `labels`, `annotations`, and `ownerReferences`.
It appears on all persisted Kubernetes objects.

### 2.3 Labels, Selectors, and Annotations

Labels and selectors are the primary mechanism for grouping and matching resources.
Annotations store non-identifying metadata for tools, rollouts, checksums, and controller hints.

#### labels

Labels are key-value pairs intended for object identity and queryable grouping.
They are used by selectors in controllers, Services, and command-line filtering.

#### selectors

Selectors match objects by label through equality-based or set-based expressions.
They determine controller ownership, Service backends, and operational grouping boundaries.

#### annotations

Annotations are arbitrary key-value metadata that do not participate in selector matching.
They are commonly used for release metadata, checksums, sidecar injection hints, and documentation.

**Commands**

```sh
kubectl get pods --show-labels
kubectl label pod demo env=dev
kubectl annotate pod demo note="test-pod"
kubectl get pods -l app=web
kubectl get pods -l 'env in (dev,stage)'
```

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: labeled-pod
  labels:
    app: web
    tier: frontend
  annotations:
    checksum/config: "abc123"
spec:
  containers:
    - name: nginx
      image: nginx:1.27
```

### 2.4 Namespaces and Scope

Namespaces partition namespaced resources into logical scopes within the cluster.
They provide name isolation and attach policy boundaries, but by themselves do not form a complete security boundary.

#### namespaced resources

Namespaced resources exist inside a namespace and are addressed by name within that namespace.
Examples include Pods, Services, Deployments, Secrets, ConfigMaps, and PersistentVolumeClaims.

#### cluster-scoped resources

Cluster-scoped resources exist globally and are not attached to a namespace.
Examples include Nodes, Namespaces, StorageClasses, PersistentVolumes, ClusterRoles, and CRDs.

**Commands**

```sh
kubectl get ns
kubectl create ns demo
kubectl config set-context --current --namespace=demo
kubectl get all -n demo
```

**YAML**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: demo
```

### 2.5 Ownership and Deletion

Kubernetes models relationships between objects through ownership metadata and managed deletion semantics.
Deletion is often asynchronous because finalizers, garbage collection, and dependent-object cleanup may delay actual removal.

#### ownerReferences

`ownerReferences` define parent-child relationships between objects.
They enable garbage collection of dependents when an owning object is deleted.

#### finalizers

Finalizers are metadata entries that block permanent deletion until cleanup logic completes.
They are commonly used when deleting an object requires external cleanup such as cloud resources or snapshots.

#### garbage collection

Garbage collection removes dependent objects based on ownership graphs.
It supports foreground, background, and orphan propagation behaviors.

**YAML**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: protected-config
  finalizers:
    - example.com/cleanup
data:
  mode: "safe"
```

---

## 3. Workloads and Controllers

Kubernetes workloads are usually expressed through Pod templates managed by higher-level controllers.
Controllers handle replica maintenance, rollout behavior, retries, stable identity, and lifecycle convergence.

### 3.1 Pod

A Pod is the smallest deployable workload unit in Kubernetes.
It groups one or more tightly coupled containers that share a network namespace, IPC namespace, and Pod-level volumes.

#### Pod sandbox

The Pod sandbox is the isolated execution environment that provides shared namespaces for containers in the Pod.
It holds the Pod IP and shared networking context.

#### restartPolicy

`restartPolicy` defines how kubelet restarts containers after exit.
Common values are `Always`, `OnFailure`, and `Never`, though higher-level controllers also shape restart behavior.

#### termination lifecycle

Pod termination begins with a graceful shutdown signal and a termination grace period.
If containers do not exit in time, they are forcibly terminated after the grace period expires.

**Commands**

```sh
kubectl run demo --image=nginx:1.27
kubectl get pod demo -o wide
kubectl describe pod demo
kubectl logs demo
kubectl exec -it demo -- sh
kubectl delete pod demo --grace-period=30
```

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: demo
spec:
  terminationGracePeriodSeconds: 30
  containers:
    - name: nginx
      image: nginx:1.27
      ports:
        - containerPort: 80
```

### 3.2 ReplicaSet

A ReplicaSet maintains a stable number of matching Pod replicas.
It creates missing Pods and deletes excess Pods until live replica count matches the declared value.

### 3.3 Deployment

A Deployment is the declarative controller for stateless replicated applications.
It manages ReplicaSets and provides rollout, rollback, and revision tracking semantics.

#### rolling update

A rolling update replaces old Pods with new Pods incrementally.
It is controlled by `maxSurge` and `maxUnavailable` to balance speed and availability.

#### rollback

Rollback restores a Deployment to a previous template revision.
It is usually performed by promoting an older ReplicaSet template back into active use.

#### revision history

Revision history records prior ReplicaSets created by template changes.
It allows inspection and rollback of earlier deployment states.

**Commands**

```sh
kubectl create deployment web --image=nginx:1.27 --replicas=3
kubectl get deploy,rs,pods
kubectl rollout status deploy/web
kubectl set image deploy/web nginx=nginx:1.28
kubectl rollout history deploy/web
kubectl rollout undo deploy/web
kubectl rollout restart deploy/web
```

**YAML**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.27
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "500m"
              memory: "256Mi"
```

### 3.4 StatefulSet

A StatefulSet is used for applications requiring stable identity, ordered rollout, and persistent storage association.
It assigns predictable Pod names and binds per-replica storage to ordinal identity.

#### stable identity

StatefulSet Pods receive deterministic names such as `db-0`, `db-1`, and `db-2`.
This enables stable peer identity for clustered databases and quorum systems.

#### ordered operations

StatefulSets create, update, and delete Pods in ordinal order by default.
This ordered behavior is important for applications with startup, shutdown, or membership dependencies.

#### headless service dependency

StatefulSets commonly depend on a headless Service for stable DNS records per Pod.
This enables identity-based service discovery rather than only load-balanced access.

**Commands**

```sh
kubectl get sts
kubectl describe sts db
kubectl rollout status sts/db
kubectl get pods -l app=db
```

**YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db
spec:
  clusterIP: None
  selector:
    app: db
  ports:
    - port: 5432
      targetPort: 5432
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: db
spec:
  serviceName: db
  replicas: 2
  selector:
    matchLabels:
      app: db
  template:
    metadata:
      labels:
        app: db
    spec:
      containers:
        - name: postgres
          image: postgres:16
          ports:
            - containerPort: 5432
          volumeMounts:
            - name: data
              mountPath: /var/lib/postgresql/data
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 5Gi
```

### 3.5 DaemonSet

A DaemonSet ensures that a Pod runs on selected nodes across the cluster.
It is typically used for node-local infrastructure such as log collectors, CNI agents, storage agents, and observability daemons.

**Commands**

```sh
kubectl get ds -A
kubectl describe ds node-agent
```

**YAML**

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-agent
spec:
  selector:
    matchLabels:
      app: node-agent
  template:
    metadata:
      labels:
        app: node-agent
    spec:
      tolerations:
        - operator: Exists
      containers:
        - name: agent
          image: fluent/fluent-bit:3.1
```

### 3.6 Job

A Job runs Pods until a specified number of successful completions occur.
It is used for finite batch processing rather than continuously running services.

#### completions and parallelism

`completions` defines how many successful runs are required.
`parallelism` defines how many Pods may run concurrently while pursuing those completions.

#### backoffLimit

`backoffLimit` bounds how many failed Pod retries occur before the Job is marked failed.
It prevents endless retries under persistent error conditions.

**Commands**

```sh
kubectl create job pi --image=perl:5.34 -- perl -Mbignum=bpi -wle 'print bpi(20)'
kubectl get jobs
kubectl logs job/pi
kubectl describe job pi
```

**YAML**

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  backoffLimit: 2
  template:
    spec:
      restartPolicy: Never
      containers:
        - name: pi
          image: perl:5.34
          command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(20)"]
```

### 3.7 CronJob

A CronJob creates Jobs on a cron-like schedule.
It is used for recurring tasks such as backups, reports, maintenance, or cleanup operations.

#### schedule and concurrencyPolicy

The schedule determines when Jobs are created.
`concurrencyPolicy` controls whether overlapping runs are allowed, forbidden, or replaced.

**Commands**

```sh
kubectl get cronjobs
kubectl create cronjob hello --image=busybox --schedule="*/5 * * * *" -- echo hello
kubectl describe cronjob hello
```

**YAML**

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/5 * * * *"
  concurrencyPolicy: Forbid
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
            - name: hello
              image: busybox:1.36
              command: ["sh", "-c", "date; echo hello"]
```

### 3.8 Init, Sidecar, and Ephemeral Containers

Different container roles inside a Pod support setup, auxiliary runtime behavior, and debugging.
Their lifecycle semantics differ and affect startup order, restart behavior, and operational use.

#### init containers

Init containers run to completion before application containers start.
They are used for setup tasks such as migration checks, configuration generation, or dependency readiness gating.

#### sidecars

Sidecars are long-running support containers that run alongside the main application container.
They commonly provide proxying, logging, certificate refresh, or synchronization functions.

#### ephemeral containers

Ephemeral containers are temporary debugging containers injected into an existing Pod.
They are not restartable and are intended for troubleshooting rather than regular application design.

**Commands**

```sh
kubectl debug -it pod/demo --image=busybox:1.36
```

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-demo
spec:
  initContainers:
    - name: init
      image: busybox:1.36
      command: ["sh", "-c", "echo preparing; sleep 2"]
  containers:
    - name: app
      image: nginx:1.27
```

### 3.9 Probes

Probes allow kubelet to evaluate container health and readiness.
They directly affect container restarts and Service endpoint inclusion.

#### livenessProbe

A liveness probe determines whether a container should be restarted because it is no longer functioning correctly.
It detects deadlock or unrecoverable runtime failure rather than mere startup delay.

#### readinessProbe

A readiness probe determines whether a container is ready to receive traffic.
If readiness fails, the Pod is removed from most Service endpoint sets without necessarily being restarted.

#### startupProbe

A startup probe protects slow-starting workloads from premature liveness failure.
While startup probe failures continue, liveness and readiness checks are suppressed for that container.

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: probe-demo
spec:
  containers:
    - name: app
      image: nginx:1.27
      ports:
        - containerPort: 80
      startupProbe:
        httpGet:
          path: /
          port: 80
        failureThreshold: 30
        periodSeconds: 5
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 3
        periodSeconds: 5
      livenessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 10
        periodSeconds: 10
```

### 3.10 Static Pods

Static Pods are Pods managed directly by the kubelet from files on the node filesystem.
They are useful for bootstrapping control-plane components but are not managed by API-level controllers in the normal way.

---

## 4. Scheduling and Resource Management

Scheduling places Pods onto Nodes based on constraints, preferences, and resource availability.
Resource management converts declared CPU, memory, and storage intent into placement, runtime isolation, QoS, and eviction behavior.

### 4.1 Scheduling Pipeline

The scheduler runs a multi-stage placement pipeline.
It first filters out infeasible nodes, then scores feasible nodes, and finally binds the Pod to the selected node.

#### filtering

Filtering removes nodes that fail hard constraints such as insufficient resources, taints, affinity requirements, or incompatible volumes.
A Pod remains Pending if no feasible node survives filtering.

#### scoring

Scoring ranks feasible nodes according to soft preferences such as spreading, affinity weights, and resource balancing.
The highest-ranked node is typically chosen for binding.

### 4.2 Requests and Limits

Requests are scheduling inputs and limits are runtime ceilings.
They directly influence node placement, CPU throttling, memory OOM behavior, and QoS class assignment.

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resources-demo
spec:
  containers:
    - name: app
      image: nginx:1.27
      resources:
        requests:
          cpu: "250m"
          memory: "128Mi"
        limits:
          cpu: "500m"
          memory: "256Mi"
```

### 4.3 QoS Classes

QoS classes determine relative eviction priority during node resource pressure.
They are derived from how CPU and memory requests and limits are declared.

#### Guaranteed

Guaranteed Pods have equal CPU and memory request and limit values on every container.
They receive the strongest protection from eviction under resource pressure.

#### Burstable

Burstable Pods have at least one request set but do not meet the Guaranteed criteria.
They are evicted after BestEffort but before Guaranteed in many pressure scenarios.

#### BestEffort

BestEffort Pods have no CPU or memory requests or limits.
They are the first eviction candidates under node pressure.

### 4.4 Affinity and Anti-Affinity

Affinity rules influence placement relative to node labels or other Pods.
They support both hard requirements and soft preferences for topology-aware scheduling.

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-demo
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: disk
                operator: In
                values: ["ssd"]
    podAntiAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
        - weight: 100
          podAffinityTerm:
            labelSelector:
              matchLabels:
                app: web
            topologyKey: kubernetes.io/hostname
  containers:
    - name: app
      image: nginx:1.27
```

### 4.5 Taints and Tolerations

Taints repel Pods from nodes unless the Pods explicitly tolerate those taints.
They are commonly used for dedicated hardware, infra nodes, and selective workload isolation.

**Commands**

```sh
kubectl taint nodes node1 dedicated=gpu:NoSchedule
kubectl describe node node1 | grep -i taint
```

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: toleration-demo
spec:
  tolerations:
    - key: "dedicated"
      operator: "Equal"
      value: "gpu"
      effect: "NoSchedule"
  containers:
    - name: app
      image: nginx:1.27
```

### 4.6 Topology Spread Constraints

Topology spread constraints limit skew across nodes, zones, or other topology domains.
They improve resilience by preventing excessive concentration of replicas in a single failure domain.

**YAML**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spread-demo
spec:
  replicas: 4
  selector:
    matchLabels:
      app: spread-demo
  template:
    metadata:
      labels:
        app: spread-demo
    spec:
      topologySpreadConstraints:
        - maxSkew: 1
          topologyKey: topology.kubernetes.io/zone
          whenUnsatisfiable: DoNotSchedule
          labelSelector:
            matchLabels:
              app: spread-demo
      containers:
        - name: app
          image: nginx:1.27
```

### 4.7 Priority and Preemption

Priority classes assign relative importance to Pods under contention.
Preemption may evict lower-priority Pods if doing so is required to schedule a higher-priority Pod.

**YAML**

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 100000
globalDefault: false
description: "High priority workload"
---
apiVersion: v1
kind: Pod
metadata:
  name: priority-demo
spec:
  priorityClassName: high-priority
  containers:
    - name: app
      image: nginx:1.27
```

### 4.8 ResourceQuota and LimitRange

ResourceQuota limits aggregate resource and object consumption in a namespace.
LimitRange sets default, minimum, or maximum per-object resource settings within that namespace.

**YAML**

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: demo-quota
  namespace: demo
spec:
  hard:
    requests.cpu: "2"
    requests.memory: 2Gi
    limits.cpu: "4"
    limits.memory: 4Gi
    pods: "10"
---
apiVersion: v1
kind: LimitRange
metadata:
  name: demo-limits
  namespace: demo
spec:
  limits:
    - type: Container
      default:
        cpu: "500m"
        memory: "256Mi"
      defaultRequest:
        cpu: "100m"
        memory: "128Mi"
```

### 4.9 Eviction and Node Pressure

Eviction is the removal of Pods due to policy or resource pressure rather than normal application exit.
Node pressure signals include memory, disk, PID, and other condition thresholds evaluated by kubelet.

#### eviction signals

Eviction signals are measurements such as low available memory, disk pressure, or inode exhaustion.
When thresholds are crossed, kubelet may evict lower-priority Pods to preserve node stability.

#### graceful node shutdown

Graceful node shutdown allows the kubelet to terminate Pods more predictably during host power-off or reboot.
It reduces abrupt disruption when supported and configured on the node OS.

---

## 5. Networking

Kubernetes networking provides flat Pod addressing, Service virtual IPs, and policy-controlled traffic paths.
It relies on pluggable implementations for interface setup, routing, service forwarding, and optional security enforcement.

### 5.1 Pod Networking Model

Every Pod receives its own routable IP address in the Kubernetes network model.
All containers in the Pod share that IP and communicate with each other over localhost inside the shared network namespace.

#### flat address space

The model assumes Pods can reach each other directly without requiring per-hop NAT between Pod IPs.
The exact implementation varies by CNI, but the API contract assumes direct Pod-level addressing.

#### Pod IP lifecycle

A Pod IP is ephemeral and tied to a specific Pod instance.
Recreated Pods usually receive new UIDs and may receive different IPs even if names are reused.

### 5.2 Service

A Service provides a stable virtual endpoint over a dynamic set of backend Pods.
It decouples clients from Pod IP churn and supports traffic distribution to healthy backends.

**Commands**

```sh
kubectl expose deployment web --port=80 --target-port=80 --type=ClusterIP
kubectl get svc
kubectl describe svc web
```

**YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web
spec:
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
```

### 5.3 Service Types

Service types define how clients reach the Service abstraction.
They alter exposure semantics while preserving the same logical backend model.

#### ClusterIP

ClusterIP is the default internal-only Service type.
It assigns a stable virtual IP reachable from within the cluster network.

#### NodePort

NodePort opens the same port on every node and forwards traffic to the Service.
It extends ClusterIP exposure to node-level ingress and often underpins external load balancer integration.

#### LoadBalancer

LoadBalancer asks an external integration to provision a load balancer for the Service.
In cloud environments this usually maps to provider-native L4 load balancing resources.

#### ExternalName

ExternalName returns a DNS CNAME to an external name rather than proxying traffic.
It performs name indirection only and does not create endpoints or a virtual IP.

#### headless Service

A headless Service has no cluster virtual IP.
It returns backend addresses directly through DNS and is commonly used with StatefulSets or client-side discovery.

**YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nodeport-web
spec:
  type: NodePort
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

### 5.4 EndpointSlice and Endpoints

Service backends are represented by endpoint resources rather than by the Service object itself.
Modern Kubernetes uses EndpointSlice as the scalable representation for Service backends.

#### EndpointSlice

EndpointSlice stores backend addresses, ports, conditions, and related metadata in a scalable sharded form.
It replaces the older Endpoints representation for most internal controller logic.

#### Endpoints

Endpoints is the legacy monolithic backend representation for a Service.
It remains for compatibility but scales less effectively in large clusters.

**Commands**

```sh
kubectl get endpointslices
kubectl describe endpointslice -l kubernetes.io/service-name=web
kubectl get endpoints web
```

### 5.5 DNS and Service Discovery

CoreDNS is the standard in-cluster DNS server for Kubernetes.
It resolves Service names and, in some cases, Pod-related records into reachable endpoints.

#### cluster domain

The cluster domain is the DNS suffix used for in-cluster resolution.
A common fully qualified Service name is `<service>.<namespace>.svc.cluster.local`.

#### DNS policy

A Pod DNS policy determines how DNS settings are injected into the Pod.
Common policies include `ClusterFirst`, `Default`, and specialized host-network-aware variants.

**Commands**

```sh
kubectl get pods -n kube-system -l k8s-app=kube-dns
kubectl run -it --rm dns-test --image=busybox:1.36 --restart=Never -- nslookup web.default.svc.cluster.local
```

### 5.6 Ingress

Ingress is the Kubernetes API for HTTP and HTTPS request routing to Services.
It defines desired routing rules but depends on an Ingress controller to implement the actual proxy data plane.

#### IngressClass

IngressClass binds an Ingress resource to a specific controller implementation.
It is the standardized mechanism for controller selection.

> Note: Ingress remains widely used, but the API is relatively frozen compared with the newer Gateway API model.

**YAML**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web
spec:
  ingressClassName: nginx
  rules:
    - host: web.example.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web
                port:
                  number: 80
```

### 5.7 Gateway API

Gateway API is the newer Kubernetes traffic-management API family for role-oriented exposure.
It separates infrastructure ownership from route ownership more cleanly than the Ingress model.

#### GatewayClass

GatewayClass defines a type of gateway implementation managed by a controller.
It plays a role similar to an infrastructure class for network entry points.

#### Gateway

Gateway defines listener configuration and infrastructure attachment points.
It represents the actual entry boundary managed by a gateway controller.

#### HTTPRoute

HTTPRoute defines HTTP matching and backend forwarding rules.
It attaches to a Gateway and expresses route ownership separately from infrastructure ownership.

**YAML**

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: edge
spec:
  gatewayClassName: example-gw
  listeners:
    - name: http
      protocol: HTTP
      port: 80
---
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: web
spec:
  parentRefs:
    - name: edge
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /
      backendRefs:
        - name: web
          port: 80
```

### 5.8 NetworkPolicy

NetworkPolicy is the API object for allowlist-based Pod traffic control.
It is enforced by the CNI implementation rather than by Kubernetes core components.

#### additive allow model

NetworkPolicy is additive and allow-oriented rather than an ordered firewall rule list.
Once a Pod is selected by a policy for a direction, only explicitly allowed traffic is permitted for that direction.

#### ingress and egress isolation

Ingress rules constrain who may connect to selected Pods.
Egress rules constrain where selected Pods may connect.

**YAML**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web
spec:
  podSelector:
    matchLabels:
      app: web
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              access: "true"
```

**Commands**

```sh
kubectl get networkpolicy
kubectl describe networkpolicy allow-web
```

### 5.9 kube-proxy and Data Plane Variants

kube-proxy implements Service forwarding through packet-handling rules on nodes.
Some clusters replace or augment kube-proxy behavior with eBPF-based data planes, but the Service API contract remains the same.

#### iptables mode

iptables mode programs chains of NAT and filter rules in the Linux kernel.
It is widely supported and commonly used.

#### IPVS mode

IPVS mode uses the Linux IP virtual server subsystem for Service forwarding.
It scales more efficiently for large numbers of Services and backends.

---

## 6. Storage

Kubernetes storage decouples workload lifecycle from data lifecycle.
It uses claims, classes, and CSI drivers to provision, attach, mount, snapshot, and expand storage.

### 6.1 Volume Model

Volumes are Pod-level storage sources that containers mount into their filesystems.
They may be ephemeral or persistent depending on source type and backend behavior.

### 6.2 PersistentVolume and PersistentVolumeClaim

PersistentVolume represents provisioned storage supply.
PersistentVolumeClaim represents workload demand for storage with specific access and capacity characteristics.

**Commands**

```sh
kubectl get pv,pvc
kubectl describe pvc data
kubectl get pvc -A
```

**YAML**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard
```

### 6.3 StorageClass

A StorageClass defines how dynamic provisioning should create storage.
It abstracts backend-specific provisioners and parameters behind a named class.

#### volumeBindingMode

`WaitForFirstConsumer` delays provisioning or binding until a Pod using the claim is scheduled.
This helps storage provisioning align with node or zone topology constraints.

**Commands**

```sh
kubectl get storageclass
kubectl describe storageclass standard
```

**YAML**

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: csi.example.com
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion: true
parameters:
  type: ssd
```

### 6.4 Access Modes and Volume Mode

Access modes define how a volume may be mounted by nodes or Pods.
Volume mode defines whether a volume is presented as a filesystem or a raw block device.

#### ReadWriteOnce

ReadWriteOnce allows a volume to be mounted read-write by one node at a time.
It is the most common block-storage access mode.

#### ReadOnlyMany

ReadOnlyMany allows read-only mount by multiple nodes.
It is useful for shared immutable content.

#### ReadWriteMany

ReadWriteMany allows read-write mount by multiple nodes.
It requires a backend that supports shared writable access.

### 6.5 CSI

CSI decouples storage-driver development from Kubernetes core releases.
It supports provisioning, attachment, mount, resize, and snapshot workflows through standard interfaces.

#### CSI controller plugin

The controller plugin handles cluster-scoped storage actions such as create, delete, attach orchestration, and resize coordination.
It usually runs as a controller Deployment.

#### CSI node plugin

The node plugin performs node-local storage actions such as staging, mounting, and unmounting.
It usually runs as a DaemonSet on nodes.

### 6.6 VolumeSnapshot

VolumeSnapshot represents a point-in-time snapshot request for a CSI-backed volume.
It requires snapshot CRDs, a snapshot controller, and a compatible CSI driver.

**YAML**

```yaml
apiVersion: snapshot.storage.k8s.io/v1
kind: VolumeSnapshot
metadata:
  name: data-snap
spec:
  volumeSnapshotClassName: csi-snapclass
  source:
    persistentVolumeClaimName: data
```

### 6.7 Common Volume Types

Built-in volume types are used for scratch space, configuration projection, and selected host access patterns.
Their lifecycle and security properties differ significantly.

#### emptyDir

`emptyDir` is created when a Pod is assigned to a node and removed when that Pod is deleted from the node.
It is suitable for scratch space, caches, and intra-Pod shared temporary storage.

#### configMap volume

A ConfigMap volume projects configuration data as files.
Mounted content may update over time as kubelet refreshes the projected data.

#### secret volume

A Secret volume projects secret data as files.
It is usually backed by in-memory storage on the node to reduce persistence exposure.

#### projected volume

A projected volume combines multiple projection sources into a single mount.
It can include ConfigMaps, Secrets, service account tokens, and downward API fields.

#### hostPath

`hostPath` mounts a path from the node filesystem directly into the Pod.
It weakens isolation and portability and is often restricted by policy.

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pvc-demo
spec:
  containers:
    - name: app
      image: nginx:1.27
      volumeMounts:
        - name: data
          mountPath: /usr/share/nginx/html
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: data
```

---

## 7. Configuration and Secrets

Kubernetes separates application code from runtime configuration through dedicated API objects and projection mechanisms.
This allows environment-specific behavior, secret scoping, and controlled updates without rebuilding images.

### 7.1 ConfigMap

A ConfigMap stores non-sensitive configuration data as key-value pairs.
It can be consumed through environment variables, command arguments, or mounted files.

**Commands**

```sh
kubectl create configmap app-config --from-literal=MODE=prod
kubectl get configmap app-config -o yaml
```

**YAML**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  MODE: "prod"
  LOG_LEVEL: "info"
```

### 7.2 Secret

A Secret stores sensitive data in the API, typically encoded in manifest form.
Practical confidentiality requires RBAC restriction, encryption at rest, careful projection, and often external secret-management integration.

**Commands**

```sh
kubectl create secret generic app-secret --from-literal=password=s3cr3t
kubectl get secret app-secret -o yaml
```

**YAML**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
stringData:
  username: admin
  password: s3cr3t
```

### 7.3 ServiceAccount

A ServiceAccount is the Kubernetes identity object used by Pods and automation inside the cluster.
Pods authenticate to the API using tokens associated with their bound ServiceAccount.

**Commands**

```sh
kubectl create serviceaccount app-sa
kubectl get sa
```

**YAML**

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app-sa
```

### 7.4 Downward API

The downward API exposes selected Pod and container metadata to running workloads.
It lets applications discover limited information about themselves without direct API queries.

---

## 8. Security

Kubernetes security spans API authentication, authorization, admission, workload isolation, network segmentation, and auditability.
A secure platform requires layered controls because no single mechanism provides complete tenant or workload isolation.

### 8.1 Authentication and Authorization

Authentication identifies the caller and authorization decides whether the request is allowed.
These checks occur before admission and before the API object is persisted.

### 8.2 RBAC

RBAC is the standard authorization model for Kubernetes API access.
It binds subjects to Roles or ClusterRoles that define allowed verbs over resources and subresources.

**Commands**

```sh
kubectl auth can-i get pods --as=system:serviceaccount:default:app-sa
kubectl auth can-i list secrets --as=system:serviceaccount:default:app-sa -n default
kubectl get role,rolebinding,clusterrole,clusterrolebinding
```

**YAML**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: default
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
  - kind: ServiceAccount
    name: app-sa
    namespace: default
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-reader
```

### 8.3 Admission Control

Admission control is the policy and mutation stage executed after authentication and authorization but before persistence.
It can mutate, validate, or reject requests through built-in plugins or external webhooks.

#### mutating admission

Mutating admission changes objects before they are stored.
It is commonly used for default injection, sidecars, labels, and policy-based mutation.

#### validating admission

Validating admission approves or rejects requests without mutating them.
It is commonly used for policy enforcement and invariants.

### 8.4 Pod Security Admission

Pod Security Admission enforces Pod Security Standards at namespace level.
It uses namespace labels to set `enforce`, `audit`, and `warn` behavior.

**Commands**

```sh
kubectl label ns demo pod-security.kubernetes.io/enforce=restricted
kubectl label ns demo pod-security.kubernetes.io/audit=restricted
kubectl label ns demo pod-security.kubernetes.io/warn=restricted
```

### 8.5 Security Context

Security context controls privilege and runtime behavior for Pods and containers.
It shapes Linux UID/GID behavior, privilege escalation, filesystem mutability, and capability exposure.

**YAML**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
spec:
  securityContext:
    fsGroup: 2000
  containers:
    - name: app
      image: nginx:1.27
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        allowPrivilegeEscalation: false
        readOnlyRootFilesystem: true
        capabilities:
          drop: ["ALL"]
        seccompProfile:
          type: RuntimeDefault
```

### 8.6 seccomp, AppArmor, SELinux, and Capabilities

Kernel-level isolation features reduce what a compromised container can do.
Their exact behavior depends on node OS, runtime support, and local policy configuration.

#### seccomp

seccomp restricts available Linux syscalls to the container.
`RuntimeDefault` is a common baseline that reduces syscall surface while preserving normal application behavior.

#### AppArmor

AppArmor applies profile-based process confinement on supported Linux nodes.
It constrains access to files, capabilities, and selected operations through kernel enforcement.

#### SELinux

SELinux uses label-based mandatory access control for processes and files.
It can provide strong isolation properties on compatible distributions when correctly configured.

#### capabilities

Linux capabilities are fine-grained kernel privileges that can be added or dropped independently.
Dropping unnecessary capabilities is safer than relying on broad root privilege.

### 8.7 Node and API Hardening Notes

Security depends not only on workload manifests but also on control-plane and node configuration.
Important hardening areas include encryption at rest, certificate rotation, audit policy, restricted host access, and least-privilege RBAC.

#### encryption at rest

Encryption at rest protects persisted API objects such as Secrets in etcd.
It must be explicitly configured and does not replace RBAC or runtime controls.

#### certificate rotation

Certificate rotation reduces long-lived credential exposure.
Kubelet and cluster certificate workflows should be managed so expired credentials do not break operations.

#### NodeRestriction and least privilege

NodeRestriction limits which node identities may mutate certain API objects.
It reduces the blast radius if a kubelet identity is compromised.

---

## 9. Observability and Operations

Operations depend on visibility into object state, node health, workload behavior, and control-plane activity.
Kubernetes exposes object status and basic signals, while full monitoring, logging, and tracing usually rely on add-ons.

### 9.1 Metrics

Metrics quantify resource usage and system behavior over time.
They support autoscaling, alerting, troubleshooting, and capacity planning.

#### metrics-server

metrics-server provides resource metrics such as node and Pod CPU and memory usage.
It supports lightweight operational visibility and autoscaling inputs rather than full historical observability.

#### kube-state-metrics

kube-state-metrics exports Kubernetes object state as metrics.
It is not part of Kubernetes core and should be understood as an add-on rather than a control-plane component.

**Commands**

```sh
kubectl top nodes
kubectl top pods -A
```

### 9.2 Logs

Kubernetes expects containers to write logs to stdout and stderr.
Cluster-wide retention, search, and aggregation are usually provided by external logging stacks.

**Commands**

```sh
kubectl logs deploy/web
kubectl logs deploy/web -c nginx
kubectl logs -f pod/demo
kubectl logs job/pi
kubectl logs pod/demo --previous
```

### 9.3 Events and Inspection

Events are short-lived records of notable state transitions or failures.
They are extremely useful for troubleshooting but should not be treated as a durable audit or metrics store.

**Commands**

```sh
kubectl get events -A --sort-by=.lastTimestamp
kubectl get events --field-selector type=Warning -A
kubectl describe pod demo
kubectl describe node node1
kubectl get pods -A -o wide
kubectl get pod demo -o yaml
```

### 9.4 Node Operations

Administrative node operations are used for maintenance, upgrades, and troubleshooting.
They affect scheduling eligibility and may trigger controlled Pod eviction.

**Commands**

```sh
kubectl cordon node1
kubectl drain node1 --ignore-daemonsets --delete-emptydir-data
kubectl uncordon node1
```

### 9.5 Rollout Operations

Rollout operations track and control Deployment changes.
They are central to safe updates, rollback, and release verification.

**Commands**

```sh
kubectl rollout status deploy/web
kubectl rollout history deploy/web
kubectl rollout undo deploy/web
kubectl rollout restart deploy/web
```

### 9.6 Common Failure States

Certain states recur frequently during troubleshooting and should be recognized quickly.
Their causes often involve scheduling constraints, image issues, storage dependencies, or probe failures.

#### Pending

Pending means the Pod has been accepted by the API but is not yet running.
Common causes include unschedulable placement, missing volumes, insufficient resources, or admission constraints.

#### CrashLoopBackOff

CrashLoopBackOff means a container repeatedly starts and exits unsuccessfully.
Common causes include configuration errors, missing dependencies, bad commands, or failing startup logic.

#### ImagePullBackOff

ImagePullBackOff means the kubelet failed to pull the container image and is backing off retries.
Common causes include wrong image references, registry authentication errors, unreachability, or rate limiting.

#### NotReady node

A NotReady node is not healthy enough for normal scheduling or operation.
Common causes include kubelet failure, network partition, runtime issues, or resource pressure conditions.

---

## 10. Service Discovery and Traffic Exposure

Kubernetes provides multiple layers for exposing services, from internal DNS to external HTTP routing.
Traffic can be exposed through Services, Ingress, Gateway API, or provider-specific infrastructure integration.

### 10.1 Internal Discovery

Internal service discovery is usually DNS-based.
Clients resolve stable Service names rather than Pod IPs directly.

### 10.2 External Exposure Patterns

External exposure depends on cluster environment and controller implementation.
Common patterns include NodePort, cloud LoadBalancer Services, Ingress controllers, and Gateway API gateways.

### 10.3 externalTrafficPolicy

`externalTrafficPolicy` affects how externally sourced traffic is forwarded.
When set to `Local`, it can preserve client source IP by avoiding cross-node forwarding to remote backends.

---

## 11. Packaging, Policy, and Extensibility

Platform teams often layer packaging, policy, and extensibility mechanisms on top of raw manifests.
These tools standardize deployment, validation, dependency handling, and domain-specific automation.

### 11.1 Helm

Helm is the package manager for templated Kubernetes application delivery.
It renders charts into manifests and manages them as named release revisions.

**Commands**

```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install web bitnami/nginx
helm list
helm upgrade web bitnami/nginx
helm rollback web 1
```

### 11.2 CRDs

CustomResourceDefinitions extend the Kubernetes API with new resource types.
They define schemas, versions, validation behavior, and storage for custom objects.

**Commands**

```sh
kubectl get crd
kubectl api-resources | grep -i example
```

### 11.3 Operators

Operators combine CRDs with custom controllers that encode domain-specific operational knowledge.
They automate tasks such as provisioning, backup, failover, upgrade, and recovery through reconciliation loops.

### 11.4 API Aggregation

API aggregation allows external API servers to appear under the Kubernetes API surface.
It differs from CRDs because the implementation is an independent API server rather than an API extension stored directly by the main apiserver.

### 11.5 Admission Webhooks

Admission webhooks are external HTTP callbacks that mutate or validate API requests.
They extend built-in policy behavior but introduce latency, availability, and operational dependency considerations.

---

## 12. Autoscaling and Runtime Extensions

Autoscaling and runtime extensions shape how workloads respond to load and how Pods are executed.
They are critical for production-grade elasticity and specialized hardware usage.

### 12.1 HorizontalPodAutoscaler

HPA scales the number of Pod replicas based on metrics such as CPU, memory, or custom signals.
It changes workload replica counts rather than changing per-Pod resource requests.

### 12.2 VerticalPodAutoscaler

VPA adjusts Pod resource requests and sometimes limits based on observed usage patterns.
It changes per-Pod sizing rather than directly changing replica count.

### 12.3 Cluster Autoscaler Boundary

Cluster Autoscaler changes the number of nodes in the cluster to satisfy scheduling demand.
It is distinct from HPA and VPA because it acts on infrastructure capacity rather than only workload declarations.

### 12.4 RuntimeClass

RuntimeClass selects a container runtime configuration for a Pod.
It is used for specialized runtimes such as sandboxed containers or alternative isolation mechanisms.

### 12.5 Device Plugins

Device plugins advertise specialized hardware such as GPUs to Kubernetes.
They expose schedulable extended resources and coordinate access to those devices from Pods.

---

## 13. Certificates, Leases, and Node Heartbeats

Kubernetes relies on short-lived status signaling and credential workflows to maintain node liveness and secure communication.
These mechanisms are foundational for control-plane correctness and cluster trust.

### 13.1 CertificateSigningRequest

CertificateSigningRequest is the API resource used to request and approve X.509 certificates through Kubernetes-managed workflows.
It is used in selected bootstrap and rotation flows, especially for kubelet certificates.

### 13.2 Lease objects

Lease objects are lightweight coordination resources used for leader election and node heartbeat reporting.
They reduce write pressure compared with updating heavier status objects for the same signaling purpose.

### 13.3 Node heartbeats

Node heartbeats signal continued node liveness to the control plane.
If heartbeats stop, the node may be marked NotReady and its workloads may later be rescheduled elsewhere depending on policy and timing.

---

## 14. Multi-Tenancy and Security Boundaries

Kubernetes is not a hard multi-tenant hypervisor boundary by default.
Safer shared usage requires combining namespaces, RBAC, quotas, network policy, admission controls, and sometimes dedicated nodes or clusters.

### 14.1 Soft multi-tenancy

Soft multi-tenancy assumes some level of trust between teams sharing a cluster.
It uses policy and operational guardrails rather than strong isolation alone.

### 14.2 Stronger isolation patterns

Stronger isolation patterns include dedicated node pools, restricted runtime settings, stricter admission, and separate clusters for high-risk tenants.
These reduce blast radius but increase operational overhead.

---

## 15. Practical Troubleshooting Commands

```sh
kubectl get all -A
kubectl get pods -A -o wide
kubectl get pod <pod-name> -o yaml
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs <pod-name> -c <container-name>
kubectl logs <pod-name> --previous
kubectl exec -it <pod-name> -- sh
kubectl get events -A --sort-by=.lastTimestamp
kubectl get events --field-selector type=Warning -A
kubectl top pods -A
kubectl top nodes
kubectl get endpointslices
kubectl get pvc,pv
kubectl describe pvc <pvc-name>
kubectl auth can-i --list
kubectl auth can-i get secrets --as=system:serviceaccount:default:app-sa -n default
kubectl api-resources
kubectl explain <resource>
```

## 16. Study Framing

These notes are optimized for certification review, platform engineering study, architecture analysis, and production operations reference.
They intentionally distinguish API objects, controllers, control-plane components, node agents, plugin contracts, policy objects, runtime behavior, and operational failure modes.
