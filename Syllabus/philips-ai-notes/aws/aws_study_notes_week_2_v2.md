# AWS Study Notes — Week 2 v2
## Networking, Private Connectivity, and Edge Traffic

> This document is the **deep technical version** of Week 2.  
> It is written as a **service-by-service study guide** with:
> - technical explanations
> - key entities
> - architecture usage
> - examples
> - best practices
> - common mistakes
> - troubleshooting cues
> - official AWS references  
>
> The goal is not just to memorize service names, but to understand **how AWS networking actually works in real architectures**.

---

# 1. Why Week 2 Matters

AWS networking is the backbone of almost every cloud architecture. Compute, storage, databases, security, migration, observability, and application delivery all depend on network design decisions.

If Week 1 created the **governance and account structure**, Week 2 creates the **traffic and connectivity model**.

A strong Week 2 understanding lets you answer questions like:

- Where does this workload live?
- How does traffic enter AWS?
- How do private workloads reach AWS services safely?
- How do multiple VPCs communicate without creating a full mesh?
- How do users reach internal apps?
- How does DNS work in a hybrid AWS environment?
- Where do DDoS, WAF, firewall inspection, and edge acceleration fit?
- How do I troubleshoot “it cannot connect” in AWS?

The central Week 2 idea is:

> **AWS networking is a layered control system across IP space, route selection, security controls, service exposure, private service access, hybrid connectivity, DNS, and edge entry.**

---

# 2. Week 2 Learning Outcomes

By the end of this week, you should be able to:

- explain every major **Amazon VPC** networking entity technically
- design public, private, and isolated subnet layouts
- explain route tables, IGWs, NAT gateways, and IPv6 egress models
- distinguish **security groups**, **NACLs**, **Network Firewall**, and **WAF**
- explain **VPC peering**, **Transit Gateway**, and **Cloud WAN**
- choose among **gateway endpoints**, **interface endpoints**, and **PrivateLink**
- explain **VPC Lattice** and **Verified Access** in modern application networking
- choose among **ALB**, **NLB**, and **GWLB**
- explain **TLS certificate management** with ACM
- explain **Site-to-Site VPN**, **Client VPN**, and **Direct Connect**
- explain **Route 53**, **Resolver**, **DNS Firewall**, and **DNSSEC**
- explain **CloudFront**, **Shield**, **WAF**, **Global Accelerator**, and **API Gateway**
- troubleshoot connectivity systematically using AWS-native tools

---

# 3. Architectural Mental Model for Week 2

Think about AWS networking in layers:

1. **Addressing layer**  
   CIDRs, subnets, ENIs, IPAM

2. **Reachability layer**  
   Route tables, IGW, NAT, peering, TGW, Cloud WAN, VPN, DX

3. **Security layer**  
   SGs, NACLs, Network Firewall, DNS Firewall, WAF, Shield

4. **Private service access layer**  
   VPC endpoints, PrivateLink, Lattice

5. **Application delivery layer**  
   ALB, NLB, GWLB, API Gateway, CloudFront, Global Accelerator

6. **Name resolution layer**  
   Route 53, private hosted zones, Resolver, forwarding

7. **Analysis and troubleshooting layer**  
   Flow Logs, Reachability Analyzer, Network Access Analyzer

When network issues occur, at least one of these layers is usually wrong.

---

# 4. Amazon VPC

Amazon VPC is the foundational networking service in AWS. It provides a **logically isolated virtual network** where you define IP ranges, subnet segmentation, route behavior, and attachment points for AWS resources.

## 4.1 VPC

### Technical purpose
A VPC creates an isolated L3 network boundary inside a Region.

### Key technical characteristics
- regional construct
- supports IPv4 and optionally IPv6
- contains subnets, route tables, SGs, NACLs, gateways, endpoints, and attachments
- controls network segmentation for workloads

### Example
A production application VPC may have:
- 3 public subnets
- 3 private app subnets
- 3 isolated DB subnets
- one TGW attachment
- S3 gateway endpoint
- CloudWatch and Systems Manager interface endpoints

### Best practices
- use non-overlapping CIDRs
- design VPCs for clear purpose, not random accumulation
- separate workloads by account/VPC when blast radius matters
- avoid putting unrelated systems into one giant VPC unless there is a clear reason

### Common mistakes
- choosing overlapping CIDRs that later block peering/TGW/hybrid routing
- mixing production and sandbox workloads in one VPC
- designing VPCs without growth headroom

---

## 4.2 CIDR blocks

### IPv4 CIDR
Defines the private IPv4 address space for the VPC.

### IPv6 CIDR
Enables dual-stack or IPv6-native networking.

### Technical importance
CIDR planning affects:
- subnet sizing
- future scaling
- peering compatibility
- TGW routing
- hybrid route summarization

### Example
A company uses:
- `10.0.0.0/16` for shared services
- `10.10.0.0/16` for prod applications
- `10.20.0.0/16` for non-prod  
This is better than random overlapping `/24`s created without a plan.

### Best practices
- reserve space by environment and account family
- align ranges with future TGW and on-prem route policy
- use IPAM for large estates

### Common mistakes
- allocating tiny CIDRs that later limit container/EKS growth
- assigning IPv6 without designing SG/NACL/egress behavior

---

## 4.3 Subnets

A subnet is an AZ-scoped slice of the VPC CIDR.

### Technical role
- placement boundary for ENIs
- route table association point
- NACL association point
- basis for public/private/isolated design

### Types of subnet patterns

#### Public subnet
A subnet with a route to an IGW and resources capable of public reachability.

Typical resources:
- internet-facing ALB
- NAT gateway
- legacy bastion host

#### Private subnet
No direct route to IGW for application resources.

Typical resources:
- app servers
- ECS tasks
- EKS nodes
- internal services

#### Isolated subnet
No internet route at all.

