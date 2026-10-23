# GCP Frequently Used Services and Sub-Services Overview

## Compute

Core compute services provide virtual machines, container orchestration, and serverless runtime execution.
These services define how workloads are instantiated, scaled, isolated, and exposed in Google Cloud.

### Compute Engine
Compute Engine is an infrastructure service for provisioning virtual machine instances on Google Cloud.
It exposes zonal and regional compute resources with configurable machine, disk, and network settings.

- **VM instances** 
 VM instances are virtualized guest systems created from machine, disk, and network definitions.
 They run operating systems and application processes as the primary compute resource.

- **Instance templates** 
 Instance templates are reusable VM configuration objects.
 They define machine type, boot disk, metadata, tags, and interface settings for instance creation.

- **Managed instance groups** 
 Managed instance groups are controller resources that maintain a fleet of homogeneous VM instances.
 They apply instance templates and support autohealing, autoscaling, and rolling update operations.

- **Autoscaling** 
 Autoscaling is a control mechanism that adjusts instance count from observed load signals.
 It scales capacity using utilization targets, load balancing demand, or custom metric thresholds.

- **Health checks** 
 Health checks are probe definitions that test endpoint availability and responsiveness.
 They are evaluated by load balancers and instance group controllers to make traffic and repair decisions.

- **Reservations** 
 Reservations are zonal capacity allocation objects for specific machine resource shapes.
 They hold compute capacity to reduce provisioning failure during demand or supply constraints.

- **Sole-tenant nodes** 
 Sole-tenant nodes are dedicated physical hosts assigned to a single customer tenancy.
 They provide hardware isolation and placement control for compliance or licensing requirements.

- **Spot VMs** 
 Spot VMs are interruptible compute instances offered from surplus capacity.
 They can be terminated by the platform and are suited to non-durable or restartable workloads.

- **Shielded VMs** 
 Shielded VMs are instances with boot integrity and guest verification protections enabled.
 They use secure boot, virtual TPM, and integrity monitoring to harden the VM startup chain.

- **OS Login** 
 OS Login is an identity-based access mechanism for Linux VM authentication.
 It maps IAM identities to OS-level accounts and centralizes SSH and sudo authorization.

- **Startup scripts** 
 Startup scripts are boot-time command payloads attached to VM resources.
 They run during instance initialization to install packages or configure local runtime state.

- **Metadata** 
 Metadata is a project or instance-scoped key-value configuration store.
 It is exposed to guest workloads through the metadata server for runtime discovery and initialization.

- **Persistent Disk** 
 Persistent Disk is network-attached block storage for VM instance attachment.
 It provides durable volumes with resize, snapshot, and replicated deployment options.

- **Snapshots** 
 Snapshots are incremental point-in-time copies of Persistent Disk volumes.
 They are stored independently of the source disk and used for backup, restore, or cloning.

- **Live migration controls** 
 Live migration controls are VM scheduling settings for host maintenance behavior.
 They determine whether a running instance is transparently migrated or terminated during infrastructure events.

### Google Kubernetes Engine (GKE)
GKE is a managed Kubernetes service for running containerized workloads in clusters.
It manages the control plane and integrates Kubernetes resources with Google Cloud networking, identity, and policy systems.

- **Standard** 
 Standard is the GKE operating mode where node infrastructure remains user-managed.
 It exposes cluster and node pool configuration for direct operational control.

- **Autopilot** 
 Autopilot is the GKE operating mode where node lifecycle is platform-managed.
 It schedules workloads onto managed capacity and enforces predefined resource and security constraints.

- **Clusters** 
 Clusters are Kubernetes control plane and worker node resource domains.
 They host API objects, scheduling state, and runtime execution for container workloads.

- **Node pools** 
 Node pools are groups of worker nodes sharing a common machine and configuration profile.
 They separate workload placement by hardware, image, upgrade policy, or scheduling constraints.

- **Cluster autoscaler** 
 Cluster autoscaler is a control loop that changes node count based on unschedulable pod demand.
 It expands or contracts node pools to align cluster capacity with requested resources.

- **Horizontal Pod Autoscaler** 
 Horizontal Pod Autoscaler is a Kubernetes controller for changing replica count from observed metrics.
 It scales a workload horizontally using CPU, memory, or external/custom signals.

- **Workload Identity** 
 Workload Identity is an identity federation model between Kubernetes and Google IAM.
 It binds Kubernetes service accounts to Google service accounts without static key distribution.

- **Ingress** 
 Ingress is a Kubernetes API resource for HTTP and HTTPS traffic routing.
 It maps hosts and paths to backend services through an integrated load balancing implementation.

- **Gateway API** 
 Gateway API is a Kubernetes resource model for layered traffic management.
 It separates gateway ownership from route ownership and defines richer routing semantics than Ingress.

- **Private clusters** 
 Private clusters are GKE clusters with privately addressed nodes and restricted control plane exposure.
 They reduce public network reachability for cluster infrastructure and administrative endpoints.

- **Release channels** 
 Release channels are version cadence tracks for GKE cluster upgrades.
 They control how quickly Kubernetes versions and related platform updates are applied.

- **Fleet management** 
 Fleet management is the administrative grouping model for multiple enrolled clusters.
 It enables shared policy, identity, and operational controls across cluster boundaries.

- **Config Sync** 
 Config Sync is a configuration reconciliation component for Kubernetes resources from Git.
 It continuously applies declared manifests to maintain cluster state convergence.

- **Policy Controller** 
 Policy Controller is an admission and audit policy engine for Kubernetes resources.
 It validates objects against constraint definitions during creation and during compliance scans.

- **Binary Authorization** 
 Binary Authorization is a deploy-time policy enforcement service for container image trust.
 It checks attestations and policy rules before allowing workloads to run.

- **Multi-cluster features** 
 Multi-cluster features are fleet-level capabilities spanning more than one Kubernetes cluster.
 They coordinate discovery, policy, and traffic behavior across distributed cluster topologies.

### Cloud Run
Cloud Run is a managed runtime for stateless container services and batch jobs.
It executes OCI-compatible containers with revision-based deployment and request-driven scaling semantics.

- **Services** 
 Services are long-lived Cloud Run resources that expose a stable network endpoint.
 They route requests to one or more immutable revisions of a containerized application.

- **Jobs** 
 Jobs are Cloud Run resources for finite execution of container tasks.
 They run to completion rather than serving inbound network traffic.

- **Revisions** 
 Revisions are immutable deployment snapshots of a Cloud Run service definition.
 Each revision captures container image, runtime settings, and traffic eligibility state.

- **Traffic splitting** 
 Traffic splitting is a revision-level request distribution mechanism.
 It assigns request percentages across deployed revisions for controlled rollout behavior.

- **Autoscaling** 
 Autoscaling is the runtime controller for adjusting active container instance count.
 It reacts to request volume and concurrency settings and can scale to zero when idle.

- **Concurrency** 
 Concurrency is the per-instance request handling limit for a Cloud Run container.
 It defines how many simultaneous requests can be processed before additional instances are created.

- **IAM authentication** 
 IAM authentication is the identity-based invocation control model for Cloud Run endpoints.
 It requires authorized principals to present valid credentials for protected service access.

