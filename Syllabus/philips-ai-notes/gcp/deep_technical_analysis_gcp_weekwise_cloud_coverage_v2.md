# Deep Technical Analysis of GCP Week-Wise Cloud Coverage — v2
## GCP Technical Study Notes with Independent Expansion of Comma-Separated and `and`-Separated Service or Entity Points

> Goal: push the notes toward a **9.8+ study bar** by making the definitions more GCP-native, less template-shaped, and more precise at the child-entity level.
> Parsing rule applied: every comma-separated value is treated as a point, and when **`and` joins two Google Cloud services or two entities**, they are treated as separate points with independent definitions.
> Format: technical definition first, with one concrete production example wherever useful.

# Google Cloud Week 1: Resource Hierarchy, Landing Zone, And Governance

## Service And Entity Coverage

### Resource Manager and Cloud Identity

- **Resource Manager**
  - **Technical definition:** Resource Manager is Google Cloud's hierarchy control plane for organizations, folders, and projects, and therefore the foundation for inheritance, ownership, and policy scope.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **Cloud Identity**
  - **Technical definition:** Cloud Identity is Google's directory and workforce identity substrate for users, groups, domains, and device-aware access feeding Google Cloud governance.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **organizations**
  - **Technical definition:** Organizations are the top-level Google Cloud hierarchy containers that anchor inherited policy, billing relationships, and centralized administration.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **folders**
  - **Technical definition:** Folders are intermediate hierarchy nodes used to group projects for delegation, inherited policy, and environment or business-unit segmentation.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **projects**
  - **Technical definition:** Projects are the primary isolation boundaries for APIs, quotas, billing links, IAM bindings, and most deployable Google Cloud resources.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **project IDs**
  - **Technical definition:** Project IDs are globally unique textual identifiers used by APIs, URLs, and resource addressing.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **project numbers**
  - **Technical definition:** Project numbers are immutable numeric identifiers frequently used by Google-managed service identities and lower-level resource references.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **labels**
  - **Technical definition:** Labels are lightweight metadata key/value pairs used for organization, automation, and cost allocation.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **tags**
  - **Technical definition:** Tags are hierarchical metadata constructs used for conditional access and organization policy evaluation on Google Cloud resources.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **Cloud Identity users**
  - **Technical definition:** Cloud Identity users are workforce identity objects representing individual human principals in Google's managed directory.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **groups**
  - **Technical definition:** Groups are directory membership objects used to assign access, delegate administration, and manage policy at scale.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.
- **domains**
  - **Technical definition:** Domains are verified namespace boundaries that anchor identity ownership and organization association in Cloud Identity.
  - **Concrete production example:** Example: a new production project is created under the correct folder so inherited IAM, organization policies, billing, labels, and Shared VPC attachments apply automatically.

### Identity and Access Management (IAM)

- **Identity**
  - **Technical definition:** Identity is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **Access Management (IAM)**
  - **Technical definition:** Access Management (IAM) is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **principals**
  - **Technical definition:** Principals are the identities that may be granted access, including users, groups, service accounts, domains, and federated identities.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **roles**
  - **Technical definition:** Roles are predefined or custom permission bundles that map principals to allowed actions.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **permissions**
  - **Technical definition:** Permissions are the granular action strings evaluated during IAM authorization checks.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **allow policies**
  - **Technical definition:** Allow policies are IAM binding documents that grant members roles on resources, optionally under conditions.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **deny policies**
  - **Technical definition:** Deny policies are explicit block documents that prevent specified permissions even when an allow grant exists elsewhere.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **IAM Conditions**
  - **Technical definition:** IAM Conditions are CEL-based predicates attached to bindings so access depends on context such as request attributes, principal attributes, or resource state.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **service accounts**
  - **Technical definition:** Service accounts are non-human identities used by workloads and Google-managed services to authenticate to APIs.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **service account keys**
  - **Technical definition:** Service account keys are long-lived credential files for service accounts and therefore represent a higher-risk credential mechanism than attached or federated identity.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **workload identity pools**
  - **Technical definition:** Workload identity pools are federation containers that let external identities map into Google Cloud principals without key creation.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **workload identity providers**
  - **Technical definition:** Workload identity providers are trust definitions inside a workload identity pool that validate and map external identity assertions.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.
- **policy bindings**
  - **Technical definition:** Policy bindings are the member-to-role grants inside an IAM policy, optionally constrained by IAM Conditions.
  - **Concrete production example:** Example: an external CI system uses workload identity federation instead of a service account key so build jobs receive temporary, scoped access only.

### Organization Policy Service

- **Organization Policy Service**
  - **Technical definition:** Organization Policy Service is Google Cloud's preventive governance layer for constraining which configurations, locations, identities, and resource behaviors are allowed across the hierarchy.
  - **Concrete production example:** Example: a team dry-runs a constraint blocking external IPs before enforcement to identify which VM templates would fail in production.
- **organization policies**
  - **Technical definition:** Organization policies are hierarchy-scoped constraint-enforcement objects applied to organizations, folders, or projects.
  - **Concrete production example:** Example: a team dry-runs a constraint blocking external IPs before enforcement to identify which VM templates would fail in production.
- **constraints**
  - **Technical definition:** Constraints are the predefined or custom rule dimensions that Organization Policy evaluates or enforces.
  - **Concrete production example:** Example: a team dry-runs a constraint blocking external IPs before enforcement to identify which VM templates would fail in production.
- **policy rules**
  - **Technical definition:** Policy rules are the allow, deny, or value-governing statements inside an organization policy.
  - **Concrete production example:** Example: a team dry-runs a constraint blocking external IPs before enforcement to identify which VM templates would fail in production.
- **inheritance**
  - **Technical definition:** Inheritance is the hierarchy behavior by which lower scopes evaluate applicable policy from parent folders or the organization.
  - **Concrete production example:** Example: a team dry-runs a constraint blocking external IPs before enforcement to identify which VM templates would fail in production.
- **dry-run policies**
  - **Technical definition:** Dry-run policies are non-enforcing organization policies used to observe potential violations before enforcing a constraint.
  - **Concrete production example:** Example: a team dry-runs a constraint blocking external IPs before enforcement to identify which VM templates would fail in production.
- **custom constraints**
  - **Technical definition:** Custom constraints are customer-authored rule definitions used to validate resource properties beyond built-in constraints.
  - **Concrete production example:** Example: a team dry-runs a constraint blocking external IPs before enforcement to identify which VM templates would fail in production.

### Cloud Asset Inventory and Policy Intelligence

- **Cloud Asset Inventory and Policy Intelligence**
  - **Technical definition:** Cloud Asset Inventory and Policy Intelligence is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **assets**
  - **Technical definition:** Assets are the discovered Google Cloud resources and attached metadata represented in Cloud Asset Inventory.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **asset feeds**
  - **Technical definition:** Asset feeds are change-delivery mechanisms that emit resource or IAM policy updates to downstream systems.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **asset history**
  - **Technical definition:** Asset history is the retained timeline of prior asset states used for audit and drift reconstruction.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **IAM policy analysis**
  - **Technical definition:** IAM policy analysis is the effective-access graph evaluation showing which principals can access which resources and through which grants.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **Policy Analyzer**
  - **Technical definition:** Policy Analyzer is the query surface for exploring effective access relationships and permission paths.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **Policy Troubleshooter**
  - **Technical definition:** Policy Troubleshooter is the decision-explainer service showing why a principal is or is not authorized for an action.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **IAM Recommender recommendations**
  - **Technical definition:** IAM Recommender recommendations are access-reduction suggestions based on observed permission usage patterns.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.

### Config Connector and Config Controller

- **Config Connector**
  - **Technical definition:** Config Connector is Google's Kubernetes-native declarative management layer that reconciles Kubernetes resource definitions into Google Cloud resources.
  - **Concrete production example:** Example: a GitOps change updates the desired private-cluster setting, and Config Connector reconciles the cluster resource without manual console drift.
- **Config Controller**
  - **Technical definition:** Config Controller is Google's managed control plane for Config Connector-based infrastructure management.
  - **Concrete production example:** Example: a GitOps change updates the desired private-cluster setting, and Config Connector reconciles the cluster resource without manual console drift.
- **Kubernetes custom resources**
  - **Technical definition:** Kubernetes custom resources are Config Connector-managed Kubernetes objects that represent desired Google Cloud resources declaratively.
  - **Concrete production example:** Example: a GitOps change updates the desired private-cluster setting, and Config Connector reconciles the cluster resource without manual console drift.
- **Google Cloud resource manifests**
  - **Technical definition:** Google Cloud resource manifests are YAML definitions describing target Google Cloud resource state for reconciliation.
  - **Concrete production example:** Example: a GitOps change updates the desired private-cluster setting, and Config Connector reconciles the cluster resource without manual console drift.
- **Config Controller instances**
  - **Technical definition:** Config Controller instances are managed control-plane environments hosting Config Connector-driven infrastructure reconciliation.
  - **Concrete production example:** Example: a GitOps change updates the desired private-cluster setting, and Config Connector reconciles the cluster resource without manual console drift.
- **namespaces**
  - **Technical definition:** Namespaces are Kubernetes scoping boundaries used to organize managed resources and administrative separation in Config Controller.
  - **Concrete production example:** Example: a GitOps change updates the desired private-cluster setting, and Config Connector reconciles the cluster resource without manual console drift.
- **GitOps-managed resources**
  - **Technical definition:** GitOps-managed resources are Google Cloud resources whose desired state is reconciled continuously from source-controlled manifests.
  - **Concrete production example:** Example: a GitOps change updates the desired private-cluster setting, and Config Connector reconciles the cluster resource without manual console drift.

### Shared VPC and project factory patterns

- **Shared VPC and project factory patterns**
  - **Technical definition:** Shared VPC and project factory patterns is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **host projects**
  - **Technical definition:** Host projects are the Shared VPC-owning projects that centralize VPC networks, subnets, and network policy.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **service projects**
  - **Technical definition:** Service projects are attached projects whose workloads consume host-project subnets and network services.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **Shared VPC networks**
  - **Technical definition:** Shared VPC networks are centrally owned VPCs exposed from a host project to one or more service projects.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **subnet attachments**
  - **Technical definition:** Subnet attachments are the relationships allowing service-project workloads to allocate interfaces in host-project subnets.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **project vending**
  - **Technical definition:** Project vending is the automated creation of projects with predefined labels, billing, IAM, APIs, logging, and network linkage.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **project ownership metadata**
  - **Technical definition:** Project ownership metadata is the structured set of labels, tags, or records identifying accountable team, environment, and business ownership.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **quota baselines**
  - **Technical definition:** Quota baselines are the minimum quota and service-enablement expectations required for a project to operate safely or fail over.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.

### Access Context Manager and VPC Service Controls

- **Access Context Manager and VPC Service Controls**
  - **Technical definition:** Access Context Manager and VPC Service Controls is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: before moving regulated analytics workloads, engineers validate that BigQuery and Cloud Storage are reachable only through the intended service perimeter paths.
- **access policies**
  - **Technical definition:** Access policies are the top-level Access Context Manager containers holding access levels and service perimeter definitions.
  - **Concrete production example:** Example: before moving regulated analytics workloads, engineers validate that BigQuery and Cloud Storage are reachable only through the intended service perimeter paths.
- **access levels**
  - **Technical definition:** Access levels are reusable context predicates describing who or what environment satisfies access conditions.
  - **Concrete production example:** Example: before moving regulated analytics workloads, engineers validate that BigQuery and Cloud Storage are reachable only through the intended service perimeter paths.
- **service perimeters**
  - **Technical definition:** Service perimeters are VPC Service Controls isolation boundaries restricting supported managed-service access and data movement.
  - **Concrete production example:** Example: before moving regulated analytics workloads, engineers validate that BigQuery and Cloud Storage are reachable only through the intended service perimeter paths.
- **perimeter bridges**
  - **Technical definition:** Perimeter bridges are controlled constructs allowing specific relationships between otherwise separate service perimeters.
  - **Concrete production example:** Example: before moving regulated analytics workloads, engineers validate that BigQuery and Cloud Storage are reachable only through the intended service perimeter paths.
- **protected services**
  - **Technical definition:** Protected services are the managed Google services placed inside a service perimeter.
  - **Concrete production example:** Example: before moving regulated analytics workloads, engineers validate that BigQuery and Cloud Storage are reachable only through the intended service perimeter paths.
- **ingress rules**
  - **Technical definition:** Ingress rules define which external identities, networks, or projects may access services inside a perimeter.
  - **Concrete production example:** Example: before moving regulated analytics workloads, engineers validate that BigQuery and Cloud Storage are reachable only through the intended service perimeter paths.
- **egress rules**
  - **Technical definition:** Egress rules define which outbound calls from a perimeter may reach destinations outside that perimeter.
  - **Concrete production example:** Example: before moving regulated analytics workloads, engineers validate that BigQuery and Cloud Storage are reachable only through the intended service perimeter paths.

### Assured Workloads

- **Assured Workloads**
  - **Technical definition:** Assured Workloads is Google's regulated-environment service for applying compliance-driven constraints on projects, resources, personnel access, and location choices.
  - **Concrete production example:** Example: a regulated workload is created only in approved regions and with restricted product choices because the chosen compliance regime enforces those limits.
- **workload folders**
  - **Technical definition:** Workload folders are Assured Workloads-created hierarchy containers grouping regulated projects under regime-specific controls.
  - **Concrete production example:** Example: a regulated workload is created only in approved regions and with restricted product choices because the chosen compliance regime enforces those limits.
- **compliance regimes**
  - **Technical definition:** Compliance regimes are the predefined regulatory frameworks that determine allowed products, regions, personnel handling, and control expectations.
  - **Concrete production example:** Example: a regulated workload is created only in approved regions and with restricted product choices because the chosen compliance regime enforces those limits.
- **resource restrictions**
  - **Technical definition:** Resource restrictions are Assured Workloads-enforced limitations on locations, products, support access, or configuration based on regime.
  - **Concrete production example:** Example: a regulated workload is created only in approved regions and with restricted product choices because the chosen compliance regime enforces those limits.
- **control packages**
  - **Technical definition:** Control packages are the bundled technical and procedural control mappings associated with a selected compliance regime.
  - **Concrete production example:** Example: a regulated workload is created only in approved regions and with restricted product choices because the chosen compliance regime enforces those limits.
- **monitoring status**
  - **Technical definition:** Monitoring status is the service's view of whether the regulated environment remains aligned with expected restrictions.
  - **Concrete production example:** Example: a regulated workload is created only in approved regions and with restricted product choices because the chosen compliance regime enforces those limits.

### Cloud Billing and quotas

