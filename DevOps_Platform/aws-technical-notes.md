# AWS Strong Technical Notes

## Compute

AWS compute services provide virtual machines, managed platform runtimes, and container orchestration control planes.
They define how execution capacity is provisioned, isolated, scheduled, and integrated with storage, identity, and networking.

### Amazon EC2
Amazon EC2 is the virtual machine service for provisioning compute capacity in AWS.
It exposes instance, storage, network, placement, and lifecycle controls through regional APIs.

- **Instances** 
 Instances are virtual machine resources launched into a subnet within a specific Availability Zone.
 They run guest operating systems and application processes with attached vCPU, memory, storage, and ENI-based networking.

- **Instance types** 
 Instance types are predefined hardware profiles for CPU, memory, network, accelerator, and storage characteristics.
 They determine the performance envelope and underlying host capabilities available to an instance.

- **AMIs** 
 Amazon Machine Images are boot templates containing an operating system, root volume configuration, and optional software state.
 They define the base image from which new EC2 instances are created.

- **Launch templates** 
 Launch templates are reusable instance launch specifications.
 They store AMI, instance type, network, storage, IAM role, and bootstrap settings for repeated provisioning.

- **Auto Scaling groups** 
 Auto Scaling groups are controllers that maintain a desired fleet size of EC2 instances.
 They launch or terminate instances based on health checks, scaling policies, and capacity settings.

- **Placement groups** 
 Placement groups are placement policies that influence how instances are distributed across physical infrastructure.
 They optimize for low latency, high throughput, or failure isolation depending on cluster, partition, or spread mode.

- **Security groups** 
 Security groups are stateful virtual firewall policies attached to ENIs.
 They evaluate ingress and egress allows by protocol, port, and peer and automatically permit return traffic for established flows.

- **Elastic Network Interfaces** 
 Elastic Network Interfaces are virtual network interfaces attached to EC2 instances.
 They carry private and optional public addressing, security group association, and MAC-level network identity.

- **Elastic IPs** 
 Elastic IPs are static public IPv4 allocations scoped to a region.
 They can be remapped between compatible resources to preserve public endpoint identity.

- **User data** 
 User data is launch-time bootstrap input passed to an EC2 instance.
 It is consumed by cloud-init or equivalent initialization agents to apply first-boot configuration.

- **Instance metadata service** 
 The Instance Metadata Service is the link-local endpoint exposing instance identity and configuration data.
 It provides runtime access to metadata and temporary credentials from within the instance.

- **Nitro System** 
 The Nitro System is the EC2 hardware and virtualization architecture for compute, storage, and networking offload.
 It minimizes host-side virtualization overhead by moving control functions into dedicated hardware and lightweight hypervisor components.

- **Dedicated Hosts** 
 Dedicated Hosts are physical servers reserved for a single AWS account.
 They provide host-level placement visibility and support licensing models tied to socket or core affinity.

- **Spot Instances** 
 Spot Instances are EC2 instances that consume spare AWS capacity at variable pricing.
 They can be interrupted by AWS and are suited to fault-tolerant or checkpointed workloads.

- **Reserved Instances** 
 Reserved Instances are billing constructs that discount instance usage for committed terms.
 They reduce cost for matching instance attributes without being a separate compute resource type.

### Elastic Beanstalk
Elastic Beanstalk is a managed application deployment service for supported web runtimes and worker processes.
It provisions and updates underlying AWS infrastructure from an application package and environment configuration.

- **Applications** 
 Applications are top-level Elastic Beanstalk containers for versions and environments.
 They group related deployable artifacts under one logical application identity.

- **Environments** 
 Environments are isolated deployments of an application version and runtime platform.
 They encapsulate EC2, Auto Scaling, load balancing, health reporting, and configuration state.

- **Application versions** 
 Application versions are immutable source bundle or artifact revisions.
 They serve as the deployable units promoted into Elastic Beanstalk environments.

- **Configuration templates** 
 Configuration templates are reusable sets of environment option values.
 They capture platform, scaling, networking, and environment variable settings for repeated environment creation.

### AWS Batch
AWS Batch is the managed batch scheduling service for containerized and EC2-based jobs.
It queues jobs, provisions compute environments, and places tasks according to resource requirements.

- **Job definitions** 
 Job definitions are reusable specifications for container image, vCPU, memory, retries, and runtime settings.
 They define how an individual batch job should execute.

- **Job queues** 
 Job queues are ordered submission endpoints for batch jobs.
 They prioritize pending work and map it to eligible compute environments.

- **Compute environments** 
 Compute environments are capacity backends for AWS Batch execution.
 They provision and manage EC2, Spot, or Fargate resources to run queued jobs.

---

## Containers and Orchestration

AWS container services provide native and Kubernetes-based orchestration for running containers.
They define task scheduling, node management, identity mapping, and traffic exposure for distributed container workloads.

### Amazon ECS
Amazon ECS is AWS’s native container orchestration service for task and service-based workloads.
It schedules containers onto EC2 or Fargate capacity using cluster, task, and service abstractions.

- **Clusters** 
 Clusters are logical scheduling boundaries for ECS tasks and services.
 They group available compute capacity and maintain orchestration metadata for workload placement.

- **Task definitions** 
 Task definitions are versioned specifications for one or more co-scheduled containers.
 They declare images, CPU, memory, IAM role, volumes, networking mode, and startup behavior.

- **Tasks** 
 Tasks are running or stopped instantiations of a task definition.
 They represent one scheduled execution unit on ECS capacity.

- **Services** 
 Services are controllers that maintain a desired number of task replicas.
 They replace failed tasks, manage rolling deployments, and optionally integrate with load balancers.

- **Capacity providers** 
 Capacity providers are ECS scheduling backends linked to EC2 Auto Scaling groups or Fargate.
 They let placement strategies consume one or more compute pools under defined weighting rules.

- **Fargate** 
 Fargate is the serverless execution mode for ECS tasks.
 It allocates compute and memory per task without exposing node-level infrastructure management.

- **Service discovery** 
 Service discovery is the registration mechanism between ECS services and DNS-based naming.
 It maps running task endpoints into discoverable service records through integrated namespace systems.

- **ECS Exec** 
 ECS Exec is the managed interactive command feature for running containers.
 It establishes a secure control channel for shell or process access without opening inbound ports.

### Amazon EKS
Amazon EKS is the managed Kubernetes control plane service for AWS.
It runs the Kubernetes API server and integrates cluster operations with VPC networking, IAM, and AWS load balancing.

- **Clusters** 
 Clusters are managed Kubernetes control planes deployed regionally by EKS.
 They host Kubernetes API state and coordinate scheduling onto attached worker capacity.

- **Managed node groups** 
 Managed node groups are AWS-managed sets of EC2 worker nodes for EKS clusters.
 They automate node provisioning, upgrade orchestration, health replacement, and cluster registration.

- **Self-managed nodes** 
 Self-managed nodes are EC2 worker nodes provisioned outside EKS-managed node group workflows.
 They provide full node customization at the cost of additional lifecycle management.

- **Fargate profiles** 
 Fargate profiles are selectors that route matching pods to Fargate execution.
 They eliminate worker node management for designated namespaces or label sets.

- **IAM Roles for Service Accounts** 
 IAM Roles for Service Accounts is the identity federation model between Kubernetes service accounts and IAM roles.
 It provides AWS API access to pods through temporary credentials instead of static secrets.

