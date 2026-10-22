# Kubernetes Technical Notes — Reference Grade Revision

Ordered technical notes aligned to the canonical master list. Each point is defined in sequence. Examples appear only where they materially improve the note. Operational procedures are included only for points that require execution steps.

## 1. Foundations and Core Concepts

### 1. What Kubernetes is

Kubernetes is a distributed control plane for containerized workloads that persists desired state in the API, schedules Pods onto nodes, and runs controllers that converge actual state toward the declared target.

### 2. Problems Kubernetes solves

Kubernetes solves multi-node container placement, service discovery, rolling updates, failure recovery, configuration injection, secret delivery, and persistent-storage orchestration across a fleet of hosts.

### 3. Containers vs virtual machines

Containers isolate processes with kernel features such as namespaces and cgroups while sharing the host kernel. Virtual machines emulate hardware, run separate guest kernels, and impose a heavier but stronger isolation boundary.

### 4. Container lifecycle basics

Container lifecycle basics cover image pull, sandbox creation, process start, health probing, restart handling, termination signaling, and garbage collection on the node.

### 5. Container images

Container images are immutable OCI artifacts that package a root filesystem, metadata, environment defaults, entrypoint settings, and content digests for reproducible execution.

### 6. Image layers

Image layers are read-only filesystem deltas stacked by the runtime to assemble the container root filesystem, with layer reuse keyed by content digest.

### 7. Container registries

Container registries store and serve image manifests, manifest lists, layer blobs, and related metadata over the OCI or Docker distribution API.

### 8. Image tags and image digests

An image tag is a mutable human-readable reference such as `nginx:1.27`, while an image digest is an immutable content-addressed identifier such as `sha256:<hash>` that identifies one exact manifest.

### 9. Linux namespaces

Linux namespaces isolate kernel-visible resources such as process IDs, mounts, network interfaces, IPC objects, hostnames, and users between process groups.

### 10. cgroups

cgroups account for and constrain CPU, memory, I/O, and process consumption for a process group, and Kubernetes relies on them to enforce Pod and container resource controls.

### 11. Union filesystems

Union filesystems compose multiple read-only layers with one writable upper layer so a container sees a merged filesystem view without changing the original image layers.

### 12. Process isolation

Process isolation prevents processes in one containerized context from directly observing or signaling unrelated processes unless namespace sharing or elevated privilege is configured.

### 13. Network isolation

Network isolation gives a Pod or containerized context its own network namespace, interfaces, routing table, and firewall context unless host networking is used.

### 14. Filesystem isolation

Filesystem isolation separates a container root filesystem from the host filesystem by mounting a runtime-assembled root and exposing only explicitly mounted volumes or paths.

### 15. Ephemeral workloads

Ephemeral workloads are workloads whose state is expected to be lost on restart or rescheduling, so durability is not tied to the lifecycle of one Pod instance.

### 16. Immutable infrastructure concepts

Immutable infrastructure concepts treat runtime artifacts and node images as replaceable units so changes are applied by replacement or redeployment rather than uncontrolled in-place mutation.

### 17. Declarative operations

Declarative operations submit the intended target state to the API and let controllers determine the actions needed to reach that state.

### 18. Imperative operations

Imperative operations issue a direct action such as create, scale, or delete immediately through a command or API call rather than only describing the final target.

### 19. Desired state

Desired state is the user-declared target configuration stored in `.spec` fields and other declarative API objects.

### 20. Reconciliation loop

A reconciliation loop reads observed state, compares it with desired state, and writes corrective API changes until the resource converges.

### 21. Control loop model

The control loop model is the Kubernetes operating pattern in which independent controllers watch resources and continuously drive state toward policy-defined targets.

### 22. Idempotency

Idempotency means the same reconciliation action can run repeatedly and still produce one correct final state without duplicating side effects.

### 23. Self-healing

Self-healing is the automatic replacement, restart, or rescheduling of failed workload instances until the requested healthy state is restored.

### 24. Horizontal scaling concepts

Horizontal scaling changes capacity by increasing or decreasing the number of workload replicas or backing nodes.

### 25. Vertical scaling concepts

Vertical scaling changes capacity by increasing or decreasing the CPU, memory, or other resource allocations of an existing workload instance.

### 26. High availability concepts

High availability concepts aim to keep a service reachable despite component failure by removing single points of failure and distributing replicas across failure domains.

### 27. Fault domains

Fault domains are infrastructure boundaries within which one failure event can affect multiple components, such as one host, rack, zone, or control-plane node.

### 28. Failure domains

Failure domains are the placement boundaries used to spread replicas so one infrastructure failure does not remove every service instance at once.

### 29. Platform engineering with Kubernetes

Platform engineering with Kubernetes builds reusable abstractions, policies, and delivery paths on top of Kubernetes so application teams consume a safer and more standardized platform.

### 30. Managed vs self-managed Kubernetes

Managed Kubernetes delegates part of the control-plane lifecycle to a provider, whereas self-managed Kubernetes requires the operator to install, upgrade, secure, and recover every cluster component.

### 31. Kubernetes distribution types

Kubernetes distribution types are vendor or project variants that package Kubernetes with different installers, defaults, operating systems, support models, and integrated add-ons.

## 2. Cluster Architecture and Topology

### 32. Cluster architecture

Cluster architecture is the overall composition of control-plane components, worker nodes, networking, storage integrations, and failure boundaries that together provide the Kubernetes execution environment.

### 33. Control plane vs worker nodes

Control-plane nodes run kube-apiserver, scheduler, controller-manager, and often etcd, while worker nodes run kubelet, the container runtime, the Service dataplane, and application Pods.

### 34. Single-node cluster model

A single-node cluster model places control-plane components and workloads on one host, which simplifies testing but leaves no host-level redundancy.

### 35. Multi-node cluster model

A multi-node cluster model separates scheduling and workload execution across multiple hosts so compute capacity and failure tolerance scale independently.

### 36. Highly available control plane

A highly available control plane runs multiple API-server instances and redundant controller and scheduler instances behind one stable endpoint so the cluster survives individual control-plane node failures.

### 37. Stacked etcd topology

Stacked etcd topology places one etcd member on each control-plane node so the same hosts run both Kubernetes control-plane processes and the etcd quorum.

### 38. External etcd topology

External etcd topology runs etcd on dedicated hosts separate from control-plane nodes, reducing resource contention and isolating quorum operations from API-server hosts.

### 39. Control plane endpoint

The control plane endpoint is the stable network address clients and nodes use to reach the Kubernetes API, typically fronted by a load balancer or virtual IP.

### 40. Control plane load balancer

A control plane load balancer distributes API traffic across multiple kube-apiserver instances and presents one stable endpoint to kubectl, kubelets, and controllers.

### 41. North-south traffic model

North-south traffic model describes how traffic flows either between external clients and cluster services or between internal services and Pods inside or across cluster boundaries.

### 42. East-west traffic model

East-west traffic model describes how traffic flows either between external clients and cluster services or between internal services and Pods inside or across cluster boundaries.

### 43. Single-cluster design

Single-cluster design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 44. Multi-cluster design

Multi-cluster design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 45. Hub-and-spoke fleet model

Hub-and-spoke fleet model is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 46. Regional cluster design

Regional cluster design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 47. Multi-region cluster strategy

Multi-region cluster strategy is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 48. Private cluster design

Private cluster design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 49. Air-gapped cluster design

Air-gapped cluster design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 50. Edge cluster design

Edge cluster design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 51. Hybrid cloud cluster design

Hybrid cloud cluster design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 52. On-premises cluster design

On-premises cluster design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 53. Blast radius design

Blast radius design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 54. Availability zone design

Availability zone design is a topology choice that determines placement boundaries, network reachability, administrative isolation, and the impact radius of infrastructure failure.

### 55. Control plane failure scenarios

Control plane failure scenarios is the failure analysis for that cluster role, including what becomes unavailable, what remains healthy, and how recovery proceeds after host, zone, or dependency loss.

### 56. Worker node failure scenarios

Worker node failure scenarios is the failure analysis for that cluster role, including what becomes unavailable, what remains healthy, and how recovery proceeds after host, zone, or dependency loss.

## 3. Cluster Bootstrapping, Installation, and PKI

### 57. Cluster bootstrap concepts

Cluster bootstrap concepts cover the sequence that creates a trust root, initializes etcd and control-plane manifests, and establishes the first administrative API access path.

### 58. kubeadm

kubeadm is the reference bootstrap tool that creates Kubernetes PKI assets, writes static Pod manifests for control-plane components, and manages cluster join and upgrade workflows.

### 59. kubeadm init

kubeadm init is a kubeadm command that either initializes the first control plane or joins an additional worker or control-plane node to an existing cluster.

### 60. kubeadm join

kubeadm join is a kubeadm command that either initializes the first control plane or joins an additional worker or control-plane node to an existing cluster.

### 61. Bootstrap tokens

Bootstrap tokens is a bootstrap authentication artifact used during node discovery and initial trust establishment.

### 62. Discovery token CA cert hash

Discovery token CA cert hash is a bootstrap authentication artifact used during node discovery and initial trust establishment.

### 63. Static Pod control plane bootstrap

Static Pod control plane bootstrap starts core control-plane components by placing manifest files under the kubelet static Pod path so kubelet launches them directly from the host.

### 64. PKI directory layout

PKI directory layout is the file structure that stores cluster CAs, component client certificates, serving certificates, front-proxy trust material, and service-account signing keys.

### 65. Admin kubeconfig

Admin kubeconfig is a kubeconfig file that defines a cluster endpoint, trust anchor, and client identity for a specific Kubernetes component or user.

### 66. Controller manager kubeconfig

Controller manager kubeconfig is a kubeconfig file that defines a cluster endpoint, trust anchor, and client identity for a specific Kubernetes component or user.

### 67. Scheduler kubeconfig

Scheduler kubeconfig is a kubeconfig file that defines a cluster endpoint, trust anchor, and client identity for a specific Kubernetes component or user.

### 68. Kubelet kubeconfig

Kubelet kubeconfig is a kubeconfig file that defines a cluster endpoint, trust anchor, and client identity for a specific Kubernetes component or user.

### 69. Kubeconfig structure

Kubeconfig structure is a kubeconfig file that defines a cluster endpoint, trust anchor, and client identity for a specific Kubernetes component or user.

### 70. Contexts, clusters, and users in kubeconfig

Contexts, clusters, and users in kubeconfig is a kubeconfig file that defines a cluster endpoint, trust anchor, and client identity for a specific Kubernetes component or user.

### 71. ClusterConfiguration

ClusterConfiguration is a kubeadm configuration API object that carries bootstrap parameters, component settings, and cluster-installation metadata.

### 72. InitConfiguration

InitConfiguration is a kubeadm configuration API object that carries bootstrap parameters, component settings, and cluster-installation metadata.

### 73. JoinConfiguration

JoinConfiguration is a kubeadm configuration API object that carries bootstrap parameters, component settings, and cluster-installation metadata.

### 74. KubeletConfiguration

KubeletConfiguration is a kubeadm configuration API object that carries bootstrap parameters, component settings, and cluster-installation metadata.

### 75. KubeProxyConfiguration

KubeProxyConfiguration is a kubeadm configuration API object that carries bootstrap parameters, component settings, and cluster-installation metadata.

### 76. Control plane certificate upload

Control plane certificate upload is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

### 77. Joining additional control plane nodes

Joining additional control plane nodes is part of cluster installation, join, or trust establishment.

### 78. Certificate generation during bootstrap

Certificate generation during bootstrap is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

### 79. Certificate renewal

Certificate renewal is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

**Operational steps**
1. Check current expiry with `kubeadm certs check-expiration` or the platform certificate inventory workflow.
2. Identify the issuing CA or external signer for the expiring certificate set.
3. Renew the certificate with `kubeadm certs renew` or the approved external PKI workflow.
4. Replace the renewed certificate and private key on the affected node or component host.
5. Restart the component that consumes the renewed certificate so it reloads the file.
6. Validate TLS handshakes, component readiness, and API reachability after renewal.

### 80. Certificate rotation

Certificate rotation is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

**Operational steps**
1. Determine whether the component rotates certificates automatically, through CSR approval, or through manual replacement.
2. Confirm the signer, trust chain, and certificate file path for the new credential.
3. Trigger or approve the rotation before the active certificate reaches expiry.
4. Reload or restart the consuming component if it does not hot-reload certificates.
5. Verify the new serial number, expiration date, and successful authenticated communication.

