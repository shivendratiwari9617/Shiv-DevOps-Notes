# Kubernetes Technical Notes — Deep Concept Guide and Commands with Examples

This file is a **deep technical-notes document** for Kubernetes.

It is designed to help you understand:

- what Kubernetes is and why it exists
- the **core architecture**
- the main Kubernetes objects and how they relate to each other
- core concepts in depth, including **Pods, Services, Deployments, Namespaces, and Ingress**
- other essential objects such as ReplicaSets, ConfigMaps, Secrets, Volumes, StatefulSets, DaemonSets, Jobs, and NetworkPolicy
- important `kubectl` commands with examples

This is intentionally written as a **technical understanding file**, not as an incident or troubleshooting note.

---

# 1) What Kubernetes is

Kubernetes is an **orchestration platform for containerized applications**.

At a high level, Kubernetes solves these problems:

- how to run containers across multiple machines
- how to keep applications available if containers or machines fail
- how to scale applications up or down
- how to expose applications over the network
- how to manage configuration, secrets, and storage
- how to roll out updates safely

A simple way to explain Kubernetes is:

> Kubernetes is a distributed control system that continuously works to make the actual state of the cluster match the desired state declared by the user.

That sentence captures its essence.

---

# 2) The Kubernetes mental model

The most important way to understand Kubernetes is to think in terms of:

1. **Desired state**
2. **Controllers**
3. **Control loop**
4. **Cluster objects**
5. **Reconciliation**

You declare what you want, for example:

- 3 replicas of an application
- exposed internally through a Service
- configured through a ConfigMap
- updated using rolling deployment rules

Kubernetes then uses controllers to constantly compare:

- desired state
- actual state

and reconcile differences.

This reconciliation model is one of the most important Kubernetes ideas.

---

# 3) Kubernetes architecture

Kubernetes has two major sides:

- **control plane**
- **worker nodes**

---

## 3.1 Control plane

The control plane is responsible for managing the cluster.

Main control plane components:

- API server
- etcd
- scheduler
- controller manager
- cloud controller manager

### API server
The API server is the central entry point of Kubernetes.

Everything in Kubernetes is managed through the API:
- `kubectl`
- controllers
- schedulers
- operators
- dashboards
- automation systems

All of them ultimately communicate with the API server.

### etcd
`etcd` is the key-value store that persists Kubernetes cluster state.

It stores:
- object definitions
- metadata
- desired state
- current cluster records

It is effectively the persistent storage for Kubernetes control data.

### Scheduler
The scheduler decides **which node** should run a Pod.

It looks at:
- resource requests
- node availability
- taints and tolerations
- affinity/anti-affinity
- topology constraints
- policies

### Controller Manager
The controller manager runs controllers that continuously reconcile cluster objects.

Examples:
- Deployment controller
- ReplicaSet controller
- Node controller
- Job controller
- EndpointSlice controller

### Cloud Controller Manager
This component integrates Kubernetes with cloud providers.

Examples:
- load balancer integration
- node lifecycle integration
- route management
- cloud volume attachment behavior

---

## 3.2 Worker nodes

Worker nodes are the machines that actually run workloads.

Main node components:

- kubelet
- container runtime
- kube-proxy

### kubelet
The kubelet is the agent running on each node.

Its job is to:
- watch for Pod assignments from the API server
- start containers through the runtime
- report node and Pod status
- execute probes
- mount volumes
- keep Pods running as declared

### Container runtime
The runtime runs the containers.

Examples:
- containerd
- CRI-O

Historically Docker was common, but Kubernetes uses the **Container Runtime Interface (CRI)** model.

### kube-proxy
`kube-proxy` handles network forwarding rules for Services.

It helps implement Service networking using mechanisms such as:
- iptables
- IPVS

Its role is to route traffic from Service IPs to backend Pods.

---

# 4) Core Kubernetes objects

Kubernetes is object-based.  
You work with YAML or API objects rather than directly issuing imperative low-level container commands.