- **Add-ons** 
 Add-ons are managed installations of common operational components such as CoreDNS, kube-proxy, and VPC CNI.
 They provide versioned lifecycle management for cluster-critical software.

- **Ingress controller integration** 
 Ingress controller integration is the reconciliation path from Kubernetes traffic resources to AWS load balancers.
 It maps ingress or gateway configuration into ALB or NLB resources through controller logic.

- **Cluster autoscaler integration** 
 Cluster autoscaler integration is the node capacity scaling model used with EKS worker nodes.
 It increases or decreases backing node groups based on unschedulable pod demand and resource pressure.

### Amazon ECR
Amazon ECR is the managed container image registry for OCI-compatible artifacts.
It stores images, manifests, and layers with IAM-based access control and lifecycle policy support.

- **Repositories** 
 Repositories are named registry namespaces for related image versions.
 They store tagged and digested images under one access and lifecycle policy boundary.

- **Image scanning** 
 Image scanning is the vulnerability analysis feature for container images in ECR.
 It inspects image contents against known vulnerability databases and emits findings.

- **Lifecycle policies** 
 Lifecycle policies are automated cleanup rules for repository images.
 They expire images by tag status, count, or age to control retention.

- **Cross-region replication** 
 Cross-region replication is the asynchronous replication model for ECR repositories.
 It copies pushed images to target regions for locality and recovery use cases.

---

## Serverless and APIs

AWS serverless services provide function execution, API ingress, and event-driven request processing.
They remove server management from execution while preserving integration with identity, observability, and messaging systems.

### AWS Lambda
AWS Lambda is the function execution service for event-driven and request-driven code.
It runs handlers in isolated execution environments that scale with invocation demand.

- **Functions** 
 Functions are the primary Lambda resources containing code, runtime settings, and execution configuration.
 They define handler entry point, memory, timeout, architecture, and IAM execution role.

- **Versions** 
 Versions are immutable snapshots of function code and configuration.
 They provide stable release identifiers for deployment and traffic routing.

- **Aliases** 
 Aliases are named references to specific function versions.
 They decouple callers from numeric versions and can split traffic between versions.

- **Event source mappings** 
 Event source mappings are poller bindings between Lambda and supported event sources.
 They read records from queues or streams and invoke the function with batched payloads.

- **Layers** 
 Layers are versioned archives of shared libraries, runtimes, or custom code.
 They are mounted into the execution environment and reused across multiple functions.

- **Provisioned concurrency** 
 Provisioned concurrency is pre-initialized execution capacity for a function version or alias.
 It reduces cold-start latency by keeping a configured number of execution environments ready.

- **Reserved concurrency** 
 Reserved concurrency is the per-function concurrency allocation and cap.
 It guarantees execution capacity for one function while limiting its maximum concurrent invocations.

- **Destinations** 
 Destinations are asynchronous invocation outcome routes for success or failure.
 They forward invocation result metadata to supported services after execution completes.

### Amazon API Gateway
Amazon API Gateway is the managed front-door service for REST, HTTP, and WebSocket APIs.
It terminates client requests and applies routing, authorization, throttling, and backend integration logic.

- **REST APIs** 
 REST APIs are the original API Gateway model with resource-path and method-oriented configuration.
 They support mapping templates, usage plans, API keys, and a broad transformation feature set.

- **HTTP APIs** 
 HTTP APIs are the streamlined API Gateway model for HTTP services.
 They provide lower-latency routing and simpler integration patterns than REST APIs.

- **WebSocket APIs** 
 WebSocket APIs are persistent bidirectional API resources based on WebSocket connections.
 They route messages using route selection logic and maintain client connection state.

- **Stages** 
 Stages are named deployment environments for an API configuration.
 They bind a deployed API snapshot to stage-specific variables, logging, and throttling settings.

- **Authorizers** 
 Authorizers are authentication and authorization components invoked before request forwarding.
 They evaluate IAM signatures, JWTs, or Lambda-generated policies to permit or deny access.

- **Usage plans** 
 Usage plans are request quota and throttle policies associated with API keys.
 They limit client consumption across one or more deployed API stages.

- **Integration types** 
 Integration types are the backend invocation models used by API Gateway.
 They define whether requests target Lambda, HTTP endpoints, AWS services, or mocked responses.

### AWS App Runner
AWS App Runner is a managed application runtime for containerized web services.
It deploys code or images into a request-serving runtime with built-in networking and autoscaling.

- **Services** 
 Services are App Runner deployment resources representing one networked application.
 They bind source, runtime configuration, autoscaling settings, and a public endpoint into one unit.

- **Auto deployments** 
 Auto deployments are source-driven redeploy triggers for App Runner services.
 They update the running service when linked repository or image changes are detected.

---

## Networking and Content Delivery

AWS networking services define virtual network boundaries, route propagation, endpoint exposure, and edge distribution.
They control how packets and requests move between workloads, users, AWS services, and external networks.

### Amazon VPC
Amazon VPC is the logically isolated virtual network service for AWS resources.
It provides private address space, routing domains, segmentation, and network policy controls.

- **CIDR blocks** 
 CIDR blocks are IP ranges allocated to a VPC or subnet.
 They define the address space from which private IPs are assigned.

- **Subnets** 
 Subnets are zonal partitions of a VPC CIDR block.
 They place resources in specific Availability Zones and define local routing association boundaries.

- **Route tables** 
 Route tables are forwarding policy objects associated with subnets.
 They map destination CIDRs to local routes, gateways, peering links, transit attachments, or appliances.

- **Internet Gateway** 
 An Internet Gateway is the VPC attachment that enables direct routing between public subnets and the internet.
 It serves as the next hop target for internet-bound or internet-sourced traffic.

- **Egress-only Internet Gateway** 
 An Egress-only Internet Gateway is the outbound-only internet attachment for IPv6 traffic.
 It allows IPv6 egress from private resources without permitting unsolicited inbound initiation.

- **NAT Gateway** 
 A NAT Gateway is the managed source NAT service for private subnet outbound internet access.
 It translates private source addresses to a public address while blocking unsolicited inbound initiation.

- **Security Groups** 
 Security Groups are stateful ENI-level firewall policies.
 They evaluate allow rules by protocol, port, and peer while automatically tracking connection state.

- **Network ACLs** 
 Network ACLs are stateless subnet-level traffic filters.
 They process ordered allow and deny rules independently for ingress and egress.

- **Elastic Network Interfaces** 
 Elastic Network Interfaces are virtual NICs attached to VPC resources such as EC2.
 They carry IP addresses, security group bindings, MAC identity, and attachment lifecycle.

- **VPC Peering** 
 VPC Peering is a private routing relationship between two VPCs.
 It enables direct traffic exchange without transitive routing across additional peers.

- **Transit Gateway** 
 Transit Gateway is the centralized routing hub for connecting multiple VPCs and hybrid networks.
 It provides transitive routing through attachment, route table, association, and propagation constructs.

- **VPC Endpoints** 
 VPC Endpoints are private connectivity resources for reaching AWS services from within a VPC.
 They avoid internet traversal by using gateway or interface endpoint models over AWS private paths.

- **PrivateLink** 
 PrivateLink is the private service exposure mechanism behind interface VPC endpoints and endpoint services.
 It publishes services through private ENIs so consumers can connect without overlapping-network routing dependencies.

