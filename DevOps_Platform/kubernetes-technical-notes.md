# Kubernetes Technical Notes

## Cluster Architecture

Kubernetes is a distributed container orchestration system built around a declarative API and controller-driven reconciliation.
Its architecture separates a replicated control plane that stores and evaluates desired state from node-level data-plane components that execute workloads.

### Control Plane
The control plane is the authoritative management layer that stores cluster state and drives scheduling, reconciliation, admission, and policy evaluation.
It exposes the Kubernetes API, persists object state, and coordinates controllers that converge live state toward declared intent.

- **kube-apiserver** 
 The API server is the front door for all Kubernetes control-plane operations.
 It validates, admits, stores, and serves API objects and is the only component that directly persists state to etcd.

- **etcd** 
 etcd is the strongly consistent distributed key-value store used as Kubernetes’ backing state database.
 It stores cluster configuration, object state, leader-election data, and metadata under a replicated Raft-based consistency model.

- **kube-scheduler** 
 The scheduler is the control-plane component that assigns unscheduled Pods to Nodes.
 It evaluates placement constraints such as resource requests, affinity, taints, topology, and scoring plugins before binding a Pod.

- **kube-controller-manager** 
 The controller manager runs built-in reconciliation loops for core Kubernetes resources.
 It watches API objects and creates or modifies dependent objects until observed state matches desired state.

- **cloud-controller-manager** 
 The cloud controller manager integrates Kubernetes with an external cloud provider control plane.
 It reconciles cloud-specific resources such as nodes, routes, and load balancers outside the core Kubernetes binaries.

### Node Components
Node components form the execution layer that runs Pods and implements networking and storage attachment on worker machines.
They consume desired state from the control plane and materialize containers, network paths, mounts, and health reporting on each node.

- **kubelet** 
 The kubelet is the node agent responsible for ensuring that PodSpecs assigned to the node are running.
 It pulls images, mounts volumes, launches containers through the runtime, and reports Pod and node status to the API server.

- **container runtime** 
 The container runtime is the software responsible for creating and running containers on a node.
 It implements the CRI interface and manages image pulls, container lifecycle, and sandbox execution for Pods.

- **kube-proxy** 
 kube-proxy is the node networking component that implements Service VIP forwarding behavior.
 It programs iptables, IPVS, or equivalent packet-handling rules so traffic sent to a Service reaches a healthy backend Pod.

- **CNI implementation** 
 A CNI implementation is the plugin or agent responsible for pod network setup on a node.
 It allocates pod IP addresses, configures interfaces and routes, and enforces network-policy semantics if supported.

- **CSI node plugin** 
 A CSI node plugin is the node-side storage component used by a CSI driver.
 It performs mount, unmount, staging, and publish operations for volumes attached to Pods on that node.

### High Availability
High availability in Kubernetes requires eliminating single points of failure in the control plane and preserving quorum for state persistence.
It typically uses multiple API servers, replicated control-plane nodes, and odd-number etcd member sets to tolerate host or zone failure.

- **API server replication** 
 API server replication is the deployment of multiple stateless API server instances behind a load balancer.
 It increases control-plane availability and request throughput while relying on shared etcd state.

- **etcd quorum** 
 etcd quorum is the majority requirement for committing writes in the distributed datastore.
 Loss of quorum prevents state mutation even if some etcd members remain reachable.

- **control-plane failure domains** 
 Control-plane failure domains are the infrastructure boundaries that can disrupt multiple control-plane components simultaneously.
 They include host, rack, zone, and network-partition events that should be considered during topology design.

---

## Kubernetes API and Object Model

Kubernetes is driven by API objects that describe desired state rather than imperative process execution.
The control plane stores these objects, assigns defaults, validates schemas, and uses controllers to reconcile status over time.

### Declarative Model
The declarative model is the operating principle in which a client submits desired object state and controllers converge the cluster toward it.
It separates intent declaration from execution details and enables idempotent reconciliation across failures and retries.

- **spec and status** 
 `spec` is the desired-state portion of an API object and `status` is the observed-state portion managed by the system.
 Controllers update status to reflect runtime reality while users and automation usually write spec.

- **reconciliation loop** 
 A reconciliation loop is a watch-and-act control algorithm that compares desired and observed state.
 It repeatedly issues changes until the object graph converges or an error condition blocks progress.

- **resource version** 
 A resource version is the opaque concurrency token associated with an API object read from storage.
 It is used for optimistic concurrency control and watch progression semantics.

### API Resources
API resources are typed objects served under versioned API groups by the API server.
They define the schema, scope, storage version, and lifecycle behavior for cluster-managed state.

- **kind** 
 `kind` identifies the object type represented by a manifest or API response.
 It is resolved together with `apiVersion` to a concrete resource schema and endpoint.

- **apiVersion** 
 `apiVersion` identifies the API group and version used to interpret the object.
 It determines which schema, conversion logic, and stability contract apply to that resource.

- **metadata** 
 `metadata` contains identifying and lifecycle information for an object.
 It includes fields such as name, namespace, UID, labels, annotations, generation, and owner references.

- **UID** 
 A UID is the immutable unique identifier assigned to an object instance.
 It distinguishes recreated objects that reuse the same name from their deleted predecessors.

### Labels, Selectors, and Annotations
Labels and selectors are the primary grouping and matching mechanism across Kubernetes resources.
Annotations carry non-identifying metadata that tools and controllers consume without participating in selector logic.

- **labels** 
 Labels are key-value metadata used for grouping, selection, and operational organization.
 They are intended for queryable identity and are evaluated by selectors in controllers and Services.

- **label selectors** 
 Label selectors are match expressions used to select sets of objects by label.
 They support equality-based and set-based filtering semantics depending on the API field.

- **annotations** 
 Annotations are unstructured key-value metadata attached to objects.
 They store tool-specific or informational data that should not be used for object identity selection.

### Namespaces and Scope
Namespaces partition namespaced resources into logical scopes inside a cluster.
They provide API-name isolation and policy boundaries but do not create a hard security boundary by themselves.

- **namespaced resources** 
 Namespaced resources exist inside a namespace and are addressed by name within that scope.
 Their lifecycle and policy attachment are isolated from resources with the same name in other namespaces.

- **cluster-scoped resources** 
 Cluster-scoped resources exist outside any namespace and are global within the cluster.
 They include objects such as Nodes, PersistentVolumes, StorageClasses, and CustomResourceDefinitions.

- **default namespace** 
 The default namespace is the implicit namespaced scope used when a client omits a namespace.
 It is a convenience default and does not confer special isolation behavior.