- **Cloud Billing and quotas**
  - **Technical definition:** Cloud Billing and quotas is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **billing accounts**
  - **Technical definition:** Billing accounts are the payer entities aggregating project charges and budget tracking.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **billing subaccounts**
  - **Technical definition:** Billing subaccounts are subordinate billing entities used in delegated or reseller billing structures.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **budgets**
  - **Technical definition:** Budgets are spend-threshold definitions tied to billing scopes for alerting and governance.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **alerts**
  - **Technical definition:** Alerts are notifications triggered when budget or spend conditions cross configured thresholds.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **project billing links**
  - **Technical definition:** Project billing links are the associations connecting project resource usage to a billing account.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **quotas**
  - **Technical definition:** Quotas are service-specific limits on resources, API calls, or capacity dimensions.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **quota limits**
  - **Technical definition:** Quota limits are the numeric ceilings applied to a particular quota dimension.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **quota usage**
  - **Technical definition:** Quota usage is the currently consumed portion of a quota limit and therefore a direct measure of headroom.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.

## Controls

### Expanded Points

- **Organization/folder/project hierarchy**
  - **Technical definition:** Organization/folder/project hierarchy is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **folder inheritance**
  - **Technical definition:** folder inheritance is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **project ownership**
  - **Technical definition:** project ownership is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **label/tag standards**
  - **Technical definition:** label/tag standards is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **project vending guardrails**
  - **Technical definition:** project vending guardrails is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **IAM least privilege**
  - **Technical definition:** IAM least privilege is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **conditional access**
  - **Technical definition:** conditional access is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service account key restrictions**
  - **Technical definition:** service account key restrictions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **workload identity federation**
  - **Technical definition:** workload identity federation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **break-glass access**
  - **Technical definition:** break-glass access is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **policy troubleshooting**
  - **Technical definition:** policy troubleshooting is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Organization Policy constraints**
  - **Technical definition:** Organization Policy constraints is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **dry-run validation**
  - **Technical definition:** dry-run validation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **custom constraints**
  - **Technical definition:** custom constraints is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **policy inheritance controls**
  - **Technical definition:** policy inheritance controls is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Cloud Asset Inventory feeds**
  - **Technical definition:** Cloud Asset Inventory feeds is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **IAM policy analysis**
  - **Technical definition:** IAM policy analysis is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Recommender review**
  - **Technical definition:** Recommender review is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **auditability for project/resource inventory**
  - **Technical definition:** auditability for project/resource inventory is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Shared VPC host/service project separation**
  - **Technical definition:** Shared VPC host/service project separation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **subnet sharing**
  - **Technical definition:** subnet sharing is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **network ownership**
  - **Technical definition:** network ownership is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service project onboarding**
  - **Technical definition:** service project onboarding is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **VPC Service Controls perimeters**
  - **Technical definition:** VPC Service Controls perimeters is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Access Context Manager access levels**
  - **Technical definition:** Access Context Manager access levels is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **ingress/egress rules**
  - **Technical definition:** ingress/egress rules is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **protected service boundaries**
  - **Technical definition:** protected service boundaries is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Billing account ownership**
  - **Technical definition:** Billing account ownership is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **quota baselines**
  - **Technical definition:** quota baselines is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **budget alerts**
  - **Technical definition:** budget alerts is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **account/project metadata governance**
  - **Technical definition:** account/project metadata governance is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

## Operations

### Expanded Points

- **Design an organization**
  - **Technical definition:** Design an organization is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **folder**
  - **Technical definition:** folder is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **project model for environments**
  - **Technical definition:** project model for environments is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **business units**
  - **Technical definition:** business units is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **shared services**
  - **Technical definition:** shared services is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Create a project factory flow with billing**
  - **Technical definition:** Create a project factory flow with billing is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **labels**
  - **Technical definition:** labels is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **quota baselines**
  - **Technical definition:** quota baselines is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **IAM**
  - **Technical definition:** IAM is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **audit logging**
  - **Technical definition:** audit logging is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **network attachment**
  - **Technical definition:** network attachment is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Review IAM policies**
  - **Technical definition:** Review IAM policies is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service accounts**
  - **Technical definition:** service accounts is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **workload identity federation**
  - **Technical definition:** workload identity federation is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service account key risk**
  - **Technical definition:** service account key risk is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Validate Organization Policy constraints with dry-run before enforcing them**
  - **Technical definition:** Validate Organization Policy constraints with dry-run before enforcing them is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Investigate access issues with Policy Troubleshooter and IAM Recommender**
  - **Technical definition:** Investigate access issues with Policy Troubleshooter and IAM Recommender is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Query Cloud Asset Inventory for project/resource ownership and landing-zone evidence**
  - **Technical definition:** Query Cloud Asset Inventory for project/resource ownership and landing-zone evidence is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Validate VPC Service Controls perimeter design before data-sensitive workloads are migrated**
  - **Technical definition:** Validate VPC Service Controls perimeter design before data-sensitive workloads are migrated is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Confirm landing-zone readiness by checking hierarchy**
  - **Technical definition:** Confirm landing-zone readiness by checking hierarchy is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **identity**
  - **Technical definition:** identity is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **billing**
  - **Technical definition:** billing is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **quota**
  - **Technical definition:** quota is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **audit**
  - **Technical definition:** audit is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **shared-network prerequisites**
  - **Technical definition:** shared-network prerequisites is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

# Google Cloud Week 2: Networking, Private Connectivity, And Edge Traffic

## Service And Entity Coverage

### Virtual Private Cloud (VPC)

- **Virtual Private Cloud (VPC)**
  - **Technical definition:** Google Cloud VPC is Google's globally scoped software-defined network for subnets, routes, firewalling, private service networking, and hybrid connectivity.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **VPC networks**
  - **Technical definition:** VPC networks are Google Cloud's global network containers holding subnets, routes, and firewall policy attachment points.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **subnets**
  - **Technical definition:** Subnets are regional IP ranges inside a VPC that determine workload placement and address allocation.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **routes**
  - **Technical definition:** Routes are forwarding decisions that determine next hop and priority for egress traffic from instances and subnets.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **firewall rules**
  - **Technical definition:** Firewall rules are stateful distributed packet filters evaluated in the VPC data plane using source, target, and protocol criteria.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **hierarchical firewall policies**
  - **Technical definition:** Hierarchical firewall policies are organization- or folder-level firewall rule sets inherited downward into projects and VPCs.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **network tags**
  - **Technical definition:** Network tags are instance-attached selectors used primarily for firewall rule targeting.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **service accounts as firewall targets**
  - **Technical definition:** Service accounts as firewall targets let network policy follow workload identity rather than only VM tag metadata.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **VPC Flow Logs**
  - **Technical definition:** VPC Flow Logs are sampled metadata records describing allowed or denied traffic observed at subnet scope.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **Firewall Insights**
  - **Technical definition:** Firewall Insights are analysis outputs identifying overly permissive, shadowed, or unused firewall rules.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.
- **Connectivity Tests**
  - **Technical definition:** Connectivity Tests are path-analysis simulations that model route, firewall, forwarding, and configuration blockers between endpoints.
  - **Concrete production example:** Example: a migration cutover is paused until Connectivity Tests and VPC Flow Logs confirm the app subnet can reach the database and private service endpoints without firewall denial.

### Shared VPC, VPC Network Peering, Private Service Access, and Private Service Connect

- **Shared VPC**
  - **Technical definition:** Shared VPC is Google's centralized network-ownership pattern in which a host project owns VPC resources and service projects consume subnets under delegated control boundaries.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **VPC Network Peering**
  - **Technical definition:** VPC Network Peering is Google's private route-exchange relationship between two VPC networks without a third-party transit layer.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **Private Service Access**
  - **Technical definition:** Private Service Access is Google's allocated-range private networking model for certain Google-managed services that require producer-side peering into customer VPC address space.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **and Private Service Connect**
  - **Technical definition:** and Private Service Connect is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **host projects**
  - **Technical definition:** Host projects are the Shared VPC-owning projects that centralize VPC networks, subnets, and network policy.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **service projects**
  - **Technical definition:** Service projects are attached projects whose workloads consume host-project subnets and network services.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **peering connections**
  - **Technical definition:** peering connections is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **allocated IP ranges**
  - **Technical definition:** allocated IP ranges is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **private connections**
  - **Technical definition:** private connections is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **service attachments**
  - **Technical definition:** service attachments is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **endpoints**
  - **Technical definition:** endpoints is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **forwarding rules**
  - **Technical definition:** forwarding rules is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.
- **published services**
  - **Technical definition:** published services is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a project factory flow creates an application project, links billing, applies labels, and attaches the project to approved Shared VPC subnets in one step.

### Cloud Router, Cloud NAT, Network Connectivity Center, Cloud VPN, and Cloud Interconnect

- **Cloud Router**
  - **Technical definition:** Cloud Router is Google's BGP-based route-control service for dynamic prefix exchange across VPN, Interconnect, and hybrid topologies.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **Cloud NAT**
  - **Technical definition:** Cloud NAT is Google's managed egress translation service for allowing outbound internet access from private workloads without assigning external IPs.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **Network Connectivity Center**
  - **Technical definition:** Network Connectivity Center is Google's hub-and-spoke coordination plane for hybrid, branch, and cross-network connectivity domains.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **Cloud VPN**
  - **Technical definition:** Cloud VPN is Google's IPSec-based encrypted tunnel service for internet-routed hybrid connectivity.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **and Cloud Interconnect**
  - **Technical definition:** and Cloud Interconnect is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **routers**
  - **Technical definition:** Routers are Cloud Router resources that own BGP sessions and dynamic route advertisement behavior.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **BGP sessions**
  - **Technical definition:** BGP sessions are the routing adjacencies used to exchange prefixes and path state with customer or partner devices.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **NAT gateways**
  - **Technical definition:** NAT gateways are Cloud NAT-managed egress translation constructs for outbound internet access from private workloads.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **spokes**
  - **Technical definition:** Spokes are Network Connectivity Center attachments representing connected networks, VPNs, VPCs, or interconnect resources.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **hubs**
  - **Technical definition:** Hubs are Network Connectivity Center core objects used to coordinate policy and pathing across multiple spokes.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **VPN gateways**
  - **Technical definition:** VPN gateways are Google Cloud-side tunnel termination resources for Cloud VPN.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **tunnels**
  - **Technical definition:** Tunnels are encrypted IPSec paths carrying traffic between Google Cloud and remote endpoints.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **VLAN attachments**
  - **Technical definition:** VLAN attachments are Cloud Interconnect logical circuits mapping customer VLANs into Google Cloud routing domains.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **Partner Interconnect**
  - **Technical definition:** Partner Interconnect is the provider-mediated Interconnect model delivered through approved connectivity partners.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **Dedicated Interconnect**
  - **Technical definition:** Dedicated Interconnect is the direct physical interconnect model using customer-controlled cross-connect capacity into Google's backbone.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.
- **Cloud Interconnect locations**
  - **Technical definition:** Cloud Interconnect locations are the physical facilities where customers or partners connect into Google's private network.
  - **Concrete production example:** Example: during DR rehearsal, engineers verify that BGP routes fail over from one Interconnect path to the backup tunnel without breaking on-premises connectivity.

### Cloud DNS

- **Cloud DNS**
  - **Technical definition:** Cloud DNS is Google's authoritative and hybrid DNS platform for public zones, private zones, forwarding, peering, and policy-driven name resolution.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **managed zones**
  - **Technical definition:** Managed zones are Cloud DNS administrative containers holding the records for a domain namespace.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **private zones**
  - **Technical definition:** Private zones are Cloud DNS zones visible only to authorized VPC networks for internal resolution.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **public zones**
  - **Technical definition:** Public zones are Cloud DNS zones whose records are served publicly on the internet.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **forwarding zones**
  - **Technical definition:** Forwarding zones are Cloud DNS configurations that forward matching queries to external resolvers.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **peering zones**
  - **Technical definition:** Peering zones are private DNS relationships that let one VPC resolve records from another VPC's private zone.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **DNS records**
  - **Technical definition:** DNS records are the name-to-answer mappings stored in a Cloud DNS zone.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **policies**
  - **Technical definition:** Cloud DNS policies are network-scoped DNS behavior controls such as inbound forwarding and logging.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **response policies**
  - **Technical definition:** Response policies are rule sets that override or synthesize DNS answers for selected query patterns.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.
- **DNSSEC settings**
  - **Technical definition:** DNSSEC settings determine whether and how DNSSEC signing is enabled for a public zone.
  - **Concrete production example:** Example: a blue/green cutover uses private-zone changes and short TTLs so internal services switch cleanly and can be rolled back quickly.

### Cloud Load Balancing

- **Cloud Load Balancing**
  - **Technical definition:** Cloud Load Balancing is Google's distributed L4/L7 ingress and traffic distribution platform with regional and global variants, health-aware routing, and backend policy control.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **global external Application Load Balancers**
  - **Technical definition:** Global external Application Load Balancers are globally distributed L7 HTTP(S) front doors with worldwide anycast ingress and centralized URL routing.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **regional external Application Load Balancers**
  - **Technical definition:** Regional external Application Load Balancers are region-scoped L7 HTTP(S) ingress services with regional frontend semantics.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **internal Application Load Balancers**
  - **Technical definition:** Internal Application Load Balancers are private L7 ingress services used for internal HTTP(S) exposure inside Google Cloud.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **proxy Network Load Balancers**
  - **Technical definition:** Proxy Network Load Balancers are proxy-based L4 balancers that terminate frontend connections before forwarding to backends.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **passthrough Network Load Balancers**
  - **Technical definition:** Passthrough Network Load Balancers are direct L4 forwarding balancers that preserve the backend connection model without proxy termination.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **backend services**
  - **Technical definition:** Backend services are traffic-policy objects that connect frontends to backends, health checks, balancing modes, and optional CDN or security policy.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **URL maps**
  - **Technical definition:** URL maps are L7 routing policies that choose backend behavior based on host and path matching.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **target proxies**
  - **Technical definition:** Target proxies are protocol-terminating proxy resources that bind listeners to URL maps or backend services.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **forwarding rules**
  - **Technical definition:** Forwarding rules are frontend listener definitions assigning IP, port, and protocol to a load-balancing target.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **health checks**
  - **Technical definition:** Health checks are active probes used to decide whether backends remain eligible for traffic.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **SSL certificates**
  - **Technical definition:** SSL certificates are Google-managed or self-managed TLS identity objects bound to HTTPS or TLS frontends.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.
- **Cloud CDN-backed backends**
  - **Technical definition:** Cloud CDN-backed backends are backend services with edge caching enabled so responses can be served from Google's edge.
  - **Concrete production example:** Example: a global HTTPS service is promoted only after health checks, URL maps, certificates, and backend capacity all validate under canary traffic.

### Cloud Armor, Cloud CDN, Cloud IDS, and Packet Mirroring

