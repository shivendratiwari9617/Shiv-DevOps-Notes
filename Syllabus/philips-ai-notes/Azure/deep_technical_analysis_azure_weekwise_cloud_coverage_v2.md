# Deep Technical Analysis of Azure Week-Wise Cloud Coverage — v2
## Azure Technical Study Notes with Independent Expansion of Comma-Separated and `and`-Separated Service or Entity Points

> Goal: push the notes toward a **9.8+ technical study bar** by making the definitions more Azure-native, less template-shaped, and more precise at the child-entity level.
> Parsing rule applied: every comma-separated value is treated as a point, and when **`and` joins two Azure services or two entities**, they are treated as separate points with independent definitions.
> Format: technical definition first, with one concrete production example wherever useful.

# Azure Week 1: Landing Zones, Subscriptions, And Governance

## Service And Entity Coverage

### Azure management groups, Azure subscriptions, Azure resource groups, and Azure landing zones

- **Azure management groups**
  - **Technical definition:** Azure Management Groups are hierarchy containers above subscriptions used to apply inherited governance, policy, RBAC, and organizational structure at scale.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **Azure subscriptions**
  - **Technical definition:** Azure Subscriptions are the primary billing, quota, and administrative boundaries for Azure resource ownership and service consumption.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **Azure resource groups**
  - **Technical definition:** Azure Resource Groups are logical containers for Azure resources sharing lifecycle, deployment scope, and policy/RBAC attachment.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **and Azure landing zones**
  - **Technical definition:** and Azure landing zones is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **tenant root group**
  - **Technical definition:** The tenant root group is the highest management group scope in an Azure tenant and therefore the top inheritance boundary for policy and RBAC.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **management group hierarchy**
  - **Technical definition:** Management group hierarchy is the nested tree of management groups used to organize subscriptions and inherited governance at scale.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **subscriptions**
  - **Technical definition:** Subscriptions are Azure billing, quota, and administrative scopes under which resource providers are enabled and resources are deployed.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **resource groups**
  - **Technical definition:** Resource groups are deployment and lifecycle scopes grouping Azure resources for ownership, ARM deployment, and policy/RBAC attachment.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **landing zone archetypes**
  - **Technical definition:** Landing zone archetypes are predefined Azure environment patterns describing how identity, networking, governance, and operations should be assembled for a class of workload.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **platform subscriptions**
  - **Technical definition:** Platform subscriptions are subscriptions reserved for shared services such as connectivity, management, identity integration, or security tooling.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **application subscriptions**
  - **Technical definition:** Application subscriptions are workload subscriptions intended to host business applications under landing-zone controls.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.
- **Azure landing zone accelerators**
  - **Technical definition:** Azure landing zone accelerators are Microsoft-provided automation packages and templates for deploying landing-zone reference patterns quickly.
  - **Concrete production example:** Example: a new workload subscription is created under the correct management group so inherited policy, RBAC, budget, and logging baselines apply automatically.

### Microsoft Entra ID and Microsoft Entra Privileged Identity Management

- **Microsoft Entra ID**
  - **Technical definition:** Microsoft Entra ID is Azure's identity directory and authentication control plane for workforce identities, applications, devices, and federation.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **Microsoft Entra Privileged Identity Management**
  - **Technical definition:** Microsoft Entra Privileged Identity Management is Azure's just-in-time privileged access service for time-bound role elevation, approval, audit, and access review.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **tenants**
  - **Technical definition:** Tenants are Microsoft Entra directory instances representing the top identity and trust boundary for users, groups, apps, and subscription associations.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **users**
  - **Technical definition:** Users are human identity objects in Entra ID used for authentication, access assignment, and governance workflows.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **groups**
  - **Technical definition:** Groups are identity collections used to simplify RBAC assignment, application access, and governance administration.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **enterprise applications**
  - **Technical definition:** Enterprise applications are tenant-local service-principal representations of applications used for SSO, consent, and access control.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **app registrations**
  - **Technical definition:** App registrations are the application definitions that describe an app's identity, credentials, redirect URIs, and API permissions.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **service principals**
  - **Technical definition:** Service principals are tenant-local security identities created from app registrations and used by applications or automation to sign in.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **managed identities**
  - **Technical definition:** Managed identities are Azure-managed service principals bound to Azure resources so workloads can authenticate without stored secrets.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **Conditional Access policies**
  - **Technical definition:** Conditional Access policies are context-aware sign-in enforcement rules using signals such as identity risk, device state, app, and location.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **access reviews**
  - **Technical definition:** Access reviews are recurring governance workflows for validating whether users still need assigned roles or group memberships.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **eligible assignments**
  - **Technical definition:** Eligible assignments are PIM assignments that require activation before privileges become effective.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **active assignments**
  - **Technical definition:** Active assignments are currently effective role grants, whether standing or activated through PIM.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.
- **role activation settings**
  - **Technical definition:** Role activation settings are PIM policy controls defining MFA, approval, justification, ticket, and duration requirements during privilege activation.
  - **Concrete production example:** Example: a production operator activates an eligible privileged role with MFA and approval instead of holding standing global administrative access.

### Azure role-based access control (Azure RBAC)

- **Azure role-based access control (Azure RBAC)**
  - **Technical definition:** Azure RBAC is Azure's authorization model for granting management-plane and selected data-plane permissions through scoped role assignments.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.
- **scopes**
  - **Technical definition:** Scopes are the Azure hierarchy levels—management group, subscription, resource group, or individual resource—at which role assignments take effect.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.
- **role definitions**
  - **Technical definition:** Role definitions are the permission bundles specifying allowed actions, notActions, dataActions, and assignable scopes for a role.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.
- **role assignments**
  - **Technical definition:** Role assignments bind a principal to a role definition at a specific scope.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.
- **built-in roles**
  - **Technical definition:** Built-in roles are Microsoft-provided predefined role definitions covering common administrative and operational patterns.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.
- **custom roles**
  - **Technical definition:** Custom roles are customer-authored Azure RBAC role definitions tailored to organization-specific least-privilege needs.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.
- **deny assignments**
  - **Technical definition:** Deny assignments are Azure authorization artifacts that explicitly block actions even when role grants might otherwise allow them.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.
- **managed identities**
  - **Technical definition:** Managed identities are frequently used RBAC assignees for automation and workload access without credential files.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.
- **privileged roles**
  - **Technical definition:** Privileged roles are high-impact administrative roles whose misuse creates broad tenant, subscription, or platform blast radius.
  - **Concrete production example:** Example: a deployment identity is granted Contributor at one resource group instead of subscription scope so blast radius stays limited.

### Azure Policy

- **Azure Policy**
  - **Technical definition:** Azure Policy is Azure's preventive and detective governance system for evaluating, enforcing, and remediating configuration state across resources and hierarchy scopes.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **policy definitions**
  - **Technical definition:** Policy definitions are the individual Azure Policy rules describing what state is evaluated and what effect is applied.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **initiatives**
  - **Technical definition:** Initiatives are grouped collections of policy definitions managed as one governance package.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **assignments**
  - **Technical definition:** Assignments apply a policy definition or initiative to a target hierarchy scope.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **exemptions**
  - **Technical definition:** Exemptions are approved exceptions recorded against a policy assignment so known deviations are tracked rather than silently ignored.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **parameters**
  - **Technical definition:** Parameters are reusable inputs supplied to policy definitions or initiatives at assignment time.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **effects**
  - **Technical definition:** Effects are Azure Policy behaviors such as Audit, Deny, Append, Modify, or DeployIfNotExists.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **compliance states**
  - **Technical definition:** Compliance states are Azure Policy evaluation outcomes indicating whether a resource currently satisfies assigned requirements.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **remediation tasks**
  - **Technical definition:** Remediation tasks are policy-driven operations that deploy or modify resources to bring them toward compliance.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.
- **policy inheritance**
  - **Technical definition:** Policy inheritance is the Azure hierarchy behavior by which child scopes evaluate policy assigned at parent scopes.
  - **Concrete production example:** Example: a Deny policy blocks public IP creation in workload subscriptions, and remediation tasks bring existing resources back into the standard.

### Azure Resource Graph, Azure Deployment Stacks, management locks, tags, and naming standards

- **Azure Resource Graph**
  - **Technical definition:** Azure Resource Graph is Azure's indexed resource inventory and query service for fast cross-subscription governance and topology analysis.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **Azure Deployment Stacks**
  - **Technical definition:** Azure Deployment Stacks are Azure deployment lifecycle constructs that track ARM/Bicep-managed resources together and can enforce delete-protection behavior.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **management locks**
  - **Technical definition:** management locks is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **tags**
  - **Technical definition:** tags is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **and naming standards**
  - **Technical definition:** and naming standards is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **resource queries**
  - **Technical definition:** Resource queries are Azure Resource Graph KQL-style expressions used to inventory, correlate, and audit Azure resources across many scopes.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **subscriptions**
  - **Technical definition:** Subscriptions are common query and governance scopes surfaced by Resource Graph and deployment-protection tooling.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **management groups**
  - **Technical definition:** Management groups are top-down governance scopes often queried to validate inheritance coverage and organizational structure.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **deployment stacks**
  - **Technical definition:** Deployment stacks are tracked collections of ARM/Bicep-deployed resources managed together for safer lifecycle control.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **stack resources**
  - **Technical definition:** Stack resources are the Azure resources associated with and governed by a deployment stack.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **deny settings**
  - **Technical definition:** Deny settings are deployment-stack options used to protect stack-managed resources from unintended deletion or modification.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **CanNotDelete locks**
  - **Technical definition:** CanNotDelete locks are management locks that prevent deletion while still allowing allowed update actions.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **ReadOnly locks**
  - **Technical definition:** ReadOnly locks are management locks that prevent modification and deletion of a resource.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **tag names**
  - **Technical definition:** Tag names are the metadata keys applied to Azure resources for governance, automation, and cost allocation.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **tag values**
  - **Technical definition:** Tag values are the metadata values paired with tag names to encode ownership, environment, application, or cost context.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.
- **naming rules**
  - **Technical definition:** Naming rules are the organization's standards defining how Azure resources should be named for consistency, automation, and discoverability.
  - **Concrete production example:** Example: Resource Graph is queried during audit to prove which subscriptions own internet-facing resources and which locks protect shared services.

### Azure Lighthouse

- **Azure Lighthouse**
  - **Technical definition:** Azure Lighthouse is Azure's cross-tenant delegated administration model for managing customer or external tenant resources from a service-provider tenant.
  - **Concrete production example:** Example: a managed service provider administers a customer's environment from its own tenant without creating guest admin accounts in every customer tenant.
- **managing tenants**
  - **Technical definition:** Managing tenants are provider or operator tenants from which delegated customer environments are administered.
  - **Concrete production example:** Example: a managed service provider administers a customer's environment from its own tenant without creating guest admin accounts in every customer tenant.
- **customer tenants**
  - **Technical definition:** Customer tenants are delegated Entra tenants whose resources are administered through Azure Lighthouse relationships.
  - **Concrete production example:** Example: a managed service provider administers a customer's environment from its own tenant without creating guest admin accounts in every customer tenant.
- **delegated resource management**
  - **Technical definition:** Delegated resource management is the Azure Lighthouse mechanism granting scoped cross-tenant administration rights.
  - **Concrete production example:** Example: a managed service provider administers a customer's environment from its own tenant without creating guest admin accounts in every customer tenant.
- **offers**
  - **Technical definition:** Offers are the Azure Lighthouse published delegation definitions describing included authorizations and accessible scopes.
  - **Concrete production example:** Example: a managed service provider administers a customer's environment from its own tenant without creating guest admin accounts in every customer tenant.
- **assignments**
  - **Technical definition:** Assignments are the accepted Lighthouse delegations applying published authorizations to customer scopes.
  - **Concrete production example:** Example: a managed service provider administers a customer's environment from its own tenant without creating guest admin accounts in every customer tenant.
- **authorizations**
  - **Technical definition:** Authorizations are the specific role grants included in a Lighthouse delegation.
  - **Concrete production example:** Example: a managed service provider administers a customer's environment from its own tenant without creating guest admin accounts in every customer tenant.
- **eligible authorizations**
  - **Technical definition:** Eligible authorizations are just-in-time delegated grants that require activation before becoming active.
  - **Concrete production example:** Example: a managed service provider administers a customer's environment from its own tenant without creating guest admin accounts in every customer tenant.

### Microsoft Cost Management

- **Microsoft Cost Management**
  - **Technical definition:** Microsoft Cost Management is Azure's spend analysis, budgeting, export, and cost-governance platform across billing scopes and subscriptions.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **billing accounts**
  - **Technical definition:** Billing accounts are the top-level Azure billing ownership entities under which invoices and charges are managed.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **billing profiles**
  - **Technical definition:** Billing profiles are billing segmentation objects used to group invoicing and payment characteristics.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **invoice sections**
  - **Technical definition:** Invoice sections are subdivisions of a billing profile used to separate charges and reporting lines.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **subscriptions**
  - **Technical definition:** Subscriptions are cost-incurring scopes linked to billing entities and cost analysis.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **budgets**
  - **Technical definition:** Budgets are defined spend thresholds associated with billing or resource scopes for alerting and accountability.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **alerts**
  - **Technical definition:** Alerts are notifications triggered when budget or forecast conditions cross configured thresholds.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **cost allocation rules**
  - **Technical definition:** Cost allocation rules redistribute shared Azure costs across target subscriptions, resource groups, or tags.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **exports**
  - **Technical definition:** Exports are scheduled output definitions delivering cost and usage data to storage or analysis destinations.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **reservations**
  - **Technical definition:** Reservations are prepaid commitments applied to eligible services for discounted billing.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.

