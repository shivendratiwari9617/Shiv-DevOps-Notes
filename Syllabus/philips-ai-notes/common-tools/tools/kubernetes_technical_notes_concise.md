# Kubernetes Technical Notes — Platform, Architecture, and Administration

This file is a **concise technical-notes document** for Kubernetes platform understanding and administration.

It is designed to cover:

- cluster architecture
- workloads
- networking
- storage
- security
- operations
- practical examples
- interview-ready explanations

---

# 1) What Kubernetes is

Kubernetes is a **container orchestration platform** used to deploy, manage, scale, and operate containerized applications.

It helps teams:
- schedule workloads
- manage desired state
- scale applications
- handle service discovery
- support rolling updates
- manage storage and networking
- enforce workload isolation and policy

A useful mental model is:

1. developers define desired state
2. Kubernetes control plane stores and evaluates that state
3. worker nodes run the workloads
4. controllers continuously try to make actual state match desired state

---

# 2) Cluster architecture

A Kubernetes cluster usually consists of:

- **control plane**
- **worker nodes**
- **pods**
- **cluster networking**
- **storage integrations**
- **API-driven controllers**

---

## 2.1 Control plane components

The control plane manages the cluster.

Core control plane components include:
- **kube-apiserver**
- **etcd**
- **kube-scheduler**
- **kube-controller-manager**
- **cloud-controller-manager** in cloud-integrated environments

### kube-apiserver
The API server is the front door of the cluster.  
All cluster state changes go through it.

### etcd
`etcd` stores the cluster’s key-value state.

### kube-scheduler
The scheduler decides which node should run a newly created pod.

### scheduling as policy
In practice, scheduling is not only placement.  
It becomes a policy layer driven by:
- requests and limits
- affinity rules
- taints and tolerations
- topology constraints
- availability goals
- workload isolation requirements

A strong architect should treat scheduling as part of platform design, not only cluster mechanics.


### kube-controller-manager
This runs controllers that reconcile cluster state, such as:
- deployment controller
- node controller
- job controller
- replica set controller

### cloud-controller-manager
This integrates Kubernetes with cloud provider behavior where applicable.

---

## 2.2 Worker node components

## 2.2.1 Node lifecycle thinking

Worker-node operations often include:
- join and bootstrap
- cordon and drain
- patching and upgrade
- replacement of failed nodes
- capacity balancing

Platform administration is easier when node lifecycle is treated as a standard operating workflow rather than an ad hoc event.

---


Worker nodes run application workloads.

Common node components:
- **kubelet**
- **container runtime**
- **kube-proxy** in many networking models

### kubelet
The kubelet is the node agent that ensures pods defined for the node are running.

### container runtime
This runs the containers.

### kube-proxy
This helps implement service networking behavior depending on cluster mode.

---

## 2.3 Declarative model and reconciliation

Kubernetes is declarative.

That means you usually define:
- what should run
- how many replicas should exist
- what configuration it needs
- what resources it can use

Controllers then reconcile the system toward that desired state.

This is one of the most important Kubernetes ideas.

---

# 3) Core API objects

Important Kubernetes objects include:

- `Pod`
- `Deployment`
- `ReplicaSet`
- `StatefulSet`
- `DaemonSet`
- `Job`
- `CronJob`
- `Service`
- `Ingress`
- `ConfigMap`
- `Secret`
- `PersistentVolumeClaim`
- `Namespace`

---

## 3.1 Pod

A pod is the smallest deployable unit in Kubernetes.

A pod can contain:
- one main container
- one or more sidecar or helper containers
- shared network namespace
- shared storage volumes

Pods are usually not managed directly in production unless for debugging or one-off tasks.

---

## 3.2 Deployment

A Deployment manages stateless application rollout and replica count.

It is commonly used for:
- web services
- APIs
- stateless backend services
- rolling updates

---

## 3.3 StatefulSet

A StatefulSet is used for stateful applications needing:
- stable identity
- stable network naming
- ordered startup or shutdown
- persistent storage patterns

Examples:
- databases
- clustered stateful services

---

## 3.4 DaemonSet

A DaemonSet ensures one pod runs on each matching node.

Common uses:
- logging agents
- monitoring agents
- node security agents
- networking components

---

## 3.5 Job and CronJob

A Job runs tasks to completion.

A CronJob runs jobs on a schedule.

Examples:
- backups
- data sync
- cleanup tasks
- report generation

---

# 4) Workloads and scaling

