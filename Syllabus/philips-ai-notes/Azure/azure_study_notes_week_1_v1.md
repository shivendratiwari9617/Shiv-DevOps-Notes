# Azure Study Notes — Week 1
## Landing Zones, Identity, RBAC, Governance, Platform Hygiene, and Cost Control

> This is the **deep Week 1 Azure guide**, written much closer to a **9.5+ bar** than a normal summary.  
> It is structured as **expert-level technical study material** with:
> - technical purpose
> - core entities
> - control-plane mechanics
> - architecture fit
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** tight structure, high signal, minimal filler.

---

# 1. Why Week 1 Matters

Week 1 is about the **control plane of Azure**.

Before compute, networking, or data platforms, Azure forces a more basic question:

> **Who owns the tenant, how is governance inherited, how is access granted, how is identity trusted, and how are subscriptions structured so that later services remain operable?**

If this layer is weak:
- teams deploy into the wrong subscriptions
- platform policy becomes inconsistent
- privileged access accumulates
- billing ownership becomes unclear
- break-glass response fails during incidents
- remediation work grows faster than the platform

Week 1 is therefore not “intro Azure.”  
It is the foundation for:
- landing zones
- subscription vending
- identity governance
- access control
- policy enforcement
- inventory and platform hygiene
- cost accountability

The core idea is:

> **Azure governance is hierarchical, inherited, identity-driven, and scope-sensitive. Most later architecture mistakes are really Week 1 mistakes discovered late.**

---

# 2. Learning Outcomes

By the end of Week 1, you should be able to:

- explain **Azure tenant**, **tenant root management group**, **management groups**, **subscriptions**, **resource groups**, and **resources** as control scopes
- explain **Azure landing zones**, platform vs application subscriptions, and why management-group hierarchy is the backbone of scalable governance
- explain **Microsoft Entra ID** tenant objects: users, groups, app registrations, enterprise applications, service principals, and managed identities
- explain **Microsoft Entra Privileged Identity Management (PIM)**, eligible vs active assignments, activation settings, approval flows, and access reviews
- explain **Conditional Access**, break-glass design, privileged role hygiene, and workload identity governance
- explain **Azure RBAC** role definitions, role assignments, scopes, built-in roles, custom roles, deny assignments, and management locks
- explain **Azure Policy** definitions, initiatives, assignments, exemptions, effects, compliance states, remediation tasks, and inheritance
- explain **Azure Resource Graph**, **deployment stacks**, **tags**, **naming standards**, and **platform hygiene controls**
- explain **Azure Lighthouse** delegated resource management
- explain **Microsoft Cost Management** billing scopes, billing profiles, invoice sections, budgets, exports, cost allocation, and reservation-awareness in governance planning
- design and troubleshoot a baseline landing-zone governance model before workloads are onboarded

---

# 3. Azure Control-Plane Mental Model

Think in planes:

1. **Hierarchy plane**  
   Tenant → management groups → subscriptions → resource groups → resources

2. **Identity plane**  
   Microsoft Entra users, groups, workloads, managed identities, Conditional Access, PIM

3. **Authorization plane**  
   Azure RBAC, custom roles, deny assignments, locks

4. **Governance plane**  
   Azure Policy, initiatives, exemptions, remediation, naming/tagging standards

5. **Platform operations plane**  
   Resource Graph, deployment stacks, landing-zone accelerators, Lighthouse, Cost Management

The Week 1 principle is:

> **Azure governance must be designed top-down, but operated bottom-up with clear ownership at each scope.**

---

# 4. Scope Hierarchy: Tenant, Management Groups, Subscriptions, Resource Groups

This is the most important Azure structure to internalize.

## 4.1 Tenant

### Technical purpose
The Microsoft Entra tenant is the top identity boundary:
- directory objects live here
- Conditional Access is evaluated here
- PIM role governance lives here
- many cross-subscription governance patterns begin here

### Best practices
- treat the tenant as a high-trust control plane
- separate tenant administration from ordinary workload administration
- document emergency access paths

### Common mistakes
- tenant-wide roles assigned casually
- no clear ownership model for Entra and Azure governance boundary

---

## 4.2 Tenant root management group

Azure management groups provide hierarchical scope above subscriptions. Azure documents that management groups support hierarchy and inheritance of RBAC and policy assignments. [1]

### Technical purpose
This is the top Azure governance scope for management-group hierarchy.

### Why it matters
If you assign policy or RBAC too low:
- governance becomes inconsistent
- platform baselines drift
- every new subscription becomes manual work