- **Flow Logs** 
 Flow Logs are records of accepted or rejected IP traffic metadata for ENIs, subnets, or VPCs.
 They capture source, destination, port, protocol, and disposition data for network analysis.

### Elastic Load Balancing
Elastic Load Balancing is the managed traffic distribution service for AWS backends.
It forwards requests or connections to healthy targets according to protocol-aware listener behavior.

- **Application Load Balancer** 
 Application Load Balancer is the Layer 7 load balancer for HTTP and HTTPS traffic.
 It routes requests by host, path, method, header, or query conditions to target groups.

- **Network Load Balancer** 
 Network Load Balancer is the Layer 4 load balancer for TCP, UDP, and TLS traffic.
 It supports static IPs, high throughput, and low-latency connection forwarding.

- **Gateway Load Balancer** 
 Gateway Load Balancer is the flow distribution service for inline network appliances.
 It forwards traffic to virtual appliances using transparent encapsulation and preserves appliance insertion semantics.

- **Target groups** 
 Target groups are backend collections registered behind listeners or rules.
 They define target type, health evaluation, protocol, and destination membership.

- **Listeners** 
 Listeners are frontend protocol and port bindings on a load balancer.
 They accept client traffic and apply routing actions to selected target groups.

- **Listener rules** 
 Listener rules are conditional routing definitions attached to ALB listeners.
 They evaluate request attributes and select actions such as forward, redirect, or fixed response.

- **Health checks** 
 Health checks are probe definitions used to determine target eligibility.
 They continuously test targets and remove unhealthy destinations from routing decisions.

### Amazon Route 53
Amazon Route 53 is the authoritative DNS and DNS-based routing service for AWS and external domains.
It resolves names and can direct clients using policy-driven record selection and health state.

- **Hosted zones** 
 Hosted zones are authoritative DNS containers for a domain namespace.
 They store record sets and define the DNS boundary managed by Route 53.

- **Public hosted zones** 
 Public hosted zones are internet-resolvable authoritative DNS zones.
 They serve records to public resolvers for delegated domain names.

- **Private hosted zones** 
 Private hosted zones are VPC-scoped authoritative DNS zones.
 They resolve only within associated VPCs and support internal service naming.

- **Record sets** 
 Record sets are DNS entries stored in a hosted zone.
 They map names to addresses, aliases, text data, mail exchange records, or service-specific values.

- **Health checks** 
 Health checks are endpoint probes used for DNS failover and health-aware routing.
 They evaluate reachability or application response validity from Route 53 checker locations.

- **Routing policies** 
 Routing policies are response selection strategies for DNS queries.
 They implement simple, weighted, latency, failover, geolocation, geoproximity, or multivalue answer logic.

### Amazon CloudFront
Amazon CloudFront is the edge delivery network for caching and serving HTTP content.
It distributes requests to edge locations and applies cache, origin, and request-processing policies.

- **Distributions** 
 Distributions are the top-level delivery configurations in CloudFront.
 They bind origins, cache behaviors, TLS settings, and edge request policies into one global endpoint.

- **Origins** 
 Origins are backend content sources such as S3 buckets, ALBs, or custom HTTP servers.
 CloudFront fetches uncached or expired content from them based on origin settings.

- **Behaviors** 
 Behaviors are path-pattern-based request handling rules within a distribution.
 They define cache policy, origin selection, protocol requirements, and edge function attachment.

- **Edge locations** 
 Edge locations are globally distributed CloudFront points of presence.
 They terminate client requests and serve cached content closer to users.

- **Origin Access Control** 
 Origin Access Control is the signed-origin access mechanism for CloudFront-to-origin communication.
 It restricts origin access so that only authorized CloudFront requests can fetch protected content.

- **Cache policies** 
 Cache policies are reusable rules controlling cache key composition and TTL behavior.
 They determine which headers, cookies, and query strings influence object caching.

- **Origin request policies** 
 Origin request policies are reusable rules for forwarding request attributes to origins.
 They control which headers, cookies, and query strings are passed beyond the edge.

### AWS Global Accelerator
AWS Global Accelerator is the global anycast traffic entry service for internet applications.
It routes client traffic onto the AWS backbone and directs it to optimal regional endpoints.

- **Accelerators** 
 Accelerators are the top-level Global Accelerator resources exposing static anycast IPs.
 They bind listeners and endpoint groups into one globally reachable ingress configuration.

- **Endpoint groups** 
 Endpoint groups are regional collections of application endpoints behind an accelerator.
 They define traffic dial, health behavior, and region-specific endpoint routing.

### AWS WAF
AWS WAF is the web application firewall service for HTTP request inspection and filtering.
It evaluates requests against rule logic before traffic reaches protected resources.

- **Web ACLs** 
 Web ACLs are the top-level policy containers for WAF rules.
 They aggregate rule evaluation and attach to supported services such as CloudFront or ALB.

- **Rules** 
 Rules are match conditions and actions applied to incoming web requests.
 They allow, block, count, or challenge requests based on headers, IPs, patterns, or labels.

- **Rule groups** 
 Rule groups are reusable collections of WAF rules.
 They package multiple inspection rules under one attachable policy unit.

### AWS Shield
AWS Shield is the managed DDoS protection service for AWS-facing endpoints.
It detects and mitigates volumetric and protocol attacks against protected resources.

- **Shield Standard** 
 Shield Standard is the default DDoS protection automatically applied to supported AWS services.
 It provides always-on baseline mitigation for common network and transport-layer attacks.

- **Shield Advanced** 
 Shield Advanced is the enhanced DDoS protection tier with additional visibility and response features.
 It adds advanced detection, attack diagnostics, and tighter integration with WAF and incident workflows.

---

## Identity, Security, and Compliance

AWS identity and security services define principals, permissions, cryptographic controls, secret storage, and audit telemetry.
They govern who can access resources, how data is protected, and how security posture is evaluated.

### AWS IAM
IAM is AWS’s identity and authorization system for API and resource access.
It evaluates principals, policies, and conditions at request time to determine allowed actions.

- **Users** 
 Users are long-lived IAM identities representing human or programmatic actors.
 They authenticate with passwords, access keys, or federation-linked workflows depending on access mode.

- **Groups** 
 Groups are collections of IAM users for shared policy attachment.
 They simplify permission assignment by applying common policies across multiple users.

- **Roles** 
 Roles are assumable IAM identities intended for temporary credential issuance.
 They rely on trust policies and STS session creation rather than long-lived secrets.

- **Policies** 
 Policies are JSON permission documents defining allowed or denied actions, resources, and conditions.
 They are evaluated across identity, resource, session, SCP, and boundary policy layers.

- **Permission boundaries** 
 Permission boundaries are managed policies that cap the maximum permissions a user or role may receive.
 They restrict effective access even when broader identity policies are attached.

- **Session policies** 
 Session policies are temporary permission restrictions applied during role assumption or federation.
 They further narrow the permissions available within a specific session.

- **Access Analyzer** 
 IAM Access Analyzer is the policy analysis service for identifying external or broad access paths.
 It evaluates resource and identity policies to detect unintended sharing or excessive exposure.

### AWS IAM Identity Center
IAM Identity Center is the workforce access management service for AWS accounts and applications.
It centralizes federated sign-in, permission set assignment, and account access provisioning.

- **Permission sets** 
 Permission sets are reusable access definitions mapped to IAM roles in target accounts.
 They translate workforce access assignments into account-local assumable roles.