The most important objects include:

- Pod
- ReplicaSet
- Deployment
- Service
- Namespace
- ConfigMap
- Secret
- Volume / PersistentVolume / PersistentVolumeClaim
- Ingress
- StatefulSet
- DaemonSet
- Job
- CronJob
- NetworkPolicy

---

# 5) Pod — the smallest deployable unit

## 5.1 What a Pod is

A **Pod** is the smallest deployable unit in Kubernetes.

A Pod usually contains:
- one main container
- optionally sidecar containers
- shared network namespace
- shared storage volumes
- shared Pod lifecycle

A Pod is **not** the same thing as a container.  
A Pod wraps one or more tightly coupled containers.

### Key properties of a Pod
Containers in the same Pod:
- share the same IP address
- share the same port space
- can communicate over `localhost`
- can share volumes

That means a Pod is more like a **runtime boundary** than just “one container.”

---

## 5.2 Pod networking

Each Pod gets its own IP address.

This is a major Kubernetes design point:
- Pods talk to each other directly over IP
- Pods do not need port mapping like Docker bridge-style access
- Services provide stable addressing on top of changing Pods

Important:
Pod IPs are usually **ephemeral**.  
They can change when Pods are recreated.

That is why applications should not depend directly on Pod IPs for stable service discovery.

---

## 5.3 Pod lifecycle

A Pod goes through phases such as:
- Pending
- Running
- Succeeded
- Failed
- Unknown

### Pending
The Pod has been accepted by the cluster but not fully started yet.

### Running
The Pod has been bound to a node and at least one container has started.

### Succeeded
All containers terminated successfully and will not restart.

### Failed
At least one container terminated unsuccessfully and the Pod is not expected to recover in that lifecycle.

### Unknown
The control plane cannot determine the Pod state.

---

## 5.4 Pod restart behavior

Restart behavior is influenced by `restartPolicy`.

Common values:
- `Always`
- `OnFailure`
- `Never`

For normal application Pods managed by Deployments, `Always` is typical.

---

## 5.5 Example Pod manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.27
      ports:
        - containerPort: 80
```

### Explanation
- `apiVersion`: API group/version
- `kind`: object type
- `metadata`: name and labels
- `spec`: desired state
- `containers`: list of containers inside the Pod

---

## 5.6 Why Pods are usually not created directly

In real systems, Pods are often managed by higher-level controllers such as:
- Deployment
- StatefulSet
- DaemonSet
- Job

This is because Pods are ephemeral.  
If a Pod dies and there is no controller managing it, it is simply gone.

---

## 5.7 Important Pod commands

Create from file:

```bash
kubectl apply -f pod.yaml
```

Get Pods:

```bash
kubectl get pods
```

Get wide output:

```bash
kubectl get pods -o wide
```

Describe a Pod:

```bash
kubectl describe pod nginx-pod
```

View logs:

```bash
kubectl logs nginx-pod
```

Execute a shell:

```bash
kubectl exec -it nginx-pod -- /bin/sh
```

Delete a Pod:

```bash
kubectl delete pod nginx-pod
```

---

# 6) ReplicaSet — maintaining a replica count

## 6.1 What a ReplicaSet is

A **ReplicaSet** ensures that a specified number of identical Pod replicas are running.

If one Pod disappears, the ReplicaSet creates another.

### Purpose
Its main job is replica maintenance:
- if too few Pods exist, create more
- if too many exist, remove extras

ReplicaSet uses:
- a Pod template
- a selector
- a replica count

---

## 6.2 Example ReplicaSet manifest

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.27
```

### Important note
In practice, ReplicaSets are usually managed by Deployments rather than manually created.

---

# 7) Deployment — declarative management for stateless apps

## 7.1 What a Deployment is

A **Deployment** is a higher-level controller used for managing stateless applications.

A Deployment manages:
- ReplicaSets
- Pod templates
- rolling updates
- rollbacks
- replica scaling

