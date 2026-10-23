# Azure Technical Notes

## Compute

Azure compute services provide virtual machines, scale-managed instance fleets, managed application runtimes, and batch execution backends.
They define how execution capacity is provisioned, isolated, placed, updated, and integrated with storage, identity, and networking.

### Azure Virtual Machines
Azure Virtual Machines is the IaaS virtual machine service for provisioning guest operating systems on Azure-managed hosts.
It exposes regional control-plane resources that create zonal or non-zonal VM instances with attached disks, NICs, identities, and availability settings.

- **Virtual machine instances** 
 Virtual machine instances are compute resources representing one guest OS deployment on Azure infrastructure.
 They run application processes with configured vCPU, memory, storage, network interfaces, and host placement attributes.

- **VM sizes** 
 VM sizes are predefined hardware profiles defining vCPU, memory, local temp storage, accelerator, and network characteristics.
 They determine the resource envelope and supported capabilities available to a VM instance.

- **Availability Sets** 
 Availability Sets are placement group resources that distribute VMs across fault and update domains within a datacenter.
 They reduce correlated downtime during host failures and planned platform maintenance for non-zonal deployments.

- **Availability Zones** 
 Availability Zones are physically separate datacenter locations within an Azure region.
 They provide zonal failure-domain isolation for zone-redundant architectures and zonal resource placement.

- **Dedicated Host** 
 Dedicated Host is the service for allocating a physical server to a single Azure subscription.
 It provides host-level isolation, placement visibility, and support for host-bound licensing requirements.

- **Spot VMs** 
 Spot VMs are Azure VM instances that consume surplus compute capacity at discounted pricing.
 They can be evicted by the platform when capacity is needed elsewhere or price conditions change.

- **Azure Compute Gallery** 
 Azure Compute Gallery is the image distribution service for VM and scale-set image versions.
 It stores gallery definitions, image definitions, and regional image replicas for controlled rollout.

- **Custom images** 
 Custom images are captured OS and software templates used to create new VMs.
 They preserve machine state as a reusable deployment baseline for repeated provisioning.

- **Boot diagnostics** 
 Boot diagnostics is the VM troubleshooting feature that captures boot logs and screenshots.
 It stores startup telemetry for failed or degraded instance initialization analysis.

- **Extensions** 
 Extensions are post-provisioning agents or scripts attached to a VM resource.
 They apply configuration, monitoring, security, or automation tasks after the VM is created.

- **Managed identities integration** 
 Managed identities integration is the identity binding model between a VM and Microsoft Entra-backed service principals.
 It provides Azure AD token-based access to Azure resources without embedded credentials.

### Virtual Machine Scale Sets
Virtual Machine Scale Sets is the managed fleet service for deploying and scaling homogeneous VM groups.
It coordinates instance placement, autoscaling, image rollout, health-driven replacement, and orchestration mode behavior.

- **Uniform orchestration** 
 Uniform orchestration is the scale-set mode where instances are treated as a homogeneous managed fleet.
 It applies one model definition across all instances and supports autoscale and rolling upgrades at fleet scope.

- **Flexible orchestration** 
 Flexible orchestration is the scale-set mode that allows more VM-level variation within a managed group.
 It supports broader instance individuality while still providing grouped lifecycle and placement semantics.

- **Autoscale** 
 Autoscale is the control mechanism that adjusts scale-set instance count from observed metrics or schedules.
 It adds or removes instances to keep utilization or capacity within configured bounds.

- **Rolling upgrades** 
 Rolling upgrades are staged replacement operations for instances in a scale set.
 They apply model changes incrementally while preserving fleet availability constraints.

- **Health probes integration** 
 Health probes integration is the linkage between scale-set lifecycle logic and load-balancer or application health signals.
 It drives repair and upgrade safety decisions based on backend serving state.

### Azure Batch
Azure Batch is the managed batch scheduling and execution service for parallel and high-throughput compute jobs.
It provisions pools of compute nodes and dispatches tasks according to job and task definitions.

- **Pools** 
 Pools are collections of compute nodes managed for Batch job execution.
 They define VM image, node count, autoscale policy, and task scheduling capacity.

- **Jobs** 
 Jobs are logical containers for related tasks in Azure Batch.
 They define scheduling and completion semantics for grouped work items.

- **Tasks** 
 Tasks are the executable work units scheduled onto Batch compute nodes.
 They run command lines or applications with defined resource files and environment settings.

- **Auto pools** 
 Auto pools are transient pools created automatically for a job.
 They provision per-job capacity and can be deleted when job execution completes.

### Azure App Service
Azure App Service is the managed PaaS runtime for web apps, APIs, and background application hosting.
It runs application code on worker instances managed by an App Service Plan and exposes HTTP-based endpoints.

- **Web Apps** 
 Web Apps are App Service resources for HTTP application hosting.
 They bind application code, runtime settings, deployment configuration, and an endpoint into one site resource.

- **App Service Plans** 
 App Service Plans are compute and pricing allocations for App Service workloads.
 They define worker SKU, instance count, scaling behavior, and the regional worker pool hosting one or more apps.

- **Deployment slots** 
 Deployment slots are alternate live environments attached to an App Service app.
 They support staged deployment and configuration swap semantics between slots and production.

- **Custom domains** 
 Custom domains are DNS hostnames mapped to an App Service application.
 They bind external names to the site endpoint and integrate with TLS certificate configuration.

- **Authentication and authorization** 
 Authentication and authorization is the built-in identity enforcement layer for App Service endpoints.
 It validates tokens or identity providers before requests reach application code when enabled.

- **Hybrid Connections** 
 Hybrid Connections is the outbound relay-based connectivity feature for App Service.
 It enables application access to selected private TCP endpoints without inbound private network integration.

---

## Containers and Orchestration

Azure container services provide image distribution, managed Kubernetes control planes, serverless container runtimes, and single-container execution.
They define container packaging, scheduling, node lifecycle, identity mapping, and traffic exposure for containerized workloads.

### Azure Kubernetes Service
Azure Kubernetes Service is the managed Kubernetes control plane service for Azure.
It runs the Kubernetes API server and integrates cluster operations with Azure networking, identity, storage, and load balancing services.

- **Clusters** 
 Clusters are regional AKS control-plane deployments with attached node capacity.
 They host Kubernetes API state and coordinate scheduling onto VM-backed or serverless execution backends.

- **Node pools** 
 Node pools are groups of worker nodes sharing a common VM size and configuration profile.
 They separate workload placement by OS, architecture, scaling policy, or hardware requirements.

- **System node pools** 
 System node pools are AKS node pools intended for cluster-critical add-ons and control components.
 They must satisfy minimum sizing and availability constraints required by cluster infrastructure workloads.

- **User node pools** 
 User node pools are AKS node pools intended for application workloads.
 They isolate tenant or workload placement from system-critical cluster services.

- **Virtual nodes** 
 Virtual nodes are the AKS integration that schedules pods onto Azure Container Instances.
 They provide rapid serverless pod execution without provisioning additional VM worker nodes.

- **Managed identities integration** 
 Managed identities integration is the identity model that binds AKS components or workloads to Entra-backed identities.
 It enables Azure API access without static secrets on nodes or inside pods.

- **Microsoft Entra integration** 
 Microsoft Entra integration is the cluster authentication model for AKS API access.
 It delegates user authentication to Entra ID and maps authorized identities into Kubernetes RBAC flows.

- **Ingress controller integration** 
 Ingress controller integration is the reconciliation path from Kubernetes ingress or gateway resources to Azure traffic services.
 It maps cluster traffic objects to Application Gateway, load balancer, or other ingress data-plane implementations.

- **Cluster autoscaler** 
 Cluster autoscaler is the control loop that changes node-pool size based on unschedulable pod demand.
 It adds or removes nodes to align cluster capacity with workload resource requests.

- **Upgrade channels** 
 Upgrade channels are AKS-managed release cadence tracks for Kubernetes version updates.
 They control when platform-driven upgrades are applied to supported cluster components.

### Azure Container Registry
Azure Container Registry is the managed OCI-compatible registry service for container images and artifacts.
It stores repositories, manifests, layers, and tags with Azure RBAC, networking, and geo-replication controls.

