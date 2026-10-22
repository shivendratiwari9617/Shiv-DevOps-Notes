# GCP Study Notes — Week 1
## Organization, Identity, Landing Zone, and Governance

> Deep technical Week 1 guide.  
> Includes:
> - service-by-service technical notes
> - key entities
> - examples
> - best practices
> - common mistakes
> - troubleshooting cues
> - official references  
>
> **Noise control:** concise, high density, minimal filler.

---

# 1. Why Week 1 Matters

Week 1 is the **control-plane foundation** of Google Cloud.

Before workloads, networks, or data platforms scale, you need to answer:

- Who owns the cloud estate?
- How are projects organized?
- How is access granted and constrained?
- How do policy guardrails inherit?
- How do you stop data exfiltration?
- How do you see assets, policy exposure, and drift?
- How are billing, quotas, and project vending controlled?

The core idea is:

> **A GCP landing zone is a governed resource hierarchy plus identity, policy, billing, visibility, and platform standards.**

If this layer is weak, every later layer becomes harder to secure and operate.

---

# 2. Learning Outcomes

By the end of Week 1, you should be able to:

- explain **Cloud Identity / Google Workspace** as the identity foundation
- explain the **resource hierarchy**: organization, folders, projects, resources
- explain **labels** and **tags** and how they differ
- explain **IAM allow policies**, **deny policies**, **principal access boundary policies**, and **IAM Conditions**
- explain **service accounts**, **service agents**, **workload identity federation**, and **service account impersonation**
- explain **Organization Policy** constraints, inheritance, tags, custom constraints, and dry-run style rollout patterns
- explain **Access Context Manager** and **VPC Service Controls**
- explain **Cloud Asset Inventory**
- explain **Policy Intelligence** tools such as Policy Analyzer, Policy Troubleshooter, and recommender-style insights
- explain **Shared VPC** and project/folder landing-zone patterns
- explain **Cloud Billing**, budgets, and **Cloud Quotas**
- explain **Config Connector** and **Config Controller**
- explain **Assured Workloads**
- explain **Cloud Audit Logs** as a governance support layer

---

# 3. Week 1 Architectural Mental Model

Think in layers:

1. **Identity layer**  
   Cloud Identity, users, groups, domains, federation

2. **Hierarchy layer**  
   organization, folders, projects

3. **Access-control layer**  
   IAM, conditions, deny, principal access boundaries, service accounts

4. **Policy and guardrail layer**  
   Organization Policy, tags, custom constraints, Access Context Manager, VPC Service Controls

5. **Visibility layer**  
   Cloud Asset Inventory, Policy Intelligence, Cloud Audit Logs

6. **Platform structure layer**  
   Shared VPC, project factories, Config Connector, Config Controller

7. **Business and compliance layer**  
   Cloud Billing, budgets, quotas, Assured Workloads

A good Week 1 design makes later networking, compute, and data architectures safer by default.

---

# 4. Cloud Identity / Google Workspace

Cloud Identity is Google’s identity foundation for centrally managing users and groups, and it can federate with external identity providers such as Active Directory and Microsoft Entra ID. [1]

## 4.1 Technical purpose

Cloud Identity provides:
- user lifecycle management
- group management
- SSO and federation foundation
- admin role delegation
- identity source for Google Cloud access

In many enterprises, Google Cloud access is not really “IAM-first.” It is **identity-source first**, then IAM maps those identities to resources.

## 4.2 Core entities

### Domain
Verified internet domain associated with the organization.

### User
Human identity in the directory.

### Group
Collection of users, groups, or service accounts.

### Dynamic group
Rule-based group membership for supported editions.

### Organizational unit
Directory-side grouping for some Google admin controls.  
Do **not** confuse this with GCP folders.

### Admin role
Delegated administrative role in Cloud Identity or Google Workspace.

### Identity federation / SSO configuration
Trust relationship that lets external identity systems authenticate users.

## 4.3 Example

An enterprise uses:
- Cloud Identity federated with Active Directory
- groups like `gcp-platform-admins`, `gcp-security-readers`, `gcp-devops-prod-approvers`
- IAM bindings granted to groups, not individual users

## 4.4 Best practices
- use groups, not direct user bindings, wherever possible
- separate workforce identity from workload identity
- federate with enterprise IdP instead of creating unmanaged local users at scale
- define admin delegation carefully

## 4.5 Common mistakes
- direct user-level IAM everywhere
- poor group naming standards
- confusing directory organizational units with GCP folders
- no joiner/mover/leaver automation

## 4.6 Troubleshooting cues
- user can authenticate but lacks access → usually IAM or group propagation issue
- user appears in IdP but not effective in GCP → federation, sync, or group scope issue
- too many emergency direct grants → identity governance model is weak