This is one of the most important Kubernetes objects.

---

## 7.2 Why Deployments matter

Deployments provide safe declarative updates.

Instead of deleting and recreating Pods manually, you define:
- desired number of replicas
- desired Pod template
- rollout strategy

Kubernetes then handles rollout behavior.

---

## 7.3 Deployment hierarchy

A Deployment manages a ReplicaSet.  
The ReplicaSet manages Pods.

So the chain is:

**Deployment -> ReplicaSet -> Pods**

This hierarchy is very important to understand.

---

## 7.4 Example Deployment manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.27
          ports:
            - containerPort: 80
```

---

## 7.5 Rolling updates

Deployments support rolling updates by gradually replacing old Pods with new Pods.

This helps reduce downtime.

Important rollout settings:
- `maxUnavailable`
- `maxSurge`

Example:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

---

## 7.6 Rollbacks

If a rollout is bad, Deployments support rollback.

Commands:

```bash
kubectl rollout status deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment
```

These are very important operational commands.

---

## 7.7 Scaling Deployments

Scale manually:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Or update the `replicas` field declaratively.

---

## 7.8 Important Deployment commands

Get Deployments:

```bash
kubectl get deployments
```

Describe Deployment:

```bash
kubectl describe deployment nginx-deployment
```

Check rollout:

```bash
kubectl rollout status deployment/nginx-deployment
```

View rollout history:

```bash
kubectl rollout history deployment/nginx-deployment
```

Undo rollout:

```bash
kubectl rollout undo deployment/nginx-deployment
```

Update image:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.28
```

---

# 8) Service — stable networking for Pods

## 8.1 What a Service is

A **Service** provides a stable network identity for a set of Pods.

Because Pods are ephemeral and can be recreated with different IPs, Kubernetes needs an abstraction that gives:
- stable DNS name
- stable virtual IP
- load balancing to matching Pods

That abstraction is the Service.

---

## 8.2 How Services work

A Service selects Pods using labels.

Example logic:
- Service selector says `app=nginx`
- all Pods with label `app=nginx` become backend endpoints

This is one of the most important Kubernetes patterns:
**labels + selectors**

---

## 8.3 Types of Services

Main Service types:

- ClusterIP
- NodePort
- LoadBalancer
- ExternalName

### ClusterIP
Default type.  
Exposes the Service internally inside the cluster.

### NodePort
Exposes the Service on a static port on every node.

### LoadBalancer
Requests an external load balancer from the cloud provider or implementation.

### ExternalName
Maps the Service to an external DNS name.

---

## 8.4 Example ClusterIP Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP
```

### Meaning
- `port`: the Service port
- `targetPort`: the Pod container port to send traffic to

---

## 8.5 Service discovery

A Service gets a DNS name, usually like:

```text
nginx-service.default.svc.cluster.local
```

Commonly inside the same namespace, applications can simply use:

```text
nginx-service
```

This makes Services the standard way for internal service discovery.

---

## 8.6 Endpoints and EndpointSlices

A Service does not directly “contain” Pods.  
It points to backend endpoints.

Modern Kubernetes typically uses **EndpointSlices** to represent sets of backend addresses efficiently.

This is important because the Service is the stable abstraction, while EndpointSlices represent actual backend targets.

---

## 8.7 Important Service commands

Get Services:

```bash
kubectl get svc
```

Describe Service:

```bash
kubectl describe svc nginx-service
```

Get endpoints:

```bash
kubectl get endpoints
```

Get EndpointSlices:

```bash
kubectl get endpointslices
```

---

# 9) Namespace — logical separation inside a cluster

## 9.1 What a Namespace is

A **Namespace** is a logical partition inside a Kubernetes cluster.

Namespaces are used to separate:
- teams
- environments
- applications
- policy boundaries
- quotas
- access controls

Important idea:
A Namespace is **not** a full security boundary by itself.  
It is a logical isolation mechanism.

---

## 9.2 Default namespaces

Common namespaces include:

- `default`
- `kube-system`
- `kube-public`
- `kube-node-lease`

### `default`
Default namespace for objects without an explicit namespace.

### `kube-system`
System components usually live here.

---

## 9.3 Why Namespaces matter

Namespaces help organize and scope:
- object names
- RBAC permissions
- network policies
- resource quotas
- default limits

Two objects can have the same name if they are in different namespaces.

---

## 9.4 Example Namespace manifest

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: payments
```