- **Repositories** 
 Repositories are named namespaces for related image versions and tags.
 They group manifests and layers under one logical path within a registry.

- **Tags** 
 Tags are mutable name references to image manifests.
 They provide human-readable version aliases that can be reassigned to different digests.

- **Manifests and digests** 
 Manifests and digests are immutable references to image content and metadata.
 They provide content-addressable identity for deployment and integrity verification.

- **Tasks** 
 Tasks are registry-native build and automation workflows in Azure Container Registry.
 They build, patch, import, or trigger artifact operations in response to source or base-image events.

- **Geo-replication** 
 Geo-replication is the regional replica model for Premium-tier registries.
 It replicates registry data to multiple Azure regions for locality and resilience.

- **Content trust and policies** 
 Content trust and policies are registry controls for image integrity and retention governance.
 They constrain artifact lifecycle or verification behavior through configured registry settings.

### Azure Container Apps
Azure Container Apps is the managed serverless container application platform built for microservices and event-driven workloads.
It runs containers on a managed environment with revision-based deployment, autoscaling, and ingress controls.

- **Managed environments** 
 Managed environments are the top-level network and execution boundaries for Container Apps.
 They host one or more container apps and define shared networking, logging, and Dapr integration scope.

- **Container apps** 
 Container apps are the deployable workload resources in the service.
 They bind container images, scale rules, revision settings, and ingress configuration into one application unit.

- **Revisions** 
 Revisions are immutable deployment snapshots of a container app configuration.
 They preserve runtime settings and can receive traffic according to traffic-splitting rules.

- **Scale rules** 
 Scale rules are autoscaling triggers based on HTTP concurrency, event sources, or custom metrics.
 They change the active replica count according to workload demand and minimum or maximum replica bounds.

- **Ingress** 
 Ingress is the HTTP or TCP exposure model for a container app.
 It controls external or internal endpoint publication, traffic routing, and revision exposure behavior.

- **Dapr integration** 
 Dapr integration is the sidecar-based application runtime option for supported Container Apps environments.
 It provides service invocation, pub/sub, state, and secret abstractions through a managed sidecar model.

### Azure Container Instances
Azure Container Instances is the single-container or container-group execution service for fast isolated runtime provisioning.
It launches containers directly without VM or orchestrator management by the customer.

- **Container groups** 
 Container groups are the scheduling and lifecycle units in Azure Container Instances.
 They run one or more co-scheduled containers sharing network namespace, storage mounts, and lifecycle state.

- **VNet integration** 
 VNet integration is the private network placement model for container groups.
 It attaches container execution to a delegated subnet for private addressing and east-west communication.

---

## Serverless and Integration

Azure serverless and integration services provide function execution, workflow orchestration, event routing, durable messaging, and API front doors.
They define asynchronous communication, event propagation, request mediation, and stateful workflow execution.

### Azure Functions
Azure Functions is the function-as-a-service runtime for event-driven and HTTP-triggered code.
It executes functions on demand under a hosting plan with trigger, binding, and scaling semantics.

- **Function apps** 
 Function apps are the deployment and configuration containers for one or more Azure Functions.
 They define hosting plan, runtime settings, identity, storage linkage, and application-level configuration.

- **Triggers** 
 Triggers are event source bindings that start function execution.
 They determine the invocation source and payload model for each function.

- **Bindings** 
 Bindings are declarative input and output connectors for Azure Functions.
 They map supported services into function parameters or outputs without explicit connector boilerplate in the code path.

- **Consumption plan** 
 The Consumption plan is the event-driven serverless hosting mode for Functions.
 It allocates compute on demand and scales automatically with execution volume.

- **Premium plan** 
 The Premium plan is the always-available elastic hosting mode for Functions.
 It supports pre-warmed instances, VNet integration, and reduced cold-start behavior.

- **Dedicated plan** 
 The Dedicated plan is the App Service Plan-based hosting mode for Functions.
 It runs functions on reserved App Service worker capacity rather than demand-allocated serverless capacity.

- **Durable Functions** 
 Durable Functions is the stateful workflow extension for Azure Functions.
 It coordinates orchestration, fan-out, checkpoints, and long-running state through event-sourced execution semantics.

### Azure Logic Apps
Azure Logic Apps is the managed workflow orchestration service for connectors, triggers, and declarative process automation.
It executes workflows through event-driven or schedule-driven triggers with connector-backed action graphs.

- **Consumption** 
 Consumption is the multi-tenant serverless hosting model for Logic Apps.
 It scales per action execution and uses shared service infrastructure with usage-based billing.

- **Standard** 
 Standard is the single-tenant hosting model for Logic Apps.
 It runs workflows on dedicated worker resources with local state, VNet integration, and hosting-plan semantics.

- **Triggers** 
 Triggers are workflow entry points in Logic Apps.
 They start execution from schedules, events, HTTP requests, or connector-originated changes.

- **Actions** 
 Actions are executable workflow steps that perform operations or data transformation.
 They run sequentially or in parallel according to the workflow graph and dependency model.

- **Connectors** 
 Connectors are managed integration adapters to external or Azure services.
 They expose authenticated operations and event hooks as workflow actions and triggers.

- **Integration accounts** 
 Integration accounts are B2B and enterprise integration metadata stores used with certain Logic Apps workflows.
 They hold schemas, maps, agreements, and trading-partner artifacts for structured message processing.

### Azure Event Grid
Azure Event Grid is the event routing service for discrete event publication and push delivery.
It distributes events from publishers to subscribed handlers using topic and subscription resources.

- **System topics** 
 System topics are Event Grid topic resources automatically associated with supported Azure services.
 They expose service-originated events without requiring a custom publisher endpoint.

- **Custom topics** 
 Custom topics are publisher-owned ingress resources for application-defined events.
 They receive event batches through the Event Grid publishing API and route them to subscribers.

- **Partner topics** 
 Partner topics are topic resources used for third-party SaaS event publication into Azure.
 They define the event ingress boundary for approved partner-originated events.

- **Event subscriptions** 
 Event subscriptions are routing bindings from a topic to one or more handlers.
 They filter and deliver matching events to supported destination endpoints.

- **Dead-lettering** 
 Dead-lettering is the failed-delivery retention mechanism for undeliverable Event Grid events.
 It stores events in a configured storage destination after delivery retry exhaustion.

### Azure Event Hubs
Azure Event Hubs is the high-throughput event ingestion and streaming service for telemetry and log pipelines.
It stores ordered event streams in partitions for producer-consumer processing at scale.

- **Namespaces** 
 Namespaces are regional management containers for Event Hubs resources.
 They define network, authentication, capacity, and feature boundaries for contained hubs.

- **Event hubs** 
 Event hubs are append-only stream resources inside a namespace.
 They accept producer events and retain them for time-based or size-based consumer replay.

- **Partitions** 
 Partitions are ordered append logs within an event hub.
 They determine parallelism, scaling boundaries, and consumer checkpoint isolation.

- **Consumer groups** 
 Consumer groups are independent read positions over the same event stream.
 They allow multiple applications to consume one event hub without sharing offset state.

- **Capture** 
 Capture is the managed export feature for Event Hubs streams.
 It writes event data to Azure Storage or Data Lake on configured window or size boundaries.

- **Dedicated clusters** 
 Dedicated clusters are isolated Event Hubs capacity deployments.
 They provide reserved throughput and infrastructure isolation for high-volume workloads.

### Azure Service Bus
Azure Service Bus is the durable messaging service for queues, publish-subscribe topics, and ordered sessions.
It provides brokered message storage with delivery guarantees, transactions, and dead-lettering semantics.

- **Namespaces** 
 Namespaces are the top-level regional containers for Service Bus entities.
 They define network, authentication, and SKU boundaries for queues and topics.

- **Queues** 
 Queues are point-to-point messaging entities in Service Bus.
 They store messages durably until a receiver processes and completes them.

- **Topics** 
 Topics are publish-subscribe messaging entities in Service Bus.
 They fan out messages to multiple subscriptions with independent delivery state.

- **Subscriptions** 
 Subscriptions are durable receivers attached to a Service Bus topic.
 They maintain independent message copies, filters, and dead-letter state.

- **Sessions** 
 Sessions are ordered message-grouping constructs for related messages.
 They provide FIFO-like processing semantics within a session identifier scope.

