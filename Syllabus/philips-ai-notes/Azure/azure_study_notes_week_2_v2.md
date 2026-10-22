# Azure Study Notes — Week 2 v2
## Networking, Private Connectivity, Hybrid Routing, Edge Traffic, and API Exposure

> This is the **9.6+ version** of Week 2.  
> It is written as **deep expert-level Azure networking study material** with:
> - technical purpose
> - core entities
> - route and policy mechanics
> - control-plane and data-plane interactions
> - architecture fit
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** tight structure, high information density, minimal filler.

---

# 1. Why Week 2 Matters

Week 2 is about **how Azure makes connectivity real**.

Once identity and governance exist, the next foundational question is:

> **How do packets move, how are they filtered, what names do clients resolve, what path wins, and what component is actually exposing the service?**

This is where many Azure designs become either:
- predictable and supportable  
or
- fragile, surprising, and expensive to troubleshoot.

The reason is simple:

A single request can depend on all of the following at once:
- address planning
- peering or transit topology
- system routes, UDRs, and BGP propagation
- NSGs, security admin rules, Firewall policy, or WAF policy
- public DNS vs private DNS resolution
- edge or reverse-proxy selection
- hybrid path selection
- backend health-probe behavior

Week 2 is therefore not “networking basics.”  
It is the architecture of:
- packet path
- route intent
- private service access
- hybrid transit
- edge exposure
- API mediation
- diagnosability

The core idea is:

> **In Azure, successful network design means route selection, security filtering, DNS, and service exposure all agree on the same intended path.**

---

# 2. Learning Outcomes

By the end of Week 2, you should be able to:

- explain **Azure Virtual Network** address planning, subnet design, NSG and ASG mechanics, UDRs, route precedence, subnet delegation, service chaining, service endpoints, private endpoints, Private Link, and peering
- explain **effective route** behavior and how **system routes**, **BGP routes**, and **user-defined routes** interact
- explain **Azure Virtual Network Manager** network groups, connectivity configs, security admin rules, deployment scope, and platform-scale governance
- explain **Azure Virtual WAN** virtual hubs, hub route tables, route propagation, routing intent, secured virtual hubs, and classic hub-spoke tradeoffs
- explain **Azure Firewall**, **Azure Firewall Policy**, **Firewall Manager**, **NAT Gateway**, and **Azure Bastion**, including outbound ownership design and rule-processing pitfalls
- explain **VPN Gateway** and **ExpressRoute**, including active-active, BGP, peering intent, route filters, asymmetric routing risks, and failover-validation methodology
- explain **Azure DNS**, **Azure Private DNS**, and **DNS Private Resolver**, including hybrid recursion, split-horizon, private endpoint DNS, and cross-network lookup patterns
- explain **Azure Load Balancer**, **Application Gateway**, **Front Door**, **Traffic Manager**, and **CDN**, and choose correctly between them
- explain **DDoS Protection**, **WAF**, and **Network Watcher** as protective and diagnostic layers
- explain **Azure API Management** policies, scope layering, products, subscriptions, revisions, versions, gateways, backend routing, and operational failure modes
- troubleshoot route conflicts, DNS split-brain, private endpoint mis-resolution, hybrid route asymmetry, egress bypass, health-probe failures, WAF false positives, and APIM policy regressions

---

# 3. The Packet-Path Mental Model

Every Azure networking problem can be decomposed into six questions:

1. **Where is the destination address?**
2. **Which route wins?**
3. **Which security control evaluates the packet?**
4. **Which name did the client resolve?**
5. **Which edge or gateway accepted the request?**
6. **Which health or policy decision removed a backend from service?**

That becomes this layered model:

1. **Address layer**  
   VNet, subnet, IP plan, non-overlap, service-specific subnet needs

2. **Route layer**  
   system route, BGP route, UDR, route-table association, transit model

3. **Filter layer**  
   NSG, security admin rule, Firewall, WAF, APIM policy

4. **Name layer**  
   public DNS, private DNS, resolver chain, forwarding, split-horizon

5. **Exposure layer**  
   Load Balancer, App Gateway, Front Door, Traffic Manager, APIM

6. **Health and observability layer**  
   probes, backend health, effective routes, IP flow verify, flow logs, connection monitor

The Week 2 principle is:

> **A packet succeeds only if all six layers agree. Troubleshooting fails when teams inspect only one.**

---

# 4. Architecture Selection Before Product Selection

Avoid product-first design.  
Start with traffic intent.

## 4.1 Regional east-west private app traffic
Use:
- VNet
- subnets
- peering or managed transit
- NSGs and route tables where needed

## 4.2 Private access to Azure PaaS
Usually prefer:
- Private Link / private endpoints
- Private DNS
- DNS Private Resolver for hybrid resolution where needed

Use service endpoints only when their tradeoffs fit.

## 4.3 Enterprise hybrid transit
Use:
- VPN Gateway for encrypted internet-based hybrid connectivity
- ExpressRoute for dedicated private connectivity
- Virtual WAN when managed transit hub scale and branch integration justify it

## 4.4 Centralized inspection and controlled egress
Use:
- Azure Firewall when you need inspection and policy
- NAT Gateway when you need scalable outbound SNAT, not full firewall logic
- both together only when outbound ownership is explicitly designed

## 4.5 Regional L7 ingress
Use:
- Application Gateway

## 4.6 Global edge ingress and acceleration
Use:
- Front Door

## 4.7 DNS-based steering only
Use:
- Traffic Manager

## 4.8 API governance
Use:
- API Management

## 4.9 Estate-wide VNet governance
Use:
- Azure Virtual Network Manager

---

# 5. Azure Virtual Network: Addressing, Segmentation, and Local Routing

Azure Virtual Network is the private networking foundation. Microsoft documents VNet peering, service endpoints, Private Link, and service network-isolation patterns. [1][2][3][4][5]

## 5.1 Technical purpose

Use VNet for:
- private address containment
- subnet-based segmentation
- hybrid termination
- private service integration
- route and policy application point for workloads

## 5.2 Core entities

### VNet
Private address boundary.

### Address space
The aggregate CIDR allocated to that VNet.