Typical resources:
- databases
- strict internal processing tiers
- internal control-plane components

### Example
Three-tier web app:
- public subnets: ALB + NAT gateways
- private app subnets: ECS service
- isolated DB subnets: Aurora cluster

### Best practices
- align subnets across multiple AZs
- keep subnet purpose clear
- do not treat public/private labels as just names; validate route behavior

### Common mistakes
- putting databases in subnets with internet paths
- using only one AZ for critical subnet tiers

---

## 4.4 Route tables

A route table contains destination prefixes and next-hop targets.

### Technical role
Route tables decide where traffic goes.

Possible targets include:
- local route
- internet gateway
- egress-only internet gateway
- NAT gateway
- VPC peering connection
- transit gateway
- gateway endpoint
- virtual private gateway in some patterns
- ENI or appliance target in specialized cases

### Key idea
A subnet is “public” or “private” largely because of its associated route table.

### Example
Private app subnet routes:
- local VPC CIDR → local
- `0.0.0.0/0` → NAT gateway
- `10.100.0.0/16` → Transit Gateway
- S3 prefix list → gateway endpoint

### Best practices
- document route intent by subnet tier
- minimize route sprawl
- validate symmetric routing in inspection designs

### Common mistakes
- forgetting return paths in multi-hop or appliance-based designs
- assuming route propagation alone solves TGW segmentation cleanly

---

## 4.5 Internet gateway (IGW)

The IGW is the VPC attachment that enables internet routing for IPv4 public traffic.

### Technical role
- provides path to and from the internet
- works with public route tables and public IP addressing

### Important note
Having an IGW attached does **not** make a resource public by itself.  
The subnet route, public IP assignment, and SG/NACL rules also matter.

### Example
An ALB in public subnets has:
- subnet route to IGW
- public-facing scheme
- DNS name publicly resolvable

### Common mistakes
- attaching IGW and assuming all subnets are public
- placing instances in IGW-routed subnets unintentionally

---

## 4.6 Egress-only internet gateway

Used for outbound-only IPv6 internet access.

### Technical role
- allows IPv6 egress
- blocks unsolicited inbound IPv6 connections initiated from the internet

### Best practice
If using IPv6 and you want private outbound-only behavior, use egress-only IGW rather than trying to copy IPv4 NAT assumptions.

---

## 4.7 NAT gateway

A NAT gateway enables outbound internet access for IPv4 private subnet resources without direct inbound internet reachability.

### Technical role
- performs source NAT
- sits in a public subnet
- typically uses an Elastic IP
- private subnets route `0.0.0.0/0` to it

### Example
A private EC2 instance downloads OS updates through a NAT gateway.

### Best practices
- deploy NAT gateways per AZ for resilient architectures
- avoid forcing cross-AZ private egress if high availability matters
- reduce NAT dependency using gateway/interface endpoints where possible

### Common mistakes
- one NAT gateway for all AZs in production-critical designs
- using NAT for AWS service access when endpoints would be safer and cheaper
- forgetting NAT is IPv4-focused; IPv6 egress patterns differ

---

## 4.8 Elastic network interface (ENI)

An ENI is the actual network interface attached to a resource.

### Technical role
ENIs carry:
- private IP addresses
- associated security groups
- MAC identity
- source/destination check behavior in some cases

### Why ENIs matter
Many AWS networking behaviors are really ENI-centric:
- SG enforcement
- Flow Logs on ENIs
- interface endpoints
- ALB/NLB target registration contexts in some cases
- attachment troubleshooting

### Example
An interface VPC endpoint creates endpoint ENIs in selected subnets.

---

## 4.9 Elastic IP (EIP)

A static public IPv4 address.

### Technical use cases
- NAT gateway
- certain public-facing EC2/NLB patterns

### Best practices
- keep use limited and intentional
- release unused EIPs
- prefer managed DNS-based exposure where possible instead of hard dependence on individual public IPs

---

## 4.10 Prefix lists

Prefix lists are reusable groups of CIDR entries.

### Technical value
- simplify SG and route references
- reduce manual repetition
- support centralized management of common ranges

### Example
Use a managed prefix list for corporate office CIDRs in SG inbound rules.

---

## 4.11 DHCP option sets

DHCP option sets control DHCP-provided values like DNS server settings and domain name behavior.

### Why it matters
In hybrid DNS environments, DHCP option sets can influence how instances resolve names and integrate with enterprise DNS models.

---

# 5. Security Groups and Network ACLs

These are often confused. They solve different problems.

## 5.1 Security groups (SGs)

A security group is a **stateful virtual firewall** applied to ENIs/resources.

### Technical behavior
- evaluated at ENI/resource boundary
- inbound and outbound rules
- allow rules only
- return traffic automatically allowed if state is established

### Design use
SGs are the primary network filter for most AWS workloads.

### Example
- ALB SG allows inbound 443 from internet
- app SG allows inbound 443 only from ALB SG
- DB SG allows inbound 3306 only from app SG

This is better than allowing from broad CIDRs.

### Best practices
- reference other SGs where possible
- keep rules purpose-driven
- separate app tiers by SG, not just subnet
- avoid giant shared SGs for unrelated workloads

### Common mistakes
- using SGs as if they can explicitly deny
- allowing `0.0.0.0/0` broadly “just to make it work”
- reusing one SG across too many unrelated systems

---

## 5.2 Network ACLs (NACLs)

A NACL is a **stateless subnet-level filter**.

### Technical behavior
- attached to subnets
- ordered numbered rules
- supports allow and deny
- return traffic must be explicitly allowed

### Good uses
- coarse subnet-wide deny patterns
- additional control layer
- centralized subnet edge filtering

### Example
A restricted subnet uses a NACL to deny known unwanted CIDRs while SGs manage normal app-level controls.