- **Dead-letter queues** 
 Dead-letter queues are subqueues for messages that cannot be processed successfully.
 They retain poison or expired messages for later inspection or replay.

- **Geo-disaster recovery** 
 Geo-disaster recovery is the metadata failover mechanism for Service Bus namespaces.
 It replicates namespace configuration aliases across regions without replicating message payload state.

### Azure API Management
Azure API Management is the managed API gateway and API governance service for HTTP-based interfaces.
It mediates inbound requests through policies, security controls, transformation, routing, and analytics.

- **API gateways** 
 API gateways are the data-plane endpoints that receive and process API requests.
 They apply configured policies and route traffic to backend services according to API definitions.

- **APIs** 
 APIs are managed interface definitions containing operations, schemas, and policies.
 They describe the request surface published through API Management.

- **Products** 
 Products are packaging and access-control groupings for one or more APIs.
 They define subscription requirements and expose APIs to developer consumers under a shared policy scope.

- **Policies** 
 Policies are XML-defined request and response processing rules in API Management.
 They transform payloads, enforce access logic, inject headers, limit rates, and route traffic.

- **Named values** 
 Named values are reusable configuration variables stored in API Management.
 They parameterize policies and can reference secrets from Azure Key Vault.

- **Revisions and versions** 
 Revisions and versions are change-management constructs for API definitions.
 They separate non-breaking updates within one API identity from intentionally distinct API contract lines.

---

## Networking and Traffic Management

Azure networking services define private IP domains, packet routing, load balancing, edge delivery, DNS resolution, and private service access.
They separate control-plane network configuration from data-plane packet and request forwarding across regional and global scopes.

### Azure Virtual Network
Azure Virtual Network is the software-defined private networking service for Azure resources.
It provides address spaces, subnet segmentation, route evaluation, and security policy attachment for deployed workloads.

- **Address spaces** 
 Address spaces are CIDR ranges assigned to a virtual network.
 They define the private IP pool from which subnet prefixes are allocated.

- **Subnets** 
 Subnets are segmented IP ranges within a virtual network.
 They define placement boundaries for resources and attach route tables, NSGs, delegations, and service endpoint settings.

- **Network Security Groups** 
 Network Security Groups are stateful packet-filtering policy resources associated with NICs or subnets.
 They process ordered allow and deny rules for inbound and outbound traffic flows.

- **Application Security Groups** 
 Application Security Groups are logical workload groupings used in NSG rule targeting.
 They decouple security policy rules from static IP-based membership.

- **User-defined routes** 
 User-defined routes are custom route-table entries attached to subnets.
 They override default system routing to direct traffic toward virtual appliances, gateways, or other next hops.

- **Subnet delegations** 
 Subnet delegations are subnet configuration bindings that reserve management rights for specific Azure services.
 They allow a delegated service to place service-managed resources into that subnet.

- **Service tags** 
 Service tags are Microsoft-managed address group abstractions used in security rules.
 They represent dynamic IP ranges for Azure services and reduce manual rule maintenance.

### Azure Load Balancer
Azure Load Balancer is the Layer 4 traffic distribution service for TCP and UDP workloads.
It maps frontend IP configurations to backend pools using hash-based distribution and health-probe gating.

- **Public Load Balancer** 
 Public Load Balancer is the internet-facing deployment mode for Azure Load Balancer.
 It exposes a public frontend IP and routes external traffic to backend resources.

- **Internal Load Balancer** 
 Internal Load Balancer is the private deployment mode for Azure Load Balancer.
 It exposes a private frontend IP within a virtual network and serves internal consumers.

- **Backend pools** 
 Backend pools are collections of load-balanced resource endpoints.
 They define which NICs or IP configurations can receive traffic for a frontend rule.

- **Health probes** 
 Health probes are endpoint checks used to determine backend availability.
 They remove failed backends from traffic distribution until the probe succeeds again.

- **Load-balancing rules** 
 Load-balancing rules bind frontends, backend pools, probes, and protocol or port mappings.
 They define how inbound traffic is translated and distributed to backends.

- **Inbound NAT rules** 
 Inbound NAT rules are one-to-one port-forwarding mappings on a load balancer frontend.
 They target individual backend instances rather than a shared distribution pool.

### Azure Application Gateway
Azure Application Gateway is the Layer 7 regional HTTP and HTTPS load balancer for web workloads.
It inspects application requests and routes them using listener, rule, and backend settings with optional WAF enforcement.

- **Listeners** 
 Listeners are frontend protocol and port bindings on an Application Gateway.
 They terminate client HTTP or HTTPS connections and associate incoming traffic with routing rules.

- **Backend pools** 
 Backend pools are the destination endpoint groups for routed web traffic.
 They contain IPs, NICs, VMSS instances, or FQDNs that receive proxied requests.

- **HTTP settings** 
 HTTP settings are backend connection profiles for routed traffic.
 They define protocol, port, affinity, timeout, and probe associations for backend communication.

- **Routing rules** 
 Routing rules connect listeners to backend pools and HTTP settings.
 They implement basic or path-based request routing semantics.

- **Web Application Firewall mode** 
 Web Application Firewall mode is the request inspection and filtering capability integrated into Application Gateway.
 It evaluates HTTP traffic against managed or custom rules before forwarding requests to backends.

- **Rewrite rules** 
 Rewrite rules are request and response modification policies at the gateway layer.
 They change headers, URLs, or query strings during request processing.

### Azure Front Door
Azure Front Door is the global edge entry service for HTTP and HTTPS traffic.
It provides anycast ingress, request acceleration, edge routing, WAF integration, and global backend selection.

- **Frontend endpoints** 
 Frontend endpoints are the externally reachable hostnames and TLS bindings exposed by Front Door.
 They terminate client requests at the Microsoft edge.

- **Origin groups** 
 Origin groups are backend collections associated with a route.
 They define health probing, load balancing, and failover behavior across origins.

- **Origins** 
 Origins are the backend application endpoints served through Front Door.
 They can be public or private endpoints depending on Front Door and Private Link configuration.

- **Routes** 
 Routes are host and path-based mappings from a frontend endpoint to an origin group.
 They define forwarding protocol, caching, and rule-set behavior for matching requests.

- **Rule sets** 
 Rule sets are edge request-processing policies attached to Front Door routes.
 They modify headers, URL behavior, redirects, or forwarding attributes before backend delivery.

- **WAF integration** 
 WAF integration is the global HTTP request inspection feature attached to Front Door.
 It evaluates requests at the edge before origin routing occurs.

### Azure Firewall
Azure Firewall is the managed stateful network security service for centralized packet and application-layer filtering.
It enforces allow and deny logic for north-south and east-west traffic using a regional data-plane deployment.

- **Firewall policy** 
 Firewall policy is the reusable rule container and control-plane object for Azure Firewall.
 It centralizes rule collections, DNS settings, TLS inspection settings, and threat-intelligence behavior.

- **Application rules** 
 Application rules are Layer 7 filtering rules for outbound web traffic.
 They allow or deny traffic based on FQDN, protocol, and destination semantics.

- **Network rules** 
 Network rules are Layer 3 and 4 filtering rules for IP and port-based traffic.
 They control packet flows by source, destination, protocol, and port.

- **DNAT rules** 
 DNAT rules are inbound translation rules on Azure Firewall.
 They map public destination addresses and ports to private internal endpoints.

- **Threat intelligence mode** 
 Threat intelligence mode is the threat feed-based filtering behavior of Azure Firewall.
 It alerts on or blocks traffic associated with known malicious indicators.

### Azure NAT Gateway
Azure NAT Gateway is the managed outbound-only source NAT service for subnet egress.
It translates private source addresses to public IP resources for internet-bound connections.

- **Subnet association** 
 Subnet association is the binding between a NAT Gateway and one or more subnets.
 It causes new outbound flows from those subnets to use the gateway for internet egress.

- **Public IP resources** 
 Public IP resources are the frontend addresses used by NAT Gateway for source translation.
 They provide the externally visible address pool used for outbound connections.

### Azure VPN Gateway
Azure VPN Gateway is the managed encrypted tunnel service for virtual network connectivity.
It establishes IPsec or OpenVPN-based tunnels between Azure VNets, branch devices, and clients.