- **Assignments** 
 Assignments bind users or groups to permission sets in specific AWS accounts.
 They determine which principals can assume which access roles through the portal.

### AWS KMS
AWS KMS is the managed cryptographic key service for encryption and signing operations.
It stores keys under controlled policies and exposes API-based cryptographic primitives.

- **Customer managed keys** 
 Customer managed keys are KMS keys created and administered by the customer.
 They support explicit lifecycle, rotation, aliasing, and fine-grained policy control.

- **AWS managed keys** 
 AWS managed keys are service-integrated KMS keys created and managed by AWS.
 They provide encryption integration with limited administrative customization.

- **Aliases** 
 Aliases are stable friendly names mapped to KMS keys.
 They decouple application configuration from concrete key identifiers.

- **Key policies** 
 Key policies are resource-based permission documents attached directly to KMS keys.
 They define which principals can administer or use the key and under what conditions.

- **Grants** 
 Grants are delegated permission records for KMS key usage.
 They allow temporary or scoped access without directly editing key policy.

- **Envelope encryption** 
 Envelope encryption is the pattern of encrypting data with a data key and protecting that data key with a KMS key.
 It reduces repeated direct use of the master key and scales efficiently for large data volumes.

### AWS Secrets Manager
AWS Secrets Manager is the managed secret storage and rotation service.
It stores sensitive values as versioned resources protected by IAM, resource policies, and audit logging.

- **Secrets** 
 Secrets are stored values such as database passwords, API keys, or tokens.
 They maintain metadata, versions, and staging labels within one secret resource.

- **Rotation** 
 Rotation is the automated replacement workflow for a secret value.
 It creates a new version and shifts staging labels according to the rotation lifecycle.

- **Resource policies** 
 Resource policies are JSON permission documents attached directly to a secret.
 They control which principals can retrieve or manage the secret across account scopes.

### AWS Systems Manager Parameter Store
Parameter Store is the hierarchical configuration and secret parameter storage component in Systems Manager.
It stores string and secure string values for runtime retrieval and configuration management.

- **Parameters** 
 Parameters are named configuration entries stored under a hierarchical path.
 They support versioning, labeling, and optional KMS encryption.

- **Parameter policies** 
 Parameter policies are lifecycle or validation controls for stored parameters.
 They define expiration, notification, or no-change monitoring behavior.

### AWS Certificate Manager
AWS Certificate Manager is the certificate lifecycle service for AWS-integrated endpoints.
It provisions, imports, validates, and renews TLS certificates for supported services.

- **Public certificates** 
 Public certificates are ACM-issued certificates for publicly resolvable domains.
 They require domain validation before issuance and auto-renew while in active use.

- **Private CA integration** 
 Private CA integration links ACM with AWS Private Certificate Authority.
 It enables issuance and lifecycle management of private certificates for internal trust domains.

- **Certificate validation** 
 Certificate validation is the proof-of-domain-control step required before certificate issuance.
 It is performed through DNS or email-based verification flows.

### AWS CloudTrail
AWS CloudTrail is the API activity logging service for AWS accounts and organizations.
It records control-plane and selected data-plane events for audit, security, and governance use cases.

- **Management events** 
 Management events are control-plane API operations that create, modify, or inspect AWS resources.
 They capture administrative actions such as instance launches, policy changes, and service configuration updates.

- **Data events** 
 Data events are high-volume records of object-level or resource-level operations on selected services.
 They capture actions such as S3 object access or Lambda invocation.

- **Insights** 
 CloudTrail Insights is anomaly detection over management event patterns.
 It identifies unusual API write or error rates relative to established baselines.

- **Organization trails** 
 Organization trails are CloudTrail configurations that apply across AWS Organizations accounts.
 They centralize event capture for multi-account governance.

### AWS Config
AWS Config is the resource inventory, configuration history, and compliance evaluation service.
It records supported resource state and evaluates that state against declarative rules.

- **Configuration recorder** 
 The configuration recorder is the component that tracks supported resource state changes.
 It captures configuration items and delivers them for storage and rule evaluation.

- **Rules** 
 Rules are compliance checks over resource configuration or relationships.
 They determine whether resources conform to expected policy or architecture requirements.

- **Conformance packs** 
 Conformance packs are grouped sets of Config rules and remediation metadata.
 They package related compliance controls into reusable governance bundles.

- **Aggregators** 
 Aggregators are centralized views of Config data across accounts and regions.
 They provide organization-wide visibility into inventory and compliance state.

### Amazon GuardDuty
Amazon GuardDuty is the threat detection service for AWS accounts, workloads, and data sources.
It analyzes telemetry such as CloudTrail, DNS, flow logs, and runtime signals to generate findings.

- **Detectors** 
 Detectors are the regional GuardDuty resources that enable threat analysis.
 They define the active detection scope and supported protection plans for an account.

- **Findings** 
 Findings are structured security alerts emitted by GuardDuty.
 They contain severity, resource context, attack-type classification, and evidence details.

- **Protection plans** 
 Protection plans are GuardDuty feature domains covering different telemetry and resource types.
 They extend detection to services such as S3, EKS, EC2 runtime, Lambda, or RDS login activity.

### AWS Security Hub
AWS Security Hub is the centralized security posture and finding aggregation service.
It normalizes findings from AWS and partner services into a common operational model.

- **Security standards** 
 Security standards are packaged control frameworks evaluated by Security Hub.
 They check resource and account state against benchmarked security requirements.

- **Findings aggregation** 
 Findings aggregation is the collection and normalization of security findings across sources, regions, and accounts.
 It consolidates alerts into a unified security operations view.

### Amazon Inspector
Amazon Inspector is the vulnerability assessment service for compute workloads and images.
It scans supported resources and reports package, network, and software exposure findings.

- **EC2 scanning** 
 EC2 scanning is vulnerability assessment for installed software packages on running instances.
 It identifies known CVEs based on observed package inventory and configuration.

- **Container image scanning** 
 Container image scanning is vulnerability assessment for images stored in ECR.
 It reports known package issues before or after deployment.

---

## Storage

AWS storage services provide object, file, block, archive, and hybrid access models.
They define how data is stored, replicated, mounted, versioned, protected, and transferred.

### Amazon S3
Amazon S3 is the object storage service for durable storage of unstructured data.
It stores data as objects in buckets with policy-driven lifecycle, replication, retention, and access control.

- **Buckets** 
 Buckets are globally named top-level containers for S3 objects and settings.
 They define namespace, policy, versioning, lifecycle, replication, and encryption behavior.

- **Objects** 
 Objects are immutable stored data units identified by a key within a bucket.
 They contain data, metadata, version state, and optional storage class attributes.

- **Storage classes** 
 Storage classes are durability and retrieval profiles for S3 objects.
 They vary by access pattern, retrieval latency, and storage or retrieval cost economics.

- **Versioning** 
 Versioning is the bucket feature that preserves multiple object versions under the same key.
 It creates version IDs for overwrite and delete operations to support recovery and auditability.

- **Lifecycle rules** 
 Lifecycle rules are automated object transition and expiration policies.
 They move objects across storage classes or delete them when configured age or filter conditions are met.

- **Object Lock** 
 Object Lock is the write-once-read-many retention control for object versions.
 It prevents deletion or overwrite for a retention period or legal hold state.