## Controls

### Expanded Points

- **Management group hierarchy**
  - **Technical definition:** Management group hierarchy is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **subscription vending**
  - **Technical definition:** subscription vending is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **resource group ownership**
  - **Technical definition:** resource group ownership is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **tag/naming policy**
  - **Technical definition:** tag/naming policy is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **landing-zone baseline inheritance**
  - **Technical definition:** landing-zone baseline inheritance is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Microsoft Entra Conditional Access**
  - **Technical definition:** Microsoft Entra Conditional Access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Privileged Identity Management activation**
  - **Technical definition:** Privileged Identity Management activation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **access reviews**
  - **Technical definition:** access reviews is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **break-glass accounts**
  - **Technical definition:** break-glass accounts is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **managed identities**
  - **Technical definition:** managed identities is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **service principal governance**
  - **Technical definition:** service principal governance is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure RBAC scope design**
  - **Technical definition:** Azure RBAC scope design is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **built-in/custom roles**
  - **Technical definition:** built-in/custom roles is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **least privilege**
  - **Technical definition:** least privilege is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **privileged role assignment**
  - **Technical definition:** privileged role assignment is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **deny assignment awareness**
  - **Technical definition:** deny assignment awareness is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure Policy initiative assignment**
  - **Technical definition:** Azure Policy initiative assignment is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **effect selection**
  - **Technical definition:** effect selection is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **exemptions**
  - **Technical definition:** exemptions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **remediation tasks**
  - **Technical definition:** remediation tasks is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **compliance reporting**
  - **Technical definition:** compliance reporting is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Deployment Stack deny settings**
  - **Technical definition:** Deployment Stack deny settings is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **management locks**
  - **Technical definition:** management locks is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Resource Graph inventory queries**
  - **Technical definition:** Resource Graph inventory queries is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **platform baseline validation**
  - **Technical definition:** platform baseline validation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Cost Management budgets**
  - **Technical definition:** Cost Management budgets is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **alerts**
  - **Technical definition:** alerts is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **exports**
  - **Technical definition:** exports is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **reservations**
  - **Technical definition:** reservations is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **subscription chargeback ownership**
  - **Technical definition:** subscription chargeback ownership is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

## Operations

### Expanded Points

- **Design a management group**
  - **Technical definition:** Design a management group is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **subscription**
  - **Technical definition:** subscription is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **resource group model for platform**
  - **Technical definition:** resource group model for platform is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **shared services**
  - **Technical definition:** shared services is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **workloads**
  - **Technical definition:** workloads is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Create subscription vending standards with naming**
  - **Technical definition:** Create subscription vending standards with naming is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **tags**
  - **Technical definition:** tags is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **budgets**
  - **Technical definition:** budgets is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **policy assignments**
  - **Technical definition:** policy assignments is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **RBAC**
  - **Technical definition:** RBAC is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **diagnostic settings**
  - **Technical definition:** diagnostic settings is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Review Microsoft Entra privileged roles**
  - **Technical definition:** Review Microsoft Entra privileged roles is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Conditional Access**
  - **Technical definition:** Conditional Access is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **managed identities**
  - **Technical definition:** managed identities is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **break-glass readiness**
  - **Technical definition:** break-glass readiness is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Validate Azure Policy compliance**
  - **Technical definition:** Validate Azure Policy compliance is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **exemptions**
  - **Technical definition:** exemptions is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **remediation tasks before onboarding workloads**
  - **Technical definition:** remediation tasks before onboarding workloads is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Query Azure Resource Graph for inventory**
  - **Technical definition:** Query Azure Resource Graph for inventory is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **ownership**
  - **Technical definition:** ownership is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **governance evidence**
  - **Technical definition:** governance evidence is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Validate landing-zone readiness before migration by checking identity**
  - **Technical definition:** Validate landing-zone readiness before migration by checking identity is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **policy**
  - **Technical definition:** policy is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **networking**
  - **Technical definition:** networking is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **billing**
  - **Technical definition:** billing is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **quotas**
  - **Technical definition:** quotas is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **operational access**
  - **Technical definition:** operational access is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

# Azure Week 2: Networking, Private Connectivity, And Edge Traffic

## Service And Entity Coverage

### Azure Virtual Network

- **Azure Virtual Network**
  - **Technical definition:** Azure Virtual Network is Azure's software-defined private network boundary for address space, subnets, routing, security policy, and private connectivity.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **virtual networks**
  - **Technical definition:** Virtual networks are the top-level Azure network boundaries containing address spaces, subnets, and network-policy attachment points.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **subnets**
  - **Technical definition:** Subnets are IP address partitions inside a virtual network where Azure resources are placed and subnet-scoped policy is applied.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **route tables**
  - **Technical definition:** Route tables are collections of routes associated to subnets for controlling next-hop behavior.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **user-defined routes**
  - **Technical definition:** User-defined routes are custom route entries overriding Azure system routing for selected destinations.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **network security groups (NSGs)**
  - **Technical definition:** Network Security Groups are stateful packet-filtering constructs applied to subnets or NICs for inbound and outbound traffic control.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **application security groups**
  - **Technical definition:** Application Security Groups are logical workload groupings used as NSG rule sources or destinations instead of raw IP ranges.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **service endpoints**
  - **Technical definition:** Service endpoints extend subnet identity to supported Azure services so access can be restricted by virtual network and traffic stays on Microsoft's backbone.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **private endpoints**
  - **Technical definition:** Private endpoints are NIC-backed private IP interfaces exposing Azure PaaS or Private Link services directly inside a VNet.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **Azure Private Link**
  - **Technical definition:** Azure Private Link is Azure's producer-consumer private connectivity model delivering services through private endpoints.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **virtual network peering**
  - **Technical definition:** Virtual network peering is Azure's private VNet-to-VNet connectivity relationship over Microsoft's backbone.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.

### Azure Virtual Network Manager and Azure Virtual WAN

- **Azure Virtual Network Manager**
  - **Technical definition:** Azure Virtual Network Manager is Azure's centralized network-governance service for defining connectivity and security intent across many virtual networks.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **Azure Virtual WAN**
  - **Technical definition:** Azure Virtual WAN is Azure's managed hub-and-spoke and branch connectivity platform for large-scale transit, remote-user, and hybrid networking.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **network groups**
  - **Technical definition:** network groups is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **connectivity configurations**
  - **Technical definition:** connectivity configurations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **security admin configurations**
  - **Technical definition:** security admin configurations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **virtual hubs**
  - **Technical definition:** virtual hubs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **hub route tables**
  - **Technical definition:** hub route tables is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **connections**
  - **Technical definition:** connections is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **secured virtual hubs**
  - **Technical definition:** secured virtual hubs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.
- **routing intent**
  - **Technical definition:** routing intent is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a migration cutover is paused until effective routes, NSG evaluation, and private endpoint DNS all confirm the app can reach the database privately.

### Azure Firewall, Azure Firewall Manager, Azure Bastion, and Azure NAT Gateway

- **Azure Firewall**
  - **Technical definition:** Azure Firewall is Azure's managed stateful network security service for L3-L7 filtering, DNAT, and policy-driven egress control.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **Azure Firewall Manager**
  - **Technical definition:** Azure Firewall Manager is Azure's orchestration layer for centrally managing Azure Firewall and associated security policy at scale.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **Azure Bastion**
  - **Technical definition:** Azure Bastion is Azure's managed browser-based remote access service for RDP and SSH into virtual machines without exposing public IPs on the VMs themselves.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **and Azure NAT Gateway**
  - **Technical definition:** and Azure NAT Gateway is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **firewalls**
  - **Technical definition:** Firewalls are Azure Firewall instances enforcing centrally managed network and application filtering.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **firewall policies**
  - **Technical definition:** Firewall policies are reusable policy objects containing rule collections, TLS inspection settings, and intelligence controls for Azure Firewall.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **rule collection groups**
  - **Technical definition:** Rule collection groups are ordered containers of Azure Firewall rule collections evaluated in priority order.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **application rules**
  - **Technical definition:** Application rules are FQDN- and protocol-aware outbound filtering rules, commonly for HTTP(S) destinations.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **network rules**
  - **Technical definition:** Network rules are IP-, port-, and protocol-based L3/L4 filtering rules.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **DNAT rules**
  - **Technical definition:** DNAT rules are Azure Firewall rules translating inbound public traffic to internal private destinations.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **public IP addresses**
  - **Technical definition:** Public IP addresses are Azure public endpoints assigned to services such as firewalls, bastion hosts, NAT gateways, or load balancers.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **bastion hosts**
  - **Technical definition:** Bastion hosts are Azure Bastion resources serving as secure browser-based access entry points into a VNet.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **NAT gateways**
  - **Technical definition:** NAT gateways are subnet-associated Azure resources providing scalable outbound SNAT using stable public IPs.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.
- **outbound IPs**
  - **Technical definition:** Outbound IPs are the public source addresses seen by external services for translated egress traffic.
  - **Concrete production example:** Example: an egress outage is traced to a new firewall rule collection group that blocked package repositories needed by VM scale set updates.

### Azure VPN Gateway and Azure ExpressRoute

- **Azure VPN Gateway**
  - **Technical definition:** Azure VPN Gateway is Azure's managed IPSec VPN service for site-to-site, point-to-site, and VNet-to-VNet encrypted connectivity.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **Azure ExpressRoute**
  - **Technical definition:** Azure ExpressRoute is Azure's dedicated private-connectivity service for linking customer networks to Microsoft backbone connectivity without relying on the public internet.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **virtual network gateways**
  - **Technical definition:** Virtual network gateways are Azure gateway resources providing VPN or ExpressRoute connectivity into a VNet.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **local network gateways**
  - **Technical definition:** Local network gateways are Azure representations of remote VPN devices and address spaces.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **VPN connections**
  - **Technical definition:** VPN connections are configured logical links between Azure VPN Gateway and another gateway or site.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **tunnels**
  - **Technical definition:** Tunnels are the encrypted IPSec paths carrying traffic between Azure and remote endpoints.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **ExpressRoute circuits**
  - **Technical definition:** ExpressRoute circuits are the connectivity and billing constructs representing dedicated private Azure network access.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **peerings**
  - **Technical definition:** Peerings are the ExpressRoute routing domains over which route exchange occurs.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **route filters**
  - **Technical definition:** Route filters are ExpressRoute controls specifying which Microsoft prefixes are advertised for supported peering scenarios.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **authorization keys**
  - **Technical definition:** Authorization keys are ExpressRoute permissions allowing a separate subscription to link to a circuit.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.
- **ExpressRoute gateways**
  - **Technical definition:** ExpressRoute gateways are Azure virtual network gateway resources enabling VNet access to an ExpressRoute circuit.
  - **Concrete production example:** Example: during DR rehearsal, BGP routes fail over from one ExpressRoute path to the VPN backup without breaking on-premises application traffic.

### Azure DNS, Azure Private DNS, and Azure DNS Private Resolver

- **Azure DNS**
  - **Technical definition:** Azure DNS is Azure's authoritative DNS hosting service for public zones and record management.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **Azure Private DNS**
  - **Technical definition:** Azure Private DNS is Azure's private DNS service for internal name resolution within linked virtual networks.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **and Azure DNS Private Resolver**
  - **Technical definition:** and Azure DNS Private Resolver is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **DNS zones**
  - **Technical definition:** DNS zones are Azure DNS namespace containers holding authoritative record sets for a domain.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **record sets**
  - **Technical definition:** Record sets are the grouped DNS answers for a specific name and type within a zone.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **private DNS zones**
  - **Technical definition:** Private DNS zones are Azure-hosted DNS zones resolvable only by linked virtual networks.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **virtual network links**
  - **Technical definition:** Virtual network links associate private DNS zones with VNets for resolution and optionally autoregistration.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **inbound endpoints**
  - **Technical definition:** Inbound endpoints are DNS Private Resolver interfaces receiving queries from hybrid or external sources into Azure resolution paths.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **outbound endpoints**
  - **Technical definition:** Outbound endpoints are DNS Private Resolver interfaces forwarding Azure-originated queries to external resolvers.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **rulesets**
  - **Technical definition:** Rulesets are forwarding-rule collections associated with one or more virtual networks.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.
- **forwarding rules**
  - **Technical definition:** Forwarding rules are suffix-based DNS decisions specifying which target resolver should receive matching queries.
  - **Concrete production example:** Example: a private endpoint rollout succeeds only after Private DNS links and DNS Private Resolver forwarding rules are validated from on-premises clients.

### Azure Load Balancer, Azure Application Gateway, Azure Front Door, Azure Traffic Manager, and Azure CDN