- **Cloud Armor**
  - **Technical definition:** Cloud Armor is Google's edge and application security policy service for WAF enforcement, rate limiting, adaptive protection, and ingress filtering.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **Cloud CDN**
  - **Technical definition:** Cloud CDN is Google's edge caching layer for reducing origin load and improving latency for cacheable content behind supported load balancers.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **Cloud IDS**
  - **Technical definition:** Cloud IDS is Google's managed network intrusion detection service that inspects mirrored traffic using signature-based threat detection.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **and Packet Mirroring**
  - **Technical definition:** and Packet Mirroring is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **security policies**
  - **Technical definition:** Security policies are Cloud Armor policy containers holding ordered match-and-action logic for requests.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **rules**
  - **Technical definition:** Rules are the individual ordered statements inside edge security policies that allow, deny, rate-limit, or label traffic.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **preconfigured WAF rules**
  - **Technical definition:** Preconfigured WAF rules are Google-managed rule sets for common application attack patterns such as SQLi and XSS.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **edge caches**
  - **Technical definition:** Edge caches are the geographically distributed Cloud CDN cache nodes serving cached responses near clients.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **cache keys**
  - **Technical definition:** Cache keys are the selected request attributes used to determine whether a request maps to an existing cached object.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **invalidations**
  - **Technical definition:** Invalidations are purge operations that remove cached content before TTL expiry.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **IDS endpoints**
  - **Technical definition:** IDS endpoints are the Cloud IDS inspection resources receiving mirrored traffic for signature analysis.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **mirrored sources**
  - **Technical definition:** Mirrored sources are the workloads or subnets whose packets are copied by Packet Mirroring.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **collectors**
  - **Technical definition:** Collectors are the destinations that receive mirrored packets for inspection or analysis.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.
- **packet mirroring policies**
  - **Technical definition:** Packet Mirroring policies define which traffic is copied and where the mirrored packets are sent.
  - **Concrete production example:** Example: a retail site replays production-like traffic against Cloud Armor rules so checkout requests are not blocked by overly aggressive WAF settings.

### API Gateway and Apigee

- **API Gateway**
  - **Technical definition:** API Gateway is Google's managed API ingress service for exposing APIs with auth, quotas, configs, and backend routing.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **Apigee**
  - **Technical definition:** Apigee is Google's full API management platform for policy mediation, environment-based promotion, analytics, developer onboarding, and partner-facing API governance.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **APIs**
  - **Technical definition:** APIs are the logical API surfaces published through API Gateway or Apigee.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **API configs**
  - **Technical definition:** API configs are the deployed configuration bundles defining auth, routes, and backend behavior for an API Gateway API.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **gateways**
  - **Technical definition:** Gateways are the serving frontends through which API Gateway exposes configured APIs.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **managed services**
  - **Technical definition:** Managed services are Service Management-backed service definitions used by API Gateway for configuration and control.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **Apigee organizations**
  - **Technical definition:** Apigee organizations are the top-level administrative containers for Apigee environments, analytics, and runtime artifacts.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **environments**
  - **Technical definition:** Environments are isolated deployment and runtime scopes inside Apigee used for dev, test, or prod separation.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **environment groups**
  - **Technical definition:** Environment groups are hostname and routing associations binding Apigee environments to external domains.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **proxies**
  - **Technical definition:** Proxies are Apigee mediation bundles defining routes, policies, and target interactions.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **deployments**
  - **Technical definition:** Deployments are the activated runtime instances of API Gateway configs or Apigee proxies.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **products**
  - **Technical definition:** Products are Apigee packages that bundle APIs, quota policy, and access plans for consumers.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **developers**
  - **Technical definition:** Developers are Apigee consumer identity records used for onboarding apps and credential issuance.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **apps**
  - **Technical definition:** Apps are Apigee consumer applications registered to receive credentials and access API products.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.
- **Apigee hybrid runtimes**
  - **Technical definition:** Apigee hybrid runtimes are Kubernetes-hosted Apigee execution components used in hybrid deployments.
  - **Concrete production example:** Example: an external partner API is promoted through Apigee environments so quota, auth policy, and backend routing are validated before full production exposure.

## Controls

### Expanded Points

- **VPC CIDR planning**
  - **Technical definition:** VPC CIDR planning is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **subnet regionality**
  - **Technical definition:** subnet regionality is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **route priority**
  - **Technical definition:** route priority is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **least-open firewall rules**
  - **Technical definition:** least-open firewall rules is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **hierarchical firewall policy inheritance**
  - **Technical definition:** hierarchical firewall policy inheritance is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **VPC Flow Logs**
  - **Technical definition:** VPC Flow Logs is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Connectivity Tests**
  - **Technical definition:** Connectivity Tests is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Shared VPC IAM boundaries**
  - **Technical definition:** Shared VPC IAM boundaries is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service project attachment**
  - **Technical definition:** service project attachment is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **VPC peering route exchange**
  - **Technical definition:** VPC peering route exchange is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Private Service Access allocated ranges**
  - **Technical definition:** Private Service Access allocated ranges is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Private Service Connect endpoints**
  - **Technical definition:** Private Service Connect endpoints is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service attachment acceptance**
  - **Technical definition:** service attachment acceptance is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Cloud Router BGP advertisements**
  - **Technical definition:** Cloud Router BGP advertisements is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud NAT port allocation**
  - **Technical definition:** Cloud NAT port allocation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Network Connectivity Center hub/spoke design**
  - **Technical definition:** Network Connectivity Center hub/spoke design is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud VPN tunnel settings**
  - **Technical definition:** Cloud VPN tunnel settings is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Interconnect VLAN attachment redundancy**
  - **Technical definition:** Interconnect VLAN attachment redundancy is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **hybrid route governance**
  - **Technical definition:** hybrid route governance is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Cloud DNS private zone visibility**
  - **Technical definition:** Cloud DNS private zone visibility is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **forwarding policies**
  - **Technical definition:** forwarding policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **peering zones**
  - **Technical definition:** peering zones is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **DNSSEC**
  - **Technical definition:** DNSSEC is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **response policies**
  - **Technical definition:** response policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **hybrid DNS boundaries**
  - **Technical definition:** hybrid DNS boundaries is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Load balancer health checks**
  - **Technical definition:** Load balancer health checks is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **backend service policies**
  - **Technical definition:** backend service policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **SSL certificates**
  - **Technical definition:** SSL certificates is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud CDN caching**
  - **Technical definition:** Cloud CDN caching is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Armor policies**
  - **Technical definition:** Cloud Armor policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **WAF rule tuning**
  - **Technical definition:** WAF rule tuning is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **API Gateway authentication**
  - **Technical definition:** API Gateway authentication is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service config rollout**
  - **Technical definition:** service config rollout is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Apigee proxy policy enforcement**
  - **Technical definition:** Apigee proxy policy enforcement is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **environment promotion**
  - **Technical definition:** environment promotion is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **traffic management**
  - **Technical definition:** traffic management is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

## Operations

### Expanded Points

- **Validate packet paths with routes**
  - **Technical definition:** Validate packet paths with routes is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **firewall rules**
  - **Technical definition:** firewall rules is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **VPC Flow Logs**
  - **Technical definition:** VPC Flow Logs is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Firewall Insights**
  - **Technical definition:** Firewall Insights is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Connectivity Tests**
  - **Technical definition:** Connectivity Tests is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Troubleshoot private access through VPC peering**
  - **Technical definition:** Troubleshoot private access through VPC peering is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Private Service Access**
  - **Technical definition:** Private Service Access is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Private Service Connect**
  - **Technical definition:** Private Service Connect is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Test Cloud VPN failover**
  - **Technical definition:** Test Cloud VPN failover is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Interconnect redundancy**
  - **Technical definition:** Cloud Interconnect redundancy is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Router BGP route exchange**
  - **Technical definition:** Cloud Router BGP route exchange is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud NAT exhaustion scenarios**
  - **Technical definition:** Cloud NAT exhaustion scenarios is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Execute DNS cutover using Cloud DNS records**
  - **Technical definition:** Execute DNS cutover using Cloud DNS records is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **forwarding zones**
  - **Technical definition:** forwarding zones is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **private zones**
  - **Technical definition:** private zones is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **rollback TTL planning**
  - **Technical definition:** rollback TTL planning is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Review load balancer health checks**
  - **Technical definition:** Review load balancer health checks is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **backend capacity**
  - **Technical definition:** backend capacity is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **URL maps**
  - **Technical definition:** URL maps is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **SSL certificates**
  - **Technical definition:** SSL certificates is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud CDN behavior**
  - **Technical definition:** Cloud CDN behavior is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Armor false positives**
  - **Technical definition:** Cloud Armor false positives is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Deploy and troubleshoot API Gateway or Apigee proxies**
  - **Technical definition:** Deploy and troubleshoot API Gateway or Apigee proxies is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **authentication**
  - **Technical definition:** authentication is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **routing**
  - **Technical definition:** routing is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **environment promotion**
  - **Technical definition:** environment promotion is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **access logs**
  - **Technical definition:** access logs is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

# Google Cloud Week 3: Compute, Google Kubernetes Engine, DevOps, And Platform Delivery

## Service And Entity Coverage

### Compute Engine and managed instance groups

- **Compute Engine and managed instance groups**
  - **Technical definition:** Compute Engine and managed instance groups is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **VM instances**
  - **Technical definition:** VM instances are running Compute Engine virtual machines with guest OS, attached disks, metadata, NICs, and IAM identity.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **machine types**
  - **Technical definition:** Machine types are the compute shape definitions specifying vCPU, memory, and hardware profile.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **disks**
  - **Technical definition:** Disks are block devices attached to instances for boot or data storage.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **images**
  - **Technical definition:** Images are reusable boot-disk templates used to launch new instances.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **machine images**
  - **Technical definition:** Machine images are broader instance-capture artifacts preserving VM configuration plus referenced disk state.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **instance templates**
  - **Technical definition:** Instance templates are immutable launch blueprints for repeatable VM provisioning and MIG rollout.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **managed instance groups (MIGs)**
  - **Technical definition:** Managed instance groups are fleet controllers that maintain, scale, and roll out groups of similar instances.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **autoscalers**
  - **Technical definition:** Autoscalers are policy objects that adjust MIG size based on metrics, load, or schedules.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **health checks**
  - **Technical definition:** Health checks are probing objects used by MIGs and load balancing to determine whether instances should remain in service.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **metadata**
  - **Technical definition:** Metadata is the per-project or per-instance key/value control surface exposed to VM workloads and startup logic.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **startup scripts**
  - **Technical definition:** Startup scripts are boot-time command payloads used to configure instances after launch.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **service accounts**
  - **Technical definition:** Service accounts are VM-attached identities used by workloads to call Google Cloud APIs without embedded secrets.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **sole-tenant nodes**
  - **Technical definition:** Sole-tenant nodes are dedicated physical hosts reserved for one customer for isolation or licensing requirements.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.
- **reservations**
  - **Technical definition:** Reservations are capacity guarantees for specified VM shapes in a zone.
  - **Concrete production example:** Example: a managed instance group rolls to a new template, and health-based replacement ensures bad instances are removed before traffic is expanded.

### Google Kubernetes Engine (GKE)

- **Google Kubernetes Engine (GKE)**
  - **Technical definition:** Google Kubernetes Engine is Google's managed Kubernetes platform with Google-managed control plane, integrated networking, policy hooks, and workload lifecycle features.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **Standard clusters**
  - **Technical definition:** Standard clusters are GKE clusters where Google manages the control plane while customers retain direct node-pool responsibility and flexibility.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **Autopilot clusters**
  - **Technical definition:** Autopilot clusters are GKE clusters where Google manages much of node provisioning and cluster operations while users express pod-level intent.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **node pools**
  - **Technical definition:** Node pools are grouped worker nodes with shared configuration inside a GKE cluster.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **nodes**
  - **Technical definition:** Nodes are the worker VMs that run Kubernetes pods and cluster agents.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **releases**
  - **Technical definition:** Releases are the Kubernetes version tracks and patch sets available to a GKE cluster.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **control planes**
  - **Technical definition:** Control planes are the Google-managed Kubernetes API and orchestration components for a cluster.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **workloads**
  - **Technical definition:** Workloads are the Kubernetes application controllers and pods running inside a cluster.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **services**
  - **Technical definition:** Services are Kubernetes networking abstractions exposing pods behind stable virtual IPs and discovery names.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **Gateway API resources**
  - **Technical definition:** Gateway API resources are Kubernetes networking objects defining L4/L7 traffic policy using the Gateway API model.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **Ingress**
  - **Technical definition:** Ingress is the Kubernetes HTTP(S) routing abstraction for exposing services through a controller-managed load balancer.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **Config Sync**
  - **Technical definition:** Config Sync is Google's Git-based Kubernetes configuration reconciliation service.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **Workload Identity**
  - **Technical definition:** Workload Identity is GKE's identity mapping mechanism that binds Kubernetes identities to Google Cloud IAM principals without static keys.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **Kubernetes service accounts**
  - **Technical definition:** Kubernetes service accounts are cluster-local identities assigned to pods for in-cluster or federated auth decisions.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.
- **GKE audit logs**
  - **Technical definition:** GKE audit logs are Cloud Audit Logs entries recording administrative and access events for GKE resources.
  - **Concrete production example:** Example: a GKE upgrade proceeds only after node-pool rollout, Workload Identity access, and workload health checks all pass in the target cluster.

### Cloud Run and Cloud Run functions

- **Cloud Run**
  - **Technical definition:** Cloud Run is Google's serverless container runtime for request-driven services and jobs with revision-based rollout and managed autoscaling.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **Cloud Run functions**
  - **Technical definition:** Cloud Run functions is Google's code-first function model running on Cloud Run's serverless execution substrate.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **services**
  - **Technical definition:** Services are Cloud Run's request-serving deployments exposed via URLs and composed of immutable revisions.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **revisions**
  - **Technical definition:** Revisions are immutable snapshots of service code and configuration used for rollout and rollback.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **jobs**
  - **Technical definition:** Jobs are Cloud Run's finite execution workloads designed for non-request-driven task processing.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **containers**
  - **Technical definition:** Containers are the OCI images executed by Cloud Run for services or jobs.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **triggers**
  - **Technical definition:** Triggers are event-routing attachments, often via Eventarc, that invoke Cloud Run workloads.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **functions**
  - **Technical definition:** Functions are code-first serverless handlers deployed onto Cloud Run's execution model.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **environment variables**
  - **Technical definition:** Environment variables are runtime configuration key/value pairs injected into Cloud Run instances.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **traffic splits**
  - **Technical definition:** Traffic splits are per-revision routing percentages controlling how requests are distributed across revisions.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **domains**
  - **Technical definition:** Domains are custom hostname mappings attached to Cloud Run services.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **service accounts**
  - **Technical definition:** Service accounts are the runtime identities used by Cloud Run services or jobs for API access.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **concurrency**
  - **Technical definition:** Concurrency is the number of simultaneous requests a single Cloud Run instance may handle.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.
