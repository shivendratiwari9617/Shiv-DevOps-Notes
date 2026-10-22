# GCP Study Notes — Week 2 v2
## Networking, Private Connectivity, Hybrid Connectivity, DNS, Edge, and API Exposure

> This is the **9.5+ technical version** of Week 2.  
> It is written as a **service-by-service study guide** with:
> - technical purpose
> - core entities
> - packet-path relevance
> - architecture usage
> - examples
> - best practices
> - common mistakes
> - troubleshooting cues
> - official references  
>
> **Noise control:** concise wording, high information density, minimal filler.

---

# 1. Why Week 2 Matters

Week 2 is about **how traffic moves in Google Cloud**.

If Week 1 defined:
- organization
- projects
- identity
- policy
- landing zone controls

then Week 2 defines:
- how resources communicate
- how traffic enters and leaves
- how hybrid networks connect
- how private service access works
- how DNS resolves names
- how edge security and caching work
- how APIs are exposed
- how operators prove packet reachability

The core idea is:

> **Google Cloud networking is a layered system of global VPC design, regional subnet placement, route selection, distributed firewalling, private service access, hybrid exchange, DNS control, and edge traffic management.**

The critical Week 2 skill is not memorizing products. It is being able to say:

- where the packet starts
- what address it gets
- which route wins
- which policy allows or blocks it
- whether the packet stays private
- where DNS resolves the name
- where the traffic is balanced, cached, inspected, or rejected

---

# 2. Learning Outcomes

By the end of Week 2, you should be able to:

- explain **VPC**, **subnets**, **primary and secondary ranges**, **alias IPs**, and route behavior
- explain **VPC firewall rules**, **hierarchical firewall policies**, **network tags**, and **service account targets**
- explain **VPC Flow Logs**, **Firewall Insights**, and **Connectivity Tests**
- explain **Shared VPC**, **VPC Network Peering**, **Private Service Access (PSA)**, and **Private Service Connect (PSC)**
- explain **Cloud Router**, **Cloud NAT**, **Network Connectivity Center**, **Cloud VPN**, and **Cloud Interconnect**
- explain **Cloud DNS** public zones, private zones, forwarding zones, peering zones, reverse lookup zones, DNS policies, response policies, and DNSSEC
- distinguish GCP load balancer families and explain the purpose of **backend services**, **backend buckets**, **NEGs**, **URL maps**, **target proxies**, **forwarding rules**, **health checks**, and **SSL certificates**
- explain **Cloud Armor**, **Cloud CDN**, **Packet Mirroring**, and **Cloud IDS**
- explain **API Gateway** and **Apigee**
- troubleshoot routing, firewall, DNS, private access, hybrid path, and edge-layer issues systematically

---

# 3. Architectural Mental Model

Think in layers:

1. **Addressing layer**  
   VPC, subnets, primary ranges, secondary ranges, alias IPs

2. **Reachability layer**  
   system routes, static routes, dynamic routes, route priority, peering, PSA, PSC, hybrid routes

3. **Enforcement layer**  
   implied firewall behavior, VPC firewall rules, hierarchical firewall policies, targets, logging

4. **Hybrid layer**  
   Cloud Router, BGP, Cloud VPN, Cloud Interconnect, Network Connectivity Center, Cloud NAT

5. **DNS layer**  
   Cloud DNS zones, forwarding, peering, policies, response policies, DNSSEC

6. **Ingress and edge layer**  
   load balancers, Cloud Armor, Cloud CDN, API Gateway, Apigee

7. **Diagnostics and inspection layer**  
   Flow Logs, Firewall Insights, Connectivity Tests, Packet Mirroring, Cloud IDS

When a network issue happens, at least one of these layers is wrong.

---

# 4. Virtual Private Cloud (VPC)

Google Cloud VPC is a **global** network construct.  
Subnets are **regional**.

This is one of the most important architectural differences in GCP networking.

## 4.1 Technical purpose

A VPC network provides:
- a private L3 network boundary
- IP address allocation through subnets
- route domain for resources in the VPC
- firewall rule scope
- attachment point for hybrid and private service mechanisms

## 4.2 Core entities

### VPC network
The top-level network boundary.

### Auto mode VPC
A legacy-style convenience mode that automatically creates one subnet per region with pre-defined CIDR allocations.

### Custom mode VPC
A VPC where you create subnets explicitly.  
This is the normal enterprise choice.

### Dynamic routing mode
Controls how dynamically learned routes are applied and propagated in the VPC design context.

### Subnet
Regional IP allocation inside a global VPC.

### Primary IP range
Main IP range of a subnet.

### Secondary IP range
Additional CIDR ranges, commonly used for GKE Pods and Services.

### Alias IP
Additional IP range assignment to an interface, often using subnet secondary ranges.

### Route
Destination/next-hop rule used by the network dataplane.

### Firewall rule
Traffic filter applied in the VPC context.

## 4.3 Global VPC implications

Because the VPC is global:
- the same VPC can host subnets in many regions
- route domain spans those subnets
- firewall rules are network-scoped, not subnet-scoped
- hybrid attachments often connect to the VPC as a whole while subnets remain regional

### Example
One production VPC may contain:
- `us-central1-app`
- `us-east1-app`
- `europe-west1-app`
- `asia-southeast1-data`

All of them are part of one global route domain.

## 4.4 Auto mode vs custom mode

### Auto mode
Useful only for very simple or legacy scenarios.

### Problems with auto mode
- poor enterprise IP planning flexibility
- subnet sprawl
- future overlap risk
- weaker long-term control