### Subnet
Placement and segmentation boundary.

### NIC
Attachment point for VM network identity and possible NSG application.

### NSG
Stateful packet filter.

### ASG
Logical grouping to simplify NSG targeting.

### Route table
Subnet-associated collection of UDRs.

### UDR
Custom route overriding default path selection within supported scenarios.

### Service endpoint
VNet identity-based connectivity to supported Azure services.

### Private endpoint
Private IP mapped to a specific target service.

### Peering
Private Azure backbone connectivity between VNets.

---

## 5.3 IP planning and subnet strategy

### Best practices
- reserve growth space for new subnets, Private Link expansion, and future hybrid integration
- avoid overlapping address spaces with on-premises, partner networks, and future cloud estates
- segment by lifecycle and control domain, not only by environment label
- preserve service-required subnets and do not over-pack generic workloads into them

### Common mistakes
- single large “shared subnet” for unrelated workloads
- address spaces too small for future private endpoints or scale sets
- overlapping IP plan discovered only at peering or ExpressRoute stage

### Expert insight
Bad CIDR planning is often misdiagnosed later as:
- peering problem
- ExpressRoute problem
- DNS problem  
when the actual issue is simply address overlap or no room to evolve.

---

## 5.4 NSGs and ASGs: real mechanics

### NSG purpose
Allow/deny traffic at L3/L4, statefully.

### Subnet NSG vs NIC NSG
Both can matter.  
A mistake here is thinking only one applies conceptually.

### Best practices
- use subnet NSGs for broad segment controls
- use NIC NSGs sparingly when workload-specific exception handling is justified
- use ASGs to reduce IP-driven rule sprawl
- document which layer owns what: subnet or NIC

### Common mistakes
- subnet NSG and NIC NSG conflicts are not documented
- large rulebases encoded as source/destination IP lists instead of ASGs
- NSGs expected to solve L7 problems that require WAF or Firewall

### Expert nuance
Many troubleshooting delays happen because teams inspect the “obvious” NSG but not the additional NSG attached at the other scope.

---

## 5.5 Route precedence and effective routes

This is one of the most important Week 2 topics.

### Route sources
- system routes
- BGP routes
- user-defined routes

### Operational truth
Do not reason about routing from memory.  
Reason from **effective routes**.

### Best practices
- inspect effective routes before changing architecture
- document route intent at subnet level
- verify next-hop behavior after every peering, gateway, or firewall insertion change
- explicitly model how default routes and hybrid advertisements interact

### Common mistakes
- assuming a UDR always “wins” in any scenario without validating applicability
- forgetting a route table is associated to the wrong subnet
- unexpected BGP propagation changing intended egress or hybrid path
- default route to firewall added, but subnet never actually associated with the route table

### Failure pattern examples
- A spoke is supposed to egress through a hub firewall, but only some subnets do because only some subnets have the UDR association.
- ExpressRoute introduces learned routes that alter return path, causing asymmetric troubleshooting confusion.
- A peered VNet appears connected, but a more-specific route elsewhere wins and traffic never follows the expected hub path.

---

## 5.6 Service chaining

Service chaining means deliberately steering traffic through a hub firewall or NVA using UDRs and peering.

### Best practices
- define service-chain intent clearly by subnet
- validate return path, not only forward path
- test east-west and north-south flows separately
- document exceptions for private endpoints and service-specific traffic

### Common mistakes
- forward path reaches firewall, return path bypasses it
- app teams assume hub-spoke means implicit transit for all flows
- security insertion changes path but probe or backend expectations are not updated

---

## 5.7 Service endpoints vs private endpoints

Microsoft documents service endpoints and Private Link as distinct methods for Azure service isolation. [2][3][4][5]

### Service endpoints
Good when:
- supported service
- public endpoint model is still acceptable
- you want VNet identity restriction without full private IP mapping

### Private endpoints
Good when:
- private IP-based service access is required
- public exposure should be removed or strongly de-emphasized
- DNS-driven private access path is acceptable and managed

### Best practices
- default to private endpoints for strong isolation-sensitive PaaS access
- use service endpoints only when their security and operational model fits
- evaluate management complexity before large-scale endpoint rollout

### Common mistakes
- teams treat service endpoints and private endpoints as interchangeable
- private endpoints deployed with no DNS design
- service-endpoint path works during pilot but later fails governance expectations

### Migration pattern nuance
Moving from service endpoints to private endpoints is often:
- a DNS migration
- a client-resolution migration
- an approval/governance migration  
not merely a networking toggle.

---

## 5.8 Private Link approval and operational ownership

### Important mechanics
A private endpoint can exist while:
- approval is pending
- DNS is wrong
- NSGs are permissive
- application still resolves the public endpoint

### Best practices
- define who approves private endpoint connections
- define who owns the private DNS zone
- define who validates application FQDN behavior
- create per-service onboarding runbooks

### Common mistakes
- consumer team assumes service owner approved the link
- endpoint is approved, but no one created the DNS zone link
- records exist, but clients use different resolvers than expected

---

## 5.9 VNet troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| VM cannot reach another subnet | filter/route | subnet NSG, NIC NSG, effective routes, next hop |
| Hub firewall sees no traffic from a spoke | route table | subnet association, UDR prefix, peering/service chaining path |
| Private endpoint deployed but traffic still public | DNS | private zone records, resolver path, cache, endpoint approval |
| Peering exists but communication still fails | route/address | overlapping CIDR, effective routes, NSG/UDR, gateway transit assumptions |
| Only some workloads work after network change | subnet design | route-table association gaps, mixed NSG placement, delegated subnet constraints |

---

# 6. Azure Virtual Network Manager and Azure Virtual WAN

These are both central-networking services, but they solve different categories of problems.

Microsoft documents Virtual Network Manager integration with Virtual WAN, virtual hub routing, and routing intent. [6][7][8]

## 6.1 Azure Virtual Network Manager (AVNM)

### Technical purpose
Estate-wide orchestration of:
- VNet connectivity
- network grouping
- security admin rules
- central network-governance rollout

### Core entities
- network group
- connectivity configuration
- security admin configuration
- deployment scope
- region targeting
- central admin operating model