Create objects in that namespace:

```yaml
metadata:
  name: api
  namespace: payments
```

---

## 9.5 Important Namespace commands

Get namespaces:

```bash
kubectl get namespaces
```

Create namespace:

```bash
kubectl create namespace payments
```

Run command in a namespace:

```bash
kubectl get pods -n payments
```

Set current namespace for context:

```bash
kubectl config set-context --current --namespace=payments
```

---

# 10) Ingress — HTTP/HTTPS routing into the cluster

## 10.1 What Ingress is

**Ingress** is an API object that defines HTTP and HTTPS routing rules for external access to Services inside the cluster.

Ingress is not the same as a load balancer implementation by itself.  
It is a routing specification.

To actually enforce Ingress rules, you need an **Ingress Controller**.

Examples:
- NGINX Ingress Controller
- cloud-native ingress controllers
- Traefik

---

## 10.2 What Ingress solves

Ingress is used for:
- host-based routing
- path-based routing
- TLS termination
- multiple services behind one entry point

Without Ingress, exposing many web applications externally can become messy.

---

## 10.3 Example Ingress manifest

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
spec:
  rules:
    - host: app.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nginx-service
                port:
                  number: 80
```

### Meaning
- incoming traffic for `app.example.com`
- path `/`
- route to Service `nginx-service` port `80`

---

## 10.4 TLS in Ingress

Ingress can terminate TLS.

Example:

```yaml
spec:
  tls:
    - hosts:
        - app.example.com
      secretName: app-tls
```

The Secret contains the certificate and key.

---

## 10.5 Ingress Controller

The Ingress resource itself only defines rules.  
An Ingress Controller watches these resources and configures real proxy/load-balancer behavior.

This distinction is critical:
- Ingress object = desired routing rules
- Ingress Controller = implementation

---

## 10.6 Important Ingress commands

Get ingress objects:

```bash
kubectl get ingress
```

Describe ingress:

```bash
kubectl describe ingress app-ingress
```

---

# 11) ConfigMap — non-sensitive configuration

## 11.1 What a ConfigMap is

A **ConfigMap** stores non-sensitive configuration as key-value pairs.

Use cases:
- app settings
- config files
- environment values
- command-line arguments

Example:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: "production"
  LOG_LEVEL: "info"
```

Use as environment variables:

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

Or mount as files.

---

# 12) Secret — sensitive configuration

## 12.1 What a Secret is

A **Secret** stores sensitive data such as:
- passwords
- API keys
- tokens
- certificates

Example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
stringData:
  username: admin
  password: supersecret
```

Use in a Pod:

```yaml
env:
  - name: DB_USER
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: username
```

### Important note
Secrets are base64-encoded in manifests or stored form, not automatically encrypted unless cluster encryption at rest is configured.

---

# 13) Volumes, PersistentVolumes, and PersistentVolumeClaims

## 13.1 Why storage is needed

Containers are often ephemeral.  
If a container is recreated, data inside its writable container layer is usually lost.

Kubernetes uses volume abstractions for storage.

---

## 13.2 Volumes

A Pod can mount a **volume**.

Examples:
- `emptyDir`
- `configMap`
- `secret`
- persistent storage-backed volumes

### `emptyDir`
Created when a Pod starts and removed when the Pod is deleted.

Useful for:
- scratch space
- shared temporary data between containers in a Pod

Example:

```yaml
volumes:
  - name: cache-volume
    emptyDir: {}