- **Azure Load Balancer**
  - **Technical definition:** Azure Load Balancer is Azure's Layer 4 traffic distribution service for TCP/UDP frontends and backend pool balancing.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **Azure Application Gateway**
  - **Technical definition:** Azure Application Gateway is Azure's Layer 7 load balancer for HTTP(S) routing, path-based routing, TLS termination, and WAF integration.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **Azure Front Door**
  - **Technical definition:** Azure Front Door is Azure's global edge service for anycast HTTP(S) acceleration, routing, WAF, and origin failover.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **Azure Traffic Manager**
  - **Technical definition:** Azure Traffic Manager is Azure's DNS-based traffic steering service for endpoint selection across regions and endpoints.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **and Azure CDN**
  - **Technical definition:** and Azure CDN is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **frontend IP configurations**
  - **Technical definition:** Frontend IP configurations are the listener-side IP objects exposed by Azure load-balancing and edge services.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **backend pools**
  - **Technical definition:** Backend pools are the sets of compute instances or origins eligible to receive traffic.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **probes**
  - **Technical definition:** Probes are health-check definitions used to determine backend availability.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **load balancing rules**
  - **Technical definition:** Load balancing rules map frontend IP and port combinations to backend pools and health behavior in Azure Load Balancer.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **listeners**
  - **Technical definition:** Listeners are Application Gateway ingress definitions binding frontend, protocol, and port.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **routing rules**
  - **Technical definition:** Routing rules are Application Gateway or Front Door policies mapping requests to backend targets based on path, host, or other criteria.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **WAF policies**
  - **Technical definition:** WAF policies are rule sets controlling managed and custom application-layer filtering in Front Door or Application Gateway.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **Front Door profiles**
  - **Technical definition:** Front Door profiles are the top-level administrative containers for Azure Front Door endpoints, origins, and routing.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **endpoints**
  - **Technical definition:** Endpoints are externally exposed service entry points in Front Door, Traffic Manager, CDN, and related platforms.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **origins**
  - **Technical definition:** Origins are the backend applications or services from which Front Door or CDN retrieves content.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **origin groups**
  - **Technical definition:** Origin groups are grouped backend origin sets with health and load-balancing behavior.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **Traffic Manager profiles**
  - **Technical definition:** Traffic Manager profiles are DNS traffic-routing policies defining endpoint selection strategy and health checks.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.
- **CDN endpoints**
  - **Technical definition:** CDN endpoints are cache-enabled delivery points from which Azure CDN serves content.
  - **Concrete production example:** Example: Front Door shifts traffic to a healthy origin group after health probes fail in one region during a planned maintenance window.

### Azure DDoS Protection, Azure Web Application Firewall, and Azure Network Watcher

- **Azure DDoS Protection**
  - **Technical definition:** Azure DDoS Protection is Azure's managed DDoS mitigation service for protecting public IP resources from volumetric and protocol attacks.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **Azure Web Application Firewall**
  - **Technical definition:** Azure Web Application Firewall is Azure's application-layer request inspection and filtering capability, typically attached to Application Gateway or Front Door.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **and Azure Network Watcher**
  - **Technical definition:** and Azure Network Watcher is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **DDoS protection plans**
  - **Technical definition:** DDoS protection plans is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **WAF policies**
  - **Technical definition:** WAF policies is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **packet capture**
  - **Technical definition:** packet capture is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **connection monitor**
  - **Technical definition:** connection monitor is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **flow logs**
  - **Technical definition:** flow logs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **effective routes**
  - **Technical definition:** effective routes is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **topology**
  - **Technical definition:** topology is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **IP flow verify**
  - **Technical definition:** IP flow verify is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.

### Azure API Management

- **Azure API Management**
  - **Technical definition:** Azure API Management is Azure's API gateway and API governance platform for publishing, securing, versioning, and monitoring APIs.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **API Management instances**
  - **Technical definition:** API Management instances is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **APIs**
  - **Technical definition:** APIs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **products**
  - **Technical definition:** products is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **subscriptions**
  - **Technical definition:** subscriptions is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **policies**
  - **Technical definition:** policies is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **backends**
  - **Technical definition:** backends is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **gateways**
  - **Technical definition:** gateways is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **revisions**
  - **Technical definition:** revisions is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **versions**
  - **Technical definition:** versions is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.
- **developer portal**
  - **Technical definition:** developer portal is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an external partner API is published through API Management with rate limits and revision testing before the new backend goes live.

## Controls

### Expanded Points

- **Virtual network CIDR planning**
  - **Technical definition:** Virtual network CIDR planning is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **subnet delegation**
  - **Technical definition:** subnet delegation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **route table association**
  - **Technical definition:** route table association is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **NSG rules**
  - **Technical definition:** NSG rules is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **application security groups**
  - **Technical definition:** application security groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Private Link approval**
  - **Technical definition:** Private Link approval is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Virtual Network Manager connectivity/security configurations**
  - **Technical definition:** Virtual Network Manager connectivity/security configurations is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Virtual WAN hub routing**
  - **Technical definition:** Virtual WAN hub routing is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **secured hub policies**
  - **Technical definition:** secured hub policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **routing intent**
  - **Technical definition:** routing intent is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure Firewall policy hierarchy**
  - **Technical definition:** Azure Firewall policy hierarchy is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rule collection groups**
  - **Technical definition:** rule collection groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **egress filtering**
  - **Technical definition:** egress filtering is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **DNAT**
  - **Technical definition:** DNAT is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **threat intelligence mode**
  - **Technical definition:** threat intelligence mode is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Bastion access**
  - **Technical definition:** Bastion access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **NAT Gateway outbound design**
  - **Technical definition:** NAT Gateway outbound design is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **VPN Gateway tunnel settings**
  - **Technical definition:** VPN Gateway tunnel settings is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **BGP**
  - **Technical definition:** BGP is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **active-active mode**
  - **Technical definition:** active-active mode is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **ExpressRoute peering**
  - **Technical definition:** ExpressRoute peering is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **route filters**
  - **Technical definition:** route filters is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private peering**
  - **Technical definition:** private peering is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **gateway resiliency**
  - **Technical definition:** gateway resiliency is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Private DNS zone linking**
  - **Technical definition:** Private DNS zone linking is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **DNS Private Resolver forwarding rules**
  - **Technical definition:** DNS Private Resolver forwarding rules is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **split-horizon DNS**
  - **Technical definition:** split-horizon DNS is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **DNS cutover controls**
  - **Technical definition:** DNS cutover controls is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Load balancer probes**
  - **Technical definition:** Load balancer probes is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Application Gateway listeners**
  - **Technical definition:** Application Gateway listeners is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **WAF policy**
  - **Technical definition:** WAF policy is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Front Door origin groups**
  - **Technical definition:** Front Door origin groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Traffic Manager routing methods**
  - **Technical definition:** Traffic Manager routing methods is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **TLS certificates**
  - **Technical definition:** TLS certificates is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Network Watcher flow logs**
  - **Technical definition:** Network Watcher flow logs is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **packet capture**
  - **Technical definition:** packet capture is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **connection monitor**
  - **Technical definition:** connection monitor is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **effective routes**
  - **Technical definition:** effective routes is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **DDoS/WAF protection policies**
  - **Technical definition:** DDoS/WAF protection policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **API Management authentication**
  - **Technical definition:** API Management authentication is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **authorization policies**
  - **Technical definition:** authorization policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rate limits**
  - **Technical definition:** rate limits is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **backends**
  - **Technical definition:** backends is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **revisions**
  - **Technical definition:** revisions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **versioning**
  - **Technical definition:** versioning is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

## Operations

### Expanded Points

- **Validate packet paths with effective routes**
  - **Technical definition:** Validate packet paths with effective routes is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **NSG rules**
  - **Technical definition:** NSG rules is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **IP flow verify**
  - **Technical definition:** IP flow verify is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Connection Monitor**
  - **Technical definition:** Connection Monitor is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **flow logs**
  - **Technical definition:** flow logs is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Troubleshoot private endpoint DNS**
  - **Technical definition:** Troubleshoot private endpoint DNS is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Private Link approval**
  - **Technical definition:** Private Link approval is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **hybrid name resolution**
  - **Technical definition:** hybrid name resolution is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Review Azure Firewall rule hits**
  - **Technical definition:** Review Azure Firewall rule hits is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **NAT Gateway port exhaustion**
  - **Technical definition:** NAT Gateway port exhaustion is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **egress paths**
  - **Technical definition:** egress paths is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **blocked traffic**
  - **Technical definition:** blocked traffic is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Test VPN Gateway failover**
  - **Technical definition:** Test VPN Gateway failover is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **ExpressRoute routing**
  - **Technical definition:** ExpressRoute routing is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **hybrid BGP route propagation**
  - **Technical definition:** hybrid BGP route propagation is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Execute DNS cutover with Azure DNS**
  - **Technical definition:** Execute DNS cutover with Azure DNS is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Private DNS**
  - **Technical definition:** Private DNS is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **DNS Private Resolver**
  - **Technical definition:** DNS Private Resolver is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rollback TTL planning**
  - **Technical definition:** rollback TTL planning is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Review Application Gateway/Front Door health probes**
  - **Technical definition:** Review Application Gateway/Front Door health probes is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **WAF false positives**
  - **Technical definition:** WAF false positives is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **origin failover**
  - **Technical definition:** origin failover is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **traffic routing**
  - **Technical definition:** traffic routing is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Deploy API Management APIs**
  - **Technical definition:** Deploy API Management APIs is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **policies**
  - **Technical definition:** policies is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **backends**
  - **Technical definition:** backends is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **revisions**
  - **Technical definition:** revisions is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **troubleshoot gateway access logs**
  - **Technical definition:** troubleshoot gateway access logs is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

# Azure Week 3: Compute, Azure Kubernetes Service, DevOps, And Platform Delivery

## Service And Entity Coverage

### Azure Virtual Machines and Azure Virtual Machine Scale Sets

- **Azure Virtual Machines**
  - **Technical definition:** Azure Virtual Machines is Azure's IaaS compute service for customer-managed operating systems and workload runtime environments.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **Azure Virtual Machine Scale Sets**
  - **Technical definition:** Azure Virtual Machine Scale Sets are Azure's fleet orchestration service for scaling and updating groups of homogeneous virtual machines.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **virtual machines**
  - **Technical definition:** Virtual machines are running Azure IaaS compute instances hosting customer-managed operating systems and applications.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **VM sizes**
  - **Technical definition:** VM sizes are Azure compute shape definitions specifying vCPU, memory, storage, networking, and accelerator profile.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **images**
  - **Technical definition:** Images are reusable VM templates used to provision virtual machines.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **disks**
  - **Technical definition:** Disks are managed block-storage devices attached to Azure virtual machines.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **network interfaces**
  - **Technical definition:** Network interfaces are NIC resources connecting VMs to virtual networks, IP configurations, and NSG scope.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **availability sets**
  - **Technical definition:** Availability sets are placement constructs reducing correlated failure by distributing VMs across fault and update domains.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **availability zones**
  - **Technical definition:** Availability zones are physically separate datacenter zones within a region used to improve resilience.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **scale sets**
  - **Technical definition:** Scale sets are Azure fleet constructs for homogeneous VM deployment, scaling, and rolling upgrades.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **upgrade policies**
  - **Technical definition:** Upgrade policies define how VM Scale Sets apply model changes across instances.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **health probes**
  - **Technical definition:** Health probes are checks used by load balancers or scale sets to determine instance availability.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **extensions**
  - **Technical definition:** Extensions are post-deployment agents or packages adding configuration, monitoring, security, or application behavior to VMs.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **custom script extensions**
  - **Technical definition:** Custom Script Extensions are VM extensions used to run customer-provided scripts after deployment.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.
- **managed identities**
  - **Technical definition:** Managed identities are Azure-managed principals attached to compute resources for secretless API access.
  - **Concrete production example:** Example: a VM scale set rolls to a new image version and unhealthy instances are automatically replaced based on the configured health probe.

### Azure Kubernetes Service (AKS)

- **Azure Kubernetes Service (AKS)**
  - **Technical definition:** Azure Kubernetes Service is Azure's managed Kubernetes platform with Azure-integrated identity, networking, policy, and cluster lifecycle features.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **clusters**
  - **Technical definition:** Clusters are AKS-managed Kubernetes environments consisting of a managed control plane and one or more node pools.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **node pools**
  - **Technical definition:** Node pools are groups of AKS nodes sharing VM type, scaling settings, and operational role.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **nodes**
  - **Technical definition:** Nodes are the worker VMs executing Kubernetes pods in an AKS cluster.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **Kubernetes versions**
  - **Technical definition:** Kubernetes versions are the supported control-plane and node-pool versions available for AKS lifecycle management.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **system node pools**
  - **Technical definition:** System node pools are node pools intended to host critical cluster system components and platform workloads.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **user node pools**
  - **Technical definition:** User node pools are node pools intended primarily for tenant or application workloads.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **managed identities**
  - **Technical definition:** Managed identities are Azure-managed identities used by AKS control-plane or node resources to call Azure APIs.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **workload identity**
  - **Technical definition:** Workload identity is AKS's federated mapping from Kubernetes service accounts to Entra application identities without pod secrets.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **ingress controllers**
  - **Technical definition:** Ingress controllers are the components implementing HTTP(S) routing and ingress policy for AKS services.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **network policies**
  - **Technical definition:** Network policies are pod-to-pod and namespace-level traffic rules enforced by the selected AKS networking stack.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **Azure Policy add-on**
  - **Technical definition:** Azure Policy add-on applies Azure governance and admission-style checks to Kubernetes resources and AKS cluster settings.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.