### Best practices
- define network groups based on governance intent, not arbitrary labels
- separate connectivity automation from security admin policy ownership
- deploy in phases
- document which environments are centrally managed and which are exceptions

### Common mistakes
- network groups mirror subscription names with no design meaning
- AVNM used before VNet taxonomy is clean
- security admin rules surprise workload teams because there is no exception model

### Expert nuance
AVNM succeeds when it encodes platform standards.  
It fails when it becomes a second place to manually manage one-off network behavior.

---

## 6.2 Security admin rules and precedence thinking

### Important concept
Security admin rules are centralized and meant to simplify at-scale policy.  
Operationally, this means teams must understand how they interact with local NSGs.

### Best practices
- use them for universal or high-confidence controls
- keep emergency exception path documented
- roll out deny-like central controls carefully
- validate critical application patterns before broad deployment

### Common mistakes
- workload team troubleshoots only local NSG
- central rule change affects many VNets at once without staged testing
- there is no single owner of central network security policy

---

## 6.3 Azure Virtual WAN (vWAN)

### Technical purpose
Managed global transit platform for:
- branch connectivity
- hybrid routing
- multi-region hub architecture
- centralized security insertion
- simplified interconnection model

### Core entities
- Virtual WAN
- virtual hub
- hub route table
- route propagation
- VPN gateway
- ExpressRoute gateway
- point-to-site gateway
- Azure Firewall / secured virtual hub
- routing intent

### Best fit
- many branches
- large hybrid estate
- multi-region transit standardization
- desire to reduce bespoke hub VNet routing management

### Common mistake
Treating vWAN as if it were just a classic hub VNet with a nicer UI.

---

## 6.4 Virtual hub routing internals

Microsoft documents virtual hub routing and routing intent concepts. [7][8]

### Important concept
A virtual hub has its own router and route tables.  
Traffic behavior depends on:
- connection association
- propagation
- route-table design
- security insertion
- intent configuration

### Best practices
- explicitly map which connections associate to which route table
- validate route propagation after every topology change
- distinguish branch-to-branch, branch-to-Azure, and internet-bound intents
- build route-validation into change windows

### Common mistakes
- all connections left on defaults with no understanding of propagation consequences
- security insertion assumed, not validated
- internet egress unexpectedly changes after routing-intent rollout

### Failure pattern examples
- Branch traffic intended for inspection bypasses firewall because propagation and association are mismatched.
- Private traffic follows expected path, but Internet traffic changes after routing intent, breaking outbound assumptions.
- Secured virtual hub introduced, but app team still expects classic hub-and-spoke route-table behavior.

---

## 6.5 Classic hub-spoke vs vWAN

### Classic hub-spoke
Better when:
- topology is simpler
- you want explicit custom route-table control
- scale and branch complexity are moderate
- platform team prefers bespoke network composition

### vWAN
Better when:
- branch scale is larger
- managed transit matters
- hub services and connection model justify the abstraction
- operational simplification outweighs custom-control preference

### Common mistake
Choosing vWAN because it sounds more enterprise, not because the transit model needs it.

---

## 6.6 AVNM / vWAN troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Expected central connectivity missing | orchestration | network group membership, config deployment state, target region/scope |
| Security policy breaks traffic estate-wide | admin policy | security admin rule rollout, exception model, affected flow patterns |
| vWAN traffic bypasses inspection | hub routing | association, propagation, intent policy, firewall insertion |
| Internet egress changes after hub update | routing intent | direct access vs forced tunnel, default-route behavior, hub route tables |
| Central network model feels unpredictable | governance design | unclear AVNM vs vWAN responsibility split, default behaviors not documented |

---

# 7. Azure Firewall, Firewall Policy, NAT Gateway, and Bastion

These services define secure egress, filtered ingress, and administrative access.

Microsoft documents Firewall Policy hierarchy, NAT Gateway integration and Bastion overview/configuration. [9][10][11][12][13]

## 7.1 Azure Firewall and policy hierarchy

### Technical purpose
Central filtering service for:
- network rules
- application rules
- DNAT
- centralized egress control
- policy-based operation

### Core entities
- firewall
- firewall policy
- rule collection group
- rule collection
- rule
- DNAT
- network rule
- application rule
- threat-intelligence settings
- policy inheritance model where used

### Expert operational point
Many failed firewall changes are not “missing rule” problems.  
They are:
- wrong rule type
- wrong collection priority
- wrong collection-group priority
- wrong assumption about FQDN vs IP matching
- wrong path due to UDR

### Best practices
- organize rule collection groups by operating domain
- separate DNAT from ordinary egress policy
- document ownership for platform, shared, and application exceptions
- review rule hit logic before adding duplicates

### Common mistakes
- application rule created when traffic actually matches network rule behavior
- DNAT path troubleshot as east-west routing issue
- duplicate rules scattered across many collections
- firewall added but routes never steer traffic there

---

## 7.2 Azure Firewall Manager

### Technical purpose
Central orchestration and policy standardization, especially in complex topologies and secured-hub patterns.

### Best fit
- many firewalls
- large estates
- standard policy lifecycle across hubs

### Common mistake
Using Firewall Manager without clear policy ownership or promotion process.

---

## 7.3 NAT Gateway and SNAT ownership

Microsoft documents NAT Gateway scaling and Azure Firewall integration patterns. [10]

### Technical purpose
High-scale outbound SNAT for subnets.

### Best practices
- choose who owns outbound public IP identity: NAT Gateway or Firewall
- attach NAT Gateway only to subnets with explicit egress design
- model outbound connection concurrency and port demand
- understand whether inspection, scale, or stable egress identity is the primary requirement

### Common mistakes
- NAT Gateway added after Firewall with no clear reason
- subnets inherit mixed outbound assumptions
- teams think NAT Gateway provides filtering or policy
- port-pressure scenarios ignored

### Expert nuance
NAT Gateway solves outbound scale and identity.  
It does not replace:
- route design
- inspection strategy
- DNS strategy
- application connection-behavior tuning

---

## 7.4 Mixed Firewall + NAT patterns