- **Site-to-site VPN** 
 Site-to-site VPN is the IPsec tunnel model between an on-premises or external network and Azure.
 It exchanges routed traffic through a gateway connection and local network definition.

- **Point-to-site VPN** 
 Point-to-site VPN is the client-access remote connectivity model for Azure VNets.
 It connects individual devices to Azure through VPN client software and gateway authentication.

- **VNet-to-VNet VPN** 
 VNet-to-VNet VPN is the encrypted tunnel model between Azure virtual networks.
 It connects VNets through gateway resources using managed VPN transport.

- **Gateway SKUs** 
 Gateway SKUs are capacity and feature profiles for VPN Gateway resources.
 They determine throughput, connection limits, zone support, and feature availability.

### Azure ExpressRoute
Azure ExpressRoute is the private dedicated connectivity service between Azure and external networks.
It bypasses the public internet data path and carries traffic over provider or direct private circuits.

- **Circuits** 
 Circuits are the logical ExpressRoute connectivity resources ordered for a peering location.
 They define bandwidth, SKU, and authorization for private connectivity.

- **Private peering** 
 Private peering is the routing domain used for private IP connectivity to Azure VNets and services.
 It exchanges routes through BGP over an ExpressRoute circuit.

- **Microsoft peering** 
 Microsoft peering is the routing domain used for selected Microsoft public services over ExpressRoute.
 It advertises service prefixes subject to supported service exposure rules.

- **ExpressRoute Global Reach** 
 ExpressRoute Global Reach is the route exchange feature between on-premises sites connected to ExpressRoute.
 It enables private transit of traffic between customer edge locations through Microsoft’s backbone.

### Azure Private Link
Azure Private Link is the private service access framework for reaching Azure or customer services through private IPs.
It maps service consumption to interface-like endpoints placed inside consumer virtual networks.

- **Private endpoints** 
 Private endpoints are NIC-backed private IP endpoints for a specific service resource.
 They provide private data-plane reachability from a VNet to the target service over Microsoft’s backbone.

- **Private link services** 
 Private link services are producer-side resources exposing a service behind a standard load balancer.
 They allow approved consumers to connect to a privately published service through private endpoints.

- **Private DNS integration** 
 Private DNS integration is the name-resolution model used with private endpoints.
 It maps service FQDNs to private IP addresses inside consumer network scopes.

### Service Endpoints
Service Endpoints is the VNet-to-service identity extension for selected Azure PaaS services.
It keeps service traffic on Azure’s backbone while preserving public service endpoints rather than private IP endpoints.

- **Subnet enablement** 
 Subnet enablement is the per-subnet configuration that activates service endpoint identity for a service type.
 It extends the VNet’s identity to requests from that subnet toward the selected service.

- **Service endpoint policies** 
 Service endpoint policies are restrictions applied to service-endpoint-enabled traffic.
 They narrow which service instances can be reached from a subnet using endpoint identity.

### Azure Traffic Manager
Azure Traffic Manager is the DNS-based global traffic distribution service.
It answers DNS queries with endpoint selection based on configured routing methods and health state.

- **Profiles** 
 Profiles are the top-level Traffic Manager resources defining routing behavior.
 They contain endpoints, health-check configuration, and routing method settings.

- **Endpoints** 
 Endpoints are the destination targets that Traffic Manager can return in DNS responses.
 They represent Azure, external, or nested profile targets under one profile.

- **Routing methods** 
 Routing methods are the DNS answer selection algorithms used by Traffic Manager.
 They include priority, weighted, performance, geographic, subnet, and multivalue routing logic.

### Azure DNS
Azure DNS is the authoritative DNS hosting service for public and private zones.
It stores zone records and answers queries for delegated domains under Azure-managed name servers.

- **Public DNS zones** 
 Public DNS zones are internet-resolvable authoritative zone resources.
 They serve records for publicly delegated namespaces.

- **Private DNS zones** 
 Private DNS zones are VNet-linked authoritative zone resources for internal resolution.
 They answer queries only within linked virtual network scopes.

- **Record sets** 
 Record sets are DNS entries stored within a zone.
 They map names to addresses, aliases, text, mail routing, or other DNS record values.

### Azure DDoS Protection
Azure DDoS Protection is the volumetric attack mitigation service for public Azure endpoints.
It monitors and automatically mitigates L3 and L4 attacks against protected public IP resources.

- **DDoS Network Protection** 
 DDoS Network Protection is the paid protection plan for virtual network-scoped resources.
 It applies enhanced telemetry, mitigation, and cost-protection features to covered public IPs.

- **Telemetry and mitigation** 
 Telemetry and mitigation is the attack-detection and response path in the service.
 It identifies abnormal traffic patterns and triggers automatic scrubbing when thresholds are exceeded.

---

## Identity, Security, and Governance

Azure identity and governance services define principals, authorization, policy enforcement, key custody, posture management, and security operations.
They govern authentication, access evaluation, data protection, compliance controls, and threat detection across subscriptions and tenants.

### Microsoft Entra ID
Microsoft Entra ID is Azure’s cloud identity directory and authentication service.
It stores users, groups, service principals, and application objects and issues tokens for resource access.

- **Users** 
 Users are directory identities representing human accounts.
 They authenticate through supported methods and receive tokens for authorized applications and resources.

- **Groups** 
 Groups are directory objects used for membership-based authorization and assignment.
 They aggregate principals for role assignment, application access, and administrative scoping.

- **Enterprise applications** 
 Enterprise applications are service principal instances representing applications in a tenant.
 They define local access settings, consent, and token issuance behavior for integrated apps.

- **App registrations** 
 App registrations are application identity definitions in Entra ID.
 They describe application metadata, redirect URIs, credentials, and permission requirements.

- **Service principals** 
 Service principals are tenant-local security identities for applications or automation.
 They receive role assignments and authenticate to Azure or other protected resources.

- **Conditional Access** 
 Conditional Access is the policy engine for access control based on context.
 It evaluates signals such as user, app, device, location, or risk before granting access.

- **Privileged Identity Management** 
 Privileged Identity Management is the just-in-time elevation and role governance service for privileged assignments.
 It activates roles temporarily and enforces approval, MFA, and audit workflows.

### Azure RBAC
Azure RBAC is the authorization model for Azure Resource Manager resource access.
It evaluates role assignments, deny assignments, conditions, and scope inheritance to determine allowed actions.

- **Role definitions** 
 Role definitions are permission bundles composed of allowed actions and data actions.
 They describe what operations can be performed at management-plane or data-plane scope.

- **Role assignments** 
 Role assignments bind a principal to a role definition at a given scope.
 They inherit downward from management group, subscription, resource group, or resource scope.

- **Deny assignments** 
 Deny assignments are explicit authorization blocks evaluated before allows.
 They prevent specified actions even when a role assignment would otherwise permit them.

- **Scopes** 
 Scopes are the hierarchy levels at which RBAC assignments apply.
 They define inheritance boundaries and the resource set affected by a role assignment.

### Managed Identities
Managed Identities is the credentialless identity feature for Azure resources.
It creates Entra-backed service principals tied to resource lifecycle or independently managed identities.

- **System-assigned** 
 System-assigned managed identities are identities bound to one Azure resource lifecycle.
 They are created and deleted with the parent resource automatically.

- **User-assigned** 
 User-assigned managed identities are standalone identity resources reusable across multiple Azure resources.
 They decouple identity lifecycle from any single compute or application resource.

### Azure Key Vault
Azure Key Vault is the managed secrets, keys, and certificates storage service.
It exposes a control plane through ARM and a data plane for secret and cryptographic operations.

- **Secrets** 
 Secrets are versioned stored values such as passwords, tokens, or connection strings.
 They are retrieved through the Key Vault data plane with identity-based authorization.

- **Keys** 
 Keys are cryptographic key objects used for wrap, unwrap, sign, verify, encrypt, or decrypt operations.
 They maintain versioned material and operation policies within a vault.

- **Certificates** 
 Certificates are managed X.509 objects stored in Key Vault.
 They integrate certificate metadata, secret material, and optional policy-driven renewal lifecycle.