### Ownership and Deletion Semantics
Kubernetes models object relationships explicitly so that dependent resources can be cleaned up automatically.
Deletion is not always immediate because controllers and finalizers may delay removal until cleanup completes.

- **owner references** 
 Owner references define parent-child relationships between Kubernetes objects.
 They enable garbage collection of dependent objects when the owning object is deleted.

- **finalizers** 
 Finalizers are metadata entries that block permanent object deletion until cleanup logic runs.
 They allow controllers to perform external or internal teardown before the object disappears from the API.

- **garbage collection** 
 Garbage collection is the controller-driven deletion of dependent objects based on owner references.
 It removes orphanable or dependent resources according to propagation policy and graph state.

### API Extensibility
Kubernetes can be extended with new APIs and controllers without modifying the core project binaries.
This allows platform teams and vendors to add domain-specific resources that behave like built-in objects.

- **CustomResourceDefinitions** 
 CustomResourceDefinitions are extension resources that register new API types with the Kubernetes API server.
 They define schema, versioning, validation, and storage behavior for custom objects.

- **aggregated APIs** 
 Aggregated APIs are external API servers registered behind the Kubernetes API aggregation layer.
 They expose additional API groups through the main API server endpoint while remaining independently implemented.

- **API versioning** 
 API versioning is the lifecycle and compatibility model used for Kubernetes resource schemas.
 It distinguishes alpha, beta, and stable behavior and supports conversion between served versions when configured.

---

## Workloads and Controllers

Kubernetes workloads are primarily expressed through Pod templates managed by higher-level controllers.
Controllers handle replica maintenance, rollout semantics, restart behavior, and state alignment over time.

### Pod
A Pod is the smallest deployable workload unit in Kubernetes.
It groups one or more tightly coupled containers that share a network namespace, IPC namespace, and mounted volumes.

- **Pod sandbox** 
 The Pod sandbox is the isolated execution context that holds the shared networking and namespace boundary for Pod containers.
 It is created before application containers and persists while the Pod exists on the node.

- **ephemeral nature** 
 Pods are ephemeral scheduling units rather than durable identities.
 Replacement Pods receive new UIDs and may run on different nodes even if recreated from the same template.

- **restart policy** 
 Restart policy defines how containers inside a Pod are restarted after exit.
 It is interpreted by the kubelet and varies among workload types that create Pods.

### ReplicaSet
A ReplicaSet is the controller that maintains a stable number of matching Pod replicas.
It continuously creates or deletes Pods until the actual replica count matches the declared target.

- **selector ownership** 
 Selector ownership is the relationship between a ReplicaSet selector and the Pods it manages.
 A ReplicaSet only adopts Pods whose labels match its selector and whose ownership is compatible.

- **replica maintenance** 
 Replica maintenance is the continuous controller action that replaces missing Pods or removes excess Pods.
 It preserves target scale despite node failures, Pod exits, or manual deletions.

### Deployment
A Deployment is the declarative controller for stateless replicated applications.
It manages ReplicaSets and orchestrates rolling updates and rollbacks to new Pod templates.

- **rolling update** 
 Rolling update is the default Deployment strategy for incrementally replacing old Pods with new Pods.
 It controls parallelism and availability using max surge and max unavailable parameters.

- **rollback** 
 Rollback is the act of returning a Deployment to a previous Pod template revision.
 It works by promoting an older ReplicaSet template state back into active rollout management.

- **revision history** 
 Revision history is the retention of prior ReplicaSets created by Deployment changes.
 It enables operational rollback and auditability of template evolution.

### StatefulSet
A StatefulSet is the workload controller for applications requiring stable identity and ordered lifecycle behavior.
It creates Pods with sticky ordinal identity, stable network names, and persistent volume association.

- **stable identity** 
 Stable identity is the predictable Pod naming and ordinal numbering behavior of StatefulSets.
 It allows stateful workloads to bind replica identity to configuration and storage.

- **ordered rollout** 
 Ordered rollout is the sequential create, update, and delete behavior applied by StatefulSets by default.
 It preserves deterministic dependency order across replicas during lifecycle transitions.

- **persistent volume association** 
 Persistent volume association is the binding of per-replica storage claims to StatefulSet Pod ordinals.
 It allows storage to outlive individual Pod recreation while remaining attached to the same logical replica.

### DaemonSet
A DaemonSet is the controller that ensures a Pod runs on selected nodes across the cluster.
It is typically used for node-level agents such as log shippers, network daemons, and storage agents.

- **node coverage** 
 Node coverage is the automatic placement of one matching DaemonSet Pod onto eligible nodes.
 Newly added nodes receive Pods automatically if they satisfy scheduling constraints.

- **toleration usage** 
 Toleration usage is common in DaemonSets because node-level agents often need to run on tainted or control-plane nodes.
 The Pod template usually includes explicit tolerations to reach infrastructure nodes.

### Job
A Job is the controller for finite, completion-oriented Pods.
It creates Pods until a specified number of successful completions has been achieved.

- **completions** 
 Completions define how many successful Pod runs are required for a Job to finish.
 The controller stops creating new Pods once the completion target is met.

- **parallelism** 
 Parallelism defines how many Job Pods may run concurrently.
 It controls throughput independently from total completions required.

- **backoff limit** 
 Backoff limit is the retry threshold for failed Job Pods before the Job is considered failed.
 It bounds repeated restart or recreation attempts under persistent error conditions.

### CronJob
A CronJob is the scheduled controller that creates Jobs on a cron-like timetable.
It is used for periodic batch execution rather than continuously running services.

- **schedule** 
 Schedule is the cron expression that determines when a CronJob should create a new Job.
 It is evaluated by the CronJob controller according to cluster time semantics.

- **concurrency policy** 
 Concurrency policy controls how overlapping CronJob runs are handled.
 It can allow, forbid, or replace overlapping Job execution instances.

### Container Lifecycle Patterns
Container lifecycle patterns define how auxiliary logic is coupled to a primary application container.
They shape startup sequencing, long-running support functions, and in-Pod process composition.

- **init containers** 
 Init containers are containers that run to completion before application containers start.
 They are used for ordered initialization tasks such as setup, dependency checks, or config generation.

- **sidecar containers** 
 Sidecar containers are long-running auxiliary containers that execute alongside the main application container in a Pod.
 They share the Pod environment and provide supporting functions such as proxying, logging, or synchronization.

- **ephemeral containers** 
 Ephemeral containers are temporary debugging containers injected into existing Pods.
 They are not restartable and are intended for troubleshooting rather than regular application design.