### Valid pattern
Use Firewall for inspection and NAT Gateway for scalable outbound SNAT in supported architectures where ownership is explicit.

### Anti-pattern
Deploy both because “more network appliances feels safer.”

### Best practices
- document packet flow end to end
- define which component owns internet egress semantics
- validate source IP expectations for downstream allowlists
- test failure behavior and path consistency

---

## 7.5 Azure Bastion

Microsoft documents Bastion as secure private-IP-based RDP/SSH access without public IPs and notes multiple SKUs and configuration differences. [11][12][13]

### Technical purpose
Administrative access plane without public IP exposure on target VMs.

### Best practices
- eliminate VM public IPs where Bastion is the intended admin path
- choose SKU based on operational needs, not default convenience
- integrate access with RBAC and privileged-access model
- keep Bastion as part of a documented management architecture

### Common mistakes
- Bastion deployed but emergency SSH/RDP still relies on public IPs
- Bastion subnet requirements overlooked
- Bastion treated as if it solved identity, session recording, or privileged workflow by itself

---

## 7.6 Firewall / NAT / Bastion troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Firewall rule exists but flow still fails | route/rule hierarchy | UDR path, collection-group priority, rule type, IP vs FQDN match |
| Workload bypasses firewall | routing | effective route, subnet UDR, peering/service chain design |
| Egress IP unexpected | outbound ownership | NAT vs Firewall design, subnet NAT association, route path |
| Outbound scale issue appears under load | SNAT design | public IP count, connection reuse, port pressure, NAT placement |
| Bastion exists but admins still can’t connect | access path | Bastion subnet/config, target VM reachability, RBAC, NSG expectations |

---

# 8. VPN Gateway and ExpressRoute: Hybrid Path Control

These services are the backbone of hybrid Azure connectivity.

Microsoft documents BGP for VPN Gateway, active-active VPN gateways, ExpressRoute peerings, route filters, resiliency guidance, and validation. [14][15][16][17][18][19]

## 8.1 VPN Gateway

### Technical purpose
Encrypted connectivity over internet for:
- site-to-site
- VNet-to-VNet
- branch scenarios
- transitional or backup hybrid patterns

### Core entities
- virtual network gateway
- local network gateway
- connection
- tunnel
- GatewaySubnet
- BGP settings
- active-active or active-standby mode

### Best practices
- design GatewaySubnet deliberately
- choose active-active early when HA is a real requirement
- use BGP where dynamic route behavior matters
- test site failover under business load, not only tunnel-up state

### Common mistakes
- gateway built, but route expectations are not documented
- static thinking applied to dynamic BGP topology
- HA assumed because two tunnels exist, even if path preference never changes

---

## 8.2 BGP mechanics and asymmetry

Microsoft documents BGP support for VPN Gateway and its configuration. [14][15]

### Important concept
BGP simplifies dynamic route exchange, but it also creates new failure modes:
- asymmetric routing
- unexpected preference changes
- hidden dependency on on-prem routing policy

### Best practices
- document ASNs, peer IPs, and intended advertisements
- coordinate route filtering and preference with on-prem teams
- validate return path during tests
- understand how gateway-mode changes alter BGP details

### Common mistakes
- operators troubleshoot only Azure side and ignore on-prem route policy
- failover tests check reachability only, not application path symmetry
- changing active-active/standby mode without understanding BGP neighbor changes

---

## 8.3 Active-active VPN

Microsoft documents active-active gateway behavior and warns about BGP IP changes and downtime during some transitions. [16][20]

### Best practices
- align on-prem device design with Azure active-active behavior
- test tunnel failover and route convergence
- document operational impact of mode changes
- avoid treating production mode change as a low-risk action

### Common mistakes
- active-active configured, but on-prem routing still pins effectively to one path
- BGP neighbors or expectations not updated after mode change

---

## 8.4 ExpressRoute

### Technical purpose
Dedicated private connectivity into Microsoft network for enterprise hybrid design.

### Core entities
- circuit
- private peering
- Microsoft peering
- route filter
- gateway
- provider edge relationship
- site resiliency model

Microsoft documents that an ExpressRoute circuit has Azure Private and Microsoft peerings configured on a pair of routers for high availability. [17]

### Peering intent in practical terms
- **Private peering**: Azure private-resource connectivity
- **Microsoft peering**: selected Microsoft service prefixes, controlled using route filters

### Best practices
- design circuit, gateway, and site resiliency together
- document what should traverse Private peering versus Microsoft peering
- use route filters intentionally
- validate business-service continuity during failover, not just circuit status

### Common mistakes
- circuit exists, but service teams don’t know which peering their path depends on
- route filter not aligned to service consumption plan
- ExpressRoute treated as a magic “reliable pipe” with no site HA design

---

## 8.5 ExpressRoute with VPN coexistence

### Valid pattern
Use ExpressRoute as primary and VPN as backup or alternate path where architecture requires it.

### Best practices
- document path preference and failover behavior
- test coexistence regularly
- validate asymmetric routing risks
- coordinate Azure and on-prem route preference intentionally

### Common mistakes
- “backup VPN” exists on diagram only
- during incident, return path follows different connectivity than forward path
- teams do not know which path is active without checking routing state

---

## 8.6 Hybrid connectivity troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Tunnel/circuit healthy but app traffic fails | route symmetry | BGP advertisements, return path, on-prem filters, UDR interaction |
| Failover did not occur as expected | HA design | active-active config, on-prem preference, convergence testing, coexistence rules |
| Expected Microsoft services unavailable over ER | peering/filter | Microsoft peering, route filter config, intended prefix selection |
| Hybrid path unexpected | route control | BGP-learned routes, VPN/ER coexistence, route propagation, firewall insertion |
| Change caused outage even though links came back | control-plane change | gateway mode changes, BGP neighbor changes, route reconvergence behavior |

---

# 9. Azure DNS, Private DNS, and DNS Private Resolver

Name resolution is a first-class architecture topic, not a side setting.

Microsoft documents DNS Private Resolver, its endpoints and rulesets, and hybrid DNS-resolution patterns. [21][22][23][24]

## 9.1 Public DNS vs private DNS

### Public Azure DNS
Authoritative public-zone hosting.