```

---

## 13.3 PersistentVolume and PersistentVolumeClaim

### PersistentVolume (PV)
A cluster storage resource.

### PersistentVolumeClaim (PVC)
A request for storage by a workload.

The claim abstracts storage consumption from the actual storage implementation.

Example PVC:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

Then mount it in a Pod.

---

# 14) StatefulSet — stable identity for stateful workloads

## 14.1 What a StatefulSet is

A **StatefulSet** is used for workloads that need:
- stable network identity
- stable storage
- ordered startup and shutdown
- persistent Pod naming

Examples:
- databases
- Kafka brokers
- ZooKeeper
- distributed systems needing identity

### Key difference from Deployment
Deployment is optimized for stateless Pods.  
StatefulSet provides stable identity.

Pod names look like:
- `db-0`
- `db-1`
- `db-2`

This ordinal identity is very important.

---

# 15) DaemonSet — one Pod per node pattern

## 15.1 What a DaemonSet is

A **DaemonSet** ensures that a Pod runs on all or selected nodes.

Common uses:
- log collection agents
- node monitoring agents
- security agents
- networking components

Examples:
- Fluent Bit
- node-exporter
- CNI-related agents

---

# 16) Job and CronJob

## 16.1 Job

A **Job** runs Pods until a task completes successfully.

This is different from a Deployment, which is intended for continuously running services.

Use cases:
- batch tasks
- database migrations
- one-time processing

## 16.2 CronJob

A **CronJob** runs Jobs on a schedule.

Example use cases:
- nightly reports
- cleanup tasks
- backup jobs

---

# 17) Labels and selectors

## 17.1 Labels

Labels are key-value metadata attached to objects.

Example:

```yaml
labels:
  app: payments
  tier: backend
  env: prod
```

Labels are crucial because many Kubernetes objects use them for selection.

---

## 17.2 Selectors

Selectors query objects by labels.

Example:
A Service may select Pods where:

```yaml
selector:
  app: payments
```

This label-selection mechanism is one of the most important design ideas in Kubernetes.

---

# 18) Requests and limits

## 18.1 Resource requests

A request tells Kubernetes the minimum resource amount needed for scheduling.

Example:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
```

The scheduler uses requests to decide placement.

---

## 18.2 Resource limits

A limit defines the maximum resource usage allowed.

Example:

```yaml
resources:
  limits:
    cpu: "500m"
    memory: "512Mi"
```

Limits affect runtime enforcement.

### Important distinction
- **requests** affect scheduling
- **limits** affect runtime boundaries

---

# 19) Probes — liveness, readiness, startup

## 19.1 Liveness probe

Checks whether the container should be restarted.

If liveness fails repeatedly, Kubernetes kills and restarts the container.

---

## 19.2 Readiness probe

Checks whether the Pod is ready to receive traffic.

If readiness fails:
- container may still run
- but it is removed from Service endpoints

This distinction is very important.

---

## 19.3 Startup probe

Used for slow-starting applications.  
It prevents liveness from killing the app too early during startup.

---

## 19.4 Example probe configuration

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 10

readinessProbe:
  httpGet:
    path: /readyz
    port: 8080
  initialDelaySeconds: 5
```

---

# 20) Networking model

Kubernetes networking has some key assumptions:

- every Pod gets an IP
- Pods can communicate without NAT inside the cluster
- Services provide stable addressing and load balancing
- DNS resolves Services and Pods
- CNI plugins implement Pod networking
- Ingress controls HTTP/HTTPS entry

This model is conceptually elegant but operationally rich.

---

# 21) DNS in Kubernetes

Kubernetes provides internal DNS so workloads can resolve Service names.

Example Service DNS:

```text
nginx-service.default.svc.cluster.local
```

Usually short names work within the same namespace.

DNS is one of the key service-discovery mechanisms in Kubernetes.

---

# 22) NetworkPolicy

A **NetworkPolicy** controls allowed network communication between Pods and namespaces, depending on CNI support.

Example use:
- default deny
- allow only frontend -> backend
- allow only backend -> database
- allow DNS

This is important because basic Kubernetes networking is typically open unless policies are applied.

---

# 23) RBAC — access control in Kubernetes

RBAC stands for **Role-Based Access Control**.

Key objects:
- Role
- ClusterRole
- RoleBinding
- ClusterRoleBinding

These control:
- who can do what
- on which resources
- in which scope

This is fundamental for secure cluster access.

---

# 24) YAML structure and common fields

Most Kubernetes manifests share this pattern:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  namespace: default
  labels:
    app: my-app
spec:
  ...
```