### Best practices
- use NACLs only where they add real value
- document ephemeral port requirements
- remember they affect every ENI in the subnet

### Common mistakes
- blocking return traffic accidentally
- using NACLs as the primary fine-grained app control
- making rule sets too complex to operate safely

---

## 5.3 SG vs NACL summary

- **SG**: stateful, ENI-scoped, allow only
- **NACL**: stateless, subnet-scoped, allow + deny

In practice:
- use SGs for most application control
- use NACLs for subnet-level guardrails where justified

---

# 6. VPC Flow Logs, Reachability Analyzer, Network Access Analyzer, and IPAM

## 6.1 VPC Flow Logs

Flow Logs capture network flow metadata for traffic to/from ENIs, subnets, or the VPC.

### What they provide
- source IP
- destination IP
- source/destination ports
- protocol
- accepted/rejected status
- bytes and packets

### What they do not provide
- full packet payload
- application-layer request content

### Example
An application cannot connect to RDS. Flow Logs show `REJECT` traffic from app subnet ENIs toward the DB port, indicating network-layer blocking.

### Best practices
- enable strategically for key environments
- centralize log analysis
- use during security investigations and connectivity triage

### Common mistakes
- expecting packet payload inspection
- enabling without log retention/analysis plan

---

## 6.2 Reachability Analyzer

A static analysis tool for AWS network connectivity.

### Technical role
It evaluates whether a source can reach a destination based on the configured network path.

### It considers
- routes
- gateways
- SGs
- NACLs
- attachment path
- ENIs

### Example
You test reachability from an app instance ENI to an RDS ENI and the analyzer points to a blocking SG rule.

### Best practices
- use for deterministic path debugging
- use before escalating “AWS networking is broken”
- use for planned validation as well as troubleshooting

### Common mistakes
- assuming it shows live packet captures
- not choosing the right source/destination objects

---

## 6.3 Network Access Analyzer

This tool validates network configurations against intended access rules.

### Technical role
It is more posture-oriented than point-to-point troubleshooting.

### Example
Define a policy that says:
- no database subnet should be internet reachable
- no private workload should be exposed via IGW

The analyzer finds violations.

### Best practices
- use it for compliance and guardrail checking
- define reusable access scopes for important security assumptions

### Common mistakes
- using it only after incidents instead of as proactive validation

---

## 6.4 VPC IP Address Manager (IPAM)

IPAM centrally manages and tracks IP space across accounts and Regions.

### Technical role
- allocation planning
- overlapping CIDR avoidance
- utilization visibility
- enterprise-scale governance

### Example
A large enterprise allocates:
- one top-level pool for prod
- one for non-prod
- one for shared services  
and sub-allocates VPC CIDRs from IPAM.

### Best practices
- adopt early in multi-account growth
- align pools to organization structure
- use it before CIDR sprawl becomes irreversible

### Common mistakes
- leaving IP planning to individual teams
- trying to retrofit IPAM only after dozens of conflicting VPCs exist

---

# 7. VPC Peering

VPC peering connects two VPCs privately.

## Technical behavior
- private IP communication
- no transitive routing
- no central route hub
- requires non-overlapping CIDRs
- route tables on both sides must support the path

## Best use cases
- small-scale direct VPC-to-VPC connection
- tightly scoped connectivity between a few environments

## Example
A shared services VPC peers with a legacy tools VPC for limited direct access.

## Best practices
- keep peering simple and intentional
- do not use full-mesh peering when TGW is the real answer
- document route dependencies

## Common mistakes
- expecting transitive reachability
- creating too many peering links in large environments
- forgetting reverse route updates

---

# 8. AWS Transit Gateway (TGW)

Transit Gateway is a regional hub router for connecting VPCs and on-premises networks.

## 8.1 Why TGW exists

Without TGW, many-VPC connectivity often becomes:
- hard to scale
- hard to reason about
- route-heavy
- operationally fragile

TGW centralizes this.

## 8.2 Core entities

### Transit gateway
The central regional routing hub.

### Attachment
A connection into the TGW, such as:
- VPC attachment
- VPN attachment
- Direct Connect gateway association path
- TGW peering attachment

### Transit gateway route table
Controls forwarding inside the TGW.

### Association
Which route table an attachment uses for forwarding decisions.

### Propagation
Which learned routes are inserted into a route table.

### Appliance mode
Used in specific inspection/service insertion designs to preserve traffic symmetry.

## 8.3 How TGW works technically

Traffic path example:
1. Packet leaves VPC subnet route table toward TGW
2. TGW receives packet on source attachment
3. TGW route table looks up destination
4. Packet forwarded to target attachment
5. Target VPC route tables and SG/NACL rules still matter

## 8.4 Design patterns

### Hub-and-spoke
Most common pattern.

### Segmented route tables
Separate TGW route tables for:
- production
- non-production
- shared services
- inspection

### Centralized egress
Traffic from workload VPCs routed through egress/inspection VPCs.

### Hybrid aggregation
VPN and DX paths terminate into TGW-based designs.

## 8.5 Example
A company with 80 VPCs uses TGW:
- prod route table only allows prod ↔ shared services
- non-prod isolated from prod
- on-prem routes propagated to both where approved

## 8.6 Best practices
- design route domains intentionally
- separate association and propagation strategy
- document which attachments can talk to which route domains
- validate asymmetric routing risk with inspection appliances

## 8.7 Common mistakes
- placing everything into one TGW route table
- assuming propagation equals proper segmentation
- forgetting subnet route tables in attached VPCs still need correct routes
- assuming TGW is globally meshed without extra design

---

# 9. AWS Cloud WAN

Cloud WAN is a managed global WAN service for connecting Regions, branches, and on-premises environments under centralized policy.

## Technical role
Cloud WAN operates above regional TGW-style patterns and helps global organizations standardize network segmentation and connectivity.

## Core entities
- core network
- segments
- attachments
- network policy
- edge locations