- **Private connectivity** 
 Private connectivity is the network integration model for private service reachability from Cloud Run.
 It uses internal ingress and VPC-connected paths to reach non-public resources.

### Cloud Functions
Cloud Functions is an event-driven function execution service for discrete handler logic.
It runs code in response to HTTP requests or event sources without direct server management.

- **HTTP triggers** 
 HTTP triggers are endpoint bindings that invoke a function through web requests.
 They expose request-response execution over HTTPS.

- **Event triggers** 
 Event triggers are bindings between platform events and function invocation.
 They cause execution when subscribed service events are emitted.

- **Pub/Sub triggers** 
 Pub/Sub triggers are event bindings from a messaging topic to a function.
 They invoke code asynchronously when messages are delivered from the topic.

- **Storage triggers** 
 Storage triggers are object event bindings from Cloud Storage to a function.
 They fire on object lifecycle actions such as create, delete, or metadata change.

### App Engine
App Engine is a platform runtime for deploying web applications and backend services.
It abstracts instance provisioning and supports versioned application deployment models.

- **Standard environment** 
 Standard environment is the sandboxed App Engine runtime model for supported language runtimes.
 It uses a constrained execution environment with fast startup and managed scaling behavior.

- **Flexible environment** 
 Flexible environment is the App Engine runtime model based on containerized workloads on VMs.
 It supports custom runtime behavior and broader OS-level configuration than the standard model.

- **Versions** 
 Versions are independently deployed builds of an App Engine service.
 They coexist under one service and provide isolated deployment states for routing control.

- **Traffic splitting** 
 Traffic splitting is the version-level routing mechanism within an App Engine service.
 It distributes requests across deployed versions by configured proportions or other routing rules.

---

## Containers, Config, and Platform Control

These services manage declarative configuration, policy enforcement, and multi-cluster platform administration.
They define how cluster state, governance, and hybrid control planes are coordinated.

### Config Sync
Config Sync is a declarative configuration synchronization service for Kubernetes resources.
It continuously reconciles cluster state against a Git-backed desired configuration source.

- **Git-backed cluster config sync** 
 Git-backed cluster config sync is the repository-driven reconciliation model for Kubernetes manifests.
 It pulls declared resources from version control and applies them to target clusters.

- **Multi-cluster config management** 
 Multi-cluster config management is the distribution of common configuration across several clusters.
 It enforces consistent manifest state and policy application at fleet scope.

### Policy Controller
Policy Controller is a Kubernetes policy validation system based on constraint evaluation.
It intercepts resource definitions and audits cluster objects against declared rules.

- **Policy enforcement** 
 Policy enforcement is the validation process that checks resources against configured constraints.
 It rejects or reports objects that violate declared policy conditions.

- **Constraint templates** 
 Constraint templates are reusable policy logic definitions with schema and validation behavior.
 They define the structure from which concrete policy constraints are instantiated.

- **Admission control** 
 Admission control is the API interception stage before a Kubernetes object is persisted.
 It applies policy checks during create, update, or delete request processing.

### Anthos / GKE Enterprise
Anthos or GKE Enterprise is a fleet-oriented Kubernetes management platform spanning multiple environments.
It extends cluster operations with centralized governance, identity, and configuration capabilities.

- **Multi-cluster management** 
 Multi-cluster management is the unified administration model for multiple Kubernetes clusters.
 It centralizes registration, visibility, and cross-cluster operational control.

- **Hybrid platform operations** 
 Hybrid platform operations are management workflows spanning cloud and on-premises cluster infrastructure.
 They provide one operational model across heterogeneous Kubernetes deployment locations.

- **Fleet-level governance** 
 Fleet-level governance is the application of policy and configuration at the registered fleet boundary.
 It propagates controls uniformly across all enrolled clusters.

---

## CI/CD, Delivery, and Automation

These services automate software build, release, artifact storage, and workflow execution.
They define how source changes become versioned artifacts and deployed runtime configurations.

### Cloud Build
Cloud Build is a managed build execution service for compiling, testing, and packaging software.
It runs containerized build steps in ephemeral workers from declarative build definitions.

- **Build steps** 
 Build steps are ordered execution units within a build definition.
 Each step runs a container command with declared inputs, environment, and outputs.

- **Triggers** 
 Triggers are event bindings that start builds from repository or source changes.
 They evaluate source conditions such as branch, tag, or event type before execution.

- **Substitutions** 
 Substitutions are runtime variable mappings injected into build configuration fields.
 They parameterize build definitions without changing the source file structure.

- **Private pools** 
 Private pools are isolated worker pools for Cloud Build execution.
 They place build jobs on dedicated network-reachable infrastructure under controlled connectivity.

- **Artifact build pipelines** 
 Artifact build pipelines are build workflows that produce deployable packages or images.
 They transform source inputs into versioned outputs for later distribution or deployment.

### Cloud Deploy
Cloud Deploy is a managed release orchestration service for progressive software promotion.
It coordinates deployment state transitions across defined target environments.

- **Delivery pipelines** 
 Delivery pipelines are ordered deployment progression definitions for release promotion.
 They define the sequence of targets and rollout behavior between environments.

- **Targets** 
 Targets are deployment destination descriptors such as clusters or runtime environments.
 They encapsulate environment-specific settings and deployment endpoint metadata.

- **Releases** 
 Releases are immutable deployment records referencing a specific application artifact set.
 They serve as the promoted unit across one or more targets in a pipeline.

- **Rollouts** 
 Rollouts are executions of a release against a single target environment.
 They track deployment progress, state transitions, and outcome for that target.

- **Promotion workflows** 
 Promotion workflows are stage advancement operations between targets in a delivery pipeline.
 They control when and how a release moves forward across environments.

- **Approvals** 
 Approvals are explicit human-controlled gates within deployment progression.
 They pause rollout advancement until an authorized principal allows continuation.

### Artifact Registry
Artifact Registry is a managed repository service for storing build outputs and packages.
It manages versioned artifacts with access control and integration into deployment runtimes.

- **Docker repositories** 
 Docker repositories are OCI-compatible storage backends for container images.
 They store image manifests and layers for runtime pull operations.

- **Helm repositories** 
 Helm repositories are chart package stores for Kubernetes application deployment metadata.
 They host versioned Helm chart archives and index information.

- **Maven repositories** 
 Maven repositories are package endpoints for Java artifacts and dependencies.
 They publish and resolve versioned components such as JARs and metadata descriptors.

- **npm repositories** 
 npm repositories are package stores for Node.js ecosystem modules.
 They support publishing and dependency retrieval for versioned JavaScript packages.

- **Python repositories** 
 Python repositories are package indexes for Python distributions and wheels.
 They support versioned package publication and pip-compatible dependency retrieval.

### Workflows
Workflows is a managed orchestration service for stateful multi-step process execution.
It coordinates API calls, variable handling, branching, and retries in a declarative flow definition.

- **Service orchestration** 
 Service orchestration is the sequencing of calls across multiple external or internal services.
 It preserves execution state and response data between dependent steps.