### Azure Private DNS
Private-zone hosting for Azure-private resolution patterns.

### Best practices
- keep public and private namespace ownership clear
- document which names are meant to resolve differently inside and outside Azure
- treat DNS design as part of service onboarding, especially for PaaS privatization

### Common mistakes
- public and private teams manage same namespace independently
- private zones created ad hoc across subscriptions
- no documentation for split-horizon expectations

---

## 9.2 Private endpoint DNS patterns

This is one of the most important Week 2 expert topics.

### Important concept
A private endpoint usually works or fails because of DNS, not because of packet reachability alone.

### Best practices
- define private DNS zone strategy per service class
- centralize where justified, but preserve clear ownership
- validate from actual client resolver path
- document cross-subscription and cross-region zone-link expectations

### Common mistakes
- private endpoint IP exists, but clients still resolve public target
- multiple zones for same service namespace cause inconsistent answers
- one region’s endpoint record is mistaken for globally correct design

### Failure pattern examples
- App in spoke A uses central private DNS correctly, but on-prem still resolves public endpoint because forwarders were not updated.
- Private endpoint approved, but VNet link missing for one environment, so only production fails.
- Two teams create different private zones for the same namespace, and resolution becomes client-path dependent.

---

## 9.3 DNS Private Resolver internals

### Core entities
- inbound endpoint
- outbound endpoint
- forwarding ruleset
- forwarding rule
- VNet link
- on-prem forwarder integration

### Best practices
- use inbound endpoints for on-prem recursion into Azure private zones
- use outbound endpoints plus rulesets for Azure-to-on-prem forwarding
- keep rulesets aligned to intended client VNets
- test recursion chain end to end

### Common mistakes
- Private Resolver deployed but no one repoints on-prem DNS forwarders
- outbound rules exist, but client VNet is not linked to the ruleset
- stale DNS cache masks configuration mistakes during change windows

---

## 9.4 Split-horizon and split-brain DNS

### Split-horizon
Intentional internal vs external answers for same logical service name.

### Split-brain failure
Different internal clients unexpectedly resolve different answers because resolver path or zone ownership is inconsistent.

### Best practices
- document authoritative source of truth for each namespace
- avoid duplicate private zones unless deliberately required
- define DNS cutover order during private-endpoint rollout
- reduce hidden recursion paths

### Common mistakes
- “It works from my VM” replaces resolver-path analysis
- private resolution works from Azure but not from branch offices
- hybrid DNS design is tested only from one source network

---

## 9.5 DNS troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Private endpoint reachable by IP but not name | DNS records | private zone record, VNet link, client resolver, cache |
| On-prem resolves public answer for private service | hybrid recursion | inbound endpoint, on-prem forwarder, zone path, split-horizon policy |
| One VNet resolves correctly, another does not | zone linkage | ruleset/zone links, resolver path, subscription-level zone ownership |
| DNS cutover unstable | cache/governance | TTL, stale resolver cache, duplicate zone ownership, missing forwarding path |
| Only one region works | record design | region-specific records, private zone consistency, client-path assumptions |

---

# 10. Load Balancer, Application Gateway, Front Door, Traffic Manager, and CDN

All distribute traffic, but they do so at different layers and with different consequences.

## 10.1 Azure Load Balancer

Microsoft documents health probes, health status, and Load Balancer operation. [25][26][27]

### Technical purpose
Regional L4 distribution for TCP/UDP workloads.

### Core entities
- frontend IP
- backend pool
- health probe
- load-balancing rule
- inbound NAT rule
- backend health status

### Best practices
- probe true readiness, not only process-alive endpoints
- monitor backend health state and reason
- separate inbound balancing decisions from outbound egress design

### Common mistakes
- using LB for HTTP routing expectations
- health probe endpoint always green even when app is degraded
- assuming backend is healthy because VM is up

---

## 10.2 Application Gateway

Microsoft documents listeners, request-routing rules, and WAF policy association levels. [28][29][30]

### Technical purpose
Regional L7 reverse proxy for HTTP(S) applications.

### Core entities
- listener
- backend pool
- backend HTTP settings
- routing rule
- path-based rule
- probe
- WAF policy
- hostname/SNI and TLS handling

### Best practices
- align listener hostnames and certificates to real request patterns
- validate backend probe host/path behavior explicitly
- use per-listener or per-path WAF association where isolation is needed
- document rewrite, host-header, and backend setting assumptions

### Common mistakes
- default listener/rule unintentionally catches traffic
- WAF policy attached at wrong scope
- backend healthy in portal only because probe is too weak
- host-header mismatch causes backend failures

### Failure pattern examples
- App Gateway listener matches `example.com`, but clients send another host, so traffic never reaches intended rule.
- Probe hits `/health` successfully, but app fails real routed paths because backend host-header expectations differ.
- Per-site WAF was intended, but global policy attachment applied stricter rules to all sites.

---

## 10.3 Front Door

Microsoft documents origin groups, health probes, best practices, and logging. [31][32][33][34]

### Technical purpose
Global anycast L7 edge for:
- acceleration
- failover
- centralized TLS termination
- WAF
- origin steering

### Core entities
- profile
- endpoint
- route
- origin
- origin group
- health probe
- WAF policy

### Health-probe nuance
If every origin in an origin group is unhealthy, Front Door considers all origins unhealthy and round-robins across all of them. [32]

### Best practices
- tune probes to actual origin health characteristics
- control origin-group design deliberately
- route logs and WAF logs centrally
- harden backend reachability and trust assumptions

### Common mistakes
- origin security model does not allow Front Door origin traffic properly
- health-probe settings overload or mischaracterize backend
- all-origins-unhealthy behavior is misunderstood during incident

---

## 10.4 Traffic Manager

### Technical purpose
DNS-based traffic steering only.

### Best fit
- simple failover/priority/weighted/performance DNS decisions
- non-HTTP scenarios where proxying is unnecessary
- low-complexity global steering

### Common mistakes
- expecting proxy-like failover speed despite DNS caching
- using Traffic Manager where Front Door’s L7 capabilities are required

---

## 10.5 CDN