### Best practice
Use **custom mode VPC** for production and landing-zone designs.

## 4.5 Best practices
- use custom mode VPC
- design CIDRs globally, not only region by region
- separate prod and nonprod network domains
- define subnet naming standards clearly
- reserve space for future secondary ranges and hybrid routes

## 4.6 Common mistakes
- designing as if VPC itself is regional
- using auto mode in enterprise environments
- choosing small subnet ranges that later block GKE or scale
- one giant flat VPC for unrelated trust boundaries

---

# 5. Subnets, Primary Ranges, Secondary Ranges, and Alias IPs

## 5.1 Subnets

Subnets are regional and define address allocation for regional resources.

### Technical role
- source of VM primary IPs
- container network planning anchor
- Flow Log configuration point
- PSC, hybrid, and private traffic planning element

## 5.2 Primary ranges

### Technical role
Main subnet CIDR from which interface addresses are allocated.

### Example
`10.10.0.0/20` for regional app instances.

## 5.3 Secondary ranges

### Technical role
Additional CIDRs attached to a subnet, commonly used for:
- GKE Pod IPs
- GKE Service IPs
- alias IP models

### Why they matter
If you plan GKE or large-scale container networking badly, subnet redesign becomes painful.

## 5.4 Alias IPs

### Technical role
Allow additional IP ranges on interfaces without requiring multiple NICs.

### Common uses
- GKE Pod networking
- advanced service segmentation
- multiple service identities on instances in some patterns

## 5.5 Example
A GKE subnet might use:
- primary range: nodes
- secondary range A: Pods
- secondary range B: Services

## 5.6 Best practices
- reserve secondary ranges before cluster rollout
- treat Pod and Service IP planning as first-class networking design
- document which ranges are node, Pod, and Service space

## 5.7 Common mistakes
- subnet planned only for VM count, not container growth
- overlapping secondary ranges across environments
- no documentation of subnet purpose

---

# 6. Routes and Route Selection

Routing determines where packets go.

## 6.1 Core route types

### System-generated routes
Created automatically for subnet local reachability and internet default behavior where applicable.

### Static routes
User-defined custom routes.

### Dynamic routes
Learned through Cloud Router via BGP.

## 6.2 Core attributes

### Destination
CIDR or applicable destination target.

### Next hop
The resource or path the packet should use.

### Priority
Tie-break input. Lower number = more preferred.

## 6.3 Next-hop types

Common next-hop patterns include:
- default internet gateway path
- VPN tunnel
- next-hop instance
- ILB next hop in supported designs
- dynamically learned hybrid path through Cloud Router
- internal packet-forwarding appliance patterns in supported topologies

## 6.4 Route evaluation mental model

Main decision factors:
1. most specific destination prefix
2. route type and validity
3. priority where applicable among matching choices
4. policy/topology constraints

## 6.5 Example
Traffic to `10.200.0.0/16` uses a dynamically learned on-prem route from Cloud Router, while general internet traffic follows default egress design.

## 6.6 Best practices
- keep route intent documented
- use dynamic routing for serious hybrid estates
- validate route advertisements during change reviews
- minimize unnecessary static routes

## 6.7 Common mistakes
- stale static routes after migration
- route advertisements broader than intended
- troubleshooting firewall before verifying route selection
- misunderstanding which dynamic routes are actually available in the target path

---

# 7. Firewall Model: Implied Rules, VPC Firewall Rules, and Hierarchical Firewall Policies

Google Cloud firewalling is distributed and precedence-aware.  
To master it, you must know the layers.

## 7.1 Implied firewall rules

By default, Google Cloud includes:
- an implied **allow egress**
- an implied **deny ingress**

These matter because they explain why outbound traffic often works until you centralize restrictions.

### Best practice
Do not rely on implied behavior in mature environments.  
Define explicit egress and ingress policy where governance matters.

---

## 7.2 VPC firewall rules

### Technical purpose
Allow or deny traffic based on direction, protocol, ports, sources/destinations, and target selection.

### Core attributes
- ingress or egress
- allow or deny
- priority
- source or destination
- protocol and ports
- target tags
- target service accounts
- logging

## 7.3 Targeting models

### Network tags
Metadata-based target selector.

**Pros**
- simple
- easy to understand

**Cons**
- weaker governance
- tag sprawl risk
- easy to misuse across workloads

### Service account targets
Identity-based target selector.

**Pros**
- aligned to workload identity
- often cleaner than tags for platform governance

**Cons**
- requires disciplined service-account design

### Best practice
Prefer **service account targets** for production workload intent where possible.

## 7.4 Priority and conflict handling

Lower numeric priority wins.

### Example
- allow internal app traffic at priority 1000
- deny broad ingress at 2000  
The allow wins because 1000 is more preferred.

## 7.5 Example
Allow inbound 443 only to VMs running as `payments-api-sa`, from the internal ALB source range.

## 7.6 Best practices
- use least privilege
- use service-account targeting for strong workload alignment
- log high-value deny/allow decisions
- name rules by function, not by ticket number
- centralize baseline patterns in reusable policy designs

## 7.7 Common mistakes
- broad `0.0.0.0/0` allowances
- too many overlapping rules
- mixing platform and app ownership in the same rule sets
- using tags as uncontrolled free-form metadata

---

## 7.8 Hierarchical firewall policies

Google documents hierarchical firewall policies as applying along with VPC firewall rules. [2]

### Technical purpose
Central inherited firewall control at the org or folder level.