---

# 5. Resource Manager and the Resource Hierarchy

Resource Manager defines the hierarchy: **organization → folders → projects → resources**. Organization policies and IAM inheritance are scoped by this structure. Google documents the hierarchy and explains that Organization Policy relies on it. [2][3]

## 5.1 Organization

### Technical purpose
Top-level root node for a company’s Google Cloud estate.

### Key characteristics
- tied to a Cloud Identity / Google Workspace account
- root for IAM inheritance
- root for Organization Policy inheritance
- root for many org-level services such as billing visibility, VPC-SC, and asset inventory exports

### Best practices
- create and use a single org root for enterprise governance
- minimize powerful direct org-level bindings
- reserve org-level admin roles for platform/security teams

### Common mistakes
- scattered project ownership without org-level governance
- too many humans with org-wide admin power

---

## 5.2 Folders

Folders are intermediate hierarchy nodes used for grouping projects and applying inherited policy and IAM.

### Common folder patterns
- by environment: `prod`, `nonprod`, `sandbox`
- by business unit: `finance`, `retail`, `platform`
- by control plane: `networking`, `security`, `shared-services`

### Example
A structure such as:

- organization
  - `platform`
    - `shared-network`
    - `security`
  - `applications`
    - `prod`
    - `nonprod`
    - `sandbox`

### Best practices
- use folders for inheritance boundaries
- align folders to governance needs, not cosmetic names
- keep folder purpose obvious

### Common mistakes
- deep, confusing folder trees with no policy meaning
- using projects as if they were folders
- designing by org chart only when environment or risk boundary is the real need

---

## 5.3 Projects

Projects are the main tenancy and lifecycle unit in GCP.

### Technical role
A project is the boundary for:
- API enablement
- quota consumption
- billing attachment
- many IAM bindings
- logging and monitoring defaults
- resource lifecycle and deletion
- service enablement

### Why projects matter
Projects are where teams actually deploy resources.  
Folders and organizations govern them, but projects are the practical operating boundary.

### Example
Separate projects for:
- `prod-payments-api`
- `nonprod-payments-api`
- `shared-observability`
- `host-network-prod`

### Best practices
- use many projects, not one giant shared project
- separate production from non-production
- separate host-network/shared-service projects from service projects
- automate project creation and baseline setup

### Common mistakes
- using one project for too many workloads
- mixing environments inside one project
- manually created projects with inconsistent APIs, labels, IAM, and billing configuration

---

## 5.4 Resources

Resources live inside projects and inherit from the hierarchy above.

### Important Week 1 point
IAM and Organization Policy are inheritance-aware, but inheritance is not “simple copy.”  
Effective access and policy are the combined result of:
- parent bindings
- child bindings
- conditions
- deny
- principal access boundary constraints
- service-specific control layers

---

# 6. Labels and Tags

Google Cloud has both **labels** and **tags**, and they are not interchangeable.

## 6.1 Labels

### Technical purpose
Metadata for organization, filtering, reporting, billing allocation, and automation.

### Characteristics
- key/value metadata
- commonly used for cost attribution and inventory grouping
- not the same as org-level conditional policy tags

### Example
Labels:
- `env=prod`
- `cost_center=fin-042`
- `owner=payments-team`

### Best practices
- standardize required labels for projects
- use labels for search, reporting, cost governance, and automation hooks
- validate label standards in project vending workflows

### Common mistakes
- free-form labels with no standards
- expecting labels alone to drive IAM or Organization Policy logic everywhere

---

## 6.2 Tags

Tags in Google Cloud are governance objects used in policy evaluation, especially for IAM Conditions and Organization Policy scenarios.

### Core entities
- tag key
- tag value
- tag binding

### Technical role
Tags are governance-aware policy signals.  
They are stronger for policy use than plain labels.

### Example
A project or resource gets a tag:
- `environment/prod`
- `data_classification/restricted`

Then IAM Conditions or Organization Policy logic can reference those tags.

### Best practices
- use tags for governance semantics
- use labels for metadata and reporting
- define a small, controlled tag taxonomy

### Common mistakes
- confusing tags with labels
- too many overlapping governance tags
- no ownership for tag-key creation

---

# 7. IAM in Google Cloud

Google Cloud IAM controls who can do what on which resources.

Google documents multiple IAM policy types: **allow**, **deny**, and **principal access boundary** policies. [4]

## 7.1 IAM allow policies

### Technical purpose
Grant roles to principals on resources.

### Core entities
- principal
- role
- binding
- condition
- resource

### Principal examples
- user
- group
- service account
- domain
- workforce or workload federated principal sets

### Role types
- basic roles
- predefined roles
- custom roles

### Example
Grant `roles/viewer` to a security-auditors group at the folder level.