## 4.1 Replica management

Replica-based workloads allow Kubernetes to maintain availability.

Example:
- if one pod crashes, the controller creates a replacement
- if desired replicas increase, more pods are scheduled

---

## 4.2 Rolling updates

Deployments support rolling updates.

This allows:
- gradual pod replacement
- reduced downtime
- controlled rollout failure handling

Important rollout ideas:
- max unavailable
- max surge
- readiness checks
- rollback if health degrades

---

## 4.3 Horizontal scaling

Horizontal scaling means adding more pod replicas.

This is commonly used for:
- HTTP services
- stateless APIs
- bursty application traffic

A common pattern is **Horizontal Pod Autoscaler** based on resource or custom metrics.

---

## 4.4 Resource requests and limits

## 4.5 Scheduling controls

Kubernetes scheduling is influenced by more than free CPU and memory.

Important controls include:
- `nodeSelector`
- node affinity
- pod affinity
- pod anti-affinity
- taints
- tolerations
- topology spread constraints

These matter when you need:
- workload isolation
- zone-aware placement
- dedicated nodes
- anti-colocation for resilience
- controlled scheduling of special workloads

---

## 4.6 Taints and tolerations

Taints repel pods from nodes unless the pod has a matching toleration.

This is commonly used for:
- dedicated workloads
- GPU nodes
- infra-only nodes
- maintenance isolation

A strong platform admin should understand that taints control **where pods should not go unless allowed**.

---

## 4.7 Affinity and anti-affinity

Affinity rules influence where pods should run.

Examples:
- place a workload near another workload
- separate replicas across nodes
- prefer certain node labels
- avoid colocation of critical replicas

This is important for resilience and topology-aware design.

---

## 4.8 Probes

Health probes are critical in production.

Important probe types:
- `readinessProbe`
- `livenessProbe`
- `startupProbe`

Mental model:
- **readiness** decides if traffic should reach the pod
- **liveness** decides if Kubernetes should restart the container
- **startup** protects slow-starting applications from premature liveness failure

Misconfigured probes are a common cause of rollout and stability problems.

---


Containers commonly define:
- `requests`
- `limits`

Requests influence scheduling.  
Limits constrain allowed resource consumption.

This is important because Kubernetes scheduling is strongly affected by resource requests.

---

# 5) Networking

Kubernetes networking is one of the most important and often most confusing topics.

## 5.1 Pod networking model

In Kubernetes, pods get their own IP address.  
Pods can generally communicate with each other across nodes without NAT assumptions inside the cluster networking model.

This requires a working **Container Network Interface (CNI)** implementation.

---

## 5.2 Service

A Service provides stable access to a set of pods.

Common service types:
- `ClusterIP`
- `NodePort`
- `LoadBalancer`
- `ExternalName`

### ClusterIP
Internal-only service access.

### NodePort
Exposes the service on a node port.

### LoadBalancer
Usually integrates with cloud load balancing.

### ExternalName
Maps to an external DNS name.

---

## 5.3 Ingress

Ingress provides HTTP/HTTPS routing into cluster services.

It is commonly used for:
- host-based routing
- path-based routing
- TLS termination
- exposing multiple services behind one entry point

Ingress usually requires an ingress controller.

---

## 5.4 DNS

Kubernetes provides internal DNS for service discovery.

Common idea:
- pods call services by DNS name instead of pod IP
- service names remain stable while pods change

---

## 5.5 NetworkPolicy

## 5.6 CNI concept

Kubernetes relies on a **Container Network Interface (CNI)** implementation for pod networking.

Operationally, the CNI affects:
- pod-to-pod connectivity
- network policy enforcement
- IP allocation
- cross-node traffic behavior

A platform architect should understand that Kubernetes defines the networking model, but the CNI implements it.

---

## 5.7 Ingress vs Gateway thinking

## 5.8 Ingress controller awareness

Ingress is only the API object.  
Actual traffic handling depends on an **ingress controller**.

That means successful ingress operations require alignment between:
- Ingress resource definition
- controller deployment
- service exposure
- DNS
- TLS configuration

A common mistake is to create an Ingress resource and assume traffic will work without a functioning controller.

---


Ingress is widely used for HTTP/HTTPS exposure, but modern platform discussions may also include Gateway-style API patterns.

A concise mental model:
- **Ingress** is the traditional Kubernetes HTTP routing abstraction
- newer gateway-oriented models provide more expressive traffic-management patterns