### 81. Kubelet client certificate rotation

Kubelet client certificate rotation is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

**Operational steps**
1. Confirm kubelet is configured with client certificate rotation enabled.
2. Inspect pending certificate signing requests with `kubectl get csr`.
3. Approve the kubelet client CSR if the environment uses manual approval.
4. Verify kubelet writes the new client certificate into its configured certificate directory.
5. Check node readiness and kubelet logs for authentication success against the API server.

### 82. Kubelet serving certificate rotation

Kubelet serving certificate rotation is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

**Operational steps**
1. Confirm serving certificate rotation is enabled and that consumers trust the configured signer.
2. Inspect pending serving CSRs and approve them if policy requires manual approval.
3. Verify the kubelet HTTPS endpoint presents the renewed certificate.
4. Test metrics, exec, or debug flows that rely on kubelet HTTPS.
5. Review kubelet logs for serving-certificate reload failures.

### 83. Front-proxy certificates

Front-proxy certificates is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

### 84. Service account signing keys

Service account signing keys is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

### 85. External CA model

External CA model is a PKI artifact or PKI process used to issue, rotate, validate, or distribute trust material between Kubernetes components.

### 86. Bootstrapping stacked etcd

Bootstrapping stacked etcd is the initialization or provisioning step for that control-plane or node topology.

### 87. Bootstrapping external etcd

Bootstrapping external etcd is the initialization or provisioning step for that control-plane or node topology.

### 88. Addon installation during bootstrap

Addon installation during bootstrap is the initialization or provisioning step for that control-plane or node topology.

### 89. CNI installation after bootstrap

CNI installation after bootstrap is the initialization or provisioning step for that control-plane or node topology.

### 90. Bare-metal installation patterns

Bare-metal installation patterns is the initialization or provisioning step for that control-plane or node topology.

### 91. Managed cluster provisioning workflows

Managed cluster provisioning workflows is the initialization or provisioning step for that control-plane or node topology.

## 4. API, Object Model, and Resource Semantics

### 92. Kubernetes API

The Kubernetes API is the versioned HTTP interface through which every object is created, retrieved, updated, deleted, listed, watched, or accessed through subresources.

### 93. API groups

API groups partition resources into logical domains such as `apps`, `batch`, and `networking.k8s.io`, allowing independent version evolution.

### 94. API versions

API versions identify the schema generation and stability level of a resource endpoint, such as `v1`, `v1beta1`, or `v1alpha1`.

### 95. Core API group

The core API group is the ungrouped legacy API surface that contains resources such as Pod, Service, Secret, ConfigMap, and Namespace.

### 96. Namespaced resources

Namespaced resources exist inside one namespace scope and can reuse names that already exist in a different namespace.

### 97. Cluster-scoped resources

Cluster-scoped resources exist at cluster scope and are not placed in a namespace.

### 98. Kinds

Kind identifies the schema type of an API object, such as Pod, Deployment, or Service.

### 99. Resources

A resource is the REST-addressable plural endpoint representation of a kind, such as `pods`, `deployments`, or `services`.

### 100. Objects

Objects are persisted API instances that combine metadata with desired and observed fields according to the selected kind and version.

### 101. Metadata

Metadata contains identity and control fields such as name, namespace, labels, annotations, owner references, and deletion state.

### 102. Spec

Spec contains the desired state that a user or controller declares for a resource.

### 103. Status

Status contains the observed state that controllers, kubelet, or system components write back after processing the resource.

### 104. Labels

Labels are indexed key-value pairs used for selection, grouping, rollout targeting, and service membership.

### 105. Equality-based label selectors

Equality-based label selectors match objects by exact key-value comparison or inequality, such as `app=web` or `tier!=backend`.

### 106. Set-based label selectors

Set-based label selectors match objects with membership operators such as `in`, `notin`, and `exists`.

### 107. Annotations

Annotations are non-indexed key-value metadata fields used for integration hints, checksums, ownership metadata, or controller-specific state.

### 108. Namespaces

Namespaces partition namespaced resources into isolated API scopes commonly used for tenancy, policy boundaries, and quota enforcement.

### 109. UIDs

A UID is the immutable system-generated identity of one object instance and does not change across normal object updates.

### 110. OwnerReferences

OwnerReferences declare ownership relationships so garbage collection can delete dependent objects when their owner is removed.

### 111. Finalizers

Finalizers are metadata entries that block final deletion until a responsible controller performs cleanup and removes its finalizer string.

### 112. Generation

Generation is the desired-state revision counter that increases when spec fields change.

### 113. ObservedGeneration

ObservedGeneration records the latest generation value a controller has processed and reflected into status.

### 114. resourceVersion

resourceVersion is the storage revision token used for optimistic concurrency, list consistency, and watch continuation.

### 115. CreationTimestamp

CreationTimestamp is the server-assigned time when the API server first persisted the object.

### 116. DeletionTimestamp

DeletionTimestamp marks that deletion has started and that finalization or graceful-termination rules are now active.

### 117. Managed fields

Managed fields record field ownership metadata for server-side apply and conflict detection.

### 118. Server-side apply

Server-side apply is the declarative merge mechanism in which the API server tracks per-field ownership by field manager.

### 119. Field management

Field management is the ownership model that records which actor last managed each field path during apply or update operations.

### 120. Field ownership conflicts

Field ownership conflicts occur when one field manager tries to apply a value to a field path owned by another manager.

### 121. Strategic merge patch

Strategic merge patch is a Kubernetes-specific patch format that merges lists using schema-defined merge keys for built-in resource types.

### 122. JSON merge patch

JSON merge patch merges or replaces object fragments according to JSON object structure without Kubernetes-specific list merge behavior.

### 123. JSON patch

JSON patch applies an ordered list of operations such as add, remove, replace, move, copy, or test to a JSON document.

### 124. YAML manifest structure

YAML manifest structure is the declarative document layout that includes `apiVersion`, `kind`, `metadata`, and resource-specific fields.

### 125. apiVersion field

The `apiVersion` field selects the API group and version used to decode, default, and validate the manifest.

### 126. kind field

The `kind` field declares which object schema the API server should instantiate.

### 127. metadata field

The `metadata` field carries identity and control-plane metadata such as name, namespace, labels, annotations, and owner references.

### 128. spec field

The `spec` field contains the user-declared desired configuration for the resource.

### 129. status field

The `status` field contains system-reported observed state for the resource.

### 130. OpenAPI schema

OpenAPI schema is the published structural description of resource fields, types, defaults, validation rules, and documentation.

### 131. Defaulting

Defaulting is the server-side insertion of omitted field values according to API behavior or schema defaults.

### 132. Validation

Validation is the server-side rejection of objects whose types, values, or field combinations violate resource rules.

### 133. Schema pruning

Schema pruning removes undeclared fields from an object when the resource schema does not preserve unknown fields.

### 134. Unknown field handling

Unknown field handling defines whether undeclared fields are rejected, pruned, or preserved based on the resource schema and API behavior.

### 135. Nullable fields

Nullable fields are schema paths that explicitly allow the JSON value `null` instead of only a concrete typed value.

### 136. Subresources

Subresources are specialized API paths such as `/status`, `/scale`, or `/eviction` that isolate a specific update or action surface from the parent resource.

### 137. Status subresource

The status subresource allows controllers and system components to update observed state without mutating spec fields.

### 138. Scale subresource

The scale subresource exposes a generic replica interface used by autoscalers and clients to read or change desired replica counts.

### 139. List operations

List operations return the current collection state for a resource type, optionally filtered by namespace, labels, fields, and pagination settings.

### 140. Watch operations

Watch operations stream resource change events after a given resourceVersion so clients can react incrementally to state transitions.

### 141. List-watch pattern

The list-watch pattern begins with a full list and then continues with a watch so informers can maintain a coherent local cache.

### 142. Watch bookmarks

Watch bookmarks are lightweight watch events that let a client advance its resourceVersion without receiving a full object payload.

### 143. Field selectors

Field selectors filter objects by selected metadata or status fields such as node name, phase, or object name.

### 144. Pagination and continue tokens

Pagination and continue tokens split a large list response into multiple consistent chunks without restarting the query from the beginning.

### 145. Graceful deletion semantics

Graceful deletion semantics delay final object removal for a configured grace period so workloads can terminate cleanly before forceful removal.

### 146. Optimistic concurrency

Optimistic concurrency is the update model in which clients use resourceVersion to avoid overwriting newer changes from another writer.

## 5. kube-apiserver and API Machinery

### 147. kube-apiserver role

The kube-apiserver is the entry point of the Kubernetes control plane. It authenticates requests, authorizes identities, runs admission, validates objects, and persists state to etcd.

### 148. API request lifecycle

API request lifecycle is the ordered flow of authentication, authorization, admission, validation, persistence, and response handling for an incoming API call.

### 149. Authentication pipeline

The authentication pipeline is the sequence of authenticators the API server evaluates to establish the caller identity.

### 150. Authorization pipeline

The authorization pipeline is the sequence of authorizers that decide whether the authenticated caller may perform the requested verb on the target resource.

### 151. Admission chain

The admission chain is the ordered set of mutating and validating admission stages that run after authorization and before persistence.

### 152. Mutating admission

Mutating admission can modify an incoming object before it is validated and stored.

### 153. Validating admission

Validating admission accepts or rejects the final request object after mutation is complete.

### 154. Built-in admission plugins

Built-in admission plugins are in-process policy modules compiled into the API server for defaulting, validation, and enforcement tasks.

### 155. Admission webhooks

Admission webhooks are external HTTP callbacks the API server invokes to mutate or validate objects during admission.

### 156. Admission webhook failurePolicy

Admission webhook failurePolicy controls whether a request fails closed or continues when the webhook cannot be reached or returns an error.

### 157. Admission webhook matchPolicy

Admission webhook matchPolicy controls how requests across equivalent API versions are matched to webhook rules.

### 158. Admission webhook sideEffects

Admission webhook sideEffects declares whether a webhook performs side effects, which affects dry-run handling and request safety.

### 159. Admission webhook timeout behavior

Admission webhook timeout behavior is the maximum wait period before the API server treats the webhook call as timed out and applies failurePolicy.

### 160. ValidatingAdmissionPolicy

ValidatingAdmissionPolicy is the API-native admission mechanism that evaluates CEL expressions against objects without requiring an external webhook.

### 161. CEL-based admission rules

CEL-based admission rules use Common Expression Language expressions to validate requests directly inside the API server.

### 162. MutatingAdmissionPolicy concepts

MutatingAdmissionPolicy concepts cover declarative mutation policy implemented through API-native mechanisms rather than external webhook code paths.

### 163. Conversion webhooks

Conversion webhooks translate custom resources between served API versions so one object can be safely read and written across multiple versions.

### 164. CRD registration

CRD registration publishes a new custom resource type into API discovery, schema validation, and storage handling.

### 165. API aggregation layer

The API aggregation layer lets non-core API servers appear under the main Kubernetes API endpoint and discovery surface.

### 166. Aggregated API servers

Aggregated API servers are delegated extension servers that expose additional API groups through the main API server.

### 167. Aggregated API server failure modes

Aggregated API server failure modes are outages, latency spikes, or TLS problems in delegated extension servers that degrade discovery or specific API groups.

### 168. OpenAPI publishing

OpenAPI publishing exposes resource schemas and field documentation to clients for validation, discovery, and tooling.

### 169. Discovery endpoints

Discovery endpoints advertise the API groups, versions, and resources supported by the cluster so clients can discover capabilities dynamically.

### 170. Storage version conversion

Storage version conversion is the translation of an object between the version stored in etcd and the version served to a client.

### 171. StorageVersion API concepts

StorageVersion API concepts describe which version of a resource is persisted and how storage-version migration can be coordinated.

### 172. API deprecation policy

API deprecation policy defines how long deprecated API versions remain served and what notice is given before removal in later Kubernetes releases.

### 173. Feature gates

Feature gates are boolean switches that enable or disable optional Kubernetes features in specific components at startup.

### 174. Audit logging

Audit logging is the emission of structured records describing API requests and their handling outcome.

### 175. Audit policy

Audit policy determines which requests are logged and what level of detail is captured at each stage.

### 176. Audit backends

Audit backends are the output targets, such as files or webhooks, to which audit events are written.

### 177. Audit stages

Audit stages are the points in request processing, such as request received or response complete, at which audit events may be emitted.

### 178. Encryption at rest

Encryption at rest is the API-server-side encryption of persisted object data before it is written to etcd.