- **min/max instances**
  - **Technical definition:** Min/max instances are scaling bounds that define Cloud Run's pre-warmed floor and upper scaling limit.
  - **Concrete production example:** Example: a Cloud Run service sends 10% of traffic to a new revision to validate latency and auth behavior before full promotion.

### App Engine and Batch

- **App Engine and Batch**
  - **Technical definition:** App Engine and Batch is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **applications**
  - **Technical definition:** Applications are the top-level App Engine application containers anchoring region and service grouping.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **services**
  - **Technical definition:** Services are independently deployable App Engine components with separate versions and traffic controls.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **versions**
  - **Technical definition:** Versions are the deployable releases of an App Engine service.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **instances**
  - **Technical definition:** Instances are the runtime processes or VMs serving an App Engine version or executing Batch work.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **queues**
  - **Technical definition:** Queues are buffering constructs used to stage asynchronous work for later processing.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **jobs**
  - **Technical definition:** Jobs are finite Batch execution units representing one submitted workload.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **tasks**
  - **Technical definition:** Tasks are the discrete work items inside a batch job or queue-driven flow.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **allocation policies**
  - **Technical definition:** Allocation policies are Batch controls determining how compute is selected and provisioned for a job.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **instance policies**
  - **Technical definition:** Instance policies are Batch compute selection rules constraining machine profile and runtime behavior.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.
- **logs**
  - **Technical definition:** Logs are the execution and operational records emitted by App Engine or Batch workloads.
  - **Concrete production example:** Example: a batch pipeline validates queue depth, task retries, and allocation policy before being approved for a peak-volume processing window.

### Artifact Registry and Binary Authorization

- **Artifact Registry**
  - **Technical definition:** Artifact Registry is Google's managed repository for storing container images, packages, and supply-chain metadata under IAM and retention control.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **Binary Authorization**
  - **Technical definition:** Binary Authorization is Google's deploy-time policy gate for enforcing image trust based on attestations and admission policy.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **repositories**
  - **Technical definition:** Repositories are Artifact Registry namespaces storing related images or packages under one policy boundary.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **packages**
  - **Technical definition:** Packages are versioned language or artifact groupings stored in Artifact Registry.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **images**
  - **Technical definition:** Images are container artifacts stored in Artifact Registry and referenced by tag or digest.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **tags**
  - **Technical definition:** Tags are mutable references pointing to a specific artifact version or image manifest.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **versions**
  - **Technical definition:** Versions are immutable released instances of packages or stored artifacts.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **attachments**
  - **Technical definition:** Attachments are metadata objects associated with an artifact for additional supply-chain context.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **cleanup policies**
  - **Technical definition:** Cleanup policies are Artifact Registry retention rules used to expire old or unneeded artifacts.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **vulnerability scanning**
  - **Technical definition:** Vulnerability scanning is the artifact-analysis capability that identifies known dependency or image vulnerabilities.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **attestations**
  - **Technical definition:** Attestations are signed statements used by Binary Authorization to prove build, test, or approval conditions.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **attestors**
  - **Technical definition:** Attestors are the trusted entities or key sets that sign and verify attestations.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **policies**
  - **Technical definition:** Policies are Binary Authorization admission rules defining which images may deploy under what trust conditions.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.
- **platform policies**
  - **Technical definition:** Platform policies are organization-scale Binary Authorization standards applied consistently across environments.
  - **Concrete production example:** Example: production admission rejects an image until required attestations and vulnerability review are present in the trusted path.

### Cloud Build and Cloud Deploy

- **Cloud Build**
  - **Technical definition:** Cloud Build is Google's managed build-execution service for compiling, testing, packaging, and producing artifacts in isolated build steps.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **Cloud Deploy**
  - **Technical definition:** Cloud Deploy is Google's progressive delivery control plane for releasing versioned artifacts across ordered deployment targets.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **build triggers**
  - **Technical definition:** Build triggers are event-driven rules that start Cloud Build executions from source changes or external events.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **build configs**
  - **Technical definition:** Build configs are declarative build specifications describing ordered steps, artifacts, substitutions, and execution behavior.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **steps**
  - **Technical definition:** Steps are the containerized actions executed in sequence during a Cloud Build run.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **artifacts**
  - **Technical definition:** Artifacts are the build outputs such as images, packages, binaries, or metadata.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **worker pools**
  - **Technical definition:** Worker pools are the dedicated or private execution environments where Cloud Build runs.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **connections**
  - **Technical definition:** Connections are repository or source integrations used by Cloud Build and Cloud Deploy.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **repositories**
  - **Technical definition:** Repositories are the source-control locations or connected source resources used by CI/CD workflows.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **delivery pipelines**
  - **Technical definition:** Delivery pipelines are Cloud Deploy's top-level progression graphs across deployment targets.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **targets**
  - **Technical definition:** Targets are Cloud Deploy destinations such as clusters or environments.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **releases**
  - **Technical definition:** Releases are versioned deployment candidates promoted through a delivery pipeline.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **rollouts**
  - **Technical definition:** Rollouts are the actual target-specific deployment executions for a release.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.
- **Skaffold configs**
  - **Technical definition:** Skaffold configs are build-and-deploy definitions Cloud Deploy uses to render and deploy artifacts.
  - **Concrete production example:** Example: a Cloud Deploy rollout is paused after canary errors spike, preserving the previous release for fast rollback.

### Infrastructure Manager and Terraform on Google Cloud

- **Infrastructure Manager and Terraform on Google Cloud**
  - **Technical definition:** Infrastructure Manager and Terraform on Google Cloud is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.
- **deployments**
  - **Technical definition:** Deployments are Infrastructure Manager-managed Terraform execution objects and infrastructure lifecycle records.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.
- **revisions**
  - **Technical definition:** Revisions are the versioned infrastructure definition states tracked across deployment changes.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.
- **Terraform configurations**
  - **Technical definition:** Terraform configurations are declarative IaC files describing desired Google Cloud resources and relationships.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.
- **state**
  - **Technical definition:** State is Terraform's record of managed resources and known attributes used for reconciliation.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.
- **service accounts**
  - **Technical definition:** Service accounts are the identities under which Terraform or Infrastructure Manager executes changes.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.
- **input variables**
  - **Technical definition:** Input variables are parameter values passed into Terraform modules or root configurations.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.
- **outputs**
  - **Technical definition:** Outputs are the exported values emitted after Terraform apply completes.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.
- **execution plans**
  - **Technical definition:** Execution plans are previews of the infrastructure changes Terraform intends to perform.
  - **Concrete production example:** Example: Terraform plan review catches an unexpected subnet recreation before apply reaches production.

### Cloud Scheduler, Eventarc, and Workflows

- **Cloud Scheduler**
  - **Technical definition:** Cloud Scheduler is Google's managed cron service for invoking HTTP, Pub/Sub, and other target types on schedules.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **Eventarc**
  - **Technical definition:** Eventarc is Google's event routing layer for delivering cloud and custom events to target services with filtering semantics.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **and Workflows**
  - **Technical definition:** and Workflows is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **jobs**
  - **Technical definition:** Jobs are Cloud Scheduler scheduled invocation definitions.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **schedules**
  - **Technical definition:** Schedules are cron-like timing expressions controlling when a scheduled job runs.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **targets**
  - **Technical definition:** Targets are the downstream endpoints or services invoked by Scheduler, Eventarc, or Workflows.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **triggers**
  - **Technical definition:** Triggers are Eventarc routing definitions subscribing to matching event streams and delivering them to a target.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **channels**
  - **Technical definition:** Channels are Eventarc abstractions representing event transport or partner event source connectivity.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **providers**
  - **Technical definition:** Providers are the event producers supplying events into Eventarc routing.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **workflows**
  - **Technical definition:** Workflows are stateful, step-based orchestration definitions executed by Google Cloud Workflows.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **executions**
  - **Technical definition:** Executions are the runtime instances of a workflow invocation.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **steps**
  - **Technical definition:** Steps are the individual operations or control-flow units inside a workflow.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **connectors**
  - **Technical definition:** Connectors are built-in integrations simplifying calls from Workflows to Google or external APIs.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.
- **callbacks**
  - **Technical definition:** Callbacks are Workflows wait-and-resume endpoints enabling asynchronous continuation from external systems.
  - **Concrete production example:** Example: an Eventarc-triggered workflow pauses on a callback until an external approval system resumes the execution.

### Vertex AI and Gemini API

- **Vertex AI and Gemini API**
  - **Technical definition:** Vertex AI and Gemini API is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **Workbench instances**
  - **Technical definition:** Workbench instances are managed notebook environments for interactive ML development in Vertex AI.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **training jobs**
  - **Technical definition:** Training jobs are managed runs executing model-training code against supplied datasets and parameters.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **custom jobs**
  - **Technical definition:** Custom jobs are generic managed compute jobs for arbitrary containerized ML work.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **models**
  - **Technical definition:** Models are registered ML artifacts containing serving metadata and deployable model state.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **endpoints**
  - **Technical definition:** Endpoints are online serving resources that host deployed models for low-latency inference.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **batch predictions**
  - **Technical definition:** Batch predictions are offline inference jobs that apply a model to a dataset and write results to storage.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **pipelines**
  - **Technical definition:** Pipelines are Vertex AI workflow graphs linking preprocessing, training, evaluation, and registration steps.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **Model Registry**
  - **Technical definition:** Model Registry is Vertex AI's versioned tracking system for registered models and model lineage.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **Model Garden**
  - **Technical definition:** Model Garden is Google's curated catalog of first-party and partner models available through Vertex AI.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **feature stores**
  - **Technical definition:** Feature stores are managed repositories for serving consistent ML features to training and inference.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **datasets**
  - **Technical definition:** Datasets are managed data resources used for training, evaluation, or annotation workflows.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **metadata**
  - **Technical definition:** Metadata is the lineage and artifact context recorded across Vertex AI runs and pipelines.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **Gemini models**
  - **Technical definition:** Gemini models are Google's generative foundation models exposed for prompt-driven inference.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **prompts**
  - **Technical definition:** Prompts are the structured natural-language or multimodal inputs sent to Gemini models.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **safety settings**
  - **Technical definition:** Safety settings are inference controls governing content filtering and harm-threshold behavior.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **evaluation jobs**
  - **Technical definition:** Evaluation jobs are managed scoring workflows that compare model or prompt behavior against datasets and metrics.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.

### Cloud TPU and BigQuery ML

- **Cloud TPU**
  - **Technical definition:** Cloud TPU is Google's accelerator platform for tensor-processing-unit-backed ML training and inference.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.
- **BigQuery ML**
  - **Technical definition:** BigQuery ML is BigQuery's in-warehouse ML capability for building and serving models directly with SQL.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.
- **TPU nodes**
  - **Technical definition:** TPU nodes are allocatable Cloud TPU resources providing accelerator capacity for training or inference.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.
- **queued resources**
  - **Technical definition:** Queued resources are TPU reservation-like capacity requests used to secure accelerator availability.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.
- **accelerator types**
  - **Technical definition:** Accelerator types are the TPU hardware generation and shape choices defining performance and memory profile.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.
- **BigQuery ML models**
  - **Technical definition:** BigQuery ML models are ML artifacts trained and stored directly inside BigQuery.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.
- **training data**
  - **Technical definition:** Training data is the dataset selected to fit an ML model.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.
- **evaluation metrics**
  - **Technical definition:** Evaluation metrics are the quantitative measures used to score model quality after training or testing.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.
- **predictions**
  - **Technical definition:** Predictions are the inferred outputs emitted by a trained model over input data.
  - **Concrete production example:** Example: a TPU capacity queue is secured before a training window so the team avoids missing a time-sensitive model retraining run.

## Controls

### Expanded Points

- **Compute Engine service accounts**
  - **Technical definition:** Compute Engine service accounts is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **OS Login**
  - **Technical definition:** OS Login is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Shielded VM**
  - **Technical definition:** Shielded VM is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **metadata access**
  - **Technical definition:** metadata access is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **image baselines**
  - **Technical definition:** image baselines is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **instance template versioning**
  - **Technical definition:** instance template versioning is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **MIG autoscaling**
  - **Technical definition:** MIG autoscaling is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **health checks**
  - **Technical definition:** health checks is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **GKE release channels**
  - **Technical definition:** GKE release channels is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **authorized networks**
  - **Technical definition:** authorized networks is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **private clusters**
  - **Technical definition:** private clusters is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Workload Identity**
  - **Technical definition:** Workload Identity is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Binary Authorization**
  - **Technical definition:** Binary Authorization is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **network policy**
  - **Technical definition:** network policy is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Pod Security Admission**
  - **Technical definition:** Pod Security Admission is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Config Sync**
  - **Technical definition:** Config Sync is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **maintenance windows**
  - **Technical definition:** maintenance windows is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Cloud Run ingress**
  - **Technical definition:** Cloud Run ingress is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **authentication**
  - **Technical definition:** authentication is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service accounts**
  - **Technical definition:** service accounts is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **concurrency**
  - **Technical definition:** concurrency is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **CPU allocation**
  - **Technical definition:** CPU allocation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **min/max instances**
  - **Technical definition:** min/max instances is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **VPC egress**
  - **Technical definition:** VPC egress is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **traffic splitting**
  - **Technical definition:** traffic splitting is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Artifact Registry IAM**
  - **Technical definition:** Artifact Registry IAM is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **cleanup policies**
  - **Technical definition:** cleanup policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **vulnerability scanning**
  - **Technical definition:** vulnerability scanning is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Binary Authorization attestations**
  - **Technical definition:** Binary Authorization attestations is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **build provenance**
  - **Technical definition:** build provenance is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **signed image promotion**
  - **Technical definition:** signed image promotion is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Cloud Build private pools**
  - **Technical definition:** Cloud Build private pools is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **repository connections**
  - **Technical definition:** repository connections is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **substitutions**
  - **Technical definition:** substitutions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service accounts**
  - **Technical definition:** service accounts is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **secret usage**
  - **Technical definition:** secret usage is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **approval gates**
  - **Technical definition:** approval gates is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Deploy target promotion**
  - **Technical definition:** Cloud Deploy target promotion is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Infrastructure Manager deployment service accounts**
  - **Technical definition:** Infrastructure Manager deployment service accounts is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Terraform state handling**
  - **Technical definition:** Terraform state handling is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **plan review**
  - **Technical definition:** plan review is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **policy validation**
  - **Technical definition:** policy validation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **rollback approach**
  - **Technical definition:** rollback approach is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Vertex AI execution roles**
  - **Technical definition:** Vertex AI execution roles is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **VPC Service Controls**
  - **Technical definition:** VPC Service Controls is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **private service access**
  - **Technical definition:** private service access is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **model approval**
  - **Technical definition:** model approval is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **endpoint deployment controls**
  - **Technical definition:** endpoint deployment controls is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **data access boundaries**
  - **Technical definition:** data access boundaries is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

## Operations

### Expanded Points

