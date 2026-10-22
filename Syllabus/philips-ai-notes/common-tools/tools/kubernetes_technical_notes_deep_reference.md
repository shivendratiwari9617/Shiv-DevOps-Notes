# Kubernetes Technical Notes — Deep Practical Reference

Technical reference generated in one pass from the practical Kubernetes concept set. Each point is covered independently. Conceptual and operational points focus on definition, mechanics, production behavior, verification, and failure boundaries. YAML is included only where object structure or manifest syntax is directly useful.

## 1. Kubernetes Foundations

### 1. Kubernetes
- Definition: Kubernetes is a declarative control-plane system for running and operating containerized workloads across a cluster.
- Core mechanics: users write desired state to the API; controllers, scheduler, and node agents reconcile actual state toward that target.
- Primary abstractions: Pods run containers, Services expose stable endpoints, controllers maintain replica intent, and storage/network plugins integrate infrastructure.
- Scheduling model: workloads are placed based on resource requests, constraints, topology, taints, and policy.
- Recovery model: failed Pods are recreated by controllers, failed nodes are handled through rescheduling and node lifecycle logic, and rollouts are controller-driven.
- Production use: Kubernetes becomes the contract between platform teams and workloads, so API stability, RBAC, policy, observability, and upgrade discipline matter as much as application code.
- Verification: confirm cluster health by checking API reachability, node readiness, critical control-plane components, DNS, and storage plugin health.
- Failure pattern: most real incidents come from bad configuration, missing resource requests, policy gaps, storage/network integration issues, and unsafe operational changes.

### 2. Purpose of Kubernetes
- Definition: Kubernetes exists to automate application lifecycle operations across a cluster, replacing host-by-host management with API-driven control loops.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 3. Problems solved by Kubernetes
- Definition: Kubernetes solves service discovery, load balancing, rollout automation, self-healing, scheduling, configuration distribution, resource isolation, and horizontal scaling for containerized applications.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 4. Containers vs virtual machines
- Definition: Containers isolate processes while sharing the host kernel; virtual machines emulate full guest operating systems on a hypervisor and usually have stronger isolation with higher overhead.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 5. Container images
- Definition: A container image is an immutable package containing application binaries, runtime dependencies, filesystem content, and startup metadata.
- Content model: images reference manifests and layers; tags are mutable references, while digests pin exact content.
- Runtime path: kubelet asks the runtime to resolve the image reference, authenticate if required, pull missing layers, and create the container filesystem.
- Production use: pin critical workloads to immutable versions, manage registry auth explicitly, and keep build provenance separate from deployment environment data.
- Verification: confirm the resolved image ID on the running Pod, pull events, registry reachability, and whether the manifest matches the intended release artifact.
- Failure modes: mutable tags, registry auth errors, rate limits, corrupted layers, and architecture mismatches between image and node.

### 6. Container registries
- Definition: A container registry stores and serves image manifests and layers so runtimes can pull versioned application images.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 7. Image tags
- Definition: An image tag is a mutable reference to an image manifest, often used as a release label such as v1.2.0 or latest.
- Content model: images reference manifests and layers; tags are mutable references, while digests pin exact content.
- Runtime path: kubelet asks the runtime to resolve the image reference, authenticate if required, pull missing layers, and create the container filesystem.
- Production use: pin critical workloads to immutable versions, manage registry auth explicitly, and keep build provenance separate from deployment environment data.
- Verification: confirm the resolved image ID on the running Pod, pull events, registry reachability, and whether the manifest matches the intended release artifact.
- Failure modes: mutable tags, registry auth errors, rate limits, corrupted layers, and architecture mismatches between image and node.

### 8. Image digests
- Definition: An image digest is an immutable content hash of an image manifest and is the safest reference for reproducible deployments.
- Content model: images reference manifests and layers; tags are mutable references, while digests pin exact content.
- Runtime path: kubelet asks the runtime to resolve the image reference, authenticate if required, pull missing layers, and create the container filesystem.
- Production use: pin critical workloads to immutable versions, manage registry auth explicitly, and keep build provenance separate from deployment environment data.
- Verification: confirm the resolved image ID on the running Pod, pull events, registry reachability, and whether the manifest matches the intended release artifact.
- Failure modes: mutable tags, registry auth errors, rate limits, corrupted layers, and architecture mismatches between image and node.

### 9. Linux container isolation
- Definition: Container isolation in Linux is implemented primarily with namespaces, cgroups, capabilities, seccomp, and layered filesystems.
- Kernel role: this is part of the Linux isolation model that containers depend on underneath Kubernetes abstractions.
- Runtime path: the container runtime configures these kernel primitives when creating the Pod sandbox and individual containers.
- Production use: isolation quality depends on runtime configuration, kernel support, capabilities, seccomp, and host-level hardening rather than on Kubernetes YAML alone.
- Verification: inspect runtime behavior, process visibility, mount namespace behavior, cgroup accounting, and node-level security settings.
- Failure modes: privileged containers, host namespace usage, weak seccomp or capability settings, and host kernel constraints reducing isolation.

### 10. Linux namespaces
- Definition: Linux namespaces isolate process IDs, network interfaces, mounts, users, IPC objects, and hostnames so a container sees its own scoped view of the system.
- Kernel role: this is part of the Linux isolation model that containers depend on underneath Kubernetes abstractions.
- Runtime path: the container runtime configures these kernel primitives when creating the Pod sandbox and individual containers.
- Production use: isolation quality depends on runtime configuration, kernel support, capabilities, seccomp, and host-level hardening rather than on Kubernetes YAML alone.
- Verification: inspect runtime behavior, process visibility, mount namespace behavior, cgroup accounting, and node-level security settings.
- Failure modes: privileged containers, host namespace usage, weak seccomp or capability settings, and host kernel constraints reducing isolation.

### 11. Linux cgroups
- Definition: Linux cgroups account for and enforce CPU, memory, I/O, and process limits for groups of processes.
- Kernel role: this is part of the Linux isolation model that containers depend on underneath Kubernetes abstractions.
- Runtime path: the container runtime configures these kernel primitives when creating the Pod sandbox and individual containers.
- Production use: isolation quality depends on runtime configuration, kernel support, capabilities, seccomp, and host-level hardening rather than on Kubernetes YAML alone.
- Verification: inspect runtime behavior, process visibility, mount namespace behavior, cgroup accounting, and node-level security settings.
- Failure modes: privileged containers, host namespace usage, weak seccomp or capability settings, and host kernel constraints reducing isolation.

### 12. Container filesystems
- Definition: Container filesystems are usually layered union filesystems where read-only image layers are combined with a writable runtime layer.
- Kernel role: this is part of the Linux isolation model that containers depend on underneath Kubernetes abstractions.
- Runtime path: the container runtime configures these kernel primitives when creating the Pod sandbox and individual containers.
- Production use: isolation quality depends on runtime configuration, kernel support, capabilities, seccomp, and host-level hardening rather than on Kubernetes YAML alone.
- Verification: inspect runtime behavior, process visibility, mount namespace behavior, cgroup accounting, and node-level security settings.
- Failure modes: privileged containers, host namespace usage, weak seccomp or capability settings, and host kernel constraints reducing isolation.

### 13. Declarative model
- Definition: The declarative model means the user submits the desired end state to the API and controllers continuously act to make actual state match it.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 14. Desired state
- Definition: Desired state is the target configuration stored in an object's spec and used by controllers as the source of truth.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 15. Reconciliation loop
- Definition: A reconciliation loop repeatedly compares desired state with observed state and performs API actions to reduce drift.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 16. Self-healing
- Definition: Self-healing is Kubernetes behavior that recreates failed Pods, reschedules workloads, and restores declared replica count after faults.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 17. Scaling
- Definition: Scaling is the controlled increase or decrease of workload capacity by changing replica count, resource sizing, or node count.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 18. High availability
- Definition: High availability is the design goal of keeping control-plane functions and applications available despite instance, node, or zone failures.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 19. Failure domains
- Definition: A failure domain is a boundary such as a host, rack, zone, or region within which a single fault can affect multiple components.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 20. Managed vs self-managed clusters
- Definition: Managed clusters outsource most control-plane lifecycle tasks to a provider, while self-managed clusters require direct administration of installation, PKI, upgrades, backups, and recovery.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

## 2. Cluster Architecture

### 21. Control plane
- Definition: The control plane is the set of components that expose the API, persist cluster state, schedule Pods, and run reconciliation controllers.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 22. Worker node responsibilities
- Definition: Worker nodes run Pods, enforce container isolation, attach storage, configure Pod networking, and report health back to the control plane.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 23. kube-apiserver
- Definition: kube-apiserver is the front end of Kubernetes that authenticates requests, authorizes actions, runs admission, validates objects, and persists state.
- Core mechanics: the component participates in the control loop by either persisting state, making scheduling decisions, reconciling objects, or enforcing node/runtime behavior.
- Data path: understand what the component watches, what state it mutates, and which external systems or plugins it depends on.
- Production considerations: availability, certificates, network reachability, RBAC, and configuration flags determine whether the component behaves predictably under load.
- Verification: inspect health endpoints, logs, leader election state where applicable, and whether downstream objects reflect expected actions.
- Failure modes: time skew, certificate expiry, unreachable dependencies, bad flags, and resource starvation often present as secondary symptoms elsewhere in the cluster.

### 24. etcd
- Definition: etcd is the strongly consistent key-value store that persists Kubernetes cluster state and configuration.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 25. kube-scheduler
- Definition: kube-scheduler selects a node for each unscheduled Pod by filtering feasible nodes and scoring the best fit.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

### 26. kube-controller-manager
- Definition: kube-controller-manager runs built-in controllers that reconcile objects such as Deployments, ReplicaSets, Jobs, and Nodes.
- Core mechanics: the component participates in the control loop by either persisting state, making scheduling decisions, reconciling objects, or enforcing node/runtime behavior.
- Data path: understand what the component watches, what state it mutates, and which external systems or plugins it depends on.
- Production considerations: availability, certificates, network reachability, RBAC, and configuration flags determine whether the component behaves predictably under load.
- Verification: inspect health endpoints, logs, leader election state where applicable, and whether downstream objects reflect expected actions.
- Failure modes: time skew, certificate expiry, unreachable dependencies, bad flags, and resource starvation often present as secondary symptoms elsewhere in the cluster.