### 179. Encryption providers

Encryption providers are the configured mechanisms, such as AES or KMS-backed envelopes, used by the API server to encrypt stored data.

### 180. KMS integration

KMS integration delegates encryption or data-key handling to an external key-management system.

### 181. KMS v2 concepts

KMS v2 concepts cover the newer gRPC-based key-management integration model and its performance and envelope-encryption semantics.

### 182. API priority and fairness

API Priority and Fairness classifies requests and allocates concurrency shares under load so critical traffic is protected.

### 183. Flow schemas

Flow schemas match incoming requests into priority levels based on user, verb, resource, and other request attributes.

### 184. Priority levels

Priority levels define concurrency shares, queueing behavior, and rejection handling for matched API request flows.

### 185. API priority and fairness tuning

API Priority and Fairness tuning is the adjustment of flow schemas and priority levels so important request classes retain service during overload.

### 186. Long-running requests

Long-running requests are requests such as watch, exec, attach, and port-forward whose lifetime differs from ordinary CRUD calls and may bypass some throttling paths.

### 187. Request timeout handling

Request timeout handling is the API-server behavior that terminates requests exceeding configured deadlines or webhook wait thresholds.

### 188. API server horizontal scaling

API server horizontal scaling is the addition of multiple kube-apiserver instances behind one endpoint to increase concurrency and availability.

### 189. API server high availability

API server high availability is the deployment of redundant kube-apiserver instances so API access continues during instance loss.

### 190. API server identity leases

API server identity leases are lease objects used by API servers to advertise distinct identities within a multi-instance control plane.

### 191. Front-proxy pattern

The front-proxy pattern places a trusted proxy in front of the API server and forwards authenticated identity information through signed request headers.

### 192. Request header authentication

Request header authentication trusts identity headers added by a configured front proxy that is itself authenticated by the API server.

### 193. Client certificate authentication

Client certificate authentication verifies the caller identity from an X.509 certificate signed by a trusted CA.

### 194. Bearer token authentication

Bearer token authentication verifies a token presented by the caller against configured token authenticators or issuers.

### 195. OIDC authentication

OIDC authentication lets the API server authenticate users through an external OpenID Connect issuer and map token claims into Kubernetes identities.

### 196. Webhook token authentication

Webhook token authentication delegates bearer-token validation to an external HTTP service.

### 197. Anonymous authentication

Anonymous authentication is the fallback API-server identity mode in which unauthenticated requests are treated as the anonymous user if enabled.

### 198. RBAC authorization

RBAC authorization evaluates the request against Role and ClusterRole policy rules bound to the caller.

### 199. Webhook authorization

Webhook authorization delegates permission evaluation to an external HTTP authorizer service.

### 200. Node authorization

Node authorization is the special authorizer that limits kubelet requests to objects the node is allowed to access or modify.

### 201. NodeRestriction admission

NodeRestriction admission prevents kubelets from modifying resources outside a constrained node-scoped security boundary.

### 202. Impersonation

Impersonation lets a caller ask the API server to evaluate a request as if it were made by another user, group, or service account, subject to explicit impersonation permission.

## 6. etcd and State Storage

### 203. etcd role

etcd is the strongly consistent key-value database that stores Kubernetes object state, revision history, leases, and control-plane coordination data.

### 204. Key-value storage model

The key-value storage model persists each object under a path-like key and stores its serialized value plus revision metadata.

### 205. Raft consensus

Raft consensus is the replicated log protocol etcd uses to keep one ordered history of writes across a quorum of members.

### 206. Raft leader election

Raft leader election chooses one member to coordinate log replication and commit decisions after startup or leader failure.

### 207. Raft followers

Raft followers replicate log entries from the leader and participate in quorum acknowledgments but do not accept authoritative writes directly.

### 208. Raft quorum

Raft quorum is the majority of voting members required to commit a write or elect a new leader.

### 209. Write-ahead log

The write-ahead log records proposed state changes durably before they are applied to the main database state.

### 210. Snapshots

Snapshots are point-in-time serialized database captures used to compact log history and support backup or restore.

### 211. Snapshot restore

Snapshot restore rebuilds an etcd data directory from a saved snapshot and resets member metadata for a new or recovered cluster.

### 212. Cluster membership

Cluster membership is the set of etcd peers participating in log replication and quorum decisions.

### 213. Member add

Member add introduces a new etcd peer into cluster membership and starts state replication to it.

### 214. Member remove

Member remove permanently removes an etcd peer from quorum participation and replication.

### 215. Learner members

Learner members receive replicated data without voting rights until they catch up sufficiently to be promoted.

### 216. Compaction

Compaction discards old historical revisions past a target revision so storage growth and watch history remain bounded.

### 217. Defragmentation

Defragmentation rewrites the backend database to reclaim free on-disk space left after deletes and compaction.

### 218. Revision history

Revision history is the ordered sequence of object versions and database revisions retained for watches and historical comparison.

### 219. Watch history window

The watch history window is the range of retained revisions from which a watch can resume before the client must relist.

### 220. Consistent reads

Consistent reads return data aligned with the current cluster state as defined by the consistency mode selected by the client.

### 221. Linearizable reads

Linearizable reads guarantee the read reflects the latest committed state known to the cluster quorum.

### 222. Serializable reads

Serializable reads may be served by a local member without the strongest global freshness guarantee, reducing coordination cost.

### 223. Lease semantics in etcd

Lease semantics in etcd associate keys with a time-bound lease so keys can expire automatically if the lease is not renewed.

### 224. Peer communication

Peer communication is the etcd member-to-member traffic used for log replication, heartbeats, and leadership coordination.

### 225. Peer TLS

Peer TLS protects member-to-member etcd communication with mutual certificate authentication and encrypted transport.

### 226. Client TLS

Client TLS protects etcd client access, such as kube-apiserver reads and writes, with certificate-based trust.

### 227. Endpoint health

Endpoint health checks whether an etcd endpoint is reachable and can participate correctly in cluster operations.

### 228. Endpoint status

Endpoint status reports metadata such as leader identity, applied index, database size, and raft term for each etcd endpoint.

### 229. Alarm conditions

Alarm conditions are etcd-protected states such as space exhaustion that block or restrict normal write behavior until remediated.

### 230. Space quota exhaustion

Space quota exhaustion occurs when the backend database reaches its configured quota, after which writes fail until space is reclaimed.

### 231. Fragmentation effects

Fragmentation effects are wasted backend pages and inflated database files that increase disk usage and can hurt etcd performance.

### 232. IOPS sensitivity

IOPS sensitivity describes etcd’s dependence on low-latency durable storage because each committed write requires synchronous disk activity.

### 233. Latency sensitivity

Latency sensitivity describes etcd’s dependence on fast peer and client round trips because quorum replication and linearizable reads are network-bound.

### 234. Data corruption scenarios

Data corruption scenarios are failures in which backend files, snapshots, or WAL contents become invalid and prevent safe database startup or replication.

### 235. Backup strategy

Backup strategy is the plan for how often snapshots are taken, where they are stored, and how restore is regularly tested.

### 236. Restore validation

Restore validation is the practice of rebuilding etcd from a snapshot in a test path to verify the backup is usable.

### 237. Disaster recovery for etcd

Disaster recovery for etcd is the full sequence used to restore database state, recreate healthy membership, and reconnect kube-apiserver after severe failure.

## 7. Scheduler, Placement, and Eviction

### 238. kube-scheduler role

The kube-scheduler selects a node for each unscheduled Pod by filtering infeasible nodes, scoring feasible nodes, and posting a binding decision.

### 239. Scheduling queue

The scheduling queue is the internal queue set that holds Pods waiting for scheduling attempts.

### 240. Active queue

The active queue contains Pods ready for immediate scheduling evaluation.

### 241. Backoff queue

The backoff queue temporarily holds Pods that recently failed scheduling so they are retried after a delay instead of causing tight retry loops.

### 242. Unschedulable queue

The unschedulable queue holds Pods that currently have no feasible node based on the latest cluster state.

### 243. Scheduling cycle

The scheduling cycle is the phase in which the scheduler selects one feasible node for a Pod by running plugins over candidate nodes.

### 244. Binding cycle

The binding cycle is the phase in which the scheduler commits the selected node assignment and runs any post-selection hooks.

### 245. Filter phase

The filter phase removes nodes that cannot satisfy hard Pod requirements such as resource requests, taints, affinity rules, or volume constraints.

### 246. Score phase

The score phase assigns relative scores to feasible nodes so the scheduler can prefer one node over another.

### 247. Normalize scoring

Normalize scoring scales or adjusts plugin-generated scores into a comparable range before final node selection.

### 248. Reserve phase

The reserve phase records provisional resource usage and other claims on the chosen node before the actual bind occurs.

### 249. Permit phase

The permit phase lets plugins delay or reject binding until an external condition, gang-scheduling condition, or policy gate is satisfied.

### 250. PreBind phase

The PreBind phase runs final preparation steps such as volume-binding operations before the scheduler writes the Pod-to-node binding.

### 251. Bind phase

The bind phase writes the Pod binding to the API so kubelet on the selected node begins Pod admission.

### 252. PostBind phase

The PostBind phase runs plugin hooks after a successful bind for cleanup or bookkeeping.

### 253. Preemption

Preemption is the scheduler action that evicts lower-priority Pods from a node so a higher-priority Pod can become schedulable.

### 254. Nominated node

A nominated node is the scheduler’s provisional target for a preempting Pod while victim Pod eviction is still in progress.

### 255. PriorityClass

PriorityClass defines an integer priority value and optional preemption policy that Pods can reference.

### 256. Pod priority

Pod priority is the numeric importance assigned to a Pod and used during queue ordering and preemption decisions.

### 257. NodeResourcesFit

NodeResourcesFit is the scheduler plugin that checks whether a node has enough allocatable CPU, memory, ephemeral storage, and other requested resources for the Pod.

### 258. nodeSelector

nodeSelector is a simple exact-match node label constraint that restricts a Pod to nodes carrying specific labels.

### 259. Node affinity

Node affinity is the scheduler rule set that expresses required or preferred node-label-based placement constraints.

### 260. Required node affinity

Required node affinity is the hard node-label constraint that must be satisfied for scheduling to succeed.

### 261. Preferred node affinity

Preferred node affinity is the soft node-label preference that influences scoring but does not block scheduling.

### 262. Pod affinity

Pod affinity attracts a Pod toward nodes already running Pods with selected labels in a specified topology domain.

### 263. Pod anti-affinity

Pod anti-affinity repels a Pod from nodes or topology domains already running Pods with selected labels.

### 264. Inter-pod affinity topology key

The inter-pod affinity topology key defines the node label domain, such as zone or hostname, over which Pod affinity or anti-affinity is evaluated.

### 265. Topology spread constraints

Topology spread constraints limit skew in replica placement across topology domains so Pods are distributed more evenly.

### 266. Taints

Taints are node marks that repel Pods unless those Pods declare matching tolerations.

### 267. Tolerations

Tolerations are Pod rules that allow scheduling onto or continued execution on nodes carrying specific taints.

### 268. Taint-based eviction

Taint-based eviction removes Pods from a node when certain taints, such as `NoExecute`, are applied and not tolerated.

### 269. Extended resources

Extended resources are schedulable non-core resource quantities, such as GPUs or vendor devices, advertised on Node objects.

### 270. GPU and device-aware scheduling

GPU and device-aware scheduling places Pods according to requested accelerator or device resources exposed through device plugins or extended resources.

### 271. Volume-aware scheduling

Volume-aware scheduling considers storage topology, attachment constraints, and claim binding when selecting a node.

### 272. WaitForFirstConsumer

WaitForFirstConsumer is a volume-binding mode that delays dynamic provisioning or PV binding until the scheduler chooses a node.

### 273. Image locality scoring

Image locality scoring favors nodes that already have the required image layers locally, reducing pull latency.

### 274. Node unschedulable state

Node unschedulable state is the condition in which a node is marked unschedulable so new Pods are not placed there.

### 275. Scheduler plugins

Scheduler plugins are extension points that implement queue sorting, filtering, scoring, reserving, permitting, binding, or post-binding logic.

### 276. Scheduler framework

The scheduler framework is the pluggable execution model that organizes scheduling behavior into well-defined extension points.

### 277. Scheduler profiles

Scheduler profiles are named scheduler configurations that let different Pods use different plugin chains through schedulerName selection.

### 278. Custom scheduler