### Best practices
- prefer groups over users
- prefer predefined roles over overly broad custom roles unless needed
- scope grants as low as practical, but high enough to avoid sprawl
- use conditions for time, attribute, or tag-aware access control

### Common mistakes
- project owner/editor style grants for convenience
- custom roles with no lifecycle review
- direct user grants that bypass group governance

---

## 7.2 IAM Conditions

Google documents IAM Conditions as attribute-based logic usable in allow and deny policies. [5]

### Technical purpose
Add context to access decisions.

### Common condition signals
- resource name
- date/time
- tags
- request context in supported scenarios

### Example
Allow a break-glass admin role only until a specific timestamp.

### Best practices
- use for temporary elevated access
- use with tags for governance-aware access
- keep expressions understandable and reviewed

### Common mistakes
- unreadable condition expressions
- conditions used where separate groups/roles would be clearer
- expired conditions left undocumented

---

## 7.3 IAM deny policies

### Technical purpose
Explicitly block permissions for specific principals or principal sets, even if allow bindings exist.

### Why they matter
Allow-only IAM models often struggle to enforce hard prohibitions.  
Deny policies provide a stronger top-down control.

### Example
Deny deletion-sensitive permissions for a broad admin population outside a break-glass set.

### Best practices
- use deny for hard guardrails
- test carefully before broad rollout
- document exceptions explicitly

### Common mistakes
- deny policies added without impact analysis
- troubleshooting access issues without remembering deny takes precedence over allow in applicable cases

---

## 7.4 Principal Access Boundary (PAB) policies

Google documents principal access boundary policies as a way to limit the resources a set of principals are eligible to access. [6]

### Technical purpose
Restrict *where* principals can exercise permissions, even if they hold broad roles.

### Example
A platform-admin principal set may have broad admin roles but only within projects under a specific folder.

### Best practices
- use for delegated administration boundaries
- align PAB scopes to folder/project boundaries
- combine with group-based administration models

### Common mistakes
- expecting PAB to replace all deny use cases
- applying PAB without clear principal-set design
- weak documentation of effective admin boundaries

---

## 7.5 Service accounts

Service accounts are workload identities.

### Technical purpose
Allow applications and services to call Google APIs as a non-human principal.

### Important distinction
- **service account** = workload identity
- **service agent** = Google-managed service principal for a Google service

### Best practices
- one service account per workload or trust boundary
- grant minimum required roles
- prefer short-lived credential mechanisms over long-lived keys
- use service account impersonation where possible

### Common mistakes
- one shared service account for many apps
- downloading and distributing service account keys broadly
- granting primitive broad roles to service accounts

---

## 7.6 Service account impersonation

### Technical purpose
Let a principal act as a service account without possessing its private key.

### Why it matters
This is usually safer than key-based access.

### Example
A CI system impersonates a deployment service account during release, instead of storing JSON keys.

### Common mistakes
- still using static keys when impersonation is available
- not separating “can use service account” from “can administer service account”

---

## 7.7 Workload Identity Federation

Google documents Workload Identity Federation as a way for on-prem or multicloud workloads to access Google Cloud using federated identities instead of service account keys. [7]

### Technical purpose
Eliminate long-lived Google service account keys for external workloads.

### Core entities
- workload identity pool
- provider
- attribute mapping
- attribute condition
- principal set
- service account binding or direct resource access patterns depending on design

### Example
A GitHub Actions or on-prem workload exchanges OIDC or external credentials for short-lived Google access through federation.

### Best practices
- prefer federation over key distribution
- define attribute mapping carefully
- use conditions to restrict trusted subjects and audiences
- test issuer and subject claims thoroughly

### Common mistakes
- weak attribute mapping conditions
- broad trust of external identities
- debugging access without inspecting mapped attributes

---

# 8. Organization Policy

Organization Policy is Google Cloud’s centralized guardrail system for controlling how resources can be configured. Google documents that Resource Manager defines where resources live and Organization Policy defines restrictions on how they can be configured. [3]

## 8.1 Technical purpose
Enforce organization-wide or folder/project-scoped restrictions.

## 8.2 Core entities

### Constraint
The rule definition being enforced.

### Policy
The configured application of a constraint at org, folder, or project scope.

### Inheritance
Child resources inherit from parents unless overridden where supported by policy semantics.

### Custom constraint
Organization-defined constraint logic for supported services/resources.

### Tag-aware policy
Policy logic influenced by tags.

### Dry-run style rollout pattern
Progressive evaluation and staged rollout practice for reducing breakage when introducing constraints.

## 8.3 Common constraint themes
- allowed resource locations
- allowed services or service usage restrictions
- external IP limitations
- domain-restricted sharing
- workload identity federation control
- service account key creation limits
- VM or platform restrictions
- compliance guardrails