## Best use cases
- global enterprise networking
- multi-Region segmentation
- branch/SD-WAN integration
- centrally managed worldwide connectivity

## Example
A multinational enterprise uses Cloud WAN to keep finance, engineering, and retail segments separated across multiple Regions and branch locations.

## Best practices
- use Cloud WAN when the problem is global networking governance, not just one-Region VPC routing
- align segments to business/security domains

## Common mistakes
- choosing Cloud WAN when a single TGW is sufficient
- treating it as just “bigger peering” rather than policy-driven WAN architecture

---

# 10. Gateway Endpoints, Interface Endpoints, and AWS PrivateLink

These three must be clearly distinguished.

## 10.1 Gateway VPC endpoints

Used for specific AWS services such as S3 and DynamoDB.

### Technical behavior
- added as route-table targets
- traffic remains on AWS network
- no NAT or IGW needed for those services

### Example
Private subnet instances access S3 through a gateway endpoint instead of NAT.

### Best practices
- use for S3/DynamoDB where applicable
- combine with endpoint policies and bucket policies where relevant

### Common mistakes
- continuing to pay NAT costs for S3-heavy traffic unnecessarily

---

## 10.2 Interface endpoints

Interface endpoints provide private access to supported AWS services and endpoint services.

### Technical behavior
- create ENIs in selected subnets
- use private IPs
- often integrate with private DNS
- powered by PrivateLink

### Controls
- endpoint SGs
- endpoint policies where supported
- DNS behavior

### Example
A private application uses interface endpoints for Systems Manager, CloudWatch, ECR, and Secrets Manager.

### Best practices
- deploy in multiple AZs where availability matters
- control SGs tightly
- understand which services support endpoint policies

### Common mistakes
- forgetting that endpoint ENIs live in chosen subnets
- assuming every endpoint behaves like a route-table endpoint

---

## 10.3 AWS PrivateLink

PrivateLink is the broader private service exposure model.

### Technical purpose
It lets a service provider expose a service privately to consumers without giving them full network-level access to the provider VPC.

### Core entities
- service provider
- endpoint service
- service consumer
- interface endpoint
- NLB-backed exposure in many provider patterns
- private DNS association

### Why it matters
PrivateLink gives **service-level private access** without:
- full peering
- overlapping-route trust
- internet exposure

### Example
A central platform team exposes an internal API privately to many application accounts through PrivateLink.

### Best practices
- choose PrivateLink when you want controlled service consumption, not broad network adjacency
- design DNS carefully
- keep provider service boundaries clean

### Common mistakes
- using peering when only one service needs to be shared
- exposing internal platforms publicly because private service exposure was not understood

---

# 11. Amazon VPC Lattice

VPC Lattice is a managed application networking service for connecting, securing, and observing services across VPCs and accounts.

## 11.1 Why Lattice exists

Traditional VPC networking solves IP connectivity.  
Modern environments also need:
- service-to-service communication
- policy at the application/service level
- easier multi-account connectivity
- consistent auth and monitoring

That is where Lattice fits.

## 11.2 Core entities

### Service network
Logical grouping of services and associated VPC access.

### Service
The application/service exposed through Lattice.

### Target group
Targets behind a Lattice service.

Supported targets can include:
- EC2 instances
- IP addresses
- Lambda functions
- ECS tasks
- Kubernetes pods
- ALBs in supported patterns

### Listener
Defines protocol and port handling.

### Rule
Controls request routing behavior.

### Auth policy
IAM-style policy controlling access to services or service networks.

## 11.3 Technical role
Lattice is for **application networking**, not raw packet forwarding.

It helps when:
- multiple services live across VPCs/accounts
- platform teams want common service connectivity and auth
- developers should not manage complex peering/TGW logic for every service interaction

## 11.4 Example
Team A in one account runs an orders service.  
Team B in another account runs a payments service.  
Rather than exposing broad VPC-level reachability, Lattice connects them through service-aware policies.

## 11.5 Best practices
- use when application-to-application communication needs consistent policy and observability
- keep service network boundaries aligned to trust boundaries
- define auth policies carefully

## 11.6 Common mistakes
- thinking Lattice replaces every VPC construct
- using it without understanding underlying network reachability and target placement
- treating it like a direct substitute for internet-facing API platforms

---

# 12. AWS Verified Access

Verified Access enables secure access to corporate applications without traditional always-on VPN dependency.

## 12.1 Core entities

### Verified Access instance
The primary container for Verified Access resources.

### Trust provider
Supplies trust context, such as:
- user identity
- device posture
- other supported trust attributes

### Application
The app being protected.

### Attachment
Connection of the app path into the Verified Access architecture.

### Policy
Rules evaluated against trust context.

## 12.2 Technical role
Verified Access enforces **application-level access decisions** based on real-time trust evaluation.

It is fundamentally different from network-extension VPN models.

## 12.3 Example
A finance application is only accessible if:
- user is in finance group
- device meets compliance posture
- access request matches policy context

## 12.4 Best practices
- use for zero-trust application access patterns
- integrate with strong identity and device trust sources
- write least-privilege access policies

## 12.5 Common mistakes
- using Verified Access when the requirement is broad network access
- confusing it with Client VPN
- not aligning app segmentation with policy groups

---

# 13. AWS Network Firewall and Firewall Manager

## 13.1 AWS Network Firewall

A managed stateful firewall and IDS/IPS-style service for VPC traffic inspection.

### Core entities
- firewall
- firewall policy
- stateless rule groups
- stateful rule groups
- firewall endpoint
- logging configuration

### Technical role
Network Firewall adds deeper inspection than SGs/NACLs. It is useful for:
- egress filtering
- east-west inspection
- domain/IP blocking
- intrusion detection/prevention style controls

### How it works
Traffic must be routed through firewall endpoints to be inspected.