- **cluster audit logs**
  - **Technical definition:** Cluster audit logs are records of Kubernetes API actions and control-plane operations in AKS.
  - **Concrete production example:** Example: an AKS upgrade is approved only after node-pool rollout, workload identity access, and ingress health checks all pass in staging.

### Azure App Service, Azure Functions, Azure Container Apps, and Azure Container Instances

- **Azure App Service**
  - **Technical definition:** Azure App Service is Azure's managed web application hosting platform for web apps, APIs, and background services on managed compute plans.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **Azure Functions**
  - **Technical definition:** Azure Functions is Azure's event-driven serverless compute service for function-based code execution under managed scaling.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **Azure Container Apps**
  - **Technical definition:** Azure Container Apps is Azure's serverless container platform for microservices and jobs with revision-based deployment and managed scaling.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **and Azure Container Instances**
  - **Technical definition:** and Azure Container Instances is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **app services**
  - **Technical definition:** app services is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **app service plans**
  - **Technical definition:** app service plans is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **deployment slots**
  - **Technical definition:** deployment slots is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **function apps**
  - **Technical definition:** function apps is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **triggers**
  - **Technical definition:** triggers is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **bindings**
  - **Technical definition:** bindings is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **container apps**
  - **Technical definition:** container apps is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **revisions**
  - **Technical definition:** revisions is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **jobs**
  - **Technical definition:** jobs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **environments**
  - **Technical definition:** environments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **replicas**
  - **Technical definition:** replicas is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.
- **container groups**
  - **Technical definition:** container groups is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a new Container Apps revision receives 10% of traffic first so latency and secret-loading behavior can be validated before full promotion.

### Azure Container Registry and Microsoft Defender for Cloud container signals

- **Azure Container Registry**
  - **Technical definition:** Azure Container Registry is Azure's managed OCI-compatible registry for container images and artifacts under Azure identity and network controls.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **Microsoft Defender for Cloud container signals**
  - **Technical definition:** Microsoft Defender for Cloud container signals are the security findings, vulnerability assessments, and runtime posture signals produced for containerized workloads in Azure.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **registries**
  - **Technical definition:** registries is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **repositories**
  - **Technical definition:** repositories is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **images**
  - **Technical definition:** images is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **tags**
  - **Technical definition:** tags is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **tasks**
  - **Technical definition:** tasks is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **webhooks**
  - **Technical definition:** webhooks is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **content trust/signing patterns**
  - **Technical definition:** content trust/signing patterns is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.
- **vulnerability findings**
  - **Technical definition:** vulnerability findings is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a production AKS deployment pulls only images from a private-endpoint-enabled registry after vulnerability findings are reviewed.

### Azure DevOps and GitHub Actions on Azure

- **Azure DevOps and GitHub Actions on Azure**
  - **Technical definition:** Azure DevOps and GitHub Actions on Azure is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **projects**
  - **Technical definition:** projects is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **repositories**
  - **Technical definition:** repositories is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **pipelines**
  - **Technical definition:** pipelines is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **environments**
  - **Technical definition:** environments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **service connections**
  - **Technical definition:** service connections is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **variable groups**
  - **Technical definition:** variable groups is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **agents**
  - **Technical definition:** agents is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **workflow runs**
  - **Technical definition:** workflow runs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **secrets**
  - **Technical definition:** secrets is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **environments**
  - **Technical definition:** environments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.
- **deployment approvals**
  - **Technical definition:** deployment approvals is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a release pipeline pauses on an approval gate before deploying infrastructure and app changes into the production subscription.

### Azure Resource Manager templates, Azure Bicep, and Azure Deployment Environments

- **Azure Resource Manager templates**
  - **Technical definition:** ARM Templates are Azure's JSON-based declarative infrastructure-as-code format for describing resource deployments.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **Azure Bicep**
  - **Technical definition:** Azure Bicep is Azure's higher-level declarative IaC language that compiles into ARM templates and simplifies modular Azure resource definitions.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **and Azure Deployment Environments**
  - **Technical definition:** and Azure Deployment Environments is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **ARM templates**
  - **Technical definition:** ARM templates is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **Bicep files**
  - **Technical definition:** Bicep files is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **modules**
  - **Technical definition:** modules is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **parameters**
  - **Technical definition:** parameters is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **outputs**
  - **Technical definition:** outputs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **deployments**
  - **Technical definition:** deployments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **environment definitions**
  - **Technical definition:** environment definitions is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **catalogs**
  - **Technical definition:** catalogs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **projects**
  - **Technical definition:** projects is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.
- **environments**
  - **Technical definition:** environments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Bicep what-if review catches an unintended subnet replacement before deployment reaches a shared hub network.

### Azure Batch

- **Azure Batch**
  - **Technical definition:** Azure Batch is Azure's managed batch-workload scheduling service for large numbers of compute-intensive jobs and tasks.
  - **Concrete production example:** Example: a genomics workload uses autoscale formulas so Batch nodes expand for a compute surge and then contract after job completion.
- **pools**
  - **Technical definition:** pools is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a genomics workload uses autoscale formulas so Batch nodes expand for a compute surge and then contract after job completion.
- **compute nodes**
  - **Technical definition:** compute nodes is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a genomics workload uses autoscale formulas so Batch nodes expand for a compute surge and then contract after job completion.
- **jobs**
  - **Technical definition:** jobs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a genomics workload uses autoscale formulas so Batch nodes expand for a compute surge and then contract after job completion.
- **tasks**
  - **Technical definition:** tasks is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a genomics workload uses autoscale formulas so Batch nodes expand for a compute surge and then contract after job completion.
- **application packages**
  - **Technical definition:** application packages is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a genomics workload uses autoscale formulas so Batch nodes expand for a compute surge and then contract after job completion.
- **start tasks**
  - **Technical definition:** start tasks is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a genomics workload uses autoscale formulas so Batch nodes expand for a compute surge and then contract after job completion.
- **autoscale formulas**
  - **Technical definition:** autoscale formulas is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a genomics workload uses autoscale formulas so Batch nodes expand for a compute surge and then contract after job completion.

### Azure Machine Learning and Azure AI Foundry

- **Azure Machine Learning**
  - **Technical definition:** Azure Machine Learning is Azure's managed ML platform for training, tracking, packaging, deploying, and monitoring machine learning workloads.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **Azure AI Foundry**
  - **Technical definition:** Azure AI Foundry is Microsoft's AI application and model-development workspace experience for orchestrating AI assets, prompts, evaluations, and deployments.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **workspaces**
  - **Technical definition:** workspaces is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **compute instances**
  - **Technical definition:** compute instances is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **compute clusters**
  - **Technical definition:** compute clusters is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **environments**
  - **Technical definition:** environments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **data assets**
  - **Technical definition:** data assets is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **datastores**
  - **Technical definition:** datastores is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **jobs**
  - **Technical definition:** jobs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **pipelines**
  - **Technical definition:** pipelines is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **model registries**
  - **Technical definition:** model registries is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **managed online endpoints**
  - **Technical definition:** managed online endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **batch endpoints**
  - **Technical definition:** batch endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **prompt flows**
  - **Technical definition:** prompt flows is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **hubs**
  - **Technical definition:** hubs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **projects**
  - **Technical definition:** projects is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **connections**
  - **Technical definition:** connections is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.

### Azure OpenAI Service

- **Azure OpenAI Service**
  - **Technical definition:** Azure OpenAI Service is Azure's managed hosted interface for OpenAI models with enterprise network, identity, quota, and content-safety controls.
  - **Concrete production example:** Example: an Azure OpenAI deployment is kept behind a private endpoint and diagnostic logs are reviewed after content-filter exceptions spike.
- **Azure OpenAI resources**
  - **Technical definition:** Azure OpenAI resources is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an Azure OpenAI deployment is kept behind a private endpoint and diagnostic logs are reviewed after content-filter exceptions spike.
- **deployments**
  - **Technical definition:** deployments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an Azure OpenAI deployment is kept behind a private endpoint and diagnostic logs are reviewed after content-filter exceptions spike.
- **models**
  - **Technical definition:** models is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an Azure OpenAI deployment is kept behind a private endpoint and diagnostic logs are reviewed after content-filter exceptions spike.
- **content filters**
  - **Technical definition:** content filters is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an Azure OpenAI deployment is kept behind a private endpoint and diagnostic logs are reviewed after content-filter exceptions spike.
- **quotas**
  - **Technical definition:** quotas is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an Azure OpenAI deployment is kept behind a private endpoint and diagnostic logs are reviewed after content-filter exceptions spike.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an Azure OpenAI deployment is kept behind a private endpoint and diagnostic logs are reviewed after content-filter exceptions spike.
- **diagnostic settings**
  - **Technical definition:** diagnostic settings is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an Azure OpenAI deployment is kept behind a private endpoint and diagnostic logs are reviewed after content-filter exceptions spike.

## Controls

### Expanded Points

- **VM managed identity**
  - **Technical definition:** VM managed identity is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **disk encryption**
  - **Technical definition:** disk encryption is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **VM extensions**
  - **Technical definition:** VM extensions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **patch settings**
  - **Technical definition:** patch settings is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **scale set upgrade policy**
  - **Technical definition:** scale set upgrade policy is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **health probes**
  - **Technical definition:** health probes is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **zone placement**
  - **Technical definition:** zone placement is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **AKS private clusters**
  - **Technical definition:** AKS private clusters is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **authorized IP ranges**
  - **Technical definition:** authorized IP ranges is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **managed identities**
  - **Technical definition:** managed identities is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **workload identity**
  - **Technical definition:** workload identity is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **network policy**
  - **Technical definition:** network policy is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Policy add-on**
  - **Technical definition:** Azure Policy add-on is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **upgrade channels**
  - **Technical definition:** upgrade channels is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **node pool isolation**
  - **Technical definition:** node pool isolation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **App Service access restrictions**
  - **Technical definition:** App Service access restrictions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **deployment slots**
  - **Technical definition:** deployment slots is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **managed identities**
  - **Technical definition:** managed identities is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **VNet integration**
  - **Technical definition:** VNet integration is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Functions identity**
  - **Technical definition:** Functions identity is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Container Apps ingress**
  - **Technical definition:** Container Apps ingress is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **revision traffic splitting**
  - **Technical definition:** revision traffic splitting is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **secrets**
  - **Technical definition:** secrets is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure Container Registry private endpoints**
  - **Technical definition:** Azure Container Registry private endpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **repository access**
  - **Technical definition:** repository access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **image scanning**
  - **Technical definition:** image scanning is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **signing patterns**
  - **Technical definition:** signing patterns is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **retention**
  - **Technical definition:** retention is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **quarantine workflow**
  - **Technical definition:** quarantine workflow is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure DevOps service connections**
  - **Technical definition:** Azure DevOps service connections is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **approvals**
  - **Technical definition:** approvals is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **protected environments**
  - **Technical definition:** protected environments is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **variable groups**
  - **Technical definition:** variable groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **secret management**
  - **Technical definition:** secret management is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **agent pool isolation**
  - **Technical definition:** agent pool isolation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **ARM/Bicep parameter controls**
  - **Technical definition:** ARM/Bicep parameter controls is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **module reuse**
  - **Technical definition:** module reuse is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **what-if review**
  - **Technical definition:** what-if review is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **deployment scopes**
  - **Technical definition:** deployment scopes is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **deployment stacks**
  - **Technical definition:** deployment stacks is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **environment catalog governance**
  - **Technical definition:** environment catalog governance is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure Machine Learning workspace private networking**
  - **Technical definition:** Azure Machine Learning workspace private networking is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **managed identities**
  - **Technical definition:** managed identities is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **compute isolation**
  - **Technical definition:** compute isolation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **model approval**
  - **Technical definition:** model approval is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **endpoint access**
  - **Technical definition:** endpoint access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **data access controls**
  - **Technical definition:** data access controls is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure OpenAI content filters**
  - **Technical definition:** Azure OpenAI content filters is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private networking**
  - **Technical definition:** private networking is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **managed identity**
  - **Technical definition:** managed identity is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **diagnostic settings**
  - **Technical definition:** diagnostic settings is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **quota governance**
  - **Technical definition:** quota governance is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

## Operations

### Expanded Points