For many admin and interview contexts, understanding Ingress deeply is still essential.

---


NetworkPolicy is used to control allowed network traffic between pods and namespaces, depending on the network plugin and policy support.

This is important for segmentation and least-privilege networking.

---

# 6) Storage

Storage matters because containers are ephemeral, but many applications need persistent data.

## 6.1 Volumes

A volume provides storage to a pod.

Common uses:
- configuration files
- scratch space
- shared data between containers in a pod
- persistent data through external storage integration

---

## 6.2 PersistentVolume and PersistentVolumeClaim

### PersistentVolume (PV)
Represents storage resource in the cluster.

### PersistentVolumeClaim (PVC)
Represents a request for storage by a workload.

This abstraction helps separate:
- storage provisioning
- storage consumption

---

## 6.3 StorageClass

A StorageClass defines how storage is provisioned.

This is important for:
- dynamic provisioning
- different performance classes
- different storage backends

---

## 6.4 Stateful storage pattern

## 6.5 CSI concept

Kubernetes commonly integrates storage through the **Container Storage Interface (CSI)** model.

Operationally, CSI affects:
- dynamic volume provisioning
- attach/mount behavior
- storage-class capabilities
- cloud and platform storage integration

A platform admin should understand that Kubernetes storage abstractions depend on working storage drivers underneath.

---

## 6.6 Backup and restore awareness

## 6.7 Volume snapshot awareness

In many real platforms, backup and recovery workflows also involve snapshot-capable storage integrations.

A strong admin should think about:
- crash-consistent vs application-consistent recovery
- snapshot timing
- restore validation
- storage-class capabilities
- workload recovery dependencies

The key lesson is that “persistent volume exists” does not automatically mean “recovery is ready.”

---


Persistent state requires backup planning.

Important storage-related operational ideas:
- application-consistent backups
- PVC and volume recovery awareness
- restore testing
- stateful workload recovery order
- etcd backup for cluster state protection

Backups matter both for application data and for cluster control-plane state.

---


Stateful workloads commonly use:
- StatefulSet
- PVC templates
- stable identities
- backup-aware operations

State in Kubernetes requires more care than stateless deployment.

---

# 7) Security

Kubernetes security spans cluster, node, workload, identity, and network layers.

## 7.1 Namespaces

Namespaces help logically separate workloads and teams.

They are useful for:
- organization
- scoping
- policy boundaries
- quota application

---

## 7.2 RBAC

Role-Based Access Control controls what users and service accounts can do.

Important objects:
- `Role`
- `ClusterRole`
- `RoleBinding`
- `ClusterRoleBinding`

RBAC is one of the most important administration topics.

---

## 7.3 ServiceAccounts

Pods can run with service accounts to access the Kubernetes API or integrated systems.

Security principle:
Use least privilege and avoid overly broad permissions.

---

## 7.4 Secrets and ConfigMaps

### ConfigMap
Stores non-sensitive configuration.

### Secret
Stores sensitive data in Kubernetes object form.

Important note:
A Secret is not automatically “safe” just because it is named Secret. Real security depends on access control, encryption, and operational handling.

---

## 7.5 Pod security and workload hardening

Important workload hardening ideas:
- run as non-root where possible
- drop unnecessary Linux capabilities
- use read-only root filesystem where appropriate
- restrict privilege escalation
- control host namespace access
- avoid privileged containers unless required

---

## 7.6 Network segmentation

## 7.7 SecurityContext ideas

Security context controls runtime security behavior for pods and containers.

Important ideas include:
- run as non-root
- user and group IDs
- filesystem permissions behavior
- privilege escalation controls
- Linux capability management

This is a very practical workload-hardening topic.

---

## 7.8 Admission control awareness

Admission control is part of cluster governance.

It can be used to:
- validate objects
- mutate objects
- enforce policy
- block risky workload definitions

This matters in enterprise platforms where platform teams want guardrails, not only documentation.

---

## 7.9 ResourceQuota and LimitRange

## 7.10 Control-plane and API security awareness

Kubernetes security is not only about pods.

Platform security also includes:
- API access control
- certificate and trust management awareness
- protection of control-plane components
- etcd protection
- auditability of administrative actions

Even in managed Kubernetes, administrators should understand these concerns conceptually.

---


In multi-tenant clusters, namespaces often use:
- `ResourceQuota`
- `LimitRange`

These help control:
- total resource consumption
- default requests/limits
- fairness between teams
- prevention of uncontrolled namespace growth