### Why they matter
They are the network equivalent of landing-zone guardrails.

### Example
At the org level:
- deny risky admin ports from public internet
- allow central management ranges

At regulated folders:
- stricter east-west restrictions
- limited egress to approved destinations

### Best practices
- use hierarchical policies for baseline controls
- use project-level firewall rules for workload-specific needs
- document inheritance and exceptions
- test before broad rollout

### Common mistakes
- central teams ignoring hierarchy and managing only per-project rules
- project teams unaware that inherited policy is blocking traffic
- no separation between universal baseline and application exceptions

---

## 7.9 Firewall troubleshooting cues
- route exists but traffic still fails → inspect hierarchical policy, then VPC firewall rules
- project team sees rule allow but connection still blocked → parent policy may deny
- traffic unexpectedly works outbound → implied allow egress may still be active

---

# 8. VPC Flow Logs, Firewall Insights, and Connectivity Tests

These are critical operator tools.

---

## 8.1 VPC Flow Logs

Google documents VPC Flow Logs as sampled, aggregated flow metadata based on 5-tuples. [3]

### Technical purpose
Observe:
- source/destination IPs
- protocol
- ports
- byte/packet patterns
- accepted/denied flow behavior depending on path and logging context

### Important limitation
Flow Logs are **not** full packet capture.

### Example
A private app cannot reach Cloud SQL over private IP. Flow Logs show egress from the app subnet but no successful return traffic.

### Best practices
- enable on important subnets
- export to Logging and analytics destinations where useful
- understand sample/aggregation behavior
- pair with Connectivity Tests for root-cause speed

### Common mistakes
- expecting payload inspection
- enabling everywhere with no retention/cost design
- investigating network issues without knowing which subnet has logs enabled

---

## 8.2 Firewall Insights

### Technical purpose
Surface ineffective, shadowed, overly permissive, or unused firewall rule patterns.

### Best practices
- review regularly
- remove stale rules
- use in governance reviews

### Common mistakes
- firewall rules accumulate forever
- no owner for cleanup
- no pre/post review after migration waves

---

## 8.3 Connectivity Tests

### Technical purpose
Model whether traffic should be able to move between endpoints and where it is blocked.

### Example
Test path from a VM NIC in one project to an internal load balancer or private IP endpoint in another.

### Best practices
- use before and after major networking changes
- use during incident triage
- verify exact source, destination, and protocol assumptions

### Common mistakes
- wrong source/destination chosen
- relying on mental path analysis instead of running a test
- ignoring that modeled reachability and actual runtime behavior may still diverge if service-layer issues exist

---

# 9. Shared VPC

Shared VPC is the standard GCP enterprise networking model.  
Google documents it as a way for resources in multiple projects to securely use a common VPC network. [5]

## 9.1 Technical purpose
Separate:
- **central network ownership**
from
- **application project ownership**

## 9.2 Core entities

### Host project
Project that owns VPC network and subnets.

### Service project
Project that consumes subnets from the host project.

### Shared subnet
Subnet in host project used by resources from service projects.

## 9.3 Why Shared VPC matters
It solves:
- centralized IP planning
- centralized firewall/network governance
- per-team project isolation
- reduced need for peering meshes

## 9.4 Example
Platform team owns:
- `host-prod-network`

Application teams own:
- `payments-prod`
- `orders-prod`
- `analytics-prod`

All deploy resources into approved subnets in the host project.

## 9.5 Best practices
- dedicate host projects to networking only
- split prod and nonprod host projects
- align subnet ownership clearly
- standardize service-project onboarding

## 9.6 Common mistakes
- deploying app resources into host projects
- weak IAM between service and host project admins
- Shared VPC used without subnet governance
- no IPAM discipline across service project growth

---

# 10. VPC Network Peering

## 10.1 Technical purpose
Provide private connectivity between two VPCs.

## 10.2 Key properties
- non-transitive
- route exchange between peers
- useful for direct network-to-network connectivity
- not a replacement for Shared VPC or large-scale hub-and-spoke design

## 10.3 Example
A transitional tools VPC peers with a production shared-services VPC during a migration phase.

## 10.4 Best practices
- keep peering topologies simple
- document import/export expectations
- validate overlapping IP constraints early

## 10.5 Common mistakes
- expecting transit routing
- peering sprawl
- using peering when Shared VPC is the better long-term model

---

# 11. Private Service Access (PSA)

Private Service Access is the managed private networking model used by certain Google-managed services, especially those that require service networking with allocated producer ranges.

## 11.1 Technical purpose
Enable private IP connectivity to managed services using allocated internal address space.

## 11.2 Core entities
- allocated IP range
- private service connection
- service networking relationship
- producer/consumer networking context

## 11.3 Typical use case
Private IP database/service deployment patterns such as managed database connectivity.

## 11.4 Example
A project allocates an internal range for managed services and uses it for private database/service access.

## 11.5 Best practices
- allocate ranges early
- reserve enough address space
- document ownership of service networking allocations
- avoid overlaps with future hybrid or subnet expansion

## 11.6 Common mistakes
- treating PSA like ordinary VPC peering
- too-small allocated ranges
- late allocation after IP plan is already constrained

---

# 12. Private Service Connect (PSC)

PSC is the service-level private publishing and consumption model.

## 12.1 Technical purpose
Provide private endpoints to consume or publish services without exposing entire VPC connectivity.

## 12.2 Core entities

### Service attachment
Producer-side object that publishes a service.

### Endpoint
Consumer-side private endpoint created with a forwarding rule.