### 27. kubelet
- Definition: kubelet is the node agent that starts containers through the runtime, mounts volumes, runs probes, and reports Pod and node status.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 28. kube-proxy
- Definition: kube-proxy programs node-level forwarding rules so Service virtual IP traffic reaches healthy backend Pods.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 29. Control plane endpoint
- Definition: The control plane endpoint is the stable address used by clients and nodes to reach the API server, usually behind a load balancer in HA clusters.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 30. Highly available cluster design
- Definition: Highly available cluster design uses redundant control-plane instances, resilient etcd topology, and a stable API endpoint to remove single points of failure.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 31. Worker node components
- Definition: Worker node components usually include kubelet, a container runtime, CNI plugins, CSI node plugins, kube-proxy, and host networking utilities.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 32. Container runtime
- Definition: A container runtime pulls images, creates sandboxes and containers, starts processes, and exposes the CRI interface to kubelet.
- Core mechanics: the component participates in the control loop by either persisting state, making scheduling decisions, reconciling objects, or enforcing node/runtime behavior.
- Data path: understand what the component watches, what state it mutates, and which external systems or plugins it depends on.
- Production considerations: availability, certificates, network reachability, RBAC, and configuration flags determine whether the component behaves predictably under load.
- Verification: inspect health endpoints, logs, leader election state where applicable, and whether downstream objects reflect expected actions.
- Failure modes: time skew, certificate expiry, unreachable dependencies, bad flags, and resource starvation often present as secondary symptoms elsewhere in the cluster.

### 33. East-west vs north-south traffic
- Definition: East-west traffic is service-to-service communication inside the platform, while north-south traffic enters or leaves the cluster from external clients or systems.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 34. Single-cluster model
- Definition: A single-cluster model keeps workloads, policy, and operations within one Kubernetes control-plane boundary.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 35. Multi-cluster model
- Definition: A multi-cluster model distributes environments, tenants, regions, or workloads across separate clusters for isolation, scale, or locality.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 36. Private cluster pattern
- Definition: A private cluster pattern restricts API server or node exposure to private networks and controlled access paths rather than public internet endpoints.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 37. Hybrid cluster pattern
- Definition: A hybrid cluster pattern spans on-premises and cloud-connected environments while keeping a consistent Kubernetes operational model.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 38. Edge cluster patterns
- Definition: Edge cluster patterns place compute near users or devices to reduce latency and tolerate constrained or intermittently connected environments.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

## 3. Cluster Bootstrapping and Access

### 39. kubeadm init
- Definition: kubeadm init bootstraps the initial control plane by generating PKI assets, writing static Pod manifests, and creating cluster configuration.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 40. kubeadm join
- Definition: kubeadm join adds a worker or control-plane node to an existing cluster by authenticating bootstrap credentials and configuring kubelet.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 41. Cluster bootstrap flow
- Definition: Cluster bootstrap flow is the sequence of creating trust material, starting etcd and API components, launching controllers, and registering nodes.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 42. Static Pod control plane bootstrap
- Definition: In kubeadm-based clusters, core control-plane components are commonly launched as static Pods supervised directly by kubelet.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 43. Bootstrap tokens
- Definition: Bootstrap tokens are short-lived credentials used by joining nodes to authenticate to the API server during enrollment.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 44. Node join trust model
- Definition: The node join trust model combines bootstrap authentication, CA pinning, and certificate signing to establish trusted node identity.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 45. kubeconfig structure
- Definition: A kubeconfig file stores named clusters, users, and contexts so a client can select an API endpoint and authentication method.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 46. kubeconfig cluster entries
- Definition: A kubeconfig cluster entry defines the API server address and trust material such as the certificate authority.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 47. kubeconfig user entries
- Definition: A kubeconfig user entry defines client credentials such as certificates, bearer tokens, or external authentication plugins.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 48. kubeconfig contexts
- Definition: A kubeconfig context binds one cluster and one user, optionally with a default namespace.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 49. CNI installation after bootstrap
- Definition: A CNI plugin must be installed after bootstrap so Pods can receive network interfaces, IP addresses, and inter-Pod routing.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 50. Bare-metal vs managed provisioning
- Definition: Bare-metal provisioning requires direct setup of hosts, networking, load balancing, storage integration, and OS lifecycle, while managed provisioning abstracts most of that work.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

## 4. Kubernetes API and Object Model

### 51. API groups
- Definition: API groups logically organize resources such as apps, batch, and networking.k8s.io, and allow Kubernetes to version related APIs independently.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 52. API versions
- Definition: An API version identifies a concrete schema and lifecycle stage for a resource representation, such as v1 or apps/v1.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 53. Kinds
- Definition: A Kind is the schema name of an API object such as Pod, Deployment, or Service.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 54. Resources
- Definition: A resource is the REST-exposed plural endpoint of a Kind, such as pods or deployments.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 55. API objects
- Definition: An API object is a persisted instance of a Kubernetes resource stored through the API server.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 56. Namespaced vs cluster-scoped resources
- Definition: Namespaced resources exist within a namespace boundary, while cluster-scoped resources exist once per cluster and are not namespaced.
- Kernel role: this is part of the Linux isolation model that containers depend on underneath Kubernetes abstractions.
- Runtime path: the container runtime configures these kernel primitives when creating the Pod sandbox and individual containers.
- Production use: isolation quality depends on runtime configuration, kernel support, capabilities, seccomp, and host-level hardening rather than on Kubernetes YAML alone.
- Verification: inspect runtime behavior, process visibility, mount namespace behavior, cgroup accounting, and node-level security settings.
- Failure modes: privileged containers, host namespace usage, weak seccomp or capability settings, and host kernel constraints reducing isolation.

### 57. Object structure
- Definition: Most Kubernetes objects contain apiVersion, kind, metadata, spec, and status fields.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
spec:
  containers:
    - name: nginx
      image: nginx:1.27
status:
  phase: Running
```

### 58. Object metadata
- Definition: metadata stores identity and control fields such as name, namespace, labels, annotations, UID, and resourceVersion.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
metadata:
  name: web
  namespace: prod
  labels:
    app: web
```

### 59. Object spec
- Definition: spec stores the desired configuration declared by a user or controller.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
```

### 60. Object status
- Definition: status stores controller-observed state, conditions, and runtime information.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
status:
  phase: Running
  conditions:
    - type: Available
      status: "True"
```

### 61. Labels
- Definition: Labels are indexed key-value pairs used for grouping, selection, scheduling, and automation.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
metadata:
  labels:
    app: api
    env: prod
```

### 62. Selectors
- Definition: Selectors are label-based queries used by controllers and Services to target matching objects.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
selector:
  matchLabels:
    app: api
```

### 63. Annotations
- Definition: Annotations are non-indexed key-value metadata used to attach descriptive or tool-specific information to objects.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
```

### 64. OwnerReferences
- Definition: OwnerReferences declare object ownership so Kubernetes can track dependents and perform cascading deletion.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
metadata:
  ownerReferences:
    - apiVersion: apps/v1
      kind: ReplicaSet
      name: web-6b7d
      uid: 11111111-2222-3333-4444-555555555555
```

### 65. Finalizers
- Definition: Finalizers are metadata keys that block deletion until a controller completes required cleanup.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
metadata:
  finalizers:
    - kubernetes.io/pvc-protection
```

### 66. Garbage collection
- Definition: Garbage collection removes dependents or orphaned objects based on ownership and deletion policy.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 67. resourceVersion
- Definition: resourceVersion is an opaque change token used for optimistic concurrency and watch continuity.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 68. generation
- Definition: generation increments when the desired state of an object changes.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 69. observedGeneration
- Definition: observedGeneration records the latest generation processed by the responsible controller.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 70. YAML manifest structure
- Definition: A Kubernetes manifest serializes an API object in YAML, typically using apiVersion, kind, metadata, and spec.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 2
```

### 71. API validation
- Definition: API validation checks whether submitted objects satisfy schema, type, and admission constraints before persistence.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 72. API defaulting
- Definition: API defaulting fills unspecified fields with API-defined default values during request processing.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 73. List operations
- Definition: A list operation returns the current set of objects for a resource type at a point in time.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 74. Watch operations
- Definition: A watch operation streams object change events after a resourceVersion for near-real-time state observation.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 75. Patch operations
- Definition: A patch operation updates selected object fields without replacing the full manifest.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 76. Server-side apply
- Definition: Server-side apply merges declarative field ownership on the API server so multiple actors can manage different parts of the same object.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

## 5. Authentication, Authorization, and Admission

### 77. Authentication methods
- Definition: Authentication methods verify caller identity before authorization and may use certificates, tokens, OIDC, or webhooks.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 78. Client certificates
- Definition: Client certificates identify users or components with x509 credentials signed by a trusted certificate authority.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 79. Authentication tokens
- Definition: Authentication tokens are bearer credentials presented to the API server, commonly for service accounts, bootstrap, or external identity providers.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 80. OIDC authentication
- Definition: OIDC authentication integrates the API server with an OpenID Connect identity provider using signed identity tokens.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 81. Webhook integration
- Definition: Webhook integration delegates authentication or admission decisions to an external HTTP service.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 82. Authorization with RBAC focus
- Definition: Authorization determines what an authenticated identity may do, and RBAC is the standard rule-based permission model in Kubernetes.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 83. Role
- Definition: A Role defines allowed verbs on resources within a single namespace.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

YAML syntax example:

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
```

### 84. ClusterRole
- Definition: A ClusterRole defines permissions that can apply cluster-wide or be bound into individual namespaces.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

YAML syntax example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
```

### 85. RoleBinding
- Definition: A RoleBinding grants a Role or ClusterRole to users, groups, or service accounts within one namespace.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

YAML syntax example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
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

### 86. ClusterRoleBinding
- Definition: A ClusterRoleBinding grants a ClusterRole across the entire cluster.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

YAML syntax example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-read-pods
subjects:
  - kind: User
    name: alice
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: pod-reader
```

### 87. ServiceAccount
- Definition: A ServiceAccount is a Kubernetes identity assigned to Pods for API access and workload authentication.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

YAML syntax example:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app-sa
  namespace: prod
```

### 88. Workload identity
- Definition: Workload identity is the mechanism by which a running Pod proves its identity to the Kubernetes API or to external systems.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 89. Admission control
- Definition: Admission control mutates or validates API requests after authentication and authorization but before persistence.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 90. Built-in admission plugins
- Definition: Built-in admission plugins are compiled into the API server and enforce standard behaviors such as namespace lifecycle or resource quotas.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 91. Admission webhooks
- Definition: Admission webhooks are external services that mutate or validate Kubernetes API requests.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 92. Pod Security Standards
- Definition: Pod Security Standards define policy profiles such as privileged, baseline, and restricted for workload security controls.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

YAML syntax example:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: prod
  labels:
    pod-security.kubernetes.io/enforce: restricted