### Technical purpose
Caching and edge acceleration for cacheable content.

### Best practices
- define cache-control strategy before rollout
- separate static asset distribution from dynamic personalized content
- document invalidation process

### Common mistakes
- no purge process
- origin headers conflict with intended cache model

---

## 10.6 Edge and load-balancing troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Backend shown down | probe | probe path, protocol, host, TLS, app readiness |
| Front Door behaves strangely during outage | origin-group logic | all-origins-unhealthy behavior, probe config, logs |
| App Gateway receives traffic but app still fails | backend settings | host header, probe host/path, path-based rule, rewrite logic |
| DNS failover too slow | Traffic Manager behavior | TTL, resolver caching, routing method expectations |
| CDN serving wrong or stale asset | cache design | headers, purge status, origin object versioning |

---

# 11. DDoS Protection, WAF, and Network Watcher

Protection and diagnostics must be designed together.

Microsoft documents DDoS monitoring, WAF policy behavior for Application Gateway, and Network Watcher flow logs and tooling. [30][35][36][37]

## 11.1 Azure DDoS Protection

### Technical purpose
Mitigate volumetric attacks and expose mitigation telemetry.

### Best practices
- attach diagnostics to Log Analytics
- define incident playbook for mitigation visibility
- distinguish L3/L4 volumetric events from L7 abuse cases

### Common mistakes
- assuming DDoS Protection covers all web attack categories
- no one reviews mitigation reports until after a major incident

---

## 11.2 WAF operational mechanics

### Technical purpose
Managed and custom HTTP(S) rule enforcement at L7.

### Best practices
- begin with monitoring/tuning where appropriate
- scope exclusions precisely
- keep policy ownership explicit
- correlate WAF events with app logs before concluding attack vs false positive

### Common mistakes
- broad exclusions weaken entire site portfolio
- block mode enabled with no known-good baseline
- app team and platform team disagree on who owns false-positive tuning

---

## 11.3 Network Watcher as first-responder toolkit

Microsoft documents Network Watcher and VNet flow logs. [36][37]

### Key tools
- effective routes
- IP flow verify
- connection monitor
- packet capture
- topology
- VNet flow logs

### Best practices
- start with effective routes and IP flow verify
- use connection monitor for critical-path validation over time
- keep flow-log retention aligned to incident-forensics needs
- standardize common diagnostics steps

### Common mistakes
- changing NSGs or routes before validating actual path state
- no baseline connectivity tests for critical dependencies
- forensics impossible because logs were never enabled

---

## 11.4 Protection and diagnostics troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| App is intermittently unavailable under attack/load | edge/protection | DDoS telemetry, WAF logs, probe behavior, backend saturation |
| Legitimate requests blocked | WAF | matched rule, scope, exclusion design, false-positive evidence |
| “Network issue” still unclear | observability | effective routes, IP flow verify, connection monitor, flow logs |
| Post-incident evidence weak | telemetry design | DDoS logs, WAF logs, retention, central diagnostics pipeline |

---

# 12. Azure API Management: Policy, Lifecycle, and Runtime Control

API Management is an API governance platform, not merely a reverse proxy.

Microsoft documents APIM policies, revisions, and versions. [38][39][40][41]

## 12.1 Technical purpose

Use APIM for:
- API mediation
- security policy enforcement
- throttling and quota
- transformation
- backend abstraction
- productization and consumer access control
- safe API lifecycle change management

## 12.2 Core entities

### APIM instance
Management and gateway boundary.

### API
Published contract surface.

### Product
Grouped API packaging for access and governance.

### Subscription
Consumer access credential/relationship.

### Policy
Declarative pipeline behavior.

### Backend
Target service endpoint.

### Revision
Safe nonbreaking change track.

### Version
Breaking-change or distinct contract line.

### Gateway
Runtime request processor.

### Developer portal
Consumer-facing onboarding and docs surface.

---

## 12.3 Policy scope layering

### Important concept
Policy behavior can be hard to reason about because scope layering matters.

### Best practices
- standardize what belongs at global, product, API, and operation scopes
- keep cross-cutting controls centralized where possible
- isolate backend-specific quirks near the API/operation level
- document policy inheritance and override strategy

### Common mistakes
- same auth or header logic copied everywhere
- one global policy carries hidden assumptions for all APIs
- operation-level exception breaks platform-wide consistency

### Failure pattern examples
- API works directly against backend but fails via APIM because a global policy injects auth expectations the backend rejects.
- One revision works and another does not because a named value or backend policy changed outside developer awareness.
- Product-level subscription requirement conflicts with assumed anonymous or identity-based flow.

---

## 12.4 Revisions vs versions

Microsoft documents revisions for safe nonbreaking API changes and versions for handling breaking changes. [39][40][41]

### Revisions
Use for:
- internal edits
- policy changes
- testing nonbreaking changes
- rollback to prior revision if needed

### Versions
Use for:
- breaking changes
- distinct contract paths
- multi-version lifecycle support

### Best practices
- revisions for rollout safety
- versions for contract differences
- document deprecation and consumer migration path
- align products and subscriptions to version strategy

### Common mistakes
- revisions treated as permanent versions
- breaking changes hidden inside same version
- old versions kept forever with no lifecycle policy

---

## 12.5 Products, subscriptions, and identity nuance

### Product model
A product should represent an access and governance boundary, not only a folder of APIs.

### Subscription model
Subscription keys are one access model, but do not confuse them with broader identity and authorization design.

### Best practices
- define who consumes which APIs and through what entitlement model
- align product boundaries with consumer classes
- rotate and govern subscription credentials deliberately
- keep backend environment mapping explicit

### Common mistakes
- every API put in its own product with no governance meaning
- subscription keys proliferate with no ownership
- APIM auth/policy model and backend auth model drift apart

---

## 12.6 APIM troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Backend works directly but not through APIM | policy/gateway | policy scope, auth injection, rewrite logic, backend config |
| New revision breaks traffic unexpectedly | lifecycle | revision differences, named values, product association, backend target drift |
| Clients confused by API change strategy | release model | versions vs revisions, portal docs, product/subscription mapping |
| Rate-limit or auth failures feel inconsistent | scope layering | global vs API vs operation policy, subscription requirement, consumer identity path |