### Example
Centralized egress VPC:
- workload VPCs send internet-bound traffic to inspection VPC
- Network Firewall evaluates domain and connection rules
- allowed traffic then exits through NAT/egress path

### Best practices
- design routing deliberately
- validate symmetry in inspection paths
- centralize logs and rule governance
- use separate policy/testing lifecycle for firewall changes

### Common mistakes
- deploying firewall without routing traffic through it
- assuming firewall “protects the VPC” by existing alone
- creating overly broad rule groups that reduce security value

---

## 13.2 AWS Firewall Manager

Firewall Manager centralizes organization-wide security policy deployment.

### Services commonly governed
- AWS WAF
- AWS Shield Advanced
- security group policies
- AWS Network Firewall
- Route 53 Resolver DNS Firewall

### Technical role
It is a **central policy distribution and compliance enforcement service** across accounts.

### Example
The security team defines a standard WAF policy and DNS Firewall policy for all internet-facing workloads across the organization.

### Best practices
- use with Organizations for central enforcement
- separate policy ownership from workload ownership clearly
- combine with delegated admin models

### Common mistakes
- managing per-account firewall controls manually in large environments
- using Firewall Manager without clear exception processes

---

# 14. Elastic Load Balancing (ALB, NLB, GWLB)

## 14.1 Application Load Balancer (ALB)

### Technical role
ALB is a Layer 7 load balancer for HTTP/HTTPS traffic.

### Key entities
- listener
- listener rule
- target group
- health check

### Strengths
- host-based routing
- path-based routing
- modern web apps and microservices
- OIDC/auth integration in relevant patterns
- HTTP-aware request handling

### Example
Requests to:
- `/api/*` go to app API target group
- `/static/*` go to static service target group

### Best practices
- use ALB for HTTP/S applications
- keep health checks application-relevant
- terminate TLS with ACM where appropriate

### Common mistakes
- choosing NLB when ALB’s HTTP features are needed
- poor health check paths causing false unhealthy states

---

## 14.2 Network Load Balancer (NLB)

### Technical role
NLB is a Layer 4 load balancer for TCP, UDP, and TLS scenarios.

### Strengths
- very high performance
- low latency
- static IP-oriented use cases
- PrivateLink provider patterns
- non-HTTP protocols

### Example
Expose a TCP-based service or privately publish a service behind PrivateLink.

### Best practices
- use when protocol or performance needs are L4-focused
- understand health check and target behavior carefully
- use for PrivateLink endpoint services where appropriate

### Common mistakes
- using NLB for web routing scenarios that clearly need ALB features

---

## 14.3 Gateway Load Balancer (GWLB)

### Technical role
GWLB inserts and scales virtual network/security appliances transparently.

### Use cases
- third-party firewalls
- IDS/IPS appliance chains
- centralized inspection VPC patterns

### Best practices
- use when appliance insertion is the requirement
- validate traffic symmetry and appliance scaling behavior

### Common mistakes
- confusing GWLB with regular app load balancing

---

# 15. AWS Certificate Manager (ACM)

ACM manages certificates for integrated AWS services.

## 15.1 Certificate types
- public certificates
- private certificates via private PKI integration
- imported certificates

## 15.2 Technical role
- issue/provision certs
- attach certs to ALB, CloudFront, API Gateway, and other integrated services
- manage renewal for eligible ACM-managed certs

## 15.3 Example
Use ACM to provide an HTTPS certificate for:
- `app.example.com` on ALB
- `api.example.com` on API Gateway
- `www.example.com` on CloudFront

## 15.4 Best practices
- prefer ACM-managed renewal where possible
- centralize certificate lifecycle visibility
- understand region/scope constraints for integrated services

## 15.5 Common mistakes
- forgetting imported certs do not get the same managed renewal behavior
- mixing certificate ownership and DNS validation processes poorly

---

# 16. Hybrid Connectivity: Site-to-Site VPN, Client VPN, and Direct Connect

## 16.1 AWS Site-to-Site VPN

Provides encrypted IPsec connectivity between on-premises and AWS.

### Core entities
- customer gateway
- virtual private gateway or transit gateway
- VPN connection
- tunnels
- static routes or BGP

### Technical role
Fast to deploy, internet-based, useful for hybrid connectivity and backup paths.

### Example
An on-prem data center connects to AWS over Site-to-Site VPN while waiting for Direct Connect to be provisioned.

### Best practices
- use redundant tunnels
- prefer dynamic routing with BGP where appropriate
- treat VPN as part of a larger hybrid design, not magic connectivity

### Common mistakes
- assuming VPN gives DX-like consistency
- failing to design failover and route preference clearly

---

## 16.2 AWS Client VPN

Managed remote-access VPN for users/devices.

### Technical role
- remote workforce access
- admin/developer access into AWS/private networks
- temporary secure access pattern

### Example
Developers connect to a Client VPN endpoint to access an internal admin portal.

### Best practices
- integrate with centralized identity
- scope routes and authorization tightly
- log access and review user groups

### Common mistakes
- using Client VPN for app-specific zero-trust scenarios where Verified Access is better
- giving overly broad network access

---

## 16.3 AWS Direct Connect (DX)

Direct Connect provides dedicated private connectivity from on-premises to AWS.

### Core entities
- dedicated connection or hosted connection
- private virtual interface (private VIF)
- public virtual interface (public VIF)
- transit virtual interface (transit VIF)
- Direct Connect gateway

### Technical role
- more predictable network path than internet-based connectivity
- higher consistency and enterprise-grade hybrid backbone
- often used for production hybrid traffic

### Private VIF
Used for private access to VPC-connected resources through the relevant architecture.

### Public VIF
Used to reach public AWS services over Direct Connect paths.

### Transit VIF
Used with Direct Connect gateway and Transit Gateway-centered hybrid designs.