## 8.4 Example
At the org level:
- restrict resource locations to approved regions
- prohibit service account key creation
- restrict external IP usage for regulated folders

At a specific folder:
- allow only approved services for sandbox or regulated workloads

## 8.5 Best practices
- define a baseline org-wide policy set
- roll out in stages
- test policy effect in lower-risk folders first
- document allowed exception process
- use tags when policies need conditional differentiation

## 8.6 Common mistakes
- applying constraints at org root without impact review
- no exception process for valid edge cases
- not aligning project vending with required constraints
- weak communication with app teams before enforcement

## 8.7 Troubleshooting cues
- resource creation suddenly failing → inspect org policy inherited from parent
- project moved to new folder and behavior changed → likely inherited policy shift
- policy appears correct but service still blocked → inspect tags, constraint mode, and effective policy path

---

# 9. Access Context Manager and VPC Service Controls

Access Context Manager provides policy constructs for context-aware access and is also the control plane for VPC Service Controls. Google documents it as an attribute-based access control mechanism based on identity, device, and IP, and notes that VPC Service Controls service perimeters can be applied using it. [8][9]

## 9.1 Access Context Manager

### Technical purpose
Define access levels and scoped context-aware policies.

### Core entities
- access policy
- access level
- service perimeter policy integration
- scoped policy in supported delegation models

### Example
Only users from managed devices and approved IP ranges can access certain sensitive admin flows.

### Best practices
- treat access levels as enterprise security policy objects
- align device/IP/user context to real trust levels
- review exceptions regularly

### Common mistakes
- overcomplicated access-level logic
- context rules that users cannot realistically satisfy
- no coordination with identity and endpoint-management teams

---

## 9.2 VPC Service Controls

### Technical purpose
Reduce data exfiltration risk by creating service perimeters around supported Google-managed services.

### Core entities
- service perimeter
- regular perimeter
- bridge perimeter
- ingress rule
- egress rule
- access level integration
- dry-run / staged rollout practice
- scoped administration in supported models

### Important concept
VPC Service Controls is **not** traditional VPC firewalling.  
It is a **service-level data exfiltration control boundary**.

### Example
Sensitive analytics projects are placed inside a perimeter so that BigQuery, Storage, and other supported services cannot be accessed from outside approved contexts.

### Best practices
- design perimeters around data sensitivity and trust zones
- start with dry-run/staged design practices before enforcement
- explicitly model ingress and egress
- test developer workflows and service-to-service dependencies

### Common mistakes
- assuming perimeters solve all network isolation needs
- perimeter design based only on org chart
- no testing for CI/CD, shared services, or third-party integrations
- accidental blocking of internal platform workflows

### Troubleshooting cues
- access denial with VPC-SC unique ID → inspect perimeter rule, access level, source context, and ingress/egress policy
- application works outside perimeter but not inside → likely dependency or egress path issue
- confusing IAM allow with VPC-SC denial → both must allow the action path

---

# 10. Cloud Asset Inventory

Cloud Asset Inventory is Google Cloud’s global metadata inventory service for assets, IAM policies, history, search, export, and analysis. Google explicitly notes it is for broad visibility and analytics, not a synchronous control plane. [10][11]

## 10.1 Technical purpose
Provide authoritative inventory and change visibility across the cloud estate.

## 10.2 Core entities

### Asset
Tracked resource, policy, or runtime-relevant entity.

### Asset type
Canonical type identifier for a resource class.

### History
Point-in-time historical metadata.

### Feed
Change stream to Pub/Sub.

### Search
Query across resources and IAM policy metadata.

### Export
Delivery to BigQuery or Cloud Storage.

### Relationship
Supported linkage between assets in enhanced inventory contexts.

## 10.3 Example
A security team exports all IAM policies and project metadata to BigQuery daily, then runs queries to detect:
- public exposure
- ownerless projects
- stale service accounts
- noncompliant labels

## 10.4 Best practices
- use Cloud Asset Inventory for inventory, audit, analytics, and evidence
- export to BigQuery for enterprise analysis
- use feeds for near-real-time monitoring patterns
- treat it as visibility, not transactional truth for immediate control decisions

## 10.5 Common mistakes
- building mission-critical real-time control logic on inventory export latency assumptions
- not standardizing asset queries and exports
- ignoring history as an audit source

## 10.6 Troubleshooting cues
- resource exists but not in expected export → inspect export timing, asset type support, permissions
- IAM search result differs from observed access → inspect deny, conditions, PAB, or service-specific access layers not fully represented in some analysis modes
- missing analytics value → inventory exists but governance questions were never defined

---

# 11. Policy Intelligence