### Health Probes
Health probes are kubelet-executed checks that influence restart and traffic-routing behavior for containers.
They are essential for making runtime health visible to both the node and higher-level controllers.

- **liveness probe** 
 A liveness probe determines whether a container should be restarted because it is no longer functioning correctly.
 Failure causes the kubelet to restart the affected container according to Pod restart policy.

- **readiness probe** 
 A readiness probe determines whether a container is ready to receive traffic.
 Failure removes the Pod from Service endpoints without necessarily restarting the container.

- **startup probe** 
 A startup probe determines whether a slow-starting container has completed initialization.
 While it is failing, liveness and readiness checks are suppressed for that container.

### Disruption Controls
Disruption controls protect workload availability during voluntary operations such as upgrades and drains.
They are especially important for replicated services and control-plane-adjacent workloads.

- **PodDisruptionBudget** 
 A PodDisruptionBudget defines the minimum available or maximum unavailable Pods during voluntary disruption.
 It is respected by eviction-aware controllers and administrative drain operations but not by all involuntary failures.

- **eviction API** 
 The eviction API is the policy-aware deletion path for Pods under voluntary disruption.
 It checks disruption budgets and admission before allowing Pod termination to proceed.

---

## Scheduling and Resource Management

Kubernetes scheduling is a multi-stage decision process that evaluates constraints, preferences, and resource availability.
Resource management translates declared CPU, memory, and storage intent into placement, runtime isolation, and eviction behavior.

### Scheduling Process
The scheduling process assigns an unscheduled Pod to a Node that satisfies placement requirements.
It filters ineligible nodes first and then scores eligible nodes before making a binding decision.

- **filtering phase** 
 The filtering phase removes nodes that cannot run the Pod.
 It evaluates hard constraints such as resource fit, taints, node selectors, and volume requirements.

- **scoring phase** 
 The scoring phase ranks the remaining feasible nodes.
 It evaluates preferences such as affinity weights, spread balance, and resource distribution to choose a best-fit node.

- **binding** 
 Binding is the act of associating a Pod with a selected Node.
 It updates the Pod’s scheduled node assignment in the API server so the kubelet can materialize it.

### Resource Requests and Limits
Requests and limits define the CPU and memory envelopes used for scheduling and runtime enforcement.
They are core to placement, QoS classification, and out-of-resource behavior on the node.

- **requests** 
 Requests are the minimum CPU or memory quantities used by the scheduler for placement decisions.
 They also influence resource shares and reservation behavior on the node.

- **limits** 
 Limits are the maximum CPU or memory quantities a container may consume at runtime.
 CPU limits are throttled by the kernel while memory limits can trigger OOM termination when exceeded.

- **extended resources** 
 Extended resources are non-standard schedulable quantities exposed by nodes or device plugins.
 They are used for discrete hardware such as GPUs or vendor-specific accelerators.

### QoS and Eviction
QoS and eviction determine which Pods are most likely to be preserved or terminated during node pressure.
They are derived from resource declarations and actual runtime pressure signals.

- **Guaranteed QoS** 
 Guaranteed QoS is assigned when every container in a Pod has equal CPU and memory request and limit values.
 These Pods receive the strongest protection from eviction under node pressure.

- **Burstable QoS** 
 Burstable QoS is assigned when a Pod has at least one request set but does not meet Guaranteed criteria.
 These Pods may consume beyond requests and are evicted after BestEffort under pressure.

- **BestEffort QoS** 
 BestEffort QoS is assigned when no CPU or memory requests or limits are set for any container in the Pod.
 These Pods are the first candidates for eviction during resource pressure.

- **eviction** 
 Eviction is the kubelet or control-plane-driven removal of Pods due to policy or resource pressure.
 It terminates Pods to restore node stability or enforce scheduling and disruption constraints.

### Placement Constraints
Placement constraints let operators shape where workloads land in the cluster.
They express both hard requirements and soft preferences over node attributes and topology domains.

- **nodeSelector** 
 `nodeSelector` is the simplest hard constraint for limiting Pod placement to nodes with matching labels.
 The scheduler only considers nodes that satisfy every specified label equality.

- **node affinity** 
 Node affinity is the richer scheduling rule system for matching Pods to labeled nodes.
 It supports required and preferred rules with set-based operators and weighted preferences.

- **pod affinity** 
 Pod affinity is the rule system that attracts a Pod toward nodes already running other matching Pods.
 It is commonly used to colocate tightly coupled services within a topology domain.

- **pod anti-affinity** 
 Pod anti-affinity is the rule system that repels a Pod from nodes or zones running matching Pods.
 It is used to spread replicas and reduce correlated failure risk.

- **topology spread constraints** 
 Topology spread constraints limit skew in Pod distribution across topology domains such as zones or nodes.
 They improve resilience by preventing overly concentrated placement of replicated workloads.

### Taints and Tolerations
Taints and tolerations form a negative scheduling model that repels Pods from selected nodes unless explicitly tolerated.
They are commonly used for dedicated nodes, special hardware, or infrastructure isolation.

- **taints** 
 Taints are node attributes that signal one or more scheduling or execution restrictions.
 They can prevent scheduling, prefer avoidance, or evict non-tolerating Pods depending on effect.

- **tolerations** 
 Tolerations are Pod rules that allow a Pod to schedule onto or remain on tainted nodes.
 They do not force placement by themselves but merely permit the scheduler or kubelet to consider the node.

### Priority and Preemption
Priority and preemption influence which Pods are favored under contention.
They provide a way to reserve scheduling importance for critical workloads without bypassing all constraints.

- **PriorityClass** 
 A PriorityClass defines an integer priority value and optional metadata for Pods.
 Higher-priority Pods are scheduled before lower-priority Pods when resources are scarce.

- **preemption** 
 Preemption is the scheduler action that evicts lower-priority Pods to make room for a pending higher-priority Pod.
 It only occurs when no feasible placement exists without freeing resources.

### Resource Governance
Resource governance prevents one namespace or workload class from exhausting cluster capacity.
It applies administrative defaults and hard consumption ceilings to multi-tenant or shared clusters.

- **ResourceQuota** 
 A ResourceQuota limits aggregate resource consumption and object counts within a namespace.
 It denies new object creation or updates that would exceed configured quota boundaries.

- **LimitRange** 
 A LimitRange defines default, minimum, and maximum resource constraints for objects in a namespace.
 It can inject default requests and limits and reject out-of-range container specifications.

---

## Networking

Kubernetes networking provides flat pod addressing, service virtual IPs, and policy-controlled traffic paths across nodes.
It relies on pluggable implementations for interface setup, routing, packet handling, and optional network security enforcement.