- **Replication** 
 Replication is the asynchronous copy mechanism for object versions between buckets.
 It transfers eligible data and metadata within-region or cross-region according to replication rules.

- **Multipart upload** 
 Multipart upload is the segmented upload protocol for large S3 objects.
 It uploads parts independently and assembles them into one final object after completion.

- **Pre-signed URLs** 
 Pre-signed URLs are time-limited signed request URLs for direct object access.
 They delegate read or write operations without distributing long-lived AWS credentials.

- **S3 Access Points** 
 S3 Access Points are named network and policy entry points into a bucket.
 They simplify per-application access control by attaching distinct policies and VPC restrictions.

- **S3 Object Lambda** 
 S3 Object Lambda is the object retrieval transformation feature for S3.
 It invokes Lambda during GET processing so applications receive transformed data instead of the stored object.

### Amazon EBS
Amazon EBS is the persistent block storage service for EC2.
It presents network-attached volumes that survive instance replacement and support snapshot-based backup.

- **General Purpose SSD** 
 General Purpose SSD is the balanced price-performance EBS volume category.
 It provides baseline and burstable IOPS for common transactional workloads.

- **Provisioned IOPS SSD** 
 Provisioned IOPS SSD is the high-performance EBS volume category for latency-sensitive workloads.
 It delivers explicitly provisioned IOPS and throughput targets.

- **Throughput Optimized HDD** 
 Throughput Optimized HDD is the EBS category tuned for large sequential workloads.
 It favors throughput over low-latency random access behavior.

- **Snapshots** 
 Snapshots are incremental point-in-time backups of EBS volumes stored durably in S3-backed snapshot infrastructure.
 They preserve block-level state for restore, cloning, and disaster recovery.

- **Encryption** 
 EBS encryption is the at-rest cryptographic protection feature for volumes and snapshots.
 It uses KMS-backed keys and applies transparently to data, I/O paths, and snapshot copies.

### Amazon EFS
Amazon EFS is the managed elastic NFS filesystem service.
It provides shared POSIX-compatible file access for multiple clients across Availability Zones.

- **File systems** 
 File systems are the top-level EFS storage resources.
 They expose a shared directory tree with managed scaling and durable storage semantics.

- **Mount targets** 
 Mount targets are zonal network endpoints through which clients access EFS.
 They place ENIs into subnets and provide IP-based entry points for NFS mounts.

- **Access points** 
 Access points are application-scoped entry views into an EFS filesystem.
 They enforce path restrictions and POSIX identity settings for mounted clients.

- **Performance modes** 
 Performance modes are filesystem tuning profiles for latency and throughput characteristics.
 They optimize internal behavior for general-purpose or high-throughput access patterns.

### Amazon FSx
Amazon FSx is the managed family of specialized file system services.
It provides AWS-operated deployments of protocol-specific or engine-specific filesystems.

- **FSx for Windows File Server** 
 FSx for Windows File Server is the managed SMB filesystem service based on Windows Server.
 It supports Active Directory integration, NTFS permissions, and Windows-native file semantics.

- **FSx for Lustre** 
 FSx for Lustre is the managed high-performance parallel filesystem service.
 It provides high-throughput shared storage optimized for compute-intensive workloads.

- **FSx for NetApp ONTAP** 
 FSx for NetApp ONTAP is the managed ONTAP-based storage service for file and block access.
 It exposes ONTAP data management features such as volumes, snapshots, and protocol flexibility.

- **FSx for OpenZFS** 
 FSx for OpenZFS is the managed OpenZFS-based filesystem service.
 It provides low-latency storage with ZFS-native features such as snapshots and cloning.

### AWS Storage Gateway
AWS Storage Gateway is the hybrid storage integration service between on-premises environments and AWS storage.
It presents local file, volume, or tape interfaces while storing data in AWS-backed services.

- **File Gateway** 
 File Gateway is the NFS and SMB file access mode of Storage Gateway.
 It stores files as objects in S3 while exposing a local file share interface.

- **Volume Gateway** 
 Volume Gateway is the iSCSI block storage mode of Storage Gateway.
 It presents cloud-backed block volumes locally with cached or stored data models.

- **Tape Gateway** 
 Tape Gateway is the virtual tape interface mode of Storage Gateway.
 It emulates physical tape workflows while storing backup data in AWS.

### AWS Backup
AWS Backup is the centralized backup policy and recovery orchestration service.
It applies plans, retention, vaulting, and restore workflows across supported AWS resource types.

- **Backup plans** 
 Backup plans are policy definitions for schedule, retention, and lifecycle behavior.
 They determine when backups run and how long recovery points are retained.

- **Backup vaults** 
 Backup vaults are logical storage containers for backup recovery points.
 They define encryption, access policy, and retention boundaries for protected copies.

- **Recovery points** 
 Recovery points are the restorable backup artifacts produced by AWS Backup.
 They represent point-in-time states that can be used for restore operations.

---

## Databases and Caching

AWS database services provide relational, key-value, document, graph, time-series, cache, and analytical storage engines.
They define consistency, replication, indexing, query patterns, and scaling boundaries for stateful applications.

### Amazon RDS
Amazon RDS is the managed relational database service for multiple SQL engines.
It automates instance provisioning, patching, backup, failover, and routine operational maintenance.

- **DB instances** 
 DB instances are the primary compute and storage resources that run RDS database engines.
 They expose database endpoints and encapsulate engine, storage, and network configuration.

- **Read replicas** 
 Read replicas are asynchronously replicated secondary database instances.
 They offload read traffic and can support promotion for recovery scenarios.

- **Multi-AZ** 
 Multi-AZ is the high-availability deployment model for synchronous standby replication.
 It maintains redundant infrastructure in another Availability Zone for failover continuity.

- **Parameter groups** 
 Parameter groups are engine configuration collections applied to instances or clusters.
 They define runtime database settings independently of the compute resource.

- **Option groups** 
 Option groups are engine-specific feature bundles for selected RDS engines.
 They enable optional extensions or integrations outside the core parameter model.

- **Snapshots** 
 Snapshots are point-in-time backups of an RDS instance.
 They capture persistent storage state for restore and cloning operations.

- **RDS Proxy** 
 RDS Proxy is the managed connection pooling layer for RDS and Aurora.
 It multiplexes application connections and improves resilience during failover or burst connection events.

### Amazon Aurora
Amazon Aurora is the AWS-managed relational database engine compatible with MySQL and PostgreSQL.
It separates compute from distributed storage and supports clustered read and write endpoints.

- **DB clusters** 
 DB clusters are Aurora database groupings that share one distributed storage volume.
 They contain a writer instance and zero or more reader instances under one cluster identity.

- **Reader endpoints** 
 Reader endpoints are cluster-level endpoints that load-balance across Aurora read replicas.
 They route read-only traffic without exposing individual replica addresses.

- **Global Database** 
 Global Database is Aurora’s cross-region replication topology.
 It links one primary region to secondary region clusters using low-lag storage replication.

- **Serverless v2** 
 Serverless v2 is Aurora’s elastic compute model.
 It adjusts database compute capacity incrementally while preserving endpoint and storage continuity.

### Amazon DynamoDB
Amazon DynamoDB is the managed key-value and document database service.
It distributes data across partitions and provides low-latency access with managed throughput and indexing.