Policy Intelligence is the family of tools for understanding and optimizing access and policy. Google documents it as a collection of tools that help understand and manage policies. [12]

## 11.1 Policy Analyzer

Google documents Policy Analyzer for allow policies as a tool to determine which principals have which access to which resources, with the note that it supports IAM allow policies and not all other control mechanisms like deny. [13]

### Technical purpose
Answer:
- who has a role
- who has a permission
- where access is granted

### Example
Find all principals who can administer storage in production folders.

### Best practices
- use for periodic access review
- combine with asset inventory and IAM review cycles
- remember scope and supported-policy limitations

### Common mistakes
- treating it as the only access truth without considering deny, VPC-SC, or service-side controls

---

## 11.2 Policy Troubleshooter

Google documents IAM Policy Troubleshooter as a tool for diagnosing why access is allowed or denied. [14]

### Technical purpose
Explain access outcomes for a principal/resource/permission tuple.

### Example
Why can `deploy-bot@...` not update a project resource even though the team expects it to?

### Best practices
- use during incident and deployment troubleshooting
- inspect the exact resource and permission
- compare expected group membership and conditions

### Common mistakes
- troubleshooting the wrong resource level
- forgetting inherited bindings or deny effects
- ignoring service account impersonation path

---

## 11.3 Recommender-style policy optimization

Policy Intelligence also includes recommendation and optimization capabilities around excess access in supported contexts.

### Technical purpose
Reduce excessive permissions and improve least privilege.

### Best practices
- review recommendations, but apply with workload knowledge
- treat recommendation output as input to governance, not blind automation

### Common mistakes
- applying least-privilege recommendations without understanding burst, break-glass, or seasonal usage patterns

---

# 12. Shared VPC

Google documents Shared VPC as a model where multiple projects attach resources to a common VPC network hosted in a designated host project. [15]

## 12.1 Technical purpose
Separate network administration from application project ownership.

## 12.2 Core entities

### Host project
Project that owns the Shared VPC network resources.

### Service project
Project that consumes subnets from the host project.

### Shared subnet
Subnet owned in host project and used by service-project resources.

### Network admin boundary
Control plane separation between central networking team and application teams.

## 12.3 Example
A platform team runs:
- `host-network-prod`
- `host-network-nonprod`

Application teams deploy GKE clusters and VMs into service projects attached to approved subnets in those host projects.

## 12.4 Best practices
- keep host projects dedicated to networking
- separate prod and nonprod host projects
- define subnet ownership and IP allocation clearly
- use Shared VPC for centralized networking, not for collapsing all workloads into one project

## 12.5 Common mistakes
- mixing host-project networking resources with unrelated app workloads
- poor subnet governance
- no clear handoff between network and app teams

## 12.6 Troubleshooting cues
- app team has project access but cannot use subnet → likely Shared VPC attachment or IAM on network resources
- cluster create fails in service project → often subnet secondary range or host-project permission issue
- IP exhaustion → subnet planning and governance issue, not only workload issue

---

# 13. Config Connector and Config Controller

Google documents Config Connector as an open-source Kubernetes add-on that lets you manage Google Cloud resources through Kubernetes, and Config Controller as a hosted version that simplifies installation and maintenance. [16][17]

## 13.1 Config Connector

### Technical purpose
Manage Google Cloud resources declaratively as Kubernetes custom resources.

### Core entities
- Kubernetes CRD-backed resource
- namespace/project mapping
- controller reconciliation loop
- service account / workload identity used by controller

### Example
A platform cluster manages:
- Pub/Sub topics
- IAM policies
- Storage buckets
- SQL instances  
through Kubernetes manifests reviewed in Git.

### Best practices
- treat config as code
- separate app-cluster concerns from infra-control concerns
- define reconciliation ownership
- enforce policy on manifests before apply

### Common mistakes
- mixing infrastructure and app manifests with no lifecycle separation
- overgranting the controller’s service account
- no Git review around changes

---

## 13.2 Config Controller

### Technical purpose
Provide a hosted control plane for Config Connector-based resource management.

### Best fit
Platform teams that want:
- managed control plane
- less cluster maintenance burden
- Kubernetes-style declarative infrastructure management for GCP resources

### Best practices
- use as a platform-control-plane tool, not an ad hoc script replacement
- define tenancy, namespaces, and RBAC carefully
- integrate with Policy Controller / Gatekeeper patterns where applicable

### Common mistakes
- no clear platform ownership
- hosted controller introduced without resource lifecycle standards
- uncontrolled namespace-to-project mappings

---

# 14. Cloud Billing, Budgets, and Quotas

Governance is incomplete without business control.

---

## 14.1 Cloud Billing

Google documents Cloud Billing accounts as the billing entity associated with projects and budget tooling. [18]