### Pod Networking Model
The Kubernetes pod networking model requires every Pod to receive its own routable IP address.
All containers in a Pod share that Pod IP and can communicate over localhost within the Pod sandbox.

- **flat address space** 
 Flat address space means Pods can address each other directly without NAT between Pod IPs in the cluster design model.
 The actual implementation depends on the CNI and underlying network, but the Pod abstraction assumes direct reachability.

- **Pod IP** 
 A Pod IP is the primary network address assigned to a Pod sandbox.
 It is allocated by the cluster networking implementation and is ephemeral across Pod recreation.

- **localhost sharing** 
 Localhost sharing is the intra-Pod communication property created by the shared network namespace.
 Containers in the same Pod can reach each other on `127.0.0.1` and shared ports.

### CNI and Data Plane Behavior
The Container Network Interface defines how Kubernetes asks a network plugin to configure Pod connectivity.
The CNI plugin is responsible for wiring Pod interfaces, routes, and often policy or overlay behavior.

- **CNI contract** 
 The CNI contract is the interface between the kubelet or runtime and a network plugin.
 It defines ADD and DEL operations for network namespace attachment and teardown.

- **overlay networking** 
 Overlay networking is a packet transport model that encapsulates Pod traffic over the underlying node network.
 It simplifies multi-node Pod routing at the cost of encapsulation overhead and MTU considerations.

- **routed networking** 
 Routed networking is a Pod network model that uses native routing rather than encapsulated overlays.
 It typically requires the underlying network to understand or propagate Pod CIDRs.

- **network policy enforcement** 
 Network policy enforcement is the data-plane behavior that applies allow rules between Pods and namespaces.
 It only works when the chosen CNI implementation supports NetworkPolicy semantics.

### Service Networking
Services provide stable virtual endpoints over dynamic Pod backends.
They decouple client discovery from Pod IP churn and implement traffic selection through label-based backend membership.

- **Service object** 
 A Service object defines a stable network access point and backend selector or mapping.
 It abstracts a changing set of Pods behind a durable DNS name and virtual IP.

- **selector-based backends** 
 Selector-based backends are Service endpoints derived automatically from labels on matching Pods.
 They change dynamically as Pods become ready, terminate, or are replaced.

- **headless Service** 
 A headless Service is a Service with no cluster virtual IP allocation.
 It returns backend records directly through DNS and is commonly used with StatefulSets or client-side discovery.

### Service Types
Service types define different exposure and routing models for network access to backends.
They reuse the same Service abstraction while changing how traffic reaches the Service.

- **ClusterIP** 
 ClusterIP is the default Service type that exposes a virtual IP reachable only from within the cluster network.
 It is implemented by kube-proxy or equivalent data-plane logic on cluster nodes.

- **NodePort** 
 NodePort is the Service type that opens the same static port on each node and forwards traffic to the Service.
 It extends ClusterIP behavior and is often used as a primitive for external load balancer integration.

- **LoadBalancer** 
 LoadBalancer is the Service type that asks an external integration to provision a load balancer for the Service.
 It usually builds on NodePort or direct backend programming depending on cloud-controller or implementation specifics.

- **ExternalName** 
 ExternalName is the Service type that returns a DNS CNAME to an external name rather than proxying traffic.
 It performs name indirection only and does not create local endpoints or a VIP.

### Endpoint Representation
Endpoint representation is how Kubernetes stores backend address information for a Service.
Modern clusters use EndpointSlice rather than the legacy Endpoints object for scalability and extensibility.

- **EndpointSlice** 
 EndpointSlice is the scalable backend endpoint resource for Services.
 It stores subsets of backend addresses, ports, conditions, and topology hints across multiple slice objects.

- **Endpoints** 
 Endpoints is the older monolithic backend representation for a Service.
 It is maintained for compatibility but is less scalable than EndpointSlice in large clusters.

- **readiness gating of endpoints** 
 Readiness gating of endpoints is the rule that only ready Pods are included as serving backends for most Service traffic.
 It prevents traffic from being sent to Pods that have not yet passed readiness evaluation.

### kube-proxy Modes
kube-proxy implements Service forwarding by programming packet-handling rules on nodes.
Its mode affects scalability, rule expression, and connection-tracking behavior.

- **iptables mode** 
 iptables mode implements Service forwarding through chains of packet-filter and NAT rules in the Linux kernel.
 It is widely supported but can become complex with large numbers of Services and endpoints.

- **IPVS mode** 
 IPVS mode implements Service forwarding through the Linux IP virtual server subsystem.
 It provides efficient load-balancing primitives and scales better for many service backends.

- **userspace mode** 
 Userspace mode is the older kube-proxy forwarding implementation that proxies traffic in a userspace process.
 It is generally obsolete compared with kernel-based forwarding modes.

### NetworkPolicy
NetworkPolicy is the Kubernetes API object for allowlist-based traffic control between Pods and network peers.
It is enforced by the network plugin rather than by Kubernetes core components.

- **pod selector scope** 
 Pod selector scope determines which Pods a NetworkPolicy applies to as policy targets.
 Once selected, those Pods become isolated for the specified direction unless allowed traffic matches a rule.

- **ingress policy** 
 Ingress policy controls which sources may connect to selected Pods.
 It does not affect egress unless egress rules are also defined.

- **egress policy** 
 Egress policy controls which destinations selected Pods may connect to.
 It is enforced only when the network plugin supports egress NetworkPolicy semantics.

- **namespace selector** 
 Namespace selector is a NetworkPolicy peer selector that matches namespaces by label.
 It is used to permit or restrict cross-namespace traffic at policy level.

### DNS and Service Discovery
DNS is the default service discovery mechanism inside Kubernetes clusters.
It maps Services and some Pods to stable names so clients can resolve endpoints dynamically.

- **CoreDNS** 
 CoreDNS is the standard in-cluster DNS server deployment for Kubernetes.
 It watches Services, EndpointSlices, and related objects and answers DNS queries for cluster-local names.

- **cluster domain** 
 The cluster domain is the DNS suffix used for in-cluster service discovery.
 Service FQDNs typically follow the pattern `<service>.<namespace>.svc.<cluster-domain>`.

- **Pod DNS policy** 
 Pod DNS policy determines how DNS configuration is injected into a Pod.
 It controls whether a Pod uses cluster DNS, node DNS, or customized resolution behavior.

---

## Service Discovery and Traffic Exposure

Kubernetes provides multiple layers for exposing services beyond Pod-to-Pod communication.
These range from internal DNS and Services to HTTP routing resources and implementation-specific gateways.