- **Retry handling** 
 Retry handling is the failure recovery mechanism for re-executing transiently failed steps.
 It applies configured retry conditions, backoff strategy, and attempt limits.

- **Conditional flow** 
 Conditional flow is branch selection based on evaluated expressions within a workflow.
 It changes execution path according to runtime data and decision logic.

- **API workflow chaining** 
 API workflow chaining is the composition of multiple API calls into one executable process.
 It passes outputs from one request into subsequent operations across the chain.

### Cloud Scheduler
Cloud Scheduler is a managed time-based job triggering service.
It emits scheduled invocations to supported target types using cron-defined timing rules.

- **Cron scheduling** 
 Cron scheduling is the time expression model used to define recurring execution intervals.
 It specifies minute, hour, day, month, and weekday timing for job dispatch.

- **HTTP targets** 
 HTTP targets are scheduled outbound web requests sent to an endpoint.
 They invoke remote handlers with configured method, headers, and payload.

- **Pub/Sub targets** 
 Pub/Sub targets are scheduled message publications to a topic.
 They convert time-based jobs into asynchronous event emissions.

### Pub/Sub
Pub/Sub is a managed asynchronous messaging service based on topics and subscriptions.
It decouples message producers from consumers through durable delivery state.

- **Topics** 
 Topics are named ingress resources for published messages.
 They receive message writes and fan out delivery to attached subscriptions.

- **Subscriptions** 
 Subscriptions are delivery resources attached to a topic.
 They maintain acknowledgement state and expose messages to consumers.

- **Push delivery** 
 Push delivery is a subscription mode that sends messages to a configured HTTPS endpoint.
 It transfers delivery responsibility to the service by initiating outbound requests.

- **Pull delivery** 
 Pull delivery is a subscription mode where consumers request available messages explicitly.
 It gives the client direct control over fetch rate and acknowledgement timing.

- **Dead-letter topics** 
 Dead-letter topics are alternate destinations for repeatedly undeliverable messages.
 They receive messages after retry policy exhaustion for later inspection or replay.

### Terraform
Terraform is a declarative infrastructure provisioning system using resource graph execution.
It computes desired state changes from configuration files and applies them through provider APIs.

- **Resource provisioning** 
 Resource provisioning is the creation and reconciliation of infrastructure from declarative definitions.
 It resolves dependencies and issues API operations to converge actual state with desired state.

- **Module-based IaC** 
 Module-based IaC is the composition model for reusable infrastructure definitions.
 It packages resources and variables into parameterized units for repeated deployment patterns.

- **Policy-driven infrastructure workflows** 
 Policy-driven infrastructure workflows are provisioning processes constrained by governance rules.
 They evaluate infrastructure changes against policy before or during execution.

### Infrastructure Manager
Infrastructure Manager is a managed service for executing Terraform-defined Google Cloud deployments.
It runs plan and apply operations within a platform-controlled workflow.

- **Managed Terraform execution** 
 Managed Terraform execution is the hosted runtime for Terraform planning and application.
 It removes the need to self-operate a separate execution environment for IaC runs.

- **IaC deployment workflows** 
 IaC deployment workflows are orchestrated sequences for infrastructure planning, approval, and apply.
 They standardize deployment state transitions for declarative cloud resources.

### Config Controller
Config Controller is a managed Kubernetes-based control plane for declarative resource management.
It hosts configuration and policy objects as API resources in a centralized administrative environment.

- **Centralized config management** 
 Centralized config management is the aggregation of configuration control into one managed plane.
 It applies desired state and governance policies across distributed resources from a single authority.

- **Policy and resource control plane** 
 Policy and resource control plane is the API-hosted management layer for declarative objects.
 It stores and reconciles configuration resources that define policy and infrastructure intent.

### Deployment Manager
Deployment Manager is a legacy declarative provisioning service for Google Cloud resources.
It applies template-based configurations through a native Google Cloud deployment engine.

- **Legacy infrastructure deployment service** 
 Legacy infrastructure deployment service is the template-driven resource creation model of Deployment Manager.
 It provisions cloud resources from YAML or Python-based configuration definitions.

### Source Repositories
Source Repositories is a legacy managed Git hosting service in Google Cloud.
It stores version-controlled source code within a Google-operated repository backend.

- **Legacy hosted source control** 
 Legacy hosted source control is a managed Git repository service for source code storage.
 It provides repository hosting without requiring self-managed version control infrastructure.

---

## Networking and Traffic Management

These services define private networking, traffic routing, hybrid connectivity, and edge security behavior.
They control how packets and requests move between workloads, users, and external systems.

### VPC
VPC is the software-defined private networking service for Google Cloud resources.
It provides IP segmentation, routing, and traffic policy boundaries for deployed workloads.

- **Networks** 
 Networks are top-level private routing domains within VPC.
 They contain subnets, routes, firewall rules, and other network-scoped resources.

- **Subnets** 
 Subnets are regional IP allocation ranges within a VPC network.
 They assign address space to instances and interfaces created in that region.

- **Routes** 
 Routes are forwarding rules that map destination prefixes to next hops.
 They determine packet path selection within and beyond the VPC boundary.

- **Firewall rules** 
 Firewall rules are stateful packet filtering policies attached to VPC traffic evaluation.
 They allow or deny ingress and egress based on protocol, port, direction, and target scope.

- **VPC Flow Logs** 
 VPC Flow Logs are telemetry records describing sampled network flow metadata.
 They capture source, destination, protocol, and traffic characteristics for subnet traffic analysis.

- **Private Google Access** 
 Private Google Access is a network feature allowing private IP resources to reach Google APIs.
 It preserves private addressing while routing API traffic over Google-managed network paths.

### Shared VPC
Shared VPC is a cross-project network sharing model centered on a host project.
It separates network ownership from workload project ownership while preserving common connectivity policy.

- **Host projects** 
 Host projects are the projects that own the shared VPC network resources.
 They contain the subnets and network configuration consumed by attached service projects.

- **Service projects** 
 Service projects are projects that attach workloads to subnets in a host project.
 They consume shared network resources without owning the network definition.

- **Centralized network administration** 
 Centralized network administration is the operational model where network policy is managed in one place.
 It consolidates routing, firewalling, and subnet governance across multiple workload projects.

### Cloud Load Balancing
Cloud Load Balancing is a managed traffic distribution service for internal and external clients.
It maps frontend listeners to backend targets using health-aware routing decisions.

- **External HTTP(S)** 
 External HTTP(S) is the public application-layer load balancing mode for web traffic.
 It terminates client HTTP or HTTPS connections and routes requests using URL-based logic.

- **Internal HTTP(S)** 
 Internal HTTP(S) is the private application-layer load balancing mode for internal consumers.
 It serves HTTP or HTTPS traffic within private Google Cloud network boundaries.

- **TCP/UDP** 
 TCP/UDP is the transport-layer load balancing mode for non-HTTP application traffic.
 It distributes packets or connections to backends without HTTP-aware request parsing.

- **SSL proxy** 
 SSL proxy is a proxy-based load balancing mode that terminates TLS for TCP applications.
 It accepts encrypted client connections and forwards proxied traffic to backend services.