- **Tables** 
 Tables are the primary DynamoDB data containers keyed by partition key and optional sort key.
 They store items as schema-flexible attribute maps under one capacity and indexing configuration.

- **Partitions** 
 Partitions are the internal storage and throughput units used by DynamoDB.
 Data placement and request scaling are driven by partition key distribution, size, and throughput limits.

- **Global Secondary Indexes** 
 Global Secondary Indexes are alternate query indexes with independent partition and sort keys.
 They support access patterns not served by the base table key schema.

- **Local Secondary Indexes** 
 Local Secondary Indexes are alternate sort-key indexes sharing the base table partition key.
 They enable additional query ordering within the same partition key domain.

- **Streams** 
 DynamoDB Streams is the ordered change data capture stream for table mutations.
 It emits insert, update, and delete records for downstream event processing.

- **TTL** 
 Time to Live is the timestamp-based item expiration mechanism.
 It marks items for asynchronous deletion after a configured epoch time.

- **On-demand capacity** 
 On-demand capacity is the request-driven throughput mode for DynamoDB.
 It scales automatically with traffic and bills per read and write request usage.

- **Provisioned capacity** 
 Provisioned capacity is the pre-allocated throughput mode for DynamoDB.
 It reserves read and write capacity units and supports optional autoscaling.

- **Global tables** 
 Global tables are multi-region active-active replicas of a DynamoDB table.
 They replicate item changes across regions for low-latency regional access and resilience.

- **DAX** 
 DynamoDB Accelerator is the in-memory caching layer for DynamoDB.
 It reduces read latency by serving item and query results from a managed cache cluster.

### Amazon ElastiCache
Amazon ElastiCache is the managed in-memory caching service for supported engines.
It provides low-latency key-based access with AWS-managed patching, scaling, and node operations.

- **Redis or Valkey** 
 Redis or Valkey is the in-memory data structure engine offered by ElastiCache.
 It supports key-value access, expiry, replication, and richer in-memory data types.

- **Memcached** 
 Memcached is the distributed in-memory cache engine offered by ElastiCache.
 It provides simple key-value caching without persistence or replication semantics.

- **Replication groups** 
 Replication groups are ElastiCache deployment topologies for Redis-compatible engines.
 They define primary-replica relationships, failover behavior, and optional sharding.

- **Parameter groups** 
 Parameter groups are engine runtime configuration sets for cache clusters.
 They control server behavior independently of application-level APIs.

### Amazon Redshift
Amazon Redshift is the managed columnar analytical data warehouse service.
It executes distributed SQL over storage and compute optimized for large-scale analytics.

- **Clusters** 
 Clusters are the provisioned compute deployments for Redshift.
 They define node composition, networking, and query execution boundaries.

- **RA3 nodes** 
 RA3 nodes are Redshift compute nodes with managed storage separation.
 They decouple local compute from scalable storage for more elastic warehouse sizing.

- **Redshift Serverless** 
 Redshift Serverless is the on-demand compute model for Redshift workloads.
 It allocates query processing capacity automatically without cluster provisioning.

- **Spectrum** 
 Redshift Spectrum is the federated query capability for data stored in S3.
 It extends SQL execution beyond local Redshift storage through external schema metadata.

### Amazon DocumentDB
Amazon DocumentDB is the managed document database service compatible with MongoDB APIs.
It stores JSON-like documents and separates compute instances from a shared storage layer.

### Amazon Neptune
Amazon Neptune is the managed graph database service for property graph and RDF workloads.
It supports graph traversals and relationship-centric queries using graph-specific query languages.

### Amazon Timestream
Amazon Timestream is the managed time-series database service for timestamped event data.
It optimizes ingestion, retention, and query execution for time-ordered metrics and telemetry.

---

## Observability and Operations

AWS observability and management services collect metrics, logs, traces, inventory, and automation state.
They define how systems are monitored, patched, queried, and provisioned over time.

### Amazon CloudWatch
Amazon CloudWatch is the monitoring and observability service for AWS metrics, logs, and alarms.
It ingests telemetry and evaluates it for dashboards, alerting, anomaly detection, and automation.

- **Metrics** 
 Metrics are time-series values published by AWS services or custom applications.
 They represent measured quantities over time and support aggregation, math, and alerting.

- **Logs** 
 Logs are timestamped event records stored in CloudWatch Logs.
 They are organized into log groups and streams for retention, querying, and subscription delivery.

- **Alarms** 
 Alarms are threshold or rule evaluations over metrics or expressions.
 They transition state and can trigger notifications or automated responses.

- **Dashboards** 
 Dashboards are configurable visual layouts of metric and alarm widgets.
 They present multi-service operational state in a centralized view.

- **Metric math** 
 Metric math is the expression engine for deriving new CloudWatch metric series.
 It combines multiple metrics into computed values for dashboards and alarms.

- **Synthetics** 
 CloudWatch Synthetics is the scripted monitoring feature for endpoints and user journeys.
 It runs scheduled canaries that test API or browser workflows from managed environments.

- **Contributor Insights** 
 Contributor Insights is the analytics feature for identifying high-cardinality contributors in logs or metrics.
 It aggregates patterns to reveal top sources driving system behavior.

### AWS X-Ray
AWS X-Ray is the distributed tracing service for request path and latency analysis.
It records trace segments and service relationships across instrumented applications.

- **Traces** 
 Traces are end-to-end records of a request moving through distributed components.
 They connect timing and metadata from multiple services into one logical execution path.

- **Segments** 
 Segments are trace components representing work done by one service or resource.
 They contain timing, annotations, exceptions, and downstream call information.

- **Sampling rules** 
 Sampling rules are policies that decide which requests are traced.
 They control trace volume through rate and reservoir settings.

### AWS Systems Manager
AWS Systems Manager is the management and automation service for AWS and hybrid compute nodes.
It centralizes configuration, command execution, patching, inventory, and operational workflows.

- **Parameter Store** 
 Parameter Store is the hierarchical configuration storage component of Systems Manager.
 It stores versioned plaintext or encrypted values retrievable at runtime.

- **Session Manager** 
 Session Manager is the secure shell and remote session feature for managed nodes.
 It provides interactive access without requiring inbound SSH or bastion exposure.

- **Run Command** 
 Run Command is the remote command execution feature for managed instances.
 It dispatches scripts or commands and collects execution status and output.

- **Patch Manager** 
 Patch Manager is the patch orchestration feature for managed nodes.
 It applies approved OS patches according to maintenance windows and compliance settings.

- **Automation** 
 Automation is the runbook execution engine within Systems Manager.
 It sequences API calls and remediation steps using versioned documents.

- **Inventory** 
 Inventory is the metadata collection feature for managed instances.
 It gathers software, configuration, and system details into queryable records.

- **Maintenance Windows** 
 Maintenance Windows are scheduled execution boundaries for operational tasks.
 They control when automation, patching, or command execution is allowed to run.

### AWS CloudFormation
AWS CloudFormation is the declarative infrastructure provisioning service for AWS resources.
It applies template-defined desired state through stack-based lifecycle management.

- **Stacks** 
 Stacks are deployed instances of a CloudFormation template.
 They create, update, and delete grouped resources as one managed unit.

- **Stack sets** 
 Stack sets are multi-account and multi-region deployment controllers for stacks.
 They propagate a common template across selected organizational targets.

- **Change sets** 
 Change sets are previews of proposed stack modifications.
 They show which resources will be created, modified, or replaced before execution.