- **Build golden VM images**
  - **Technical definition:** Build golden VM images is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **roll out managed instance group updates**
  - **Technical definition:** roll out managed instance group updates is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **test autoscaling**
  - **Technical definition:** test autoscaling is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **roll back instance templates**
  - **Technical definition:** roll back instance templates is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Upgrade GKE control planes/node pools**
  - **Technical definition:** Upgrade GKE control planes/node pools is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **review workload rollout health**
  - **Technical definition:** review workload rollout health is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **troubleshoot image pulls**
  - **Technical definition:** troubleshoot image pulls is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validate Workload Identity**
  - **Technical definition:** validate Workload Identity is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Deploy Cloud Run services/functions**
  - **Technical definition:** Deploy Cloud Run services/functions is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **shift traffic between revisions**
  - **Technical definition:** shift traffic between revisions is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **tune concurrency**
  - **Technical definition:** tune concurrency is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **troubleshoot cold starts or failed revisions**
  - **Technical definition:** troubleshoot cold starts or failed revisions is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Promote container images through Artifact Registry with vulnerability scan review and Binary Authorization enforcement**
  - **Technical definition:** Promote container images through Artifact Registry with vulnerability scan review and Binary Authorization enforcement is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Run Cloud Build pipelines**
  - **Technical definition:** Run Cloud Build pipelines is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **promote releases with Cloud Deploy**
  - **Technical definition:** promote releases with Cloud Deploy is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validate deployment targets**
  - **Technical definition:** validate deployment targets is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **roll back failed rollouts**
  - **Technical definition:** roll back failed rollouts is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Review Terraform plans with Infrastructure Manager and reconcile drift from infrastructure as code**
  - **Technical definition:** Review Terraform plans with Infrastructure Manager and reconcile drift from infrastructure as code is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Run Vertex AI training**
  - **Technical definition:** Run Vertex AI training is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **model registration**
  - **Technical definition:** model registration is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **endpoint deployment**
  - **Technical definition:** endpoint deployment is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **pipeline execution**
  - **Technical definition:** pipeline execution is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **inference troubleshooting**
  - **Technical definition:** inference troubleshooting is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

# Google Cloud Week 4: Data, Storage, Backup, And Stateful Reliability

## Service And Entity Coverage

### Cloud Storage

- **Cloud Storage**
  - **Technical definition:** Cloud Storage is Google's durable object storage platform for buckets, object generations, lifecycle control, retention, and event integration.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **buckets**
  - **Technical definition:** Buckets are Cloud Storage's top-level namespace and policy containers.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **objects**
  - **Technical definition:** Objects are the stored data items in Cloud Storage, addressed by bucket and object name.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **generations**
  - **Technical definition:** Generations are immutable version identifiers that change whenever an object is replaced.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **storage classes**
  - **Technical definition:** Storage classes are Cloud Storage cost and retrieval profiles optimized for different access patterns.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **object versioning**
  - **Technical definition:** Object versioning preserves prior noncurrent object generations after overwrite or deletion.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **retention policies**
  - **Technical definition:** Retention policies are bucket-level immutability timers preventing object deletion or overwrite before the timer expires.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **lifecycle rules**
  - **Technical definition:** Lifecycle rules automate transition or deletion based on object age and other conditions.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **soft delete**
  - **Technical definition:** Soft delete is a recovery-oriented deletion behavior retaining deleted objects for a reversible window.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **bucket lock**
  - **Technical definition:** Bucket lock is the irreversible action that makes an active retention policy immutable.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **hierarchical namespace**
  - **Technical definition:** Hierarchical namespace is Cloud Storage's folder-aware namespace mode for directory-like semantics and finer-grained administrative structure.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.
- **managed folders**
  - **Technical definition:** Managed folders are named access and administration boundaries within hierarchical-namespace buckets.
  - **Concrete production example:** Example: an accidentally deleted object is restored from an earlier generation because versioning and retention were enabled before the incident.

### Persistent Disk, Hyperdisk, and Filestore

- **Persistent Disk**
  - **Technical definition:** Persistent Disk is Google's block storage service for Compute Engine with zonal and regional durability options.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **Hyperdisk**
  - **Technical definition:** Hyperdisk is Google's configurable-performance block storage family for high-throughput and high-IOPS workloads.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **and Filestore**
  - **Technical definition:** and Filestore is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **persistent disks**
  - **Technical definition:** Persistent disks are durable block volumes attachable to Compute Engine instances.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **snapshots**
  - **Technical definition:** Snapshots are point-in-time copies of disk state used for restore and cloning.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **images**
  - **Technical definition:** Images are VM boot templates derived from disks or snapshots.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **regional disks**
  - **Technical definition:** Regional disks are synchronously replicated persistent disks spanning two zones in a region.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **Hyperdisk volumes**
  - **Technical definition:** Hyperdisk volumes are configurable-performance block devices in the Hyperdisk family.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **Filestore instances**
  - **Technical definition:** Filestore instances are managed NFS file-system deployments.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **shares**
  - **Technical definition:** Shares are exported file-system paths or logical capacity domains provided to clients.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **backups**
  - **Technical definition:** Backups are retained copies of storage state for recovery or migration.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.
- **replication**
  - **Technical definition:** Replication is the copying of storage state across service-managed failure domains.
  - **Concrete production example:** Example: a zonal failure is mitigated by restoring from snapshot or promoting a regional-disk-backed workload in the alternate zone.

### Cloud SQL and AlloyDB for PostgreSQL

- **Cloud SQL and AlloyDB for PostgreSQL**
  - **Technical definition:** Cloud SQL and AlloyDB for PostgreSQL is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **instances**
  - **Technical definition:** Instances are the top-level Cloud SQL or AlloyDB runtime resources hosting database compute and administrative settings.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **databases**
  - **Technical definition:** Databases are the logical data containers inside a managed relational runtime.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **users**
  - **Technical definition:** Users are the database authentication identities permitted to connect to a managed relational engine.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **flags**
  - **Technical definition:** Flags are engine configuration parameters tuning runtime behavior, performance, or compatibility.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **backups**
  - **Technical definition:** Backups are retained database copies and log material used for restore workflows.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **point-in-time recovery (PITR)**
  - **Technical definition:** Point-in-time recovery is the ability to restore a database to an exact earlier timestamp using backups plus transaction logs.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **read replicas**
  - **Technical definition:** Read replicas are asynchronously or service-specifically replicated copies used for read scale or DR.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **high availability**
  - **Technical definition:** High availability is the managed failover topology that keeps a standby or redundant serving path ready for promotion.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **AlloyDB clusters**
  - **Technical definition:** AlloyDB clusters are the top-level AlloyDB groupings combining storage and one or more compute instances.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **primary instances**
  - **Technical definition:** Primary instances are the writable AlloyDB compute nodes currently owning the cluster's write role.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **read pool instances**
  - **Technical definition:** Read pool instances are AlloyDB read-scale compute nodes serving replica traffic.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.
- **automated backups**
  - **Technical definition:** Automated backups are service-generated recurring backups governed by schedule and retention settings.
  - **Concrete production example:** Example: a database maintenance change is approved only after PITR restore testing and replica promotion succeed in rehearsal.

### Spanner, Bigtable, and Firestore

- **Spanner**
  - **Technical definition:** Spanner is Google's horizontally scalable relational database with synchronous replication, transaction semantics, and strong consistency capabilities across regions.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **Bigtable**
  - **Technical definition:** Bigtable is Google's wide-column NoSQL database optimized for low-latency, high-throughput workloads with key-range-based distribution.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **and Firestore**
  - **Technical definition:** and Firestore is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **Spanner instances**
  - **Technical definition:** Spanner instances are the provisioned compute and replication-capacity containers for one or more Spanner databases.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **databases**
  - **Technical definition:** Databases are the logical data stores hosted inside a distributed database instance.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **tables**
  - **Technical definition:** Tables are the primary structured storage objects holding schema-defined rows.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **splits**
  - **Technical definition:** Splits are Spanner key-range partitions used internally for horizontal scaling and load distribution.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **backups**
  - **Technical definition:** Backups are consistent retained copies of distributed database state.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **change streams**
  - **Technical definition:** Change streams are ordered feeds of data modifications emitted for downstream processing.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **Bigtable instances**
  - **Technical definition:** Bigtable instances are the top-level capacity containers for Bigtable clusters and tables.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **clusters**
  - **Technical definition:** Clusters are grouped serving and storage nodes providing locality, throughput, and replication domains.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **tables**
  - **Technical definition:** Tables are the primary structured storage objects holding schema-defined rows.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **column families**
  - **Technical definition:** Column families are Bigtable schema groups defining storage and garbage-collection policy for related columns.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **app profiles**
  - **Technical definition:** App profiles are Bigtable routing and performance policy objects controlling how clients use clusters.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **Firestore databases**
  - **Technical definition:** Firestore databases are the top-level document-store containers holding collections, documents, and indexes.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **collections**
  - **Technical definition:** Collections are Firestore groupings of related documents under one path segment.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **documents**
  - **Technical definition:** Documents are Firestore's primary JSON-like data records identified by path and fields.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **indexes**
  - **Technical definition:** Indexes are query-support structures enabling efficient retrieval for specific filter and ordering patterns.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.
- **TTL policies**
  - **Technical definition:** TTL policies are automatic expiration rules deleting documents or rows after a time threshold is crossed.
  - **Concrete production example:** Example: an SRE investigates Bigtable latency by checking whether hot-key behavior is caused by uneven row-key distribution and app-profile routing.

### Memorystore

- **Memorystore**
  - **Technical definition:** Memorystore is Google's managed in-memory service for Redis- or Memcached-compatible caching workloads.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.
- **Memorystore for Redis instances**
  - **Technical definition:** Memorystore for Redis instances are managed Redis-compatible runtime deployments.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.
- **Memorystore for Memcached instances**
  - **Technical definition:** Memorystore for Memcached instances are managed Memcached-compatible cache deployments.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.
- **clusters**
  - **Technical definition:** Clusters are grouped cache-serving topologies providing scale and availability.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.
- **nodes**
  - **Technical definition:** Nodes are the individual serving units contributing memory and processing capacity.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.
- **replicas**
  - **Technical definition:** Replicas are secondary nodes receiving copied state for read scale or failover readiness.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.
- **endpoints**
  - **Technical definition:** Endpoints are the client connection addresses used to reach the managed cache.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.
- **persistence settings**
  - **Technical definition:** Persistence settings define whether and how in-memory data is durably captured to backups or disk.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.
- **maintenance windows**
  - **Technical definition:** Maintenance windows are scheduled periods during which disruptive service maintenance may occur.
  - **Concrete production example:** Example: a failover test verifies that clients reconnect to the new endpoint path without losing critical cache-backed session behavior.

### Pub/Sub

- **Pub/Sub**
  - **Technical definition:** Pub/Sub is Google's managed asynchronous messaging platform with topic-subscription delivery, filtering, replay, and retention semantics.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **topics**
  - **Technical definition:** Topics are the named publish endpoints receiving messages from producers.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **subscriptions**
  - **Technical definition:** Subscriptions are the delivery configurations that consume messages from a topic.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **messages**
  - **Technical definition:** Messages are the payload plus attribute and delivery metadata units transmitted through Pub/Sub.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **schemas**
  - **Technical definition:** Schemas are validation definitions enforcing expected message structure.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **snapshots**
  - **Technical definition:** Snapshots are point-in-time captures of a subscription's acknowledgment state for replay.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **dead-letter topics**
  - **Technical definition:** Dead-letter topics are fallback destinations for messages that exceed retry thresholds.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **retry policies**
  - **Technical definition:** Retry policies are subscription settings controlling backoff behavior for redelivery.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **filters**
  - **Technical definition:** Filters are subscription expressions deciding which topic messages are delivered to that subscription.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **ordering keys**
  - **Technical definition:** Ordering keys are publisher-supplied keys requesting in-order delivery for related message streams.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.
- **seek operations**
  - **Technical definition:** Seek operations reset subscription delivery to a chosen timestamp or snapshot.
  - **Concrete production example:** Example: a subscriber bug creates backlog and retries, but dead-letter topics keep poison messages from blocking healthy traffic forever.

### BigQuery

- **BigQuery**
  - **Technical definition:** BigQuery is Google's serverless analytical data warehouse for large-scale SQL processing, storage, reservations, and in-database ML.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **datasets**
  - **Technical definition:** Datasets are the top-level administrative containers for BigQuery tables, views, routines, and access controls.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **tables**
  - **Technical definition:** Tables are the main structured storage objects holding analytical data in BigQuery.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **views**
  - **Technical definition:** Views are SQL-defined logical abstractions over underlying BigQuery data.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **materialized views**
  - **Technical definition:** Materialized views are precomputed query results maintained for faster repeated access.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **partitions**
  - **Technical definition:** Partitions are segmented slices of a table used to reduce scan scope and cost.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **clusters**
  - **Technical definition:** Clusters are BigQuery's physical table-ordering mechanism for pruning and query efficiency.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **jobs**
  - **Technical definition:** Jobs are the execution records for query, load, extract, or copy operations.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **reservations**
  - **Technical definition:** Reservations are purchased or assigned slot-capacity pools used for workload isolation and performance control.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **slots**
  - **Technical definition:** Slots are BigQuery's units of query execution capacity.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **routines**
  - **Technical definition:** Routines are reusable functions or procedures stored in BigQuery.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **connections**
  - **Technical definition:** Connections are definitions BigQuery uses to reach external systems or federated data sources.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.
- **BigQuery ML models**
  - **Technical definition:** BigQuery ML models are models trained, stored, and queried directly from BigQuery SQL.
  - **Concrete production example:** Example: partitioning and reservations are tuned so a daily analytics job stays within slot targets and avoids scanning unnecessary historical data.

### Dataflow, Dataproc, and Cloud Composer

- **Dataflow**
  - **Technical definition:** Dataflow is Google's managed Apache Beam execution service for batch and streaming pipelines with worker autoscaling and service-managed runtime behavior.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **Dataproc**
  - **Technical definition:** Dataproc is Google's managed Spark and Hadoop platform for cluster-based big-data processing.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **and Cloud Composer**
  - **Technical definition:** and Cloud Composer is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **Dataflow jobs**
  - **Technical definition:** Dataflow jobs are the runtime executions of Beam-defined batch or streaming pipelines.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **templates**
  - **Technical definition:** Templates are parameterized reusable pipeline packages for launching Dataflow jobs.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **workers**
  - **Technical definition:** Workers are the compute instances executing pipeline stages and parallel transformations.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **pipelines**
  - **Technical definition:** Pipelines are DAG-like data processing graphs describing transforms and data movement.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **Dataproc clusters**
  - **Technical definition:** Dataproc clusters are managed Spark/Hadoop compute clusters.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **jobs**
  - **Technical definition:** Jobs are execution submissions running code on Dataproc or data-processing backends.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **workflows**
  - **Technical definition:** Workflows are Dataproc orchestrated sequences of one or more dependent jobs.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **Cloud Composer environments**
  - **Technical definition:** Cloud Composer environments are managed Airflow deployments containing schedulers, workers, and runtime configuration.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **DAGs**
  - **Technical definition:** DAGs are Airflow workflow definitions expressing task dependencies.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.