- **Backend services** 
 Backend services are load balancing resources that define traffic handling for a backend group.
 They bind health checks, balancing policy, and attached endpoints into one routing unit.

- **Health checks** 
 Health checks are probe resources used to determine backend serving state.
 They drive backend eligibility decisions for traffic distribution and failover behavior.

### Cloud DNS
Cloud DNS is a managed authoritative DNS service for public and private zones.
It stores DNS record data and answers queries for delegated domains.

- **Public zones** 
 Public zones are authoritative DNS zones resolvable from the public internet.
 They host record sets for externally delegated domain namespaces.

- **Private zones** 
 Private zones are DNS zones resolvable only within associated VPC networks.
 They provide internal name resolution without public visibility.

- **Managed DNS records** 
 Managed DNS records are authoritative record sets stored under a DNS zone.
 They map names to addresses, aliases, service data, or arbitrary text metadata.

### Cloud NAT
Cloud NAT is a managed source network address translation service for private resources.
It enables outbound internet access without assigning external IP addresses to each workload.

- **Private egress** 
 Private egress is outbound connectivity initiated from privately addressed resources.
 It preserves inbound isolation while permitting external destination access.

- **NAT gateway for private workloads** 
 NAT gateway for private workloads is the address translation function performed by Cloud NAT.
 It rewrites private source addresses to routable external addresses for outbound sessions.

### Cloud Router
Cloud Router is a managed dynamic routing control plane for hybrid network connectivity.
It exchanges route information using BGP with attached external routers or services.

- **Dynamic routing** 
 Dynamic routing is the automatic learning and advertisement of network prefixes.
 It updates routing behavior as topology and available paths change.

- **BGP routing for hybrid connectivity** 
 BGP routing for hybrid connectivity is the route exchange mechanism between cloud and external networks.
 It establishes peering sessions to advertise reachable prefixes over VPN or Interconnect.

### Cloud CDN
Cloud CDN is an edge caching layer integrated with Google load balancing.
It serves cacheable content from edge locations to reduce origin fetch latency.

- **Edge caching** 
 Edge caching is the storage of cacheable responses at geographically distributed serving points.
 It reduces round-trip distance and repeat origin retrieval for identical content.

- **Static and dynamic acceleration** 
 Static and dynamic acceleration is the optimization of response delivery for different content types.
 It improves transfer performance through caching and edge-assisted request handling behavior.

### Cloud Armor
Cloud Armor is a policy engine for edge and load balancer traffic protection.
It evaluates requests against security rules before forwarding them to backends.

- **WAF policies** 
 WAF policies are application-layer inspection rules for HTTP request filtering.
 They match requests against attack signatures or custom expressions to allow or block traffic.

- **Rate limiting** 
 Rate limiting is a traffic control function that constrains request frequency by policy key.
 It mitigates abuse by enforcing thresholds over time windows.

- **Edge security filtering** 
 Edge security filtering is request evaluation performed before backend delivery.
 It reduces backend exposure by enforcing security decisions at the network edge.

### Private Service Connect
Private Service Connect is a private service connectivity framework using internal IP endpoints.
It decouples service producers and consumers while keeping traffic on private network paths.

- **Private managed service access** 
 Private managed service access is the consumption model for reaching supported services through private endpoints.
 It exposes managed service interfaces without public IP dependency.

- **Private service publishing** 
 Private service publishing is the producer-side model for exposing services to consumer networks privately.
 It binds a service implementation to a private endpoint interface consumable by other tenants or projects.

### Traffic Director
Traffic Director is a managed traffic control plane for service networking and proxy configuration.
It distributes routing and policy state to data plane proxies or clients.

- **Service traffic control** 
 Service traffic control is the policy-based steering of requests among service instances.
 It defines balancing, failover, and routing behavior for service communication paths.

- **Advanced routing policies** 
 Advanced routing policies are rule sets for traffic selection using request or metadata attributes.
 They support weighted, header-based, path-based, and conditional routing decisions.

- **Service mesh traffic management** 
 Service mesh traffic management is the governance of east-west traffic inside a service mesh topology.
 It applies centrally defined communication policies across participating services.

### Service Directory
Service Directory is a managed registry for service names, endpoints, and metadata.
It provides structured discovery information for internal service consumers.

- **Internal service discovery** 
 Internal service discovery is the lookup process for resolving internal service endpoints by name.
 It enables clients to find current endpoint information without hardcoded addresses.

- **Service registration** 
 Service registration is the publication of endpoint and metadata records into the directory.
 It makes service instances discoverable through the registry namespace model.

### Cloud VPN
Cloud VPN is a managed IPsec tunnel service for secure network connectivity.
It encrypts traffic between Google Cloud and external networks over public transport paths.

- **Site-to-site VPN** 
 Site-to-site VPN is the network model that links two routed private networks through encrypted tunnels.
 It carries traffic between on-premises and cloud environments using tunnel endpoints and routing policies.

- **Hybrid connectivity** 
 Hybrid connectivity is the connectivity pattern between cloud networks and external infrastructure.
 It combines secure routing paths and network integration across environment boundaries.

### Cloud Interconnect
Cloud Interconnect is a private connectivity service between customer networks and Google Cloud.
It provides high-bandwidth transport that does not traverse the public internet data path.

- **Dedicated Interconnect** 
 Dedicated Interconnect is the direct physical connection model into Google’s edge network.
 It uses provisioned circuits for private, high-capacity hybrid network transport.

- **Partner Interconnect** 
 Partner Interconnect is the provider-mediated private connectivity model for Google Cloud access.
 It delivers private connectivity through a supported third-party network operator.

- **Hybrid private connectivity** 
 Hybrid private connectivity is the private transport path linking cloud and external networks.
 It carries enterprise traffic without relying on public internet routing.

### Network Intelligence Center
Network Intelligence Center is a network diagnostics and topology analysis suite.
It evaluates reachability, configuration correctness, and network structure from deployed state.

- **Connectivity Tests** 
 Connectivity Tests are simulated path analyses between defined source and destination endpoints.
 They inspect routing, firewall, and configuration state to explain reachability outcomes.

- **Network Analyzer** 
 Network Analyzer is a configuration assessment engine for cloud network resources.
 It scans deployed state and reports findings related to performance, exposure, or misconfiguration.

- **Performance diagnostics** 
 Performance diagnostics are analyses of network latency, packet loss, or transport quality symptoms.
 They help isolate communication degradation across connected network segments.

- **Topology visibility** 
 Topology visibility is the graphical representation of deployed network relationships and paths.
 It exposes how resources are connected across routing and service boundaries.

---

## Identity, Security, and Compliance

These services define authentication, authorization, key management, auditability, and policy enforcement.
They control who can access resources, how secrets are protected, and how governance is applied.

### IAM
IAM is the authorization system for assigning permissions to principals over Google Cloud resources.
It evaluates policy bindings and conditions at request time to determine allowed actions.

- **Predefined roles** 
 Predefined roles are Google-managed collections of permissions for common access patterns.
 They provide stable permission bundles maintained by the platform.

- **Custom roles** 
 Custom roles are user-defined permission bundles created for organization-specific access models.
 They allow exact permission scoping beyond predefined role boundaries.