This is important in shared platforms.

---


Use NetworkPolicy and namespace boundaries to reduce unnecessary connectivity.

This limits blast radius.

---

# 8) Operations and administration

## 8.1 Common kubectl workflow

A platform admin often uses `kubectl` to:
- inspect resources
- describe failures
- check rollout state
- read logs
- exec into containers
- scale workloads
- apply or delete resources

---

## 8.2 Common commands

Get nodes:

```bash
kubectl get nodes
```

Get pods in all namespaces:

```bash
kubectl get pods -A
```

Describe a pod:

```bash
kubectl describe pod <pod-name> -n <namespace>
```

Read logs:

```bash
kubectl logs <pod-name> -n <namespace>
```

Follow logs:

```bash
kubectl logs -f <pod-name> -n <namespace>
```

Exec into a container:

```bash
kubectl exec -it <pod-name> -n <namespace> -- /bin/sh
```

Check rollout status:

```bash
kubectl rollout status deployment/<deployment-name> -n <namespace>
```

Scale a deployment:

```bash
kubectl scale deployment <deployment-name> --replicas=5 -n <namespace>
```

Apply manifest:

```bash
kubectl apply -f app.yaml
```

Delete manifest resources:

```bash
kubectl delete -f app.yaml
```

---

## 8.3 Troubleshooting workflow

A practical Kubernetes troubleshooting flow:

1. check pod status
2. inspect events
3. describe workload
4. inspect logs
5. verify service endpoints
6. verify resource pressure
7. verify network policy or DNS issues
8. verify storage or scheduling constraints

---

## 8.4 Common failure patterns

### Pod Pending
Possible causes:
- insufficient CPU or memory
- PVC not bound
- node selector mismatch
- taints/tolerations issue

### CrashLoopBackOff
Possible causes:
- app startup failure
- bad config
- secret missing
- dependency unavailable
- command or entrypoint issue

### ImagePullBackOff
Possible causes:
- wrong image name
- registry auth issue
- image tag missing
- network issue

### Service not reachable
Possible causes:
- selector mismatch
- wrong target port
- ingress/controller issue
- network policy block

---

## 8.5 Upgrade and maintenance ideas

## 8.6 PodDisruptionBudget

A PodDisruptionBudget helps protect workload availability during voluntary disruptions such as:
- node drains
- upgrades
- maintenance operations

This is important because planned operations should not accidentally take down too many replicas of a service.

---

## 8.7 Cluster upgrades and control-plane care

Kubernetes platform operations often include:
- version compatibility awareness
- staged upgrade planning
- node-by-node worker upgrades
- addon compatibility checks
- ingress/CNI/CSI compatibility verification

Cluster upgrades are not only about the control plane; ecosystem compatibility matters too.

---

## 8.8 etcd backup awareness

Because `etcd` stores cluster state, protecting it is operationally critical.

A strong platform admin should understand:
- etcd is part of the control-plane recovery story
- backup without restore testing is weak
- etcd loss can severely impact cluster recovery

Even when a managed service abstracts this away, the concept still matters.

---

## 8.9 Multi-tenant governance

Shared Kubernetes platforms need governance in areas such as:
- namespaces
- RBAC
- quotas
- network policies
- workload standards
- approved base images
- secret handling
- auditability

Kubernetes becomes much harder when many teams share one cluster without clear platform rules.

---

## 8.10 Observability and auditability

## 8.11 Managed vs self-managed cluster thinking

A platform architect should distinguish between:
- **managed Kubernetes** responsibilities
- **self-managed Kubernetes** responsibilities

In managed environments, the provider often handles some control-plane operations.  
In self-managed environments, the platform team owns much more of:
- upgrades
- etcd care
- control-plane protection
- certificate handling
- disaster recovery

This distinction matters in architecture and admin interviews.

---

## 8.12 Service mesh awareness

Not every Kubernetes platform uses a service mesh, but architects should know where it fits.

A service mesh is typically considered when teams need:
- advanced service-to-service traffic control
- mTLS between services
- richer traffic policy
- deeper service communication observability

This is not a Kubernetes core object model topic, but it is a common platform-architecture extension.

---


A serious Kubernetes platform should have visibility into:
- node health
- pod health
- events
- logs
- metrics
- storage health
- network issues
- API audit trails

Administration without observability becomes reactive and fragile.

---


Platform administration also includes:
- node draining
- planned upgrades
- backup strategy for critical state
- etcd protection
- workload disruption planning
- observability and audit logging