- **Access policies** 
 Access policies are legacy data-plane authorization entries on a Key Vault.
 They grant vault operation permissions directly to principals independent of Azure RBAC.

- **RBAC authorization mode** 
 RBAC authorization mode is the Key Vault access model that delegates data-plane authorization to Azure RBAC.
 It aligns vault permissions with role-assignment-based governance instead of vault-local access policies.

- **Private endpoint integration** 
 Private endpoint integration is the private network access model for Key Vault data-plane traffic.
 It exposes the service through a VNet-local private IP rather than a public endpoint.

### Managed HSM
Managed HSM is the dedicated hardware-backed key management service for high-assurance cryptographic workloads.
It provides HSM-isolated key storage and cryptographic operations through a managed cluster service.

- **HSM pools** 
 HSM pools are the top-level Managed HSM deployment resources.
 They define the security boundary, region, and administrative control scope for contained keys.

- **Role separation** 
 Role separation is the built-in administrative model for Managed HSM operations.
 It separates management, crypto usage, and security administration duties across distinct role paths.

### Azure Policy
Azure Policy is the governance engine for enforcing and auditing Azure resource configuration.
It evaluates resource state against policy definitions during deployment and through periodic compliance scans.

- **Policy definitions** 
 Policy definitions are rule objects expressing allowed or denied resource configuration logic.
 They define evaluation conditions and effects such as deny, audit, modify, or deployIfNotExists.

- **Initiatives** 
 Initiatives are grouped collections of policy definitions under one assignment boundary.
 They package multiple controls into a single compliance set.

- **Assignments** 
 Assignments bind a policy definition or initiative to a scope.
 They activate evaluation and effect enforcement for resources within that scope.

- **Exemptions** 
 Exemptions are scoped exceptions to policy assignments.
 They suppress compliance violations for justified resources without removing the governing assignment.

- **Remediation tasks** 
 Remediation tasks are follow-up operations that bring noncompliant resources into policy-conforming state.
 They execute modify or deployIfNotExists effects against existing resources.

### Microsoft Defender for Cloud
Microsoft Defender for Cloud is the cloud security posture management and workload protection platform for Azure and connected environments.
It assesses resource configuration, recommends hardening actions, and emits workload threat detections.

- **Secure score** 
 Secure score is the aggregate posture metric produced by Defender for Cloud.
 It summarizes control implementation state across assessed resources and recommendations.

- **Recommendations** 
 Recommendations are control findings about security misconfiguration or missing protections.
 They identify remediations that improve posture and reduce resource exposure.

- **Defender plans** 
 Defender plans are workload-specific protection packages within Defender for Cloud.
 They enable detection and sensor-based analysis for resource types such as servers, SQL, or containers.

### Microsoft Sentinel
Microsoft Sentinel is the cloud-native SIEM and SOAR platform built on Log Analytics.
It ingests security telemetry, correlates analytics, generates incidents, and drives automated response workflows.

- **Workspaces** 
 Workspaces are the Log Analytics backends used by Sentinel.
 They store ingested security events, query results, and analytic state.

- **Analytics rules** 
 Analytics rules are detection definitions that create incidents from matched telemetry.
 They run scheduled, near-real-time, or fusion-based logic across workspace data.

- **Incidents** 
 Incidents are investigation containers created from one or more alerts.
 They group findings, entities, severity, and response state for security operations.

- **Playbooks** 
 Playbooks are Logic App-based automation workflows triggered by Sentinel events.
 They perform enrichment, notification, containment, or remediation actions from security detections.

### Azure Resource Manager
Azure Resource Manager is the management-plane deployment and resource orchestration layer for Azure.
It processes ARM and Bicep deployments, enforces RBAC and policy, and exposes resource provider APIs.

- **Resource providers** 
 Resource providers are service-specific management endpoints registered in a subscription.
 They expose resource types and operations under a provider namespace such as Microsoft.Compute or Microsoft.Network.

- **ARM templates** 
 ARM templates are JSON declarative deployment documents for Azure resources.
 They describe desired resource state, dependencies, parameters, and outputs for idempotent deployment execution.

- **Bicep** 
 Bicep is the domain-specific language that compiles to ARM templates.
 It provides a higher-level syntax for defining Azure resource deployments declaratively.

### Management Groups
Management Groups are tenant-level hierarchy containers above subscriptions in Azure.
They provide a scope for RBAC, Policy, and governance inheritance across multiple subscriptions.

- **Hierarchy** 
 Hierarchy is the parent-child tree structure formed by management groups and subscriptions.
 It determines policy and role assignment inheritance for descendant scopes.

- **Root management group** 
 The root management group is the top-level governance scope in an Entra tenant.
 It is the ultimate inheritance point for management-group-scoped policies and assignments.

### Subscriptions
Subscriptions are billing and administrative boundaries for Azure resources.
They scope resource-provider registration, quotas, RBAC inheritance, and service consumption.

- **Resource providers registration** 
 Resource provider registration is the subscription-level activation of a service namespace.
 It enables the subscription to create and manage resource types under that provider.

- **Quotas** 
 Quotas are subscription or region-scoped limits on resource counts or capacity.
 They constrain allocation and API operations until increased or released.

### Resource Groups
Resource Groups are logical resource containers within a subscription.
They define a management scope for RBAC, locks, tags, and lifecycle grouping but not network or performance isolation.

- **Locks** 
 Locks are management-plane protections against accidental deletion or modification.
 They enforce read-only or delete-block semantics on scoped resources.

- **Tags** 
 Tags are key-value metadata attached to Azure resources and scopes.
 They support organization, automation targeting, and cost allocation workflows.

---

## Storage

Azure storage services provide object, file, queue, table, and hierarchical namespace storage models.
They define access semantics, redundancy, lifecycle policies, performance tiers, and protocol exposure for persistent data.

### Azure Storage Accounts
Azure Storage Accounts are the top-level management resources for Azure Storage services.
They define region, redundancy model, performance tier, namespace features, authentication settings, and contained service endpoints.

- **General-purpose v2 accounts** 
 General-purpose v2 accounts are the standard Storage Account type for Blob, Files, Queues, and Tables.
 They support the broadest feature set including access tiers, lifecycle rules, and Data Lake namespace options.

- **Premium accounts** 
 Premium accounts are storage accounts backed by SSD-oriented performance profiles for selected services.
 They trade higher cost for lower latency and more predictable throughput behavior.

- **Redundancy options** 
 Redundancy options are the storage replication models for account data durability.
 They include local, zone-redundant, geo-redundant, and read-access geo-redundant replication semantics.

- **Shared access signatures** 
 Shared access signatures are signed delegated access tokens for storage operations.
 They grant time-limited permissions to clients without exposing the account key.

- **Private endpoints** 
 Private endpoints are private IP network interfaces for Storage Account service endpoints.
 They provide private data-plane access from a VNet to Blob, File, Queue, or Table services.

- **Firewall and network rules** 
 Firewall and network rules are Storage Account network access controls.
 They allow or deny data-plane access by IP, subnet, or private endpoint path.

### Azure Blob Storage
Azure Blob Storage is the object storage service for unstructured data in Azure.
It stores blobs in containers with tiering, replication, versioning, and object lifecycle features.

- **Containers** 
 Containers are namespace units for blobs within a storage account.
 They group blob objects under one logical path and access policy context.

- **Block blobs** 
 Block blobs are the general-purpose blob type for object content storage.
 They store data as ordered blocks committed into an immutable blob version state.

- **Append blobs** 
 Append blobs are blob objects optimized for append-only write patterns.
 They support sequential block appends without in-place block updates.

- **Page blobs** 
 Page blobs are random-access blob objects optimized for frequent read-write operations.
 They store data in 512-byte pages and back services such as unmanaged VM disks.

- **Access tiers** 
 Access tiers are object storage cost and retrieval profiles in Blob Storage.
 They define hot, cool, and archive economics for objects based on access frequency.

- **Versioning** 
 Versioning is the feature that preserves prior blob states after overwrite operations.
 It creates immutable historical versions addressable independently from the current blob.

- **Lifecycle management** 
 Lifecycle management is the rule engine for automatic blob transition and expiration.
 It moves or deletes data based on age, access conditions, and object filters.