- **IAM Conditions** 
 IAM Conditions are expressions attached to policy bindings for contextual authorization.
 They restrict permission grants based on attributes such as resource, time, or request state.

- **Policy bindings** 
 Policy bindings are entries that connect a principal to a role on a resource.
 They form the atomic authorization relationships evaluated by IAM.

### Service Accounts
Service Accounts are non-human identities used by applications and platform workloads.
They authenticate to Google APIs and represent machine principals in IAM policy.

- **Workload identity principals** 
 Workload identity principals are non-human execution identities recognized during authorization evaluation.
 They represent workloads rather than users in credential and policy flows.

- **Service-to-service authentication** 
 Service-to-service authentication is the identity verification model between machine workloads.
 It uses signed credentials or tokens to establish trusted caller identity across services.

### Workload Identity
Workload Identity is the identity mapping mechanism between GKE workloads and Google IAM.
It replaces embedded service account keys with federated workload access.

- **GKE workload-to-IAM mapping** 
 GKE workload-to-IAM mapping is the binding between a Kubernetes service account and a Google service account.
 It lets pods obtain Google API access under mapped IAM permissions.

### Secret Manager
Secret Manager is a managed secret storage and access control service.
It stores sensitive values as versioned secret resources protected by IAM and audit logging.

- **Secret versions** 
 Secret versions are immutable revisions of the value stored in a secret resource.
 They allow controlled rotation, rollback, and staged activation of secret material.

- **Rotation workflows** 
 Rotation workflows are processes that replace active secret values on a defined cadence or trigger.
 They update version state while preserving controlled secret lifecycle transitions.

- **IAM-controlled secret access** 
 IAM-controlled secret access is the authorization model for viewing or managing secret data.
 It restricts operations on secret resources through role-based policy evaluation.

### Cloud KMS
Cloud KMS is a managed cryptographic key service for encryption, decryption, and signing operations.
It stores key material under controlled lifecycle and access policies.

- **Key rings** 
 Key rings are regional organizational containers for cryptographic keys.
 They group keys by location and administrative structure.

- **Crypto keys** 
 Crypto keys are logical resources representing a cryptographic function and policy boundary.
 They own one or more key versions used for actual cryptographic operations.

- **Key versions** 
 Key versions are immutable instances of cryptographic material under a crypto key.
 They support enablement, disablement, rotation, and version-specific operation control.

- **Encryption key management** 
 Encryption key management is the lifecycle control process for keys used in data protection.
 It governs generation, rotation, storage, and use of key material for encryption workloads.

- **Signing keys** 
 Signing keys are cryptographic keys used to create or verify digital signatures.
 They support integrity and authenticity guarantees for signed payloads or artifacts.

### Certificate Manager
Certificate Manager is a managed service for TLS certificate provisioning and association.
It stores certificate resources and maps them to serving endpoints for TLS termination.

- **Managed certificates** 
 Managed certificates are platform-provisioned TLS certificate resources for validated domains.
 They automate issuance and renewal without manual certificate upload.

- **Certificate maps** 
 Certificate maps are hostname-to-certificate association resources for serving infrastructure.
 They select which certificate is presented for incoming TLS requests.

- **TLS lifecycle management** 
 TLS lifecycle management is the handling of certificate issuance, renewal, rotation, and retirement.
 It maintains valid certificate state throughout the endpoint security lifecycle.

### Organization Policy
Organization Policy is a constraint system for governing allowable resource configurations.
It applies inherited policy rules across organizations, folders, and projects.

- **Org-level constraints** 
 Org-level constraints are top-down governance rules attached at the organizational hierarchy.
 They define allowed or denied behaviors for descendant resources and projects.

- **Resource restrictions** 
 Resource restrictions are configuration constraints on what can be created or modified.
 They limit deployment options such as regions, external IP use, or service settings.

- **Policy guardrails** 
 Policy guardrails are preventive controls that stop resources from violating governance rules.
 They are enforced during resource lifecycle operations rather than after deployment.

### VPC Service Controls
VPC Service Controls is a perimeter-based isolation system for managed service data access.
It restricts data movement between protected services and external contexts.

- **Service perimeters** 
 Service perimeters are logical boundaries around projects and managed services.
 They define trusted scopes from which protected service access is permitted.

- **Data exfiltration control** 
 Data exfiltration control is the restriction of data access from outside approved security boundaries.
 It reduces the risk of managed service data leaving protected environments.

### Security Command Center
Security Command Center is a centralized security findings and posture management platform.
It aggregates asset risk signals, detections, and configuration issues into one security view.

- **Security findings** 
 Security findings are structured issue records representing detected threats or control failures.
 They contain severity, source, and affected resource context for investigation.

- **Posture management** 
 Posture management is the assessment of deployed resources against security best-practice controls.
 It measures compliance state and identifies systemic gaps in cloud configuration.

- **Misconfiguration visibility** 
 Misconfiguration visibility is the exposure of insecure or noncompliant resource settings.
 It highlights configuration states that increase attack surface or policy drift.

### Cloud Audit Logs
Cloud Audit Logs is the event logging system for administrative, data, and policy actions.
It records request activity across Google Cloud services for traceability and compliance.

- **Admin Activity** 
 Admin Activity logs are records of control plane operations that change configuration or state.
 They capture create, update, delete, and administrative management actions.

- **Data Access** 
 Data Access logs are records of operations that read, write, or execute against data-bearing resources.
 They provide visibility into content-level access rather than just configuration changes.

- **System Event** 
 System Event logs are platform-generated records describing internal lifecycle actions on resources.
 They capture maintenance, scheduling, and other service-initiated state transitions.

- **Policy Denied** 
 Policy Denied logs are records of requests blocked by IAM or organizational policy.
 They show attempted actions that failed due to enforced authorization or governance rules.

### Policy Troubleshooter
Policy Troubleshooter is a diagnostic service for explaining authorization decisions.
It evaluates policy paths and effective permissions for a principal-resource-action combination.

- **IAM access path troubleshooting** 
 IAM access path troubleshooting is the decomposition of an authorization result into contributing bindings and conditions.
 It reveals which policies granted or denied a requested action.

- **Effective permission analysis** 
 Effective permission analysis is the calculation of all permissions a principal holds on a resource.
 It resolves inherited roles and conditions into evaluated access capability.

---

## Observability and Reliability

These services collect telemetry, generate incidents, analyze execution behavior, and support recovery processes.
They define how workload health, latency, errors, and recoverability are measured and controlled.

### Cloud Monitoring
Cloud Monitoring is a time-series telemetry and alerting service for cloud and application metrics.
It stores metric data and evaluates conditions for dashboards, SLOs, and incident generation.

- **Metrics Explorer** 
 Metrics Explorer is the interactive query interface for time-series metric data.
 It supports filtering, aggregation, alignment, and chart rendering over selected metrics.

- **Dashboards** 
 Dashboards are composed visual layouts of charts and status widgets.
 They present metric and incident state across one or more monitored systems.

- **Alerting policies** 
 Alerting policies are condition definitions that create incidents from metric or log evaluations.
 They compare incoming telemetry against thresholds, absence rules, or composite logic.