### Direct Connect gateway
Associates DX connectivity into broader AWS routing models.

### Example
A company uses:
- DX as primary hybrid path
- Site-to-Site VPN as backup
- TGW as regional routing hub on AWS side

### Best practices
- design redundancy and resilience explicitly
- understand route advertisement behavior
- pair DX with backup VPN
- validate segmentation in TGW + DX designs

### Common mistakes
- assuming DX is automatically encrypted
- ignoring failover when DX experiences issues
- not planning BGP and route priorities carefully

---

# 17. Route 53 and Route 53 Resolver

Route 53 is a DNS and traffic management service. Resolver handles DNS resolution behavior for VPCs.

## 17.1 Hosted zones

### Public hosted zone
Authoritative DNS for internet-facing names.

### Private hosted zone
Authoritative DNS for private names resolved within associated VPCs.

### Example
- `api.example.com` in public hosted zone
- `db.internal.example.com` in private hosted zone

### Best practices
- separate public and private naming cleanly
- associate private zones only where needed
- keep DNS ownership model clear

### Common mistakes
- creating confusing overlapping internal naming patterns
- forgetting VPC association requirements for private zones

---

## 17.2 Record types and alias records

Route 53 stores DNS records.  
Alias records are AWS-aware and can point to AWS resources like:
- ALB
- CloudFront
- API Gateway in supported patterns

### Best practice
Prefer alias records for supported AWS targets rather than using raw IP addresses.

---

## 17.3 Routing policies

### Simple routing
Single answer pattern.

### Weighted routing
Traffic split by weight.

**Example:** blue/green rollout 90/10.

### Latency routing
Routes users to the region with the lowest latency profile.

### Failover routing
Primary/secondary DNS failover.

### Geolocation routing
Responds based on user geographic location.

### Geoproximity routing
Routes based on geographic biasing logic.

### Multivalue answer routing
Returns multiple healthy values.

### Best practices
- choose routing policy based on actual traffic objective
- use health checks where failover behavior matters

### Common mistakes
- using weighted routing when health-aware failover was the actual need
- misunderstanding geolocation vs latency behavior

---

## 17.4 Route 53 health checks

Health checks actively test endpoints and can influence DNS failover decisions.

### Example
Primary app endpoint is unhealthy, so failover record responds with DR endpoint.

---

## 17.5 Route 53 Resolver

Resolver handles recursive resolution inside VPCs.

### Core entities
- inbound endpoint
- outbound endpoint
- forwarding rules
- private hosted zone resolution behavior

### Inbound endpoint
Lets on-prem DNS forward queries into AWS private DNS space.

### Outbound endpoint
Lets VPCs forward specific DNS queries to external/on-prem resolvers.

### Forwarding rules
Define which domains are forwarded where.

### Example
Queries for `corp.local` are forwarded from AWS to on-prem DNS via outbound Resolver endpoints.

### Best practices
- centralize hybrid DNS architecture
- make forwarding rules explicit
- document split-horizon/private zone behavior

### Common mistakes
- designing hybrid networking but ignoring hybrid DNS
- forgetting DNS is often the real issue in “network connectivity” incidents

---

## 17.6 Resolver DNS Firewall

Filters outbound DNS queries from VPCs.

### Technical role
- block malicious domains
- enforce allow/deny policies
- add DNS-layer egress control

### Example
Block known command-and-control domains from private subnets.

### Best practices
- combine with threat intel and security governance
- log and review blocked queries

### Common mistakes
- thinking DNS Firewall replaces network firewalls or WAF

---

## 17.7 DNSSEC validation

Resolver DNSSEC validation helps protect against spoofed DNS responses for supported scenarios.

### Best practice
Use when DNS trust integrity is important in enterprise/hybrid designs.

---

# 18. Amazon CloudFront

CloudFront is AWS’s global CDN and edge delivery platform.

## 18.1 Core entities
- distribution
- origin
- cache behavior
- edge location
- invalidation
- origin protection features such as controlled origin access patterns

## 18.2 Technical role
- cache content near users
- reduce latency
- shield origins from load
- integrate with WAF and TLS
- accelerate both static and dynamic delivery patterns

## 18.3 Example
A global web app uses:
- CloudFront in front of ALB
- WAF at CloudFront
- static asset caching at edge
- dynamic content forwarded to origin selectively

## 18.4 Best practices
- put CloudFront in front of internet-heavy web apps
- tune cache behaviors per path
- protect origins so they are not bypassed directly
- use HTTPS end-to-end where appropriate

## 18.5 Common mistakes
- treating CloudFront as only for static sites
- not protecting the origin against direct access
- caching patterns incorrectly for dynamic or personalized content

---

# 19. AWS WAF

AWS WAF is a Layer 7 firewall for HTTP/S traffic.

## 19.1 Core entities
- web ACL
- rule
- rule group
- managed rule group
- association

## 19.2 Technical role
WAF evaluates HTTP requests and blocks/permits based on application-layer rules.

### Typical protections
- common exploit signatures
- bot/rate controls
- IP and geo filtering
- request pattern filtering

## 19.3 Example
A public API behind API Gateway is protected with WAF managed rules and custom rate limiting.

## 19.4 Best practices
- use managed rules as a baseline
- add targeted custom protections
- monitor false positives carefully
- place WAF where public HTTP traffic enters

## 19.5 Common mistakes
- assuming WAF protects non-HTTP protocols
- enabling rules without reviewing application impact
- treating WAF as substitute for secure coding

---

# 20. AWS Shield

Shield provides DDoS protection for AWS resources.

## 20.1 Shield Standard
Baseline protection included for common AWS resources.

## 20.2 Shield Advanced
Enhanced DDoS protections and operational support features for higher-risk environments.

## 20.3 Technical role
Protects public-facing services from large-scale network and transport-layer attack patterns.