### Technical purpose
Attach projects to a billing account, centralize spend visibility, and support budget/governance workflows.

### Core entities
- billing account
- project billing link
- budget
- cost reports
- export destinations

### Best practices
- centralize billing-account governance
- separate billing administration from project administration
- export billing data for analytics and chargeback

### Common mistakes
- projects created without proper billing linkage controls
- no spend ownership model
- platform teams blind to cost because billing data was not exported or categorized

---

## 14.2 Budgets

Google documents the Budget API and budget tooling for alerts and large-scale programmatic budget management. [19][20]

### Technical purpose
Set cost thresholds and receive alerts.

### Example
Budget at billing-account level for prod shared services, with alert thresholds at 50%, 80%, and forecasted overrun.

### Best practices
- create budgets by product/team/environment
- use both actual and forecasted thresholds
- align alerts to owners who can act

### Common mistakes
- one top-level budget only
- alerts sent to finance but not engineering
- thresholds too noisy or too lax

---

## 14.3 Cloud Quotas

Google documents Cloud Quotas as the quota system for monitoring and updating quota usage, with project-level being the dominant adjustment scope for most services. [21][22]

### Technical purpose
Control service consumption ceilings and protect platform operations.

### Core entities
- quota
- usage
- quota preference / adjustment where supported
- quota project
- quota alerting

### Example
A migration program pre-requests Compute Engine quota increases in target projects before cutover waves.

### Best practices
- review quotas before launches and migrations
- alert on quota usage
- define quota readiness as part of project-vending and migration checklists
- understand quota project behavior for API clients

### Common mistakes
- discovering quota limits during production launch
- assuming quotas are only finance limits
- no monitoring for critical service quotas

---

# 15. Assured Workloads

Google documents Assured Workloads as a framework-driven control package model where the top-level folder becomes an Assured Workloads folder and controls are inherited by descendants. It also monitors folders for compliance violations. [23][24]

## 15.1 Technical purpose
Help regulated workloads meet required compliance control frameworks through managed guardrails and monitoring.

## 15.2 Core entities

### Assured Workloads folder
Top-level folder with inherited compliance controls.

### Control package / framework
Selected compliance package.

### Violation monitoring
Detection of noncompliant changes.

### Supported products set
Services approved under a framework/package.

## 15.3 Example
A regulated workload folder is created with a selected framework, then projects under it inherit region, service, and compliance constraints.

## 15.4 Best practices
- use for workloads with explicit regulatory requirements
- assess existing projects before migration into the folder
- understand supported-product restrictions
- monitor violations continuously

## 15.5 Common mistakes
- moving a workload into Assured Workloads without compatibility assessment
- assuming all needed services are available under every control package
- no process to remediate folder violation findings

---

# 16. Cloud Audit Logs

Google documents Cloud Audit Logs as the audit logging layer for administrative activities, data access, system events, and policy-denied events, with Data Access logs generally disabled by default except for BigQuery. [25][26]

## 16.1 Technical purpose
Provide authoritative audit records for governance, incident response, and compliance evidence.

## 16.2 Core entities

### Admin Activity logs
Administrative operations on resources.

### Data Access logs
Reads and writes to resource data for supported services.

### System Event logs
Google-system-triggered events.

### Policy Denied logs
Records of policy-based denials.

### Log sink
Routing destination for logs.

### Retention / bucket
Storage and retention configuration in Logging.

## 16.3 Example
A security investigation uses:
- Admin Activity logs to identify who changed IAM
- Policy Denied logs to confirm org policy blocks
- Data Access logs for sensitive service usage where enabled

## 16.4 Best practices
- export key audit logs centrally
- enable needed Data Access logs intentionally
- protect log sinks and retention policies
- treat audit logs as critical evidence data

## 16.5 Common mistakes
- assuming all useful Data Access logs are on by default
- no centralized sink/export
- weak access control on logs

---

# 17. Landing Zone Design Patterns

Week 1 is not only about individual services. It is about combining them.

## 17.1 Common landing-zone structure

### Identity
- Cloud Identity federated with enterprise IdP
- groups mapped to admin and app personas

### Hierarchy
- organization
- top folders by platform/app/environment
- projects as workload units

### Access
- IAM group-based bindings
- service accounts for workloads
- conditions and deny/PAB where needed

### Guardrails
- Organization Policy baseline
- Access Context Manager
- VPC Service Controls for sensitive data zones

### Visibility
- Cloud Asset Inventory exports
- Policy Analyzer and Troubleshooter
- Cloud Audit Logs export

### Platform
- Shared VPC host projects
- project vending automation
- Config Connector or Config Controller for declarative control

### Business and compliance
- Cloud Billing standards
- budgets and quota readiness
- Assured Workloads for regulated environments

---