### Best practices
- keep tenant root assignments limited and intentional
- use it for universal guardrails, not noisy local exceptions
- avoid broad operational-role assignment here unless truly required

### Common mistakes
- all policies placed at tenant root with no hierarchy strategy
- overbroad RBAC at root creates unnecessary blast radius

---

## 4.3 Management groups

Azure documents management groups as a way to organize subscriptions and inherit governance conditions downward. [1]

### Technical purpose
Management groups let you:
- group subscriptions by platform, business unit, environment, geography, or control model
- inherit policy and RBAC downward
- design a scalable landing-zone hierarchy

### Typical hierarchy pattern
A common enterprise model:
- tenant root
  - platform
  - landing zones
  - sandbox
  - decommissioned
  - optionally business-unit or regulatory branches

### Best practices
- design hierarchy for governance inheritance, not org-chart aesthetics
- keep the tree understandable
- separate platform subscriptions from application subscriptions
- use management groups to express operating model differences

### Common mistakes
- management-group tree mirrors HR org chart but not governance reality
- too much nesting with no policy purpose
- platform and application subscriptions mixed together

---

## 4.4 Subscriptions

Subscriptions are both:
- deployment boundaries
- billing/accountability boundaries
- quota boundaries
- governance-assignment scopes

### Technical purpose
Use subscriptions to isolate:
- environments
- business units
- security boundaries
- lifecycle patterns
- cost ownership

### Best practices
- design subscription vending standards
- assign subscription owner/accountability clearly
- treat subscription as a serious operating boundary, not just a folder
- use separate subscriptions for platform shared services where justified

### Common mistakes
- one giant subscription for everything
- production and dev mixed without governance difference
- subscription naming inconsistent with ownership

---

## 4.5 Resource groups

Azure documents resource groups as containers for related resources of a solution. [2]

### Technical purpose
Resource groups are not a replacement for subscriptions.  
They are deployment, lifecycle, and RBAC scoping containers below subscription level.

### Best practices
- group by lifecycle and operational ownership
- keep resource groups purposeful
- avoid mixing unrelated systems in one resource group
- use RGs to express deployment units and delegated ownership

### Common mistakes
- treating RGs as arbitrary folders
- one RG per subscription with no operational separation
- app and platform resources mixed with different lifecycles

---

## 4.6 Scope hierarchy troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| New subscription missing baseline controls | management-group placement | parent management group, inherited policy/RBAC, vending workflow |
| Team has access in one subscription but not sibling subscription | scope design | assignment scope, inheritance path, group membership |
| Policy behavior inconsistent across environments | hierarchy design | management-group structure, exclusions, duplicate assignments |
| Billing ownership unclear | subscription model | subscription purpose, owner mapping, billing scope linkage |

---

# 5. Azure Landing Zones

Azure landing zones are Microsoft’s scalable target architecture and design pattern for governed Azure adoption. Microsoft documents landing zone design principles and design areas through the Cloud Adoption Framework. [3][4]

## 5.1 Technical purpose

Use landing zones to establish:
- platform subscriptions
- shared services boundaries
- identity and governance baselines
- scalable workload onboarding model
- standardized inheritance and operational controls

## 5.2 Core concepts

### Platform subscriptions
Typically hold shared management, connectivity, identity-adjacent, or security services.

### Application subscriptions
Hold workload resources under platform guardrails.

### Archetypes and accelerators
Reference implementations and deployment approaches for standardizing baseline environments.

### Design areas
Landing zones are not just “create subscriptions.”  
They span:
- identity
- management
- resource organization
- network topology
- security posture
- governance
- business continuity
- platform automation

## 5.3 Landing-zone operating model

### Best practices
- separate platform engineering from workload teams
- define subscription vending guardrails before mass onboarding
- standardize management-group placement rules
- attach policy, budgets, RBAC baselines, and monitoring expectations during vending

### Common mistakes
- landing zone treated as one-time deployment, not operating model
- subscriptions created manually outside hierarchy
- platform shared services mixed into first application subscription

## 5.4 Landing-zone readiness checklist
Before onboarding workloads, validate:
- hierarchy placement
- RBAC baseline
- policy baseline
- naming/tagging enforcement
- budget ownership
- break-glass readiness
- diagnostic and inventory visibility
- quota and regional assumptions

---

# 6. Microsoft Entra ID Foundations

Microsoft Entra is the identity control plane behind Azure access and workload identity. Microsoft documents app registrations, service principals, Conditional Access, access reviews, and managed identities. [5][6][7][8][9]