- **Immutability policies** 
 Immutability policies are WORM retention controls for protected blob data.
 They prevent modification or deletion during a defined retention interval or legal hold.

### Azure Files
Azure Files is the managed file share service for SMB and NFS-compatible access.
It exposes fully managed file shares backed by Azure Storage and mountable from cloud or on-premises systems.

- **File shares** 
 File shares are the top-level filesystem exports in Azure Files.
 They store directories and files under one quota, protocol, and access control boundary.

- **SMB access** 
 SMB access is the Windows-compatible file protocol mode for Azure Files.
 It supports mounted file shares with ACL semantics and optional Active Directory integration.

- **NFS access** 
 NFS access is the POSIX-oriented file protocol mode for Azure Files.
 It exposes selected premium file shares to Linux clients through NFS semantics.

- **Azure File Sync** 
 Azure File Sync is the hybrid caching and synchronization service for Azure Files.
 It keeps one or more Windows Servers synchronized with an Azure file share and optionally tiers cold data to the cloud.

### Azure Queue Storage
Azure Queue Storage is the simple message queue service in Azure Storage.
It stores large volumes of lightweight messages for asynchronous producer-consumer workflows.

- **Queues** 
 Queues are message containers in Queue Storage.
 They hold messages until consumers retrieve and delete them.

- **Messages** 
 Messages are discrete queue payload entries with visibility and expiration semantics.
 They become temporarily invisible after retrieval until processed or made visible again.

### Azure Table Storage
Azure Table Storage is the schemaless key-attribute NoSQL service in Azure Storage.
It stores entities partitioned by partition key and uniquely identified by row key.

- **Tables** 
 Tables are entity collections within a storage account.
 They store flexible property bags under one partitioning and access model.

- **Entities** 
 Entities are the individual records stored in a table.
 They are keyed by partition key and row key for scalable lookup and partition grouping.

### Azure Data Lake Storage Gen2
Azure Data Lake Storage Gen2 is the hierarchical namespace extension over Azure Blob Storage for analytics workloads.
It adds directory semantics, atomic rename, and ACL-based filesystem behavior on top of blob data storage.

- **Hierarchical namespace** 
 Hierarchical namespace is the filesystem metadata model added to a storage account.
 It enables directory-level operations and more efficient analytics file management than flat blob listing.

- **POSIX-style ACLs** 
 POSIX-style ACLs are filesystem permission entries used in Data Lake Storage Gen2.
 They control access at directory and file level independently from Azure RBAC.

---

## Databases and Caching

Azure database services provide relational, globally distributed NoSQL, managed open-source engines, and in-memory cache services.
They define consistency, availability, failover, indexing, query, and scaling semantics for stateful applications.

### Azure SQL Database
Azure SQL Database is the fully managed relational database service based on the SQL Server engine.
It provides database-level PaaS deployment with automated patching, backups, high availability, and managed failover.

- **Single database** 
 Single database is the isolated deployment model for one Azure SQL database.
 It allocates compute, storage, and performance characteristics independently per database resource.

- **Elastic pools** 
 Elastic pools are shared compute containers for multiple Azure SQL databases.
 They distribute pooled performance capacity across databases with variable demand.

- **Serverless compute tier** 
 Serverless compute tier is the demand-responsive compute model for eligible Azure SQL databases.
 It scales compute automatically and can auto-pause during inactivity depending on configuration.

- **Hyperscale** 
 Hyperscale is the storage and scaling architecture for very large Azure SQL databases.
 It separates compute from distributed storage and supports fast scaling and copy operations.

- **Failover groups** 
 Failover groups are cross-region continuity resources for Azure SQL databases.
 They provide listener endpoints and coordinated failover behavior across paired database sets.

### Azure SQL Managed Instance
Azure SQL Managed Instance is the near full-instance PaaS deployment model for SQL Server workloads.
It exposes managed SQL Server instance semantics with native VNet placement and broader compatibility than single-database Azure SQL.

- **Instance pools** 
 Instance pools are shared virtual machine groups for faster Managed Instance deployment and cost optimization.
 They host multiple managed instances on pre-allocated compute infrastructure.

- **VNet deployment** 
 VNet deployment is the network isolation model for SQL Managed Instance.
 It places the service into a delegated subnet with private IP connectivity.

### Azure Cosmos DB
Azure Cosmos DB is the globally distributed, multi-model NoSQL database service in Azure.
It provides bounded latency, multi-region replication, tunable consistency, and partition-based horizontal scaling.

- **Accounts** 
 Accounts are the top-level Cosmos DB resources defining API type, regions, consistency, and networking.
 They host databases and containers under one globally distributed control boundary.

- **Databases** 
 Databases are logical groupings of containers within a Cosmos DB account.
 They provide an administrative namespace for related data containers.

- **Containers** 
 Containers are the primary data and throughput units in Cosmos DB.
 They store items, indexes, partition-key configuration, and request-unit throughput settings.

- **Partition keys** 
 Partition keys are application-defined attributes used to distribute data across physical partitions.
 They determine scale distribution, query locality, and hot-partition risk.

- **Consistency levels** 
 Consistency levels are the read consistency guarantees available in Cosmos DB.
 They range from strong to eventual and affect latency, staleness, and global distribution behavior.

- **Autoscale throughput** 
 Autoscale throughput is the request-unit capacity mode that adjusts available throughput automatically.
 It scales within configured limits in response to observed demand.

- **Global distribution** 
 Global distribution is the multi-region replication model of Cosmos DB accounts.
 It replicates data to selected regions and enables regional reads with configured failover policies.

### Azure Database for PostgreSQL
Azure Database for PostgreSQL is the managed PostgreSQL service in Azure.
It provides PostgreSQL-compatible deployment with Azure-managed patching, backup, and high-availability options.

- **Flexible Server** 
 Flexible Server is the primary deployment model for Azure Database for PostgreSQL.
 It exposes zone-aware placement, configurable maintenance, and adjustable compute or storage settings.

- **Read replicas** 
 Read replicas are asynchronously replicated secondary PostgreSQL servers.
 They serve read traffic and support offloading from the primary server.

- **High availability** 
 High availability is the redundant deployment mode for PostgreSQL Flexible Server.
 It maintains standby infrastructure for zonal or same-zone failover depending on configuration.

### Azure Database for MySQL
Azure Database for MySQL is the managed MySQL service in Azure.
It provides MySQL-compatible database hosting with managed lifecycle, backup, and connectivity features.

- **Flexible Server** 
 Flexible Server is the primary Azure MySQL deployment model.
 It offers configurable compute, storage, maintenance, and zone-aware high-availability options.

- **Read replicas** 
 Read replicas are asynchronous copies of the MySQL primary server.
 They handle read-only traffic and support scale-out read patterns.

### Azure Cache for Redis
Azure Cache for Redis is the managed in-memory cache service based on Redis technology.
It provides low-latency key-value access with managed patching, scaling, persistence, and network controls.

- **Basic** 
 Basic is the single-node, non-replicated deployment tier for Azure Cache for Redis.
 It provides one cache instance without SLA-backed replication.

- **Standard** 
 Standard is the replicated cache tier for Azure Cache for Redis.
 It provides primary-replica high availability within the service architecture.

- **Premium** 
 Premium is the advanced cache tier with added isolation, clustering, persistence, and network features.
 It supports VNet integration, data persistence, and higher scaling limits.

- **Clustering** 
 Clustering is the shard-based horizontal scaling model for compatible Redis tiers.
 It partitions keys across multiple cache nodes to increase capacity and throughput.

- **Persistence** 
 Persistence is the feature that writes in-memory Redis data to durable storage.
 It enables data recovery across cache restarts depending on persistence configuration.

---

## Observability and Operations

Azure observability and operations services collect metrics, logs, traces, topology data, and operational state across Azure and hybrid resources.
They define how telemetry is ingested, queried, correlated, alerted on, and used to automate operational response.

### Azure Monitor
Azure Monitor is the primary telemetry platform for metrics, logs, alerts, and visualization in Azure.
It collects platform and application signals and evaluates them through workspaces, metric stores, and alert engines.

- **Metrics** 
 Metrics are near-real-time time-series measurements emitted by Azure resources and applications.
 They are stored in the Azure Monitor metrics database and support aggregation, charting, and alerting.