---

# 13. End-to-End Reference Architecture Example

An enterprise needs:
- segmented VNets
- central inspection
- hybrid connectivity
- private PaaS
- global web delivery
- controlled API exposure
- diagnosable routing and DNS behavior

A strong design might be:

### Core segmentation
- VNet/subnet model with non-overlapping IP plan
- hub-spoke or vWAN based on branch/transit complexity
- AVNM for central connectivity and security admin standardization where scale justifies it

### Hybrid
- ExpressRoute for primary private connectivity
- VPN Gateway for complementary or backup path
- BGP documented and failover-tested

### Private service access
- private endpoints for critical PaaS
- centralized private DNS strategy
- DNS Private Resolver for Azure↔on-prem recursion

### Egress and admin
- Azure Firewall for inspection
- NAT Gateway only where outbound scale/identity design explicitly needs it
- Bastion for privileged admin access without VM public IPs

### Edge
- Application Gateway for regional L7 ingress
- Front Door for global edge and failover
- Traffic Manager only when DNS steering is enough
- CDN for cacheable content acceleration

### API
- APIM for authentication mediation, throttling, policy, revisions, and version governance

### Observability
- effective-route checks in every network change
- Network Watcher baseline diagnostics
- WAF/DDoS/App Gateway/Front Door/APIM logs centralized

This is the Week 2 principle:

> The best Azure network is the one whose route path, DNS path, security path, and exposure path can all be explained clearly before an incident.

---

# 14. Troubleshooting Flow

When Azure networking fails, inspect in this order:

1. **Address truth**  
   Is the destination IP plan valid and non-overlapping?

2. **Route truth**  
   Which route actually won: system, BGP, or UDR?

3. **Filter truth**  
   Did NSG, security admin rule, Firewall, WAF, or APIM policy block the flow?

4. **DNS truth**  
   What answer did the client resolve, and from which resolver chain?

5. **Exposure truth**  
   Which edge/gateway/listener/product actually processed the request?

6. **Health truth**  
   Are backends, probes, tunnels, origins, and gateways healthy?

7. **Evidence truth**  
   What do effective routes, IP flow verify, connection monitor, flow logs, and service logs prove?

---

# 15. Common Week 2 Mistakes

- CIDR planning treated as temporary
- route tables attached inconsistently across subnets
- service endpoints treated as equivalent to private endpoints
- private endpoints deployed with no DNS governance
- peering and service chaining assumed instead of validated
- AVNM and vWAN both used without role clarity
- Firewall and NAT Gateway combined with no explicit outbound ownership
- Bastion deployed while public management still exists
- ExpressRoute bought without true site-resiliency design
- VPN failover assumed but never tested under route convergence
- private DNS zones duplicated across teams
- Front Door, App Gateway, and APIM overlap with unclear responsibility boundaries
- WAF block mode enabled without tuning
- troubleshooting begins from assumptions instead of effective routes and DNS answers

---

# 16. Revision Checklist

You should be able to explain clearly:

- how VNet, subnet, NSG, ASG, UDR, service chaining, service endpoints, private endpoints, Private Link, and peering interact
- how to reason about route precedence and why effective routes matter more than theory alone
- why private endpoint rollouts are often DNS projects more than network projects
- how AVNM differs from vWAN and when each is justified
- how Firewall Policy hierarchy affects rule processing and troubleshooting
- how NAT Gateway changes outbound design without replacing inspection logic
- how VPN Gateway and ExpressRoute differ, and how BGP and route filters change hybrid behavior
- how DNS Private Resolver supports Azure↔on-prem recursive resolution
- how to choose between Load Balancer, Application Gateway, Front Door, Traffic Manager, and CDN
- how DDoS, WAF, and Network Watcher support both protection and diagnosis
- how APIM policy scope, revisions, versions, products, and subscriptions support API runtime governance

---

# 17. Memory Anchors

- **VNet** = private network boundary
- **subnet** = segmentation and placement boundary
- **effective routes** = operational truth of route selection
- **NSG** = stateful L3/L4 filter
- **ASG** = NSG targeting abstraction
- **UDR** = custom route intent
- **service endpoint** = VNet identity to supported public-service endpoint model
- **private endpoint** = private IP path to service
- **Private Link** = private service connectivity framework
- **service chaining** = route steering through central inspection path
- **AVNM** = estate-wide VNet connectivity and security-admin orchestration
- **vWAN** = managed transit and branch hub architecture
- **routing intent** = declarative steering of Private/Internet traffic in vWAN
- **Azure Firewall** = centralized inspection/filtering
- **NAT Gateway** = scalable outbound SNAT
- **Bastion** = private admin access plane
- **VPN Gateway** = encrypted internet-based hybrid path
- **ExpressRoute** = dedicated private hybrid path
- **Private DNS** = private authoritative zone
- **DNS Private Resolver** = managed hybrid recursive/forwarding layer
- **Load Balancer** = regional L4
- **Application Gateway** = regional L7
- **Front Door** = global edge L7
- **Traffic Manager** = DNS steering
- **CDN** = cache acceleration
- **WAF** = L7 filtering
- **Network Watcher** = packet-path diagnostics
- **APIM** = API control plane
- **revision** = safe nonbreaking change track
- **version** = breaking-change contract line

---

# 18. Official References

## Core networking
- Azure Virtual Network peering overview  
  https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview

- Azure virtual network service endpoints overview  
  https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-service-endpoints-overview

- Integrate Azure services with virtual networks for network isolation  
  https://learn.microsoft.com/en-us/azure/virtual-network/vnet-integration-for-azure-services

- What is Azure Private Link?  
  https://learn.microsoft.com/en-us/azure/private-link/private-link-overview

## AVNM and Virtual WAN
- Virtual Network Manager and Virtual WAN  
  https://learn.microsoft.com/en-us/azure/virtual-wan/virtual-network-manager-integration

- About virtual hub routing  
  https://learn.microsoft.com/en-us/azure/virtual-wan/about-virtual-hub-routing