### Forwarding rule
Endpoint frontend used to reach the PSC-published service.

### Acceptance model
Producer control over which consumers may attach.

## 12.3 PSC patterns

### PSC to Google APIs / Google services
Private access model to selected Google-managed services.

### PSC to published services
Producer-consumer model between your own services or service providers and consumers.

## 12.4 Example
A platform team publishes an internal payments authorization service through PSC to many consumer projects.

## 12.5 PSC vs PSA
- **PSA** = private producer-managed service networking with allocated ranges
- **PSC** = endpoint/service publication model for service-level private access

## 12.6 Best practices
- use PSC when you want service exposure, not broad VPC adjacency
- define consumer acceptance carefully
- document producer/consumer ownership and support model

## 12.7 Common mistakes
- using peering when one service is all that should be shared
- not understanding consumer acceptance flow
- troubleshooting endpoint failure without checking forwarding rules and service attachments

---

# 13. Cloud Router

Cloud Router is the dynamic route exchange service based on BGP. Google documents it as the mechanism for exchanging routes between VPC and peer networks such as VPN and Interconnect. [6]

## 13.1 Technical purpose
Learn and advertise routes dynamically.

## 13.2 Core entities
- router
- peer
- ASN
- BGP session
- advertised routes
- learned routes

## 13.3 Technical role
Cloud Router is central to:
- HA VPN
- Dedicated Interconnect
- Partner Interconnect
- many serious hybrid routing designs

## 13.4 Example
On-prem routers establish BGP with Cloud Router over HA VPN, and selected branch CIDRs become reachable in the VPC.

## 13.5 Best practices
- review advertisement mode and route scope
- plan ASNs intentionally
- monitor BGP session health
- control route visibility by environment

## 13.6 Common mistakes
- over-advertising prefixes
- no documentation of learned routes
- hybrid routing assumptions not tested during failover

---

# 14. Cloud NAT

Cloud NAT provides outbound internet access for instances without external IP addresses.

## 14.1 Technical purpose
Allow private workloads to reach public destinations without assigning public IPs directly.

## 14.2 Core entities
- Cloud NAT gateway config
- Cloud Router association
- NAT IPs
- port allocation behavior

## 14.3 Example
Private GKE nodes access package repositories and external APIs using Cloud NAT.

## 14.4 Best practices
- monitor port utilization
- size NAT IPs appropriately
- reduce unnecessary internet egress with private connectivity where possible
- validate NAT behavior during scale tests

## 14.5 Common mistakes
- no NAT capacity review
- assuming NAT is a firewall
- outbound failures misdiagnosed as DNS or app issues when port exhaustion is the actual problem

---

# 15. Network Connectivity Center (NCC)

Google documents NCC as a hub-and-spoke enterprise connectivity suite. [7]

## 15.1 Technical purpose
Centralize multi-site, multi-VPC, and hybrid connectivity governance.

## 15.2 Core entities
- hub
- spoke
- spoke type
- propagation/governance structure

## 15.3 Typical spoke types
- VPN
- Dedicated Interconnect
- Partner Interconnect
- router appliance

## 15.4 Example
A global enterprise connects many branches and multiple cloud regions to a central hub model rather than building ad hoc connectivity everywhere.

## 15.5 Best practices
- use NCC when topology scale justifies it
- define spokes by trust and route domain
- document hub ownership and change controls

## 15.6 Common mistakes
- choosing NCC for very simple topologies
- no clear route governance
- weak spoke naming and ownership standards

---

# 16. Cloud VPN

## 16.1 Technical purpose
Provide encrypted IPSec tunnels between Google Cloud and peer networks.

## 16.2 Core entities
- VPN gateway
- tunnel
- peer gateway
- shared secret
- Cloud Router integration in dynamic routing designs

## 16.3 HA VPN
The production pattern is generally **HA VPN**, not ad hoc single tunnel setups.

## 16.4 Example
Two data centers connect to Google Cloud through redundant HA VPN tunnels and Cloud Router BGP peers.

## 16.5 Best practices
- use HA designs
- pair with Cloud Router for dynamic routing where possible
- test tunnel failover
- document crypto and routing dependencies

## 16.6 Common mistakes
- one-tunnel production design
- no failover testing
- hybrid route assumptions only documented, not validated

---

# 17. Cloud Interconnect

Google documents Cloud Interconnect as the private connectivity offering with Dedicated and Partner Interconnect, usually combined with Cloud Router. [8]

## 17.1 Technical purpose
Provide private, high-capacity, predictable connectivity from on-premises networks to Google Cloud.

## 17.2 Core entities
- Dedicated Interconnect
- Partner Interconnect
- VLAN attachment
- Cloud Router
- redundancy design
- Interconnect location

## 17.3 Example
A regulated production environment uses redundant Partner Interconnect or Dedicated Interconnect links plus Cloud Router and VPN backup.

## 17.4 Best practices
- design redundant paths explicitly
- separate prod and nonprod connectivity
- validate failover behavior
- align route advertisement with environment segmentation

## 17.5 Common mistakes
- assuming one Interconnect path is enough
- no backup path
- route failover never tested

---

# 18. Cloud DNS

Google documents Cloud DNS as providing public and private zones as well as forwarding and peering zones. [9][10][11]

## 18.1 Technical purpose
Provide authoritative and internal DNS resolution.

## 18.2 Core entities
- public zone
- private zone
- forwarding zone
- peering zone
- record set
- DNS policy
- response policy
- reverse lookup zone
- DNSSEC

---

## 18.3 Public zones