Example node drain:

```bash
kubectl drain <node-name> --ignore-daemonsets
```

Uncordon after maintenance:

```bash
kubectl uncordon <node-name>
```

---

# 9) Practical YAML examples

## 9.1 Deployment example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-api
  namespace: prod
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-api
  template:
    metadata:
      labels:
        app: web-api
    spec:
      containers:
        - name: web-api
          image: nginx:stable
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

---

## 9.2 Service example

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-api
  namespace: prod
spec:
  selector:
    app: web-api
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP
```

---

## 9.3 ConfigMap example

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: web-config
  namespace: prod
data:
  APP_MODE: "production"
  LOG_LEVEL: "info"
```

---

## 9.4 PersistentVolumeClaim example

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-data
  namespace: prod
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

---

## 9.5 NetworkPolicy example

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-from-api
  namespace: prod
spec:
  podSelector:
    matchLabels:
      app: db
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: web-api
```

---

## 9.6 Readiness and liveness example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api
  namespace: prod
spec:
  replicas: 2
  selector:
    matchLabels:
      app: api
  template:
    metadata:
      labels:
        app: api
    spec:
      containers:
        - name: api
          image: nginx:stable
          ports:
            - containerPort: 80
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
          livenessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 15
            periodSeconds: 20
```

---

## 9.7 ResourceQuota example

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: team-a-quota
  namespace: team-a
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
    pods: "30"
```

---

## 9.8 RBAC example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: prod
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader-binding
  namespace: prod
subjects:
  - kind: ServiceAccount
    name: app-sa
    namespace: prod
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-reader
```

---

## 9.9 PodDisruptionBudget example

## 9.10 Taints and tolerations example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: gpu-workload
spec:
  tolerations:
    - key: "dedicated"
      operator: "Equal"
      value: "gpu"
      effect: "NoSchedule"
  containers:
    - name: gpu-workload
      image: nginx:stable
```

---

## 9.11 Affinity example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-api
  template:
    metadata:
      labels:
        app: web-api
    spec:
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - weight: 100
              podAffinityTerm:
                labelSelector:
                  matchExpressions:
                    - key: app
                      operator: In
                      values:
                        - web-api
                topologyKey: kubernetes.io/hostname
      containers:
        - name: web-api
          image: nginx:stable
```

---

## 9.12 Startup probe example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: slow-start-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: slow-start-app
  template:
    metadata:
      labels:
        app: slow-start-app
    spec:
      containers:
        - name: slow-start-app
          image: nginx:stable
          startupProbe:
            httpGet:
              path: /
              port: 80
            failureThreshold: 30
            periodSeconds: 10