- **Roll out VM image updates**
  - **Technical definition:** Roll out VM image updates is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **scale set upgrades**
  - **Technical definition:** scale set upgrades is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **health probe changes**
  - **Technical definition:** health probe changes is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **extension remediation**
  - **Technical definition:** extension remediation is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Upgrade AKS clusters/node pools**
  - **Technical definition:** Upgrade AKS clusters/node pools is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **troubleshoot pod scheduling**
  - **Technical definition:** troubleshoot pod scheduling is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **workload identity**
  - **Technical definition:** workload identity is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **ingress**
  - **Technical definition:** ingress is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **image pulls**
  - **Technical definition:** image pulls is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rollout health**
  - **Technical definition:** rollout health is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Deploy App Service slots**
  - **Technical definition:** Deploy App Service slots is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Functions**
  - **Technical definition:** Functions is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Container Apps revisions**
  - **Technical definition:** Container Apps revisions is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **traffic splits**
  - **Technical definition:** traffic splits is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rollback failed revisions**
  - **Technical definition:** rollback failed revisions is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Promote images through Azure Container Registry with vulnerability review and signing controls**
  - **Technical definition:** Promote images through Azure Container Registry with vulnerability review and signing controls is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Run Azure DevOps/GitHub Actions pipelines with approvals**
  - **Technical definition:** Run Azure DevOps/GitHub Actions pipelines with approvals is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **deployment environments**
  - **Technical definition:** deployment environments is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rollback**
  - **Technical definition:** rollback is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **artifact promotion**
  - **Technical definition:** artifact promotion is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Review ARM/Bicep what-if output**
  - **Technical definition:** Review ARM/Bicep what-if output is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **deploy infrastructure**
  - **Technical definition:** deploy infrastructure is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **troubleshoot drift**
  - **Technical definition:** troubleshoot drift is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rollback failed deployments**
  - **Technical definition:** rollback failed deployments is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Run Azure Machine Learning training**
  - **Technical definition:** Run Azure Machine Learning training is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **pipeline execution**
  - **Technical definition:** pipeline execution is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **model registration**
  - **Technical definition:** model registration is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **endpoint deployment**
  - **Technical definition:** endpoint deployment is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **endpoint troubleshooting**
  - **Technical definition:** endpoint troubleshooting is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

# Azure Week 4: Data, Storage, Backup, And Stateful Reliability

## Service And Entity Coverage

### Azure Storage and Azure Data Lake Storage Gen2

- **Azure Storage**
  - **Technical definition:** Azure Storage is Azure's general-purpose storage platform covering blobs, files, queues, tables, and account-level durability and access controls.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **Azure Data Lake Storage Gen2**
  - **Technical definition:** Azure Data Lake Storage Gen2 is the hierarchical-namespace-enabled data lake capability built on Azure Blob Storage for analytics and file-style access.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **storage accounts**
  - **Technical definition:** Storage accounts are the top-level administrative resources holding Azure Storage services and durability/network configuration.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **containers**
  - **Technical definition:** Containers are Blob Storage namespace containers holding blob objects.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **blobs**
  - **Technical definition:** Blobs are object data entities stored in Azure Blob Storage.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **access tiers**
  - **Technical definition:** Access tiers are blob cost and retrieval profiles such as hot, cool, or archive.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **lifecycle management policies**
  - **Technical definition:** Lifecycle management policies are rule-based automations transitioning or deleting blobs based on age and conditions.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **object replication**
  - **Technical definition:** Object replication is asynchronous container-level blob replication between source and destination storage accounts.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **immutable storage**
  - **Technical definition:** Immutable storage is Azure Storage's WORM capability preventing modification or deletion during retention or legal hold.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **versioning**
  - **Technical definition:** Versioning preserves previous blob versions after writes or deletes.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **snapshots**
  - **Technical definition:** Snapshots are point-in-time read-only versions of blob or disk data.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **file shares**
  - **Technical definition:** File shares are Azure Files shared storage constructs inside a storage account.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **queues**
  - **Technical definition:** Queues are Azure Storage queue entities used for simple asynchronous messaging.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **tables**
  - **Technical definition:** Tables are Azure Table Storage NoSQL entities under a storage account.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **hierarchical namespace**
  - **Technical definition:** Hierarchical namespace is ADLS Gen2's capability enabling directory semantics and POSIX-style ACLs over blob storage.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **directories**
  - **Technical definition:** Directories are path structures inside hierarchical-namespace-enabled storage.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **access control lists**
  - **Technical definition:** Access control lists are POSIX-like permission entries governing path access in ADLS Gen2.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.

### Azure managed disks, Azure Files, and Azure NetApp Files

- **Azure managed disks**
  - **Technical definition:** Azure Managed Disks is Azure's managed block storage platform for virtual machine disks with performance tiers and snapshot support.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **Azure Files**
  - **Technical definition:** Azure Files is Azure's managed SMB/NFS file share service for shared file access from cloud and hybrid workloads.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **and Azure NetApp Files**
  - **Technical definition:** and Azure NetApp Files is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **managed disks**
  - **Technical definition:** managed disks is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **snapshots**
  - **Technical definition:** snapshots is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **disk encryption sets**
  - **Technical definition:** disk encryption sets is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **shared disks**
  - **Technical definition:** shared disks is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **file shares**
  - **Technical definition:** file shares is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **NetApp accounts**
  - **Technical definition:** NetApp accounts is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **capacity pools**
  - **Technical definition:** capacity pools is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **volumes**
  - **Technical definition:** volumes is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **snapshots**
  - **Technical definition:** snapshots is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.
- **backup policies**
  - **Technical definition:** backup policies is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional outage drill verifies disk snapshot restore, Azure Files access, and NetApp volume recovery before a storage migration wave starts.

### Azure SQL Database, Azure SQL Managed Instance, Azure Database for PostgreSQL, Azure Database for MySQL, and AlloyDB-equivalent migration considerations

- **Azure SQL Database**
  - **Technical definition:** Azure SQL Database is Azure's managed PaaS relational database service for SQL Server-compatible workloads at database scope.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **Azure SQL Managed Instance**
  - **Technical definition:** Azure SQL Managed Instance is Azure's managed SQL Server-compatible platform with broader instance-level compatibility and VNet-integrated deployment.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **Azure Database for PostgreSQL**
  - **Technical definition:** Azure Database for PostgreSQL is Azure's managed PostgreSQL platform with managed backups, HA options, and Azure network integration.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **Azure Database for MySQL**
  - **Technical definition:** Azure Database for MySQL is Azure's managed MySQL platform for relational workloads with managed operations and backup controls.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **and AlloyDB-equivalent migration considerations**
  - **Technical definition:** and AlloyDB-equivalent migration considerations is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **logical servers**
  - **Technical definition:** Logical servers are Azure SQL administrative parents holding databases and server-scoped configuration.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **databases**
  - **Technical definition:** Databases are the logical relational data stores hosted by Azure SQL, PostgreSQL, or MySQL services.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **elastic pools**
  - **Technical definition:** Elastic pools are Azure SQL shared compute pools that distribute capacity across multiple databases.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **managed instances**
  - **Technical definition:** Managed instances are SQL Managed Instance deployments exposing broader SQL Server compatibility inside a VNet.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **failover groups**
  - **Technical definition:** Failover groups are cross-region failover constructs providing listener endpoints and disaster recovery sequencing.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **backups**
  - **Technical definition:** Backups are managed retained database copies and transaction-log chains used for restore.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **private endpoints**
  - **Technical definition:** Private endpoints are private IP exposures of database services into a VNet through Private Link.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **firewall rules**
  - **Technical definition:** Firewall rules are service-level IP allowlist controls governing database connectivity over public endpoints.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **read replicas**
  - **Technical definition:** Read replicas are secondary read-only copies used for read scale or disaster recovery.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.
- **point-in-time restore**
  - **Technical definition:** Point-in-time restore is the ability to restore a database to a selected earlier timestamp.
  - **Concrete production example:** Example: a database cutover is approved only after failover group testing and point-in-time restore prove the recovery path works.

### Azure Cosmos DB

- **Azure Cosmos DB**
  - **Technical definition:** Azure Cosmos DB is Azure's globally distributed NoSQL database service with configurable consistency, partitioned scale, and multi-region topology support.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **accounts**
  - **Technical definition:** Accounts are the top-level Cosmos DB resources controlling API type, consistency, regions, and billing model.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **databases**
  - **Technical definition:** Databases are logical namespaces grouping one or more containers in Cosmos DB.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **containers**
  - **Technical definition:** Containers are the main scalable storage and throughput units in Cosmos DB.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **partition keys**
  - **Technical definition:** Partition keys are item attributes used to distribute data and request units across logical and physical partitions.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **items**
  - **Technical definition:** Items are the stored JSON-like records in a Cosmos DB container.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **consistency levels**
  - **Technical definition:** Consistency levels are Cosmos DB's tunable read consistency models trading latency against staleness guarantees.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **regions**
  - **Technical definition:** Regions are the Azure geographies hosting Cosmos DB replicas for locality and resilience.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **multi-region writes**
  - **Technical definition:** Multi-region writes allow multiple writable Cosmos DB regions at the same time.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **change feed**
  - **Technical definition:** Change feed is the ordered stream of inserts and updates emitted from a Cosmos DB container.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **indexing policies**
  - **Technical definition:** Indexing policies define which paths are indexed and how query behavior is supported.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **throughput**
  - **Technical definition:** Throughput is the allocated request-unit capacity assigned to Cosmos DB resources.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **autoscale**
  - **Technical definition:** Autoscale is the throughput mode that adjusts RU/s automatically up to a defined maximum.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.
- **backups**
  - **Technical definition:** Backups are retained copies of Cosmos DB data according to the selected backup mode and retention settings.
  - **Concrete production example:** Example: a hot partition caused by a poor partition key is identified after RU throttling spikes on one logical key range in production.

### Azure Cache for Redis

- **Azure Cache for Redis**
  - **Technical definition:** Azure Cache for Redis is Azure's managed Redis-compatible in-memory data service for caching, session state, and low-latency data access.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.
- **caches**
  - **Technical definition:** caches is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.
- **clustering**
  - **Technical definition:** clustering is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.
- **replicas**
  - **Technical definition:** replicas is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.
- **persistence**
  - **Technical definition:** persistence is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.
- **access keys**
  - **Technical definition:** access keys is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.
- **firewall rules**
  - **Technical definition:** firewall rules is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a maintenance event is tested to ensure applications reconnect cleanly to the cache and session behavior remains stable.

### Azure Event Hubs, Azure Service Bus, and Azure Event Grid

- **Azure Event Hubs**
  - **Technical definition:** Azure Event Hubs is Azure's high-throughput streaming ingestion platform for event and telemetry pipelines.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **Azure Service Bus**
  - **Technical definition:** Azure Service Bus is Azure's enterprise messaging broker for queues, topics, subscriptions, sessions, and durable message handling.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **and Azure Event Grid**
  - **Technical definition:** and Azure Event Grid is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **namespaces**
  - **Technical definition:** Namespaces are the top-level administrative containers for Event Hubs or Service Bus entities.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **event hubs**
  - **Technical definition:** Event hubs are partitioned streaming ingress entities receiving high-throughput event data.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **consumer groups**
  - **Technical definition:** Consumer groups are independent reader positions over an Event Hubs stream.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **partitions**
  - **Technical definition:** Partitions are ordered parallel segments of event stream storage and consumption.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **capture**
  - **Technical definition:** Capture is Event Hubs' feature for automatically archiving streaming data to storage destinations.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **queues**
  - **Technical definition:** Queues are Service Bus entities providing point-to-point durable message delivery.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **topics**
  - **Technical definition:** Topics are Service Bus publish-subscribe entities receiving messages for fan-out.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **subscriptions**
  - **Technical definition:** Subscriptions are Service Bus entities attached to a topic for filtered independent delivery.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **dead-letter queues**
  - **Technical definition:** Dead-letter queues are message holding areas for undeliverable, expired, or rejected messages.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **events**
  - **Technical definition:** Events are the payload notifications routed by Event Grid from publishers to subscribers.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **event subscriptions**
  - **Technical definition:** Event subscriptions are Event Grid routing definitions connecting event sources to handlers.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **domains**
  - **Technical definition:** Domains are Event Grid top-level multi-topic grouping constructs for large publisher estates.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.
- **system topics**
  - **Technical definition:** System topics are Azure-managed Event Grid topics automatically created for supported Azure sources.
  - **Concrete production example:** Example: a dead-letter queue reveals malformed messages that would otherwise block downstream processing in a critical order pipeline.

### Azure Data Factory, Azure Synapse Analytics, Azure Databricks, Azure Stream Analytics, Microsoft Fabric, and Microsoft OneLake

- **Azure Data Factory**
  - **Technical definition:** Azure Data Factory is Azure's managed data integration and orchestration service for ETL/ELT pipelines and data movement.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **Azure Synapse Analytics**
  - **Technical definition:** Azure Synapse Analytics is Azure's analytics workspace platform combining SQL, Spark, pipelines, and data integration patterns.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **Azure Databricks**
  - **Technical definition:** Azure Databricks is Azure's managed Databricks-based analytics platform for Spark, notebooks, ML, and lakehouse workloads.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **Azure Stream Analytics**
  - **Technical definition:** Azure Stream Analytics is Azure's managed real-time stream-processing service for SQL-like event processing over streaming data.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **Microsoft Fabric**
  - **Technical definition:** Microsoft Fabric is Microsoft's unified SaaS analytics platform spanning data engineering, warehousing, BI, and integrated lakehouse patterns.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **and Microsoft OneLake**
  - **Technical definition:** and Microsoft OneLake is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **factories**
  - **Technical definition:** factories is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **pipelines**
  - **Technical definition:** pipelines is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **datasets**
  - **Technical definition:** datasets is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **linked services**
  - **Technical definition:** linked services is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **triggers**
  - **Technical definition:** triggers is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **integration runtimes**
  - **Technical definition:** integration runtimes is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **Synapse workspaces**
  - **Technical definition:** Synapse workspaces is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **SQL pools**
  - **Technical definition:** SQL pools is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **Spark pools**
  - **Technical definition:** Spark pools is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **Databricks workspaces**
  - **Technical definition:** Databricks workspaces is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **clusters**
  - **Technical definition:** clusters is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **jobs**
  - **Technical definition:** jobs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **streams**
  - **Technical definition:** streams is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **lakehouses**
  - **Technical definition:** lakehouses is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **warehouses**
  - **Technical definition:** warehouses is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.