```

### 93. Namespace-level security enforcement
- Definition: Namespace-level security enforcement applies policy to all matching workloads in a namespace, commonly through admission labels or policy controllers.
- Kernel role: this is part of the Linux isolation model that containers depend on underneath Kubernetes abstractions.
- Runtime path: the container runtime configures these kernel primitives when creating the Pod sandbox and individual containers.
- Production use: isolation quality depends on runtime configuration, kernel support, capabilities, seccomp, and host-level hardening rather than on Kubernetes YAML alone.
- Verification: inspect runtime behavior, process visibility, mount namespace behavior, cgroup accounting, and node-level security settings.
- Failure modes: privileged containers, host namespace usage, weak seccomp or capability settings, and host kernel constraints reducing isolation.

### 94. Audit logging
- Definition: Audit logging records API requests, user identity, verbs, targets, and response stages for security and operational analysis.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 95. API request tracing
- Definition: API request tracing correlates request flow and latency across components to identify slow operations and bottlenecks.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

## 6. etcd and Control Plane State

### 96. etcd role in Kubernetes
- Definition: etcd stores the authoritative cluster state that the API server reads and writes for all Kubernetes objects.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 97. Why etcd matters
- Definition: etcd health directly affects API consistency, cluster recovery, leader election, and the ability of controllers to observe current state.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 98. Raft consensus
- Definition: Raft is the consensus algorithm etcd uses to replicate writes and maintain strongly consistent state across members.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 99. Quorum
- Definition: Quorum is the minimum majority of members required to commit writes safely in a distributed control-plane datastore.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 100. Raft leader
- Definition: The Raft leader is the member that accepts writes and replicates them to follower members.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 101. Consistency
- Definition: Consistency means committed state changes are observed in the same order by cluster members and clients according to the datastore read model in use.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 102. etcd snapshots
- Definition: An etcd snapshot is a point-in-time backup of the key-value database used for recovery.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 103. etcd restore
- Definition: etcd restore rebuilds datastore state from a snapshot and is used when the live etcd state is lost, corrupted, or unrecoverable.
- Restore mechanics: snapshot restore writes a new data directory and rewrites member metadata rather than modifying the existing live data directory in place.
- Control-plane impact: the API server must point to the restored etcd instance or cluster, so manifest or configuration changes are usually required.
- HA consideration: restoring one member is not the same as rejoining it to an existing healthy cluster; follow topology-specific recovery procedures.
- Verification: check member list, endpoint health, alarms, API server startup, and whether Kubernetes objects are visible and internally consistent after restore.
- Risk: restoring to an old snapshot reverts cluster state to that point in time, so later object changes are lost unless reapplied from another source of truth such as GitOps.
- Production rule: rehearse restore procedures regularly because a snapshot without a tested restore path is not a real backup strategy.

### 104. etcd disaster recovery
- Definition: etcd disaster recovery covers backup validation, restore procedures, member replacement, and control-plane reconfiguration after severe failure.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 105. etcd peer and client TLS
- Definition: Peer TLS secures communication between etcd members, while client TLS secures connections from API servers and admin tools to etcd.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 106. etcd health checks
- Definition: etcd health checks verify endpoint responsiveness, leader availability, quorum health, and database status.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 107. etcd compaction
- Definition: etcd compaction removes superseded revision history to control database growth.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 108. etcd defragmentation
- Definition: etcd defragmentation rewrites the backend database to reclaim space after compaction.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

### 109. etcd operational care
- Definition: etcd operational care includes backup, certificate rotation, latency monitoring, storage management, compaction, and defragmentation.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

## 7. Pods and Workload Controllers

### 110. Pod lifecycle
- Definition: The Pod lifecycle includes creation, scheduling, image pull, container start, readiness, running, termination, and final deletion.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 111. Container restart behavior
- Definition: Container restart behavior is controlled by exit status and restartPolicy, with kubelet restarting failed containers when policy allows.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 112. Multi-container Pods
- Definition: A multi-container Pod runs tightly coupled containers that share the same network namespace and volumes.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 113. Init containers
- Definition: Init containers run to completion before application containers start and are used for setup, validation, or dependency preparation.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

### 114. Sidecar pattern
- Definition: A sidecar is a companion container in the same Pod that provides support functions such as proxying, log shipping, or config reload.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 115. ReplicaSet
- Definition: A ReplicaSet ensures that a specified number of matching Pods exist at all times.
- API shape: the object is declared in YAML and persisted through the Kubernetes API, where controllers or system components act on its spec.
- Key behavior: metadata identifies the object, spec carries desired state, and status or events reveal observed runtime state when applicable.
- Production use: define labels, ownership, security boundaries, and resource intent explicitly so the object behaves predictably during reconciliation.
- Operational rule: validate selectors, names, access scope, and field immutability before applying changes to production.
- Verification: inspect the object, related events, dependent resources, and the controller or plugin that is expected to act on it.
- Failure modes: most issues come from selector drift, missing dependencies, invalid references, policy denial, or unsupported plugin behavior.

YAML syntax example:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: web-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
```

### 116. Deployment
- Definition: A Deployment manages stateless application rollout by creating and updating ReplicaSets according to the declared Pod template.
- Core fields: `.spec.replicas`, `.spec.selector`, `.spec.template`, `.spec.strategy`, `.spec.minReadySeconds`, and `.spec.progressDeadlineSeconds` control behavior.
- Controller behavior: when the Pod template changes, the Deployment creates a new ReplicaSet, scales it up, and scales the previous ReplicaSet down according to rollout strategy.
- Selector rule: `.spec.selector` must match template labels and is effectively immutable after creation; mismatches break ownership and rollout behavior.
- Production use: always define requests, limits, readiness probes, liveness or startup probes where needed, and a rolling update strategy tuned for availability.
- Availability control: combine multiple replicas, readiness gating, topology spreading, and PodDisruptionBudget so maintenance and rollouts do not drop service capacity.
- Rollback behavior: Deployment stores revision history and can roll back to a previous ReplicaSet if the new revision fails readiness or progress checks.
- Verification: check `observedGeneration`, rollout status, ready replicas, ReplicaSet history, events, and whether Service endpoints have shifted to the new Pods.
- Failure modes: selector mistakes, bad image tags, missing readiness probes, insufficient resources, and schema changes that require coordinated application rollout.

YAML syntax example:

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
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.27
```

### 117. StatefulSet
- Definition: A StatefulSet manages stateful Pods that require stable identity, ordered rollout, and persistent storage association.
- Identity model: each Pod gets a stable ordinal name, stable DNS identity, and typically a dedicated PersistentVolumeClaim.
- Controller behavior: StatefulSet creates, deletes, and updates Pods in ordinal order unless configured for parallel Pod management.
- Storage coupling: volumeClaimTemplates bind storage identity to the ordinal Pod, so rescheduling keeps data affinity across restarts.
- Production use: use for databases, brokers, or clustered systems that depend on durable identity and predictable membership changes.
- Update behavior: rolling updates occur one ordinal at a time by default; application-level quorum and readiness handling must tolerate this sequence.
- Verification: confirm Pod ordinals, stable DNS names, PVC binding, volume attachment, readiness, and application quorum before moving to higher ordinals.
- Failure modes: broken headless Service, unbound PVCs, quorum loss during updates, and storage classes that do not match access or topology requirements.

YAML syntax example:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: db
spec:
  serviceName: db
  replicas: 3
  selector:
    matchLabels:
      app: db
```

### 118. Persistent storage
- Definition: Persistent storage is storage that survives Pod recreation and is typically provided through PersistentVolumes and claims.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 119. DaemonSet
- Definition: A DaemonSet ensures one or more copies of a Pod run on selected nodes, commonly for logging, monitoring, or networking agents.
- API shape: the object is declared in YAML and persisted through the Kubernetes API, where controllers or system components act on its spec.
- Key behavior: metadata identifies the object, spec carries desired state, and status or events reveal observed runtime state when applicable.
- Production use: define labels, ownership, security boundaries, and resource intent explicitly so the object behaves predictably during reconciliation.
- Operational rule: validate selectors, names, access scope, and field immutability before applying changes to production.
- Verification: inspect the object, related events, dependent resources, and the controller or plugin that is expected to act on it.
- Failure modes: most issues come from selector drift, missing dependencies, invalid references, policy denial, or unsupported plugin behavior.

YAML syntax example:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-agent
spec:
  selector:
    matchLabels:
      app: node-agent
```

### 120. Job
- Definition: A Job runs Pods until a specified completion count succeeds.
- API shape: the object is declared in YAML and persisted through the Kubernetes API, where controllers or system components act on its spec.
- Key behavior: metadata identifies the object, spec carries desired state, and status or events reveal observed runtime state when applicable.
- Production use: define labels, ownership, security boundaries, and resource intent explicitly so the object behaves predictably during reconciliation.
- Operational rule: validate selectors, names, access scope, and field immutability before applying changes to production.
- Verification: inspect the object, related events, dependent resources, and the controller or plugin that is expected to act on it.
- Failure modes: most issues come from selector drift, missing dependencies, invalid references, policy denial, or unsupported plugin behavior.

YAML syntax example:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: backup
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
        - name: backup
          image: busybox
```

### 121. CronJob
- Definition: A CronJob creates Jobs on a schedule defined by cron syntax.
- API shape: the object is declared in YAML and persisted through the Kubernetes API, where controllers or system components act on its spec.
- Key behavior: metadata identifies the object, spec carries desired state, and status or events reveal observed runtime state when applicable.
- Production use: define labels, ownership, security boundaries, and resource intent explicitly so the object behaves predictably during reconciliation.
- Operational rule: validate selectors, names, access scope, and field immutability before applying changes to production.
- Verification: inspect the object, related events, dependent resources, and the controller or plugin that is expected to act on it.
- Failure modes: most issues come from selector drift, missing dependencies, invalid references, policy denial, or unsupported plugin behavior.

YAML syntax example:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: nightly-backup
spec:
  schedule: "0 2 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: Never
```

### 122. Scheduled workloads
- Definition: Scheduled workloads are tasks triggered at defined times or intervals rather than continuously running services.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 123. Static Pods
- Definition: Static Pods are Pods launched directly by kubelet from manifest files on a node rather than created through the API server.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 124. When static Pods are used
- Definition: Static Pods are mainly used for bootstrapping or running node-local critical components such as control-plane services.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 125. Controller reconciliation
- Definition: Controller reconciliation is the repeated process of comparing desired and actual state and issuing API actions to converge them.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 126. Rollout ownership
- Definition: Rollout ownership identifies which controller is responsible for revision history, update strategy, and Pod replacement during change.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

## 8. Pod Specification and Runtime Behavior

### 127. Container command
- Definition: The container command field overrides the image entrypoint and defines the executable started inside the container.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

YAML syntax example:

```yaml
containers:
  - name: app
    image: busybox
    command: ["sh", "-c"]
    args: ["echo hello"]
```

### 128. Container args
- Definition: Container args provide command-line parameters to the image entrypoint or overridden command.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 129. env
- Definition: env defines explicit environment variables injected into a container at startup.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

YAML syntax example:

```yaml
env:
  - name: LOG_LEVEL
    value: info
```

### 130. envFrom
- Definition: envFrom imports multiple environment variables from a ConfigMap or Secret into a container.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

YAML syntax example:

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

### 131. Container ports
- Definition: Container ports declare named or numeric ports used for documentation and Service targetPort mapping.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

YAML syntax example:

```yaml
ports:
  - name: http
    containerPort: 8080