- Configure Virtual WAN hub routing intent and policies  
  https://learn.microsoft.com/en-us/azure/virtual-wan/how-to-routing-policies

- Securing internet access with routing intent  
  https://learn.microsoft.com/en-us/azure/virtual-wan/about-internet-routing

## Firewall, NAT, Bastion
- Azure Firewall policy rule sets  
  https://learn.microsoft.com/en-us/azure/firewall/policy-rule-sets

- Integrate NAT Gateway with Azure Firewall in hub-and-spoke  
  https://learn.microsoft.com/en-us/azure/nat-gateway/tutorial-hub-spoke-nat-firewall

- Integrate Azure Firewall with NAT Gateway V2  
  https://learn.microsoft.com/en-us/azure/firewall/integrate-with-nat-gateway-v2

- Azure Bastion overview  
  https://learn.microsoft.com/en-us/azure/bastion/bastion-overview

- Azure Bastion configuration settings  
  https://learn.microsoft.com/en-us/azure/bastion/configuration-settings

- Azure Bastion documentation  
  https://learn.microsoft.com/en-us/azure/bastion/

## Hybrid connectivity
- VPN Gateway documentation  
  https://learn.microsoft.com/en-us/azure/vpn-gateway/

- About BGP and VPN Gateway  
  https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-bgp-overview

- Configure BGP for Azure VPN Gateway  
  https://learn.microsoft.com/en-us/azure/vpn-gateway/configure-bgp

- About active-active VPN gateways  
  https://learn.microsoft.com/en-us/azure/vpn-gateway/about-active-active-gateways

- Change a gateway to active-active mode  
  https://learn.microsoft.com/en-us/azure/vpn-gateway/gateway-change-active-active

- ExpressRoute documentation  
  https://learn.microsoft.com/en-us/azure/expressroute/

- ExpressRoute circuits and peering  
  https://learn.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings

- Configure route filters for Microsoft peering  
  https://learn.microsoft.com/en-us/azure/expressroute/how-to-routefilter-portal

- Design and architect ExpressRoute for resiliency  
  https://learn.microsoft.com/en-us/azure/expressroute/design-architecture-for-resiliency

- ExpressRoute gateway resiliency validation  
  https://learn.microsoft.com/en-us/azure/expressroute/resiliency-validation

## DNS
- Azure DNS Private Resolver overview  
  https://learn.microsoft.com/en-us/azure/dns/dns-private-resolver-overview

- Azure DNS Private Resolver endpoints and rulesets  
  https://learn.microsoft.com/en-us/azure/dns/private-resolver-endpoints-rulesets

- Resolve Azure and on-premises domains  
  https://learn.microsoft.com/en-us/azure/dns/private-resolver-hybrid-dns

- Azure DNS Private Resolver architecture  
  https://learn.microsoft.com/en-us/azure/architecture/networking/architecture/azure-dns-private-resolver

## Load balancing and edge
- Azure Load Balancer documentation  
  https://learn.microsoft.com/en-us/azure/load-balancer/

- Azure Load Balancer health probes  
  https://learn.microsoft.com/en-us/azure/load-balancer/load-balancer-custom-probe-overview

- Manage Azure Load Balancer health status  
  https://learn.microsoft.com/en-us/azure/load-balancer/load-balancer-manage-health-status

- Azure Application Gateway listener configuration  
  https://learn.microsoft.com/en-us/azure/application-gateway/configuration-listeners

- Application Gateway request routing rules configuration  
  https://learn.microsoft.com/en-us/azure/application-gateway/configuration-request-routing-rules

- WAF policy overview for Application Gateway  
  https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/policy-overview

- Create WAF policies for Application Gateway  
  https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/create-waf-policy-ag

- Azure Front Door health probes  
  https://learn.microsoft.com/en-us/azure/frontdoor/health-probes

- Azure Front Door origins and origin groups  
  https://learn.microsoft.com/en-us/azure/frontdoor/origin

- Azure Front Door best practices  
  https://learn.microsoft.com/en-us/azure/frontdoor/best-practices

- Configure Azure Front Door logs  
  https://learn.microsoft.com/en-us/azure/frontdoor/standard-premium/how-to-logs

## Protection and diagnostics
- Network Watcher documentation  
  https://learn.microsoft.com/en-us/azure/network-watcher/

- Virtual network flow logs overview  
  https://learn.microsoft.com/en-us/azure/network-watcher/vnet-flow-logs-overview

- Manage virtual network flow logs  
  https://learn.microsoft.com/en-us/azure/network-watcher/vnet-flow-logs-manage

- Monitor Azure DDoS Protection  
  https://learn.microsoft.com/en-us/azure/ddos-protection/monitor-ddos-protection

- View Azure DDoS Protection logs  
  https://learn.microsoft.com/en-us/azure/ddos-protection/ddos-view-diagnostic-logs

## API Management
- Policies in Azure API Management  
  https://learn.microsoft.com/en-us/azure/api-management/api-management-howto-policies

- Revisions in Azure API Management  
  https://learn.microsoft.com/en-us/azure/api-management/api-management-revisions

- Versions in Azure API Management  
  https://learn.microsoft.com/en-us/azure/api-management/api-management-versions

- Use revisions for safe API changes  
  https://learn.microsoft.com/en-us/azure/api-management/api-management-get-started-revise-api

---

# 19. Final Summary

Week 2 is about **making Azure packet paths predictable, private service access reliable, hybrid routing understandable, and edge/API exposure governable**.

The strongest mental model is:

- **VNet + subnets + NSGs + routes** = local packet-path foundation
- **Private Link + Private DNS + Private Resolver** = private-service connectivity and resolution
- **AVNM + vWAN** = central network governance and transit design
- **Firewall + NAT + Bastion** = inspection, egress identity, and admin access
- **VPN Gateway + ExpressRoute** = hybrid path control
- **Load Balancer + App Gateway + Front Door + Traffic Manager + CDN** = traffic exposure and distribution
- **DDoS + WAF + Network Watcher** = protection and diagnostics
- **APIM** = API runtime and lifecycle control

When you can explain the **address model, route model, filter model, DNS model, exposure model, and hybrid model** together, Azure Week 2 is understood architecturally.