- **Notification channels** 
 Notification channels are configured delivery endpoints for incident events.
 They route alert notifications to email, messaging, webhook, or paging destinations.

- **Uptime checks** 
 Uptime checks are synthetic probes managed within Cloud Monitoring.
 They periodically test endpoint reachability and response validity from selected locations.

- **SLO/SLI** 
 SLO/SLI is the reliability measurement framework inside Cloud Monitoring.
 It computes service-level indicators and compares them to defined objective targets.

- **Custom metrics** 
 Custom metrics are user-defined time-series written into the monitoring backend.
 They represent application or domain-specific signals not emitted by default platform integrations.

### Managed Service for Prometheus
Managed Service for Prometheus is a managed metric ingestion path for Prometheus-format telemetry.
It accepts scraped metrics and makes them available in the Google Cloud monitoring backend.

- **Prometheus metric ingestion** 
 Prometheus metric ingestion is the collection and storage of Prometheus exposition-format samples.
 It preserves label-based metric structure for querying and alert evaluation.

- **Kubernetes metric collection** 
 Kubernetes metric collection is the scrape-based retrieval of metrics from cluster workloads and components.
 It gathers endpoint-exposed telemetry across pods, services, and cluster infrastructure.

### Cloud Logging
Cloud Logging is a managed log ingestion, storage, routing, and query system.
It centralizes structured and unstructured logs from services, resources, and applications.

- **Log Router** 
 Log Router is the rule-processing layer for incoming log entries.
 It applies routing logic to store, export, or exclude logs based on configured filters.

- **Log buckets** 
 Log buckets are storage containers for log entries within Cloud Logging.
 They define retention, location, and access boundaries for collected logs.

- **Log sinks** 
 Log sinks are export configurations from Cloud Logging to downstream destinations.
 They forward matching log entries to storage, analytics, or messaging systems.

- **Log-based metrics** 
 Log-based metrics are derived time-series generated from matching log patterns.
 They convert event frequency or extracted values into monitoring-compatible metric streams.

- **Centralized log search** 
 Centralized log search is the indexed query capability over aggregated log datasets.
 It filters and retrieves logs across many resources from one query interface.

### Error Reporting
Error Reporting is a service for aggregating and grouping application runtime errors.
It normalizes repeated failures into issue groups for operational analysis.

- **Exception aggregation** 
 Exception aggregation is the collection of repeated runtime failure events into one logical issue.
 It reduces duplicate error noise by consolidating similar stack traces and messages.

- **Error grouping** 
 Error grouping is the similarity classification process for incoming failure records.
 It identifies recurring error signatures and associates them with a common error group.

### Cloud Trace
Cloud Trace is a distributed tracing backend for request path and latency analysis.
It stores span data representing timing relationships across service boundaries.

- **Distributed tracing** 
 Distributed tracing is the end-to-end recording of request execution across multiple components.
 It models a request as linked spans with parent-child timing relationships.

- **Request latency analysis** 
 Request latency analysis is the measurement of time spent across spans in a trace.
 It attributes delay to specific services or operations along the request path.

### Cloud Profiler
Cloud Profiler is a continuous statistical profiling service for running applications.
It samples runtime behavior to identify CPU and memory hotspots in production systems.

- **CPU profiling** 
 CPU profiling is the periodic sampling of executing stack frames for processor usage analysis.
 It identifies code paths consuming a disproportionate share of compute time.

- **Heap profiling** 
 Heap profiling is the analysis of allocated and retained memory objects in a process.
 It highlights allocation growth and retention patterns that affect memory efficiency.

- **Performance bottleneck analysis** 
 Performance bottleneck analysis is the interpretation of sampled execution data to isolate inefficiencies.
 It correlates hotspots with latency, contention, or excessive resource consumption.

### Uptime Checks
Uptime Checks is a synthetic probing capability for endpoint availability verification.
It executes periodic network requests and records response state for monitoring evaluation.

- **Synthetic availability checks** 
 Synthetic availability checks are externally initiated probes that test whether a service responds correctly.
 They measure availability independently of internal application telemetry.

- **External endpoint validation** 
 External endpoint validation is the verification of public-facing endpoint reachability and correctness.
 It confirms response status, latency, and optionally expected content behavior.

### Backup and DR Service
Backup and DR Service is a managed control plane for backup policy and recovery orchestration.
It coordinates protection schedules and recovery operations for supported workloads.

- **Centralized backup orchestration** 
 Centralized backup orchestration is the policy-driven scheduling and coordination of backup operations.
 It applies consistent backup behavior across multiple protected resources from one control plane.

- **Recovery workflows** 
 Recovery workflows are controlled processes for restoring protected data or application state.
 They define the sequence and scope of recovery actions after data loss or service disruption.

---

## Storage and Stateful Services

These services provide object, file, block, cache, and database persistence models.
They define how state is stored, replicated, accessed, and recovered across workloads.

### Cloud Storage
Cloud Storage is an object storage service for unstructured data in buckets.
It stores data as objects addressed by name with durability, lifecycle, and access control features.

- **Standard** 
 Standard is the Cloud Storage class for frequently accessed objects.
 It provides low-latency retrieval with no retrieval-oriented cold access behavior.

- **Nearline** 
 Nearline is the Cloud Storage class for infrequently accessed objects.
 It lowers storage cost while retaining online access semantics.

- **Coldline** 
 Coldline is the Cloud Storage class for rarely accessed objects.
 It further reduces storage cost with access economics suited to low retrieval frequency.

- **Archive** 
 Archive is the Cloud Storage class for long-term retention with minimal access frequency.
 It provides the lowest storage cost among standard object storage classes.

- **Lifecycle rules** 
 Lifecycle rules are automated object management policies attached to a bucket.
 They transition or delete objects when age or other conditions are met.

- **Versioning** 
 Versioning is the bucket feature that retains previous object generations after overwrite or delete.
 It preserves historical object states as separate immutable generations.

- **Retention policies** 
 Retention policies are time-based controls preventing object deletion or replacement before expiry.
 They enforce minimum retention periods at the bucket level.

### Filestore
Filestore is a managed network file storage service exposing NFS shares.
It provides shared POSIX-style filesystem access to multiple clients over the network.

- **Managed NFS** 
 Managed NFS is the platform-operated network file protocol service presented by Filestore.
 It exports file shares without requiring user-managed file server infrastructure.

- **Shared file storage** 
 Shared file storage is the concurrent filesystem access model exposed to multiple clients.
 It allows several workloads to mount and operate on the same directory hierarchy.

### Persistent Disk
Persistent Disk is durable network block storage for compute instance attachment.
It presents volumes as persistent block devices independent of VM lifecycle.

- **Standard persistent disk** 
 Standard persistent disk is HDD-backed block storage for general or sequential workloads.
 It favors lower cost over low-latency random I/O performance.

- **SSD persistent disk** 
 SSD persistent disk is solid-state-backed block storage for higher performance workloads.
 It provides lower latency and higher IOPS than standard persistent disk.

- **Regional persistent disk** 
 Regional persistent disk is synchronously replicated block storage across two zones in one region.
 It increases availability for attached stateful workloads under zonal failure conditions.