### Ingress
Ingress is the Kubernetes API resource for HTTP and HTTPS request routing into a cluster.
It defines desired routing rules but relies on an Ingress controller to implement the actual data plane.

- **Ingress resource** 
 An Ingress resource describes host- and path-based routing rules for HTTP traffic.
 It references Services as backends and can include TLS configuration metadata.

- **Ingress controller** 
 An Ingress controller is the implementation that watches Ingress resources and configures a real proxy or load balancer.
 Different controllers may interpret annotations, TLS, and path semantics differently.

- **IngressClass** 
 IngressClass identifies which controller should implement a given Ingress resource.
 It provides an explicit binding between an Ingress object and a controller implementation.

### Gateway API
Gateway API is the newer Kubernetes traffic-management API family for role-oriented network exposure.
It separates infrastructure ownership from route ownership more cleanly than the Ingress model.

- **GatewayClass** 
 A GatewayClass defines a type of gateway infrastructure managed by a controller.
 It is analogous to a storage or ingress class but for Gateway API implementations.

- **Gateway** 
 A Gateway is the infrastructure-facing resource that instantiates listeners and network entry points.
 It represents the actual L4 or L7 ingress boundary managed by a controller.

- **HTTPRoute** 
 HTTPRoute is the routing resource for HTTP traffic in Gateway API.
 It attaches to one or more Gateways and defines host, path, header, and filter-based routing rules.

- **ReferenceGrant** 
 ReferenceGrant is the cross-namespace permission object used by Gateway API.
 It allows specific cross-namespace object references that would otherwise be disallowed for safety.

### External Exposure Patterns
External exposure patterns describe how cluster services become reachable from outside the cluster.
The implementation depends on cloud integration, bare-metal networking, or dedicated ingress data planes.

- **cloud load balancer integration** 
 Cloud load balancer integration is the controller path that provisions provider load balancers for Services or Gateways.
 It maps Kubernetes objects to cloud-native networking resources and back-populates status fields.

- **bare-metal exposure** 
 Bare-metal exposure is the pattern used when no cloud provider provisions external load balancers automatically.
 It usually relies on NodePort, BGP-speaking controllers, VIP announcers, or dedicated ingress appliances.

- **external traffic policy** 
 External traffic policy controls how externally sourced traffic is forwarded to Service backends.
 It can preserve client source IP by limiting backend selection to node-local endpoints when set appropriately.

---

## Storage

Kubernetes storage separates compute lifecycle from persistent data lifecycle through abstract volume APIs.
It uses claims, classes, and CSI drivers to provision, attach, mount, snapshot, and expand storage across diverse backends.

### Volume Model
Volumes are the mechanism by which Pods access persistent or ephemeral storage.
They are mounted into containers and exist at the Pod level rather than the individual container level.

- **volume in Pod spec** 
 A volume in a Pod spec declares a named storage source available to all containers in that Pod.
 Containers mount the volume by reference into their own filesystem paths.

- **ephemeral volumes** 
 Ephemeral volumes are storage sources whose lifecycle is tied to the Pod.
 They are created for Pod use and are removed when the Pod is permanently deleted.

- **persistent volumes** 
 Persistent volumes are cluster resources representing provisioned storage independent of any single Pod.
 They can outlive Pods and be rebound to future consumers according to policy.

### PersistentVolume and PersistentVolumeClaim
PersistentVolume and PersistentVolumeClaim decouple storage supply from storage demand.
They let cluster administrators expose storage and workloads request it without hard-coding backend details.

- **PersistentVolume** 
 A PersistentVolume is a cluster-scoped storage resource representing a real backing volume.
 It describes capacity, access modes, reclaim policy, and the storage driver or source used.

- **PersistentVolumeClaim** 
 A PersistentVolumeClaim is a namespaced request for storage by a workload.
 It specifies requested size, access mode, and optionally class so the control plane can bind or provision a volume.

- **binding** 
 Binding is the control-plane process that associates a claim with a matching volume.
 It can attach a pre-existing PersistentVolume or trigger dynamic provisioning through a StorageClass.

### StorageClass and Provisioning
A StorageClass defines how dynamic provisioning should create persistent storage.
It abstracts backend-specific parameters and provisioning policies behind a named storage tier.

- **StorageClass** 
 A StorageClass is the class resource that describes a storage provisioner and its parameters.
 It is referenced by claims to request a particular backend, performance profile, or policy set.

- **dynamic provisioning** 
 Dynamic provisioning is the automatic creation of storage when a claim is made.
 It occurs through a provisioner, usually a CSI controller, rather than through a pre-created PersistentVolume.

- **reclaim policy** 
 Reclaim policy determines what happens to the backing storage after the claim is released.
 Common behaviors include deleting the backing resource or retaining it for manual recovery.

- **volume binding mode** 
 Volume binding mode controls when a claim is bound and when provisioning occurs relative to Pod scheduling.
 `WaitForFirstConsumer` delays provisioning or binding until a Pod is scheduled so topology-aware storage can be selected correctly.

### Access and Mount Semantics
Access and mount semantics determine how a volume may be attached and used by Pods.
These semantics are constrained by both the Kubernetes API and the actual capabilities of the underlying storage system.

- **ReadWriteOnce** 
 ReadWriteOnce means the volume can be mounted read-write by one node at a time.
 Multiple Pods on that same node may still use it if the implementation permits.

- **ReadOnlyMany** 
 ReadOnlyMany means the volume can be mounted read-only by multiple nodes concurrently.
 It is useful for shared reference data or immutable content distribution.

- **ReadWriteMany** 
 ReadWriteMany means the volume can be mounted read-write by multiple nodes concurrently.
 It requires a backend that supports shared writable access semantics.

- **volume mode** 
 Volume mode defines whether storage is presented as a mounted filesystem or a raw block device.
 It affects how the kubelet and runtime expose the volume to containers.

### CSI
The Container Storage Interface is the plugin contract used by Kubernetes to integrate external storage systems.
CSI decouples storage-driver implementation from Kubernetes core releases and supports a broad storage feature set.

- **CSI controller plugin** 
 A CSI controller plugin is the control-plane-facing part of a storage driver.
 It handles provisioning, attach-detach coordination, snapshot operations, expansion, and other cluster-scoped actions.

- **CSI node plugin** 
 A CSI node plugin is the node-local part of a storage driver.
 It stages, mounts, and unmounts volumes on the node where Pods are running.

- **CSI sidecars** 
 CSI sidecars are helper controllers that translate Kubernetes APIs into CSI calls.
 They implement patterns such as external provisioning, external attacher behavior, resizer, and snapshotter integration.