- **shortcuts**
  - **Technical definition:** shortcuts is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Data Factory pipeline fails during cutover because the self-hosted integration runtime cannot reach the target network until firewall rules are fixed.

### Azure AI Search, Microsoft Purview, and data classification

- **Azure AI Search**
  - **Technical definition:** Azure AI Search is Azure's search and retrieval platform for indexing, searching, enrichment, and AI-assisted retrieval patterns.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **Microsoft Purview**
  - **Technical definition:** Microsoft Purview is Microsoft's data governance and catalog platform for scanning, classification, lineage, and policy-oriented data visibility.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **and data classification**
  - **Technical definition:** and data classification is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **search services**
  - **Technical definition:** search services is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **indexes**
  - **Technical definition:** indexes is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **indexers**
  - **Technical definition:** indexers is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **data sources**
  - **Technical definition:** data sources is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **skillsets**
  - **Technical definition:** skillsets is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **Purview accounts**
  - **Technical definition:** Purview accounts is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **collections**
  - **Technical definition:** collections is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **scans**
  - **Technical definition:** scans is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **classifications**
  - **Technical definition:** classifications is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **glossary terms**
  - **Technical definition:** glossary terms is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **lineage**
  - **Technical definition:** lineage is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **data map assets**
  - **Technical definition:** data map assets is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.

### Azure Key Vault and Azure Backup

- **Azure Key Vault**
  - **Technical definition:** Azure Key Vault is Azure's managed service for storing secrets, keys, and certificates under identity and network controls.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **Azure Backup**
  - **Technical definition:** Azure Backup is Azure's policy-driven backup service for protected workloads and recovery-point management.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **vaults**
  - **Technical definition:** vaults is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **keys**
  - **Technical definition:** keys is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **secrets**
  - **Technical definition:** secrets is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **certificates**
  - **Technical definition:** certificates is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **key versions**
  - **Technical definition:** key versions is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **access policies**
  - **Technical definition:** access policies is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **RBAC assignments**
  - **Technical definition:** RBAC assignments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **backup vaults**
  - **Technical definition:** backup vaults is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **Recovery Services vaults**
  - **Technical definition:** Recovery Services vaults is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **backup policies**
  - **Technical definition:** backup policies is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **recovery points**
  - **Technical definition:** recovery points is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **snapshots**
  - **Technical definition:** snapshots is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.
- **restore jobs**
  - **Technical definition:** restore jobs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a secret rotation is validated end to end by confirming dependent applications can still authenticate through the Key Vault private endpoint.

## Controls

### Expanded Points

- **Storage account public access prevention**
  - **Technical definition:** Storage account public access prevention is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **network rules**
  - **Technical definition:** network rules is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **shared access signatures**
  - **Technical definition:** shared access signatures is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **lifecycle policies**
  - **Technical definition:** lifecycle policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **immutable storage**
  - **Technical definition:** immutable storage is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **versioning**
  - **Technical definition:** versioning is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **replication**
  - **Technical definition:** replication is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **customer-managed keys**
  - **Technical definition:** customer-managed keys is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Disk encryption sets**
  - **Technical definition:** Disk encryption sets is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **snapshot policies**
  - **Technical definition:** snapshot policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Files identity-based access**
  - **Technical definition:** Azure Files identity-based access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure NetApp Files backup policies**
  - **Technical definition:** Azure NetApp Files backup policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **throughput/capacity controls**
  - **Technical definition:** throughput/capacity controls is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure SQL firewall rules**
  - **Technical definition:** Azure SQL firewall rules is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Microsoft Entra authentication**
  - **Technical definition:** Microsoft Entra authentication is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **transparent data encryption**
  - **Technical definition:** transparent data encryption is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **backup retention**
  - **Technical definition:** backup retention is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **failover groups**
  - **Technical definition:** failover groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **point-in-time restore**
  - **Technical definition:** point-in-time restore is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Cosmos DB partition-key design**
  - **Technical definition:** Cosmos DB partition-key design is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **consistency level**
  - **Technical definition:** consistency level is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **indexing policy**
  - **Technical definition:** indexing policy is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **multi-region writes**
  - **Technical definition:** multi-region writes is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **backup mode**
  - **Technical definition:** backup mode is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **throughput/autoscale**
  - **Technical definition:** throughput/autoscale is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Event Hubs capture**
  - **Technical definition:** Event Hubs capture is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **partition count**
  - **Technical definition:** partition count is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **consumer groups**
  - **Technical definition:** consumer groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Service Bus lock duration**
  - **Technical definition:** Service Bus lock duration is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **sessions**
  - **Technical definition:** sessions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **duplicate detection**
  - **Technical definition:** duplicate detection is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Event Grid filtering**
  - **Technical definition:** Event Grid filtering is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **retry policies**
  - **Technical definition:** retry policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **delivery endpoints**
  - **Technical definition:** delivery endpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Data Factory integration runtimes**
  - **Technical definition:** Data Factory integration runtimes is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **linked service credentials**
  - **Technical definition:** linked service credentials is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **managed identities**
  - **Technical definition:** managed identities is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Synapse workspace firewall**
  - **Technical definition:** Synapse workspace firewall is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Databricks secret scopes**
  - **Technical definition:** Databricks secret scopes is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Fabric/OneLake access controls**
  - **Technical definition:** Fabric/OneLake access controls is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Microsoft Purview scans**
  - **Technical definition:** Microsoft Purview scans is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **classifications**
  - **Technical definition:** classifications is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **glossary ownership**
  - **Technical definition:** glossary ownership is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **lineage capture**
  - **Technical definition:** lineage capture is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure AI Search access controls**
  - **Technical definition:** Azure AI Search access controls is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Key Vault soft delete**
  - **Technical definition:** Key Vault soft delete is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **purge protection**
  - **Technical definition:** purge protection is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **RBAC**
  - **Technical definition:** RBAC is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private endpoints**
  - **Technical definition:** private endpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **key rotation**
  - **Technical definition:** key rotation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **backup policies**
  - **Technical definition:** backup policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **recovery point retention**
  - **Technical definition:** recovery point retention is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **restore permissions**
  - **Technical definition:** restore permissions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

## Operations

### Expanded Points

- **Restore blobs/files/snapshots**
  - **Technical definition:** Restore blobs/files/snapshots is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **validate retention policies**
  - **Technical definition:** validate retention policies is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **review replication**
  - **Technical definition:** review replication is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **troubleshoot storage access**
  - **Technical definition:** troubleshoot storage access is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Resize disks**
  - **Technical definition:** Resize disks is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **restore snapshots**
  - **Technical definition:** restore snapshots is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **validate file share access**
  - **Technical definition:** validate file share access is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **test Azure NetApp Files recovery**
  - **Technical definition:** test Azure NetApp Files recovery is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Test Azure SQL failover groups**
  - **Technical definition:** Test Azure SQL failover groups is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **point-in-time restore**
  - **Technical definition:** point-in-time restore is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **read replicas**
  - **Technical definition:** read replicas is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **connection limits**
  - **Technical definition:** connection limits is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **migration cutover**
  - **Technical definition:** migration cutover is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Troubleshoot Cosmos DB hot partitions**
  - **Technical definition:** Troubleshoot Cosmos DB hot partitions is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **RU throttling**
  - **Technical definition:** RU throttling is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **indexing**
  - **Technical definition:** indexing is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **consistency**
  - **Technical definition:** consistency is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **change feed**
  - **Technical definition:** change feed is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **multi-region replication**
  - **Technical definition:** multi-region replication is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Monitor Event Hubs lag**
  - **Technical definition:** Monitor Event Hubs lag is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Service Bus dead-letter queues**
  - **Technical definition:** Service Bus dead-letter queues is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **retry behavior**
  - **Technical definition:** retry behavior is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Event Grid delivery failures**
  - **Technical definition:** Event Grid delivery failures is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Run Data Factory/Synapse/Databricks pipelines**
  - **Technical definition:** Run Data Factory/Synapse/Databricks pipelines is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **validate integration runtimes**
  - **Technical definition:** validate integration runtimes is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **troubleshoot streaming jobs**
  - **Technical definition:** troubleshoot streaming jobs is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Review Purview scans**
  - **Technical definition:** Review Purview scans is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **data classification**
  - **Technical definition:** data classification is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **lineage**
  - **Technical definition:** lineage is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **data access evidence**
  - **Technical definition:** data access evidence is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Rotate Key Vault secrets/keys**
  - **Technical definition:** Rotate Key Vault secrets/keys is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **validate private endpoint access**
  - **Technical definition:** validate private endpoint access is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **test backups**
  - **Technical definition:** test backups is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **restore recovery points**
  - **Technical definition:** restore recovery points is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

# Azure Week 5: Observability, FinOps, And Operational Excellence

## Service And Entity Coverage

### Azure Monitor, Log Analytics workspaces, Application Insights, Azure Monitor Agent, and Data Collection Rules

- **Azure Monitor**
  - **Technical definition:** Azure Monitor is Azure's central telemetry and alerting platform for metrics, logs, alerts, dashboards, and operational insights.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **Log Analytics workspaces**
  - **Technical definition:** Log Analytics Workspaces are Azure Monitor's KQL-backed log storage and query environments for centralized operational data.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **Application Insights**
  - **Technical definition:** Application Insights is Azure's application performance monitoring service for requests, dependencies, traces, and end-user telemetry.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **Azure Monitor Agent**
  - **Technical definition:** Azure Monitor Agent is Azure's current telemetry collection agent for sending performance, log, and other data into Azure Monitor pipelines.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **and Data Collection Rules**
  - **Technical definition:** and Data Collection Rules is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **metrics**
  - **Technical definition:** Metrics are time-series measurements representing resource or application health and performance.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **logs**
  - **Technical definition:** Logs are structured or semi-structured event records ingested into Azure Monitor.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **workspaces**
  - **Technical definition:** Workspaces are the log query and storage boundaries used by Log Analytics.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **tables**
  - **Technical definition:** Tables are the logical schemas used by Log Analytics to store specific telemetry categories.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **queries**
  - **Technical definition:** Queries are Kusto Query Language expressions used to retrieve and analyze telemetry.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **alert rules**
  - **Technical definition:** Alert rules are Azure Monitor evaluation objects triggering actions from metric or log conditions.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **dashboards**
  - **Technical definition:** Dashboards are Azure portal visual compositions of operational widgets and telemetry.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **workbooks**
  - **Technical definition:** Workbooks are interactive Azure Monitor reports combining query results, parameters, and visuals.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **agents**
  - **Technical definition:** Agents are telemetry collectors sending logs and metrics from machines into Azure Monitor.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **data collection endpoints**
  - **Technical definition:** Data collection endpoints are ingestion endpoints used by Azure Monitor collection pipelines.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **data collection rules**
  - **Technical definition:** Data collection rules define what telemetry is collected, transformed, and routed to which destinations.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **action groups**
  - **Technical definition:** Action groups are reusable notification and automation targets invoked by alert rules.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **application maps**
  - **Technical definition:** Application maps are dependency visualizations inferred from Application Insights telemetry.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.

### Azure Monitor managed service for Prometheus and Azure Managed Grafana

- **Azure Monitor managed service for Prometheus**
  - **Technical definition:** Azure Monitor Managed Service for Prometheus is Azure's managed Prometheus-compatible metrics collection and query backend.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **Azure Managed Grafana**
  - **Technical definition:** Azure Managed Grafana is Azure's managed Grafana service for dashboards, alerting, and observability visualization.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **Prometheus workspaces**
  - **Technical definition:** Prometheus workspaces is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **scrape configs**
  - **Technical definition:** scrape configs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **rule groups**
  - **Technical definition:** rule groups is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **Grafana workspaces**
  - **Technical definition:** Grafana workspaces is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **dashboards**
  - **Technical definition:** Dashboards are Azure portal visual compositions of operational widgets and telemetry.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **data sources**
  - **Technical definition:** data sources is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **users**
  - **Technical definition:** users is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **teams**
  - **Technical definition:** teams is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.
- **service accounts**
  - **Technical definition:** service accounts is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an incident timeline is reconstructed from KQL queries, metrics, and Application Insights traces after a rollout causes latency regression.

### Azure Activity Log, Azure Service Health, Azure Resource Health, Azure Monitor Change Analysis, and Azure Network Watcher