- **schedulers**
  - **Technical definition:** Schedulers are the Airflow components deciding when tasks should run and queue.
  - **Concrete production example:** Example: a streaming Dataflow job is debugged by checking worker behavior, backlog, and template parameters after throughput drops.

### Dataplex, Data Catalog, Cloud Data Fusion, and Sensitive Data Protection

- **Dataplex**
  - **Technical definition:** Dataplex is Google's governed data-lake management layer for lakes, zones, assets, lineage, and policy-driven data operations.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **Data Catalog**
  - **Technical definition:** Data Catalog is Google's metadata discovery and tagging system for searchable data-asset understanding and governance.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **Cloud Data Fusion**
  - **Technical definition:** Cloud Data Fusion is Google's managed visual data-integration platform for ETL-style pipeline building and execution.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **and Sensitive Data Protection**
  - **Technical definition:** and Sensitive Data Protection is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **lakes**
  - **Technical definition:** Lakes are Dataplex top-level governance containers grouping zones and assets.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **zones**
  - **Technical definition:** Zones are Dataplex subdivisions separating raw, curated, or otherwise differently governed data areas.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **assets**
  - **Technical definition:** Assets are the attached data sources or storage resources governed in a Dataplex context.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **entries**
  - **Technical definition:** Entries are Data Catalog metadata records representing discoverable data resources.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **tags**
  - **Technical definition:** Tags are Data Catalog metadata annotations used to classify and govern data assets.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **lineage**
  - **Technical definition:** Lineage is the captured graph showing how data moves between sources, transformations, and outputs.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **pipelines**
  - **Technical definition:** Pipelines are data integration flows transforming and moving data between systems.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **Wrangler transformations**
  - **Technical definition:** Wrangler transformations are Data Fusion visual data-cleaning steps authored in the Wrangler interface.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **inspection templates**
  - **Technical definition:** Inspection templates are Sensitive Data Protection definitions describing what sensitive patterns to detect and how to inspect.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **de-identification templates**
  - **Technical definition:** De-identification templates are Sensitive Data Protection rule sets for masking, tokenizing, or transforming sensitive fields.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.
- **findings**
  - **Technical definition:** Findings are the sensitive-data discovery results emitted by inspection scans or jobs.
  - **Concrete production example:** Example: a sensitive-data scan classifies customer PII, and policy tags plus Dataplex governance ensure only approved analysts can query it.

### Document AI, Vision AI, Speech-to-Text, and Cloud Translation

- **Document AI**
  - **Technical definition:** Document AI is Google's document extraction platform for converting semi-structured documents into structured fields using managed processors.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **Vision AI**
  - **Technical definition:** Vision AI is Google's managed computer-vision platform for image and video analysis using trained models or processors.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **Speech-to-Text**
  - **Technical definition:** Speech-to-Text is Google's managed speech recognition service for transcribing audio into text with configurable recognition behavior.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **and Cloud Translation**
  - **Technical definition:** and Cloud Translation is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **processors**
  - **Technical definition:** Processors are managed extraction or analysis models configured for a specific document or AI task.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **processor versions**
  - **Technical definition:** Processor versions are the versioned implementations of a processor.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **datasets**
  - **Technical definition:** Datasets are the labeled or evaluation corpora used to improve or measure AI service behavior.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **evaluations**
  - **Technical definition:** Evaluations are the scored outputs comparing model or processor results against labeled expectations.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **models**
  - **Technical definition:** Models are the trained or managed inference artifacts used by the AI service.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **glossaries**
  - **Technical definition:** Glossaries are preferred-term mapping resources used to force domain-specific translation behavior.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **adaptive MT datasets**
  - **Technical definition:** Adaptive MT datasets are customer-provided translation examples used to adapt translation output.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.
- **long-running operations**
  - **Technical definition:** Long-running operations are asynchronous job handles representing work that completes over time.
  - **Concrete production example:** Example: a document-processing rollout is approved only after evaluation results show extraction quality remains above the expected threshold for invoices.

### Secret Manager and Cloud Key Management Service (Cloud KMS)

- **Secret Manager**
  - **Technical definition:** Secret Manager is Google's managed service for storing versioned secrets under IAM, replication, and rotation controls.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **Cloud Key Management Service (Cloud KMS)**
  - **Technical definition:** Cloud KMS is Google's managed cryptographic key lifecycle service for encryption, decryption, signing, verification, and external-key integration.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **secrets**
  - **Technical definition:** Secrets are the logical containers under which versioned secret payloads are stored in Secret Manager.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **secret versions**
  - **Technical definition:** Secret versions are the immutable payload instances beneath a secret.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **replication policies**
  - **Technical definition:** Replication policies define whether secret payloads replicate automatically or to selected locations.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **rotation schedules**
  - **Technical definition:** Rotation schedules define when new secret versions should be created or rotated.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **key rings**
  - **Technical definition:** Key rings are location-scoped administrative containers for cryptographic keys in Cloud KMS.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **keys**
  - **Technical definition:** Keys are the top-level cryptographic resources used for encryption, signing, or verification.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **key versions**
  - **Technical definition:** Key versions are the specific generations of cryptographic material beneath a key.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **protection levels**
  - **Technical definition:** Protection levels define whether key material is software-backed, HSM-backed, or externally backed.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **import jobs**
  - **Technical definition:** Import jobs are Cloud KMS workflows used to securely import externally generated key material.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.
- **external key managers**
  - **Technical definition:** External key managers are integrations that keep key material outside Google Cloud while permitting cryptographic operations.
  - **Concrete production example:** Example: an application fails to decrypt data after key rotation, and the team traces the issue to missing access on the new key version.

## Controls

### Expanded Points

- **Cloud Storage uniform bucket-level access**
  - **Technical definition:** Cloud Storage uniform bucket-level access is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **public access prevention**
  - **Technical definition:** public access prevention is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **object versioning**
  - **Technical definition:** object versioning is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **retention policies**
  - **Technical definition:** retention policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **bucket lock**
  - **Technical definition:** bucket lock is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **soft delete**
  - **Technical definition:** soft delete is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **customer-managed encryption keys (CMEK)**
  - **Technical definition:** customer-managed encryption keys (CMEK) is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Disk snapshot policies**
  - **Technical definition:** Disk snapshot policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **regional disk placement**
  - **Technical definition:** regional disk placement is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Filestore backup policies**
  - **Technical definition:** Filestore backup policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **encryption**
  - **Technical definition:** encryption is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **private access**
  - **Technical definition:** private access is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **restore controls**
  - **Technical definition:** restore controls is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Cloud SQL/AlloyDB private IP**
  - **Technical definition:** Cloud SQL/AlloyDB private IP is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **authorized networks**
  - **Technical definition:** authorized networks is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **IAM database authentication**
  - **Technical definition:** IAM database authentication is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **backup retention**
  - **Technical definition:** backup retention is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **point-in-time recovery**
  - **Technical definition:** point-in-time recovery is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **maintenance windows**
  - **Technical definition:** maintenance windows is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **flags**
  - **Technical definition:** flags is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **high availability**
  - **Technical definition:** high availability is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Spanner leader/region configuration**
  - **Technical definition:** Spanner leader/region configuration is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **backup schedules**
  - **Technical definition:** backup schedules is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **change streams**
  - **Technical definition:** change streams is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Bigtable app profiles**
  - **Technical definition:** Bigtable app profiles is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Firestore security rules**
  - **Technical definition:** Firestore security rules is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **index governance**
  - **Technical definition:** index governance is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **TTL**
  - **Technical definition:** TTL is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **backups**
  - **Technical definition:** backups is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Pub/Sub IAM**
  - **Technical definition:** Pub/Sub IAM is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **schema validation**
  - **Technical definition:** schema validation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **dead-letter topics**
  - **Technical definition:** dead-letter topics is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **retry policies**
  - **Technical definition:** retry policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **message retention**
  - **Technical definition:** message retention is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **ordering keys**
  - **Technical definition:** ordering keys is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **subscription filters**
  - **Technical definition:** subscription filters is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **BigQuery dataset/table IAM**
  - **Technical definition:** BigQuery dataset/table IAM is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **row-level security**
  - **Technical definition:** row-level security is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **column-level security**
  - **Technical definition:** column-level security is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **policy tags**
  - **Technical definition:** policy tags is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **authorized views**
  - **Technical definition:** authorized views is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **reservations**
  - **Technical definition:** reservations is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **partitioning**
  - **Technical definition:** partitioning is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **clustering**
  - **Technical definition:** clustering is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **query cost controls**
  - **Technical definition:** query cost controls is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Dataflow service accounts**
  - **Technical definition:** Dataflow service accounts is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **worker networking**
  - **Technical definition:** worker networking is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **streaming engine**
  - **Technical definition:** streaming engine is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **templates**
  - **Technical definition:** templates is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Dataproc cluster IAM**
  - **Technical definition:** Dataproc cluster IAM is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Composer environment access**
  - **Technical definition:** Composer environment access is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **DAG permissions**
  - **Technical definition:** DAG permissions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Dataplex governance**
  - **Technical definition:** Dataplex governance is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Data Catalog tags**
  - **Technical definition:** Data Catalog tags is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **lineage capture**
  - **Technical definition:** lineage capture is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Sensitive Data Protection inspection templates**
  - **Technical definition:** Sensitive Data Protection inspection templates is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **de-identification templates**
  - **Technical definition:** de-identification templates is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **VPC Service Controls**
  - **Technical definition:** VPC Service Controls is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Secret Manager replication**
  - **Technical definition:** Secret Manager replication is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **rotation**
  - **Technical definition:** rotation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **IAM**
  - **Technical definition:** IAM is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud KMS key rotation**
  - **Technical definition:** Cloud KMS key rotation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **key version state**
  - **Technical definition:** key version state is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **imported keys**
  - **Technical definition:** imported keys is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **CMEK usage**
  - **Technical definition:** CMEK usage is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

## Operations

### Expanded Points

- **Restore Cloud Storage object versions**
  - **Technical definition:** Restore Cloud Storage object versions is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validate lifecycle/retention**
  - **Technical definition:** validate lifecycle/retention is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **review public access**
  - **Technical definition:** review public access is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **test replication dependencies**
  - **Technical definition:** test replication dependencies is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Restore disks and Filestore backups**
  - **Technical definition:** Restore disks and Filestore backups is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validate mount access**
  - **Technical definition:** validate mount access is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **review throughput/capacity**
  - **Technical definition:** review throughput/capacity is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Test Cloud SQL/AlloyDB failover**
  - **Technical definition:** Test Cloud SQL/AlloyDB failover is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **point-in-time recovery**
  - **Technical definition:** point-in-time recovery is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **read replica promotion**
  - **Technical definition:** read replica promotion is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **connection limits**
  - **Technical definition:** connection limits is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **maintenance changes**
  - **Technical definition:** maintenance changes is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Troubleshoot Spanner latency**
  - **Technical definition:** Troubleshoot Spanner latency is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Bigtable hot tablets**
  - **Technical definition:** Bigtable hot tablets is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Firestore indexes**
  - **Technical definition:** Firestore indexes is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **multi-region consistency behavior**
  - **Technical definition:** multi-region consistency behavior is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Monitor Pub/Sub backlog**
  - **Technical definition:** Monitor Pub/Sub backlog is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **dead-letter topics**
  - **Technical definition:** dead-letter topics is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **retry behavior**
  - **Technical definition:** retry behavior is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **ordering keys**
  - **Technical definition:** ordering keys is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **subscriber lag**
  - **Technical definition:** subscriber lag is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Tune BigQuery partitioning/clustering**
  - **Technical definition:** Tune BigQuery partitioning/clustering is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **reservations**
  - **Technical definition:** reservations is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **query cost**
  - **Technical definition:** query cost is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **materialized views**
  - **Technical definition:** materialized views is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **BigQuery ML jobs**
  - **Technical definition:** BigQuery ML jobs is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Operate Dataflow streaming pipelines**
  - **Technical definition:** Operate Dataflow streaming pipelines is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Dataproc jobs**
  - **Technical definition:** Dataproc jobs is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Composer DAGs**
  - **Technical definition:** Composer DAGs is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Data Fusion pipelines**
  - **Technical definition:** Cloud Data Fusion pipelines is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Classify sensitive data with Sensitive Data Protection and validate governed data access through Dataplex/Data Catalog**
  - **Technical definition:** Classify sensitive data with Sensitive Data Protection and validate governed data access through Dataplex/Data Catalog is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Rotate secrets**
  - **Technical definition:** Rotate secrets is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **review Cloud KMS key usage**
  - **Technical definition:** review Cloud KMS key usage is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validate CMEK permissions**
  - **Technical definition:** validate CMEK permissions is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **troubleshoot encryption access failures**
  - **Technical definition:** troubleshoot encryption access failures is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

# Google Cloud Week 5: Observability, FinOps, And Operational Excellence

## Service And Entity Coverage

### Cloud Monitoring, Cloud Logging, Error Reporting, Cloud Trace, and Cloud Profiler

- **Cloud Monitoring**
  - **Technical definition:** Cloud Monitoring is Google's metrics, alerting, dashboarding, SLO, and uptime monitoring platform for workloads and cloud resources.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **Cloud Logging**
  - **Technical definition:** Cloud Logging is Google's centralized log ingestion, storage, routing, and querying service for Google Cloud and integrated workloads.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **Error Reporting**
  - **Technical definition:** Error Reporting is Google's application exception aggregation and grouping service for repeated error signatures.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **Cloud Trace**
  - **Technical definition:** Cloud Trace is Google's distributed tracing service for request latency and dependency timing analysis.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **and Cloud Profiler**
  - **Technical definition:** and Cloud Profiler is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **metrics**
  - **Technical definition:** Metrics are time-series measurements representing workload or infrastructure behavior.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **monitored resources**
  - **Technical definition:** Monitored resources are the typed entities whose telemetry is modeled and labeled in Monitoring and Logging.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **alerting policies**
  - **Technical definition:** Alerting policies are Monitoring rule objects evaluating conditions and triggering incidents or notifications.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **uptime checks**
  - **Technical definition:** Uptime checks are active probes testing service availability or response from managed locations.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **notification channels**
  - **Technical definition:** Notification channels are the destinations used to deliver Monitoring incidents and alerts.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **dashboards**
  - **Technical definition:** Dashboards are curated visual views combining telemetry, charts, and service context.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **log buckets**
  - **Technical definition:** Log buckets are Cloud Logging storage containers with retention, location, and access settings.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **log sinks**
  - **Technical definition:** Log sinks are routing rules exporting matching logs to destinations such as BigQuery, Pub/Sub, or Storage.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **log-based metrics**
  - **Technical definition:** Log-based metrics are metrics derived from matching patterns in log entries.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **error groups**
  - **Technical definition:** Error groups are aggregated recurring exception signatures identified by Error Reporting.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **traces**
  - **Technical definition:** Traces are request-level timing records showing end-to-end latency across services.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **spans**
  - **Technical definition:** Spans are the timed subunits of work inside a trace.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **profiles**
  - **Technical definition:** Profiles are continuous CPU, heap, or thread-behavior samples collected by Cloud Profiler.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.