```

### 132. imagePullPolicy
- Definition: imagePullPolicy controls when kubelet asks the runtime to pull an image before starting a container.
- Content model: images reference manifests and layers; tags are mutable references, while digests pin exact content.
- Runtime path: kubelet asks the runtime to resolve the image reference, authenticate if required, pull missing layers, and create the container filesystem.
- Production use: pin critical workloads to immutable versions, manage registry auth explicitly, and keep build provenance separate from deployment environment data.
- Verification: confirm the resolved image ID on the running Pod, pull events, registry reachability, and whether the manifest matches the intended release artifact.
- Failure modes: mutable tags, registry auth errors, rate limits, corrupted layers, and architecture mismatches between image and node.

YAML syntax example:

```yaml
imagePullPolicy: IfNotPresent
```

### 133. Liveness probe
- Definition: A liveness probe detects whether a container is unhealthy and should be restarted.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

YAML syntax example:

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
```

### 134. Readiness probe
- Definition: A readiness probe determines whether a Pod should receive Service traffic.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

YAML syntax example:

```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
```

### 135. Startup probe
- Definition: A startup probe delays liveness and readiness failure handling until a slow-starting application has initialized.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

YAML syntax example:

```yaml
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
```

### 136. Resource requests
- Definition: A resource request is the minimum CPU or memory amount used by the scheduler for placement decisions.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

### 137. Resource limits
- Definition: A resource limit is the maximum CPU or memory a container may consume before throttling or OOM enforcement occurs.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 138. QoS classes
- Definition: QoS classes classify Pods as Guaranteed, Burstable, or BestEffort based on requests and limits, influencing eviction order.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

### 139. Pod-level SecurityContext
- Definition: A Pod-level SecurityContext sets defaults shared across containers in the Pod where supported.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

YAML syntax example:

```yaml
spec:
  securityContext:
    fsGroup: 2000
  containers:
    - name: app
      image: nginx:1.27
```

### 140. Container-level SecurityContext
- Definition: A container-level SecurityContext applies execution restrictions to one specific container.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

YAML syntax example:

```yaml
containers:
  - name: app
    image: nginx:1.27
    securityContext:
      runAsNonRoot: true
      allowPrivilegeEscalation: false
```

### 141. Graceful shutdown
- Definition: Graceful shutdown is the termination period during which Kubernetes sends SIGTERM and waits before forcefully killing processes.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

### 142. Termination signals
- Definition: Termination signals are POSIX signals, typically SIGTERM followed by SIGKILL, used to stop container processes.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

### 143. Lifecycle hooks
- Definition: Lifecycle hooks are container callbacks such as postStart and preStop executed during startup or termination.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

YAML syntax example:

```yaml
lifecycle:
  preStop:
    exec:
      command: ["sh", "-c", "sleep 5"]
```

### 144. Image pull secrets
- Definition: Image pull secrets supply registry credentials so kubelet can authenticate when pulling private images.
- Content model: images reference manifests and layers; tags are mutable references, while digests pin exact content.
- Runtime path: kubelet asks the runtime to resolve the image reference, authenticate if required, pull missing layers, and create the container filesystem.
- Production use: pin critical workloads to immutable versions, manage registry auth explicitly, and keep build provenance separate from deployment environment data.
- Verification: confirm the resolved image ID on the running Pod, pull events, registry reachability, and whether the manifest matches the intended release artifact.
- Failure modes: mutable tags, registry auth errors, rate limits, corrupted layers, and architecture mismatches between image and node.

YAML syntax example:

```yaml
imagePullSecrets:
  - name: regcred
```

### 145. Private registry access
- Definition: Private registry access is authenticated retrieval of container images from non-public registries.
- Content model: images reference manifests and layers; tags are mutable references, while digests pin exact content.
- Runtime path: kubelet asks the runtime to resolve the image reference, authenticate if required, pull missing layers, and create the container filesystem.
- Production use: pin critical workloads to immutable versions, manage registry auth explicitly, and keep build provenance separate from deployment environment data.
- Verification: confirm the resolved image ID on the running Pod, pull events, registry reachability, and whether the manifest matches the intended release artifact.
- Failure modes: mutable tags, registry auth errors, rate limits, corrupted layers, and architecture mismatches between image and node.

### 146. Ephemeral containers
- Definition: Ephemeral containers are temporary containers added to a running Pod for debugging and are not part of normal restart behavior.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

## 9. Scheduling and Placement

### 147. Scheduler workflow
- Definition: Scheduler workflow filters feasible nodes, scores them, selects the best fit, and creates a binding for the Pod.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

### 148. Scheduling decisions
- Definition: Scheduling decisions are based on requests, constraints, topology, taints, priorities, and plugin scoring.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 149. Requests
- Definition: Requests define the minimum CPU or memory amount considered by the scheduler during Pod placement.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

YAML syntax example:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

### 150. Bin-packing
- Definition: Bin-packing places workloads efficiently onto nodes while still respecting resource and policy constraints.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 151. nodeSelector
- Definition: nodeSelector is a simple exact-match label constraint that limits which nodes may run a Pod.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
nodeSelector:
  disktype: ssd
```

### 152. Node affinity
- Definition: Node affinity expresses label-based node placement rules with required and preferred terms.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

YAML syntax example:

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
        - matchExpressions:
            - key: topology.kubernetes.io/zone
              operator: In
              values: ["us-east-1a"]
```

### 153. Pod affinity and anti-affinity
- Definition: Pod affinity and anti-affinity place Pods relative to other Pods based on labels and topology domains.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

YAML syntax example:

```yaml
affinity:
  podAntiAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: web
        topologyKey: kubernetes.io/hostname
```

### 154. Taints
- Definition: A taint marks a node so only Pods with matching tolerations can be scheduled or remain there.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

### 155. Tolerations
- Definition: A toleration allows a Pod to match a tainted node but does not itself force scheduling onto that node.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

YAML syntax example:

```yaml
tolerations:
  - key: dedicated
    operator: Equal
    value: gpu
    effect: NoSchedule
```

### 156. Dedicated nodes
- Definition: Dedicated nodes are reserved for selected workloads using labels, taints, and scheduling constraints.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 157. Topology spread constraints
- Definition: Topology spread constraints limit skew of matching Pods across nodes, zones, or other topology domains.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

YAML syntax example:

```yaml
topologySpreadConstraints:
  - maxSkew: 1
    topologyKey: topology.kubernetes.io/zone
    whenUnsatisfiable: DoNotSchedule
    labelSelector:
      matchLabels:
        app: web
```

### 158. Zone-aware placement
- Definition: Zone-aware placement distributes workloads across availability zones to reduce correlated failure risk.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

### 159. PriorityClass
- Definition: A PriorityClass assigns an integer scheduling priority to Pods and influences ordering and preemption.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

YAML syntax example:

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: critical-app
value: 100000
globalDefault: false
```

### 160. Preemption
- Definition: Preemption removes lower-priority Pods so a higher-priority Pod can be scheduled when resources are scarce.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

### 161. Eviction
- Definition: Eviction is the controlled removal of Pods during resource pressure or maintenance, subject to policy and QoS rules.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

### 162. PodDisruptionBudget
- Definition: A PodDisruptionBudget limits voluntary disruptions by requiring a minimum number or percentage of Pods to remain available.
- Scope: PDBs affect evictions from drain or controller-initiated voluntary actions; they do not prevent involuntary failures such as node crashes.
- Key fields: use either `minAvailable` or `maxUnavailable` with a selector that matches the intended workload Pods.
- Production use: PDBs are essential for quorum-sensitive systems and for stateless services that must retain enough healthy replicas during maintenance.
- Operational trade-off: an overly strict PDB can block node drains and cluster upgrades entirely, so it must match real replica counts and SLOs.
- Verification: test drain behavior in staging, observe allowed disruptions, and confirm workload availability during one-node maintenance.

YAML syntax example:

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: web-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: web
```

### 163. Availability during maintenance
- Definition: Availability during maintenance is preserved through replica distribution, readiness checks, and disruption-aware node draining.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

## 10. Services, Networking, and Service Discovery

### 164. Pod networking model
- Definition: The Pod networking model gives each Pod its own IP and expects direct Pod-to-Pod communication inside the cluster.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 165. CNI
- Definition: CNI is the standard interface Kubernetes uses to invoke network plugins for Pod network setup and teardown.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 166. Service types
- Definition: Service types define how a stable virtual endpoint is exposed inside or outside the cluster.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

YAML syntax example:

```yaml
spec:
  type: LoadBalancer
```

### 167. ClusterIP Service
- Definition: A ClusterIP Service exposes an internal virtual IP reachable inside the cluster.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 168. NodePort Service
- Definition: A NodePort Service exposes a Service on a static port on every node and forwards traffic to backend Pods.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 169. LoadBalancer Service
- Definition: A LoadBalancer Service requests an external load balancer from the environment and maps it to a Service.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 170. ExternalName Service
- Definition: An ExternalName Service maps a Service name to an external DNS name without creating a proxy or virtual IP.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 171. kube-proxy behavior
- Definition: kube-proxy watches Services and backends and programs iptables, IPVS, or nftables rules for packet forwarding.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 172. Service virtual IP routing
- Definition: Service virtual IP routing is the node-level packet translation that sends traffic addressed to a Service VIP to one of its healthy backends.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 173. CoreDNS service discovery
- Definition: CoreDNS resolves cluster DNS names for Services and, where configured, Pods so workloads can discover each other by name.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 174. Endpoints
- Definition: Endpoints is the legacy object that stores backend IPs and ports selected by a Service.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 175. EndpointSlices
- Definition: EndpointSlices are the scalable replacement for Endpoints that partition backend data across multiple resources.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 176. Ingress
- Definition: Ingress is a layer-7 routing API for external HTTP and HTTPS traffic that maps hosts and paths to Services.
- Architecture: the Ingress object stores desired routes, while an ingress controller converts that intent into proxy or load-balancer configuration.
- Key fields: `.spec.ingressClassName`, rules, paths, backend Service references, and TLS entries determine route ownership and certificate handling.
- Production use: pair Ingress with a highly available controller, managed certificate flow, health checks, and clear ownership boundaries for shared domains.
- Operational rule: path behavior, rewrite logic, and annotation semantics are controller-specific, so do not assume every controller interprets Ingress the same way.
- Verification: confirm that the controller has accepted the resource, the external address is published, TLS is valid, and backend Services have healthy endpoints.
- Failure modes: wrong ingress class, missing controller, Service port mismatch, incorrect path type, stale TLS secret, and upstream readiness problems.

YAML syntax example:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web
spec:
  rules:
    - host: app.example.com
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