## 6.1 Core entities

### User
Human identity in the directory.

### Group
Reusable authorization unit for access assignment and governance.

### App registration
Global application definition in the tenant.

### Service principal
Tenant-local security principal representing an application instance.

### Enterprise application
Operational/admin view of a service principal and app access in the tenant.

### Managed identity
Platform-managed workload identity tied to Azure resources.

## 6.2 App registrations vs enterprise applications vs service principals

Microsoft documents the relationship between application objects and service principals. [5]

### Important distinction
- **App registration / application object** = application definition
- **Service principal / enterprise app** = identity instance used in a tenant

### Why it matters
Many operational mistakes come from confusing:
- app registration ownership
- service principal permissions
- workload runtime identity

### Best practices
- keep app-registration ownership explicit
- use groups for assignment where appropriate
- avoid secret-heavy service principals when managed identities are available

### Common mistakes
- permissions assigned to the wrong object
- nobody knows who owns the enterprise app
- service principal secrets persist forever

---

## 6.3 Managed identities

Microsoft documents two managed identity types: system-assigned and user-assigned. [8]

### System-assigned
Lifecycle tied to the resource.

### User-assigned
Independent identity reusable across resources.

### Best practices
- prefer managed identities over client secrets for Azure-hosted workloads
- use user-assigned identities where lifecycle independence matters
- assign only the required RBAC permissions
- document where each managed identity is used

### Common mistakes
- service principals with long-lived secrets used where managed identity fits
- broad Contributor access granted to workload identities
- no inventory of reused user-assigned managed identities

---

## 6.4 Conditional Access

Microsoft documents Conditional Access as the Zero Trust policy engine for access decisions. [6][7]

### Technical purpose
Use signals such as:
- user
- workload identity
- application
- device state
- location
- risk
to enforce access conditions.

### Best practices
- build CA in layers
- test with report-only or pilot patterns where appropriate
- protect privileged access first
- define break-glass exclusions carefully
- keep policy naming and ownership standardized

### Common mistakes
- broad CA policy with no staged rollout
- break-glass accounts not excluded correctly
- emergency accounts protected by the same failing dependency they are meant to bypass

---

## 6.5 Access reviews

Microsoft documents access reviews for group, app, and role-assignment governance. [9][10]

### Technical purpose
Continuously validate whether users should retain access.

### Best practices
- review privileged groups and app access regularly
- assign business or technical reviewers deliberately
- use reviews to remove stale access, not just generate audit evidence

### Common mistakes
- reviews created but outcomes not enforced
- low-context reviewers approve everything
- privileged groups never reviewed because “admins know what they need”

---

## 6.6 Entra troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| App can’t access Azure resource | identity/authorization | managed identity vs service principal, token audience, RBAC assignment scope |
| User blocked unexpectedly | Conditional Access | included users/groups, conditions, report-only history, break-glass exclusions |
| Enterprise app exists but auth model unclear | object model | app registration owner, service principal permissions, credential type |
| Access review completed but access remains | governance process | review action settings, group/app assignment path, reviewer scope |

---

# 7. Microsoft Entra Privileged Identity Management (PIM)

Microsoft documents PIM role settings, eligible role activation, MFA/approval requirements, and assignment duration controls. [11][12]

## 7.1 Technical purpose

PIM reduces standing privilege by enabling:
- eligible assignments
- just-in-time activation
- approval workflows
- MFA enforcement
- assignment duration control
- privileged role auditing

## 7.2 Core entities

### Eligible assignment
User can activate the role when needed.

### Active assignment
Role is already active.

### Activation
Time-bound elevation event.

### Role settings
Rules for approval, MFA, maximum activation duration, and notifications.

## 7.3 Eligible vs active

### Important distinction
- **eligible** = least privilege posture
- **active** = always-on privilege

### Best practices
- prefer eligible for privileged roles
- keep activation durations short
- require justification and approval where risk warrants
- review emergency permanent assignments regularly

### Common mistakes
- privileged roles left permanently active
- MFA not required for elevation
- too many approvers or unclear approval chain breaks activation flow during incidents

## 7.4 Break-glass accounts

### Technical purpose
Emergency access path when normal auth controls fail.

### Best practices
- keep very few
- store and monitor securely
- exclude from ordinary CA policies carefully
- test access periodically
- log and alert on use

### Common mistakes
- break-glass account exists only on paper
- credentials inaccessible during emergency
- account not excluded from failing CA pattern