- **Azure Activity Log**
  - **Technical definition:** Azure Activity Log is Azure's subscription-level control-plane event log for create, update, delete, and administrative actions.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **Azure Service Health**
  - **Technical definition:** Azure Service Health is Azure's feed for service incidents, planned maintenance, and advisory events affecting tenant resources.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **Azure Resource Health**
  - **Technical definition:** Azure Resource Health is Azure's service for reporting platform-observed health state of individual Azure resources.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **Azure Monitor Change Analysis**
  - **Technical definition:** Azure Monitor Change Analysis is Azure's change-visibility capability for correlating resource and application changes with incidents.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **and Azure Network Watcher**
  - **Technical definition:** and Azure Network Watcher is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **activity events**
  - **Technical definition:** activity events is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **service health alerts**
  - **Technical definition:** service health alerts is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **resource health states**
  - **Technical definition:** resource health states is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **change records**
  - **Technical definition:** change records is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **packet captures**
  - **Technical definition:** packet captures is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **connection monitors**
  - **Technical definition:** connection monitors is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **NSG flow logs**
  - **Technical definition:** NSG flow logs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.
- **topology**
  - **Technical definition:** topology is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Network Watcher IP flow verify quickly proves that an NSG deny rule, not application failure, is blocking a production connection.

### Azure Automation and Azure Update Manager

- **Azure Automation**
  - **Technical definition:** Azure Automation is Azure's automation platform for runbooks, schedules, and hybrid worker execution.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **Azure Update Manager**
  - **Technical definition:** Azure Update Manager is Azure's patch orchestration service for assessing and scheduling updates on Azure and Arc-managed machines.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **automation accounts**
  - **Technical definition:** automation accounts is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **runbooks**
  - **Technical definition:** runbooks is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **schedules**
  - **Technical definition:** schedules is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **hybrid workers**
  - **Technical definition:** hybrid workers is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **update configurations**
  - **Technical definition:** update configurations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **machines**
  - **Technical definition:** machines is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **update assessments**
  - **Technical definition:** update assessments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.
- **maintenance configurations**
  - **Technical definition:** maintenance configurations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: update compliance falls below target, and a runbook plus maintenance configuration is used to patch noncompliant servers safely.

### Microsoft Defender for Cloud, Microsoft Sentinel, and Microsoft Purview

- **Microsoft Defender for Cloud**
  - **Technical definition:** Microsoft Defender for Cloud is Azure's cloud security posture and workload protection platform for recommendations, plans, and compliance visibility.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **Microsoft Sentinel**
  - **Technical definition:** Microsoft Sentinel is Microsoft's cloud-native SIEM and SOAR platform for security analytics, incidents, automation, and hunting.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **and Microsoft Purview**
  - **Technical definition:** and Microsoft Purview is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **secure score**
  - **Technical definition:** secure score is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **recommendations**
  - **Technical definition:** recommendations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **regulatory compliance controls**
  - **Technical definition:** regulatory compliance controls is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **Defender plans**
  - **Technical definition:** Defender plans is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **Sentinel workspaces**
  - **Technical definition:** Sentinel workspaces is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **data connectors**
  - **Technical definition:** data connectors is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **analytics rules**
  - **Technical definition:** analytics rules is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **incidents**
  - **Technical definition:** incidents is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **watchlists**
  - **Technical definition:** watchlists is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **playbooks**
  - **Technical definition:** playbooks is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.
- **Purview classifications**
  - **Technical definition:** Purview classifications is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Purview scan results and lineage are reviewed to prove where regulated data moved before granting analysts access to a new lakehouse.

### Microsoft Cost Management, Azure Advisor, Azure Reservations, and Azure savings plan for compute

- **Microsoft Cost Management**
  - **Technical definition:** Microsoft Cost Management is Azure's spend analysis, budgeting, export, and cost-governance platform across billing scopes and subscriptions.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **Azure Advisor**
  - **Technical definition:** Azure Advisor is Azure's recommendation service for reliability, cost, performance, security, and operational excellence improvements.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **Azure Reservations**
  - **Technical definition:** Azure Reservations are Azure's pre-purchased committed capacity constructs for reducing cost on eligible services.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **and Azure savings plan for compute**
  - **Technical definition:** and Azure savings plan for compute is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **cost analysis views**
  - **Technical definition:** cost analysis views is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **budgets**
  - **Technical definition:** Budgets are defined spend thresholds associated with billing or resource scopes for alerting and accountability.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **exports**
  - **Technical definition:** Exports are scheduled output definitions delivering cost and usage data to storage or analysis destinations.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **alerts**
  - **Technical definition:** Alerts are notifications triggered when budget or forecast conditions cross configured thresholds.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **recommendations**
  - **Technical definition:** recommendations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **reservations**
  - **Technical definition:** Reservations are prepaid commitments applied to eligible services for discounted billing.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **savings plans**
  - **Technical definition:** savings plans is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **scopes**
  - **Technical definition:** scopes is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.
- **amortized cost views**
  - **Technical definition:** amortized cost views is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: budget alerts fire when a migration wave exceeds forecasted spend so the team can investigate oversizing before month-end charges land.

### Azure Machine Learning model monitoring, responsible AI dashboard, Azure OpenAI Service, and Azure AI Content Safety

- **Azure Machine Learning model monitoring**
  - **Technical definition:** Azure Machine Learning model monitoring is Azure ML's capability for observing deployed model behavior, drift, and data quality.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **responsible AI dashboard**
  - **Technical definition:** Responsible AI Dashboard is Azure's interpretability and fairness analysis interface for examining ML model behavior and explanations.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **Azure OpenAI Service**
  - **Technical definition:** Azure OpenAI Service is Azure's managed hosted interface for OpenAI models with enterprise network, identity, quota, and content-safety controls.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **and Azure AI Content Safety**
  - **Technical definition:** and Azure AI Content Safety is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **monitoring signals**
  - **Technical definition:** monitoring signals is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **data drift**
  - **Technical definition:** data drift is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **model explanations**
  - **Technical definition:** model explanations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **content filters**
  - **Technical definition:** content filters is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **safety categories**
  - **Technical definition:** safety categories is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **evaluations**
  - **Technical definition:** evaluations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.
- **diagnostic logs**
  - **Technical definition:** diagnostic logs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a model is registered and promoted to a managed online endpoint only after training metrics and approval checks pass.

## Controls

### Expanded Points

- **Azure Monitor workspace topology**
  - **Technical definition:** Azure Monitor workspace topology is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **diagnostic settings**
  - **Technical definition:** diagnostic settings is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **data collection rules**
  - **Technical definition:** data collection rules is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **log retention**
  - **Technical definition:** log retention is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **table access**
  - **Technical definition:** table access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **alert rules**
  - **Technical definition:** alert rules is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **action groups**
  - **Technical definition:** action groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **workbook access**
  - **Technical definition:** workbook access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Application Insights sampling**
  - **Technical definition:** Application Insights sampling is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Managed Prometheus scrape config**
  - **Technical definition:** Managed Prometheus scrape config is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rule groups**
  - **Technical definition:** rule groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Grafana data source permissions**
  - **Technical definition:** Grafana data source permissions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **dashboard access**
  - **Technical definition:** dashboard access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **identity configuration**
  - **Technical definition:** identity configuration is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Activity Log export**
  - **Technical definition:** Activity Log export is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Service Health alerts**
  - **Technical definition:** Service Health alerts is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Resource Health tracking**
  - **Technical definition:** Resource Health tracking is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Network Watcher flow logs**
  - **Technical definition:** Network Watcher flow logs is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **packet capture permissions**
  - **Technical definition:** packet capture permissions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **change analysis visibility**
  - **Technical definition:** change analysis visibility is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Automation account identity**
  - **Technical definition:** Automation account identity is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **runbook permissions**
  - **Technical definition:** runbook permissions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **hybrid worker access**
  - **Technical definition:** hybrid worker access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Update Manager maintenance configuration**
  - **Technical definition:** Update Manager maintenance configuration is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **patch compliance settings**
  - **Technical definition:** patch compliance settings is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Defender for Cloud plan enablement**
  - **Technical definition:** Defender for Cloud plan enablement is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **regulatory compliance**
  - **Technical definition:** regulatory compliance is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Sentinel data connector access**
  - **Technical definition:** Sentinel data connector access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **analytics rule tuning**
  - **Technical definition:** analytics rule tuning is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **incident automation**
  - **Technical definition:** incident automation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Purview scan/classification access**
  - **Technical definition:** Purview scan/classification access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **retention**
  - **Technical definition:** retention is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Cost Management budgets**
  - **Technical definition:** Cost Management budgets is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **exports**
  - **Technical definition:** exports is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **cost allocation tags**
  - **Technical definition:** cost allocation tags is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Advisor recommendation review**
  - **Technical definition:** Advisor recommendation review is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **reservation ownership**
  - **Technical definition:** reservation ownership is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **savings plan commitment tracking**
  - **Technical definition:** savings plan commitment tracking is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **scope permissions**
  - **Technical definition:** scope permissions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure Machine Learning monitoring thresholds**
  - **Technical definition:** Azure Machine Learning monitoring thresholds is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **responsible AI artifact access**
  - **Technical definition:** responsible AI artifact access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure OpenAI diagnostic settings**
  - **Technical definition:** Azure OpenAI diagnostic settings is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **content safety filters**
  - **Technical definition:** content safety filters is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **private endpoint access**
  - **Technical definition:** private endpoint access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

## Operations

### Expanded Points

- **Build Azure Monitor dashboards**
  - **Technical definition:** Build Azure Monitor dashboards is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Kusto Query Language (KQL) queries**
  - **Technical definition:** Kusto Query Language (KQL) queries is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **alert rules**
  - **Technical definition:** alert rules is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **action groups**
  - **Technical definition:** action groups is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **incident timelines**
  - **Technical definition:** incident timelines is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Validate Prometheus scraping**
  - **Technical definition:** Validate Prometheus scraping is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Grafana dashboards**
  - **Technical definition:** Grafana dashboards is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **log ingestion**
  - **Technical definition:** log ingestion is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **metric cardinality**
  - **Technical definition:** metric cardinality is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Use Activity Log**
  - **Technical definition:** Use Activity Log is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Service Health**
  - **Technical definition:** Service Health is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Resource Health**
  - **Technical definition:** Resource Health is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Change Analysis**
  - **Technical definition:** Change Analysis is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Network Watcher during incidents**
  - **Technical definition:** Network Watcher during incidents is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Run automation jobs**
  - **Technical definition:** Run automation jobs is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **patch assessments**
  - **Technical definition:** patch assessments is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **maintenance configurations**
  - **Technical definition:** maintenance configurations is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **update compliance reviews**
  - **Technical definition:** update compliance reviews is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Triage Defender for Cloud recommendations**
  - **Technical definition:** Triage Defender for Cloud recommendations is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Sentinel incidents**
  - **Technical definition:** Sentinel incidents is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **analytics rules**
  - **Technical definition:** analytics rules is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Purview classification findings**
  - **Technical definition:** Purview classification findings is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Review budgets**
  - **Technical definition:** Review budgets is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **cost exports**
  - **Technical definition:** cost exports is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Advisor recommendations**
  - **Technical definition:** Advisor recommendations is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **reservations**
  - **Technical definition:** reservations is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **savings plans**
  - **Technical definition:** savings plans is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **right-sizing actions**
  - **Technical definition:** right-sizing actions is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Review model drift**
  - **Technical definition:** Review model drift is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **responsible AI dashboards**
  - **Technical definition:** responsible AI dashboards is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure OpenAI content filtering**
  - **Technical definition:** Azure OpenAI content filtering is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **AI safety evidence**
  - **Technical definition:** AI safety evidence is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

# Azure Week 6: DR, Resilience, And Migration Execution

## Service And Entity Coverage

### Azure Site Recovery and Recovery Services vaults

- **Azure Site Recovery and Recovery Services vaults**
  - **Technical definition:** Azure Site Recovery and Recovery Services vaults is an Azure managed service whose behavior should be understood in terms of control plane, data plane, scope, and failure implications.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **vaults**
  - **Technical definition:** Vaults are the management containers holding Site Recovery policies, replicated items, and recovery metadata.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **replicated items**
  - **Technical definition:** Replicated items are the protected machines or workloads being continuously replicated by Site Recovery.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **replication policies**
  - **Technical definition:** Replication policies define replication frequency, retention, and recovery-point behavior.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **mobility agents**
  - **Technical definition:** Mobility agents are the installed replication agents on protected source machines for Site Recovery.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **recovery plans**
  - **Technical definition:** Recovery plans are orchestrated runbooks sequencing groups of protected items during failover.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **test failovers**
  - **Technical definition:** Test failovers are isolated recovery drills that launch replicated workloads without affecting production.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **failovers**
  - **Technical definition:** Failovers are actions promoting replicated workloads into the target environment after outage or planned DR.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **failbacks**
  - **Technical definition:** Failbacks are actions returning workload authority from recovery target back to the original site.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **recovery points**
  - **Technical definition:** Recovery points are the time-specific replicated states from which recovery may be performed.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.
- **protected machines**
  - **Technical definition:** Protected machines are source servers currently under Site Recovery protection.
  - **Concrete production example:** Example: a quarterly Site Recovery drill uses isolated test failover networks so replicated workloads can be validated without affecting production.