### Technical purpose
Public internet DNS for domains you own.

### Best practices
- automate record management
- track ownership of registrar and DNS changes
- protect change workflows

---

## 18.4 Private zones

### Technical purpose
DNS visible only from associated VPCs.

### Example
`db.internal.example.com` resolves privately only from application VPCs.

### Best practices
- keep private naming conventions consistent
- document zone-to-network associations
- centralize shared service zones where appropriate

### Common mistakes
- associating private zones to wrong networks
- split-horizon behavior not documented

---

## 18.5 Forwarding zones

Google documents forwarding zones as private zones that forward DNS queries to configured targets. [11]

### Technical purpose
Forward selected queries to on-prem or external DNS.

### Example
`corp.local` queries forward to on-prem DNS.

### Best practices
- use conditional forwarding only where needed
- verify routing and firewall path to DNS targets
- avoid ambiguous overlapping zones

### Common mistakes
- forwarding loops
- target resolvers unreachable from Google Cloud
- using forwarding when private zone association was enough

---

## 18.6 Peering zones

Google documents peering zones as private zones that enable DNS requests between Cloud DNS zones in different VPC networks. [10]

### Technical purpose
Enable cross-network DNS visibility without copying records manually.

### Best practices
- use for structured multi-network DNS sharing
- keep DNS peering design aligned to network architecture
- remember DNS peering is not VPC peering

### Common mistakes
- confusing peering zones with network peering
- manual DNS duplication instead of proper peering-zone design

---

## 18.7 Reverse lookup private zones

### Technical purpose
Provide PTR-style internal reverse resolution patterns where needed.

### Best practices
- use only where reverse resolution has operational value
- document ownership and update process

---

## 18.8 DNS policies and response policies

### DNS policies
Influence DNS behavior in hybrid and network resolution scenarios.

### Response policies
Override, block, or rewrite answers in security and control use cases.

### Best practices
- treat response policies as security-sensitive controls
- test carefully before rollout
- document exception handling

### Common mistakes
- DNS overrides introduced with no rollback plan
- no explanation to app teams when responses differ from upstream DNS

---

## 18.9 DNSSEC

### Technical purpose
Protect DNS integrity for public zones.

### Best practices
- use where public DNS integrity matters
- coordinate registrar and operational steps carefully

### Common mistakes
- enabling without process readiness
- public DNS changes made without DNSSEC-aware validation

---

# 19. Cloud Load Balancing

Load balancing in GCP is a product family, not a single thing.

## 19.1 Core shared entities

### Backend service
Google documents backend services as defining how traffic is distributed and including health checks, protocol, timeouts, session behavior, and more. [12]

### Backend bucket
Static content backend for supported HTTP(S) delivery patterns.

### NEG (network endpoint group)
Endpoint abstraction for certain backend types, including zonal, serverless, hybrid/internet patterns depending on LB family.

### URL map
Host/path routing policy for application load balancing. [13]

### Target proxy
Frontend proxy resource for HTTP(S), SSL, or TCP proxy families.

### Forwarding rule
Frontend listener binding IP/port to target object.

### Health check
Backend availability signal.

### SSL certificate
Frontend TLS credential.

---

## 19.2 Global external Application Load Balancer

### Technical purpose
Global Layer 7 application delivery through Google’s edge.

### Best fit
- public web apps
- global APIs
- multi-region web entry

### Example
A global ecommerce frontend uses:
- global external ALB
- URL map
- backend services in multiple regions
- Cloud Armor
- Cloud CDN

### Best practices
- pair with Cloud Armor for public apps
- use meaningful health checks
- keep URL map rules testable
- separate static and dynamic backends

### Common mistakes
- routing complexity with no tests
- health checks that do not reflect real readiness
- backend service sprawl with inconsistent settings

---

## 19.3 Regional external Application Load Balancer

### Technical purpose
Regional Layer 7 external application delivery.

### Best fit
Regional app patterns or locality-controlled designs.

---

## 19.4 Internal Application Load Balancer

### Technical purpose
Private L7 balancing for east-west traffic.

### Best fit
- internal APIs
- microservices
- shared platform services

### Example
Internal ALB fronts region-local microservices for private consumers only.

---

## 19.5 Proxy Network Load Balancer

### Technical purpose
Transport-layer proxy balancing with backend-service model features.

### Best fit
Advanced TCP proxy patterns.

---

## 19.6 Passthrough Network Load Balancer

### Technical purpose
Transport balancing without full proxy termination.

### Best fit
High-performance or packet-preservation-sensitive patterns.

## 19.7 Load balancer selection rule
Choose by:
- internal or external
- global or regional
- Layer 7 or transport
- proxy or passthrough
- backend type support

### Common mistake
Choosing by product name only.

---

## 19.8 Health checks

### Best practices
- lightweight but meaningful
- app-aware readiness
- avoid “always 200” fake health endpoints

### Common mistakes
- too strict during startup
- too weak to detect real failure

---

## 19.9 URL maps

Google documents URL maps as routing objects for host/path-based decisions. [13]

### Example
- `/api/*` → API backend service
- `/static/*` → backend bucket
- `admin.example.com` → admin backend service

### Best practices
- use tests
- version route changes
- keep host/path logic understandable

### Common mistakes
- too many ad hoc rules
- route behavior not validated before release

---

# 20. Cloud CDN

Google documents Cloud CDN as edge caching integrated with Application Load Balancers and driven by HTTP caching semantics. [14][15]

## 20.1 Technical purpose
Reduce latency and backend load through edge caching.