### 177. Ingress controller
- Definition: An ingress controller watches Ingress resources and configures a proxy or load balancer to enforce those routes.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 178. NetworkPolicy
- Definition: NetworkPolicy is a label-based firewall policy for Pod traffic that controls allowed ingress, egress, or both, subject to plugin support.
- Enforcement model: policy is deny-by-default only for selected Pods and selected directions once at least one policy applies; unselected traffic remains unchanged.
- Key selectors: Pod selectors choose target Pods, namespace selectors widen source or destination scope, and port rules narrow allowed protocols and ports.
- Plugin dependency: Kubernetes defines the API, but enforcement is implemented by the CNI or network policy engine; unsupported plugins ignore the intent.
- Production use: start with namespace-scoped default-deny policies, then add explicit allow rules for DNS, ingress paths, metrics, and required east-west dependencies.
- Verification: test both allowed and denied paths with temporary Pods, confirm plugin logs or policy status, and verify DNS and egress exceptions explicitly.
- Failure modes: missing DNS allowance, wrong selector scope, forgetting egress control, and assuming policy applies to hostNetwork or external traffic the same way across plugins.

YAML syntax example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-from-api
spec:
  podSelector:
    matchLabels:
      app: db
  policyTypes: ["Ingress"]
```

### 179. Internal traffic
- Definition: Internal traffic is communication within the cluster or private network boundary.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 180. External traffic flow
- Definition: External traffic flow describes how traffic enters from outside the cluster and reaches Services or Ingress backends.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 181. Port forwarding
- Definition: Port forwarding tunnels a local TCP port to a Pod or Service over the API connection for temporary access.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 182. Connectivity debugging
- Definition: Connectivity debugging verifies DNS, endpoints, routes, open ports, and policy enforcement between clients and Services.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

## 11. Storage and Persistent Data

### 183. Volumes
- Definition: A volume is a storage abstraction mounted into one or more containers in a Pod.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 184. Pod-level storage lifecycle
- Definition: Pod-level storage lifecycle means non-persistent volumes exist only for the lifetime of the Pod that references them.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 185. PersistentVolume
- Definition: A PersistentVolume is a cluster storage resource representing provisioned or pre-existing durable storage.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

YAML syntax example:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-data
spec:
  capacity:
    storage: 10Gi
  accessModes: ["ReadWriteOnce"]
```

### 186. PersistentVolumeClaim
- Definition: A PersistentVolumeClaim requests storage capacity, access mode, and storage class on behalf of a workload.
- Binding model: a claim binds either to a dynamically provisioned volume or to a pre-existing PersistentVolume that satisfies the request.
- Workload coupling: Pods reference claims rather than raw volumes so storage identity can be managed independently from Pod lifecycle.
- Production use: align requested size, access mode, storage class, and topology with the actual runtime behavior of the application.
- Operational rule: treat `ReadWriteOnce`, `ReadOnlyMany`, and `ReadWriteMany` as storage-system guarantees, not generic filesystem assumptions.
- Verification: inspect PVC phase, events, bound PV details, mount status on the node, and application-level read/write behavior.
- Failure modes: Pending claims, wrong access mode, default StorageClass mismatch, zone mismatch, or CSI driver permission problems.

YAML syntax example:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data
spec:
  accessModes: ["ReadWriteOnce"]
  resources:
    requests:
      storage: 10Gi
```

### 187. StorageClass
- Definition: StorageClass defines how dynamic storage is provisioned, including the provisioner, parameters, reclaim behavior, and volume expansion support.
- Provisioning model: when a matching PVC is created, the external provisioner or CSI driver creates backing storage according to StorageClass parameters.
- Binding behavior: `volumeBindingMode` influences whether provisioning occurs immediately or waits until a Pod is scheduled to honor topology constraints.
- Production use: create separate classes for performance tiers, encryption requirements, topology constraints, and reclaim policies instead of one default class for everything.
- Operational rule: the default class should be intentional because omitted `storageClassName` on PVCs often binds to it automatically.
- Verification: confirm the provisioner sidecars are healthy, PVC events show provisioning success, and the resulting PV reflects the expected class and topology.
- Failure modes: wrong provisioner name, unsupported parameters, delayed binding confusion, and reclaim policy causing data deletion when retention was required.

YAML syntax example:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: csi.example.com
allowVolumeExpansion: true
```

### 188. Dynamic provisioning
- Definition: Dynamic provisioning automatically creates backing storage when a matching PVC is created.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 189. CSI
- Definition: CSI is the standard interface used by Kubernetes and storage systems to provision, attach, mount, and snapshot volumes.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 190. Access modes
- Definition: Access modes describe how a volume can be mounted, such as ReadWriteOnce or ReadOnlyMany.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 191. Reclaim policy
- Definition: A reclaim policy defines what happens to underlying storage after a bound PVC is deleted, such as Delete or Retain.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 192. PV/PVC binding behavior
- Definition: PV/PVC binding behavior is the matching process between claims and volumes, including delayed binding for topology-aware classes.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 193. Volume expansion
- Definition: Volume expansion increases PVC capacity and, when supported, grows the backing storage and filesystem.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 194. Filesystem resize behavior
- Definition: Filesystem resize behavior is the node-side expansion step required after block capacity growth so the mounted filesystem can use the new space.
- Kernel role: this is part of the Linux isolation model that containers depend on underneath Kubernetes abstractions.
- Runtime path: the container runtime configures these kernel primitives when creating the Pod sandbox and individual containers.
- Production use: isolation quality depends on runtime configuration, kernel support, capabilities, seccomp, and host-level hardening rather than on Kubernetes YAML alone.
- Verification: inspect runtime behavior, process visibility, mount namespace behavior, cgroup accounting, and node-level security settings.
- Failure modes: privileged containers, host namespace usage, weak seccomp or capability settings, and host kernel constraints reducing isolation.

### 195. Stateful storage patterns
- Definition: Stateful storage patterns align workload identity, volume ownership, backup, and recovery with StatefulSets and durable storage.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 196. VolumeSnapshot
- Definition: A VolumeSnapshot captures point-in-time state of a CSI volume for backup, cloning, or restore workflows.
- Datastore role: this point affects consistency, write durability, leader behavior, or recoverability of the Kubernetes control-plane state store.
- Production use: prioritize low latency, durable storage, certificate hygiene, and tested backup or restore procedures over raw capacity alone.
- Operational rule: keep quorum intact, observe alarms and endpoint health, and avoid maintenance actions that reduce the cluster below majority.
- Verification: inspect endpoint status, leader state, alarm status, snapshot integrity, API consistency, and control-plane responsiveness after changes.
- Failure modes: quorum loss, disk latency, certificate expiry, stale restores, and running maintenance on the wrong member or at the wrong time.

YAML syntax example:

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

### 197. Backup
- Definition: Backup is the creation of a recoverable copy of persistent data or control-plane state for later restore.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 198. Restore concepts
- Definition: Restore concepts cover recreating usable application or volume state from a snapshot or backup copy.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 199. Storage attach troubleshooting
- Definition: Storage attach troubleshooting checks controller attach operations, CSI events, and node visibility of the device.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 200. Mount operations
- Definition: Mount operations attach a filesystem, block device, or projected data source into the container-visible filesystem path.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 201. PVC claim issues
- Definition: PVC claim issues include pending claims, access mode mismatches, wrong storage class, quota failures, and topology constraints.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

## 12. Configuration and Secrets

### 202. ConfigMap
- Definition: A ConfigMap stores non-sensitive configuration data as key-value pairs or file content.
- API shape: the object is declared in YAML and persisted through the Kubernetes API, where controllers or system components act on its spec.
- Key behavior: metadata identifies the object, spec carries desired state, and status or events reveal observed runtime state when applicable.
- Production use: define labels, ownership, security boundaries, and resource intent explicitly so the object behaves predictably during reconciliation.
- Operational rule: validate selectors, names, access scope, and field immutability before applying changes to production.
- Verification: inspect the object, related events, dependent resources, and the controller or plugin that is expected to act on it.
- Failure modes: most issues come from selector drift, missing dependencies, invalid references, policy denial, or unsupported plugin behavior.

YAML syntax example:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  LOG_LEVEL: info
```

### 203. Secret usage patterns
- Definition: Secret usage patterns define how sensitive values are supplied as environment variables, mounted files, or image pull credentials.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 204. Environment variable injection
- Definition: Environment variable injection exposes ConfigMap or Secret values to a container process at startup.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

YAML syntax example:

```yaml
env:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: app-secret
        key: password
```

### 205. File-based mounts
- Definition: File-based mounts project configuration or secret data into a container filesystem as files and directories.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

YAML syntax example:

```yaml
volumes:
  - name: config
    configMap:
      name: app-config
containers:
  - name: app
    volumeMounts:
      - name: config
        mountPath: /etc/app
```

### 206. Immutable configuration patterns
- Definition: Immutable configuration patterns avoid in-place mutation and instead roll out new object versions to trigger predictable updates.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 207. Secret types
- Definition: Secret types classify stored data such as Opaque, TLS, dockerconfigjson, or service-account-token.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

YAML syntax example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
stringData:
  password: s3cr3t
```

### 208. Secret rotation
- Definition: Secret rotation is the controlled replacement of credentials before expiration, revocation, or compromise.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 209. Secret protection basics
- Definition: Secret protection basics include encryption at rest, least-privilege access, and limiting exposure through logs or environment variables.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 210. External secret integration
- Definition: External secret integration synchronizes secrets from external stores such as Vault or cloud secret managers into Kubernetes.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

## 13. Security in Practice

### 211. Least-privilege RBAC
- Definition: Least-privilege RBAC grants only the verbs and resources required for a workload or user to perform its function.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 212. Secret protection
- Definition: Secret protection covers access control, encryption, short-lived credentials, and minimizing distribution of sensitive values.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

YAML syntax example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
stringData:
  username: appuser
  password: strong-secret