## 7.5 PIM troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| User is eligible but can’t activate role | PIM settings | MFA requirement, approval requirement, activation duration, scope |
| Role activation works but permissions still fail | RBAC/propagation | assignment scope, inherited permissions, refresh timing |
| Emergency admin path fails | break-glass design | exclusion rules, credential accessibility, tenant restrictions |
| Too many privileged roles remain active | governance posture | eligible vs active policy, review cadence, role-setting design |

---

# 8. Azure RBAC

Azure RBAC is the authorization engine for Azure resources. Microsoft documents role definitions, custom roles, and deny assignments. [13][14][15]

## 8.1 Technical purpose

Use Azure RBAC to answer:
- who can do what
- at which scope
- using which role definition

## 8.2 Core entities

### Scope
Management group, subscription, resource group, or resource.

### Role definition
Set of allowed actions and data actions.

### Role assignment
Binding of principal + role definition + scope.

### Built-in role
Microsoft-provided role definition.

### Custom role
Organization-defined role definition.

### Deny assignment
Explicit deny of actions even if a role assignment would otherwise allow them.

## 8.3 Scope mechanics

### Important concept
Assignments inherit downward.  
A subscription-level role assignment affects all resource groups and resources below it unless other controls intervene.

### Best practices
- assign at the narrowest scope that still supports operations
- use groups, not direct user assignments, where possible
- separate platform admin from workload operator roles
- review inherited privilege carefully

### Common mistakes
- Contributor assigned at subscription because it is easy
- direct user assignments everywhere
- scope too high for convenience, causing excessive blast radius

---

## 8.4 Built-in roles vs custom roles

### Built-in roles
Use first where appropriate.

### Custom roles
Use when built-ins are too broad or too narrow.

Microsoft documents custom roles as fine-grained access controls you create yourself. [14]

### Best practices
- prefer built-ins when they clearly fit
- create custom roles only with a repeatable need
- keep custom roles documented and version-controlled
- test custom-role actions against real workflows

### Common mistakes
- many slightly different custom roles with no governance
- custom role created to patch poor scope design
- role missing necessary actions, leading to operator workarounds

---

## 8.5 Deny assignments

Microsoft documents deny assignments as a way to block actions even when role assignments grant access. [15]

### Technical purpose
A deny assignment is a guardrail, not ordinary access delegation.

### Best practices
- understand deny behavior before troubleshooting RBAC failures
- document all user-assigned deny logic if used
- coordinate deny controls with deployment stacks and platform controls

### Common mistakes
- operator sees “Owner” or “Contributor” and assumes access must work
- deny behavior forgotten in troubleshooting

## 8.6 RBAC troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| User has role but action still denied | deny/role scope | deny assignment, exact scope, management-group inheritance, data-plane vs control-plane action |
| Operator can manage RG but not one resource | scope/resource nuance | child resource scope, provider-specific permissions, lock or deny behavior |
| Custom role works for one team but not another | assignment path | principal group membership, assignment scope, missing actions |
| Workload identity overprivileged | role hygiene | inherited assignments, reused identities, broad built-in role use |

---

# 9. Azure Policy

Azure Policy is the governance enforcement engine. Microsoft documents policy definitions, assignments, initiatives, exemptions, compliance states, and remediation tasks. [16][17][18][19][20]

## 9.1 Technical purpose

Use Azure Policy to:
- evaluate resources against rules
- enforce allowed configurations
- deploy or modify supporting settings
- report compliance state
- remediate existing noncompliant resources in supported cases

## 9.2 Core entities

### Policy definition
Single rule with parameters and effect.

### Initiative
Group of policy definitions toward one governance objective.

### Assignment
Application of a definition or initiative to a scope.

### Exemption
Waiver from evaluation or enforcement for justified cases.

### Parameter
Reusable value passed into definitions or initiatives.

### Effect
Action such as audit, deny, modify, or deployIfNotExists.

### Compliance state
Evaluation result for resource applicability and adherence.

### Remediation task
Follow-up operation to bring applicable resources into compliance using assignment identity where required.

## 9.3 Definitions, initiatives, assignments

### Important distinction
- **definition** = one rule
- **initiative** = policy bundle
- **assignment** = where the rule/bundle actually applies

### Best practices
- assign initiatives for platform baselines
- keep assignment naming meaningful
- use parameters instead of many duplicate definitions
- place policy at the highest appropriate scope