# 18. Service Selection and Comparison Guide

## 18.1 Labels vs tags
- **labels** = metadata, reporting, cost, filtering
- **tags** = governance-aware policy signal

## 18.2 Allow vs deny vs PAB
- **allow** = grants permissions
- **deny** = explicitly blocks permissions
- **PAB** = restricts which resources principals may access

## 18.3 Shared VPC vs one-project networking
- **Shared VPC** = centralized networking across many projects
- **one-project networking** = simpler but weaker separation for larger estates

## 18.4 Cloud Asset Inventory vs Policy Intelligence
- **Cloud Asset Inventory** = inventory, history, exports, search
- **Policy Intelligence** = access analysis and troubleshooting

## 18.5 Organization Policy vs VPC Service Controls
- **Organization Policy** = configuration guardrails
- **VPC Service Controls** = service-level data exfiltration boundary

## 18.6 Config Connector vs Config Controller
- **Config Connector** = self-managed Kubernetes-based GCP resource control
- **Config Controller** = hosted control plane for that model

---

# 19. End-to-End Example

A large enterprise wants:
- federated identity
- centralized networking
- regulated folder for finance
- strict region controls
- project-per-application model
- inventory exports
- policy troubleshooting
- no service account keys

A strong design could be:

- Cloud Identity federated with enterprise IdP
- org root with folders for `platform`, `prod`, `nonprod`, `regulated`
- group-based IAM
- deny and PAB used for delegated admin boundaries
- org policy restricts regions and service account key creation
- sensitive projects inside VPC Service Controls perimeter
- Cloud Asset Inventory exported to BigQuery
- Shared VPC host projects for central networks
- project vending pipeline applies labels, tags, APIs, billing, audit sinks
- Config Controller manages baseline resources declaratively
- Assured Workloads used for the regulated folder
- quotas and budgets reviewed during project creation and migration planning

This is the Week 1 principle:

> the landing zone is the product of hierarchy, identity, policy, visibility, and automation working together.

---

# 20. Troubleshooting Framework

## 20.1 Access issue
- principal identity correct?
- group membership correct?
- allow binding present?
- deny policy blocking?
- PAB limiting scope?
- condition expression failing?
- VPC-SC or service-side control denying?

## 20.2 Resource creation blocked
- organization policy inherited?
- folder move changed effective policy?
- required tag missing?
- service not allowed?
- quota exhausted?
- API not enabled in project?

## 20.3 Project operations inconsistent
- billing account linked?
- project created outside vending process?
- missing labels/tags?
- Shared VPC attachment missing?
- folder placement wrong?

## 20.4 Audit or inventory gaps
- Cloud Asset export configured?
- feed/export permissions correct?
- Data Access logs enabled where needed?
- sink destination secure and active?

---

# 21. Common Week 1 Mistakes

- no clean folder strategy
- treating projects as random buckets
- direct user IAM grants
- service account key sprawl
- labels used where tags were needed
- org policies applied with no rollout plan
- VPC-SC perimeter designed without workflow testing
- Shared VPC host projects mixed with app workloads
- asset inventory collected but never analyzed
- audit logs not exported centrally
- quotas discovered only during launch or migration
- compliance workloads placed outside Assured Workloads or equivalent controls

---

# 22. Week 1 Revision Checklist

You should be able to explain clearly:

- what the organization, folders, projects, and resources are
- how labels differ from tags
- how allow policies, deny policies, PABs, and IAM Conditions differ
- how service accounts differ from service agents
- why impersonation is better than long-lived keys
- what Workload Identity Federation solves
- what Organization Policy enforces and how inheritance works
- what Access Context Manager and VPC Service Controls do
- what Cloud Asset Inventory is and what it is not
- what Policy Analyzer and Troubleshooter are used for
- what Shared VPC host and service projects are
- how Config Connector differs from Config Controller
- how billing, budgets, and quotas fit into landing-zone readiness
- what Assured Workloads changes for regulated workloads
- what Cloud Audit Logs provide and which logs need deliberate enablement

---

# 23. Memory Anchors

- **Cloud Identity** = workforce identity foundation
- **organization** = root of GCP governance
- **folder** = inheritance boundary
- **project** = main workload tenancy unit
- **label** = metadata
- **tag** = governance signal
- **IAM allow** = grants access
- **IAM deny** = blocks access
- **PAB** = limits reachable resources for principals
- **service account** = workload identity
- **impersonation** = short-lived use of service account identity
- **Workload Identity Federation** = external workload access without keys
- **Organization Policy** = guardrails on configuration
- **Access Context Manager** = context-aware access policy layer
- **VPC Service Controls** = data exfiltration perimeter
- **Cloud Asset Inventory** = inventory and history
- **Policy Intelligence** = analyze and troubleshoot policy
- **Shared VPC** = centralized networking across projects
- **Config Connector** = KRM-style GCP resource management
- **Config Controller** = hosted Config Connector control plane
- **Cloud Billing** = billing account and spend control layer
- **Cloud Quotas** = service consumption ceilings
- **Assured Workloads** = regulated folder controls
- **Cloud Audit Logs** = audit evidence trail