## 20.2 Core entities
- CDN-enabled backend
- cache mode
- cache key
- origin response headers
- invalidation

## 20.3 Example
Static product images are cached at edge; personalized account endpoints are not.

## 20.4 Best practices
- define which paths are cacheable
- control behavior with `Cache-Control` or `CDN-Cache-Control`
- separate static from dynamic traffic clearly
- use invalidation intentionally, not as a default deployment action

## 20.5 Common mistakes
- caching personalized content
- origin headers not aligned to desired behavior
- broad invalidations used too often

## 20.6 Invalidation notes
Google documents invalidation as path and optional host based, and warns to match pre-rewrite host/path behavior in rewrite scenarios. [16]

---

# 21. Cloud Armor

Google documents Cloud Armor as edge security with security policies, WAF features, and DDoS protections, including preconfigured WAF rules and tuning guidance. [17][18]

## 21.1 Technical purpose
Protect public applications from malicious traffic at the edge.

## 21.2 Core entities
- security policy
- rule
- preconfigured WAF rules
- rate limiting / traffic controls
- edge protection posture

## 21.3 Example
A public checkout service uses:
- allowlist for admin endpoints
- rate limiting
- preconfigured WAF rules
- tuned exclusions for false positives

## 21.4 Best practices
- start in visibility/tuning mode when needed
- use stable managed rules in most production cases
- tune signatures rather than disabling protection broadly
- centralize baseline policy and app-specific exceptions

## 21.5 Common mistakes
- enabling many rules without false-positive review
- no ownership for tuning
- relying on Cloud Armor to replace secure coding or backend auth controls

---

# 22. Packet Mirroring and Cloud IDS

Google documents Cloud IDS as depending on Packet Mirroring. [19][20]

## 22.1 Packet Mirroring

### Technical purpose
Copy selected traffic for inspection or analytics.

### Core entities
- mirrored source
- mirroring policy
- collector or IDS endpoint
- filters

### Best practices
- mirror only required subnets and flows
- filter for high-value traffic
- watch cost and downstream capacity

### Common mistakes
- mirroring entire VPCs unnecessarily
- no consumer/analysis workflow

---

## 22.2 Cloud IDS

### Technical purpose
Managed intrusion detection on mirrored traffic.

### Core entities
- IDS endpoint
- packet mirroring policy
- threat logs
- exception/tuning model

### Example
Internet-bound and internet-facing traffic from critical subnets is mirrored to Cloud IDS for inspection.

### Best practices
- start with critical paths
- tune mirroring filters to reduce cost
- route logs into security operations workflows

### Common mistakes
- enabling IDS with no investigation process
- assuming IDS replaces firewalls or WAF
- inspecting too much low-value traffic

---

# 23. API Gateway

Google documents API Gateway as a managed API layer integrated with Google Cloud for authorization, traffic handling, and monitoring. [21]

## 23.1 Technical purpose
Provide simpler managed API ingress for Google Cloud backends.

## 23.2 Core entities
- API
- API config
- gateway
- managed service definition

## 23.3 How it works
1. define API
2. create API config from OpenAPI or supported gRPC config
3. deploy config to gateway
4. route to backend

Google documents API creation, config creation, and deployment as distinct steps. [22][23]

## 23.4 Example
A Cloud Run gRPC service is exposed through API Gateway with gateway-level auth and monitoring.

## 23.5 Best practices
- version API configs
- keep backend contract aligned to OpenAPI/gRPC config
- use for straightforward gateway needs
- separate environments/projects clearly

## 23.6 Common mistakes
- no config promotion discipline
- gateway deployed in wrong project
- using API Gateway where lifecycle-rich API management is required

---

# 24. Apigee

Google documents Apigee around organizations, environments, environment groups, API proxies, revisions, and deployments. [24][25]

## 24.1 Technical purpose
Provide full API management and API product platform capabilities.

## 24.2 Core entities

### Organization
Top-level Apigee container.

### Environment
Runtime execution context for deployed proxies.

### Environment group
Grouping used for hostname/base path exposure patterns.

### API proxy
Deployable API mediation artifact.

### Proxy revision
Versioned deployable iteration.

### Deployment
Binding of proxy revision to environment.

### API product
Packaged API exposure model.

### Developer
Consumer identity model.

### Developer app
App credential container for API product consumption.

## 24.3 Example
A partner API platform uses:
- separate dev, test, prod environments
- proxy revision promotion
- API products
- developer apps
- managed analytics and policy mediation

## 24.4 Best practices
- treat proxy revisions as release artifacts
- separate environments clearly
- design product and consumer model intentionally
- choose Apigee when API platform lifecycle matters

## 24.5 Common mistakes
- using Apigee for trivial gateway-only use cases
- poor revision lifecycle management
- no product/developer ownership model

---

# 25. API Gateway vs Apigee

- **API Gateway**: simpler managed gateway
- **Apigee**: full API management platform

Choose **API Gateway** when:
- you need straightforward managed ingress
- policy/product lifecycle is limited
- backend exposure is relatively simple

Choose **Apigee** when:
- you need API products
- developer onboarding matters
- mediation/policy depth matters
- environment promotion and lifecycle governance are core

---

# 26. End-to-End Architecture Example

A multinational enterprise needs:
- centralized networking
- private managed database access
- internal east-west service exposure
- global web entry
- WAF
- CDN
- hybrid connectivity
- partner APIs
- selective IDS

A strong design:

### Core network
- custom mode prod Shared VPC
- regional app and data subnets
- reserved secondary ranges for GKE