```

### 213. Encryption at rest
- Definition: Encryption at rest encrypts object data, including Secrets, before it is written to persistent storage.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 214. KMS integration
- Definition: KMS integration delegates data-encryption-key protection to an external key management service.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 215. Pod
- Definition: A Pod is the smallest deployable Kubernetes object and defines one or more tightly coupled containers sharing network and volumes.
- Network model: all containers in a Pod share the same Pod IP and port space, so sidecars and app containers communicate over localhost.
- Storage model: containers in the Pod can share mounted volumes; Pod-scoped ephemeral data is lost when the Pod is deleted unless persistent volumes are used.
- Lifecycle: kubelet creates the sandbox, pulls images, starts init containers first, then app containers, and reports conditions such as Initialized, Ready, and ContainersReady.
- Production use: Pods are rarely created directly for long-lived services; controllers such as Deployments and StatefulSets should own them.
- Operational rule: define probes, resource requests and limits, termination behavior, and security context explicitly to avoid unstable runtime behavior.
- Verification: inspect phase, container states, restart count, conditions, events, and logs rather than using phase alone as a health signal.
- Failure modes: CrashLoopBackOff, OOMKilled, image pull failure, volume mount errors, failed probes, and node-level network or runtime problems.

YAML syntax example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
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

### 216. Container hardening with SecurityContext
- Definition: Container hardening with SecurityContext reduces attack surface by enforcing non-root execution, dropping capabilities, controlling seccomp, and preventing privilege escalation.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 217. Network isolation with NetworkPolicy
- Definition: Network isolation with NetworkPolicy restricts allowed traffic paths between workloads instead of allowing all Pod-to-Pod communication by default.
- Kernel role: this is part of the Linux isolation model that containers depend on underneath Kubernetes abstractions.
- Runtime path: the container runtime configures these kernel primitives when creating the Pod sandbox and individual containers.
- Production use: isolation quality depends on runtime configuration, kernel support, capabilities, seccomp, and host-level hardening rather than on Kubernetes YAML alone.
- Verification: inspect runtime behavior, process visibility, mount namespace behavior, cgroup accounting, and node-level security settings.
- Failure modes: privileged containers, host namespace usage, weak seccomp or capability settings, and host kernel constraints reducing isolation.

### 218. Certificate management
- Definition: Certificate management covers issuance, trust distribution, renewal, and replacement of TLS credentials for components and users.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 219. Certificate trust chain
- Definition: A certificate trust chain is the sequence of certificate authorities and signed certificates used to validate identity.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 220. Auditability
- Definition: Auditability is the ability to reconstruct who changed what, when, and through which API action.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 221. Cluster security observability
- Definition: Cluster security observability combines audit logs, events, metrics, and security tooling to detect suspicious activity or policy violations.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

## 14. Node and Cluster Operations

### 222. Node registration
- Definition: Node registration is the process by which kubelet authenticates and creates or updates its Node object in the API.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 223. Node readiness
- Definition: Node readiness is the condition indicating whether the control plane currently considers a node healthy enough to run workloads.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 224. Node lifecycle
- Definition: Node lifecycle covers registration, health reporting, scheduling, draining, maintenance, and removal.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 225. Cordon
- Definition: Cordon marks a node unschedulable so new Pods are not placed on it.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 226. Drain
- Definition: Drain evicts workload Pods from a node in preparation for maintenance, replacement, or upgrade.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 227. Uncordon
- Definition: Uncordon returns a previously cordoned node to schedulable state.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 228. Kubelet responsibilities
- Definition: Kubelet responsibilities include Pod lifecycle management, probe execution, volume mounts, container runtime interaction, and node status reporting.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 229. Node troubleshooting
- Definition: Node troubleshooting starts with kubelet logs, runtime state, disk pressure, CNI health, and host network reachability.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 230. Container runtime inspection
- Definition: Container runtime inspection uses CRI tooling and service logs to check images, sandboxes, containers, and runtime health.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 231. Cluster upgrade flow
- Definition: Cluster upgrade flow is the ordered production procedure for moving Kubernetes control-plane and worker components to a supported target version.
- Pre-check 1: validate target version compatibility, supported version skew, addon compatibility, CSI/CNI support, and whether admission or CRD changes affect workloads.
- Pre-check 2: take and verify etcd backup, capture cluster state, review maintenance windows, confirm rollback boundaries, and freeze risky changes during the upgrade window.
- Control plane sequence: upgrade one control-plane node at a time when HA exists, keeping API availability through the load-balanced endpoint.
- Node sequence: cordon and drain each worker, upgrade kubelet/runtime packages or images, restart node services, verify readiness, then uncordon before moving on.
- Workload protection: ensure Deployments have multiple replicas, StatefulSets have safe quorum behavior, and PodDisruptionBudgets do not block intended maintenance.
- Addon handling: verify CoreDNS, kube-proxy, CNI, CSI, metrics-server, ingress controllers, and GitOps or policy controllers after the core version moves.
- Validation 1: confirm API availability, node readiness, component versions, DNS, storage mounts, Service routing, admission policies, and application SLO signals.
- Validation 2: run smoke tests against critical applications, canary traffic paths, and backup or restore workflows that depend on cluster APIs.
- Rollback boundary: control-plane binaries can often be rolled back only within supported skew limits; etcd restore is the last-resort path if state corruption occurs.
- Production rule: never batch-upgrade all workers at once; progress node-by-node or pool-by-pool while observing real service behavior between steps.

### 232. Control plane first upgrade order
- Definition: The control plane must be upgraded before kubelets so the API remains at the newest supported version in the skew matrix.
- Reasoning: kubelets are allowed to be behind the API server within supported limits, but the reverse direction is not supported safely.
- Sequence: upgrade API servers, then controller-manager and scheduler, then cluster add-ons that depend on API behavior, and only then worker components.
- HA handling: in multi-control-plane clusters, upgrade one control-plane node at a time behind the load balancer to preserve quorum and API access.
- Verification: after each control-plane node, confirm API health checks, leader election stability, controller reconciliations, and etcd endpoint health.
- Risk: upgrading workers first can leave nodes talking to an older or incompatible control plane, causing registration, admission, or runtime failures.

### 233. Worker node upgrade order
- Definition: Worker node upgrade order is the production-safe sequence used to rotate or upgrade nodes after the control plane is stable.
- Sequence step 1: cordon the node to stop new scheduling and review whether daemon or static Pods on the node require special handling.
- Sequence step 2: drain the node with respect to PodDisruptionBudgets and workload topology so service capacity stays above target.
- Sequence step 3: upgrade kubelet, runtime, OS image, or node pool version according to the provisioning model, then restart node services.
- Sequence step 4: wait for the node to return Ready, verify CNI and CSI functionality, and inspect kubelet and runtime logs for regressions.
- Sequence step 5: uncordon the node and watch replacement Pods for readiness, endpoint registration, and application health before proceeding.
- Production rule: rotate nodes in small batches or one-by-one per failure domain to avoid correlated downtime.

### 234. Certificate renewal
- Definition: Certificate renewal regenerates or reissues expiring control-plane or client certificates before they break TLS or authentication.
- Security path: this point controls identity, permission, request mutation or validation, secret handling, or traceability inside the API path or workload runtime.
- Production use: enforce least privilege, short-lived credentials where possible, explicit policy boundaries, and separation of human and workload identities.
- Operational rule: verify both allow and deny behavior; a security control is incomplete if only the success path is tested.
- Verification: inspect effective RBAC, admission decisions, mounted credentials, audit records, certificate validity, and whether Secrets are exposed only through intended channels.
- Failure modes: overbroad bindings, hidden inherited permissions, expired certificates, webhook outages, and secret distribution through logs or environment leaks.

### 235. kubeconfig regeneration
- Definition: kubeconfig regeneration updates embedded client certificates or cluster connection data used by components and administrators.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 236. etcd backup
- Definition: etcd backup is the creation of a consistent snapshot of the cluster datastore used for disaster recovery and risky maintenance protection.
- Preconditions: run the snapshot against a healthy endpoint using the correct client certificates, and verify endpoint status before capturing backup.
- Data captured: Kubernetes object state stored in etcd, including cluster configuration, workload definitions, RBAC, and object metadata.
- Operational rule: store snapshots off-node and off-cluster, with retention, integrity verification, and restricted access because the data may contain sensitive objects.
- Verification: after snapshot creation, inspect snapshot status or metadata, validate restore procedure on a non-production environment, and document the exact cluster version and member topology.
- Limitation: an etcd snapshot does not back up external systems such as cloud load balancers, external databases, object storage, or secrets stored outside etcd.
- Failure mode: backups taken from unhealthy or partitioned members may not give a reliable recovery point.

### 237. Restore workflow
- Definition: Restore workflow is the end-to-end recovery sequence that turns a backup artifact into a functioning cluster or workload state.
- Step 1: identify the exact failure scope—single object, namespace, storage volume, node pool, control plane, or full cluster—before choosing a restore target.
- Step 2: stop or isolate components that would overwrite restored state, such as controllers, schedulers, or applications still writing to damaged storage.
- Step 3: restore the correct layer first: etcd for cluster metadata, persistent volumes for application data, then dependent application components.
- Step 4: rewire consumers to the restored state by updating mounts, endpoints, member lists, or control-plane manifests as required.
- Step 5: validate readiness, API integrity, DNS, application consistency, and any external dependencies that are not part of the backup itself.
- Production rule: every restore workflow needs clearly documented RPO, RTO, validation checks, and a reconciliation step for changes made after the backup point.

### 238. Safe maintenance
- Definition: Safe maintenance combines scheduling controls, disruption budgets, backups, and validation to minimize service interruption.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 239. Operational cleanup
- Definition: Operational cleanup removes unused resources, stale images, failed objects, and orphaned storage to reduce drift and operational risk.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 240. Recovery basics
- Definition: Recovery basics cover service restoration steps, health validation, and return-to-normal operations after failure.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 241. Capacity planning
- Definition: Capacity planning estimates future compute, memory, storage, and control-plane scale requirements.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 242. Failure handling
- Definition: Failure handling defines how the platform detects, contains, and recovers from node, storage, network, or control-plane faults.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

## 15. Observability and Troubleshooting

### 243. kubectl get
- Definition: kubectl get lists resources and selected status fields from the API server.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 244. kubectl describe
- Definition: kubectl describe shows detailed object state, conditions, and recent events.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 245. kubectl logs
- Definition: kubectl logs retrieves stdout and stderr streams from a container for troubleshooting.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 246. kubectl exec
- Definition: kubectl exec runs a command inside a running container for inspection or debugging.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 247. Events
- Definition: Events are time-stamped records of state changes, warnings, or controller actions attached to involved objects.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 248. kubectl top
- Definition: kubectl top shows live CPU and memory usage from the metrics API.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 249. metrics-server
- Definition: metrics-server collects resource usage from kubelets and exposes it through the metrics API for kubectl top and HPA.
- Core mechanics: the component participates in the control loop by either persisting state, making scheduling decisions, reconciling objects, or enforcing node/runtime behavior.
- Data path: understand what the component watches, what state it mutates, and which external systems or plugins it depends on.
- Production considerations: availability, certificates, network reachability, RBAC, and configuration flags determine whether the component behaves predictably under load.
- Verification: inspect health endpoints, logs, leader election state where applicable, and whether downstream objects reflect expected actions.
- Failure modes: time skew, certificate expiry, unreachable dependencies, bad flags, and resource starvation often present as secondary symptoms elsewhere in the cluster.

### 250. Basic resource observability
- Definition: Basic resource observability includes CPU, memory, restart counts, conditions, and recent events for workloads and nodes.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 251. Pending Pod troubleshooting
- Definition: Pending Pod troubleshooting focuses on scheduler constraints, insufficient resources, PVC binding, taints, and image pull delays.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 252. CrashLoopBackOff
- Definition: CrashLoopBackOff is the backoff state entered when a container repeatedly starts and exits unsuccessfully.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 253. OOMKilled
- Definition: OOMKilled indicates a container was terminated because it exceeded its memory limit or encountered severe memory pressure.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 254. ImagePullBackOff
- Definition: ImagePullBackOff indicates repeated image pull failure due to auth issues, bad image names, or registry or network problems.
- Content model: images reference manifests and layers; tags are mutable references, while digests pin exact content.
- Runtime path: kubelet asks the runtime to resolve the image reference, authenticate if required, pull missing layers, and create the container filesystem.
- Production use: pin critical workloads to immutable versions, manage registry auth explicitly, and keep build provenance separate from deployment environment data.
- Verification: confirm the resolved image ID on the running Pod, pull events, registry reachability, and whether the manifest matches the intended release artifact.
- Failure modes: mutable tags, registry auth errors, rate limits, corrupted layers, and architecture mismatches between image and node.

### 255. Probe failures
- Definition: Probe failures occur when liveness, readiness, or startup checks do not meet success conditions.
- Runtime path: this point influences how kubelet starts containers, decides readiness, enforces resources or security, and shuts workloads down.
- Production use: define it explicitly in manifests so rollout, autoscaling, and disruption behavior remain predictable across nodes.
- Operational rule: align probe timing, resource values, and shutdown logic with real application startup and termination behavior.
- Verification: inspect container state, probe results, restart counts, events, and whether Services include only healthy Pods.
- Failure modes: probes that flap, limits that trigger OOM kills, insecure runtime defaults, and SIGTERM handlers that do not complete within grace periods.

### 256. Rollout failures
- Definition: Rollout failures occur when new Pods do not become ready or controller progress deadlines are exceeded.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 257. DNS troubleshooting context
- Definition: DNS troubleshooting examines cluster name resolution, CoreDNS behavior, search paths, and reachable upstream resolvers.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 258. Service troubleshooting context
- Definition: Service troubleshooting examines selectors, endpoints, target ports, and node-level forwarding behavior.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 259. NetworkPolicy troubleshooting
- Definition: NetworkPolicy troubleshooting verifies selectors, directions, ports, namespace labels, and plugin enforcement behavior.
- Network path: this point determines how Pods receive addresses, discover peers, expose endpoints, or filter traffic between clients and workloads.
- Production use: design for stable naming, explicit policy, controller ownership, health-checked backends, and clear separation of east-west and north-south flows.
- Operational rule: validate DNS, endpoint population, policy enforcement, and external entrypoints together because failures often cross layers.
- Verification: inspect EndpointSlices, CoreDNS behavior, kube-proxy or CNI state, ingress controller status, and real client connectivity from inside and outside the cluster.
- Failure modes: empty endpoints, wrong selectors, stale DNS assumptions, plugin-specific policy gaps, and path or port mismatches at the ingress boundary.

### 260. PVC troubleshooting context
- Definition: PVC troubleshooting examines claim status, storage class, events, access modes, and provisioner behavior.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 261. PV troubleshooting context
- Definition: PV troubleshooting examines volume phase, claim binding, reclaim policy, and backing storage health.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 262. Storage troubleshooting
- Definition: Storage troubleshooting inspects events, CSI controller logs, node mounts, access modes, and capacity state.
- Storage path: this point determines how capacity is requested, provisioned, attached, mounted, retained, or restored for workloads.
- Production use: align topology, access modes, reclaim behavior, and backup strategy with the actual persistence guarantees the application needs.
- Operational rule: validate both provisioning and node mount behavior; a bound claim is not proof that the application can safely read and write data.
- Verification: inspect PVC and PV phase, provisioner events, node mount state, filesystem resize state, and application-level I/O behavior.
- Failure modes: delayed binding confusion, zone mismatch, unsupported access modes, stale attachments, and reclaim policy deleting data unexpectedly.

### 263. Node-level troubleshooting
- Definition: Node-level troubleshooting examines kubelet health, runtime state, disk pressure, networking, and system logs.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 264. Container runtime logs
- Definition: Container runtime logs show image pull errors, container lifecycle failures, and sandbox-level issues from the CRI implementation.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 265. Control plane troubleshooting
- Definition: Control plane troubleshooting inspects API server, scheduler, controller-manager, and etcd health, logs, certificates, and endpoints.
- Architecture role: this point defines how control-plane availability, workload placement, network reachability, and failure isolation are structured.
- Production use: choose the model based on blast radius, region or zone boundaries, latency, compliance, and operational ownership.
- Operational rule: design for component replacement, certificate rotation, DNS continuity, and clear separation between control-plane and application failure paths.
- Verification: validate API reachability, node registration, traffic routing, fault-domain spread, and expected behavior during a single-node or single-zone loss.
- Failure modes: hidden single points of failure, insufficient control-plane redundancy, and network dependencies that collapse multiple failure domains together.

### 266. Debug containers
- Definition: Debug containers are temporary containers used to inspect a running Pod or its namespaces without rebuilding the application image.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 267. Temporary test Pods
- Definition: Temporary test Pods are short-lived Pods used to verify DNS, connectivity, storage, or service behavior from inside the cluster.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 268. Port-forward workflows
- Definition: Port-forward workflows provide temporary local access to internal Pods or Services without externally exposing them.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 269. Rollout
- Definition: A rollout is the controlled progression of a new workload revision.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 270. Rollback
- Definition: A rollback reverts a controller-managed workload to a previous known-good revision.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 271. Release troubleshooting
- Definition: Release troubleshooting inspects revision history, events, readiness, image changes, and controller conditions during deployment failure.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

## 16. Scaling, Availability, and Reliability

### 272. Manual scaling
- Definition: Manual scaling changes replica count or node count directly rather than through autoscaling controllers.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 273. Replica management
- Definition: Replica management is the maintenance of the desired number of workload instances over time.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 274. Horizontal Pod Autoscaler
- Definition: HPA adjusts the replica count of a scalable workload based on observed metrics compared with configured targets.
- Control loop: the HPA controller reads metrics APIs, computes desired replicas, and writes scale changes to the target resource such as a Deployment.
- Metrics sources: common inputs are CPU, memory, or custom/external metrics exposed through metrics adapters.
- Production use: HPA works best when requests are accurate, readiness probes prevent premature traffic, and startup spikes are handled with stabilization behavior.
- Operational rule: autoscaling does not replace capacity planning; the cluster still needs enough node headroom or Cluster Autoscaler support.
- Verification: confirm metrics availability, HPA conditions, current versus desired replicas, and whether scaling decisions align with real traffic.
- Failure modes: missing metrics, bad resource requests, scaling oscillation, slow startup, and external dependencies that become the true bottleneck.

YAML syntax example:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: web
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: web
  minReplicas: 2
  maxReplicas: 10
```