A custom scheduler is an alternative scheduler implementation that watches unscheduled Pods and posts bindings outside the default kube-scheduler logic.

### 279. Multiple schedulers

Multiple schedulers allow different scheduling policies to coexist in one cluster by targeting Pods with different scheduler names.

### 280. Queue sort plugins

Queue sort plugins control the ordering of Pods in the scheduling queue before a scheduling attempt begins.

### 281. Descheduler concepts

Descheduler concepts cover the post-scheduling process of evicting Pods to improve balance, policy compliance, or topology distribution after initial placement.

### 282. Descheduler policies

Descheduler policies are the rules that decide which already-running Pods are eligible for eviction during descheduler execution.

### 283. Unschedulable Pod backoff

Unschedulable Pod backoff is the increasing retry delay applied to Pods that repeatedly fail scheduling.

### 284. Scheduling events

Scheduling events are changes in cluster state, such as new nodes or deleted Pods, that make previously unschedulable Pods worth retrying.

## 8. Controllers and Reconciliation

### 285. kube-controller-manager role

The kube-controller-manager runs reconciliation loops that watch desired state and submit API changes until workloads, nodes, and cluster resources converge.

### 286. Shared informers

Shared informers maintain local watch-backed caches and deliver change notifications to multiple controllers without opening separate watches per controller.

### 287. Lister caches

Lister caches are informer-backed in-memory stores that controllers query instead of calling the API server on every reconcile pass.

### 288. Work queues

Work queues hold reconcile keys, support retries, and apply rate limiting so controllers process changes reliably and efficiently.

### 289. Reconciliation model

The reconciliation model is the controller pattern in which observed state is compared to desired state and corrective actions are retried until convergence.

### 290. Retry backoff

Retry backoff is the increasing delay applied to repeated reconcile failures so a broken dependency does not cause tight error loops.

### 291. Leader election for controllers

Leader election for controllers ensures that only one active controller instance performs a given control loop when multiple replicas exist.

### 292. Deployment controller

Deployment controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 293. ReplicaSet controller

ReplicaSet controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 294. StatefulSet controller

StatefulSet controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 295. DaemonSet controller

DaemonSet controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 296. Job controller

Job controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 297. CronJob controller

CronJob controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 298. Namespace controller

Namespace controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 299. Node controller

Node controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 300. ServiceAccount controller

ServiceAccount controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 301. Endpoint controller

Endpoint controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 302. EndpointSlice controller

EndpointSlice controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 303. PersistentVolume controller

PersistentVolume controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 304. AttachDetach controller

AttachDetach controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 305. Garbage collector

Garbage collector is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 306. TTL-after-finished controller

TTL-after-finished controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 307. ResourceQuota controller

ResourceQuota controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 308. HorizontalPodAutoscaler controller

HorizontalPodAutoscaler controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 309. CertificateSigningRequest controller

CertificateSigningRequest controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 310. Disruption controller

Disruption controller is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

### 311. Pod garbage collection

Pod garbage collection is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 312. Controller ownership model

Controller ownership model is a controller loop responsible for maintaining the expected state of its target resource set and issuing API updates when drift appears.

## 9. cloud-controller-manager and Cloud Integration

### 313. cloud-controller-manager role

The cloud-controller-manager runs cloud-specific control loops for nodes, routes, and external load balancers through the provider API.

### 314. Cloud-provider abstraction

Cloud-provider abstraction is the interface boundary that lets Kubernetes call provider-specific logic without hard-coding one cloud implementation into generic controllers.

### 315. Cloud node controller

The cloud node controller initializes Node objects with provider metadata such as addresses, zone, and provider ID and handles certain cloud node lifecycle events.

### 316. Provider ID

Provider ID is the stable cloud-specific instance identifier recorded on a Node so Kubernetes can map the Node object to its underlying VM or machine resource.

### 317. Node initialization

Node initialization is the cloud-assisted setup in which a new Node receives provider metadata and becomes schedulable only after required initialization completes.

### 318. Region labels

Region labels identify the larger topology domain in which a node runs and are consumed by placement and resilience logic.

### 319. Zone labels

Zone labels identify the finer-grained topology domain in which a node runs, typically one availability zone.

### 320. Cloud load balancer controller

The cloud load balancer controller provisions and updates external load-balancer resources for Services of type LoadBalancer.

### 321. Route controller

The route controller programs provider route tables so Pod CIDRs on nodes are reachable when the cloud networking model requires route propagation.

### 322. Cloud route programming

Cloud route programming is the process of creating or reconciling network routes in the cloud provider so node and Pod traffic can traverse correctly.

### 323. Cloud volume lifecycle hooks

Cloud volume lifecycle hooks are provider-specific operations used during attach, detach, or cleanup of block storage integrated with Kubernetes volumes.

### 324. External cloud provider model

The external cloud provider model runs cloud integration outside core Kubernetes binaries so provider logic can evolve independently of the main project.

### 325. Cloud API retry behavior

Cloud API retry behavior is the retry strategy cloud controllers use when provider API calls fail transiently or are throttled.

### 326. Cloud failure modes

Cloud failure modes are provider-side outages, throttling, stale metadata, or delayed reconciliation behaviors that affect cluster networking, node state, or load balancers.

### 327. Provisioning lag

Provisioning lag is the delay between a Kubernetes object request and the time the cloud resource actually becomes usable.

### 328. LoadBalancer Service provisioning

LoadBalancer Service provisioning is the reconciliation path that allocates an external address and maps it to Service backends.

### 329. Node address population

Node address population is the process of filling the Node object with internal and external addresses discovered from the provider.

### 330. Cloud network interface reconciliation

Cloud network interface reconciliation is the provider-side synchronization of node NIC state, addresses, or related networking metadata with the Kubernetes view.

## 10. Node Architecture and Node Lifecycle

### 331. Worker node architecture

Worker node architecture is the composition of kubelet, the container runtime, networking components, storage plugins, and the host operating system used to execute Pods.

### 332. Node registration

Node registration is the process by which kubelet creates or updates its Node object in the API and reports capacity, addresses, and labels.

### 333. Node object

A Node object is the cluster’s API representation of one worker or control-plane host, including metadata, capacity, allocatable resources, addresses, and health conditions.

### 334. Node conditions

Node conditions are health signals such as Ready, MemoryPressure, DiskPressure, and PIDPressure published on the Node object.

### 335. Node heartbeat

Node heartbeat is the periodic liveness signal sent by kubelet through status updates and leases so the control plane knows the node is still active.

### 336. Lease renewals

Lease renewals are the lightweight heartbeat updates written to the Lease object used for efficient node liveness tracking.

### 337. Node allocatable

Node allocatable is the quantity of node resource available to Pods after subtracting system-reserved, kube-reserved, and eviction-threshold reservations from total capacity.

### 338. Capacity vs allocatable

Capacity is the raw measured resource total on the host, whereas allocatable is the schedulable remainder exposed to workloads.

### 339. Node labels

Node labels are key-value metadata used for scheduling decisions, topology awareness, and operational grouping.

### 340. Node taints

Node taints are node-level scheduling repulsion markers that influence placement and eviction behavior.

### 341. Cordoned nodes

Cordoned nodes are marked unschedulable so no new Pods are placed there, though existing Pods continue running.

### 342. Drained nodes

Drained nodes have their evictable Pods removed so the node can undergo maintenance or decommissioning.

### 343. MemoryPressure

MemoryPressure is the node condition indicating the host is under memory stress and kubelet may need to evict Pods.

### 344. DiskPressure

DiskPressure is the node condition indicating the host filesystem or image filesystem is low on free space.

### 345. PIDPressure

PIDPressure is the node condition indicating the host is close to exhausting available process IDs.

### 346. NetworkUnavailable

NetworkUnavailable is the node condition indicating required node networking is not ready, often before CNI initialization completes.

### 347. Ready condition

The Ready condition indicates whether the node is healthy and able to accept and run Pods correctly.

### 348. NotReady state

NotReady state means the control plane considers the node unhealthy or unable to run Pods reliably.

### 349. Unreachable state

Unreachable state means the control plane has lost contact with the node and cannot confirm its current health.

### 350. Graceful node shutdown

Graceful node shutdown is the kubelet-managed termination flow that stops Pods in priority order when the host operating system is shutting down cleanly.

### 351. Node problem detection

Node problem detection is the process of surfacing host-level kernel, filesystem, hardware, or runtime issues into Kubernetes-visible conditions or events.

### 352. Host OS considerations

Host OS considerations cover kernel version, filesystem support, security modules, package lifecycle, and compatibility requirements for Kubernetes components.

### 353. Kernel dependencies

Kernel dependencies are the Linux capabilities, cgroup features, network primitives, and storage features Kubernetes and the runtime require from the host kernel.

### 354. Time synchronization

Time synchronization keeps node clocks aligned so certificates, logs, leases, and distributed-system timing remain valid.

### 355. Swap behavior

Swap behavior is the host memory-swapping policy that affects kubelet resource enforcement and Pod memory management.

### 356. cgroup driver alignment

cgroup driver alignment is the requirement that kubelet and the container runtime use compatible cgroup management modes, such as systemd or cgroupfs.

### 357. System-reserved resources

System-reserved resources are CPU, memory, and other reservations set aside for non-Kubernetes operating-system processes.

### 358. Kube-reserved resources

Kube-reserved resources are CPU, memory, and other reservations set aside for Kubernetes node components such as kubelet and the runtime.

### 359. Node replacement workflow

Node replacement workflow is the procedure for removing a failed or retired node and introducing a new host with equivalent cluster integration.

**Operational steps**
1. Drain the old node if it is still reachable and wait for workload eviction and storage detach to complete.
2. Delete the old Node object only after residual attachments, routes, and cloud references are gone.
3. Provision the replacement host with the approved operating-system image and Kubernetes prerequisites.
4. Install the container runtime, kubelet, and required node agents.
5. Join the replacement node to the cluster and restore required labels and taints.
6. Validate CNI, CSI, DaemonSet scheduling, and normal workload placement on the new node.

### 360. Node decommission workflow

Node decommission workflow is the controlled process for draining a node, removing it from cluster inventory, and retiring the underlying host.

**Operational steps**
1. Cordon the node to stop new Pod placement.
2. Drain the node while respecting PodDisruptionBudget and DaemonSet behavior.
3. Confirm stateful workloads have detached storage and moved successfully.
4. Remove the host from infrastructure inventory, monitoring, and configuration-management systems.
5. Delete the Node object and retire host credentials and access paths.
6. Validate that no Services, routes, or load-balancer backends still reference the retired node.

### 361. Windows node considerations

Windows node considerations are the scheduling, networking, storage, and runtime differences that apply when Kubernetes workloads run on Windows hosts.

## 11. kubelet Internals

### 362. kubelet role

The kubelet is the node agent that translates assigned PodSpecs into sandbox, container, volume, and status operations on one host.

### 363. Pod sync loop

The Pod sync loop is kubelet’s repeated reconciliation cycle that ensures each assigned Pod’s running state matches the desired PodSpec.

### 364. Pod workers

Pod workers are per-Pod goroutines that serialize lifecycle operations for each Pod so concurrent updates do not corrupt state.

### 365. Static Pods

Static Pods are Pods defined by files on disk that kubelet runs directly without needing the API server as the source of truth.

### 366. Mirror Pods

Mirror Pods are read-only API representations of Static Pods created so the control plane can observe locally managed workloads.

### 367. Pod admission on node

Pod admission on node is the kubelet validation step that rejects Pods the node cannot run because of resource, security, or configuration constraints.

### 368. Pod sandbox creation

Pod sandbox creation is the runtime operation that creates the shared Pod network namespace and foundational sandbox context before containers start.

### 369. Sandbox recreation

Sandbox recreation is the rebuild of the Pod sandbox when the original sandbox dies or its network namespace becomes invalid.

### 370. Container start sequence

Container start sequence is the ordered runtime path of image pull, container create, pre-start setup, and process start within the Pod sandbox.

### 371. Image pull workflow

Image pull workflow is the kubelet-to-runtime process that resolves credentials, checks image presence, downloads manifests and layers, and unpacks the image when needed.

### 372. Image pull policy

Image pull policy controls whether kubelet always pulls an image, pulls only when not present locally, or never pulls from a registry.

### 373. Image credential resolution

Image credential resolution is the process of locating registry credentials from imagePullSecrets, node credential providers, or runtime configuration.

### 374. ImagePullSecrets