## 20.4 Example
An e-commerce company protects its CloudFront and Route 53-based public architecture with Shield-focused operational practices.

## 20.5 Best practices
- combine Shield with CloudFront, Route 53, WAF, and resilient architecture
- understand public attack surfaces
- use advanced protections where business risk justifies them

## 20.6 Common mistakes
- assuming Shield replaces WAF
- assuming DDoS resilience is only a security-team issue rather than an architecture issue

---

# 21. AWS Global Accelerator

Global Accelerator provides static anycast IPs and routes client traffic over the AWS global network to healthy regional endpoints.

## 21.1 Core entities
- accelerator
- listener
- endpoint group
- endpoint

## 21.2 Technical role
- improve network path quality
- support fast failover
- preserve stable entry points

## 21.3 Example
A global gaming service uses Global Accelerator to route users to the nearest healthy regional endpoint with fast failover.

## 21.4 Best practices
- use when global path optimization and static entry points matter
- pair with regional HA design

## 21.5 Common mistakes
- confusing it with CloudFront caching
- using it when the problem is web content delivery rather than network pathing

---

# 22. Amazon API Gateway

API Gateway is a managed API front door.

## 22.1 API types
- REST APIs
- HTTP APIs
- WebSocket APIs

## 22.2 Technical role
- request entry point for APIs
- auth integration
- throttling
- backend integration
- request/response handling
- API lifecycle management

## 22.3 Private connectivity concepts
- private APIs
- VPC links
- internal backend integration patterns

## 22.4 Example
A company exposes public REST APIs through API Gateway, while internal microservices use private integrations into VPC-based backends.

## 22.5 Best practices
- use API Gateway when API management is the problem
- choose REST vs HTTP vs WebSocket intentionally
- align auth and throttling to business/API requirements
- separate public and private API exposure clearly

## 22.6 Common mistakes
- using API Gateway when a simple ALB would suffice
- using ALB when the requirement is full API policy/throttling/governance
- not understanding private API connectivity patterns

---

# 23. Comparison Guide for Common Design Decisions

## 23.1 Peering vs Transit Gateway vs Cloud WAN
- **Peering**: direct, simple, non-transitive
- **TGW**: regional hub routing
- **Cloud WAN**: global policy-driven WAN

## 23.2 NAT gateway vs VPC endpoint
- **NAT**: outbound internet path for private IPv4 workloads
- **Endpoint**: private path to supported AWS services

## 23.3 PrivateLink vs Peering
- **PrivateLink**: share/consume specific services privately
- **Peering**: broader network-level connectivity

## 23.4 ALB vs NLB vs GWLB
- **ALB**: HTTP/HTTPS application routing
- **NLB**: TCP/UDP/TLS performance and static endpoint patterns
- **GWLB**: appliance insertion

## 23.5 Client VPN vs Verified Access
- **Client VPN**: network access
- **Verified Access**: app access based on trust

## 23.6 CloudFront vs Global Accelerator
- **CloudFront**: edge caching/content delivery
- **Global Accelerator**: network path optimization to regional endpoints

## 23.7 WAF vs Network Firewall
- **WAF**: Layer 7 HTTP request filtering
- **Network Firewall**: VPC traffic inspection and network-layer controls

---

# 24. End-to-End Reference Architecture Example

A large enterprise needs:
- multi-account workloads
- private internal services
- public web application
- remote developer access
- on-prem ERP connectivity
- DNS integration
- centralized security enforcement
- global user performance

A strong design could be:

### Account and VPC model
- shared services account
- security/inspection account
- app accounts by environment
- separate VPCs for app, shared services, and inspection

### Core routing
- Transit Gateway as regional hub
- segmented TGW route tables
- hybrid routes from on-prem through DX and VPN backup

### Internet and private access
- public ALBs in public subnets
- apps in private subnets
- DBs in isolated subnets
- gateway endpoint for S3
- interface endpoints for management/control-plane services

### Service-to-service and app access
- VPC Lattice for cross-account app connectivity
- Verified Access for internal user-facing app access
- API Gateway for public API exposure

### Security
- SGs for workload filtering
- Network Firewall for centralized egress inspection
- WAF at CloudFront/API layer
- Shield and resilient global architecture for DDoS posture
- Firewall Manager for org-wide central policy

### DNS and edge
- Route 53 public and private hosted zones
- Resolver endpoints for hybrid DNS
- CloudFront for edge acceleration
- Global Accelerator for global failover-sensitive entry paths where needed

This example shows the Week 2 principle:
> different networking services solve different layers of the connectivity problem.

---

# 25. Troubleshooting Framework for AWS Networking

When traffic fails, diagnose in this order:

## 25.1 DNS
- Does the hostname resolve?
- Public or private zone?
- Resolver forwarding correct?
- CloudFront/API/ALB DNS target correct?

## 25.2 Route path
- Is the destination in the same subnet/VPC?
- Does route table point to the correct next hop?
- Does TGW/peering/VPN/DX route exist?

## 25.3 Security controls
- SG inbound/outbound rules?
- NACL return path?
- Firewall policy?
- WAF/DNS Firewall behavior?

## 25.4 Attachment and endpoint health
- TGW attachment up?
- VPN tunnels up?
- Endpoint ENIs healthy?
- load balancer target health good?

## 25.5 Service-layer checks
- ALB listener/rules valid?
- ACM cert valid?
- API Gateway integration working?
- CloudFront origin reachable?

## 25.6 Analysis tools
- Flow Logs
- Reachability Analyzer
- Network Access Analyzer
- relevant service logs/metrics

---

# 26. Common Week 2 Mistakes