- **Drift detection** 
 Drift detection is the comparison process between template-defined state and actual deployed state.
 It identifies resources that have diverged from CloudFormation management.

### AWS CloudShell
AWS CloudShell is the browser-based shell environment for AWS account operations.
It provides authenticated CLI access and persistent home directory storage within a managed shell session.

---

## Integration and Messaging

AWS integration services provide queues, topics, event buses, workflow orchestration, and streaming connectors.
They define asynchronous communication, fan-out delivery, event matching, and stateful process execution.

### Amazon SQS
Amazon SQS is the managed message queue service for decoupled asynchronous processing.
It stores messages durably until consumers receive and delete them.

- **Standard queues** 
 Standard queues are the default SQS queue type with high throughput and at-least-once delivery.
 They provide best-effort ordering and may deliver duplicate messages.

- **FIFO queues** 
 FIFO queues are the ordered SQS queue type with exactly-once processing semantics within queue guarantees.
 They preserve message group ordering and support deduplication logic.

- **Dead-letter queues** 
 Dead-letter queues are secondary queues for messages that exceed retry thresholds.
 They isolate failed messages for analysis, replay, or remediation.

- **Visibility timeout** 
 Visibility timeout is the temporary invisibility period after a consumer receives a message.
 It prevents concurrent processing while the message remains unacknowledged.

- **Long polling** 
 Long polling is the receive behavior that waits for messages before returning a response.
 It reduces empty responses and unnecessary API polling overhead.

### Amazon SNS
Amazon SNS is the publish-subscribe messaging service for fan-out event delivery.
It replicates published messages to multiple subscribed endpoints or services.

- **Topics** 
 Topics are the logical publish endpoints in SNS.
 They receive messages and distribute them to attached subscriptions.

- **Subscriptions** 
 Subscriptions are protocol-specific delivery bindings attached to a topic.
 They define destination type and endpoint for incoming topic messages.

- **FIFO topics** 
 FIFO topics are ordered SNS topics for compatible FIFO delivery paths.
 They preserve ordering and deduplication semantics for supported subscribers.

- **Filter policies** 
 Filter policies are subscription-level message selection rules.
 They deliver only messages whose attributes match configured conditions.

### Amazon EventBridge
Amazon EventBridge is the event bus and routing service for AWS and custom event sources.
It matches events against rules and forwards them to configured targets.

- **Event buses** 
 Event buses are routing containers for events in EventBridge.
 They receive events and serve as the evaluation scope for rules.

- **Rules** 
 Rules are event pattern or schedule matchers attached to an event bus.
 They select events and invoke one or more targets when conditions are met.

- **Scheduler** 
 EventBridge Scheduler is the time-based invocation service in the EventBridge family.
 It triggers API calls or target invocations according to schedules and retry policies.

- **Pipes** 
 Pipes are point-to-point integrations between an event source and a target.
 They optionally apply filtering, enrichment, and transformation in transit.

- **Schema Registry** 
 Schema Registry is the event schema discovery and storage component in EventBridge.
 It captures event structure and supports typed code binding generation.

### AWS Step Functions
AWS Step Functions is the stateful workflow orchestration service for AWS applications.
It executes JSON-defined state machines with branching, retries, waits, and service integrations.

- **Standard workflows** 
 Standard workflows are durable long-running state machine executions.
 They retain detailed execution history and provide exactly-once state progression semantics.

- **Express workflows** 
 Express workflows are high-throughput short-duration state machine executions.
 They trade long execution retention for lower cost and higher event-processing scale.

- **Map state** 
 Map state is the iterator state type in Step Functions.
 It runs nested logic over multiple input items with configurable concurrency behavior.

- **Service integrations** 
 Service integrations are native task bindings between Step Functions and AWS APIs.
 They invoke supported services directly without requiring intermediary Lambda functions.

### Amazon MQ
Amazon MQ is the managed message broker service for Apache ActiveMQ and RabbitMQ.
It provides broker-compatible protocols for applications requiring traditional queue or topic semantics.

### Amazon MSK
Amazon MSK is the managed Apache Kafka service for streaming data platforms.
It provisions broker infrastructure, storage, and networking for Kafka-compatible producers and consumers.

- **Provisioned clusters** 
 Provisioned clusters are fixed-capacity Kafka deployments on MSK-managed brokers.
 They expose broker endpoints and user-configured storage and instance sizing.

- **Serverless** 
 MSK Serverless is the on-demand compute model for Kafka workloads.
 It scales broker capacity automatically without cluster infrastructure management.

---

## Analytics and Data Engineering

AWS analytics services provide query engines, ETL, stream processing, search, and distributed compute backends.
They define how datasets are cataloged, transformed, queried, streamed, and indexed.

### Amazon Athena
Amazon Athena is the serverless SQL query service for data stored in S3.
It executes queries over external datasets using schema metadata from AWS Glue or compatible catalogs.

- **Workgroups** 
 Workgroups are administrative and cost-control boundaries for Athena query execution.
 They separate settings, output locations, encryption requirements, and usage tracking.

- **Prepared statements** 
 Prepared statements are parameterized SQL definitions stored in Athena.
 They support repeatable execution of templated queries with bound values.

### AWS Glue
AWS Glue is the managed data integration and metadata service for ETL and analytics pipelines.
It provides schema cataloging, discovery, transformation jobs, and workflow orchestration.

- **Data Catalog** 
 The Data Catalog is Glue’s centralized metadata repository for tables, schemas, and partitions.
 It stores queryable dataset definitions used by Athena, Glue, EMR, and other analytics services.

- **Crawlers** 
 Crawlers are metadata discovery jobs that inspect data stores and infer schema.
 They create or update catalog tables based on detected structure and partition layout.

- **Jobs** 
 Jobs are managed ETL execution units in Glue.
 They run transformation code on Spark-based infrastructure to process datasets.

- **Workflows** 
 Workflows are orchestration graphs connecting crawlers, jobs, and triggers.
 They sequence multi-step data integration pipelines under one managed control flow.

- **Triggers** 
 Triggers are execution conditions that start Glue jobs or crawlers.
 They can be schedule-based, event-based, or dependency-based.

### Amazon EMR
Amazon EMR is the managed cluster and serverless data processing service for big data frameworks.
It provisions execution backends for Spark, Hadoop, Hive, and related distributed analytics engines.

- **EMR on EC2** 
 EMR on EC2 is the cluster-based deployment mode for EMR.
 It gives direct control over node composition, scaling, bootstrap actions, and framework configuration.

- **EMR Serverless** 
 EMR Serverless is the on-demand execution mode for Spark and Hive workloads.
 It allocates compute for submitted jobs without persistent cluster management.

- **EMR on EKS** 
 EMR on EKS is the EMR execution model that runs analytics jobs on EKS-managed Kubernetes capacity.
 It maps EMR jobs into containerized execution within EKS clusters.

### Amazon Kinesis
Amazon Kinesis is the managed streaming platform for ingestion, delivery, and real-time processing.
It provides durable event streams and managed delivery paths for continuous data flows.

- **Kinesis Data Streams** 
 Kinesis Data Streams is the durable append-only streaming service in Kinesis.
 It stores ordered records in shards for low-latency producer and consumer processing.

- **Kinesis Data Firehose** 
 Kinesis Data Firehose is the managed streaming delivery service.
 It buffers, transforms, and writes streaming records to destinations such as S3, Redshift, or OpenSearch.