### Common mistakes
- duplicate policy definitions for small variations
- initiatives used with no owner or business meaning
- policy assigned too low, causing drift across subscriptions

---

## 9.4 Effects

### Audit
Report but don’t block.

### Deny
Block creation/update when rule violated.

### Modify
Adjust properties during creation/update in supported scenarios.

### deployIfNotExists
Deploy supporting resources/config where required.

### Best practices
- start with audit or pilot patterns where needed
- move to deny only after exception and onboarding flow are mature
- use modify and deployIfNotExists with clear remediation identity design

### Common mistakes
- deny effect deployed with no exception process
- modify effect used without understanding side effects
- deployIfNotExists assigned without tested managed identity/remediation path

---

## 9.5 Exemptions and remediation

Microsoft documents exemption structure and remediation tasks. [18][20]

### Best practices
- exemptions should be explicit, documented, time-bound when possible
- remediation tasks should be monitored, not assumed successful
- treat exemptions as governance records, not silent bypasses

### Common mistakes
- using exclusions or low-scope assignment placement instead of proper exemptions
- remediation tasks created but not reviewed
- temporary waivers become permanent drift

---

## 9.6 Compliance states and inheritance

Microsoft documents compliance evaluation and applicability behavior. [19]

### Important concept
Noncompliance is not always “bad resource.”  
Sometimes it is:
- not applicable
- exempt
- awaiting remediation
- assignment identity misconfigured
- parent-scope inheritance misunderstood

## 9.7 Azure Policy troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Resource creation denied unexpectedly | policy assignment/effect | inherited assignment, deny effect, parameters, policy alias match |
| Initiative compliance confusing | assignment model | child definitions, applicability, exemptions, evaluation timing |
| Remediation task fails | identity/effect | assignment managed identity, target permissions, supported effect behavior |
| Teams bypass policy by using different scope | governance design | assignment placement, subscription vending, hierarchy gaps |

---

# 10. Resource Graph, Deployment Stacks, Locks, Tags, and Naming

These services and practices make the platform operable at scale.

---

## 10.1 Azure Resource Graph (ARG)

Microsoft documents ARG as cross-subscription and cross-tenant-scale querying over Azure resources using KQL-like querying. [21]

### Technical purpose
Use ARG for:
- inventory
- governance evidence
- ownership validation
- policy drift investigation
- estate-wide reporting

### Best practices
- standardize common governance queries
- use ARG in operational reviews, not just ad hoc debugging
- tie tag/naming/owner evidence to ARG queries

### Common mistakes
- manual portal inspection across dozens of subscriptions
- no common query library for platform teams

---

## 10.2 Deployment stacks

Microsoft documents deployment stacks as resources that manage a set of Azure resources as a cohesive unit, with behavior for unmanaged resources and RBAC-based access control. [22][23]

### Technical purpose
Use deployment stacks to manage lifecycle of a deployed set of resources and control how removed template resources are handled.

### Important concept
A deployment stack is not just “another deployment.”  
It adds lifecycle management for the stack-managed resources.

### Best practices
- use where template-managed ownership should remain explicit
- define action-on-unmanage behavior carefully
- control who can modify stacks
- document interaction with platform guardrails and deny behavior

### Common mistakes
- team removes resource from template and is surprised by detach/delete behavior
- stack ownership unclear
- deployment stack used without lifecycle discipline

---

## 10.3 Management locks

Microsoft documents two common lock levels: **CanNotDelete** and **ReadOnly**. [24]

### Technical purpose
Protect critical resources from accidental change or deletion.

### CanNotDelete
Prevents deletion.

### ReadOnly
Prevents modification and deletion.

### Best practices
- use locks on critical production resources deliberately
- combine with RBAC, don’t replace RBAC
- document lock ownership and removal process
- understand operational impact before using ReadOnly

### Common mistakes
- ReadOnly lock breaks expected operations such as config changes
- lock applied with no break-glass removal process
- locks used to compensate for poor RBAC design everywhere

---

## 10.4 Tags and naming standards

Microsoft documents Azure naming rules, recommended naming/tagging patterns, and tag governance with Azure Policy. [25][26][27][28]

### Technical purpose
Tags and naming conventions support:
- inventory
- automation
- ownership
- cost reporting
- governance queries
- operational clarity

### Best practices
- define required tags: owner, environment, application, cost center, data classification where relevant
- define naming convention before automation scales out
- validate tags via Azure Policy
- keep tag vocabulary controlled