ImagePullSecrets are Secret references attached to a Pod or ServiceAccount and used to authenticate image pulls from private registries.

### 375. Readiness probes

Readiness probes determine whether a container should receive Service traffic by reporting when the application is ready to serve requests.

### 376. Liveness probes

Liveness probes determine whether kubelet should restart a running container because the process is stuck or unhealthy.

### 377. Startup probes

Startup probes protect slow-starting applications by delaying liveness and readiness failure handling until initialization completes.

### 378. HTTP probes

HTTP probes send an HTTP request to the container endpoint and evaluate the response as the probe result.

### 379. TCP probes

TCP probes check whether a TCP socket can be opened on the target container port.

### 380. Exec probes

Exec probes run a command inside the container and use its exit code as the probe result.

### 381. gRPC probe support concepts

gRPC probe support concepts cover health checking through the gRPC health protocol instead of HTTP or raw TCP checks.

### 382. Lifecycle hooks

Lifecycle hooks are container lifecycle callbacks such as postStart and preStop executed by kubelet around container start and shutdown.

### 383. postStart hook

The postStart hook is a container callback executed immediately after container creation, before the container is considered fully started.

### 384. preStop hook

The preStop hook is a container callback executed before kubelet sends the final termination signal during shutdown.

### 385. Volume mounting

Volume mounting is the node-side process of attaching, staging, mounting, and wiring volumes into the Pod filesystem namespace.

### 386. Secret projection

Secret projection places Secret data into a volume tree mounted into the Pod so applications can read credential files at runtime.

### 387. ConfigMap projection

ConfigMap projection places ConfigMap keys into mounted files inside a projected or direct volume.

### 388. Projected volumes

Projected volumes combine multiple volume sources, such as Secrets, ConfigMaps, Downward API, and service-account tokens, into one mounted directory tree.

### 389. Downward API projection

Downward API projection injects selected Pod metadata or resource fields into files or environment variables visible to the container.

### 390. Pod status updates

Pod status updates are kubelet writes that report Pod phase, conditions, IP information, and container summaries back to the API server.

### 391. Container status updates

Container status updates report per-container state such as waiting reason, running state, restart count, and last termination information.

### 392. Restart policy enforcement

Restart policy enforcement is kubelet’s logic that decides whether terminated containers in a Pod should be restarted based on the Pod restart policy and container exit state.

### 393. Graceful termination handling

Graceful termination handling is kubelet’s shutdown flow of running preStop hooks, sending signals, waiting for the grace period, and then force-killing remaining processes.

### 394. Eviction manager

The eviction manager monitors node resource pressure and terminates Pods according to thresholds and priority so node health is preserved.

### 395. Soft eviction thresholds

Soft eviction thresholds define pressure levels that must remain breached for a configured duration before kubelet starts evicting Pods.

### 396. Hard eviction thresholds

Hard eviction thresholds define pressure levels at which kubelet can evict Pods immediately to protect node stability.

### 397. Image garbage collection

Image garbage collection removes unused image data when disk usage crosses configured thresholds.

### 398. Container garbage collection

Container garbage collection deletes dead containers and their runtime metadata when retention policies are exceeded.

### 399. Container log handling

Container log handling is the kubelet-managed collection, rotation, and retention of container stdout and stderr log files on the node.

### 400. Node status reporting

Node status reporting is kubelet’s publication of node conditions, addresses, capacity, allocatable resources, and daemon endpoints.

### 401. Device plugin registration

Device plugin registration is the process by which a device plugin advertises allocatable hardware resources to kubelet.

### 402. Pod resources API

The Pod resources API exposes the actual CPU, device, and memory assignments that kubelet made for running Pods to local consumers.

### 403. RuntimeClass selection

RuntimeClass selection chooses the runtime handler and, optionally, overhead or scheduling constraints applied to a Pod.

### 404. Ephemeral container support

Ephemeral container support is the kubelet and API capability that injects debug-only containers into an existing Pod without restarting the workload.

### 405. CPU Manager

CPU Manager is the kubelet subsystem that can pin eligible containers to dedicated CPU sets for stronger CPU isolation and cache locality.

### 406. Topology Manager

Topology Manager coordinates CPU, device, and memory placement hints so latency-sensitive workloads get better NUMA alignment.

### 407. Memory Manager

Memory Manager is the kubelet subsystem that allocates memory according to topology-aware policies for eligible workloads.

### 408. Node allocatable enforcement

Node allocatable enforcement ensures kubelet does not let Pods consume resources reserved for the system or Kubernetes daemons.

### 409. Credential provider plugins

Credential provider plugins are node-side executables that dynamically fetch registry credentials for kubelet image pulls.

### 410. Checkpoint API concepts

Checkpoint API concepts cover the persistence and retrieval of selected runtime or device-allocation state on the node for restart continuity and introspection.

## 12. Container Runtime and Execution

### 411. Container runtime

Container runtime is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 412. CRI

CRI is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 413. CRI runtime service

CRI runtime service is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 414. CRI image service

CRI image service is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 415. PodSandbox model

PodSandbox model is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 416. Pause container

Pause container is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 417. OCI image spec

OCI image spec is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 418. OCI runtime spec

OCI runtime spec is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 419. containerd

containerd is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 420. CRI-O

CRI-O is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 421. runc

runc is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 422. Shim processes

Shim processes is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 423. gVisor

gVisor is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 424. Kata Containers

Kata Containers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 425. Snapshotters

Snapshotters is a Kubernetes storage snapshot or backup concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 426. OverlayFS snapshotter

OverlayFS snapshotter is a Kubernetes storage snapshot or backup concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 427. stargz snapshotter concepts

stargz snapshotter concepts is a Kubernetes storage snapshot or backup concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 428. Image layer unpacking

Image layer unpacking is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 429. Container filesystem assembly

Container filesystem assembly is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 430. Container process lifecycle

Container process lifecycle is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 431. Runtime sandbox failure scenarios

Runtime sandbox failure scenarios is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 432. Runtime isolation models

Runtime isolation models is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 433. Rootless containers considerations

Rootless containers considerations is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 434. User namespaces in containers

User namespaces in containers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 435. Seccomp interaction with runtime

Seccomp interaction with runtime is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 436. AppArmor interaction with runtime

AppArmor interaction with runtime is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 437. SELinux interaction with runtime

SELinux interaction with runtime is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

## 13. Pods and Container Model

### 438. Pod

A Pod is the smallest schedulable Kubernetes object. The scheduler binds one Pod to one node, kubelet creates one sandbox network namespace for that Pod, and every container in the Pod shares the same Pod IP address, loopback interface, and mounted volumes.

**YAML example**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: default
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.27.2
      ports:
        - containerPort: 80
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 10
      resources:
        requests:
          cpu: "100m"
          memory: "128Mi"
        limits:
          cpu: "500m"
          memory: "256Mi"
```

### 439. Single-container Pod

Single-container Pod is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 440. Multi-container Pod

Multi-container Pod is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 441. Init containers

Init containers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 442. Sidecar containers

Sidecar containers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 443. Ephemeral containers

Ephemeral containers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 444. Pod phases

Pod phases is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 445. Pod conditions

Pod conditions is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 446. Pod readiness gates

Pod readiness gates is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 447. Pod scheduling gates

Pod scheduling gates is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 448. Pod IP

Pod IP is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 449. Pod DNS

Pod DNS is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 450. Pod hostname

Pod hostname is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 451. Pod subdomain

Pod subdomain is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 452. Shared network namespace

Shared network namespace is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 453. Shared IPC namespace

Shared IPC namespace is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 454. Shared PID namespace

Shared PID namespace is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 455. Shared volumes in Pod

Shared volumes in Pod is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 456. Pod restart policy

Pod restart policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 457. Pod security context

Pod security context is a Kubernetes runtime hardening or authorization control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 458. Container security context

Container security context is a Kubernetes runtime hardening or authorization control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 459. CPU requests

CPU requests is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 460. Memory requests

Memory requests is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 461. Ephemeral storage requests

Ephemeral storage requests is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 462. CPU limits

CPU limits is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 463. Memory limits

Memory limits is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 464. Ephemeral storage limits

Ephemeral storage limits is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 465. QoS classes

QoS classes is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 466. Guaranteed QoS

Guaranteed QoS is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 467. Burstable QoS

Burstable QoS is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 468. BestEffort QoS

BestEffort QoS is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 469. Pod disruption behavior

Pod disruption behavior is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 470. TerminationGracePeriodSeconds

TerminationGracePeriodSeconds is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 471. Pod immutability rules

Pod immutability rules is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 472. Pod debugging workflow

Pod debugging workflow is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 473. Pod overhead

Pod overhead is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 474. Host aliases

Host aliases is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 475. DNS policy

DNS policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 476. dnsConfig

dnsConfig is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

## 14. Workload Controllers

### 477. ReplicationController

ReplicationController is a Kubernetes controller loop that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 478. ReplicaSet

ReplicaSet is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 479. Deployment

A Deployment is a declarative workload controller for stateless Pods. The Deployment controller creates and scales ReplicaSets, compares rollout progress with the requested Pod template, and preserves revision history so a failed rollout can be rolled back.

**YAML example**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
  namespace: default
spec:
  replicas: 3
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: web
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.27.2
          ports:
            - containerPort: 80
```

### 480. Deployment rollout

Deployment rollout is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 481. RollingUpdate strategy

RollingUpdate strategy is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 482. Recreate strategy

Recreate strategy is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 483. maxUnavailable

maxUnavailable is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 484. maxSurge

maxSurge is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 485. Rollback

Rollback is a Kubernetes rollback or revision-recovery concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 486. Revision history

Revision history is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 487. Paused rollout

Paused rollout is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 488. Progress deadline

Progress deadline is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 489. StatefulSet

A StatefulSet is a workload controller for ordered, identity-aware Pods. Each Pod receives a stable ordinal, stable DNS identity, and a persistent volume claim association that survives Pod recreation.

**YAML example**
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  namespace: default
spec:
  serviceName: mysql
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8.4
          volumeMounts:
            - name: data
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 20Gi
```

### 490. OrderedReady policy

OrderedReady policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 491. Parallel Pod management policy

Parallel Pod management policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 492. Stable network identity

Stable network identity is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 493. Stable storage identity

Stable storage identity is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 494. PersistentVolumeClaim templates

PersistentVolumeClaim templates is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 495. Headless Service for StatefulSet

Headless Service for StatefulSet is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 496. StatefulSet update strategy

StatefulSet update strategy is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 497. Partitioned rollout in StatefulSet

Partitioned rollout in StatefulSet is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 498. DaemonSet

DaemonSet is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 499. Node-targeted agents

Node-targeted agents is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 500. DaemonSet RollingUpdate

DaemonSet RollingUpdate is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 501. OnDelete update strategy

OnDelete update strategy is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 502. Job

Job is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 503. Parallelism

Parallelism is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 504. Completions

Completions is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 505. BackoffLimit

BackoffLimit is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 506. ActiveDeadlineSeconds

ActiveDeadlineSeconds is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 507. PodFailurePolicy

PodFailurePolicy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 508. SuccessPolicy concepts

SuccessPolicy concepts is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 509. Indexed Jobs

Indexed Jobs is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 510. CronJob

CronJob is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 511. Cron schedule syntax

Cron schedule syntax is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 512. ConcurrencyPolicy

ConcurrencyPolicy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 513. StartingDeadlineSeconds

StartingDeadlineSeconds is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 514. Suspended CronJobs

Suspended CronJobs is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 15. Configuration, Secrets, and Service Accounts

### 515. ConfigMap

A ConfigMap stores non-secret configuration as key-value string data or file-like content. Pods can consume ConfigMap data through environment variables, command arguments, or mounted files without rebuilding the image.

**YAML example**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: default
data:
  APP_MODE: production
  LOG_LEVEL: info
```

### 516. Secret

A Secret stores sensitive bytes such as passwords, bearer tokens, TLS keys, or registry credentials. Secret values are base64-encoded in the manifest representation and can be encrypted at rest in the API server storage layer.

**YAML example**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
  namespace: default
type: Opaque
data:
  username: YWRtaW4=
  password: UzNjcjN0IQ==