- **Kinesis Data Analytics** 
 Kinesis Data Analytics is the stream-processing execution layer for SQL or application logic over streaming inputs.
 It continuously computes derived results from in-flight event data.

### Amazon OpenSearch Service
Amazon OpenSearch Service is the managed search and log analytics service for OpenSearch clusters.
It provisions nodes, storage, indexing endpoints, and management features for full-text and analytics workloads.

- **Domains** 
 Domains are managed OpenSearch cluster deployments within the service.
 They define node topology, storage, networking, and access policy configuration.

- **Indexes** 
 Indexes are logical data structures storing documents for search and aggregation.
 They define mappings, shards, and analyzers that affect query behavior.

### AWS Lake Formation
AWS Lake Formation is the governance and access control layer for data lakes on AWS.
It centralizes permissions, table metadata integration, and fine-grained access over lake datasets.

---

## AI and Machine Learning

AWS AI and ML services provide managed development, training, model registry, feature storage, and inference hosting.
They define machine learning lifecycle resources from experimentation to production prediction.

### Amazon SageMaker
Amazon SageMaker is the managed machine learning platform for building, training, and deploying models.
It provides integrated services for notebooks, training jobs, pipelines, model registry, and endpoints.

- **Studio** 
 Studio is the web-based development environment for SageMaker workflows.
 It centralizes notebooks, jobs, pipelines, artifacts, and model operations in one workspace.

- **Notebook instances** 
 Notebook instances are managed Jupyter-based compute environments for interactive ML development.
 They provide persistent development machines with attached storage and IAM-integrated access.

- **Training jobs** 
 Training jobs are managed compute executions that fit models using supplied code and input data.
 They provision ephemeral infrastructure and output trained model artifacts on completion.

- **Processing jobs** 
 Processing jobs are managed compute executions for preprocessing, postprocessing, and evaluation tasks.
 They run containerized code on ephemeral infrastructure outside the training lifecycle.

- **Endpoints** 
 Endpoints are online inference resources for hosted models.
 They expose stable invocation URLs backed by one or more deployed model containers.

- **Batch Transform** 
 Batch Transform is the offline inference mode for large datasets.
 It reads input from storage, processes records asynchronously, and writes results back to storage.

- **Pipelines** 
 Pipelines are declarative ML workflow graphs in SageMaker.
 They sequence preprocessing, training, evaluation, and registration steps with artifact dependencies.

- **Feature Store** 
 Feature Store is the managed repository for machine learning feature values.
 It stores and serves curated feature data for training and online inference use cases.

- **Model Registry** 
 Model Registry is the versioned catalog of model packages and metadata.
 It tracks approval state, lineage, and promotion readiness for deployment.

- **Experiments** 
 Experiments are tracking structures for runs, parameters, metrics, and artifacts.
 They preserve comparative training history across iterative development cycles.

---

## Migration and Transfer

AWS migration services move servers, databases, files, storage, and application connectivity into AWS.
They define replication, synchronization, modernization, and hybrid data movement workflows.

### AWS Application Migration Service
AWS Application Migration Service is the block-level server replication and cutover service for lift-and-shift migration.
It continuously replicates source servers into AWS and launches converted EC2 instances for cutover.

### AWS Database Migration Service
AWS DMS is the managed database migration and replication service.
It moves data from source engines to target stores with optional ongoing change data capture.

- **Replication instances** 
 Replication instances are the managed compute resources that run DMS migration tasks.
 They handle source reads, transformation steps, and target writes during migration.

- **Endpoints** 
 Endpoints are source and target connection definitions in DMS.
 They store engine-specific connectivity settings and authentication information.

- **Migration tasks** 
 Migration tasks are the executable units that move data between endpoints.
 They define full load, CDC, or combined migration behavior.

### AWS DataSync
AWS DataSync is the managed data transfer service for file and object datasets.
It accelerates online movement between on-premises storage, AWS storage, and supported cloud endpoints.

- **Agents** 
 Agents are virtual appliances deployed near source or destination storage.
 They perform optimized data transfer for protocols requiring local connectivity.

- **Tasks** 
 Tasks are reusable transfer configurations in DataSync.
 They define source, destination, filters, schedule, and transfer options.

### AWS Transfer Family
AWS Transfer Family is the managed file transfer endpoint service for SFTP, FTPS, and FTP.
It exposes managed protocol endpoints backed by AWS storage services.

- **Servers** 
 Servers are managed protocol endpoints in Transfer Family.
 They terminate client file-transfer sessions and map them to backing storage.

- **Users** 
 Users are identity records associated with a Transfer Family server.
 They define authentication, home directory mapping, and protocol access behavior.

---

## Governance, Multi-Account, and Cost

AWS governance services organize accounts, enforce guardrails, standardize environments, and analyze spend.
They define administrative boundaries, landing zones, recommendation engines, and cost monitoring controls.

### AWS Organizations
AWS Organizations is the multi-account governance service for centralized account administration.
It groups accounts hierarchically and applies organization-level policies and billing relationships.

- **Accounts** 
 Accounts are isolated administrative, security, and billing boundaries in AWS.
 They contain resources, IAM configuration, quotas, and region-specific service state.

- **Organizational Units** 
 Organizational Units are hierarchy nodes used to group AWS accounts.
 They provide inheritance boundaries for policies and governance structure.

- **Service Control Policies** 
 Service Control Policies are organization-level permission guardrails.
 They limit the maximum actions principals in member accounts can perform.

- **Tag policies** 
 Tag policies are governance rules for standardizing resource tagging across accounts.
 They define allowed or expected tag keys and value formats for supported services.

### AWS Control Tower
AWS Control Tower is the landing zone orchestration service built on AWS Organizations.
It automates baseline account structure, controls, logging, and account vending.

- **Landing zone** 
 A landing zone is the baseline multi-account environment configured by Control Tower.
 It establishes shared accounts, logging, identity, and governance defaults.

- **Guardrails** 
 Guardrails are preventive or detective controls managed through Control Tower.
 They enforce or monitor compliance through SCPs and Config-based rules.

- **Account Factory** 
 Account Factory is the standardized account provisioning workflow in Control Tower.
 It creates new accounts with predefined placement and baseline configuration.

### AWS Trusted Advisor
AWS Trusted Advisor is the recommendation service for cost, performance, resilience, security, and service quotas.
It evaluates account state and produces check-based optimization findings.

### AWS Cost Explorer
AWS Cost Explorer is the spend analysis and forecasting service for AWS billing data.
It queries historical and projected cost and usage dimensions for reporting and optimization.

### AWS Budgets
AWS Budgets is the threshold-based spend and usage monitoring service.
It tracks actual or forecasted consumption against configured limits and emits notifications.

### AWS Cost and Usage Report
The Cost and Usage Report is the detailed billing export for AWS consumption.
It delivers line-item cost and usage data to S3 for downstream analytics and chargeback workflows.

### AWS Resource Access Manager
AWS Resource Access Manager is the cross-account resource sharing service in AWS.
It exposes supported resources such as subnets or transit gateways to other accounts without duplication.

---
## Notes on Scope

These notes are optimized for **technical clarity**, **certification revision**, and **interview prep**.
They are intentionally service-centric, implementation-focused, and written to distinguish **service**, **resource**, **feature**, and **control mechanism** as cleanly as possible.