### Hyperdisk
Hyperdisk is a configurable block storage family with decoupled capacity and performance settings.
It exposes storage profiles tuned for throughput or IOPS-intensive workloads.

- **Balanced** 
 Hyperdisk Balanced is the general-purpose profile within the Hyperdisk storage family.
 It balances performance characteristics for mixed read and write workloads.

- **Extreme** 
 Hyperdisk Extreme is the high-IOPS profile within the Hyperdisk storage family.
 It targets latency-sensitive workloads requiring very high transaction rates.

- **Throughput-optimized storage** 
 Throughput-optimized storage is the profile tuned for sustained large-block transfer performance.
 It is designed for workloads where bandwidth is more critical than IOPS density.

### Cloud SQL
Cloud SQL is a managed relational database service for common transactional database engines.
It automates instance management, patching, backup, and replication operations.

- **MySQL** 
 MySQL is the managed deployment option for the MySQL relational database engine.
 It exposes MySQL-compatible storage and query behavior under Cloud SQL control.

- **PostgreSQL** 
 PostgreSQL is the managed deployment option for the PostgreSQL relational database engine.
 It exposes PostgreSQL-compatible database behavior with managed operational lifecycle.

- **SQL Server** 
 SQL Server is the managed deployment option for the Microsoft SQL Server engine.
 It provides SQL Server database functionality under a Google-operated service model.

- **HA configuration** 
 HA configuration is the high-availability deployment mode for Cloud SQL instances.
 It uses redundant instance architecture to support failover during zonal disruption or maintenance.

- **Read replicas** 
 Read replicas are secondary database instances receiving asynchronous replication from a primary.
 They serve read traffic and can support disaster recovery or offloaded query workloads.

- **Backups** 
 Backups are database state captures retained for restore operations.
 They support point-in-time or snapshot-based recovery depending on engine capabilities.

### Spanner
Spanner is a distributed relational database with external consistency and horizontal scalability.
It combines SQL semantics with multi-node replication and distributed transaction support.

- **Regional instances** 
 Regional instances are Spanner deployments whose replicas are contained within one region.
 They optimize for in-region latency and availability characteristics.

- **Multi-region instances** 
 Multi-region instances are Spanner deployments replicated across several regions.
 They provide higher geographic resilience and wider read/write placement options.

- **Horizontal scaling** 
 Horizontal scaling is Spanner’s capacity expansion model through additional serving and storage resources.
 It increases throughput and dataset capacity without a single-node ceiling.

- **Strong consistency** 
 Strong consistency is the read model in which reads reflect the latest committed writes.
 It guarantees globally ordered transaction visibility within Spanner’s consistency model.

### Memorystore
Memorystore is a managed in-memory data service for cache-oriented engines.
It provides low-latency key-based access without self-managed cache infrastructure.

- **Redis** 
 Redis is the in-memory data structure engine offered through Memorystore.
 It supports key-value operations, expiry, and server-side data structure semantics.

- **Memcached** 
 Memcached is the in-memory cache engine offered through Memorystore.
 It provides simple distributed key-value caching without durable persistence features.

- **Managed caching** 
 Managed caching is the platform-operated cache lifecycle and availability model.
 It handles provisioning, maintenance, and service operations for in-memory cache instances.

### Bigtable
Bigtable is a distributed wide-column NoSQL database for sparse, high-volume datasets.
It stores data in row-key ordered tables designed for high-throughput low-latency access.

- **Wide-column storage** 
 Wide-column storage is a schema model where rows contain flexible columns grouped into families.
 It supports sparse datasets with timestamped cell versions and row-key locality.

- **Low-latency high-throughput serving** 
 Low-latency high-throughput serving is the access characteristic of Bigtable’s storage architecture.
 It supports large-scale read and write traffic with predictable performance for key-based access patterns.

---

## Resource Organization and Governance

These services organize cloud resources hierarchically and analyze inventory, cost, and policy state.
They define administrative boundaries, metadata, and optimization signals across the environment.

### Resource Manager
Resource Manager is the hierarchy service for organizing Google Cloud administrative scopes.
It defines parent-child structure for policy inheritance, billing grouping, and access control boundaries.

- **Organization** 
 Organization is the root administrative node for resources associated with an enterprise domain.
 It is the highest inheritance point for IAM and organizational policy.

- **Folders** 
 Folders are intermediate hierarchy nodes used to group projects logically.
 They provide delegated administration and inherited policy boundaries below the organization node.

- **Projects** 
 Projects are primary resource containers and API enablement boundaries in Google Cloud.
 They isolate billing, IAM policy, quotas, and deployed service resources.

### Cloud Asset Inventory
Cloud Asset Inventory is a service for enumerating and exporting resource metadata and history.
It captures asset configuration, policy state, and change records across scopes.

- **Asset inventory** 
 Asset inventory is the current-state catalog of supported resources and their metadata.
 It provides searchable visibility into deployed assets across projects and organizations.

- **Change history** 
 Change history is the recorded sequence of asset state transitions over time.
 It enables temporal analysis of configuration changes and governance drift.

- **Search and export** 
 Search and export is the retrieval capability for querying asset metadata and extracting results.
 It supports compliance review, reporting, and downstream data processing workflows.

### Recommender
Recommender is an analysis service that generates optimization suggestions from observed resource state.
It identifies opportunities for cost, security, and performance improvements.

- **Rightsizing recommendations** 
 Rightsizing recommendations are proposals to adjust resource sizing based on measured utilization.
 They target overprovisioned or underprovisioned compute and infrastructure resources.

- **IAM recommendations** 
 IAM recommendations are suggestions to reduce unnecessary permission grants.
 They analyze access usage and propose narrower role assignments where applicable.

- **Cost optimization insights** 
 Cost optimization insights are findings about spending inefficiencies in deployed resources.
 They highlight resources or configurations with reducible cost characteristics.

### Billing
Billing is the cost accounting system for metered Google Cloud resource usage.
It associates consumption with billing accounts and exposes budget and reporting controls.

- **Budgets** 
 Budgets are configured cost thresholds for selected billing scopes or filters.
 They trigger notifications when actual or forecasted spend approaches defined values.

- **Cost allocation** 
 Cost allocation is the attribution of cloud charges to projects, labels, or business dimensions.
 It structures usage cost for reporting, chargeback, or ownership tracking.

- **Spend tracking** 
 Spend tracking is the ongoing measurement of accrued charges over time.
 It provides visibility into current and historical consumption cost patterns.

### Quotas
Quotas are service-specific usage limits enforced on API calls and resource consumption.
They constrain allocation and request rates within a defined administrative scope.

- **Service quotas** 
 Service quotas are maximum allowed values for service operations or resource counts.
 They cap usage such as CPU allocation, API calls, or address consumption.

- **Capacity limits** 
 Capacity limits are enforced upper bounds on provisionable infrastructure or service resources.
 They affect scaling and creation attempts once the allowed threshold is reached.

### Labels and Tags
Labels and Tags are metadata systems for categorizing and selecting resources.
They provide machine-readable grouping inputs for policy, billing, and administration.