Main top-level sections:
- `apiVersion`
- `kind`
- `metadata`
- `spec`

Many objects also expose `status`, but that is usually generated by the system, not authored by users.

---

# 25) Declarative vs imperative usage

Kubernetes supports both styles.

## Declarative
You define YAML and apply it.

```bash
kubectl apply -f deployment.yaml
```

## Imperative
You run direct commands.

```bash
kubectl create deployment nginx --image=nginx
```

In real systems, declarative management is preferred because it is versionable and reproducible.

---

# 26) Important kubectl commands with examples

## 26.1 Cluster information

```bash
kubectl cluster-info
kubectl version
kubectl get nodes
kubectl describe node <node-name>
```

---

## 26.2 Working with manifests

Apply file:

```bash
kubectl apply -f app.yaml
```

Delete file:

```bash
kubectl delete -f app.yaml
```

View rendered object:

```bash
kubectl get -f app.yaml
```

---

## 26.3 Get resources

```bash
kubectl get pods
kubectl get deployments
kubectl get svc
kubectl get ingress
kubectl get namespaces
kubectl get all
```

Wide output:

```bash
kubectl get pods -o wide
```

YAML output:

```bash
kubectl get deployment nginx-deployment -o yaml
```

---

## 26.4 Describe resources

```bash
kubectl describe pod nginx-pod
kubectl describe deployment nginx-deployment
kubectl describe svc nginx-service
kubectl describe ingress app-ingress
```

`describe` is useful because it shows:
- detailed fields
- events
- conditions
- probe config
- selectors
- endpoints

---

## 26.5 Logs

```bash
kubectl logs nginx-pod
kubectl logs -f nginx-pod
kubectl logs nginx-pod -c nginx
```

Previous container logs:

```bash
kubectl logs nginx-pod --previous
```

---

## 26.6 Exec into containers

```bash
kubectl exec -it nginx-pod -- /bin/sh
```

For multi-container Pods:

```bash
kubectl exec -it nginx-pod -c sidecar -- /bin/sh
```

---

## 26.7 Port forwarding

```bash
kubectl port-forward pod/nginx-pod 8080:80
kubectl port-forward svc/nginx-service 8080:80
```

This forwards local traffic to a Pod or Service.

---

## 26.8 Scaling and rollout

```bash
kubectl scale deployment nginx-deployment --replicas=5
kubectl rollout status deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment
```

---

## 26.9 Editing resources

```bash
kubectl edit deployment nginx-deployment
```

Set image:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.28
```

---

## 26.10 Namespace-scoped commands

```bash
kubectl get pods -n payments
kubectl describe svc api -n payments
kubectl logs api-pod -n payments
```

---

## 26.11 Context and config

Show current context:

```bash
kubectl config current-context
```

List contexts:

```bash
kubectl config get-contexts
```

Switch context:

```bash
kubectl config use-context my-cluster
```

Set default namespace:

```bash
kubectl config set-context --current --namespace=payments
```

---

# 27) Example full application set

Below is a small example using Namespace, Deployment, Service, and Ingress.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: demo
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  namespace: demo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
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
---
apiVersion: v1
kind: Service
metadata:
  name: web-service
  namespace: demo
spec:
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-ingress
  namespace: demo
spec:
  rules:
    - host: web.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web-service
                port:
                  number: 80
```