- **service graphs**
  - **Technical definition:** Service graphs are dependency visualizations inferred from telemetry relationships between services.
  - **Concrete production example:** Example: an incident timeline combines alert policies, logs, traces, and profiles to identify a latency regression introduced by a new release.

### Managed Service for Prometheus and Google Cloud Managed Service for Grafana

- **Managed Service for Prometheus**
  - **Technical definition:** Managed Service for Prometheus is Google's Prometheus-compatible managed metrics collection and query backend.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **Google Cloud Managed Service for Grafana**
  - **Technical definition:** Google Cloud Managed Service for Grafana is Google's managed Grafana workspace service for dashboards, alerting, and multi-source visualization.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **collectors**
  - **Technical definition:** Collectors are ingestion components scraping, receiving, or forwarding Prometheus-compatible metrics.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **rules**
  - **Technical definition:** Rules are Prometheus recording or alert expressions evaluated against collected metrics.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **metrics scopes**
  - **Technical definition:** Metrics scopes are cross-project observability groupings allowing one project to view metrics from multiple projects.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **workspaces**
  - **Technical definition:** Workspaces are the managed Grafana administrative environments containing dashboards and data sources.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **dashboards**
  - **Technical definition:** Dashboards are Grafana visual compositions used to monitor metrics, logs, and service behavior.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **data sources**
  - **Technical definition:** Data sources are the backend connections Grafana uses to query telemetry systems.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **alerting**
  - **Technical definition:** Alerting is the rule-evaluation and notification behavior for abnormal telemetry conditions.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.
- **service accounts**
  - **Technical definition:** Service accounts are the identities used by managed observability components to read or write telemetry.
  - **Concrete production example:** Example: a platform team reduces noisy alerts by tightening Prometheus rules and removing high-cardinality labels from exported metrics.

### Network Intelligence Center and Personalized Service Health

- **Network Intelligence Center and Personalized Service Health**
  - **Technical definition:** Network Intelligence Center and Personalized Service Health is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: during a network outage, Connectivity Tests and Firewall Insights show a newly added deny rule is blocking east-west traffic.
- **Connectivity Tests**
  - **Technical definition:** Connectivity Tests are path-validation models checking reachability and blockers between endpoints.
  - **Concrete production example:** Example: during a network outage, Connectivity Tests and Firewall Insights show a newly added deny rule is blocking east-west traffic.
- **Performance Dashboard**
  - **Technical definition:** Performance Dashboard is the view of latency and packet-loss characteristics across Google Cloud network paths.
  - **Concrete production example:** Example: during a network outage, Connectivity Tests and Firewall Insights show a newly added deny rule is blocking east-west traffic.
- **Firewall Insights**
  - **Technical definition:** Firewall Insights are analysis outputs highlighting ineffective or risky firewall rules.
  - **Concrete production example:** Example: during a network outage, Connectivity Tests and Firewall Insights show a newly added deny rule is blocking east-west traffic.
- **Network Analyzer insights**
  - **Technical definition:** Network Analyzer insights are generated findings about networking misconfigurations, inefficiencies, or availability risks.
  - **Concrete production example:** Example: during a network outage, Connectivity Tests and Firewall Insights show a newly added deny rule is blocking east-west traffic.
- **service health events**
  - **Technical definition:** Service health events are Personalized Service Health records describing customer-impacting provider-side incidents or maintenance.
  - **Concrete production example:** Example: during a network outage, Connectivity Tests and Firewall Insights show a newly added deny rule is blocking east-west traffic.
- **impacted resources**
  - **Technical definition:** Impacted resources are the specific projects, services, or assets identified as affected by a service health event.
  - **Concrete production example:** Example: during a network outage, Connectivity Tests and Firewall Insights show a newly added deny rule is blocking east-west traffic.

### Cloud Billing, Billing export to BigQuery, budgets, Recommender, and Active Assist

- **Cloud Billing**
  - **Technical definition:** Cloud Billing is Google's payor and cost-association system for linking project spend to billing ownership, budgets, exports, and cost governance.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **Billing export to BigQuery**
  - **Technical definition:** Billing export to BigQuery is Google's detailed spend export mechanism for granular FinOps analysis and reporting.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **budgets**
  - **Technical definition:** budgets is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **Recommender**
  - **Technical definition:** Recommender is Google's recommendation engine for rightsizing, IAM reduction, reliability, and cost optimization actions.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **and Active Assist**
  - **Technical definition:** and Active Assist is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **billing accounts**
  - **Technical definition:** Billing accounts are the payer entities aggregating project charges and budget tracking.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **budgets**
  - **Technical definition:** Budgets are spend-threshold definitions tied to billing scopes for alerting and governance.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **alerts**
  - **Technical definition:** Alerts are notifications triggered when budget or spend conditions cross configured thresholds.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **exported billing tables**
  - **Technical definition:** exported billing tables is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **recommendations**
  - **Technical definition:** recommendations is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **insights**
  - **Technical definition:** insights is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **commitments**
  - **Technical definition:** commitments is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.
- **utilization metrics**
  - **Technical definition:** utilization metrics is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a DR rehearsal is delayed because regional quota usage leaves insufficient headroom for temporary failover capacity.

### Security Command Center, Event Threat Detection, Policy Intelligence, and Google Security Operations

- **Security Command Center**
  - **Technical definition:** Security Command Center is Google's centralized cloud security posture, asset, finding, and attack-path analysis platform.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **Event Threat Detection**
  - **Technical definition:** Event Threat Detection is Google's managed detection capability for suspicious activity and malicious patterns in cloud telemetry.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **Policy Intelligence**
  - **Technical definition:** Policy Intelligence is Google's access-analysis and recommendation capability for explaining effective access, detecting excess privilege, and improving IAM posture.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **and Google Security Operations**
  - **Technical definition:** and Google Security Operations is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **findings**
  - **Technical definition:** findings is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **sources**
  - **Technical definition:** sources is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **assets**
  - **Technical definition:** Assets are the discovered Google Cloud resources and attached metadata represented in Cloud Asset Inventory.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **attack paths**
  - **Technical definition:** attack paths is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **detectors**
  - **Technical definition:** detectors is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **rules**
  - **Technical definition:** rules is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **IAM analysis**
  - **Technical definition:** IAM analysis is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **cases**
  - **Technical definition:** cases is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **alerts**
  - **Technical definition:** alerts is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **parsers**
  - **Technical definition:** parsers is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **feeds**
  - **Technical definition:** feeds is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.
- **detections**
  - **Technical definition:** detections is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: during an audit, the team queries asset history and policy analysis to prove who owned a project and which principals could access a sensitive dataset.

### Vertex AI Model Monitoring, Vertex AI Experiments, Vertex ML Metadata, and Vertex AI model evaluation

- **Vertex AI Model Monitoring**
  - **Technical definition:** Vertex AI Model Monitoring is Google's model-observability service for drift, skew, and serving-behavior monitoring.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **Vertex AI Experiments**
  - **Technical definition:** Vertex AI Experiments is Google's ML experiment tracking capability for runs, metrics, and comparison workflows.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **Vertex ML Metadata**
  - **Technical definition:** Vertex ML Metadata is Google's lineage store for ML artifacts, executions, and contextual relationships.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **and Vertex AI model evaluation**
  - **Technical definition:** and Vertex AI model evaluation is a Google Cloud managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **monitoring jobs**
  - **Technical definition:** monitoring jobs is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **baselines**
  - **Technical definition:** baselines is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **skew/drift metrics**
  - **Technical definition:** skew/drift metrics is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **experiments**
  - **Technical definition:** experiments is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **runs**
  - **Technical definition:** runs is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **metadata artifacts**
  - **Technical definition:** metadata artifacts is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **executions**
  - **Technical definition:** executions is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **contexts**
  - **Technical definition:** contexts is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **evaluation datasets**
  - **Technical definition:** evaluation datasets is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.
- **evaluation metrics**
  - **Technical definition:** evaluation metrics is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: a Vertex AI model is registered and deployed only after evaluation jobs show it meets accuracy and safety thresholds.

## Controls

### Expanded Points

- **Cloud Monitoring alert policies**
  - **Technical definition:** Cloud Monitoring alert policies is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **notification channels**
  - **Technical definition:** notification channels is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **SLOs**
  - **Technical definition:** SLOs is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **uptime checks**
  - **Technical definition:** uptime checks is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **log retention**
  - **Technical definition:** log retention is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **log sinks**
  - **Technical definition:** log sinks is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **log-based metrics**
  - **Technical definition:** log-based metrics is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **metrics scopes**
  - **Technical definition:** metrics scopes is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **dashboard permissions**
  - **Technical definition:** dashboard permissions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Managed Service for Prometheus rule evaluation**
  - **Technical definition:** Managed Service for Prometheus rule evaluation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **collection configuration**
  - **Technical definition:** collection configuration is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **metric scope design**
  - **Technical definition:** metric scope design is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Grafana data source permissions**
  - **Technical definition:** Grafana data source permissions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **dashboard sharing**
  - **Technical definition:** dashboard sharing is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Network Intelligence Center connectivity tests**
  - **Technical definition:** Network Intelligence Center connectivity tests is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **firewall insights**
  - **Technical definition:** firewall insights is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **analyzer recommendations**
  - **Technical definition:** analyzer recommendations is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service health event routing**
  - **Technical definition:** service health event routing is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **incident notification paths**
  - **Technical definition:** incident notification paths is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Billing export**
  - **Technical definition:** Billing export is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **budget thresholds**
  - **Technical definition:** budget thresholds is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **cost labels**
  - **Technical definition:** cost labels is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Recommender permissions**
  - **Technical definition:** Recommender permissions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **commitment utilization**
  - **Technical definition:** commitment utilization is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **anomaly review**
  - **Technical definition:** anomaly review is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Security Command Center organization activation**
  - **Technical definition:** Security Command Center organization activation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **finding sources**
  - **Technical definition:** finding sources is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Event Threat Detection rules**
  - **Technical definition:** Event Threat Detection rules is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **attack path visibility**
  - **Technical definition:** attack path visibility is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Policy Intelligence access analysis**
  - **Technical definition:** Policy Intelligence access analysis is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Google Security Operations ingestion**
  - **Technical definition:** Google Security Operations ingestion is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Vertex AI model monitoring thresholds**
  - **Technical definition:** Vertex AI model monitoring thresholds is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **experiment metadata**
  - **Technical definition:** experiment metadata is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **ML metadata lineage**
  - **Technical definition:** ML metadata lineage is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **evaluation metrics**
  - **Technical definition:** evaluation metrics is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **model governance evidence**
  - **Technical definition:** model governance evidence is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

## Operations

### Expanded Points

- **Build service dashboards**
  - **Technical definition:** Build service dashboards is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **alert policies**
  - **Technical definition:** alert policies is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **log-based metrics**
  - **Technical definition:** log-based metrics is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **uptime checks**
  - **Technical definition:** uptime checks is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **traces**
  - **Technical definition:** traces is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **profiles**
  - **Technical definition:** profiles is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **incident timelines**
  - **Technical definition:** incident timelines is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Tune Prometheus alerts**
  - **Technical definition:** Tune Prometheus alerts is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **metric cardinality**
  - **Technical definition:** metric cardinality is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Grafana dashboards**
  - **Technical definition:** Grafana dashboards is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **cross-project observability**
  - **Technical definition:** cross-project observability is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Use Connectivity Tests**
  - **Technical definition:** Use Connectivity Tests is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Firewall Insights**
  - **Technical definition:** Firewall Insights is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Network Analyzer**
  - **Technical definition:** Network Analyzer is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Personalized Service Health during network incidents**
  - **Technical definition:** Personalized Service Health during network incidents is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Review billing export**
  - **Technical definition:** Review billing export is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **budget alerts**
  - **Technical definition:** budget alerts is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Recommender savings**
  - **Technical definition:** Recommender savings is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **commitment utilization**
  - **Technical definition:** commitment utilization is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **right-sizing actions**
  - **Technical definition:** right-sizing actions is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Triage Security Command Center findings**
  - **Technical definition:** Triage Security Command Center findings is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Event Threat Detection alerts**
  - **Technical definition:** Event Threat Detection alerts is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **IAM analysis results**
  - **Technical definition:** IAM analysis results is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Google Security Operations cases**
  - **Technical definition:** Google Security Operations cases is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Review Vertex AI model drift**
  - **Technical definition:** Review Vertex AI model drift is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **skew**
  - **Technical definition:** skew is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **experiments**
  - **Technical definition:** experiments is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **metadata lineage**
  - **Technical definition:** metadata lineage is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **model evaluation results**
  - **Technical definition:** model evaluation results is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

# Google Cloud Week 6: DR, Resilience, And Migration Execution

## Service And Entity Coverage

### Backup and DR Service

- **Backup and DR Service**
  - **Technical definition:** Backup and DR Service is Google's policy-driven backup orchestration and recovery platform for protected workloads and recovery points.
  - **Concrete production example:** Example: a restore job runs in non-production to prove recovery points are actually usable, not just present.
- **backup/recovery appliances**
  - **Technical definition:** Backup/recovery appliances are the infrastructure components holding or orchestrating backup data and recovery functions.
  - **Concrete production example:** Example: a restore job runs in non-production to prove recovery points are actually usable, not just present.
- **backup plans**
  - **Technical definition:** Backup plans are policy objects defining what is protected, how often backups run, and how recovery points are retained.
  - **Concrete production example:** Example: a restore job runs in non-production to prove recovery points are actually usable, not just present.
- **backup vaults**
  - **Technical definition:** Backup vaults are secured storage domains holding protected recovery data.
  - **Concrete production example:** Example: a restore job runs in non-production to prove recovery points are actually usable, not just present.
- **protected resources**
  - **Technical definition:** Protected resources are the workloads, disks, or databases included in backup scope.
  - **Concrete production example:** Example: a restore job runs in non-production to prove recovery points are actually usable, not just present.
- **recovery points**
  - **Technical definition:** Recovery points are the restorable backup states created at specific times.
  - **Concrete production example:** Example: a restore job runs in non-production to prove recovery points are actually usable, not just present.
- **backup/recovery policies**
  - **Technical definition:** Backup and recovery policies are control definitions governing schedule, retention, and recovery behavior.
  - **Concrete production example:** Example: a restore job runs in non-production to prove recovery points are actually usable, not just present.