### Common mistakes
- free-form tag values
- too many optional tags with no usage
- names encode inconsistent fields across teams
- relying on tags for critical automation without governance

## 10.5 Platform hygiene troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Inventory report inconsistent | ARG/tagging | tag coverage, query scope, naming convention drift |
| IaC cleanup behaves unexpectedly | deployment stack | stack ownership, actionOnUnmanage behavior, deny settings |
| Operators can’t update critical resource | lock/deny | ReadOnly lock, CanNotDelete lock, deny assignment, stack settings |
| Cost reports messy | naming/tagging | required tags missing, inconsistent values, shared subscription model |

---

# 11. Azure Lighthouse

Microsoft documents Azure Lighthouse as delegated resource management across customer tenants, enabling authorized users, groups, and service principals to manage delegated scopes without accounts in each customer tenant. [29][30]

## 11.1 Technical purpose

Use Azure Lighthouse for:
- multi-tenant delegated administration
- MSP or centralized platform-team operations
- governed cross-tenant management

## 11.2 Core entities

### Delegated resource management
Mechanism enabling cross-tenant administration.

### Offer / onboarding model
How customer scopes are delegated.

### Authorization
Which roles are granted to which managing-tenant principals.

### Eligible authorization
PIM-style delegation option in supported scenarios.

## 11.3 Best practices
- keep delegated scopes minimal
- standardize onboarding packages
- align Lighthouse roles with least privilege
- document customer-tenant access expectations clearly

## 11.4 Common mistakes
- broad delegation because onboarding was rushed
- central team can access too much with poor audit context
- Lighthouse assumed to replace customer governance responsibilities

## 11.5 Lighthouse troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Central team can’t manage delegated subscription | onboarding/authorization | delegated scope, Lighthouse authorization, principal type |
| Access works in one customer tenant but not another | tenant onboarding | offer/authorization differences, scope mismatch |
| Privileged access not governed consistently | delegated governance | PIM/eligible authorization model, role granularity, customer expectations |

---

# 12. Microsoft Cost Management and Billing Scopes

Microsoft documents Cost Management, billing scopes, billing profiles and invoice sections for MCA accounts, exports, budgets, and cost allocation rules. [31][32][33][34][35][36]

## 12.1 Technical purpose

Use Cost Management to:
- analyze and optimize spend
- assign cost ownership
- automate export of cost data
- create budgets and alerts
- allocate shared costs
- understand billing hierarchy and invoice structure

## 12.2 Core entities

### Billing account
Top billing relationship.

### Billing profile
Invoice-producing billing scope in MCA patterns.

### Invoice section
Cost organization unit within a billing profile.

### Subscription
Azure consumption scope often mapped to owners or environments.

### Budget
Threshold alerting control for cost.

### Export
Recurring cost/usage data export.

### Cost allocation rule
Mechanism to split shared costs.

### Reservation
Prepaid commitment construct that affects cost governance and utilization review.

## 12.3 Billing scope mechanics

Microsoft documents that MCA billing accounts can contain billing profiles and invoice sections. [33][34]

### Why it matters
Engineering governance and finance governance intersect here.

### Best practices
- map billing structure to accountability model
- separate invoice and operational ownership concepts clearly
- define subscription-to-cost-owner mapping
- use invoice sections and exports intentionally, not accidentally

### Common mistakes
- billing structure designed after subscriptions already sprawl
- finance and platform teams use different naming/owner vocabulary
- shared platform costs not allocated transparently

---

## 12.4 Budgets, exports, and cost allocation

### Budgets
Use for proactive threshold alerts.

### Exports
Use for recurring analysis and FinOps workflows.

### Cost allocation
Use to distribute shared-platform or central-subscription costs.

Microsoft documents exports and cost allocation rules for these purposes. [32][35][36]

### Best practices
- attach budgets during subscription vending
- send alerts to engineering and finance owners
- automate exports early
- decide whether showback or chargeback is the model
- treat tags as cost data quality inputs

### Common mistakes
- budgets configured but nobody operationally reviews them
- exports enabled but no consumer exists
- shared-network or security costs left unallocated, distorting app economics

## 12.5 Cost governance troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Subscription spend appears ownerless | subscription governance | owner tag, budget recipients, billing mapping, vending metadata |
| Budget alert fires but team can’t act | operating model | alert recipients, subscription owner, RG ownership, spend visibility |
| Shared platform costs distort workload reports | allocation model | cost allocation rules, tag quality, billing profile structure |
| Export data not useful | data hygiene | export scope, tag coverage, naming consistency, billing hierarchy |