- **Logs** 
 Logs are records stored in Log Analytics workspaces for query-driven analysis.
 They capture platform events, resource diagnostics, and custom application telemetry.

- **Diagnostic settings** 
 Diagnostic settings are resource-level export configurations for Azure platform logs and metrics.
 They route selected telemetry to Log Analytics, Event Hubs, or Storage Accounts.

- **Alerts** 
 Alerts are condition definitions that create fired alert instances from metrics, logs, or activity signals.
 They trigger notifications or automation when monitored conditions are satisfied.

- **Action groups** 
 Action groups are reusable notification and automation targets for alert rules.
 They dispatch alert responses to email, SMS, webhooks, or automation workflows.

- **Workbooks** 
 Workbooks are interactive visualization documents over Azure Monitor and related data sources.
 They combine queries, charts, text, and parameters into operational dashboards.

### Log Analytics
Log Analytics is the query and storage backend for Azure Monitor logs.
It stores telemetry in workspaces and exposes Kusto Query Language for analytics and correlation.

- **Workspaces** 
 Workspaces are regional log storage and query boundaries in Log Analytics.
 They define retention, access control, and ingestion scope for collected data.

- **KQL** 
 KQL is the query language used for Log Analytics and other Azure data exploration services.
 It supports filtering, joins, aggregation, parsing, and time-series analysis over log data.

- **Data collection rules** 
 Data collection rules are configurations that define how telemetry is collected, transformed, and routed.
 They control ingestion behavior for supported agents and data sources.

### Application Insights
Application Insights is the application performance monitoring service within Azure Monitor.
It collects request, dependency, exception, trace, and usage telemetry from instrumented applications.

- **Requests** 
 Requests are inbound application operation records captured by Application Insights.
 They measure duration, result code, success state, and correlation context for handled calls.

- **Dependencies** 
 Dependencies are outbound calls from an application to external services or components.
 They record latency, target, result, and correlation information for downstream operations.

- **Live Metrics** 
 Live Metrics is the near-real-time streaming telemetry view for instrumented applications.
 It displays current request rate, failure rate, and performance signals without waiting for standard ingestion.

- **Availability tests** 
 Availability tests are synthetic probes defined in Application Insights.
 They check endpoint reachability and response correctness from distributed test locations.

### Network Watcher
Network Watcher is the network diagnostics and monitoring service for Azure networking resources.
It provides flow inspection, topology, packet capture, connection tests, and effective configuration analysis.

- **Connection Monitor** 
 Connection Monitor is the end-to-end network path monitoring feature in Network Watcher.
 It tests connectivity, latency, and path health between defined source and destination endpoints.

- **NSG flow logs** 
 NSG flow logs are telemetry records about network flows processed by NSG rules.
 They capture source, destination, protocol, and allow or deny decisions for analyzed traffic.

- **Packet capture** 
 Packet capture is the on-demand network packet recording capability for Azure VMs.
 It stores captured packets for deeper transport and protocol troubleshooting.

- **IP flow verify** 
 IP flow verify is the rule-evaluation diagnostic for a specific traffic flow.
 It determines whether a packet would be allowed or denied by effective NSG rules.

### Azure Advisor
Azure Advisor is the recommendation engine for cost, reliability, security, operational excellence, and performance.
It analyzes deployed Azure resources and emits optimization suggestions based on observed configuration and usage patterns.

- **Recommendation categories** 
 Recommendation categories are the broad optimization domains used by Azure Advisor.
 They classify findings into cost, performance, reliability, security, or operations improvement areas.

- **Personalized recommendations** 
 Personalized recommendations are account-specific findings generated from current Azure estate state.
 They identify actionable configuration or sizing improvements for the scoped resources.

---

## Analytics and Data Engineering

Azure analytics services provide data integration, warehousing, streaming, lakehouse access, and distributed compute platforms.
They define how data is ingested, transformed, cataloged, queried, processed, and served for analytics workloads.

### Azure Synapse Analytics
Azure Synapse Analytics is the analytics platform that combines SQL analytics, Spark, pipelines, and workspace-based integration.
It provides a unified management surface over data warehouse, big data, and integration workloads.

- **Workspaces** 
 Workspaces are the top-level Synapse administrative and collaboration boundaries.
 They contain SQL pools, Spark pools, pipelines, linked services, and security configuration.

- **Dedicated SQL pools** 
 Dedicated SQL pools are provisioned MPP data warehouse compute resources in Synapse.
 They store and query relational warehouse data with reserved compute capacity.

- **Serverless SQL pools** 
 Serverless SQL pools are on-demand query endpoints in Synapse.
 They execute T-SQL over files in storage without dedicated warehouse provisioning.

- **Spark pools** 
 Spark pools are managed Apache Spark compute resources in Synapse.
 They provide distributed execution for notebooks, jobs, and data engineering tasks.

- **Pipelines** 
 Pipelines are orchestration resources in Synapse derived from Azure Data Factory semantics.
 They sequence data movement and transformation activities across sources and compute backends.

### Azure Data Factory
Azure Data Factory is the managed data integration and orchestration service for ETL and ELT workflows.
It executes pipelines that move and transform data across cloud and hybrid data sources.

- **Factories** 
 Factories are the top-level management resources for Data Factory deployments.
 They contain pipelines, datasets, linked services, triggers, and integration runtimes.

- **Pipelines** 
 Pipelines are orchestration graphs of activities in Data Factory.
 They sequence copy, control flow, and transformation operations with dependency logic.

- **Activities** 
 Activities are the executable units inside a Data Factory pipeline.
 They perform data movement, transformation, control flow, or external compute invocation.

- **Linked services** 
 Linked services are connection definitions for data stores or compute services.
 They hold endpoint and authentication metadata used by datasets and activities.

- **Datasets** 
 Datasets are structured references to data within a data store.
 They describe location, schema, and format information for pipeline activities.

- **Integration runtimes** 
 Integration runtimes are the execution backends used by Data Factory.
 They provide Azure-hosted, self-hosted, or managed VNet-based execution for connectors and movement.

### Azure Databricks
Azure Databricks is the managed Apache Spark-based analytics and data engineering platform integrated with Azure.
It provides collaborative notebooks, job execution, cluster management, and lakehouse-oriented processing.

- **Workspaces** 
 Workspaces are the top-level administrative boundaries in Azure Databricks.
 They contain notebooks, jobs, clusters, SQL assets, and identity integration configuration.

- **Clusters** 
 Clusters are Spark execution environments provisioned for notebooks or jobs.
 They define node type, autoscaling, runtime version, and attached libraries.

- **Jobs** 
 Jobs are scheduled or triggered execution definitions in Databricks.
 They run notebooks, scripts, or workflows on defined compute resources.

- **SQL warehouses** 
 SQL warehouses are Databricks SQL compute endpoints for query workloads.
 They execute SQL statements independently of interactive Spark clusters.

### Azure Stream Analytics
Azure Stream Analytics is the managed stream processing service for event and telemetry pipelines.
It evaluates continuous queries over streaming inputs and emits transformed outputs with event-time semantics.

- **Streaming jobs** 
 Streaming jobs are the executable processing resources in Stream Analytics.
 They bind inputs, query logic, and outputs into one continuously running pipeline.

- **Inputs** 
 Inputs are streaming or reference data sources consumed by a Stream Analytics job.
 They provide ordered event streams or static lookup data for query evaluation.

- **Outputs** 
 Outputs are destination endpoints for transformed stream results.
 They receive projected, aggregated, or filtered event data from the running job.

### Microsoft Fabric
Microsoft Fabric is a broader SaaS analytics platform that sits adjacent to core Azure data services rather than replacing them as ARM-native resources.
It provides unified analytics experiences but follows platform boundaries and management models distinct from most Azure resource-provider patterns.

---

## AI and Machine Learning

Azure AI and ML services provide model development, managed training, inference hosting, and prebuilt AI APIs.
They define ML lifecycle management, artifact versioning, online and batch serving, and AI endpoint consumption semantics.

### Azure Machine Learning
Azure Machine Learning is the managed machine learning platform for experimentation, training, model management, and deployment.
It organizes ML assets inside a workspace and coordinates compute, data, environments, and endpoint resources.