- **restore jobs**
  - **Technical definition:** Restore jobs are executed recovery actions recreating data or systems from a selected recovery point.
  - **Concrete production example:** Example: a restore job runs in non-production to prove recovery points are actually usable, not just present.

### Migrate to Virtual Machines and Migrate to Containers

- **Migrate to Virtual Machines**
  - **Technical definition:** Migrate to Virtual Machines is Google's VM migration service for replication, test cloning, and cutover of server-based workloads.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **Migrate to Containers**
  - **Technical definition:** Migrate to Containers is Google's modernization service for transforming selected VM-based applications into container deployment artifacts.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **source VMs**
  - **Technical definition:** Source VMs are the origin virtual machines being replicated or transformed during migration.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **target projects**
  - **Technical definition:** Target projects are the Google Cloud projects designated to receive migrated workloads or generated artifacts.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **replication cycles**
  - **Technical definition:** Replication cycles are the repeated sync phases keeping a target sufficiently current with the source workload.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **test clones**
  - **Technical definition:** Test clones are non-production launched copies used to validate migrated behavior before final cutover.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **cutover jobs**
  - **Technical definition:** Cutover jobs are the final migration actions that shift workload authority to the Google Cloud target.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **migration waves**
  - **Technical definition:** Migration waves are grouped workload batches scheduled to move together based on dependency and risk logic.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **migration plans**
  - **Technical definition:** Migration plans are stepwise execution blueprints defining prerequisites, sequencing, and rollback considerations.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **fit assessments**
  - **Technical definition:** Fit assessments are suitability analyses determining whether a workload matches a target platform or modernization path.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **artifacts**
  - **Technical definition:** Artifacts are the generated images, manifests, or outputs produced during migration workflows.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.
- **generated deployment files**
  - **Technical definition:** Generated deployment files are the rendered manifests or configuration files used to deploy transformed workloads.
  - **Concrete production example:** Example: a VM migration wave uses test clones first so startup behavior, IAM, and network reachability can be validated before cutover.

### Database Migration Service

- **Database Migration Service**
  - **Technical definition:** Database Migration Service is Google's managed relational migration platform for full-load, continuous replication, and cutover workflows.
  - **Concrete production example:** Example: a database migration runs full load plus CDC until lag is near zero, then cutover executes with validation queries.
- **connection profiles**
  - **Technical definition:** Connection profiles are the source and target connectivity definitions used by Database Migration Service.
  - **Concrete production example:** Example: a database migration runs full load plus CDC until lag is near zero, then cutover executes with validation queries.
- **migration jobs**
  - **Technical definition:** Migration jobs are the managed workflows executing full load, replication, and cutover logic.
  - **Concrete production example:** Example: a database migration runs full load plus CDC until lag is near zero, then cutover executes with validation queries.
- **conversion workspaces**
  - **Technical definition:** Conversion workspaces are schema-assessment and conversion environments used for heterogeneous migrations.
  - **Concrete production example:** Example: a database migration runs full load plus CDC until lag is near zero, then cutover executes with validation queries.
- **source databases**
  - **Technical definition:** Source databases are the origin systems from which data is being migrated.
  - **Concrete production example:** Example: a database migration runs full load plus CDC until lag is near zero, then cutover executes with validation queries.
- **destination databases**
  - **Technical definition:** Destination databases are the target systems receiving migrated data.
  - **Concrete production example:** Example: a database migration runs full load plus CDC until lag is near zero, then cutover executes with validation queries.
- **change data capture (CDC)**
  - **Technical definition:** Change Data Capture is the incremental replication mode that streams source changes after initial load.
  - **Concrete production example:** Example: a database migration runs full load plus CDC until lag is near zero, then cutover executes with validation queries.
- **migration phases**
  - **Technical definition:** Migration phases are the lifecycle stages such as assessment, full load, CDC, validation, and cutover.
  - **Concrete production example:** Example: a database migration runs full load plus CDC until lag is near zero, then cutover executes with validation queries.

### Migration Center

- **Migration Center**
  - **Technical definition:** Migration Center is Google's discovery, assessment, grouping, and migration-planning service for large migration programs.
  - **Concrete production example:** Example: discovery data is grouped into migration waves so dependent services move together instead of breaking each other during cutover.
- **discovery clients**
  - **Technical definition:** Discovery clients are the agents or integrations collecting inventory and performance data from migration-candidate systems.
  - **Concrete production example:** Example: discovery data is grouped into migration waves so dependent services move together instead of breaking each other during cutover.
- **assets**
  - **Technical definition:** Assets are the discovered servers, databases, or applications represented in Migration Center inventory.
  - **Concrete production example:** Example: discovery data is grouped into migration waves so dependent services move together instead of breaking each other during cutover.
- **groups**
  - **Technical definition:** Groups are organizational collections of migration assets used for planning and reporting.
  - **Concrete production example:** Example: discovery data is grouped into migration waves so dependent services move together instead of breaking each other during cutover.
- **preference sets**
  - **Technical definition:** Preference sets are rule bundles expressing target preferences such as region, machine family, or modernization approach.
  - **Concrete production example:** Example: discovery data is grouped into migration waves so dependent services move together instead of breaking each other during cutover.
- **cost estimates**
  - **Technical definition:** Cost estimates are the projected target-environment spend calculations for discovered workloads.
  - **Concrete production example:** Example: discovery data is grouped into migration waves so dependent services move together instead of breaking each other during cutover.
- **fit assessments**
  - **Technical definition:** Fit assessments are suitability analyses for possible target platform options.
  - **Concrete production example:** Example: discovery data is grouped into migration waves so dependent services move together instead of breaking each other during cutover.
- **migration plans**
  - **Technical definition:** Migration plans are grouped workload transition plans based on assessment and wave logic.
  - **Concrete production example:** Example: discovery data is grouped into migration waves so dependent services move together instead of breaking each other during cutover.

### Storage Transfer Service and Transfer Appliance

- **Storage Transfer Service**
  - **Technical definition:** Storage Transfer Service is Google's managed data movement service for scheduled or on-demand transfers between supported storage systems.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **Transfer Appliance**
  - **Technical definition:** Transfer Appliance is Google's physical device for offline transfer of very large datasets into Google Cloud.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **transfer jobs**
  - **Technical definition:** Transfer jobs are the top-level scheduled or on-demand data movement definitions in Storage Transfer Service.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **sources**
  - **Technical definition:** Sources are the origin storage systems from which transfer reads data.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **sinks**
  - **Technical definition:** Sinks are the destination storage systems into which transfer writes data.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **agents**
  - **Technical definition:** Agents are the software components used for file-system and certain on-premises transfer scenarios.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **schedules**
  - **Technical definition:** Schedules are the timing definitions controlling when transfer jobs execute.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **manifests**
  - **Technical definition:** Manifests are explicit file or object lists restricting transfer scope to named items.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **appliances**
  - **Technical definition:** Appliances are the physical Transfer Appliance devices used to move large datasets offline.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **jobs**
  - **Technical definition:** Jobs are the executed appliance or logical transfer runs associated with a transfer workflow.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.
- **transfer logs**
  - **Technical definition:** Transfer logs are the operational records describing status, failures, and item-level outcomes.
  - **Concrete production example:** Example: petabyte-scale archive data is moved with Transfer Appliance and then reconciled with transfer logs before source decommissioning.

### Disaster Recovery Patterns

- **Disaster Recovery Patterns**
  - **Technical definition:** Disaster Recovery Patterns are the architectural and procedural constructs that define how Google Cloud workloads recover, fail over, and return to steady state.
  - **Concrete production example:** Example: a quarterly game day validates failover order, DNS changes, secret access, and monitoring evidence for critical services.
- **runbooks**
  - **Technical definition:** Runbooks are procedural recovery documents defining exact restore, failover, and validation steps.
  - **Concrete production example:** Example: a quarterly game day validates failover order, DNS changes, secret access, and monitoring evidence for critical services.
- **recovery tiers**
  - **Technical definition:** Recovery tiers are categorized DR service levels reflecting different RTO/RPO and standby cost tradeoffs.
  - **Concrete production example:** Example: a quarterly game day validates failover order, DNS changes, secret access, and monitoring evidence for critical services.
- **dependency maps**
  - **Technical definition:** Dependency maps are recorded relationships between apps, identities, networks, and data systems that govern recovery order.
  - **Concrete production example:** Example: a quarterly game day validates failover order, DNS changes, secret access, and monitoring evidence for critical services.
- **restore order**
  - **Technical definition:** Restore order is the dependency-aware sequence in which systems must be recovered to produce a functioning environment.
  - **Concrete production example:** Example: a quarterly game day validates failover order, DNS changes, secret access, and monitoring evidence for critical services.
- **failover steps**
  - **Technical definition:** Failover steps are the ordered actions shifting service authority to the recovery environment.
  - **Concrete production example:** Example: a quarterly game day validates failover order, DNS changes, secret access, and monitoring evidence for critical services.
- **failback steps**
  - **Technical definition:** Failback steps are the ordered actions returning authority from the recovery site to primary.
  - **Concrete production example:** Example: a quarterly game day validates failover order, DNS changes, secret access, and monitoring evidence for critical services.
- **game day evidence**
  - **Technical definition:** Game day evidence is the collected proof from resilience exercises showing what was tested and whether objectives were met.
  - **Concrete production example:** Example: a quarterly game day validates failover order, DNS changes, secret access, and monitoring evidence for critical services.

## DR Dependencies From Earlier Weeks

### Expanded Points

- **Cloud DNS**
  - **Technical definition:** Cloud DNS is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Load Balancing**
  - **Technical definition:** Cloud Load Balancing is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud VPN**
  - **Technical definition:** Cloud VPN is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Interconnect are covered in Week 2**
  - **Technical definition:** Cloud Interconnect are covered in Week 2 is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Cloud Storage replication**
  - **Technical definition:** Cloud Storage replication is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Filestore backups**
  - **Technical definition:** Filestore backups is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud SQL high availability**
  - **Technical definition:** Cloud SQL high availability is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Spanner multi-region configurations**
  - **Technical definition:** Spanner multi-region configurations is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Bigtable replication**
  - **Technical definition:** Bigtable replication is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Pub/Sub retry/dead-letter patterns are covered in Week 4**
  - **Technical definition:** Pub/Sub retry/dead-letter patterns are covered in Week 4 is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Cloud Monitoring**
  - **Technical definition:** Cloud Monitoring is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud Logging**
  - **Technical definition:** Cloud Logging is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Personalized Service Health**
  - **Technical definition:** Personalized Service Health is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Security Command Center**
  - **Technical definition:** Security Command Center is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Cloud KMS are covered in earlier weeks**
  - **Technical definition:** Cloud KMS are covered in earlier weeks is a Google Cloud entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and control-plane consequences.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

## Controls

### Expanded Points

- **Backup and DR backup plan scope**
  - **Technical definition:** Backup and DR backup plan scope is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **backup vault permissions**
  - **Technical definition:** backup vault permissions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **immutability posture**
  - **Technical definition:** immutability posture is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **restore permissions**
  - **Technical definition:** restore permissions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **recovery point retention**
  - **Technical definition:** recovery point retention is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Migrate to Virtual Machines replication settings**
  - **Technical definition:** Migrate to Virtual Machines replication settings is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **test clone configuration**
  - **Technical definition:** test clone configuration is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **cutover settings**
  - **Technical definition:** cutover settings is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **target project mapping**
  - **Technical definition:** target project mapping is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **service account permissions**
  - **Technical definition:** service account permissions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **rollback planning**
  - **Technical definition:** rollback planning is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Migrate to Containers fit assessment**
  - **Technical definition:** Migrate to Containers fit assessment is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **generated artifacts**
  - **Technical definition:** generated artifacts is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **cluster target configuration**
  - **Technical definition:** cluster target configuration is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **deployment validation**
  - **Technical definition:** deployment validation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Database Migration Service connection profiles**
  - **Technical definition:** Database Migration Service connection profiles is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **private connectivity**
  - **Technical definition:** private connectivity is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **full-load settings**
  - **Technical definition:** full-load settings is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **change data capture (CDC)**
  - **Technical definition:** change data capture (CDC) is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validation**
  - **Technical definition:** validation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **cutover controls**
  - **Technical definition:** cutover controls is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Migration Center discovery scope**
  - **Technical definition:** Migration Center discovery scope is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **asset grouping**
  - **Technical definition:** asset grouping is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **preference sets**
  - **Technical definition:** preference sets is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **dependency mapping**
  - **Technical definition:** dependency mapping is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **wave planning**
  - **Technical definition:** wave planning is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Storage Transfer Service schedule**
  - **Technical definition:** Storage Transfer Service schedule is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **source/sink permissions**
  - **Technical definition:** source/sink permissions is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **agent configuration**
  - **Technical definition:** agent configuration is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **transfer validation**
  - **Technical definition:** transfer validation is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **Transfer Appliance chain-of-custody controls**
  - **Technical definition:** Transfer Appliance chain-of-custody controls is a Google Cloud control that should be understood in terms of preventive value, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

## Operations

### Expanded Points

- **Run backup restore tests**
  - **Technical definition:** Run backup restore tests is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validate recovery points**
  - **Technical definition:** validate recovery points is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **collect recovery evidence**
  - **Technical definition:** collect recovery evidence is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Execute VM migration test clones**
  - **Technical definition:** Execute VM migration test clones is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **cutover jobs**
  - **Technical definition:** cutover jobs is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **rollback planning**
  - **Technical definition:** rollback planning is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **post-cutover validation**
  - **Technical definition:** post-cutover validation is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Assess container migration fit**
  - **Technical definition:** Assess container migration fit is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **review generated Kubernetes artifacts**
  - **Technical definition:** review generated Kubernetes artifacts is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validate target clusters**
  - **Technical definition:** validate target clusters is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Run database migration full load plus CDC**
  - **Technical definition:** Run database migration full load plus CDC is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **validate data**
  - **Technical definition:** validate data is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **monitor replication lag**
  - **Technical definition:** monitor replication lag is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **rehearse cutover/rollback**
  - **Technical definition:** rehearse cutover/rollback is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Use Migration Center for inventory**
  - **Technical definition:** Use Migration Center for inventory is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **grouping**
  - **Technical definition:** grouping is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **wave planning**
  - **Technical definition:** wave planning is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **migration reporting**
  - **Technical definition:** migration reporting is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Transfer file/object datasets with Storage Transfer Service or Transfer Appliance and validate checksums/cutover sync**
  - **Technical definition:** Transfer file/object datasets with Storage Transfer Service or Transfer Appliance and validate checksums/cutover sync is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.

### Expanded Points

- **Run DR game days with failover**
  - **Technical definition:** Run DR game days with failover is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **failback**
  - **Technical definition:** failback is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **monitoring**
  - **Technical definition:** monitoring is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **security access**
  - **Technical definition:** security access is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
- **dependency validation**
  - **Technical definition:** dependency validation is a Google Cloud operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, cutover, or recovery when exact cloud behavior matters.