```

### 517. Opaque Secret

Opaque Secret is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 518. TLS Secret

TLS Secret is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 519. dockerconfigjson Secret

dockerconfigjson Secret is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 520. Environment variable injection

Environment variable injection is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 521. envFrom

envFrom is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 522. ConfigMap volumes

ConfigMap volumes is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 523. Secret volumes

Secret volumes is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 524. Projected volumes

Projected volumes is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 525. Immutable ConfigMaps

Immutable ConfigMaps is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 526. Immutable Secrets

Immutable Secrets is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 527. ServiceAccount

ServiceAccount is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 528. Default ServiceAccount

Default ServiceAccount is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 529. ServiceAccount token projection

ServiceAccount token projection is a Kubernetes identity or authentication artifact that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 530. Projected service account tokens

Projected service account tokens is a Kubernetes identity or authentication artifact that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 531. BoundServiceAccountTokenVolume

BoundServiceAccountTokenVolume is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 532. External secret operators

External secret operators is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 533. CSI secret store patterns

CSI secret store patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 534. Configuration drift

Configuration drift is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 535. Hot-reload patterns

Hot-reload patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 536. Checksum annotation restart pattern

Checksum annotation restart pattern is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 16. Services and Discovery

### 537. Service

A Service is a stable network abstraction that maps a fixed DNS name and usually a virtual IP address to a changing backend set. EndpointSlice controllers keep the Service backend list aligned with selected Pods, while kube-proxy or an eBPF dataplane forwards packets to those backends.

**YAML example**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-svc
  namespace: default
spec:
  selector:
    app: web
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```

### 538. ClusterIP

ClusterIP is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 539. Headless Service

Headless Service is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 540. NodePort

NodePort is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 541. LoadBalancer Service

LoadBalancer Service is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 542. ExternalName Service

ExternalName Service is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 543. Service selectors

Service selectors is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 544. Services without selectors

Services without selectors is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 545. Cluster DNS

Cluster DNS is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 546. CoreDNS

CoreDNS is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 547. CoreDNS plugin chain

CoreDNS plugin chain is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 548. DNS search paths

DNS search paths is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 549. Service VIP

Service VIP is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 550. Virtual IP routing

Virtual IP routing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 551. Session affinity

Session affinity is a Kubernetes placement constraint that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 552. ExternalTrafficPolicy

ExternalTrafficPolicy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 553. InternalTrafficPolicy

InternalTrafficPolicy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 554. HealthCheckNodePort

HealthCheckNodePort is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 555. Node-local traffic handling

Node-local traffic handling is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 556. Topology-aware routing hints

Topology-aware routing hints is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 557. Endpoint object

Endpoint object is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 558. EndpointSlice object

EndpointSlice object is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 559. Service discovery conventions

Service discovery conventions is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 560. SRV records

SRV records is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 561. DNS failure scenarios

DNS failure scenarios is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

## 17. Networking Model and Dataplane

### 562. Kubernetes networking model

Kubernetes networking model is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 563. Pod-to-Pod communication

Pod-to-Pod communication is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 564. Pod-to-Service communication

Pod-to-Service communication is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 565. Service-to-Pod routing

Service-to-Pod routing is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 566. Node-to-Pod communication

Node-to-Pod communication is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 567. Pod CIDR

Pod CIDR is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 568. Service CIDR

Service CIDR is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 569. Cluster DNS domain

Cluster DNS domain is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 570. CNI

CNI is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 571. IPAM

IPAM is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 572. veth pair

veth pair is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 573. Bridge networking

Bridge networking is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 574. Overlay networking

Overlay networking is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 575. VXLAN

VXLAN is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 576. Geneve

Geneve is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 577. Underlay networking

Underlay networking is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 578. BGP-based routing

BGP-based routing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 579. Route programming

Route programming is a Kubernetes traffic-routing or forwarding concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 580. Encapsulation

Encapsulation is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 581. MTU tuning

MTU tuning is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 582. Hairpin traffic

Hairpin traffic is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 583. HostNetwork Pods

HostNetwork Pods is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 584. hostPort

hostPort is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 585. Dual-stack networking

Dual-stack networking is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 586. IPv4 clusters

IPv4 clusters is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 587. IPv6 clusters

IPv6 clusters is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 588. Dual-stack Services

Dual-stack Services is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 589. NodeLocal DNSCache

NodeLocal DNSCache is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 590. eBPF dataplane

eBPF dataplane is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 591. kube-proxy replacement

kube-proxy replacement is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 18. kube-proxy, Conntrack, and Service Routing

### 592. kube-proxy role

kube-proxy role is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 593. iptables mode

iptables mode is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 594. IPVS mode

IPVS mode is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 595. Userspace proxy mode legacy

Userspace proxy mode legacy is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 596. Service rule programming

Service rule programming is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 597. ClusterIP datapath

ClusterIP datapath is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 598. NodePort datapath

NodePort datapath is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 599. External IP handling

External IP handling is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 600. Session affinity implementation

Session affinity implementation is a Kubernetes placement constraint that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 601. EndpointSlice consumption

EndpointSlice consumption is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 602. Service update propagation

Service update propagation is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 603. Topology-aware hints

Topology-aware hints is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 604. Conntrack interaction

Conntrack interaction is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 605. Conntrack exhaustion scenarios

Conntrack exhaustion scenarios is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 606. kube-proxy-free cluster operations

kube-proxy-free cluster operations is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 19. Ingress, Gateway API, and Traffic Management

### 607. Ingress resource

Ingress resource is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 608. Ingress controller

Ingress controller is a Kubernetes controller loop that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 609. IngressClass

IngressClass is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 610. Default IngressClass

Default IngressClass is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 611. Host-based routing

Host-based routing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 612. Path-based routing

Path-based routing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 613. TLS termination

TLS termination is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 614. Wildcard hosts

Wildcard hosts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 615. Rewrite rules

Rewrite rules is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 616. Canary ingress patterns

Canary ingress patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 617. Gateway API

Gateway API is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 618. GatewayClass

GatewayClass is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 619. Gateway

Gateway is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 620. Listener

Listener is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 621. HTTPRoute

HTTPRoute is a Kubernetes traffic-routing or forwarding concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 622. TCPRoute

TCPRoute is a Kubernetes traffic-routing or forwarding concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 623. TLSRoute

TLSRoute is a Kubernetes traffic-routing or forwarding concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 624. GRPCRoute

GRPCRoute is a Kubernetes traffic-routing or forwarding concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 625. UDPRoute concepts

UDPRoute concepts is a Kubernetes traffic-routing or forwarding concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 626. ReferenceGrant

ReferenceGrant is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 627. Traffic splitting

Traffic splitting is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 628. Weighted routing

Weighted routing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 629. Blue-green traffic management

Blue-green traffic management is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 630. Retry policy

Retry policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 631. Timeout policy

Timeout policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 632. Mirroring traffic

Mirroring traffic is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 633. North-south gateway patterns

North-south gateway patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 634. East-west gateway patterns

East-west gateway patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 20. Network Security and Policy

### 635. NetworkPolicy

A NetworkPolicy defines allowed ingress or egress traffic for selected Pods at layer 3 and layer 4. Enforcement depends on the active CNI plugin, so a manifest alone does not isolate traffic unless the dataplane implements policy evaluation.

**YAML example**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web-ingress
  namespace: default
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
              app: ingress
      ports:
        - protocol: TCP
          port: 80