```

---


```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: web-api-pdb
  namespace: prod
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: web-api
```

---

# 10) Best practices

## 10.1 Design around namespaces and ownership

Map cluster organization to:
- teams
- environments
- applications
- access boundaries

---

## 10.2 Prefer controllers over standalone pods

Use Deployments, StatefulSets, Jobs, and DaemonSets instead of hand-managed pods.

---

## 10.3 Set requests and limits carefully

Do not leave resource sizing undefined in important workloads.

---

## 10.4 Use readiness and liveness probes wisely

Health checks affect routing and restart behavior.  
Poor probes can create self-inflicted instability.

---

## 10.5 Treat RBAC and secrets seriously

Security mistakes in Kubernetes are often configuration mistakes, not software bugs.

---

## 10.6 Observe the platform

## 10.7 Design for disruption

Assume pods, nodes, and networks can fail.

Good platform design uses:
- multiple replicas
- readiness probes
- anti-affinity where needed
- PodDisruptionBudgets
- backup and restore testing

---

## 10.8 Standardize workload policy

Platform teams should standardize:
- resource defaults
- health-check patterns
- security context expectations
- labeling conventions
- ingress exposure rules
- secret handling rules

This improves reliability and governance.

---

## 10.9 Keep manifests reviewable

## 10.10 Separate platform standards from application freedom

A strong Kubernetes platform gives application teams freedom within guardrails.

Typical platform standards include:
- namespace conventions
- labels and annotations
- security baselines
- ingress exposure rules
- default quotas and limits
- approved storage and network patterns

This is one of the biggest differences between a cluster and a platform.

---

## 10.11 Test recovery, not just deployment

Many teams test rollout but not recovery.

A stronger platform practice includes testing:
- pod restart behavior
- node failure handling
- backup restore
- secret rotation
- upgrade rollback
- network policy impact

Operational maturity is proven more by recovery than by happy-path deployment.

---


Good manifests are:
- explicit
- minimal
- version controlled
- consistent in labeling and naming
- easy to audit

This matters for both operations and security.

---


A Kubernetes platform should have:
- metrics
- logs
- events
- audit visibility
- workload health monitoring

---

# 11) Interview-ready technical explanations

## What is the role of the control plane?

A strong answer:

> The control plane manages cluster state and scheduling. It exposes the API, stores state in etcd, schedules pods, and runs controllers that reconcile actual cluster state toward the desired state defined by users.

---

## What is the difference between a Deployment and a StatefulSet?

A strong answer:

> A Deployment is mainly for stateless replicated applications, while a StatefulSet is for workloads that need stable identity, stable storage association, and ordered behavior. StatefulSets are commonly used for databases and clustered stateful systems.

---

## What is the difference between a Service and an Ingress?

A strong answer:

> A Service provides stable access to a set of pods inside or outside the cluster depending on service type, while an Ingress provides HTTP or HTTPS routing rules for exposing services externally through an ingress controller.

---

## Why are requests and limits important?

A strong answer:

> Requests influence scheduling decisions and represent reserved resource expectations, while limits cap allowed resource usage. They matter for cluster efficiency, fairness, and workload stability.

---

## Why is RBAC important in Kubernetes?

## What is the difference between readiness, liveness, and startup probes?

A strong answer:

> Readiness determines whether a pod should receive traffic, liveness determines whether Kubernetes should restart the container, and startup protects slow-starting applications from being restarted too early by liveness checks.

---

## Why are taints and tolerations important?

A strong answer:

> Taints and tolerations are important because they let platform teams keep certain nodes reserved or restricted unless a workload is explicitly allowed there. This is useful for infra nodes, GPU nodes, or dedicated workloads.

---

## Why are ResourceQuota and LimitRange important in shared clusters?

A strong answer:

> They are important because they prevent one namespace or team from consuming uncontrolled cluster resources and help enforce more consistent workload sizing in multi-tenant platforms.

---

## Why does etcd matter so much?

## What is the difference between a cluster and a platform?

A strong answer:

> A cluster is the Kubernetes infrastructure itself, while a platform includes the operating model built on top of the cluster: access controls, guardrails, networking patterns, storage standards, observability, security policy, and day-2 operational practices.

---

## Why is an ingress controller important?

A strong answer:

> An Ingress resource only defines routing intent. The ingress controller is the component that actually implements that routing, integrates with the underlying network path, and makes the Ingress operational.

---

## Why do managed and self-managed Kubernetes require different admin thinking?

A strong answer:

> Because the responsibility boundary changes. In managed Kubernetes, some control-plane and infrastructure duties are handled by the provider, while in self-managed Kubernetes the platform team owns much more of upgrades, control-plane health, backups, certificates, and disaster recovery.

---


A strong answer:

> etcd stores Kubernetes cluster state. If it is lost or corrupted and there is no tested recovery path, cluster recovery becomes much harder. That is why backup and restore awareness is essential for platform administration.

---


A strong answer:

> RBAC is important because Kubernetes is API-driven and access to the API controls powerful cluster actions. RBAC ensures users and service accounts only have the permissions they actually need.

---

# 12) Final summary

The most important way to understand Kubernetes is this:

- the control plane manages desired state
- worker nodes run workloads and require lifecycle management
- pods are the basic execution unit
- controllers maintain the declared state
- scheduling is shaped by requests, affinity rules, taints, tolerations, and topology decisions
- services and ingress provide connectivity while CNI implements the network model and ingress controllers make edge routing real
- volumes, PVCs, StorageClasses, CSI, and recovery planning shape storage behavior
- RBAC, namespaces, quotas, admission control, security contexts, and policy support security and governance
- operations depend on observability, rollout control, upgrade care, disruption planning, backup awareness, and tested recovery
- platform thinking goes beyond the cluster to include standards, guardrails, ownership, and day-2 operations

If you understand:
- cluster architecture
- control plane and nodes
- workloads and scheduling
- networking, ingress, and CNI concepts
- storage, CSI, and recovery concepts
- security, RBAC, quotas, and control-plane awareness
- kubectl operations
- probes and rollout safety
- disruption control and platform governance
- managed vs self-managed responsibility boundaries
- common failure patterns

then you understand the practical technical core of Kubernetes at a strong architect-and-admin level.