---

# 13. End-to-End Landing-Zone Baseline Example

A mature Week 1 Azure model might look like this:

### Hierarchy
- tenant root
  - platform
  - landing-zones
  - sandbox
  - decommissioned

### Platform subscriptions
- identity / privileged admin support
- management / governance / logging
- connectivity / shared network
- security shared services

### Workload subscriptions
- per environment or per application domain depending on risk and ownership

### Entra controls
- PIM for privileged roles
- Conditional Access for admins and workload access patterns
- break-glass accounts tested and monitored
- access reviews for privileged groups and enterprise apps

### RBAC
- group-based assignments
- narrow scope by default
- custom roles only where justified

### Policy
- baseline initiative at management-group scope
- tag/naming enforcement
- allowed locations/SKUs
- diagnostic and security baseline patterns
- exemptions documented with owners and expiry where possible

### Hygiene
- ARG query pack for inventory
- locks on critical platform assets
- deployment stacks for tightly managed IaC-owned resources

### Cost
- subscription vending includes budget, owner, tags, and export expectations

This is the Week 1 principle:

> Azure governance is not a set of isolated features. It is a coordinated operating system for identity, scope, policy, access, hygiene, and cost ownership.

---

# 14. Troubleshooting Flow

When Azure foundational governance behaves unexpectedly, inspect in this order:

1. **Scope truth**
   - what exact scope is involved?
   - tenant, management group, subscription, RG, or resource?

2. **Identity truth**
   - which principal is acting?
   - user, group, service principal, or managed identity?

3. **Authorization truth**
   - which RBAC assignment exists?
   - is there inheritance, deny, or lock interference?

4. **Governance truth**
   - is Azure Policy evaluating or blocking?
   - is there an exemption or remediation issue?

5. **Platform-hygiene truth**
   - do naming, tags, deployment stacks, or management-group placement explain the behavior?

6. **Commercial truth**
   - is billing/accountability structure distorting ownership or scope decisions?

---

# 15. Common Week 1 Mistakes

- no management-group strategy before subscription growth
- subscriptions used as folders instead of control boundaries
- resource groups used to compensate for poor subscription design
- Entra app objects and service principals confused
- managed identities ignored in favor of static secrets
- break-glass accounts untested
- permanent privileged access instead of eligible PIM assignments
- Contributor assigned too broadly
- custom roles created without lifecycle governance
- Azure Policy rolled out with deny but no exemption/remediation path
- tags and names defined informally instead of governed
- locks used without operational removal process
- billing ownership designed after platform sprawl

---

# 16. Revision Checklist

You should be able to explain clearly:

- how tenant, management groups, subscriptions, resource groups, and resources differ
- why landing zones are governance architecture, not just starter templates
- the relationship between app registrations, enterprise applications, service principals, and managed identities
- how Conditional Access and PIM support privileged access governance
- how eligible and active assignments differ
- how Azure RBAC scopes, role definitions, assignments, custom roles, and deny assignments interact
- how Azure Policy definitions, initiatives, assignments, exemptions, effects, compliance, and remediation work
- why Resource Graph, deployment stacks, locks, tags, and naming are operational controls
- how Azure Lighthouse supports delegated multi-tenant management
- how billing accounts, billing profiles, invoice sections, budgets, exports, cost allocation, and reservations influence governance

---

# 17. Memory Anchors

- **tenant** = identity boundary
- **tenant root management group** = top Azure governance scope
- **management group** = governance inheritance layer
- **subscription** = control + billing + quota boundary
- **resource group** = deployment/lifecycle container
- **landing zone** = scalable governed Azure foundation
- **app registration** = application definition
- **service principal** = tenant-local application identity
- **enterprise application** = operational view of service principal
- **managed identity** = Azure-managed workload identity
- **Conditional Access** = access policy engine
- **PIM** = just-in-time privilege control
- **eligible assignment** = can activate
- **active assignment** = currently privileged
- **RBAC** = authorization engine
- **custom role** = tailored permission set
- **deny assignment** = explicit block
- **Azure Policy** = governance enforcement engine
- **initiative** = policy bundle
- **exemption** = documented waiver
- **remediation task** = compliance repair action
- **ARG** = cross-scope inventory query engine
- **deployment stack** = lifecycle-managed IaC resource set
- **lock** = accidental change/deletion guard
- **tag** = metadata for governance and cost
- **Lighthouse** = delegated multi-tenant administration
- **billing profile / invoice section** = billing organization layers
- **budget** = spend threshold alert
- **cost allocation** = shared-cost distribution