```

### 636. Ingress NetworkPolicy

Ingress NetworkPolicy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 637. Egress NetworkPolicy

Egress NetworkPolicy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 638. Default deny ingress

Default deny ingress is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 639. Default deny egress

Default deny egress is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 640. Namespace isolation

Namespace isolation is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 641. Pod selector semantics

Pod selector semantics is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 642. Namespace selector semantics

Namespace selector semantics is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 643. IPBlock rules

IPBlock rules is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 644. Policy enforcement by CNI

Policy enforcement by CNI is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 645. L3/L4 policy model

L3/L4 policy model is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 646. DNS egress policy patterns

DNS egress policy patterns is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 647. NetworkPolicy limitations by CNI

NetworkPolicy limitations by CNI is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 648. Multi-tenant network segmentation

Multi-tenant network segmentation is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 649. Egress gateway patterns

Egress gateway patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 650. mTLS in service mesh

mTLS in service mesh is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

## 21. Storage, CSI, and Data Protection

### 651. Kubernetes volume model

Kubernetes volume model is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 652. emptyDir

emptyDir is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 653. hostPath

hostPath is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 654. configMap volume

configMap volume is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 655. secret volume

secret volume is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 656. projected volume

projected volume is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 657. PersistentVolume

PersistentVolume is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 658. PersistentVolumeClaim

PersistentVolumeClaim is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 659. StorageClass

StorageClass is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 660. Dynamic provisioning

Dynamic provisioning is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 661. Static provisioning

Static provisioning is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 662. Reclaim policy

Reclaim policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 663. Retain reclaim policy

Retain reclaim policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 664. Delete reclaim policy

Delete reclaim policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 665. Recycle policy legacy

Recycle policy legacy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 666. Access modes

Access modes is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 667. ReadWriteOnce

ReadWriteOnce is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 668. ReadOnlyMany

ReadOnlyMany is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 669. ReadWriteMany

ReadWriteMany is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 670. ReadWriteOncePod

ReadWriteOncePod is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 671. Filesystem volume mode

Filesystem volume mode is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 672. Block volume mode

Block volume mode is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 673. Immediate binding

Immediate binding is a Kubernetes binding or attachment operation that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 674. WaitForFirstConsumer binding

WaitForFirstConsumer binding is a Kubernetes binding or attachment operation that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 675. CSI

CSI is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 676. CSI controller plugin

CSI controller plugin is a Kubernetes controller loop that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 677. CSI node plugin

CSI node plugin is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 678. CSI external provisioner

CSI external provisioner is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 679. CSI attacher

CSI attacher is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 680. CSI resizer

CSI resizer is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 681. CSI snapshotter

CSI snapshotter is a Kubernetes storage snapshot or backup concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 682. CSI sidecar failure handling

CSI sidecar failure handling is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 683. Volume attachment

Volume attachment is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 684. Mount propagation

Mount propagation is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 685. Volume snapshots

Volume snapshots is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 686. VolumeSnapshotClass

VolumeSnapshotClass is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 687. Volume cloning

Volume cloning is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 688. Online volume expansion

Online volume expansion is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 689. Offline volume expansion

Offline volume expansion is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 690. Raw block volumes

Raw block volumes is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 691. Topology-aware storage

Topology-aware storage is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 692. Stateful storage patterns

Stateful storage patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 693. Attach failure scenarios

Attach failure scenarios is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 694. Mount failure scenarios

Mount failure scenarios is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 695. Backup consistency considerations

Backup consistency considerations is a Kubernetes backup or state-capture procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

## 22. Security, Identity, and Policy Enforcement

### 696. RBAC

RBAC is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 697. Role

Role is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 698. ClusterRole

ClusterRole is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 699. RoleBinding

RoleBinding is a Kubernetes binding or attachment operation that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 700. ClusterRoleBinding

ClusterRoleBinding is a Kubernetes binding or attachment operation that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 701. Aggregation rules for ClusterRoles

Aggregation rules for ClusterRoles is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 702. Workload identity

Workload identity is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 703. Node identity

Node identity is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 704. Bootstrap node identity

Bootstrap node identity is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 705. Client certificate authentication

Client certificate authentication verifies the caller identity from an X.509 certificate signed by a trusted CA.

### 706. Certificate signer concepts

Certificate signer concepts is a Kubernetes certificate or trust-management concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 707. OIDC identity integration

OIDC identity integration is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 708. Webhook authentication and authorization

Webhook authentication and authorization is a Kubernetes external callback integration that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 709. Pod Security Standards

Pod Security Standards is a Kubernetes runtime hardening or authorization control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 710. Pod Security Admission

Pod Security Admission is a Kubernetes admission-stage enforcement mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 711. Baseline profile

Baseline profile is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 712. Restricted profile

Restricted profile is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 713. Privileged profile

Privileged profile is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 714. securityContext

securityContext is a Kubernetes runtime hardening or authorization control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 715. runAsNonRoot

runAsNonRoot is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 716. readOnlyRootFilesystem

readOnlyRootFilesystem is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 717. allowPrivilegeEscalation

allowPrivilegeEscalation is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 718. privileged containers

privileged containers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 719. hostPID

hostPID is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 720. hostIPC

hostIPC is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 721. hostNetwork security implications

hostNetwork security implications is a Kubernetes runtime hardening or authorization control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 722. Linux capabilities

Linux capabilities is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 723. Seccomp

Seccomp is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 724. AppArmor

AppArmor is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 725. SELinux

SELinux is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 726. fsGroup

fsGroup is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 727. Supplemental groups

Supplemental groups is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 728. sysctls

sysctls is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 729. Admission policy

Admission policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 730. OPA Gatekeeper

OPA Gatekeeper is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 731. Kyverno

Kyverno is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 732. Secret encryption

Secret encryption is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 733. KMS providers

KMS providers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 734. Image signing concepts

Image signing concepts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 735. Image provenance

Image provenance is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 736. Software bill of materials

Software bill of materials is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 737. Supply chain security

Supply chain security is a Kubernetes runtime hardening or authorization control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 738. Control-plane certificate expiry recovery

Control-plane certificate expiry recovery is a Kubernetes certificate or trust-management concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

## 23. Resource Governance, Isolation, and Capacity

### 739. ResourceQuota

ResourceQuota is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 740. LimitRange

LimitRange is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 741. Default requests

Default requests is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 742. Default limits

Default limits is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 743. Namespace capacity control

Namespace capacity control is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 744. CPU overcommit

CPU overcommit is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 745. Memory overcommit

Memory overcommit is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 746. Ephemeral storage quotas

Ephemeral storage quotas is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 747. HugePages

HugePages is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 748. NUMA-aware placement

NUMA-aware placement is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 749. Dedicated node pools

Dedicated node pools is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 750. GPU scheduling

GPU scheduling is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 751. Device plugins

Device plugins is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 752. RuntimeClass isolation

RuntimeClass isolation is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 753. Taints for isolation

Taints for isolation is a Kubernetes placement and eviction control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 754. Tolerations for dedicated workloads

Tolerations for dedicated workloads is a Kubernetes placement and eviction allowance that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 755. Pod overhead

Pod overhead is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 756. Node allocatable reservations

Node allocatable reservations is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 757. System-reserved and kube-reserved tuning

System-reserved and kube-reserved tuning is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 758. Cluster capacity planning

Cluster capacity planning is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 759. Bin-packing strategy

Bin-packing strategy is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 24. Autoscaling and Elasticity

### 760. HorizontalPodAutoscaler

HorizontalPodAutoscaler is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 761. autoscaling/v2

autoscaling/v2 is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 762. CPU-based scaling

CPU-based scaling is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 763. Memory-based scaling

Memory-based scaling is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 764. Custom metrics scaling

Custom metrics scaling is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 765. External metrics scaling

External metrics scaling is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 766. Metrics API

Metrics API is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 767. Custom Metrics API

Custom Metrics API is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 768. External Metrics API

External Metrics API is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 769. metrics-server

metrics-server is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 770. Metrics adapter patterns

Metrics adapter patterns is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 771. Prometheus adapter patterns

Prometheus adapter patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 772. Scale target ref

Scale target ref is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 773. Min replicas

Min replicas is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 774. Max replicas

Max replicas is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 775. Scale stabilization

Scale stabilization is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 776. HPA behavior policies

HPA behavior policies is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 777. VerticalPodAutoscaler

VerticalPodAutoscaler is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 778. VPA recommendation mode

VPA recommendation mode is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 779. VPA auto mode

VPA auto mode is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 780. VPA and HPA interaction

VPA and HPA interaction is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 781. Cluster Autoscaler

Cluster Autoscaler is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 782. Node group scaling

Node group scaling is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 783. Expander strategies

Expander strategies is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 784. Scale-down behavior

Scale-down behavior is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 785. Scale-from-zero

Scale-from-zero is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 786. KEDA

KEDA is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 787. Event-driven autoscaling

Event-driven autoscaling is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 25. Reliability, Availability, and Resilience

### 788. Self-healing

Self-healing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 789. Replica-based availability

Replica-based availability is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 790. Readiness gating

Readiness gating is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 791. Liveness-based recovery

Liveness-based recovery is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 792. Startup protection

Startup protection is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 793. Graceful shutdown

Graceful shutdown is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 794. PodDisruptionBudget

PodDisruptionBudget is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 795. PDB edge cases during upgrades

PDB edge cases during upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 796. Eviction API

Eviction API is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 797. Voluntary disruptions

Voluntary disruptions is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 798. Involuntary disruptions

Involuntary disruptions is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 799. Anti-affinity for high availability

Anti-affinity for high availability is a Kubernetes placement constraint that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 800. Topology spread for high availability

Topology spread for high availability is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 801. Multi-zone resilience

Multi-zone resilience is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 802. Control plane high availability

Control plane high availability is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 803. etcd quorum resilience

etcd quorum resilience is a Kubernetes consensus requirement that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 804. Failure domain awareness

Failure domain awareness is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 805. SLOs

SLOs is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 806. SLIs

SLIs is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 807. Error budgets

Error budgets is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 808. Canary analysis

Canary analysis is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 809. Chaos engineering

Chaos engineering is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 810. Fault injection testing

Fault injection testing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 26. Observability, Telemetry, and Diagnostics

### 811. Cluster logging

Cluster logging is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 812. Container stdout/stderr logging

Container stdout/stderr logging is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 813. Node log collection

Node log collection is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 814. Centralized logging stack

Centralized logging stack is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 815. Metrics

Metrics is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 816. Application metrics

Application metrics is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 817. Node metrics

Node metrics is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 818. kube-state-metrics

kube-state-metrics is a Kubernetes metrics collection or autoscaling signal concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 819. Prometheus

Prometheus is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 820. Alerting

Alerting is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 821. Tracing

Tracing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 822. Distributed tracing

Distributed tracing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 823. OpenTelemetry

OpenTelemetry is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 824. Events

Events is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 825. Audit logs

Audit logs is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 826. Correlation across telemetry signals

Correlation across telemetry signals is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 827. Debug containers

Debug containers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 828. kubectl debug

kubectl debug is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 829. CrashLoopBackOff analysis

CrashLoopBackOff analysis is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 830. ImagePullBackOff analysis

ImagePullBackOff analysis is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 831. Pending Pod diagnosis

Pending Pod diagnosis is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 832. Service connectivity debugging

Service connectivity debugging is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 833. DNS troubleshooting

DNS troubleshooting is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 834. NetworkPolicy troubleshooting

NetworkPolicy troubleshooting is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 835. Volume mount troubleshooting

Volume mount troubleshooting is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 836. Scheduler diagnostics

Scheduler diagnostics is a Kubernetes scheduling mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 837. kubelet diagnostics

kubelet diagnostics is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 838. etcd diagnostics

etcd diagnostics is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 27. Upgrades, Version Skew, and Lifecycle Management

### 839. Kubernetes version skew policy

Kubernetes version skew policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 840. Version skew between control plane and kubelet

Version skew between control plane and kubelet is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 841. Version skew between kubectl and cluster

Version skew between kubectl and cluster is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 842. Control plane upgrade strategy

Control plane upgrade strategy is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Read target release notes, removed APIs, and supported version-skew rules.
2. Take and validate an etcd snapshot before changing any control-plane component.
3. Upgrade one control-plane node at a time in an HA cluster unless the platform orchestrates upgrades differently.
4. Upgrade kube-apiserver before upgrading worker kubelets.
5. Validate `/readyz`, leader election, admission webhooks, and aggregated APIs after each control-plane node update.
6. Upgrade kubelet packages on the control-plane node after static Pods return to Ready.
7. Repeat on remaining control-plane nodes.
8. Start worker-node upgrades only after control-plane stability checks pass.

### 843. kubeadm upgrade workflow

kubeadm upgrade workflow is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Check the current node versions with `kubectl get nodes -o wide`.
2. Review supported upgrade targets with `kubeadm upgrade plan`.
3. Back up etcd and copy `/etc/kubernetes` to protected storage.
4. Upgrade the `kubeadm` package on one control-plane node.
5. Run `kubeadm upgrade apply <target-version>` on the first control-plane node.
6. Upgrade `kubelet` and `kubectl` packages on that node and restart kubelet.
7. Run `kubeadm upgrade node` on the remaining control-plane nodes after their package update.
8. Drain each worker node, upgrade `kubeadm` and `kubelet`, run `kubeadm upgrade node`, restart kubelet, and uncordon the node.
9. Validate CoreDNS, kube-proxy, CSI drivers, admission webhooks, and critical workloads after the upgrade.

### 844. Node upgrade strategy

Node upgrade strategy is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Confirm the target kubelet version is supported by the current control-plane version.
2. Review PodDisruptionBudgets, DaemonSets, and local-storage dependencies on the target node.
3. Cordon the node with `kubectl cordon <node-name>`.
4. Drain the node with `kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data`.
5. Upgrade the operating-system packages, kubelet, container runtime, and approved node agents.
6. Restart kubelet and the runtime and verify the node returns to `Ready`.
7. Uncordon the node with `kubectl uncordon <node-name>`.
8. Validate workload redistribution, CNI connectivity, CSI mounts, and node metrics before continuing.

### 845. Rolling node upgrades

Rolling node upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 846. Surge node upgrades

Surge node upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 847. In-place node upgrades

In-place node upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 848. Drain before upgrade

Drain before upgrade is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Review PodDisruptionBudgets and singleton workloads on the target node.
2. Cordon the node so the scheduler stops placing new Pods there.
3. Run `kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data`.
4. Wait for evictable Pods to leave the node and confirm detach operations finish.
5. Begin the upgrade only after the node is fully drained.

### 849. Cordoning nodes

Cordoning nodes is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 850. Uncordoning nodes

Uncordoning nodes is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 851. PDB-aware upgrades

PDB-aware upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 852. Managed node group upgrades

Managed node group upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 853. Self-managed node upgrade workflow

Self-managed node upgrade workflow is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 854. OS patching

OS patching is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 855. Kernel upgrade considerations

Kernel upgrade considerations is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 856. Container runtime upgrades

Container runtime upgrades is a Kubernetes runtime execution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 857. CNI upgrades

CNI upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Review plugin release notes, supported Kubernetes versions, and kernel requirements.
2. Back up the current plugin manifests or Helm values.
3. Upgrade control-plane-facing plugin components first if the plugin requires ordered sequencing.
4. Roll node agents gradually across the cluster.
5. Validate Pod-to-Pod, Pod-to-Service, DNS, and NetworkPolicy behavior after each batch.
6. Complete the rollout only after dataplane and policy checks pass.

### 858. CSI upgrades

CSI upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Confirm compatibility between the target CSI version, the Kubernetes version, and the storage backend.
2. Back up the existing CSI manifests or release values.
3. Upgrade CSI controller components and sidecars first if required by the driver.
4. Roll node plugins in controlled batches.
5. Validate provision, attach, mount, expand, and snapshot operations before finishing the rollout.

### 859. Addon compatibility checks

Addon compatibility checks is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 860. CoreDNS upgrades

CoreDNS upgrades is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Back up the current Corefile ConfigMap and Deployment or Helm values.
2. Roll out the target CoreDNS image or chart revision.
3. Confirm Pod readiness and healthy `kube-dns` Service endpoints.
4. Test Service discovery, external lookups, and any configured stub-domain behavior.
5. Watch query latency and error rate before closing the change window.

### 861. kube-proxy upgrades

kube-proxy upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 862. kubelet upgrades

kubelet upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Verify supported version skew between kubelet and the current control plane.
2. Drain the node if the maintenance procedure requires workload eviction.
3. Upgrade the kubelet package and approved configuration.
4. Restart kubelet and verify node readiness.
5. Validate Pod lifecycle behavior and uncordon the node if it was drained.

### 863. API deprecation review

API deprecation review is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

**Operational steps**
1. Scan manifests, charts, and CRDs for deprecated or removed API versions.
2. Compare the findings with the target Kubernetes release notes.
3. Convert deprecated objects to supported versions before the upgrade window.
4. Validate admission webhooks, controllers, and automation against the updated schemas.
5. Apply the converted resources in a staging environment and test reconciliation.

### 864. Feature gate changes across upgrades

Feature gate changes across upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 865. Certificate renewal during lifecycle

Certificate renewal during lifecycle is a Kubernetes certificate or trust-management concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 866. Blue-green cluster upgrades

Blue-green cluster upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 867. Canary upgrades

Canary upgrades is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 868. Rollback planning

Rollback planning is a Kubernetes rollback or revision-recovery concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Define the health checks, metrics regressions, or SLO failures that trigger rollback.
2. Record the pre-change versions, manifests, Helm revisions, and package versions.
3. Verify the rollback path in a lower environment for high-risk changes.
4. Execute rollback in reverse dependency order when required.
5. Validate API health, workload readiness, and data consistency after rollback.

## 28. Backup, Disaster Recovery, and Recovery Drills

### 869. Disaster recovery strategy

Disaster recovery strategy is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 870. Recovery point objective

Recovery point objective is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 871. Recovery time objective

Recovery time objective is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 872. Control plane backup

Control plane backup is a Kubernetes backup or state-capture procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 873. etcd backup

etcd backup is a Kubernetes backup or state-capture procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Identify the etcd endpoint and the client certificate, key, and CA files used by the control plane.
2. Run `etcdctl snapshot save /backup/etcd-<timestamp>.db` with the correct TLS parameters.
3. Record the etcd version, endpoint list, and snapshot timestamp.
4. Copy the snapshot to storage outside the control-plane failure domain.
5. Validate the snapshot with `etcdctl snapshot status`.
6. Periodically perform an isolated restore test to confirm the snapshot is usable.

### 874. etcd restore

etcd restore is a Kubernetes restore or rebuild procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Stop or isolate the failed control plane so no component writes stale state during recovery.
2. Run `etcdctl snapshot restore` with a new data directory and intended member configuration.
3. Recreate peer and client certificate placement for the restored member set.
4. Update static Pod manifests or service definitions to use the restored data directory and peer endpoints.
5. Start etcd and confirm quorum and endpoint health.
6. Start kube-apiserver and validate object state, controller health, and node registration.

### 875. Application state backup

Application state backup is a Kubernetes backup or state-capture procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 876. Persistent volume backup

Persistent volume backup is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 877. Crash-consistent backup

Crash-consistent backup is a Kubernetes backup or state-capture procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 878. Application-consistent backup

Application-consistent backup is a Kubernetes backup or state-capture procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 879. Volume snapshot backup

Volume snapshot backup is a Kubernetes storage lifecycle concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 880. Restore testing

Restore testing is a Kubernetes restore or rebuild procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 881. Backup validation drills

Backup validation drills is a Kubernetes backup or state-capture procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Choose the restore scope such as namespace, application, volume, or full control plane.
2. Restore the backup into an isolated validation environment.
3. Verify object integrity, Secret usability, DNS dependencies, and application startup paths.
4. Measure actual recovery time against the recovery objective.
5. Update the backup process or runbook if any validation step fails.

### 882. Namespace-level restore

Namespace-level restore is a Kubernetes restore or rebuild procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 883. Cluster-level restore

Cluster-level restore is a Kubernetes restore or rebuild procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 884. Cross-region recovery

Cross-region recovery is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 885. Multi-cluster failover

Multi-cluster failover is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 886. Backup retention policy

Backup retention policy is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 887. Recovery runbooks

Recovery runbooks is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 888. Control plane rebuild

Control plane rebuild is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

**Operational steps**
1. Provision or rebuild the failed control-plane host with the approved operating system and Kubernetes prerequisites.
2. Restore or regenerate PKI and kubeconfig assets for the control-plane components.
3. Install kubeadm, kubelet, and the runtime at the supported version.
4. Rejoin the node with `kubeadm join --control-plane` or the platform-specific equivalent.
5. Verify static Pods, API reachability, leader election, and etcd membership after rejoin.

### 889. Control plane static Pod recovery

Control plane static Pod recovery is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 890. Worker node replacement

Worker node replacement is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

**Operational steps**
1. Drain the old node if it is still reachable.
2. Remove the old node from the cluster after detach and cleanup complete.
3. Provision the replacement host and install runtime, kubelet, and required agents.
4. Join the replacement node and restore required labels and taints.
5. Validate networking, storage, and workload scheduling on the new node.

### 891. Secret recovery strategy

Secret recovery strategy is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Identify the authoritative secret source such as encrypted backup, external vault, or GitOps secret system.
2. Restore the Secret with the correct namespace, type, and keys.
3. Verify dependent Pods can consume the restored values through mounts or environment variables.
4. Restart workloads only if their secret-consumption path does not hot-reload.
5. Plan post-incident credential rotation if exposure is possible.

### 892. Certificate recovery

Certificate recovery is a Kubernetes certificate or trust-management concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 893. DNS recovery

DNS recovery is a Kubernetes cluster name-resolution concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Inspect CoreDNS Pod health, logs, and restart count in `kube-system`.
2. Verify the `kube-dns` Service and its EndpointSlice backends.
3. Test DNS resolution from an application Pod with `nslookup` or `dig`.
4. Check upstream forwarding, NodeLocal DNSCache, and network reachability if configured.
5. Roll out corrected DNS configuration and re-test from multiple namespaces.

### 894. Ingress and load balancer recovery

Ingress and load balancer recovery is a Kubernetes recovery procedure or resilience control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

**Operational steps**
1. Verify ingress or gateway controller health and leader election.
2. Validate Service backends and external address allocation.
3. Reconcile missing cloud or bare-metal load-balancer state.
4. Confirm DNS records still point to the intended external address.
5. Test route matching and TLS termination after recovery.

## 29. Packaging, Delivery, GitOps, and Release Engineering

### 895. Helm

Helm is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 896. Helm charts

Helm charts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 897. Chart dependencies

Chart dependencies is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 898. Chart repositories

Chart repositories is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 899. Values files

Values files is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 900. Templates

Templates is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 901. Release lifecycle

Release lifecycle is a Kubernetes time-bound coordination primitive that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 902. helm install

helm install is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 903. helm upgrade

helm upgrade is a Kubernetes upgrade-lifecycle procedure that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 904. helm rollback

helm rollback is a Kubernetes rollback or revision-recovery concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 905. helm history

helm history is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 906. helm diff patterns

helm diff patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 907. Kustomize

Kustomize is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 908. Bases

Bases is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 909. Overlays

Overlays is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 910. Strategic merge patches in Kustomize

Strategic merge patches in Kustomize is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 911. JSON patches in Kustomize

JSON patches in Kustomize is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 912. GitOps

GitOps is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 913. Desired state delivery

Desired state delivery is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 914. Drift detection

Drift detection is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 915. Argo CD

Argo CD is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 916. Flux

Flux is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 917. Sync waves

Sync waves is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 918. Hooks

Hooks is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 919. Secret management in GitOps

Secret management in GitOps is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 920. Progressive delivery

Progressive delivery is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 921. Canary rollout controllers

Canary rollout controllers is a Kubernetes controller loop that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 922. Release safety checks

Release safety checks is a Kubernetes time-bound coordination primitive that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

## 30. Commands, Tools, and Day-2 Operations

### 923. kubectl get

kubectl get is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 924. kubectl describe

kubectl describe is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 925. kubectl apply

kubectl apply is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 926. kubectl create

kubectl create is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 927. kubectl delete

kubectl delete is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 928. kubectl edit

kubectl edit is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 929. kubectl patch

kubectl patch is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 930. kubectl replace

kubectl replace is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 931. kubectl diff

kubectl diff is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 932. kubectl logs

kubectl logs is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 933. kubectl exec

kubectl exec is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 934. kubectl attach

kubectl attach is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 935. kubectl cp

kubectl cp is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 936. kubectl port-forward

kubectl port-forward is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 937. kubectl top

kubectl top is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 938. kubectl events

kubectl events is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 939. kubectl rollout

kubectl rollout is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 940. kubectl rollout restart

kubectl rollout restart is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 941. kubectl rollout status

kubectl rollout status is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 942. kubectl rollout undo

kubectl rollout undo is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 943. kubectl scale

kubectl scale is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 944. kubectl expose

kubectl expose is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 945. kubectl label

kubectl label is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 946. kubectl annotate

kubectl annotate is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 947. kubectl taint

kubectl taint is a Kubernetes placement and eviction control that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 948. kubectl cordon

kubectl cordon is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 949. kubectl uncordon

kubectl uncordon is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 950. kubectl drain

kubectl drain is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 951. kubectl debug

kubectl debug is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 952. kubectl auth can-i

kubectl auth can-i is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 953. kubectl api-resources

kubectl api-resources is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 954. kubectl api-versions

kubectl api-versions is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 955. kubectl explain

kubectl explain is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 956. kubectl config contexts

kubectl config contexts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 957. kubectl config use-context

kubectl config use-context is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 958. kubectl config current-context

kubectl config current-context is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 959. kubectl cluster-info

kubectl cluster-info is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 960. kubectl get events

kubectl get events is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 961. jsonpath output

jsonpath output is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 962. custom-columns output

custom-columns output is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 963. helm list

helm list is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 964. helm status

helm status is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 965. helm get values

helm get values is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 966. helm template

helm template is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 967. helm lint

helm lint is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 968. kustomize build

kustomize build is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 969. etcdctl

etcdctl is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 970. crictl

crictl is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 971. ctr

ctr is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 972. nerdctl concepts

nerdctl concepts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 973. journalctl

journalctl is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 974. systemctl

systemctl is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 975. ss and netstat for networking diagnostics

ss and netstat for networking diagnostics is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 976. iptables and nft diagnostics

iptables and nft diagnostics is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 977. ip route diagnostics

ip route diagnostics is a Kubernetes traffic-routing or forwarding concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

## 31. Custom Resources, Operators, and Extensibility

### 978. CustomResourceDefinition

CustomResourceDefinition is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 979. Structural schemas

Structural schemas is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 980. CRD versions

CRD versions is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 981. Served versions

Served versions is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 982. Stored versions

Stored versions is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 983. Conversion strategy

Conversion strategy is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 984. Conversion webhook

Conversion webhook is a Kubernetes external callback integration that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 985. Additional printer columns

Additional printer columns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 986. Subresources for CRDs

Subresources for CRDs is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 987. Status subresource for CRDs

Status subresource for CRDs is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 988. Scale subresource for CRDs

Scale subresource for CRDs is a Kubernetes scaling control or replica-management surface that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 989. Custom controllers

Custom controllers is a Kubernetes controller loop that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 990. client-go informers

client-go informers is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 991. controller-runtime

controller-runtime is a Kubernetes controller loop that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 992. Operators

Operators is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 993. Operator reconciliation patterns

Operator reconciliation patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 994. Finalizer patterns in operators

Finalizer patterns in operators is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 995. Status conditions in custom resources

Status conditions in custom resources is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 996. Kubebuilder concepts

Kubebuilder concepts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 997. Operator SDK concepts

Operator SDK concepts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

## 32. Service Mesh, Multi-Cluster, and Platform Engineering

### 998. Service mesh

Service mesh is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 999. Sidecar injection

Sidecar injection is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1000. Ambient mesh concepts

Ambient mesh concepts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1001. mTLS

mTLS is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1002. Traffic policy in service mesh

Traffic policy in service mesh is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 1003. Service mesh operational concerns

Service mesh operational concerns is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 1004. Cilium advanced networking

Cilium advanced networking is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1005. eBPF service acceleration

eBPF service acceleration is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 1006. Cluster API

Cluster API is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1007. Machine objects

Machine objects is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1008. MachineDeployments

MachineDeployments is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1009. Bare-metal load balancing

Bare-metal load balancing is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1010. MetalLB

MetalLB is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1011. Node Feature Discovery

Node Feature Discovery is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1012. Virtual kubelet concepts

Virtual kubelet concepts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1013. Cluster federation concepts

Cluster federation concepts is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1014. Multi-cluster policy management

Multi-cluster policy management is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 1015. Multi-cluster service discovery

Multi-cluster service discovery is a Kubernetes service-discovery or virtual-addressing concept that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 1016. Multi-cluster traffic failover

Multi-cluster traffic failover is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1017. Multi-tenancy patterns

Multi-tenancy patterns is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1018. Platform guardrails

Platform guardrails is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1019. Golden path templates

Golden path templates is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1020. Internal developer platforms

Internal developer platforms is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1021. FinOps in Kubernetes

FinOps in Kubernetes is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1022. Cost allocation by namespace

Cost allocation by namespace is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1023. Compliance controls in Kubernetes

Compliance controls in Kubernetes is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1024. Policy as code

Policy as code is a Kubernetes policy object or policy mechanism that affects component behavior, API processing, runtime execution, or operational outcomes where that field, resource, or subsystem is consumed.

### 1025. Workload placement governance

Workload placement governance is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1026. Advanced scheduling integrations

Advanced scheduling integrations is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.

### 1027. Future-facing Kubernetes API evolution

Future-facing Kubernetes API evolution is a Kubernetes mechanism or resource that influences control-plane behavior, node execution, networking, storage, security, or day-2 operations depending on which component consumes it.