- **Resource classification** 
 Resource classification is the metadata-based grouping of resources by function or environment.
 It assigns structured identifiers for organization and filtering.

- **Ownership mapping** 
 Ownership mapping is the attachment of metadata indicating responsible team or business owner.
 It links resources to accountable operators or organizational units.

- **Policy targeting** 
 Policy targeting is the selection of resources for governance based on attached metadata.
 It scopes automation or enforcement using tag or label matches.

- **Cost allocation** 
 Cost allocation is the use of metadata to segment billing data by chosen dimensions.
 It improves financial reporting granularity across technical and organizational boundaries.

### Policy Intelligence
Policy Intelligence is an analytical capability for evaluating IAM exposure and permission scope.
It derives access insights from effective policy state and observed usage patterns.

- **Access analysis** 
 Access analysis is the evaluation of which principals can reach which resources and how.
 It resolves inherited roles and bindings into concrete access paths.

- **Least privilege insights** 
 Least privilege insights are findings indicating permissions broader than observed need.
 They support reduction of excess access by comparing granted permissions to usage evidence.

---

## Migration and Transfer

These services move applications, databases, and object data into Google Cloud.
They define migration execution models for compute modernization, database replication, and bulk transfer.

### Migrate to Containers
Migrate to Containers is a workload modernization service for converting VM applications into containers.
It analyzes VM-hosted applications and produces containerized deployment artifacts.

- **VM to container modernization** 
 VM to container modernization is the transformation of VM-based application packaging into container form.
 It extracts runnable application components and emits deployable container artifacts and manifests.

### Database Migration Service
Database Migration Service is a managed database migration control plane.
It orchestrates source-to-target database replication and cutover for supported engines.

- **Managed database migration** 
 Managed database migration is the platform-operated setup and execution of database transfer workflows.
 It configures replication connectivity and migration state without user-operated migration tooling.

- **Continuous replication** 
 Continuous replication is the ongoing transfer of source database changes to the target system.
 It keeps destination state near-current until cutover is performed.

### Storage Transfer Service
Storage Transfer Service is a managed bulk data movement service for files and objects.
It executes one-time or recurring transfer jobs between storage systems.

- **Bucket-to-bucket transfer** 
 Bucket-to-bucket transfer is the object copy process between storage buckets.
 It moves object data and metadata from one bucket source to another bucket destination.

- **On-prem to cloud transfer** 
 On-prem to cloud transfer is the ingestion of local or external file data into Google Cloud storage.
 It uses transfer jobs to move data from non-cloud sources into cloud object storage.

- **Scheduled data movement** 
 Scheduled data movement is the recurring execution model for transfer jobs.
 It repeats transfer operations at configured intervals to keep destinations synchronized.

---

## Data and ML Platform

These services provide analytical storage, distributed processing, and machine learning lifecycle management.
They define how datasets are transformed, models are trained, and predictions are served and monitored.

### Vertex AI
Vertex AI is a managed machine learning platform covering training, registry, serving, and monitoring.
It centralizes ML lifecycle resources under one control plane and execution model.

- **Workbench** 
 Workbench is the managed notebook environment for interactive ML development.
 It provides notebook-based access to code execution, data, and ML platform resources.

- **Datasets** 
 Datasets are managed resources representing training or evaluation data collections.
 They organize source data for downstream ML workflows and artifact lineage.

- **Training** 
 Training is the execution process that fits model parameters from input data.
 It runs managed or custom workloads to produce model artifacts.

- **Custom jobs** 
 Custom jobs are user-defined compute executions on Vertex AI infrastructure.
 They run arbitrary containers or code for training or data processing tasks.

- **Hyperparameter tuning** 
 Hyperparameter tuning is the coordinated execution of multiple training trials with varying parameters.
 It searches parameter combinations to optimize selected evaluation metrics.

- **Pipelines** 
 Pipelines are declarative DAG-based ML workflow definitions.
 They orchestrate dependent steps such as preprocessing, training, evaluation, and deployment.

- **Experiments** 
 Experiments are tracking constructs for runs, parameters, metrics, and outputs.
 They preserve comparative development history across iterative model work.

- **Metadata** 
 Metadata is the lineage and artifact relationship store for Vertex AI resources.
 It records how datasets, models, executions, and pipeline outputs are connected.

- **Model Registry** 
 Model Registry is the versioned storage system for managed model artifacts.
 It tracks model versions and associated metadata for controlled promotion and deployment.

- **Endpoints** 
 Endpoints are online serving resources for deployed machine learning models.
 They receive prediction requests and route them to attached model deployments.

- **Batch prediction** 
 Batch prediction is the asynchronous inference mode for large input datasets.
 It processes many records offline and writes prediction outputs to storage destinations.

- **Feature Store** 
 Feature Store is a managed repository for reusable machine learning feature values.
 It standardizes feature definitions and access patterns for training and serving workloads.

- **Model Monitoring** 
 Model Monitoring is the observation system for deployed model input and prediction behavior.
 It detects drift, skew, and anomalous production patterns against defined baselines.

### BigQuery
BigQuery is a serverless analytical data warehouse for large-scale SQL processing.
It separates storage from execution and runs distributed queries over structured datasets.

- **Data warehouse** 
 Data warehouse is the analytical storage model used for large-scale query processing.
 It organizes structured data for aggregation, reporting, and historical analysis.

- **SQL analytics** 
 SQL analytics is the execution of declarative SQL queries over warehouse data.
 It supports filtering, joining, aggregation, and transformation across large datasets.

- **Feature preparation** 
 Feature preparation is the transformation of raw analytical data into model input variables.
 It derives and materializes ML-ready attributes through query-based processing.

- **ML-adjacent analytics** 
 ML-adjacent analytics is analytical processing performed around machine learning workflows.
 It includes feature analysis, evaluation measurement, and data quality assessment tasks.

### Dataflow
Dataflow is a managed distributed data processing service based on the Apache Beam model.
It executes batch and streaming pipelines with autoscaled worker orchestration.

- **Batch pipelines** 
 Batch pipelines are bounded data processing graphs with finite input datasets.
 They run to completion and produce deterministic output from a fixed input set.

- **Streaming pipelines** 
 Streaming pipelines are unbounded data processing graphs for continuously arriving events.
 They maintain ongoing execution state and process records as they arrive.

- **Apache Beam execution** 
 Apache Beam execution is the runtime interpretation of Beam pipeline definitions on Dataflow workers.
 It maps Beam transforms into distributed execution stages managed by the service.

### Dataproc
Dataproc is a managed cluster service for Hadoop and Spark-based data processing.
It provisions analytics clusters with configurable software stacks and infrastructure profiles.

- **Spark** 
 Spark is a distributed compute engine for large-scale in-memory and batch data processing.
 It executes parallel transformations, SQL workloads, and iterative analytics over cluster resources.

- **Hadoop** 
 Hadoop is a distributed data processing ecosystem for cluster-based storage and computation.
 It provides batch-oriented processing components for large dataset workflows.

- **Distributed preprocessing** 
 Distributed preprocessing is the parallel transformation of raw data across multiple worker nodes.
 It prepares large datasets for analytics or machine learning when single-node processing is insufficient.