### Snapshots and Expansion
Snapshots and expansion extend Kubernetes storage beyond basic create-and-mount behavior.
They allow point-in-time capture and capacity growth under supported drivers and policies.

- **VolumeSnapshot** 
 A VolumeSnapshot is the API object representing a request for a point-in-time volume snapshot.
 It is implemented through CSI snapshot support rather than through built-in in-tree storage logic.

- **VolumeSnapshotClass** 
 A VolumeSnapshotClass defines the driver and parameters used when creating snapshots.
 It plays a role analogous to StorageClass for snapshot operations.

- **volume expansion** 
 Volume expansion is the increase of a persistent volume’s capacity through a claim update.
 It requires support from the StorageClass, provisioner, and often the filesystem for online growth.

### Common Volume Types
Kubernetes supports several built-in volume sources with different durability and lifecycle characteristics.
These types are often used for configuration injection, scratch space, or legacy integrations.

- **emptyDir** 
 `emptyDir` is an ephemeral volume created when a Pod is assigned to a node.
 It persists for the life of the Pod and is deleted when the Pod is permanently removed from that node.

- **configMap volume** 
 A ConfigMap volume projects configuration data as files into a Pod.
 The kubelet periodically refreshes mounted content to reflect updated source data subject to implementation timing.

- **secret volume** 
 A Secret volume projects secret data into a Pod as files.
 It stores content in memory-backed filesystems on the node when possible to reduce disk persistence exposure.

- **projected volume** 
 A projected volume combines multiple sources such as Secrets, ConfigMaps, service account tokens, and downward API into one mount.
 It provides a unified file projection surface assembled by the kubelet.

- **hostPath** 
 `hostPath` mounts a path from the node filesystem directly into a Pod.
 It breaks workload portability and weakens isolation, so it is usually restricted in secure clusters.

---

## Configuration and Secrets

Kubernetes separates application code from runtime configuration through first-class API objects and projection mechanisms.
This enables configuration mutation, secret scoping, and identity injection without rebuilding container images.

### ConfigMap
A ConfigMap stores non-secret configuration data as key-value pairs.
It can be consumed as environment variables, command arguments, or mounted files inside Pods.

- **environment injection** 
 Environment injection maps ConfigMap keys into container environment variables.
 Values are expanded at container start and do not automatically change in already-running processes.

- **volume projection** 
 Volume projection mounts ConfigMap keys as files in a container filesystem.
 Updated content can appear in the mounted volume without restarting the Pod, subject to kubelet refresh behavior.

### Secret
A Secret stores sensitive data in base64-encoded form within the Kubernetes API.
It is intended for credentials and tokens but requires additional cluster hardening for true confidentiality.

- **opaque secret** 
 An Opaque Secret is the generic key-value secret type in Kubernetes.
 It stores arbitrary data without additional structural validation.

- **typed secret** 
 A typed Secret uses a built-in secret type such as TLS or docker config JSON.
 It provides schema expectations for specific consumers or integration paths.

- **encryption at rest** 
 Encryption at rest is the API-server storage protection mechanism for Secrets and other objects in etcd.
 It requires explicit configuration and protects persisted data rather than every in-memory access path.

### Service Accounts
A ServiceAccount is the Kubernetes identity object used by Pods and automation inside the cluster.
It is distinct from human identities and is primarily used for API authentication and authorization.

- **default service account** 
 The default service account is the automatically created ServiceAccount in each namespace.
 Pods use it unless a different service account is explicitly specified.

- **service account token projection** 
 Service account token projection is the modern mechanism for mounting short-lived API tokens into Pods.
 It replaces long-lived auto-generated token Secrets with bounded audience and expiration semantics.

### Downward API
The downward API exposes Pod and container metadata to running workloads.
It allows a container to learn selected fields from its own Pod without querying the Kubernetes API directly.

- **environment exposure** 
 Environment exposure injects selected metadata fields into container environment variables.
 It is resolved at container start rather than as a continuously synchronized data stream.

- **file projection** 
 File projection exposes selected metadata fields as files in a mounted volume.
 It is useful when applications expect runtime metadata through the filesystem instead of environment variables.

---

## Security

Kubernetes security spans API authentication, authorization, workload isolation, network restrictions, and node hardening.
A secure cluster requires layered controls because no single feature provides complete tenant or workload isolation.

### Authentication and Authorization
Authentication identifies who or what is making an API request and authorization decides whether that request is allowed.
These stages occur before admission and before any object mutation reaches persistent storage.

- **authentication** 
 Authentication is the process of verifying the identity attached to an API request.
 Kubernetes supports multiple authenticators such as client certificates, bearer tokens, and external identity integrations.

- **authorization** 
 Authorization is the decision process that allows or denies an authenticated request.
 It evaluates request attributes against one or more authorization modules such as RBAC.

- **anonymous access** 
 Anonymous access is the unauthenticated request path when enabled or not otherwise rejected.
 It should generally be minimized because unauthenticated requests still reach authorization evaluation.

### RBAC
Role-Based Access Control is the standard authorization model for Kubernetes API operations.
It binds subjects to roles that contain allowed verbs over resources and subresources.

- **Role** 
 A Role is a namespaced permission object in RBAC.
 It grants allowed verbs over resources within one namespace scope.

- **ClusterRole** 
 A ClusterRole is a cluster-scoped RBAC permission object.
 It can grant permissions over cluster-scoped resources or be bound into namespaces as reusable permission logic.

- **RoleBinding** 
 A RoleBinding grants a Role or ClusterRole to subjects within a namespace.
 Its effect is limited to that namespace even when it references a ClusterRole.

- **ClusterRoleBinding** 
 A ClusterRoleBinding grants a ClusterRole across the whole cluster.
 It is used for cluster-wide permissions and should be tightly controlled.

### Admission Control
Admission control is the API-stage policy and mutation layer that executes after authentication and authorization but before persistence.
It can reject, mutate, or validate objects according to built-in or external admission logic.

- **mutating admission** 
 Mutating admission plugins can modify incoming objects before they are stored.
 They commonly inject defaults, sidecars, labels, or policy-driven configuration.

- **validating admission** 
 Validating admission plugins inspect incoming objects and either allow or reject them.
 They do not mutate the object and are used for policy enforcement and schema-adjacent checks.

- **admission webhooks** 
 Admission webhooks are external HTTP callbacks used for mutating or validating API requests.
 They extend admission behavior beyond built-in plugins and must be designed carefully for latency and failure handling.

- **Pod Security Admission** 
 Pod Security Admission is the built-in admission mechanism that enforces Pod Security Standards at namespace level.
 It labels namespaces with enforce, audit, and warn modes for privileged, baseline, or restricted policy levels.