### Reachability
- Cloud Router with Interconnect and HA VPN
- dynamic route control
- Cloud NAT for private outbound internet

### Private access
- PSA for managed database/private managed services
- PSC for internal private service publication across projects

### DNS
- Cloud DNS public zones for internet domains
- private zones for internal names
- forwarding zones to on-prem DNS
- peering zones across selected multi-network designs

### Edge
- global external ALB
- Cloud Armor
- Cloud CDN
- managed certificates
- URL maps for app segmentation

### APIs
- API Gateway for internal platform APIs
- Apigee for partner-facing governed APIs

### Inspection and diagnostics
- Flow Logs on key subnets
- Connectivity Tests in change validation
- Firewall Insights for hygiene
- Packet Mirroring + Cloud IDS on high-value traffic only

This is the Week 2 principle:

> the packet path, DNS path, policy path, and edge path must be designed together.

---

# 27. Troubleshooting Framework

## 27.1 Traffic cannot connect
1. Source IP correct?
2. Route exists?
3. More specific route or lower-priority route winning?
4. Hierarchical firewall policy blocking?
5. VPC firewall rule blocking?
6. Service-layer issue after network path?

## 27.2 Hybrid path broken
1. VPN or Interconnect healthy?
2. Cloud Router BGP session up?
3. Route advertised and learned?
4. Prefix scope correct?
5. NAT unexpectedly involved?

## 27.3 Private managed service unreachable
1. Using correct model: PSA or PSC?
2. Allocated range exists and is non-overlapping?
3. Service attachment / endpoint configured?
4. DNS resolving private target correctly?

## 27.4 DNS resolution wrong
1. Public or private zone?
2. Private zone visible to correct VPC?
3. Forwarding target reachable?
4. Peering zone configured?
5. Response policy changing answer?
6. DNSSEC issue in public DNS path?

## 27.5 Edge app failing
1. Backend service healthy?
2. URL map correct?
3. Certificate valid?
4. Cloud Armor blocking?
5. Cloud CDN caching wrong response?
6. Backend bucket or NEG mismatch?

## 27.6 API failing
1. API config current?
2. Gateway deployed?
3. Backend reachable?
4. Auth or quota issue?
5. In Apigee: proxy deployed to right environment/group?

---

# 28. Common Week 2 Mistakes

- forgetting that GCP VPC is global
- weak subnet and secondary-range planning
- relying on implied firewall behavior in mature environments
- broad tag-based firewalling with weak governance
- not understanding parent hierarchical firewall policy effects
- confusing Shared VPC, peering, PSA, and PSC
- poor Cloud Router advertisement control
- no NAT capacity planning
- hybrid DNS not designed with network path validation
- wrong load balancer family chosen
- CDN enabled without cache-semantics review
- WAF rules enabled with no tuning
- mirroring too much traffic into IDS
- using API Gateway when Apigee is required, or Apigee when the problem is simple gateway ingress

---

# 29. Revision Checklist

You should be able to explain clearly:

- how global VPC and regional subnets interact
- why auto mode is usually weaker than custom mode
- how primary, secondary, and alias IP ranges are used
- how route specificity and priority affect path choice
- how implied firewall rules differ from explicit rules
- how VPC firewall rules differ from hierarchical firewall policies
- why service-account firewall targets can be better than tags
- what Flow Logs, Firewall Insights, and Connectivity Tests each do
- how Shared VPC differs from VPC peering
- how PSA differs from PSC
- what Cloud Router does in VPN and Interconnect
- what NAT port exhaustion means operationally
- when NCC is useful
- how private zones, forwarding zones, peering zones, reverse lookup zones, DNS policies, and response policies differ
- how each load balancer family differs
- how backend services, URL maps, proxies, forwarding rules, NEGs, health checks, and certificates fit together
- how Cloud Armor, Cloud CDN, Packet Mirroring, and Cloud IDS fit together
- when to choose API Gateway and when to choose Apigee

---

# 30. Memory Anchors

- **VPC** = global network boundary
- **subnet** = regional address segment
- **primary range** = main subnet IP space
- **secondary range** = extra CIDR for Pods/Services and advanced patterns
- **alias IP** = extra interface-address allocation model
- **route** = next-hop logic
- **implied firewall** = default allow egress / deny ingress
- **VPC firewall rule** = workload traffic filter
- **hierarchical firewall policy** = central inherited firewall guardrail
- **tag** = metadata target selector
- **service account target** = identity-based firewall selector
- **Flow Logs** = traffic metadata
- **Connectivity Tests** = modeled reachability
- **Shared VPC** = central network, separate projects
- **peering** = direct private VPC-to-VPC link
- **PSA** = private managed-service networking
- **PSC** = private service publication/consumption
- **Cloud Router** = BGP route exchange
- **Cloud NAT** = outbound internet for private resources
- **NCC** = hub-and-spoke connectivity
- **Cloud VPN** = encrypted hybrid tunnel
- **Interconnect** = private dedicated/partner connectivity
- **Cloud DNS** = managed DNS
- **forwarding zone** = DNS forwarder
- **peering zone** = cross-network DNS visibility
- **backend service** = load-balancer behavior core
- **URL map** = host/path routing
- **Cloud Armor** = edge protection and WAF
- **Cloud CDN** = edge caching
- **Packet Mirroring** = copy traffic for inspection
- **Cloud IDS** = managed IDS from mirrored traffic
- **API Gateway** = simple managed API ingress
- **Apigee** = full API platform

---

# 31. Official References