### 275. Vertical Pod Autoscaler
- Definition: VPA recommends or updates CPU and memory requests and limits based on observed usage patterns.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 276. Cluster Autoscaler
- Definition: Cluster Autoscaler adjusts node group size when Pods cannot be scheduled or when nodes are underutilized and safe to remove.
- Decision model: scale-up happens when pending Pods have unsatisfied resource or scheduling constraints that an eligible node group can satisfy.
- Scale-down model: nodes are removed only when workloads can be rescheduled elsewhere and local conditions such as DaemonSets or PDBs permit eviction.
- Production use: tune node group templates, labels, taints, and quotas so new nodes actually match workload scheduling requirements.
- Operational rule: Cluster Autoscaler complements HPA; one scales Pods, the other scales nodes.
- Verification: inspect pending Pods, autoscaler events, cloud or infrastructure group state, and whether new nodes join Ready with expected labels and capacity.
- Failure modes: unsupported scheduling constraints, exhausted infrastructure quotas, slow node provisioning, and PDBs or local storage blocking scale-down.

### 277. Stateless high availability patterns
- Definition: Stateless HA patterns rely on multiple replicas, readiness gating, and load balancing across healthy Pods.
- Control-plane meaning: the concept influences how controllers compare desired state to observed state and decide corrective action.
- Production use: express intent through stable APIs, resource requests, probes, and controller-owned objects rather than through one-off node changes.
- Availability impact: the concept determines whether workloads survive node loss, rollout errors, and repeated reconciliation without creating drift.
- Verification: observe controller status, replica counts, endpoint readiness, rescheduling behavior, and whether repeated applies remain non-destructive.
- Failure modes: imperative hotfixes, missing probes, insufficient replicas, and topology concentration inside a single failure domain.

### 278. Stateful availability
- Definition: Stateful availability depends on durable storage, stable identity, quorum awareness, and application-level failover design.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 279. Disruption handling
- Definition: Disruption handling covers voluntary and involuntary interruptions and the policies used to limit their impact.
- Scheduling path: this point influences which nodes are feasible, how candidates are scored, and whether a Pod may remain on a node after conditions change.
- Production use: combine these controls with accurate requests and topology design so capacity, isolation, and resilience goals are enforced by the scheduler.
- Operational rule: test constraints together; affinity, taints, quotas, and PDBs can interact in ways that block rollout or maintenance.
- Verification: inspect scheduling events, node labels or taints, chosen topology domains, and whether replacement Pods land where policy expects.
- Failure modes: impossible placement rules, missing tolerations, over-constrained anti-affinity, and preemption or eviction causing unexpected availability loss.

### 280. Failure recovery
- Definition: Failure recovery is the restoration of workload or platform service after node, process, storage, or network failure.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 281. Resilience during upgrades or node loss
- Definition: Resilience during upgrades or node loss depends on replica spread, disruption budgets, health checks, and durable control-plane state.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

## 17. Packaging and Delivery

### 282. Manifest-based deployment workflows
- Definition: Manifest-based deployment workflows apply version-controlled YAML directly to the cluster through kubectl or automation.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 283. Kustomize
- Definition: Kustomize customizes base manifests through overlays, patches, generators, and name transformations without using a template engine.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

### 284. Kustomize overlays
- Definition: A Kustomize overlay is an environment-specific customization layer applied on top of a reusable base manifest set.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

YAML syntax example:

```yaml
# kustomization.yaml
resources:
  - ../../base
patches:
  - path: replica-patch.yaml
```

### 285. Helm charts
- Definition: A Helm chart is a packaged collection of templates, defaults, and metadata used to generate Kubernetes manifests.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

### 286. Helm values
- Definition: Helm values are input data used to parameterize chart templates at render time.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

YAML syntax example:

```yaml
# values.yaml
replicaCount: 2
image:
  repository: nginx
  tag: "1.27"
```

### 287. Helm templating
- Definition: Helm templating renders chart templates into manifests using values and template functions.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

### 288. Helm install
- Definition: Helm install creates a new release by rendering a chart with values and submitting the resulting manifests to the API server.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