### What this example demonstrates
- a dedicated namespace
- a deployment with 2 replicas
- a Service selecting Pods via labels
- an Ingress routing external HTTP traffic to the Service

Apply it:

```bash
kubectl apply -f app.yaml
```

Inspect it:

```bash
kubectl get all -n demo
kubectl get ingress -n demo
```

---

# 28) Common interview-ready technical explanations

## What is a Pod?

A strong answer:

> A Pod is the smallest deployable unit in Kubernetes. It wraps one or more tightly coupled containers that share the same network namespace, IP address, port space, and optionally shared volumes. Pods are ephemeral and are usually managed by higher-level controllers rather than created directly.

---

## What is a Service?

A strong answer:

> A Service provides stable network access to a dynamic set of Pods selected by labels. Since Pod IPs are ephemeral, Services provide a stable virtual IP and DNS name and load-balance traffic to the current backend endpoints.

---

## What is a Deployment?

A strong answer:

> A Deployment is a higher-level controller for managing stateless applications declaratively. It manages ReplicaSets and Pods, supports rolling updates and rollbacks, and ensures the desired replica count and Pod template are maintained over time.

---

## What is a Namespace?

A strong answer:

> A Namespace is a logical partition inside a Kubernetes cluster used for organization, scoping, access control, quotas, and policy separation. It is primarily a logical isolation mechanism rather than a full security boundary by itself.

---

## What is Ingress?

A strong answer:

> Ingress is a Kubernetes API object that defines HTTP and HTTPS routing rules for external traffic entering the cluster. It typically supports host-based and path-based routing and requires an Ingress Controller to implement the routing behavior.

---

# 29) Open-source references

These are useful sources for deeper technical reading:

1. Kubernetes concepts  
   https://kubernetes.io/docs/concepts/

2. Kubernetes architecture  
   https://kubernetes.io/docs/concepts/architecture/

3. Pods  
   https://kubernetes.io/docs/concepts/workloads/pods/

4. ReplicaSet  
   https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

5. Deployment  
   https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

6. Service  
   https://kubernetes.io/docs/concepts/services-networking/service/

7. Ingress  
   https://kubernetes.io/docs/concepts/services-networking/ingress/

8. Namespace  
   https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

9. ConfigMap  
   https://kubernetes.io/docs/concepts/configuration/configmap/

10. Secret  
   https://kubernetes.io/docs/concepts/configuration/secret/

11. Volumes  
   https://kubernetes.io/docs/concepts/storage/volumes/

12. PersistentVolume  
   https://kubernetes.io/docs/concepts/storage/persistent-volumes/

13. StatefulSet  
   https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/

14. DaemonSet  
   https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

15. Job  
   https://kubernetes.io/docs/concepts/workloads/controllers/job/

16. CronJob  
   https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/

17. DNS for Services and Pods  
   https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/

18. NetworkPolicy  
   https://kubernetes.io/docs/concepts/services-networking/network-policies/

19. RBAC  
   https://kubernetes.io/docs/reference/access-authn-authz/rbac/

20. kubectl overview  
   https://kubernetes.io/docs/reference/kubectl/

---

# 30) Final revision summary

The most important way to understand Kubernetes is this:

- Kubernetes is a **declarative orchestration system**
- you define desired state through API objects
- the control plane stores and manages cluster state
- controllers continuously reconcile actual state toward desired state
- Pods are the smallest runtime unit
- Deployments manage stateless application rollouts
- Services provide stable networking
- Namespaces provide logical separation
- Ingress provides HTTP/HTTPS routing into the cluster
- labels and selectors connect objects together
- `kubectl` is the primary client for interacting with the API

If you deeply understand:
- architecture
- Pods
- Services
- Deployments
- Namespaces
- Ingress
- storage
- config
- networking
- controllers
- commands

then you understand the core technical model of Kubernetes very well.