## Core networking
- VPC overview  
  https://docs.cloud.google.com/vpc/docs/overview

- VPC networks  
  https://docs.cloud.google.com/vpc/docs/vpc

- VPC Flow Logs  
  https://docs.cloud.google.com/vpc/docs/flow-logs

- Configure VPC Flow Logs  
  https://docs.cloud.google.com/vpc/docs/using-flow-logs

- Access flow logs  
  https://docs.cloud.google.com/vpc/docs/access-flow-logs

- Organization policy constraints for VPC Flow Logs  
  https://docs.cloud.google.com/vpc/docs/org-policy-flow-logs

## Shared VPC and private connectivity
- Shared VPC overview  
  https://docs.cloud.google.com/vpc/docs/shared-vpc

- Shared VPC provisioning  
  https://docs.cloud.google.com/vpc/docs/provisioning-shared-vpc

- Private Service Connect  
  https://cloud.google.com/vpc/docs/private-service-connect

- Private services access  
  https://cloud.google.com/vpc/docs/private-services-access

## Hybrid connectivity
- Network Connectivity concepts  
  https://docs.cloud.google.com/network-connectivity/docs/concepts

- Choosing a Network Connectivity product  
  https://docs.cloud.google.com/network-connectivity/docs/how-to/choose-product

- Cloud Router documentation  
  https://docs.cloud.google.com/network-connectivity/docs/router

- Cloud Interconnect overview  
  https://docs.cloud.google.com/network-connectivity/docs/interconnect/concepts/overview

## DNS
- Cloud DNS overview  
  https://docs.cloud.google.com/dns/docs/overview

- DNS zones overview  
  https://docs.cloud.google.com/dns/docs/zones/zones-overview

- Managed zones  
  https://docs.cloud.google.com/dns/docs/zones

- Forwarding zones  
  https://docs.cloud.google.com/dns/docs/zones/forwarding-zones

- Peering zones  
  https://docs.cloud.google.com/dns/docs/zones/peering-zones

- Cloud DNS best practices  
  https://docs.cloud.google.com/dns/docs/best-practices

## Load balancing and edge
- Application Load Balancer overview  
  https://docs.cloud.google.com/load-balancing/docs/application-load-balancer

- Backend services overview  
  https://docs.cloud.google.com/load-balancing/docs/backend-service

- URL maps  
  https://docs.cloud.google.com/load-balancing/docs/url-map

- Cloud Armor overview  
  https://docs.cloud.google.com/armor/docs/cloud-armor-overview

- Cloud Armor best practices  
  https://docs.cloud.google.com/armor/docs/best-practices

- Cloud Armor preconfigured WAF rules  
  https://docs.cloud.google.com/armor/docs/waf-rules

- Tune preconfigured WAF rules  
  https://docs.cloud.google.com/armor/docs/rule-tuning

- Cloud CDN overview  
  https://docs.cloud.google.com/cdn/docs/overview

- Cloud CDN setup overview  
  https://docs.cloud.google.com/cdn/docs/using-cdn

- Cloud CDN caching overview  
  https://docs.cloud.google.com/cdn/docs/caching

- Cloud CDN invalidation overview  
  https://docs.cloud.google.com/cdn/docs/cache-invalidation-overview

- Invalidate cached content  
  https://docs.cloud.google.com/cdn/docs/invalidating-cached-content

## Inspection
- Cloud IDS overview  
  https://docs.cloud.google.com/intrusion-detection-system/docs/overview

- Configure Cloud IDS  
  https://docs.cloud.google.com/intrusion-detection-system/docs/configuring-ids

- Cloud IDS best practices  
  https://docs.cloud.google.com/intrusion-detection-system/docs/best-practice

## API exposure
- About API Gateway  
  https://docs.cloud.google.com/api-gateway/docs/about-api-gateway

- Create an API  
  https://docs.cloud.google.com/api-gateway/docs/creating-api

- Create an API config  
  https://docs.cloud.google.com/api-gateway/docs/creating-api-config

- Deploy an API to a gateway  
  https://docs.cloud.google.com/api-gateway/docs/deploying-api

- API Gateway how-to guides  
  https://docs.cloud.google.com/api-gateway/docs/how-to

- Apigee organization structure  
  https://docs.cloud.google.com/apigee/docs/api-platform/fundamentals/organization-structure

- Apigee environments and environment groups  
  https://docs.cloud.google.com/apigee/docs/api-platform/fundamentals/environments-overview

- Working with Apigee environments  
  https://docs.cloud.google.com/apigee/docs/api-platform/fundamentals/environments-working-with

- Apigee deployment overview  
  https://docs.cloud.google.com/apigee/docs/api-platform/deploy/ui-deploy-overview

---

# 32. Final Summary

Week 2 is about **how GCP networks are built, connected, secured, resolved, and exposed**.

The strongest mental model is:

- **VPC, subnets, secondary ranges, alias IPs, and routes** define addressability and packet movement
- **firewall rules and hierarchical policies** define allowed traffic
- **Shared VPC, peering, PSA, and PSC** define private connectivity choices
- **Cloud Router, NAT, VPN, Interconnect, and NCC** define hybrid and large-scale transport
- **Cloud DNS** defines internal and external name resolution
- **load balancers, Cloud Armor, and Cloud CDN** define ingress, protection, and acceleration
- **Packet Mirroring and Cloud IDS** define inspection
- **API Gateway and Apigee** define API exposure strategies

When you can explain the **packet path, policy path, DNS path, and edge path** together, Week 2 is understood architecturally.