### 289. Helm upgrade
- Definition: Helm upgrade renders a new chart version or values set and applies the resulting manifest changes to an existing release.
- Change scope: any template or values delta can change Pods, Services, RBAC, CRDs, hooks, or annotations, so review rendered manifests before production rollout.
- Production workflow: lint the chart, template locally or in CI, diff against the live release, apply the upgrade, and watch rollout status of affected workloads.
- State handling: Helm stores release history so previous revisions remain available for inspection and rollback.
- Operational rule: treat Helm as a packaging layer, not as permission to bypass Kubernetes rollout safeguards such as probes, PDBs, and canary strategy.
- Verification: confirm release status, rendered manifest correctness, controller health, and application readiness after the new revision is applied.
- Failure modes: values drift, immutable field changes, broken hooks, CRD ordering issues, and charts that hide risky defaults behind templating.

### 290. Helm rollback
- Definition: Helm rollback reapplies manifests from a previous stored Helm revision to restore a known-good release state.
- Behavior: rollback changes Kubernetes resources, but it does not automatically revert external side effects such as database schema changes or out-of-band infrastructure edits.
- Production workflow: inspect release history, choose the exact revision, evaluate whether forward-only migrations exist, then execute rollback and watch controllers converge.
- Verification: confirm release revision, workload image and config state, endpoint health, and application-level correctness after rollback.
- Risk: rollback can fail on immutable fields, missing dependent secrets, or resources mutated outside Helm's last-known manifest set.
- Operational rule: use rollback as part of a release plan, not as a substitute for pre-deployment testing and progressive delivery.

### 291. Helm uninstall
- Definition: Helm uninstall removes a release record and deletes the Kubernetes resources managed by that Helm release.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

### 292. Release versioning
- Definition: Release versioning tracks successive deployed revisions of an application, chart, or manifest set.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 293. Rollout strategies
- Definition: Rollout strategies define how a new version replaces the old version, such as rolling, canary, or blue-green.
- Diagnostic role: this point is used to inspect API objects, runtime state, recent changes, or failure symptoms during normal operations or incidents.
- Production use: correlate object status, events, logs, and application behavior; no single command or symptom is sufficient on its own.
- Operational rule: establish a narrow time window, identify the first failing component, and separate cause from downstream symptoms before changing state.
- Verification: confirm the observed symptom disappears after remediation and that control-plane, node, and application signals all return to steady state.
- Failure modes: over-reliance on one tool output, ignoring events or conditions, and applying fixes before the actual dependency failure is understood.

### 294. Blue-green deployment
- Definition: Blue-green deployment runs old and new versions side by side and switches traffic between them.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 295. Canary deployment
- Definition: Canary deployment gradually exposes a small portion of traffic to a new version before full rollout.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 296. GitOps
- Definition: GitOps treats Git as the declared source of truth and uses controllers to reconcile cluster state to repository content.
- Control model: the reconciliation loop runs continuously, detecting drift and either correcting it automatically or surfacing it for operator action.
- Operational benefit: change history, peer review, and rollback become repository-driven rather than ad hoc imperative commands against production clusters.
- Production use: separate platform and application repositories or paths clearly, define environment overlays, and gate promotion through CI validation.
- Risk boundary: GitOps reconciles declared Kubernetes state, but it does not automatically make application data, external systems, or manual hotfixes safe.
- Verification: inspect sync status, last applied revision, drift state, health checks, and whether reconciled manifests match the intended environment overlay.
- Failure modes: secrets handling, unsafe auto-sync on unreviewed changes, repository drift, and controllers fighting with imperative changes from humans or other tools.

### 297. Flux
- Definition: Flux is a GitOps toolkit that reconciles cluster state from Git and related sources through controllers.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

## 18. Extensibility and Production Readiness

### 298. CustomResourceDefinition
- Definition: A CustomResourceDefinition extends the Kubernetes API with a new type that behaves like a native resource from the client's perspective.
- Schema model: CRDs define group, version, names, scope, and often an OpenAPI schema used for validation and defaulting.
- API behavior: once installed, custom resources can be listed, watched, patched, and reconciled by controllers exactly like built-in objects.
- Production use: CRDs should have versioning strategy, schema evolution discipline, status conventions, and documented ownership boundaries before adoption.
- Operational rule: changing CRD schema or versions is an API migration problem; treat it with the same care as any production contract change.
- Verification: confirm the CRD is Established, schema validation works, conversions behave correctly, and the controller that uses the resource updates status as expected.

YAML syntax example:

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: widgets.example.com
spec:
  group: example.com
  names:
    kind: Widget
    plural: widgets
  scope: Namespaced
```

### 299. Custom resources
- Definition: A custom resource is an instance of a type introduced by a CustomResourceDefinition.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
apiVersion: example.com/v1
kind: Widget
metadata:
  name: sample
spec:
  size: large
```

### 300. Operator pattern
- Definition: The operator pattern packages domain-specific operational knowledge in a controller that manages custom resources.
- Control model: the operator watches custom resources, interprets desired state, and performs complex actions such as provisioning, failover, backup, or upgrade.
- Value: it turns manual runbooks into reconciled automation while keeping the operational interface inside the Kubernetes API.
- Production use: evaluate operators for idempotency, failure handling, status reporting, upgrade compatibility, and whether they expose dangerous actions without safeguards.
- Verification: confirm reconcile loops are healthy, status conditions reflect reality, and external resources created by the operator remain consistent with desired state.
- Failure modes: controller bugs, poor status modeling, unsafe defaults, and upgrades that change CRD schema or controller behavior without a migration path.

### 301. API extensibility
- Definition: API extensibility allows Kubernetes to be extended with custom types, aggregated APIs, conversion, and admission logic.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 302. Kubernetes Namespace
- Definition: A Kubernetes Namespace is a logical scope for resource names, policy, RBAC, and quota separation.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

YAML syntax example:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: prod
```

### 303. Labeling strategy
- Definition: A labeling strategy defines consistent keys and values for ownership, environment, app identity, and selection.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 304. Environment separation
- Definition: Environment separation isolates dev, test, and production resources through namespaces, clusters, accounts, or a combination of them.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 305. Resource hygiene
- Definition: Resource hygiene is disciplined use of requests, limits, quotas, labels, and cleanup to keep clusters predictable and maintainable.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

### 306. ResourceQuota
- Definition: A ResourceQuota caps aggregate resource consumption and object counts within a namespace.
- API behavior: the concept affects how objects are represented, selected, updated, observed, or garbage-collected by the Kubernetes API.
- Production use: treat object schema and metadata as contracts because controllers, Services, policy engines, and automation depend on them.
- Operational rule: change labels, selectors, ownership, and apply field managers deliberately to avoid orphaning resources or creating controller conflicts.
- Verification: inspect the live object, managed fields, events, dependent resources, and whether the expected controller has observed the latest generation.
- Failure modes: selector drift, unmanaged field conflicts, accidental cascading deletion, and relying on status before the responsible controller updates it.

YAML syntax example:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: team-quota
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
```

### 307. LimitRange policies
- Definition: LimitRange policies enforce default, minimum, or maximum container and Pod resource settings within a namespace.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

YAML syntax example:

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: default-limits
spec:
  limits:
    - type: Container
      default:
        cpu: "500m"
        memory: "512Mi"
```

### 308. Restore
- Definition: Restore is the act of rebuilding usable state from a previously captured backup or snapshot.
- Core mechanics: the concept affects how Kubernetes expresses intent, reconciles state, or constrains runtime behavior inside the cluster.
- Production relevance: it changes how platform teams design APIs, topology, failure handling, scheduling, security, or delivery workflows.
- Operational interpretation: apply the concept through concrete resources, controller settings, policies, or lifecycle procedures rather than treating it as theory only.
- Verification: confirm the effect through object state, events, metrics, logs, and workload behavior in the relevant failure or change scenario.
- Common failure mode: misuse usually appears as configuration drift, hidden coupling, availability loss, or assumptions that do not match actual controller behavior.

### 309. Upgrade planning
- Definition: Upgrade planning is the technical preparation required before changing Kubernetes, node image, addon, or workload platform versions in production.
- Compatibility analysis: validate target versions for the API server, kubelet, kubectl, CNI, CSI, ingress, metrics, GitOps, and security tooling.
- Risk analysis: identify immutable field changes, deprecated APIs, CRD conversions, admission changes, and workloads that depend on removed behavior.
- Execution design: define maintenance windows, canary clusters or node pools, communication plans, backup checkpoints, and explicit stop conditions.
- Verification plan: decide what signals prove success—API health, node readiness, DNS, storage mounts, application SLOs, and business transaction tests.
- Rollback plan: document which layers can be rolled back safely and which require forward-fix only behavior, especially around database and CRD migrations.

### 310. Version skew awareness
- Definition: Version skew awareness is understanding the supported version distance between Kubernetes components during upgrade or mixed-version operation.
- Critical rule: the API server generally leads; kubelets and clients may lag only within documented skew limits for the target release line.
- Operational impact: ignoring skew limits can break node registration, admission, API compatibility, addon behavior, or controller expectations.
- Production use: plan upgrades by component family and verify that managed services, addons, and tooling all support the target skew window.
- Verification: compare actual server, kubelet, and client versions cluster-wide instead of assuming node pools are uniform.
- Risk: long-lived skew increases the chance of hidden API deprecations, unsupported flags, and inconsistent operational behavior across nodes.

### 311. Runbooks
- Definition: Runbooks are documented step-by-step procedures for common incidents, maintenance tasks, and recovery actions.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.

### 312. Operational discipline
- Definition: Operational discipline is consistent execution of change control, validation, observability, backup, and incident response procedures.
- Preconditions: confirm scope, dependencies, version or policy constraints, and whether the action affects control-plane availability or workload disruption.
- Execution: perform the change in ordered steps, limiting concurrency and observing the system after each step before continuing.
- Production safeguards: preserve quorum, honor PodDisruptionBudgets, take backups when state is involved, and freeze unrelated changes during execution.
- Verification: check component health, events, logs, status conditions, and application behavior instead of relying on command success alone.
- Failure handling: define a rollback or forward-fix path before the operation starts, especially when schema or persistent data changes are involved.
- Risk: the most common failure pattern is hidden dependency breakage in DNS, admission, storage, networking, or external integrations.

### 313. Production best practices
- Definition: Production best practices are design and operational defaults that improve security, reliability, recoverability, and maintainability of Kubernetes platforms.
- Platform role: this point shapes how clusters are packaged, promoted, extended, or governed as reusable platform infrastructure.
- Production use: standardize repository layout, ownership, naming, overlays or values, policy defaults, and promotion flow across environments.
- Operational rule: keep declarative sources authoritative and review manifest or API changes before reconciliation reaches production.
- Verification: inspect rendered manifests, sync status, API establishment, policy enforcement, and whether platform defaults are applied consistently.
- Failure modes: configuration drift, template abstraction hiding risky changes, CRD version mistakes, and governance rules that differ between environments.