- **Workspaces** 
 Workspaces are the top-level administrative boundaries for Azure Machine Learning.
 They contain experiments, models, compute, environments, endpoints, and linked storage resources.

- **Compute instances** 
 Compute instances are managed single-user development machines for interactive ML work.
 They provide notebook-capable compute with workspace-integrated identity and storage access.

- **Compute clusters** 
 Compute clusters are autoscaling multi-node execution backends for training or batch workloads.
 They provision and deallocate VMs based on queued jobs and min-max settings.

- **Jobs** 
 Jobs are the executable workload resources in Azure Machine Learning.
 They run training, command, sweep, or pipeline tasks on configured compute.

- **Models** 
 Models are versioned ML artifacts registered in a workspace or registry.
 They represent deployable or trackable outputs from training and packaging workflows.

- **Environments** 
 Environments are versioned runtime definitions for ML jobs and deployments.
 They package container base image, conda dependencies, and runtime metadata into a reusable execution profile.

- **Online endpoints** 
 Online endpoints are HTTPS inference resources for real-time model serving.
 They route prediction requests to one or more deployments with traffic allocation settings.

- **Batch endpoints** 
 Batch endpoints are asynchronous inference resources for batch scoring workloads.
 They submit jobs that process large datasets and write outputs to storage.

- **Registries** 
 Registries are shared cross-workspace asset repositories in Azure Machine Learning.
 They store reusable models, environments, and components for multi-workspace distribution.

### Azure AI Services
Azure AI Services is the managed family of prebuilt cognitive API services in Azure.
It exposes HTTP endpoints for vision, language, speech, document, and decision-oriented AI workloads.

- **Multi-service resources** 
 Multi-service resources are shared provisioning resources that expose multiple AI capabilities under one endpoint and billing boundary.
 They simplify access control and key management for supported service combinations.

- **Language service** 
 Language service is the NLP capability set within Azure AI Services.
 It exposes text analytics, conversational language, and classification features through managed APIs.

- **Speech service** 
 Speech service is the speech recognition, synthesis, and translation capability set.
 It processes audio streams or text over managed endpoints with model-backed inference.

- **Vision service** 
 Vision service is the image and video analysis capability set in Azure AI Services.
 It exposes managed APIs for visual feature extraction and classification tasks.

- **Document Intelligence** 
 Document Intelligence is the document parsing and structured extraction service.
 It analyzes files and returns extracted fields, layout, and recognized document elements.

### Azure OpenAI Service
Azure OpenAI Service is the Azure-hosted managed API service for OpenAI foundation models under Azure governance boundaries.
It exposes model deployments through Azure resources, authentication, networking, and compliance controls rather than direct public OpenAI tenancy.

- **Deployments** 
 Deployments are named model-serving configurations inside an Azure OpenAI resource.
 They map a selected model version to a callable endpoint path and quota scope.

- **Content filtering and abuse monitoring** 
 Content filtering and abuse monitoring are the policy and safety enforcement layers around model usage.
 They evaluate prompts and completions under configured or platform-managed safety controls.

---

## Migration, Backup, and Hybrid

Azure migration, backup, and hybrid services move workloads into Azure, protect state, and extend Azure control to external infrastructure.
They define replication, recovery, onboarding, inventory, and hybrid management semantics across cloud and non-cloud environments.

### Azure Migrate
Azure Migrate is the migration hub and assessment service for discovering, evaluating, and moving workloads to Azure.
It aggregates migration tools and produces readiness, sizing, dependency, and migration tracking outputs.

- **Discovery and assessment** 
 Discovery and assessment is the inventory and readiness analysis capability within Azure Migrate.
 It collects workload metadata and evaluates suitability for target Azure services.

- **Migration tooling integration** 
 Migration tooling integration is the orchestration surface connecting Azure Migrate to move engines and partner tools.
 It tracks migration project state across supported workload categories.

### Azure Site Recovery
Azure Site Recovery is the disaster recovery replication and failover orchestration service for machines and selected workloads.
It continuously replicates workload state to a target region or site and coordinates test or actual failovers.

- **Replication policies** 
 Replication policies are configuration objects defining replication frequency, retention, and snapshot behavior.
 They control how protected workloads are replicated and recovered.

- **Recovery plans** 
 Recovery plans are ordered failover orchestration definitions in Site Recovery.
 They sequence machine groups, scripts, and manual actions during recovery.

- **Test failover** 
 Test failover is the isolated recovery validation workflow in Site Recovery.
 It launches replicated workloads in a non-production context without disrupting ongoing protection.

### Azure Backup
Azure Backup is the managed backup orchestration service for Azure and hybrid workloads.
It creates recovery points and enforces retention and restore workflows through policy-driven vault resources.

- **Recovery Services vaults** 
 Recovery Services vaults are backup and recovery management containers for supported workload types.
 They store metadata, policies, and recovery-point coordination state.

- **Backup policies** 
 Backup policies are schedule and retention rule definitions for protected resources.
 They determine when backups run and how long restore points are kept.

- **Recovery points** 
 Recovery points are the restorable backup artifacts produced by Azure Backup.
 They represent point-in-time recoverable states for protected resources.

- **Instant restore** 
 Instant restore is the temporary snapshot-based fast recovery path for supported backups.
 It enables rapid restore before longer-term vault copy paths are required.

### Azure Arc
Azure Arc is the hybrid resource management framework for projecting non-Azure resources into Azure control-plane governance.
It extends Azure Resource Manager, Policy, Defender, and management capabilities to servers, Kubernetes, and data services outside Azure.

- **Arc-enabled servers** 
 Arc-enabled servers are non-Azure machines represented as Azure resources.
 They receive Azure governance, inventory, and extension management through Arc agents.

- **Arc-enabled Kubernetes** 
 Arc-enabled Kubernetes is the projection of external Kubernetes clusters into Azure management scope.
 It enables policy, GitOps, and inventory features over connected clusters.

- **GitOps configuration** 
 GitOps configuration is the declarative cluster state synchronization feature in Arc-enabled Kubernetes.
 It reconciles manifests from a Git source into connected clusters.

---

## Cost Management and Resource Organization

Azure cost and organization services define hierarchy, spend tracking, optimization, and administrative grouping across tenants and subscriptions.
They separate financial visibility and governance from runtime isolation while supporting large-scale multi-subscription operations.

### Microsoft Cost Management
Microsoft Cost Management is the spend analysis, budgeting, and cost governance service for Azure and supported Microsoft clouds.
It aggregates consumption and billing data into scopes such as billing account, management group, subscription, or resource group.

- **Cost analysis** 
 Cost analysis is the interactive reporting view over Azure spending data.
 It slices usage cost by scope, resource, tag, meter, or time dimension.

- **Budgets** 
 Budgets are threshold-based spend tracking resources in Cost Management.
 They compare actual or forecasted cost against configured limits and can trigger notifications.

- **Exports** 
 Exports are scheduled delivery jobs for cost and usage datasets.
 They write billing data to storage for downstream analytics or chargeback workflows.

- **Reservations and savings visibility** 
 Reservations and savings visibility is the reporting dimension for committed-use discount constructs.
 It exposes coverage and utilization behavior of reserved-capacity and savings-plan-like commitments where applicable.

### Azure Advisor
Azure Advisor is the recommendation engine for optimizing Azure deployments across cost, security, reliability, performance, and operations.
It analyzes active resource configuration and usage patterns and emits actionable optimization recommendations.

- **Cost recommendations** 
 Cost recommendations are Advisor findings related to reducing unnecessary spend.
 They identify idle, underutilized, or discount-eligible resources from observed usage patterns.

- **Reliability recommendations** 
 Reliability recommendations are Advisor findings related to resiliency and continuity.
 They identify missing redundancy, backup, or availability configuration that increases outage risk.

### Tags
Tags are key-value metadata labels that can be attached to Azure resources and scopes.
They support organization, policy targeting, automation selection, and cost reporting segmentation.

- **Inheritance through policy** 
 Inheritance through policy is the governance pattern used to propagate tags to descendant resources.
 It applies modify or append effects during resource creation or remediation.

- **Cost attribution** 
 Cost attribution is the use of tag metadata to segment billing analysis.
 It associates resource consumption with teams, environments, or projects for financial reporting.