### Workload Security Context
Security context settings control privilege-related behavior for Pods and containers at runtime.
They shape Linux user identity, filesystem permissions, privilege escalation, syscall filtering, and capability handling.

- **Pod security context** 
 A Pod security context applies security defaults across containers in a Pod.
 It defines settings such as FSGroup and supplemental groups at Pod scope.

- **container security context** 
 A container security context applies privilege and runtime settings to an individual container.
 It can override or refine Pod-level defaults for that container.

- **runAsNonRoot** 
 `runAsNonRoot` requires the container to run without UID 0 at runtime.
 It is a safeguard against accidental root execution when images or configs are misaligned.

- **readOnlyRootFilesystem** 
 `readOnlyRootFilesystem` mounts the container root filesystem read-only.
 It reduces post-exploitation write surface and forces mutable state onto explicit writable mounts.

- **allowPrivilegeEscalation** 
 `allowPrivilegeEscalation` controls whether a process can gain more privileges than its parent.
 Disabling it helps limit privilege gains from setuid binaries or similar mechanisms.

- **Linux capabilities** 
 Linux capabilities are fine-grained kernel privileges that can be added or dropped per container.
 Dropping unnecessary capabilities reduces the attack surface compared with full root privilege.

### Kernel and Runtime Isolation
Kernel and runtime isolation features reduce what containers can do even when compromised.
These controls work below the Kubernetes API layer and depend on node OS and runtime support.

- **seccomp** 
 seccomp is the Linux syscall filtering mechanism used to restrict available system calls to a container.
 Kubernetes can apply a runtime default profile or a custom seccomp profile through security context settings.

- **AppArmor** 
 AppArmor is a Linux security module that applies path- and capability-aware confinement profiles to processes.
 Kubernetes can reference preloaded AppArmor profiles to restrict container behavior on supporting nodes.

- **SELinux** 
 SELinux is a Linux mandatory access control system that labels processes and files with security contexts.
 Kubernetes can pass SELinux options through security context to support policy-enforced workload confinement on compatible systems.

- **privileged containers** 
 Privileged containers run with elevated kernel access and weakened isolation boundaries.
 They should be minimized because they can access host devices, namespaces, and sensitive kernel interfaces.

### Network and Transport Security
Network and transport security protects API traffic, service communication paths, and east-west reachability.
It relies on TLS, network segmentation, and implementation-specific mechanisms such as service meshes or mTLS overlays.

- **TLS for API server** 
 TLS for the API server protects client-to-control-plane communication.
 It provides encryption and server authentication and is typically combined with client-certificate or token-based auth.

- **mTLS overlays** 
 mTLS overlays are implementation-specific patterns that provide mutual TLS between workloads.
 They are not a native Kubernetes feature but are commonly implemented through service-mesh or sidecar data planes.

- **NetworkPolicy segmentation** 
 NetworkPolicy segmentation restricts Pod communication paths by declared allow rules.
 It reduces blast radius by limiting lateral movement between namespaces and workloads.

### Audit and Visibility
Audit and visibility features provide traceability for API actions and workload security state.
They are critical for incident response, compliance review, and detection of unauthorized control-plane operations.

- **audit logging** 
 Audit logging is the API-server feature that records request metadata and optionally object contents for API activity.
 It is configured by policy and can emit staged events such as request received, response started, and response complete.

- **events** 
 Events are lightweight, short-lived records of notable cluster actions or failures.
 They support troubleshooting but are not a durable security audit source.

### Multi-tenancy and Isolation Boundaries
Kubernetes is not a hard multi-tenant hypervisor boundary by default.
Safe multi-tenancy requires combining namespace isolation, RBAC, network policy, node separation, and workload hardening.

- **soft multi-tenancy** 
 Soft multi-tenancy is the model in which multiple teams share a cluster under policy and organizational controls.
 It assumes some degree of trust and uses namespaces, quotas, and RBAC as primary boundaries.

- **harder isolation patterns** 
 Harder isolation patterns use dedicated node pools, stricter admission, minimal privileges, and stronger runtime controls.
 They reduce cross-tenant risk but still may not equal the security boundary of separate clusters or VMs.

---

## Observability and Operations

Kubernetes operations depend on visibility into object state, node health, workload behavior, and control-plane activity.
Observability spans metrics, logs, traces, events, and object inventory rather than a single built-in subsystem.

### Metrics
Metrics quantify the health and performance of cluster components and workloads over time.
They are essential for alerting, autoscaling, capacity planning, and SLO-driven operations.

- **resource metrics** 
 Resource metrics are CPU and memory usage signals collected for nodes and Pods.
 They are commonly exposed through metrics-server for lightweight autoscaling and CLI visibility.

- **application metrics** 
 Application metrics are workload-defined time-series exported by applications or sidecars.
 They are typically scraped by Prometheus-compatible systems rather than stored natively by Kubernetes.

- **kube-state-metrics** 
 kube-state-metrics is an external exporter that turns Kubernetes object state into Prometheus metrics.
 It is not part of Kubernetes core and should be understood as an observability add-on rather than a control-plane component.

### Logs
Logs provide textual or structured event details from applications and system components.
Kubernetes itself does not include a full cluster-wide logging backend and expects external aggregation systems.

- **container stdout and stderr** 
 Container stdout and stderr are the default logging streams captured by the node runtime and kubelet integration.
 They provide the standard log source for most application containers in Kubernetes.

- **node logs** 
 Node logs are the host-level and system-component logs generated by kubelet, runtime, kernel, and OS services.
 They are essential for diagnosing scheduling, mount, CNI, and runtime failures that are invisible inside the Pod.

### Events and Status Inspection
Events and status inspection help operators understand recent state transitions without querying raw logs first.
They are especially useful for identifying scheduling failures, image pull problems, probe failures, and policy denials.

- **kubectl describe** 
 `kubectl describe` is the inspection workflow that shows object details, status, recent events, and linked references.
 It is often the fastest way to diagnose why a Pod or controller is not progressing.

- **events stream** 
 The events stream is the short-lived cluster record of notable state changes and warnings.
 It is useful for troubleshooting but can age out quickly and should not be treated as a long-term audit source.

### Cluster Lifecycle Operations
Cluster lifecycle operations change cluster topology, versions, and workload placement over time.
They must be coordinated carefully to preserve availability and policy conformance.

- **cordon** 
 Cordon marks a node unschedulable for new Pods.
 Existing Pods remain running until they terminate naturally or are evicted.