- overlapping CIDRs across accounts
- single-AZ network design for critical workloads
- public subnet misuse
- broad `0.0.0.0/0` SG rules “for testing” that become permanent
- forgetting NACL stateless return traffic
- using NAT when endpoints are better
- building peering meshes instead of hub routing
- no TGW route segmentation
- treating Direct Connect as encrypted by default
- weak DNS design in hybrid environments
- exposing internal services publicly instead of using PrivateLink/Lattice/Verified Access
- deploying WAF or Network Firewall without tuning/ownership
- confusing CloudFront, Global Accelerator, and API Gateway roles

---

# 27. Week 2 Revision Checklist

You should be able to explain clearly:

- what a VPC is and how it differs from a subnet
- why route tables define public/private behavior
- how IGW, egress-only IGW, and NAT gateway differ
- how ENIs and SGs interact
- why NACLs are stateless
- what Flow Logs show
- when to use Reachability Analyzer vs Network Access Analyzer
- why IPAM matters in enterprise AWS
- why peering is non-transitive
- how TGW attachments, associations, and propagations work
- when Cloud WAN is appropriate
- the difference between gateway endpoints, interface endpoints, and PrivateLink
- what VPC Lattice solves
- how Verified Access differs from Client VPN
- when to choose ALB, NLB, or GWLB
- how ACM certificate management works
- how Site-to-Site VPN differs from Direct Connect
- what Route 53 Resolver does
- what DNS Firewall does
- how CloudFront, WAF, Shield, Global Accelerator, and API Gateway fit together

---

# 28. Memory Anchors

- **VPC** = isolated virtual network
- **Subnet** = AZ-level address segment
- **Route table** = next-hop logic
- **IGW** = public internet path
- **Egress-only IGW** = outbound-only IPv6 internet
- **NAT gateway** = outbound IPv4 internet for private workloads
- **ENI** = actual network interface
- **SG** = stateful ENI firewall
- **NACL** = stateless subnet filter
- **Flow Logs** = traffic metadata
- **Reachability Analyzer** = path reasoning
- **IPAM** = address governance
- **Peering** = direct private VPC link
- **TGW** = regional routing hub
- **Cloud WAN** = global WAN governance
- **Gateway endpoint** = private S3/DynamoDB access
- **Interface endpoint** = private ENI-based service access
- **PrivateLink** = private service exposure/consumption
- **VPC Lattice** = application networking layer
- **Verified Access** = trust-based app access
- **Network Firewall** = VPC traffic inspection
- **Firewall Manager** = centralized security policy rollout
- **ALB** = layer 7 load balancing
- **NLB** = layer 4 load balancing
- **GWLB** = appliance insertion
- **ACM** = certificate lifecycle
- **Site-to-Site VPN** = encrypted hybrid tunnel
- **Client VPN** = remote user network access
- **DX** = dedicated hybrid connectivity
- **Route 53** = DNS and traffic steering
- **Resolver** = VPC DNS resolution and forwarding
- **DNS Firewall** = DNS-layer filtering
- **CloudFront** = CDN and edge delivery
- **WAF** = web request filtering
- **Shield** = DDoS protection
- **Global Accelerator** = global path optimization
- **API Gateway** = managed API entry layer

---

# 29. Official AWS Documentation References

## VPC core
- Amazon VPC Documentation  
  https://docs.aws.amazon.com/vpc/

- Reachability Analyzer  
  https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html

- Network Access Analyzer  
  https://docs.aws.amazon.com/vpc/latest/network-access-analyzer/what-is-network-access-analyzer.html

## Connectivity
- AWS Transit Gateway  
  https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html

- AWS Cloud WAN  
  https://docs.aws.amazon.com/network-manager/latest/cloudwan/

- AWS PrivateLink  
  https://docs.aws.amazon.com/vpc/latest/privatelink/

- Amazon VPC Lattice  
  https://docs.aws.amazon.com/vpc-lattice/

- AWS Verified Access  
  https://docs.aws.amazon.com/verified-access/

## Security and enforcement
- AWS Network Firewall  
  https://docs.aws.amazon.com/network-firewall/

- AWS Firewall Manager  
  https://docs.aws.amazon.com/firewall-manager/

## Load balancing and TLS
- Elastic Load Balancing  
  https://docs.aws.amazon.com/elasticloadbalancing/

- AWS Certificate Manager  
  https://docs.aws.amazon.com/acm/

## Hybrid connectivity
- AWS Site-to-Site VPN  
  https://docs.aws.amazon.com/vpn/latest/s2svpn/

- AWS Client VPN  
  https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/

- AWS Direct Connect  
  https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html

## DNS and edge
- Amazon Route 53 Developer Guide  
  https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html

- Route 53 Resolver  
  https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver.html

- Resolver DNS Firewall  
  https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-dns-firewall.html

- Amazon CloudFront Developer Guide  
  https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html

- AWS WAF  
  https://docs.aws.amazon.com/waf/

- AWS Shield  
  https://docs.aws.amazon.com/shield/

- AWS Global Accelerator  
  https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html

## API exposure
- Amazon API Gateway Documentation  
  https://docs.aws.amazon.com/apigateway/

---

# 30. Final Summary

Week 2 is about **how traffic is designed, controlled, protected, and exposed in AWS**.

To master Week 2, you must understand both:
- **individual services**
- **how those services combine into architectures**

The strongest mental model is:

- **VPC, subnets, and CIDRs** define where workloads live
- **route tables and gateways** define where packets can go
- **SGs, NACLs, firewalls, and WAF** define what traffic is permitted
- **TGW, Cloud WAN, VPN, and DX** define how environments connect
- **endpoints, PrivateLink, Lattice, and Verified Access** define how private services and apps are consumed
- **ALB, NLB, GWLB, CloudFront, Global Accelerator, and API Gateway** define how traffic enters and is distributed
- **Route 53 and Resolver** define how names are resolved
- **Flow Logs and analyzers** help you prove why connectivity works or fails

When you can explain those relationships clearly, Week 2 is not just memorized — it is understood architecturally.