---

# 24. Official References

## Identity and hierarchy
- Cloud Identity overview  
  https://docs.cloud.google.com/identity/docs/overview

- Resource hierarchy overview  
  https://docs.cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy

- Resource Manager overview  
  https://docs.cloud.google.com/resource-manager/docs/resource-manager-overview

- Organization resource setup  
  https://docs.cloud.google.com/resource-manager/docs/creating-managing-organization

- Folders  
  https://docs.cloud.google.com/resource-manager/docs/creating-managing-folders

- Projects  
  https://docs.cloud.google.com/resource-manager/docs/manage-projects-within-folder

## IAM
- IAM policy types  
  https://docs.cloud.google.com/iam/docs/policy-types

- IAM Conditions overview  
  https://docs.cloud.google.com/iam/docs/conditions-overview

- Workload identities  
  https://docs.cloud.google.com/iam/docs/workload-identities

- Workload Identity Federation  
  https://docs.cloud.google.com/iam/docs/workload-identity-federation

- Principal access boundary policies  
  https://docs.cloud.google.com/iam/docs/principal-access-boundary-policies

- Manage access to service accounts  
  https://docs.cloud.google.com/iam/docs/manage-access-service-accounts

## Guardrails and context
- Organization Policy and Resource Manager relationship  
  https://docs.cloud.google.com/resource-manager/docs/resource-manager-overview

- Access Context Manager overview  
  https://docs.cloud.google.com/access-context-manager/docs/overview

- Create an access policy  
  https://docs.cloud.google.com/access-context-manager/docs/create-access-policy

- VPC Service Controls guidance  
  https://docs.cloud.google.com/access-context-manager/docs/overview

## Visibility and analysis
- Cloud Asset Inventory overview  
  https://docs.cloud.google.com/asset-inventory/docs/asset-inventory-overview

- Cloud Asset Inventory best practices  
  https://docs.cloud.google.com/asset-inventory/docs/best-practice

- Policy Intelligence overview  
  https://docs.cloud.google.com/policy-intelligence/docs/overview

- Policy Analyzer  
  https://docs.cloud.google.com/policy-intelligence/docs/policy-analyzer-overview

- Policy Troubleshooter  
  https://docs.cloud.google.com/policy-intelligence/docs/troubleshoot-access

## Platform structure
- Shared VPC overview  
  https://docs.cloud.google.com/vpc/docs/shared-vpc

- Shared VPC provisioning  
  https://docs.cloud.google.com/vpc/docs/provisioning-shared-vpc

- Config Connector overview  
  https://docs.cloud.google.com/config-connector/docs/overview

- Config Controller overview  
  https://docs.cloud.google.com/kubernetes-engine/config-controller/docs/overview

## Business and compliance
- Budgets  
  https://docs.cloud.google.com/billing/docs/how-to/budgets

- Budget API overview  
  https://docs.cloud.google.com/billing/docs/how-to/budget-api-overview

- Cloud Quotas overview  
  https://docs.cloud.google.com/docs/quotas/overview

- View and manage quotas  
  https://docs.cloud.google.com/docs/quotas/view-manage

- Assured Workloads overview  
  https://docs.cloud.google.com/assured-workloads/docs/overview

- Assured Workloads key concepts  
  https://docs.cloud.google.com/assured-workloads/docs/key-concepts

## Audit
- Cloud Audit Logs overview  
  https://docs.cloud.google.com/logging/docs/audit

- Best practices for Cloud Audit Logs  
  https://docs.cloud.google.com/logging/docs/audit/best-practices

---

# 25. Final Summary

Week 1 is the **governance control layer** of Google Cloud.

The strongest mental model is:

- **Cloud Identity** provides workforce identity
- **Resource Manager** provides hierarchy
- **IAM** provides access control
- **Organization Policy** provides configuration guardrails
- **Access Context Manager** and **VPC Service Controls** provide context-aware and exfiltration-aware controls
- **Cloud Asset Inventory** and **Policy Intelligence** provide visibility and analysis
- **Shared VPC**, **Config Connector**, and **Config Controller** provide platform structure
- **Billing**, **budgets**, **quotas**, and **Assured Workloads** provide business and compliance governance
- **Cloud Audit Logs** provide evidence

When you can explain not only what each service is, but how hierarchy, identity, policy, and visibility combine into a landing zone, Week 1 is understood architecturally.