### Azure Migrate and Azure Database Migration Service

- **Azure Migrate**
  - **Technical definition:** Azure Migrate is Azure's migration discovery, assessment, and server migration orchestration platform.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **Azure Database Migration Service**
  - **Technical definition:** Azure Database Migration Service is Azure's managed service for migrating supported databases with online or offline migration workflows.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **projects**
  - **Technical definition:** projects is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **appliances**
  - **Technical definition:** appliances is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **discovered servers**
  - **Technical definition:** discovered servers is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **assessments**
  - **Technical definition:** assessments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **groups**
  - **Technical definition:** groups is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **migration waves**
  - **Technical definition:** migration waves is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **replication jobs**
  - **Technical definition:** replication jobs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **test migrations**
  - **Technical definition:** test migrations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **cutovers**
  - **Technical definition:** cutovers is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **source endpoints**
  - **Technical definition:** source endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **target endpoints**
  - **Technical definition:** target endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **migration projects**
  - **Technical definition:** migration projects is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.
- **migration activities**
  - **Technical definition:** migration activities is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Azure Migrate test migration validates boot, identity, and network reachability before the final production cutover window is approved.

### Azure Resource Mover

- **Azure Resource Mover**
  - **Technical definition:** Azure Resource Mover is Azure's region-to-region move orchestration service for selected Azure resources and dependencies.
  - **Concrete production example:** Example: a regional move is committed only after dependency validation confirms the target region has matching network and platform prerequisites.
- **move collections**
  - **Technical definition:** move collections is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional move is committed only after dependency validation confirms the target region has matching network and platform prerequisites.
- **resources**
  - **Technical definition:** resources is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional move is committed only after dependency validation confirms the target region has matching network and platform prerequisites.
- **dependencies**
  - **Technical definition:** dependencies is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional move is committed only after dependency validation confirms the target region has matching network and platform prerequisites.
- **prepare operations**
  - **Technical definition:** prepare operations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional move is committed only after dependency validation confirms the target region has matching network and platform prerequisites.
- **initiate move operations**
  - **Technical definition:** initiate move operations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional move is committed only after dependency validation confirms the target region has matching network and platform prerequisites.
- **commit operations**
  - **Technical definition:** commit operations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional move is committed only after dependency validation confirms the target region has matching network and platform prerequisites.
- **discard operations**
  - **Technical definition:** discard operations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a regional move is committed only after dependency validation confirms the target region has matching network and platform prerequisites.

### Azure Data Box and Azure Storage Mover

- **Azure Data Box**
  - **Technical definition:** Azure Data Box is Azure's physical data transfer service for moving large datasets into or out of Azure using shipped devices.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **Azure Storage Mover**
  - **Technical definition:** Azure Storage Mover is Azure's data movement orchestration service for migrating files into Azure storage using managed mover agents.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **Data Box orders**
  - **Technical definition:** Data Box orders is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **devices**
  - **Technical definition:** devices is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **jobs**
  - **Technical definition:** jobs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **shares**
  - **Technical definition:** shares is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **transfer logs**
  - **Technical definition:** transfer logs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **Storage Mover agents**
  - **Technical definition:** Storage Mover agents is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **projects**
  - **Technical definition:** projects is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **endpoints**
  - **Technical definition:** endpoints is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **job definitions**
  - **Technical definition:** job definitions is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.
- **job runs**
  - **Technical definition:** job runs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: an accidentally deleted blob is restored from version history because versioning and immutable-retention controls were enabled beforehand.

### Azure Arc

- **Azure Arc**
  - **Technical definition:** Azure Arc is Azure's hybrid and multicloud management platform for projecting external servers, Kubernetes clusters, and services into Azure governance and operations.
  - **Concrete production example:** Example: on-premises servers receive Azure Policy guest-configuration checks through Arc so hybrid compliance is measured centrally.
- **Azure Arc-enabled servers**
  - **Technical definition:** Azure Arc-enabled servers is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: on-premises servers receive Azure Policy guest-configuration checks through Arc so hybrid compliance is measured centrally.
- **connected machines**
  - **Technical definition:** connected machines is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: on-premises servers receive Azure Policy guest-configuration checks through Arc so hybrid compliance is measured centrally.
- **extensions**
  - **Technical definition:** extensions is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: on-premises servers receive Azure Policy guest-configuration checks through Arc so hybrid compliance is measured centrally.
- **policies**
  - **Technical definition:** policies is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: on-premises servers receive Azure Policy guest-configuration checks through Arc so hybrid compliance is measured centrally.
- **guest configurations**
  - **Technical definition:** guest configurations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: on-premises servers receive Azure Policy guest-configuration checks through Arc so hybrid compliance is measured centrally.
- **update management integrations**
  - **Technical definition:** update management integrations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: on-premises servers receive Azure Policy guest-configuration checks through Arc so hybrid compliance is measured centrally.

### Azure Load Testing and Azure Chaos Studio

- **Azure Load Testing**
  - **Technical definition:** Azure Load Testing is Azure's managed load-generation service for performance and scale validation.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **Azure Chaos Studio**
  - **Technical definition:** Azure Chaos Studio is Azure's managed chaos engineering service for controlled fault injection and resilience testing.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **load tests**
  - **Technical definition:** load tests is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **test engines**
  - **Technical definition:** test engines is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **test runs**
  - **Technical definition:** test runs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **metrics**
  - **Technical definition:** metrics is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **chaos experiments**
  - **Technical definition:** chaos experiments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **targets**
  - **Technical definition:** targets is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **capabilities**
  - **Technical definition:** capabilities is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **steps**
  - **Technical definition:** steps is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **branches**
  - **Technical definition:** branches is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.
- **faults**
  - **Technical definition:** faults is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: Chaos Studio injects network latency into one service tier while monitoring confirms whether retry and timeout behavior is resilient.

### Azure Well-Architected Review

- **Azure Well-Architected Review**
  - **Technical definition:** Azure Well-Architected Review is Microsoft's structured assessment framework for evaluating workload design against architectural best practices.
  - **Concrete production example:** Example: a Well-Architected assessment identifies missing zone redundancy and backup validation, then tracks those gaps in an action plan.
- **workloads**
  - **Technical definition:** workloads is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Well-Architected assessment identifies missing zone redundancy and backup validation, then tracks those gaps in an action plan.
- **pillars**
  - **Technical definition:** pillars is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Well-Architected assessment identifies missing zone redundancy and backup validation, then tracks those gaps in an action plan.
- **assessments**
  - **Technical definition:** assessments is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Well-Architected assessment identifies missing zone redundancy and backup validation, then tracks those gaps in an action plan.
- **recommendations**
  - **Technical definition:** recommendations is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Well-Architected assessment identifies missing zone redundancy and backup validation, then tracks those gaps in an action plan.
- **action plans**
  - **Technical definition:** action plans is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: a Well-Architected assessment identifies missing zone redundancy and backup validation, then tracks those gaps in an action plan.

## DR Dependencies From Earlier Weeks

### Expanded Points

- **Azure DNS**
  - **Technical definition:** Azure DNS is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Traffic Manager**
  - **Technical definition:** Azure Traffic Manager is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Front Door**
  - **Technical definition:** Azure Front Door is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Load Balancer**
  - **Technical definition:** Azure Load Balancer is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Application Gateway**
  - **Technical definition:** Azure Application Gateway is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **VPN Gateway**
  - **Technical definition:** VPN Gateway is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **ExpressRoute are covered in Week 2**
  - **Technical definition:** ExpressRoute are covered in Week 2 is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure Storage replication**
  - **Technical definition:** Azure Storage replication is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Files**
  - **Technical definition:** Azure Files is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure NetApp Files**
  - **Technical definition:** Azure NetApp Files is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure SQL failover groups**
  - **Technical definition:** Azure SQL failover groups is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Cosmos DB multi-region writes**
  - **Technical definition:** Azure Cosmos DB multi-region writes is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Event Hubs**
  - **Technical definition:** Azure Event Hubs is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Service Bus**
  - **Technical definition:** Azure Service Bus is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Key Vault**
  - **Technical definition:** Azure Key Vault is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Azure Backup are covered in Week 4**
  - **Technical definition:** Azure Backup are covered in Week 4 is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure Monitor**
  - **Technical definition:** Azure Monitor is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Log Analytics**
  - **Technical definition:** Log Analytics is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Service Health**
  - **Technical definition:** Service Health is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Resource Health**
  - **Technical definition:** Resource Health is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Defender for Cloud**
  - **Technical definition:** Defender for Cloud is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Sentinel**
  - **Technical definition:** Sentinel is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Cost Management are covered in Week 5**
  - **Technical definition:** Cost Management are covered in Week 5 is an Azure entity, setting, or architectural construct whose technical importance depends on scope, lifecycle behavior, and operational impact.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

## Controls

### Expanded Points

- **Azure Site Recovery replication policies**
  - **Technical definition:** Azure Site Recovery replication policies is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **recovery point retention**
  - **Technical definition:** recovery point retention is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **test failover networks**
  - **Technical definition:** test failover networks is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **recovery plan groups**
  - **Technical definition:** recovery plan groups is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **failback settings**
  - **Technical definition:** failback settings is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **vault access controls**
  - **Technical definition:** vault access controls is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Azure Migrate appliance access**
  - **Technical definition:** Azure Migrate appliance access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **discovery scope**
  - **Technical definition:** discovery scope is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **assessment assumptions**
  - **Technical definition:** assessment assumptions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **dependency mapping**
  - **Technical definition:** dependency mapping is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **target sizing**
  - **Technical definition:** target sizing is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **migration wave grouping**
  - **Technical definition:** migration wave grouping is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **cutover controls**
  - **Technical definition:** cutover controls is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Database Migration Service source/target connectivity**
  - **Technical definition:** Database Migration Service source/target connectivity is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **migration mode**
  - **Technical definition:** migration mode is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **schema compatibility**
  - **Technical definition:** schema compatibility is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **validation**
  - **Technical definition:** validation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **downtime window**
  - **Technical definition:** downtime window is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rollback planning**
  - **Technical definition:** rollback planning is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Resource Mover dependency validation**
  - **Technical definition:** Resource Mover dependency validation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **prepare/move/commit flow**
  - **Technical definition:** prepare/move/commit flow is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **target region readiness**
  - **Technical definition:** target region readiness is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rollback checkpoints**
  - **Technical definition:** rollback checkpoints is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Data Box chain-of-custody**
  - **Technical definition:** Data Box chain-of-custody is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **encryption**
  - **Technical definition:** encryption is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **job access**
  - **Technical definition:** job access is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **Storage Mover agent configuration**
  - **Technical definition:** Storage Mover agent configuration is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **endpoint permissions**
  - **Technical definition:** endpoint permissions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **transfer validation**
  - **Technical definition:** transfer validation is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Chaos Studio experiment permissions**
  - **Technical definition:** Chaos Studio experiment permissions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **target selection**
  - **Technical definition:** target selection is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **blast-radius control**
  - **Technical definition:** blast-radius control is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **stop conditions**
  - **Technical definition:** stop conditions is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **monitoring integration**
  - **Technical definition:** monitoring integration is an Azure control that should be understood in terms of preventive strength, blast radius, and auditability.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

## Operations

### Expanded Points

- **Run Azure Site Recovery test failovers**
  - **Technical definition:** Run Azure Site Recovery test failovers is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **planned failovers**
  - **Technical definition:** planned failovers is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **unplanned failovers**
  - **Technical definition:** unplanned failovers is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **failbacks**
  - **Technical definition:** failbacks is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **recovery plan validation**
  - **Technical definition:** recovery plan validation is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Use Azure Migrate for discovery**
  - **Technical definition:** Use Azure Migrate for discovery is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **dependency grouping**
  - **Technical definition:** dependency grouping is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **assessment**
  - **Technical definition:** assessment is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **test migration**
  - **Technical definition:** test migration is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **cutover**
  - **Technical definition:** cutover is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **rollback planning**
  - **Technical definition:** rollback planning is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Execute database migration rehearsals**
  - **Technical definition:** Execute database migration rehearsals is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **validate data**
  - **Technical definition:** validate data is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **monitor replication**
  - **Technical definition:** monitor replication is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **complete cutover with rollback criteria**
  - **Technical definition:** complete cutover with rollback criteria is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Move regional resources with Azure Resource Mover and validate dependency readiness**
  - **Technical definition:** Move regional resources with Azure Resource Mover and validate dependency readiness is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Transfer offline or large datasets with Azure Data Box or Azure Storage Mover and validate checksums/cutover sync**
  - **Technical definition:** Transfer offline or large datasets with Azure Data Box or Azure Storage Mover and validate checksums/cutover sync is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.

### Expanded Points

- **Run Azure Chaos Studio and Azure Load Testing exercises with monitoring**
  - **Technical definition:** Run Azure Chaos Studio and Azure Load Testing exercises with monitoring is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **stop conditions**
  - **Technical definition:** stop conditions is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
- **post-test evidence**
  - **Technical definition:** post-test evidence is an Azure operational workflow used to validate production behavior and reduce change risk.
  - **Concrete production example:** Example: this point becomes operationally important during rollout, audit, migration, or recovery when exact Azure behavior matters.