- **drain** 
 Drain is the administrative operation that evicts or deletes Pods from a node prior to maintenance.
 It respects PodDisruptionBudgets for evictable workloads and ignores some unmanaged or daemonized Pods unless overridden.

- **uncordon** 
 Uncordon removes the unschedulable mark from a node.
 The scheduler can then place new Pods on that node again.

### Failure Domains
Failure domains are the infrastructure or logical boundaries across which failures can correlate.
Good workload design spreads replicas across these domains to reduce blast radius.

- **node failure domain** 
 The node failure domain is the individual machine or VM boundary for Pod execution.
 A node outage removes all non-redundant Pods scheduled there until controllers replace them elsewhere.

- **zone failure domain** 
 The zone failure domain is the datacenter or availability-zone boundary underlying cluster nodes.
 Highly available workloads should spread across zones when the cluster and storage topology support it.

- **control-plane failure domain** 
 The control-plane failure domain is the boundary across which API availability and state mutation can be impacted together.
 It includes API server, etcd, and network paths required for cluster management.

---

## Packaging and Policy

Kubernetes platforms often add packaging, policy, and lifecycle abstractions above raw manifests.
These tools standardize rollout, validation, dependency management, and reconciliation across large estates.

### Helm
Helm is the package manager for templated Kubernetes application delivery.
It renders parameterized charts into Kubernetes manifests and applies them as a release.

- **chart** 
 A chart is the package format used by Helm.
 It contains templates, default values, metadata, and optional dependencies for an application or platform component.

- **release** 
 A release is an installed instance of a Helm chart in a cluster.
 It tracks rendered manifests and revision history for upgrade and rollback operations.

- **values** 
 Values are the input data used to parameterize chart templates.
 They are merged from defaults, files, and CLI overrides before rendering.

### Operators
Operators are controller-based software that encode operational knowledge for a specific application domain.
They typically combine custom resources with reconciliation logic to automate lifecycle tasks.

- **custom controller** 
 A custom controller is a reconciliation loop written for a specific custom or built-in resource type.
 It watches objects and drives external or internal side effects until desired state is achieved.

- **operator pattern** 
 The operator pattern is the use of Kubernetes APIs plus controllers to manage complex applications declaratively.
 It turns application operations such as backup, failover, and scaling into API-driven reconciliation logic.

### Policy Tooling
Policy tooling extends or formalizes governance beyond built-in admission and RBAC.
It is commonly used to enforce platform standards, tenancy restrictions, and security baselines consistently.

- **validating admission webhook** 
 A validating admission webhook is an external policy endpoint that approves or rejects API requests.
 It is often used for cross-object or organization-specific policy that built-in validation cannot express.

- **mutating admission webhook** 
 A mutating admission webhook is an external policy endpoint that rewrites incoming objects before storage.
 It is commonly used for label injection, sidecar injection, and defaulting behavior.

- **policy engines** 
 Policy engines are controller or admission systems that evaluate rules against Kubernetes resources.
 They are typically deployed as add-ons rather than as part of Kubernetes core.

---

## Advanced Concepts

Advanced Kubernetes use involves extensibility, rollout safety, API evolution, and platform boundary design.
These topics are central for platform engineering, cluster productization, and operating large multi-team environments.

### Custom Resources and Controllers
Custom resources let teams define their own declarative APIs inside Kubernetes.
Controllers then give those resources meaning by reconciling them into running state or external infrastructure.

- **custom resource** 
 A custom resource is an instance of a type registered by a CRD.
 It behaves like a native Kubernetes object from the client perspective but requires a controller for functional reconciliation.

- **status subresource** 
 The status subresource is the API path used for controller-written observed state updates.
 It allows status mutation without giving a client write access to the full object spec.

- **conversion webhook** 
 A conversion webhook translates custom resources between served API versions.
 It preserves compatibility as schemas evolve while maintaining a chosen storage version.

### Rollout Safety and Progressive Delivery
Rollout safety mechanisms reduce the risk of application outages during change.
Kubernetes provides some native controls, while more advanced progressive delivery usually relies on additional controllers.

- **max surge and max unavailable** 
 Max surge and max unavailable define how aggressively a Deployment may add and remove Pods during rollout.
 They balance rollout speed against service availability.

- **partitioned StatefulSet updates** 
 Partitioned StatefulSet updates let operators hold back updates for lower ordinals while advancing higher ones.
 They provide more controlled rollout behavior for ordered stateful workloads.

- **canary and blue-green patterns** 
 Canary and blue-green patterns are deployment strategies that shift traffic gradually or switch between environments.
 They are not fully native primitives in Kubernetes and usually depend on ingress, service mesh, or deployment tooling.

### Multi-cluster and Platform Boundaries
Some workloads or organizations outgrow a single-cluster operational model.
Multiple clusters are used for blast-radius control, compliance separation, region placement, or workload-class isolation.

- **cluster as failure domain** 
 The cluster itself is a major operational failure domain in Kubernetes.
 Separate clusters reduce control-plane and shared-policy blast radius at the cost of higher management overhead.

- **federation boundary** 
 Federation boundary refers to the point at which policy, identity, and deployment coordination span multiple clusters.
 Kubernetes core does not provide a universal multi-cluster control plane, so implementations vary widely.

### Common Failure Modes
Kubernetes failures often arise from the interaction between API objects, controllers, node agents, and infrastructure dependencies.
Studying failure patterns is critical for accurate troubleshooting and resilient design.

- **Pending Pods** 
 Pending Pods are Pods that have been accepted by the API but are not yet running on a node.
 Common causes include unschedulable constraints, missing volumes, or insufficient cluster resources.

- **CrashLoopBackOff** 
 CrashLoopBackOff is the backoff state shown when a container repeatedly starts and exits unsuccessfully.
 It usually indicates application failure, bad configuration, missing dependencies, or failing startup logic.

- **ImagePullBackOff** 
 ImagePullBackOff is the backoff state shown when the kubelet cannot pull a container image successfully.
 Common causes include authentication failure, image name errors, registry unreachability, or rate limiting.

- **NotReady node** 
 A NotReady node is a node whose kubelet or status conditions indicate it is not fit for normal scheduling or operation.
 Causes include network partition, kubelet failure, runtime failure, disk pressure, or lost heartbeats.

- **evicted Pod** 
 An evicted Pod is a Pod terminated by the kubelet due to node pressure or policy-driven eviction.
 It is not a normal application exit and usually indicates capacity or resource-management problems.

---
## Study Framing

These notes are optimized for certification review, architecture analysis, and platform engineering study.
They are intentionally strict about separating API object, controller, control-plane component, node agent, data-plane behavior, and policy boundary.