---

# 18. Official References

## Hierarchy and landing zones
- What are Azure management groups?  
  https://learn.microsoft.com/en-us/azure/governance/management-groups/overview

- Use the Azure portal and Azure Resource Manager to manage resource groups  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal

- Azure landing zone design areas  
  https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/design-areas

- Azure landing zone design principles  
  https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/design-principles

## Identity and privileged access
- Apps and service principals in Microsoft Entra ID  
  https://learn.microsoft.com/en-us/entra/identity-platform/app-objects-and-service-principals

- Managed identities for Azure resources overview  
  https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/overview

- Microsoft Entra Conditional Access overview  
  https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview

- Build Conditional Access policies  
  https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-policies

- Access reviews overview  
  https://learn.microsoft.com/en-us/entra/id-governance/access-reviews-overview

- Create an access review  
  https://learn.microsoft.com/en-us/entra/id-governance/create-access-review

- Configure Microsoft Entra role settings in PIM  
  https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-how-to-change-default-settings

- Activate a Microsoft Entra role in PIM  
  https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-how-to-activate-role

## Authorization and governance
- Understand Azure role definitions  
  https://learn.microsoft.com/en-us/azure/role-based-access-control/role-definitions

- Azure custom roles  
  https://learn.microsoft.com/en-us/azure/role-based-access-control/custom-roles

- List Azure deny assignments  
  https://learn.microsoft.com/en-us/azure/role-based-access-control/deny-assignments

- Overview of Azure Policy  
  https://learn.microsoft.com/en-us/azure/governance/policy/overview

- Build policies to enforce compliance  
  https://learn.microsoft.com/en-us/azure/governance/policy/tutorials/create-and-manage

- Details of the policy exemption structure  
  https://learn.microsoft.com/en-us/azure/governance/policy/concepts/exemption-structure

- Azure Policy compliance states  
  https://learn.microsoft.com/en-us/azure/governance/policy/concepts/compliance-states

- Details of the policy remediation task structure  
  https://learn.microsoft.com/en-us/azure/governance/policy/concepts/remediation-structure

## Platform hygiene
- Overview of Azure Resource Graph  
  https://learn.microsoft.com/en-us/azure/governance/resource-graph/overview

- Create and deploy Azure deployment stacks in Bicep  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deployment-stacks

- Quickstart: Create and deploy a deployment stack with Bicep  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/quickstart-create-deployment-stacks

- Lock your Azure resources to protect your infrastructure  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/lock-resources

- Naming rules and restrictions for Azure resources  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/resource-name-rules

- Define your tagging strategy  
  https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-tagging

- Tutorial: Manage tag governance with Azure Policy  
  https://learn.microsoft.com/en-us/azure/governance/policy/tutorials/govern-tags

## Delegated administration and cost
- Azure Lighthouse documentation  
  https://learn.microsoft.com/en-us/azure/lighthouse/

- Azure Lighthouse architecture  
  https://learn.microsoft.com/en-us/azure/lighthouse/concepts/architecture

- Cost Management + Billing  
  https://learn.microsoft.com/en-us/azure/cost-management-billing/

- Overview of Cost Management  
  https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/overview-cost-management

- Tutorial: Create and manage Cost Management exports  
  https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/tutorial-improved-exports

- Allocate Azure costs  
  https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/allocate-costs

- Introduction to cost allocation  
  https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/cost-allocation-introduction

- View your billing accounts in Azure portal  
  https://learn.microsoft.com/en-us/azure/cost-management-billing/manage/view-all-accounts

- Organize your invoice based on your needs  
  https://learn.microsoft.com/en-us/azure/cost-management-billing/manage/mca-section-invoice

---

# 19. Final Summary

Week 1 is about **how Azure is organized, controlled, governed, and made safe before large-scale workload adoption**.

The strongest mental model is:

- **management groups + subscriptions + resource groups** = scope design
- **landing zones** = scalable platform architecture
- **Entra + PIM + Conditional Access + managed identities** = identity and privileged access
- **RBAC + deny + locks** = authorization guardrails
- **Azure Policy + initiatives + exemptions + remediation** = governance enforcement
- **ARG + deployment stacks + tags + naming** = platform hygiene and evidence
- **Lighthouse + Cost Management** = delegated operations and accountability

When you can explain the **scope model, identity model, authorization model, governance model, and cost-ownership model** together, Azure Week 1 is understood architecturally.
