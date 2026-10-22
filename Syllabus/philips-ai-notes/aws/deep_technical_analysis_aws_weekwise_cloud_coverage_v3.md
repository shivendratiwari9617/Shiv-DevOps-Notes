# Deep Technical Analysis of AWS Week-Wise Cloud Coverage — v3
## AWS Technical Study Notes with Independent Expansion of Comma-Separated and `and`-Separated Service/Entity Points

> Goal: push the notes toward a **9.8+ AWS study standard** by reducing template feel, tightening AWS-specific definitions, and making child entities and controls more technically distinct.
> Parsing rule applied: every comma-separated value is treated as a point, and when **`and` links two AWS services or two AWS entities**, they are treated as separate points with independent definitions.

# AWS Week 1: Landing Zone, Account Strategy, And Governance

## Service And Entity Coverage

### AWS Organizations and AWS Account Management

- **AWS Organizations**
  - **Technical definition:** AWS Organizations is AWS's multi-account governance service for building an account hierarchy, attaching inherited policy, delegating service administration, and consolidating billing and governance scope.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **AWS Account Management**
  - **Technical definition:** AWS Account Management is the account-scoped administrative layer for contacts, support metadata, alternate contacts, Region enablement, and other account-level operational settings.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **organization**
  - **Technical definition:** An organization is the top-level AWS Organizations container that owns account hierarchy, trusted-service integrations, and policy inheritance scope.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **root**
  - **Technical definition:** The root is the highest hierarchy node in an AWS Organization under which all OUs and accounts ultimately reside and inherit organization-level guardrails.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **management account**
  - **Technical definition:** The management account is the AWS account with authority to administer the organization, billing, and organization-level integrations.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **member accounts**
  - **Technical definition:** Member accounts are AWS accounts attached to an organization and subject to its hierarchy and policy model.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **organizational units (OUs)**
  - **Technical definition:** Organizational units are hierarchy containers used to group accounts for inherited governance, delegation, and environment segmentation.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **delegated administrators**
  - **Technical definition:** Delegated administrators are member accounts explicitly authorized to manage supported AWS services on behalf of the organization without using the management account for every operation.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **service control policies (SCPs)**
  - **Technical definition:** Service control policies are organization-level permission guardrails that define the maximum AWS API permissions available to accounts or OUs, regardless of IAM allow policy.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **resource control policies (RCPs)**
  - **Technical definition:** Resource control policies are organization-scoped guardrails for supported resource configuration domains, intended to constrain what members may deploy or configure.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **tag policies**
  - **Technical definition:** Tag policies are organization-level metadata-governance rules that standardize allowed tag keys, values, and capitalization across accounts.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **backup policies**
  - **Technical definition:** Backup policies are organization-level policy definitions used to standardize backup-plan behavior across supported resources in multiple accounts.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **AI services opt-out policies**
  - **Technical definition:** AI services opt-out policies are organization-level controls that govern whether supported AWS AI services may use customer content for service-improvement purposes.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **AWS accounts**
  - **Technical definition:** AWS accounts are AWS's primary tenancy boundary for IAM, billing, quotas, and many service control planes.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **primary contacts**
  - **Technical definition:** Primary contacts are the main administrative ownership records for an AWS account, used for account-level communications and responsibility.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **alternate contacts**
  - **Technical definition:** Alternate contacts are account-specific billing, operations, or security contact records used to route AWS account communications to the right role.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **AWS Regions**
  - **Technical definition:** AWS Regions are geographically isolated AWS deployment areas whose enablement affects placement, compliance, and disaster recovery options per account.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **Region opt-in status**
  - **Technical definition:** Region opt-in status is the per-account enablement state that determines whether opt-in AWS Regions are available for use.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.

### AWS Control Tower

- **AWS Control Tower**
  - **Technical definition:** AWS Control Tower is AWS's managed landing-zone service for provisioning and governing multi-account environments through opinionated baselines, account vending, and guardrail enforcement.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **landing zone**
  - **Technical definition:** A landing zone in AWS Control Tower is the managed multi-account baseline containing governance accounts, logging paths, identity expectations, and guardrail configuration.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **Account Factory**
  - **Technical definition:** Account Factory is Control Tower's account-vending workflow for creating new governed AWS accounts from a standardized baseline.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **governed accounts**
  - **Technical definition:** Governed accounts are AWS accounts enrolled in Control Tower and therefore subject to its baselines, lifecycle checks, and guardrails.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **registered organizational units (OUs)**
  - **Technical definition:** Registered OUs are the organization units explicitly brought under Control Tower governance and account-factory management.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **preventive controls**
  - **Technical definition:** Preventive controls are Control Tower guardrails that stop noncompliant actions before they succeed, typically via policy enforcement.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **detective controls**
  - **Technical definition:** Detective controls are Control Tower guardrails that evaluate deployed state after the fact and flag noncompliance rather than blocking the change.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **proactive controls**
  - **Technical definition:** Proactive controls are pre-deployment guardrails that validate infrastructure definitions before resources are created.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **drift status**
  - **Technical definition:** Drift status is Control Tower's assessment of whether enrolled environments still match the expected landing-zone baseline.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.

### AWS Identity and Access Management (IAM)

- **AWS Identity and Access Management (IAM)**
  - **Technical definition:** AWS IAM is AWS's identity and authorization control plane for users, roles, policies, trust relationships, and temporary credential issuance inside an AWS account.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **IAM users**
  - **Technical definition:** IAM users are long-lived account-scoped identities with credentials and attached permissions, generally discouraged for workload use compared with roles.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **IAM groups**
  - **Technical definition:** IAM groups are account-local collections of IAM users used to simplify permission assignment to humans.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **IAM roles**
  - **Technical definition:** IAM roles are assumable AWS identities that issue temporary credentials when trust policy and caller conditions are satisfied.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **IAM policies**
  - **Technical definition:** IAM policies are JSON authorization documents evaluated by the IAM policy engine to allow or deny specific actions under specific resource and condition scope.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **instance profiles**
  - **Technical definition:** Instance profiles are the EC2 attachment objects that expose an IAM role to an instance through the instance metadata path.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **access keys**
  - **Technical definition:** Access keys are long-lived IAM user API credentials and therefore a higher-risk credential form than temporary role-based credentials.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **MFA devices**
  - **Technical definition:** MFA devices are second-factor authenticators bound to users or the root identity to strengthen login and sensitive action security.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **permissions boundaries**
  - **Technical definition:** Permissions boundaries are maximum-permission filters attached to IAM principals that cap what identity policies may effectively grant.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **role trust policies**
  - **Technical definition:** Role trust policies are resource-based policy documents on IAM roles that define which principals may assume the role and under what conditions.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **analyzers**
  - **Technical definition:** IAM Access Analyzer analyzers are analysis scopes that continuously evaluate policies and resource configurations for unintended external access.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **findings**
  - **Technical definition:** IAM Access Analyzer findings are outputs that identify public, cross-account, or otherwise sensitive access paths inferred from AWS policies.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **archive rules**
  - **Technical definition:** Archive rules are finding filters used to automatically suppress Access Analyzer results that match known acceptable patterns.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **access previews**
  - **Technical definition:** Access previews simulate how a proposed policy change would alter external access before the resource is deployed or updated.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **policy validation checks**
  - **Technical definition:** Policy validation checks are static and semantic analyses of IAM policies that flag errors, overly broad access, or best-practice violations.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.

### AWS IAM Identity Center

- **AWS IAM Identity Center**
  - **Technical definition:** AWS IAM Identity Center is AWS's workforce access service for federating users and groups into AWS accounts and AWS-integrated applications through centrally managed permission assignments.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **identity sources**
  - **Technical definition:** Identity sources are the authoritative directories—internal store or external IdP—from which IAM Identity Center receives user and group identity.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **users**
  - **Technical definition:** Users are workforce identities represented in IAM Identity Center and assignable to AWS accounts or SSO-integrated applications.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **groups**
  - **Technical definition:** Groups are IAM Identity Center identity collections used to grant account or application access at scale.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **permission sets**
  - **Technical definition:** Permission sets are reusable access bundles that provision IAM roles and associated permissions into target AWS accounts.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **account assignments**
  - **Technical definition:** Account assignments bind Identity Center users or groups to permission sets for specific AWS accounts, producing resulting access.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.
- **application assignments**
  - **Technical definition:** Application assignments grant federated users or groups access to SSO-integrated applications through Identity Center.
  - **Concrete production example:** Example: a production operator or workload uses this so access is temporary, scoped, and auditable instead of relying on standing credentials.

### AWS Service Catalog and AWS Resource Access Manager

- **AWS Service Catalog**
  - **Technical definition:** AWS Service Catalog is AWS's governed self-service platform for publishing approved infrastructure or platform products with launch and change constraints.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **AWS Resource Access Manager**
  - **Technical definition:** AWS Resource Access Manager is AWS's cross-account sharing service for exposing supported resources to specific accounts, OUs, or organizations without resource duplication.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **portfolios**
  - **Technical definition:** Portfolios are Service Catalog containers that group products and control which principals may discover or launch them.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **products**
  - **Technical definition:** Products are the governed self-service offerings published through Service Catalog, typically backed by infrastructure templates.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **provisioning artifacts**
  - **Technical definition:** Provisioning artifacts are versioned implementations of a Service Catalog product, such as a specific CloudFormation template release.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **provisioned products**
  - **Technical definition:** Provisioned products are live instantiated deployments created from Service Catalog products under catalog governance.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **constraints**
  - **Technical definition:** Constraints are Service Catalog controls that limit launch roles, allowed parameter values, or template behaviors for products.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **launch paths**
  - **Technical definition:** Launch paths are the valid product-portfolio-principal combinations through which a user is allowed to provision a product.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **portfolio access**
  - **Technical definition:** Portfolio access defines which principals or accounts may view or launch the products inside a Service Catalog portfolio.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **resource shares (AWS RAM shares)**
  - **Technical definition:** RAM resource shares are the share objects that expose selected AWS resources to other accounts, OUs, or organizations.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **principals**
  - **Technical definition:** Principals are the accounts, organization units, or organizations allowed to consume RAM-shared resources.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **resources**
  - **Technical definition:** Resources are the supported AWS entities exposed through a RAM share for controlled cross-account use.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **managed permissions**
  - **Technical definition:** Managed permissions are AWS-defined RAM permission templates that determine what consumers may do with a shared resource.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **customer managed permissions**
  - **Technical definition:** Customer managed permissions are customer-authored RAM permission definitions that tailor consumer access to a shared resource.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **invitations**
  - **Technical definition:** Invitations are RAM acceptance objects used when a share requires explicit recipient acknowledgment.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.

### AWS CloudFormation StackSets

- **AWS CloudFormation StackSets**
  - **Technical definition:** AWS CloudFormation StackSets is the multi-account, multi-Region deployment feature for pushing one CloudFormation-defined baseline into many targets with centralized operation control.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **stack sets (StackSets)**
  - **Technical definition:** StackSets are the top-level CloudFormation multi-target deployment objects that define one stack model across many accounts and Regions.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **stack instances**
  - **Technical definition:** Stack instances are the account-and-Region-specific deployments created from a StackSet.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **stack operations**
  - **Technical definition:** Stack operations are coordinated create, update, or delete actions performed by StackSets against selected stack instances.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **administrator accounts**
  - **Technical definition:** Administrator accounts are the accounts from which StackSet orchestration is initiated and managed.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **execution roles**
  - **Technical definition:** Execution roles are target-account roles assumed by StackSets to create or update resources in each destination account.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **target accounts**
  - **Technical definition:** Target accounts are the AWS accounts where StackSet instances are deployed.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **target organizational units (OUs)**
  - **Technical definition:** Target OUs are hierarchy scopes used by service-managed StackSets to target many accounts by organization placement.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.
- **operation preferences**
  - **Technical definition:** Operation preferences define concurrency, Region order, failure tolerance, and deployment sequencing for StackSet operations.
  - **Concrete production example:** Example: a platform team relies on this when onboarding a new production account so inherited guardrails, logging, and approved deployment paths are enforced from day one.

### AWS CloudTrail and AWS Config

- **AWS CloudTrail**
  - **Technical definition:** AWS CloudTrail is AWS's API audit service for capturing management, data, and insight events across accounts and Regions.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **AWS Config**
  - **Technical definition:** AWS Config is AWS's configuration recording and compliance-evaluation service for tracking resource state history, rules, and drift from expected settings.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **trails**
  - **Technical definition:** CloudTrail trails are configurations that continuously deliver selected AWS API events into destinations such as S3 and CloudWatch Logs.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **organization trails**
  - **Technical definition:** Organization trails are CloudTrail trails created at the organization level so member-account events are centrally captured.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **event data stores**
  - **Technical definition:** Event data stores are CloudTrail Lake-backed event repositories for queryable AWS API activity with configurable selectors and retention.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **management events**
  - **Technical definition:** Management events are CloudTrail records for AWS control-plane actions such as create, modify, and delete operations on resources.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **data events**
  - **Technical definition:** Data events are CloudTrail records for resource-level actions such as S3 object operations or Lambda invoke activity.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **Insights events**
  - **Technical definition:** Insights events are CloudTrail anomaly outputs highlighting unusual management API rates or error patterns.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **configuration recorders**
  - **Technical definition:** Configuration recorders are AWS Config components that capture supported resource configuration state and change history.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **delivery channels**
  - **Technical definition:** Delivery channels are AWS Config outputs that specify where snapshots and configuration-change notifications are delivered.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **configuration items**
  - **Technical definition:** Configuration items are the versioned records AWS Config creates to represent the state of one resource over time.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **rules**
  - **Technical definition:** AWS Config rules are compliance-evaluation logic units that test resource settings or account posture against desired policy.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **conformance packs**
  - **Technical definition:** Conformance packs are deployable bundles of AWS Config rules and remediation guidance for repeatable governance baselines.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **configuration aggregators**
  - **Technical definition:** Configuration aggregators consolidate Config data across accounts and Regions into one queryable compliance and inventory view.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **remediation actions**
  - **Technical definition:** Remediation actions are AWS Config-linked automation workflows that attempt to correct noncompliant state.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.

### AWS Security Hub and AWS Audit Manager

- **AWS Security Hub**
  - **Technical definition:** AWS Security Hub is AWS's centralized security posture and finding aggregation service for collecting, normalizing, and scoring findings across AWS security tools and partner sources.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **AWS Audit Manager**
  - **Technical definition:** AWS Audit Manager is AWS's evidence-collection service for mapping AWS control evidence to audit frameworks and producing assessment-ready artifacts.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **security standards**
  - **Technical definition:** security standards is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **controls**
  - **Technical definition:** Controls are the individual evaluative checks within Security Hub or Audit Manager frameworks that map to specific technical or compliance expectations.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **findings**
  - **Technical definition:** Findings are normalized security issue records collected or generated by AWS Security Hub and related integrations.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **insights**
  - **Technical definition:** Insights are Security Hub query-driven aggregations that highlight priority patterns across many findings.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **automation rules**
  - **Technical definition:** automation rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **finding aggregators**
  - **Technical definition:** finding aggregators is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **administrator accounts**
  - **Technical definition:** administrator accounts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **member accounts**
  - **Technical definition:** member accounts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **assessments**
  - **Technical definition:** Assessments are Audit Manager workspaces that collect evidence and control results against a chosen framework.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **frameworks**
  - **Technical definition:** Frameworks are Audit Manager assessment models that group controls and evidence expectations for a specific audit objective.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **control sets**
  - **Technical definition:** Control sets are grouped collections of related controls within an Audit Manager framework or assessment.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **evidence folders**
  - **Technical definition:** Evidence folders are Audit Manager containers used to organize collected control evidence by assessment and control scope.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **evidence**
  - **Technical definition:** evidence is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **Evidence Finder**
  - **Technical definition:** Evidence Finder is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **assessment reports**
  - **Technical definition:** assessment reports is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.

### AWS Artifact, AWS Health, and AWS Service Quotas

- **AWS Artifact**
  - **Technical definition:** AWS Artifact is AWS's portal for compliance reports, certifications, and contractual documents such as SOC reports and agreements.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **AWS Health**
  - **Technical definition:** AWS Health is AWS's service-event and account-impact feed for provider-side incidents, maintenance events, and resource-affecting notifications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **and AWS Service Quotas**
  - **Technical definition:** and AWS Service Quotas is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **reports**
  - **Technical definition:** Reports are AWS Artifact documents such as SOC, PCI, or ISO-related attestations made available for customer review.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **agreements**
  - **Technical definition:** Agreements are AWS legal or contractual artifacts exposed through AWS Artifact for account acceptance and governance.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **notifications**
  - **Technical definition:** notifications is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **AWS Health events**
  - **Technical definition:** AWS Health events is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **affected entities**
  - **Technical definition:** affected entities is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **event status codes**
  - **Technical definition:** event status codes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **service quotas**
  - **Technical definition:** Service quotas are the defined resource or API ceilings applied by AWS services to an account or Region.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **applied quota values**
  - **Technical definition:** applied quota values is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **quota increase requests**
  - **Technical definition:** quota increase requests is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **quota request templates**
  - **Technical definition:** quota request templates is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **usage metrics**
  - **Technical definition:** usage metrics is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.

## Controls

### Expanded Points

- **AWS Organizations policy inheritance**
  - **Technical definition:** AWS Organizations policy inheritance is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **trusted access**
  - **Technical definition:** trusted access is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **delegated administration**
  - **Technical definition:** delegated administration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **consolidated billing**
  - **Technical definition:** consolidated billing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **account contact ownership**
  - **Technical definition:** account contact ownership is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Region opt-in/opt-out**
  - **Technical definition:** Region opt-in/opt-out is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **account metadata governance**
  - **Technical definition:** account metadata governance is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Control Tower account baselines**
  - **Technical definition:** AWS Control Tower account baselines is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **control enablement**
  - **Technical definition:** control enablement is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **drift detection**
  - **Technical definition:** drift detection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **landing-zone versioning**
  - **Technical definition:** landing-zone versioning is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **control remediation**
  - **Technical definition:** control remediation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **IAM identity-based policies**
  - **Technical definition:** IAM identity-based policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **role assumption**
  - **Technical definition:** role assumption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **multi-factor authentication (MFA)**
  - **Technical definition:** multi-factor authentication (MFA) is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **attribute-based access control (ABAC)**
  - **Technical definition:** attribute-based access control (ABAC) is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Security Token Service (AWS STS)**
  - **Technical definition:** AWS Security Token Service (AWS STS) is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM Access Analyzer findings**
  - **Technical definition:** IAM Access Analyzer findings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS IAM Identity Center workforce identity federation**
  - **Technical definition:** AWS IAM Identity Center workforce identity federation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **permission-set provisioning**
  - **Technical definition:** permission-set provisioning is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **session duration**
  - **Technical definition:** session duration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **account assignment review**
  - **Technical definition:** account assignment review is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Service Catalog launch constraints**
  - **Technical definition:** AWS Service Catalog launch constraints is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **template constraints**
  - **Technical definition:** template constraints is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **tag update constraints**
  - **Technical definition:** tag update constraints is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **portfolio sharing**
  - **Technical definition:** portfolio sharing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **launch permissions**
  - **Technical definition:** launch permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Resource Access Manager resource-share permissions**
  - **Technical definition:** AWS Resource Access Manager resource-share permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS CloudFormation StackSets service-managed permissions**
  - **Technical definition:** AWS CloudFormation StackSets service-managed permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **self-managed permissions**
  - **Technical definition:** self-managed permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **automatic deployments**
  - **Technical definition:** automatic deployments is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **account filters**
  - **Technical definition:** account filters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Region targeting**
  - **Technical definition:** Region targeting is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **failure tolerance**
  - **Technical definition:** failure tolerance is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **concurrency**
  - **Technical definition:** concurrency is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS CloudTrail multi-Region trails**
  - **Technical definition:** AWS CloudTrail multi-Region trails is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **organization-wide logging**
  - **Technical definition:** organization-wide logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **log file integrity validation**
  - **Technical definition:** log file integrity validation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **retention**
  - **Technical definition:** retention is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Config organization aggregation**
  - **Technical definition:** AWS Config organization aggregation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Security Hub delegated administration**
  - **Technical definition:** AWS Security Hub delegated administration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region aggregation**
  - **Technical definition:** cross-Region aggregation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Foundational Security Best Practices**
  - **Technical definition:** AWS Foundational Security Best Practices is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **CIS AWS Foundations Benchmark**
  - **Technical definition:** CIS AWS Foundations Benchmark is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Audit Manager assessment scoping**
  - **Technical definition:** AWS Audit Manager assessment scoping is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Artifact report access**
  - **Technical definition:** AWS Artifact report access is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **agreement acceptance**
  - **Technical definition:** agreement acceptance is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Health organizational view**
  - **Technical definition:** AWS Health organizational view is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Service Quotas baselines**
  - **Technical definition:** Service Quotas baselines is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **utilization monitoring**
  - **Technical definition:** utilization monitoring is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **quota-risk review**
  - **Technical definition:** quota-risk review is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Expanded Points

- **Create a landing-zone account model with management account**
  - **Technical definition:** Create a landing-zone account model with management account is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **shared services accounts**
  - **Technical definition:** shared services accounts is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **workload accounts**
  - **Technical definition:** workload accounts is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **environment-specific OUs**
  - **Technical definition:** environment-specific OUs is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Design an SCP model for preventive guardrails without blocking required platform operations**
  - **Technical definition:** Design an SCP model for preventive guardrails without blocking required platform operations is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Enroll accounts and OUs into AWS Control Tower and review drift remediation paths**
  - **Technical definition:** Enroll accounts and OUs into AWS Control Tower and review drift remediation paths is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Review IAM role trust policies**
  - **Technical definition:** Review IAM role trust policies is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **access keys**
  - **Technical definition:** access keys is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **unused access findings**
  - **Technical definition:** unused access findings is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Provision AWS IAM Identity Center permission sets and validate account assignments**
  - **Technical definition:** Provision AWS IAM Identity Center permission sets and validate account assignments is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Use AWS CloudFormation StackSets for multi-account baseline deployment and rollback**
  - **Technical definition:** Use AWS CloudFormation StackSets for multi-account baseline deployment and rollback is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Investigate API activity with AWS CloudTrail and validate drift/compliance with AWS Config**
  - **Technical definition:** Investigate API activity with AWS CloudTrail and validate drift/compliance with AWS Config is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Triage AWS Security Hub findings and prepare AWS Audit Manager evidence for compliance review**
  - **Technical definition:** Triage AWS Security Hub findings and prepare AWS Audit Manager evidence for compliance review is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Validate landing-zone readiness before migration by checking identity**
  - **Technical definition:** Validate landing-zone readiness before migration by checking identity is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **audit**
  - **Technical definition:** audit is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **account ownership**
  - **Technical definition:** account ownership is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **quotas**
  - **Technical definition:** quotas is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **health events**
  - **Technical definition:** health events is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 2: Networking, Private Connectivity, And Edge Traffic

## Service And Entity Coverage

### Amazon Virtual Private Cloud (Amazon VPC)

- **Amazon Virtual Private Cloud (Amazon VPC)**
  - **Technical definition:** Amazon VPC is AWS's logically isolated virtual network construct for IP addressing, segmentation, routing, and private service access within a Region.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **VPCs**
  - **Technical definition:** Within Amazon VPC, VPCs is a network-scoped construct that controls address allocation, attachment behavior, routing, segmentation, or path visibility inside a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **subnets**
  - **Technical definition:** Subnets are Availability Zone-scoped CIDR partitions inside a VPC used to place resources and attach subnet-level controls.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **route tables**
  - **Technical definition:** Route tables are subnet- or gateway-associated routing policies that determine the next hop for traffic leaving a subnet.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **internet gateways**
  - **Technical definition:** Internet gateways are horizontally scaled VPC attachments that enable IPv4 public internet reachability for routable subnets and public IPs.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **egress-only internet gateways**
  - **Technical definition:** Egress-only internet gateways allow outbound-only IPv6 internet connectivity from a VPC without permitting unsolicited inbound initiation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **NAT gateways**
  - **Technical definition:** NAT gateways provide managed source-NAT for outbound internet access from private subnets while blocking unsolicited inbound sessions.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **elastic IP addresses**
  - **Technical definition:** Within Amazon VPC, elastic IP addresses is a network-scoped construct that controls address allocation, attachment behavior, routing, segmentation, or path visibility inside a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **elastic network interfaces**
  - **Technical definition:** Within Amazon VPC, elastic network interfaces is a network-scoped construct that controls address allocation, attachment behavior, routing, segmentation, or path visibility inside a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **prefix lists**
  - **Technical definition:** Within Amazon VPC, prefix lists is a network-scoped construct that controls address allocation, attachment behavior, routing, segmentation, or path visibility inside a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **DHCP option sets**
  - **Technical definition:** DHCP option sets define the DHCP-delivered DNS and network settings used by instances in a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **security groups**
  - **Technical definition:** Security groups are stateful ENI-attached firewalls that evaluate explicit inbound and outbound allow rules.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **network ACLs**
  - **Technical definition:** Within Amazon VPC, network ACLs is a network-scoped construct that controls address allocation, attachment behavior, routing, segmentation, or path visibility inside a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **VPC Flow Logs**
  - **Technical definition:** VPC Flow Logs are metadata records of accepted or rejected network traffic observed at ENI, subnet, or VPC scope.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Amazon VPC Reachability Analyzer**
  - **Technical definition:** Within Amazon VPC, Amazon VPC Reachability Analyzer is a network-scoped construct that controls address allocation, attachment behavior, routing, segmentation, or path visibility inside a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Amazon VPC Network Access Analyzer**
  - **Technical definition:** Within Amazon VPC, Amazon VPC Network Access Analyzer is a network-scoped construct that controls address allocation, attachment behavior, routing, segmentation, or path visibility inside a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Amazon VPC IP Address Manager (IPAM)**
  - **Technical definition:** Within Amazon VPC, Amazon VPC IP Address Manager (IPAM) is a network-scoped construct that controls address allocation, attachment behavior, routing, segmentation, or path visibility inside a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### AWS Transit Gateway and AWS Cloud WAN

- **AWS Transit Gateway**
  - **Technical definition:** AWS Transit Gateway is AWS's regional network transit hub for connecting many VPCs, VPNs, and Direct Connect attachments through centrally managed route tables.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **AWS Cloud WAN**
  - **Technical definition:** AWS Cloud WAN is AWS's managed global network control plane for building segmented wide-area connectivity across Regions and on-premises sites.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **transit gateways**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, transit gateways is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **transit gateway attachments**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, transit gateway attachments is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **transit gateway route tables**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, transit gateway route tables is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **route table associations**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, route table associations is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **route table propagations**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, route table propagations is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **peering attachments**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, peering attachments is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **global networks**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, global networks is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **core networks**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, core networks is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **core network policies**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, core network policies is a transit-network construct that shapes attachment topology, route-domain behavior, segmentation, or inter-Region propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **segments**
  - **Technical definition:** Segments are Cloud WAN routing-isolation domains used to separate traffic and policy between network constituencies.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **attachments**
  - **Technical definition:** Attachments are the logical connections that link VPCs, VPNs, Direct Connect gateways, or other domains into a Transit Gateway or Cloud WAN core network.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **network function groups**
  - **Technical definition:** Network function groups are Cloud WAN constructs used to steer traffic through inserted network appliances or inspection paths.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### Amazon VPC Peering, Amazon VPC Endpoints, and AWS PrivateLink

- **Amazon VPC Peering**
  - **Technical definition:** Amazon VPC Peering is AWS's direct private routing relationship between two VPCs without a separate transit service.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Amazon VPC Endpoints**
  - **Technical definition:** Amazon VPC Endpoints are private-access constructs that let resources inside a VPC reach supported AWS services without traversing the public internet.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **and AWS PrivateLink**
  - **Technical definition:** and AWS PrivateLink is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **VPC peering connections**
  - **Technical definition:** Within VPC peering, VPC endpoints, or AWS PrivateLink, VPC peering connections is a private-connectivity construct that determines how services or networks communicate without public internet traversal.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **requester VPCs**
  - **Technical definition:** Within VPC peering, VPC endpoints, or AWS PrivateLink, requester VPCs is a private-connectivity construct that determines how services or networks communicate without public internet traversal.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **accepter VPCs**
  - **Technical definition:** Within VPC peering, VPC endpoints, or AWS PrivateLink, accepter VPCs is a private-connectivity construct that determines how services or networks communicate without public internet traversal.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **interface endpoints**
  - **Technical definition:** Interface endpoints are ENI-based VPC endpoints that privately expose supported AWS or PrivateLink services into a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **gateway endpoints**
  - **Technical definition:** Gateway endpoints are VPC endpoint types for S3 and DynamoDB that add private service routes without ENI-based endpoints.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Gateway Load Balancer endpoints**
  - **Technical definition:** Within VPC peering, VPC endpoints, or AWS PrivateLink, Gateway Load Balancer endpoints is a private-connectivity construct that determines how services or networks communicate without public internet traversal.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **endpoint services**
  - **Technical definition:** Endpoint services are the PrivateLink-published service definitions fronted by NLBs and consumable through interface endpoints.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **endpoint network interfaces**
  - **Technical definition:** Within VPC peering, VPC endpoints, or AWS PrivateLink, endpoint network interfaces is a private-connectivity construct that determines how services or networks communicate without public internet traversal.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **endpoint policies**
  - **Technical definition:** Endpoint policies are resource policies attached to supported VPC endpoints that further constrain which requests may traverse the endpoint.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **private DNS names**
  - **Technical definition:** Within VPC peering, VPC endpoints, or AWS PrivateLink, private DNS names is a private-connectivity construct that determines how services or networks communicate without public internet traversal.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### Amazon VPC Lattice and AWS Verified Access

- **Amazon VPC Lattice**
  - **Technical definition:** Amazon VPC Lattice is AWS's application networking service for service-to-service routing, auth policy, and traffic management across VPC and account boundaries.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **AWS Verified Access**
  - **Technical definition:** AWS Verified Access is AWS's identity- and device-aware application access service for enforcing zero-trust style access decisions to private applications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **service networks**
  - **Technical definition:** Service networks are VPC Lattice groupings that connect services and associated VPCs under a shared application-network domain.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **services**
  - **Technical definition:** Services in VPC Lattice are the application endpoints exposed through the lattice data plane and governed by lattice routing and auth policy.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **target groups**
  - **Technical definition:** Target groups are collections of backend endpoints receiving traffic after request matching and health evaluation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **listeners**
  - **Technical definition:** Listeners are protocol-and-port entry definitions that accept traffic into a VPC Lattice service or load-balancing layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **rules**
  - **Technical definition:** Rules are match-and-action objects that determine which requests are routed to which target groups or backends.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **auth policies**
  - **Technical definition:** Auth policies are VPC Lattice access policies that control who may invoke a service at the application-network layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **resource configurations**
  - **Technical definition:** resource configurations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Verified Access instances**
  - **Technical definition:** Verified Access instances is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Verified Access groups**
  - **Technical definition:** Verified Access groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Verified Access endpoints**
  - **Technical definition:** Verified Access endpoints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **trust providers**
  - **Technical definition:** Trust providers are the integrated identity or device-context sources Verified Access uses to evaluate access policy.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **access policies**
  - **Technical definition:** access policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **application domains**
  - **Technical definition:** application domains is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### AWS Network Firewall and AWS Firewall Manager

- **AWS Network Firewall**
  - **Technical definition:** AWS Network Firewall is AWS's managed stateful and stateless network-inspection service for filtering, domain controls, and deep packet policy in VPC traffic paths.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **AWS Firewall Manager**
  - **Technical definition:** AWS Firewall Manager is AWS's organization-wide security policy manager for centrally deploying and auditing supported WAF, network firewall, and related protections.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **firewalls**
  - **Technical definition:** firewalls is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **firewall policies**
  - **Technical definition:** Firewall policies are AWS Network Firewall rule containers that define which stateless and stateful rule groups are applied to traffic.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **rule groups**
  - **Technical definition:** rule groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **stateless rules**
  - **Technical definition:** stateless rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **stateful rules**
  - **Technical definition:** stateful rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **TLS inspection configurations**
  - **Technical definition:** TLS inspection configurations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **alert logs**
  - **Technical definition:** alert logs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **flow logs**
  - **Technical definition:** flow logs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Firewall Manager policies**
  - **Technical definition:** Firewall Manager policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **policy scopes**
  - **Technical definition:** policy scopes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **resource sets**
  - **Technical definition:** resource sets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **compliance status**
  - **Technical definition:** Compliance status is Firewall Manager's view of whether targeted resources still conform to the centrally defined policy.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### Elastic Load Balancing and AWS Certificate Manager

- **Elastic Load Balancing**
  - **Technical definition:** Elastic Load Balancing is AWS's managed traffic-distribution platform for routing requests to healthy targets at Layer 4 or Layer 7.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **AWS Certificate Manager**
  - **Technical definition:** AWS Certificate Manager is AWS's managed certificate lifecycle service for issuing, importing, renewing, and binding TLS certificates to supported AWS endpoints.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Application Load Balancers**
  - **Technical definition:** Within Elastic Load Balancing or ACM, Application Load Balancers is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Network Load Balancers**
  - **Technical definition:** Within Elastic Load Balancing or ACM, Network Load Balancers is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Gateway Load Balancers**
  - **Technical definition:** Within Elastic Load Balancing or ACM, Gateway Load Balancers is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **target groups**
  - **Technical definition:** Target groups are collections of registered backends with health checks and routing association from the load balancer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **listeners**
  - **Technical definition:** Listeners are port/protocol entry definitions that accept traffic on a load balancer and bind it to routing rules.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **listener rules**
  - **Technical definition:** Listener rules are ordered request-matching statements that determine which target group receives a request on an ALB.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **health checks**
  - **Technical definition:** Health checks are active probes used by ELB to decide whether a backend should remain eligible for traffic.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **load balancer attributes**
  - **Technical definition:** Within Elastic Load Balancing or ACM, load balancer attributes is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **public certificates**
  - **Technical definition:** Within Elastic Load Balancing or ACM, public certificates is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **private certificates**
  - **Technical definition:** Within Elastic Load Balancing or ACM, private certificates is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **imported certificates**
  - **Technical definition:** Within Elastic Load Balancing or ACM, imported certificates is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **domain names**
  - **Technical definition:** Within Elastic Load Balancing or ACM, domain names is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **subject alternative names (SANs)**
  - **Technical definition:** Within Elastic Load Balancing or ACM, subject alternative names (SANs) is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **validation options**
  - **Technical definition:** Within Elastic Load Balancing or ACM, validation options is a traffic-distribution or TLS identity construct that affects request acceptance, target routing, and certificate-backed trust.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### AWS Site-to-Site VPN, AWS Client VPN, and AWS Direct Connect

- **AWS Site-to-Site VPN**
  - **Technical definition:** AWS Site-to-Site VPN is AWS's IPSec-based hybrid network service for connecting customer gateways to AWS network boundaries over encrypted tunnels.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **AWS Client VPN**
  - **Technical definition:** AWS Client VPN is AWS's managed remote-access VPN service for connecting end users securely into AWS VPC environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **and AWS Direct Connect**
  - **Technical definition:** and AWS Direct Connect is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **customer gateways**
  - **Technical definition:** Customer gateways are AWS representations of on-premises VPN devices and their external connection parameters.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **virtual private gateways**
  - **Technical definition:** Virtual private gateways are the AWS-side VPN termination and route-attachment points used with Site-to-Site VPN on a VPC.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **VPN connections**
  - **Technical definition:** VPN connections is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **VPN tunnels**
  - **Technical definition:** VPN tunnels is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **tunnel options**
  - **Technical definition:** Tunnel options define IPSec parameters, BGP settings, and inside tunnel behavior that shape VPN compatibility and failover behavior.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **customer gateway devices**
  - **Technical definition:** customer gateway devices is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Client VPN endpoints**
  - **Technical definition:** Client VPN endpoints are the managed VPN listener resources users connect to for remote access into AWS networks.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **target network associations**
  - **Technical definition:** target network associations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **authorization rules**
  - **Technical definition:** Authorization rules define which CIDR destinations connected Client VPN users are allowed to reach.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Direct Connect connections**
  - **Technical definition:** Direct Connect connections are the physical dedicated links between customer premises and AWS Direct Connect locations.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **link aggregation groups (LAGs)**
  - **Technical definition:** link aggregation groups (LAGs) is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **private virtual interfaces**
  - **Technical definition:** Private virtual interfaces are Direct Connect logical interfaces used to reach private AWS address space such as VPC routes via DXGW/VGW/TGW.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **public virtual interfaces**
  - **Technical definition:** public virtual interfaces is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **transit virtual interfaces**
  - **Technical definition:** Transit virtual interfaces are Direct Connect logical interfaces used to reach Transit Gateway-attached route domains.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Direct Connect gateways**
  - **Technical definition:** Direct Connect gateways are AWS routing abstractions that connect Direct Connect virtual interfaces to multiple VPCs or Transit Gateways.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **hosted connections**
  - **Technical definition:** hosted connections is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### Amazon Route 53 and Amazon Route 53 Resolver

- **Amazon Route 53**
  - **Technical definition:** Amazon Route 53 is AWS's DNS service for authoritative zones, health-checked routing, domain registration, and traffic steering.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Amazon Route 53 Resolver**
  - **Technical definition:** Amazon Route 53 Resolver is AWS's recursive and hybrid DNS component for inbound/outbound forwarding, DNS firewalling, and VPC name-resolution control.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **hosted zones**
  - **Technical definition:** Hosted zones are Route 53 namespace containers that hold DNS records for a domain and control authoritative answer publication.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **records**
  - **Technical definition:** records is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **routing policies**
  - **Technical definition:** Routing policies define how Route 53 chooses among multiple possible answers, such as weighted, failover, latency, or geolocation-based responses.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **health checks**
  - **Technical definition:** Health checks are Route 53 probes or status sources used to influence failover and routing decisions.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **reusable delegation sets**
  - **Technical definition:** reusable delegation sets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **domains**
  - **Technical definition:** domains is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **key-signing keys (KSKs)**
  - **Technical definition:** key-signing keys (KSKs) is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **inbound Resolver endpoints**
  - **Technical definition:** inbound Resolver endpoints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **outbound Resolver endpoints**
  - **Technical definition:** outbound Resolver endpoints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **Resolver rules**
  - **Technical definition:** Resolver rules define which DNS suffixes should be forwarded to which outbound targets.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **rule associations**
  - **Technical definition:** rule associations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **query logging configurations**
  - **Technical definition:** query logging configurations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **DNS Firewall rule groups**
  - **Technical definition:** DNS Firewall rule groups are Route 53 Resolver DNS filtering policies used to allow, block, or alert on specific DNS domains or patterns.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **domain lists**
  - **Technical definition:** domain lists is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### Amazon CloudFront, AWS WAF, AWS Shield, and AWS Global Accelerator

- **Amazon CloudFront**
  - **Technical definition:** Amazon CloudFront is AWS's global edge delivery network for caching, request routing, and edge-side application handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **AWS WAF**
  - **Technical definition:** AWS WAF is AWS's application-layer filtering service for HTTP request inspection, rate limiting, and rule-driven blocking, allow, or label actions.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **AWS Shield**
  - **Technical definition:** AWS Shield is AWS's DDoS protection service for AWS resources, with Shield Advanced adding stronger detection, response support, and economic protections.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **and AWS Global Accelerator**
  - **Technical definition:** and AWS Global Accelerator is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **distributions**
  - **Technical definition:** Distributions are CloudFront configurations that define origins, cache behavior, TLS settings, and edge processing rules for delivered content.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **origins**
  - **Technical definition:** Origins are the upstream content or application endpoints CloudFront fetches from when cache misses occur.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **cache behaviors**
  - **Technical definition:** Cache behaviors are CloudFront path-based rule blocks that control caching, request forwarding, and edge treatment.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **cache policies**
  - **Technical definition:** Cache policies define which request attributes contribute to cache key selection and TTL behavior in CloudFront.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **origin request policies**
  - **Technical definition:** Origin request policies define which headers, cookies, and query strings CloudFront forwards to origins.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **response headers policies**
  - **Technical definition:** response headers policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **invalidations**
  - **Technical definition:** invalidations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **key groups**
  - **Technical definition:** key groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **origin access controls**
  - **Technical definition:** origin access controls is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **web ACLs**
  - **Technical definition:** Web ACLs are WAF policy containers that group request-inspection rules and attach them to protected resources.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **rules**
  - **Technical definition:** rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **rule groups**
  - **Technical definition:** Rule groups are reusable sets of WAF rules that can be attached inside a web ACL.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **managed rule groups**
  - **Technical definition:** managed rule groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **IP sets**
  - **Technical definition:** IP sets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **labels**
  - **Technical definition:** labels is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **protections**
  - **Technical definition:** protections is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **protection groups**
  - **Technical definition:** protection groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **accelerators**
  - **Technical definition:** Accelerators are Global Accelerator resources that expose static anycast IPs and route traffic to healthy regional endpoints.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **listeners**
  - **Technical definition:** Listeners in Global Accelerator are protocol/port definitions used to accept traffic on an accelerator.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **endpoint groups**
  - **Technical definition:** Endpoint groups are Regional collections of accelerator backends and traffic settings.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **endpoints**
  - **Technical definition:** endpoints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **static anycast IP addresses**
  - **Technical definition:** static anycast IP addresses is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **endpoint weights**
  - **Technical definition:** endpoint weights is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **traffic dials**
  - **Technical definition:** traffic dials is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### Amazon API Gateway

- **Amazon API Gateway**
  - **Technical definition:** Amazon API Gateway is AWS's managed API front door for REST, HTTP, and WebSocket APIs with auth, throttling, staging, and integration workflows.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **REST APIs**
  - **Technical definition:** REST APIs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **HTTP APIs**
  - **Technical definition:** HTTP APIs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **WebSocket APIs**
  - **Technical definition:** WebSocket APIs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **resources**
  - **Technical definition:** Resources are REST API path components in API Gateway that structure hierarchical URL paths.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **routes**
  - **Technical definition:** routes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **methods**
  - **Technical definition:** Methods are operation definitions such as GET or POST attached to API resources and bound to integrations or auth behavior.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **integrations**
  - **Technical definition:** Integrations are the backend connectivity definitions linking API Gateway methods to Lambda, HTTP, AWS services, or mock targets.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **stages**
  - **Technical definition:** Stages are named API deployment environments that expose a specific deployed API configuration to callers.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **deployments**
  - **Technical definition:** Deployments are immutable API Gateway snapshots of API configuration promoted into stages.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **authorizers**
  - **Technical definition:** Authorizers are API Gateway auth components that evaluate identity or tokens before a request is forwarded to the backend.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **usage plans**
  - **Technical definition:** Usage plans define request quotas and throttling limits associated with API keys in supported API Gateway modes.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **API keys**
  - **Technical definition:** API keys are caller-identification tokens used with usage plans for throttling and metering, not primary strong authentication.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **custom domain names**
  - **Technical definition:** custom domain names is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **VPC links**
  - **Technical definition:** VPC links is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

## Controls

### Expanded Points

- **VPC CIDR allocation**
  - **Technical definition:** VPC CIDR allocation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **subnet routing**
  - **Technical definition:** subnet routing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **route table associations**
  - **Technical definition:** route table associations is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **security group rules**
  - **Technical definition:** security group rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **network ACL rules**
  - **Technical definition:** network ACL rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **flow log delivery**
  - **Technical definition:** flow log delivery is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IPAM pool rules**
  - **Technical definition:** IPAM pool rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **overlapping CIDR detection**
  - **Technical definition:** overlapping CIDR detection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Transit Gateway route domains**
  - **Technical definition:** Transit Gateway route domains is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **attachment acceptance**
  - **Technical definition:** attachment acceptance is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **route propagation**
  - **Technical definition:** route propagation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **appliance mode**
  - **Technical definition:** appliance mode is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Resource Access Manager sharing**
  - **Technical definition:** AWS Resource Access Manager sharing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Cloud WAN segment isolation**
  - **Technical definition:** Cloud WAN segment isolation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **attachment policies**
  - **Technical definition:** attachment policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **route sharing**
  - **Technical definition:** route sharing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **VPC peering non-overlapping CIDRs**
  - **Technical definition:** VPC peering non-overlapping CIDRs is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint policies**
  - **Technical definition:** endpoint policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **private DNS**
  - **Technical definition:** private DNS is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **allowed principals**
  - **Technical definition:** allowed principals is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **acceptance settings**
  - **Technical definition:** acceptance settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **private service exposure boundaries**
  - **Technical definition:** private service exposure boundaries is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **VPC Lattice service network associations**
  - **Technical definition:** VPC Lattice service network associations is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **routing rules**
  - **Technical definition:** routing rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **TLS settings**
  - **Technical definition:** TLS settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **target group health checks**
  - **Technical definition:** target group health checks is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Verified Access identity trust**
  - **Technical definition:** Verified Access identity trust is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **device trust**
  - **Technical definition:** device trust is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **policy-based access**
  - **Technical definition:** policy-based access is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **access log delivery**
  - **Technical definition:** access log delivery is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Network Firewall centralized inspection**
  - **Technical definition:** AWS Network Firewall centralized inspection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Suricata-compatible rules**
  - **Technical definition:** Suricata-compatible rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **domain list rules**
  - **Technical definition:** domain list rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **default actions**
  - **Technical definition:** default actions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Firewall Manager policies**
  - **Technical definition:** AWS Firewall Manager policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **security group policies**
  - **Technical definition:** security group policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Elastic Load Balancing TLS security policies**
  - **Technical definition:** Elastic Load Balancing TLS security policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **certificates**
  - **Technical definition:** certificates is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **target group attributes**
  - **Technical definition:** target group attributes is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-zone load balancing**
  - **Technical definition:** cross-zone load balancing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **access logs**
  - **Technical definition:** access logs is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Certificate Manager managed renewal**
  - **Technical definition:** AWS Certificate Manager managed renewal is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **VPN IKE/IPsec parameters**
  - **Technical definition:** VPN IKE/IPsec parameters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **BGP routing**
  - **Technical definition:** BGP routing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **tunnel inside CIDRs**
  - **Technical definition:** tunnel inside CIDRs is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **pre-shared keys**
  - **Technical definition:** pre-shared keys is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Client VPN authorization rules**
  - **Technical definition:** Client VPN authorization rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Direct Connect BGP authentication**
  - **Technical definition:** Direct Connect BGP authentication is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **allowed prefixes**
  - **Technical definition:** allowed prefixes is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **MACsec**
  - **Technical definition:** MACsec is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **resiliency model**
  - **Technical definition:** resiliency model is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Route 53 TTLs**
  - **Technical definition:** Route 53 TTLs is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **routing policies**
  - **Technical definition:** routing policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **health-check routing**
  - **Technical definition:** health-check routing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **DNSSEC signing**
  - **Technical definition:** DNSSEC signing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **delegation**
  - **Technical definition:** delegation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Route 53 Resolver conditional forwarding**
  - **Technical definition:** Route 53 Resolver conditional forwarding is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **DNS Firewall rules**
  - **Technical definition:** DNS Firewall rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **query logging**
  - **Technical definition:** query logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **CloudFront viewer protocol policies**
  - **Technical definition:** CloudFront viewer protocol policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **signed URLs**
  - **Technical definition:** signed URLs is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **signed cookies**
  - **Technical definition:** signed cookies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **origin access control**
  - **Technical definition:** origin access control is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS WAF rate-based rules**
  - **Technical definition:** AWS WAF rate-based rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Bot Control**
  - **Technical definition:** Bot Control is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Shield Advanced**
  - **Technical definition:** AWS Shield Advanced is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Global Accelerator traffic dials**
  - **Technical definition:** Global Accelerator traffic dials is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint weights**
  - **Technical definition:** endpoint weights is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **API Gateway authorizers**
  - **Technical definition:** API Gateway authorizers is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource policies**
  - **Technical definition:** resource policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **mutual TLS**
  - **Technical definition:** mutual TLS is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **stage variables**
  - **Technical definition:** stage variables is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **request validation**
  - **Technical definition:** request validation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS WAF associations**
  - **Technical definition:** AWS WAF associations is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Expanded Points

- **Validate VPC packet paths with route tables**
  - **Technical definition:** Validate VPC packet paths with route tables is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **security groups**
  - **Technical definition:** security groups is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **network ACLs**
  - **Technical definition:** network ACLs is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Reachability Analyzer**
  - **Technical definition:** Reachability Analyzer is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Network Access Analyzer**
  - **Technical definition:** Network Access Analyzer is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Review NAT gateway saturation**
  - **Technical definition:** Review NAT gateway saturation is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IPAM allocation**
  - **Technical definition:** IPAM allocation is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **route drift**
  - **Technical definition:** route drift is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **overlapping CIDRs before migration**
  - **Technical definition:** overlapping CIDRs before migration is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Onboard Transit Gateway attachments**
  - **Technical definition:** Onboard Transit Gateway attachments is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate route propagation**
  - **Technical definition:** validate route propagation is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **test segmentation between network domains**
  - **Technical definition:** test segmentation between network domains is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Publish or consume private services through VPC endpoints and AWS PrivateLink without internet exposure**
  - **Technical definition:** Publish or consume private services through VPC endpoints and AWS PrivateLink without internet exposure is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Investigate dropped flows in AWS Network Firewall and tune rule groups safely**
  - **Technical definition:** Investigate dropped flows in AWS Network Firewall and tune rule groups safely is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Review Elastic Load Balancing target health**
  - **Technical definition:** Review Elastic Load Balancing target health is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **listener rules**
  - **Technical definition:** listener rules is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **TLS certificates**
  - **Technical definition:** TLS certificates is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **traffic shifting**
  - **Technical definition:** traffic shifting is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Test Site-to-Site VPN tunnel failover and Direct Connect backup routing**
  - **Technical definition:** Test Site-to-Site VPN tunnel failover and Direct Connect backup routing is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Execute DNS cutover with Route 53 records**
  - **Technical definition:** Execute DNS cutover with Route 53 records is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Resolver rules**
  - **Technical definition:** Resolver rules is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rollback TTL planning**
  - **Technical definition:** rollback TTL planning is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Tune CloudFront cache behavior**
  - **Technical definition:** Tune CloudFront cache behavior is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **origin failover**
  - **Technical definition:** origin failover is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS WAF false positives**
  - **Technical definition:** AWS WAF false positives is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Global Accelerator failover**
  - **Technical definition:** Global Accelerator failover is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Deploy and troubleshoot API Gateway stages**
  - **Technical definition:** Deploy and troubleshoot API Gateway stages is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **private integrations**
  - **Technical definition:** private integrations is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **access logs**
  - **Technical definition:** access logs is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **canary releases**
  - **Technical definition:** canary releases is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 3: Compute, Kubernetes, CI/CD, And Platform Delivery

## Service And Entity Coverage

### Amazon Elastic Compute Cloud (Amazon EC2) and Amazon EC2 Auto Scaling

- **Amazon Elastic Compute Cloud (Amazon EC2)**
  - **Technical definition:** Amazon EC2 is AWS's virtual machine service for provisioning instance-based compute with full guest-OS control and AWS-integrated networking, storage, and identity.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Amazon EC2 Auto Scaling**
  - **Technical definition:** Amazon EC2 Auto Scaling is AWS's fleet-scaling control plane for adjusting instance counts based on policy, schedules, health, and demand signals.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **instances**
  - **Technical definition:** Instances are the running EC2 virtual machines that provide guest OS, attached storage, ENIs, and compute capacity.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Amazon Machine Images (AMIs)**
  - **Technical definition:** AMIs are machine templates that define the base OS and block-device mapping used to launch EC2 instances.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **instance types**
  - **Technical definition:** Instance types are hardware profile definitions that specify vCPU, memory, networking, storage characteristics, and accelerator options.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **key pairs**
  - **Technical definition:** key pairs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **elastic network interfaces**
  - **Technical definition:** elastic network interfaces is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **instance profiles**
  - **Technical definition:** instance profiles is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **user data**
  - **Technical definition:** user data is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Elastic IP addresses**
  - **Technical definition:** Elastic IP addresses are static public IPv4 addresses that can be remapped between resources within an account scope.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **EC2 Image Builder image pipelines**
  - **Technical definition:** EC2 Image Builder image pipelines is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Auto Scaling groups**
  - **Technical definition:** Auto Scaling groups are EC2 fleet containers that maintain desired instance count and react to scaling policies or health changes.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **launch templates**
  - **Technical definition:** Launch templates are versioned EC2 launch specifications that define AMI, instance type, IAM profile, networking, and metadata settings.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **scaling policies**
  - **Technical definition:** Scaling policies define the conditions and increments by which an Auto Scaling group adds or removes instances.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **scheduled actions**
  - **Technical definition:** scheduled actions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **lifecycle hooks**
  - **Technical definition:** lifecycle hooks is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **warm pools**
  - **Technical definition:** Warm pools are pre-initialized instances held by an Auto Scaling group to reduce scale-out latency.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **instance refreshes**
  - **Technical definition:** instance refreshes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **health checks**
  - **Technical definition:** Health checks are the signals Auto Scaling uses to decide whether an instance is healthy enough to remain in service.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.

### Amazon Elastic Container Registry (Amazon ECR)

- **Amazon Elastic Container Registry (Amazon ECR)**
  - **Technical definition:** Amazon ECR is AWS's OCI-compatible artifact registry for storing, replicating, scanning, and distributing container images and related artifacts.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **private repositories**
  - **Technical definition:** private repositories is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **public repositories**
  - **Technical definition:** public repositories is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **images**
  - **Technical definition:** Images are the OCI manifests and referenced layers stored in ECR for container deployment and promotion.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **tags**
  - **Technical definition:** tags is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **manifests**
  - **Technical definition:** manifests is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **lifecycle policies**
  - **Technical definition:** Lifecycle policies are ECR cleanup rules that expire images based on tag and retention conditions.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **pull through cache rules**
  - **Technical definition:** pull through cache rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **replication configurations**
  - **Technical definition:** Replication configurations are ECR settings that copy repository content across Regions or accounts.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.

### Amazon Elastic Container Service (Amazon ECS) and AWS Fargate

- **Amazon Elastic Container Service (Amazon ECS)**
  - **Technical definition:** Amazon ECS is AWS's container orchestration service for running tasks and long-lived services across EC2 or Fargate capacity with AWS-native scheduling semantics.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **AWS Fargate**
  - **Technical definition:** AWS Fargate is AWS's serverless container compute engine for running ECS tasks or EKS pods without direct worker-node management.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **clusters**
  - **Technical definition:** Clusters are ECS logical grouping boundaries for running services and tasks and associating them with capacity providers.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **services**
  - **Technical definition:** Services are ECS controllers that keep a desired number of task replicas running and optionally fronted by load balancers.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **task definitions**
  - **Technical definition:** Task definitions are versioned workload specifications that declare container images, CPU, memory, networking, IAM roles, and runtime settings.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **tasks**
  - **Technical definition:** Tasks are the running instantiations of an ECS task definition, representing one scheduled execution unit.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **capacity providers**
  - **Technical definition:** Capacity providers define which compute capacity model ECS should use, such as EC2-backed or Fargate-backed execution.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **container instances**
  - **Technical definition:** container instances is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **service deployments**
  - **Technical definition:** service deployments is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **task sets**
  - **Technical definition:** task sets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Amazon ECS tasks on AWS Fargate**
  - **Technical definition:** Amazon ECS tasks on AWS Fargate is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Amazon ECS services on AWS Fargate**
  - **Technical definition:** Amazon ECS services on AWS Fargate is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.

### Amazon Elastic Kubernetes Service (Amazon EKS) and Karpenter

- **Amazon Elastic Kubernetes Service (Amazon EKS)**
  - **Technical definition:** Amazon EKS is AWS's managed Kubernetes control plane service for running Kubernetes clusters integrated with AWS identity, networking, and storage primitives.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Karpenter**
  - **Technical definition:** Karpenter is a Kubernetes-aware node-provisioning controller, commonly used with EKS, that creates EC2 capacity directly from pod scheduling requirements.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **clusters**
  - **Technical definition:** Clusters are EKS-managed Kubernetes control plane instances with associated worker-node and networking integration.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **control planes**
  - **Technical definition:** control planes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **managed node groups**
  - **Technical definition:** Managed node groups are AWS-managed sets of EC2 worker nodes with coordinated lifecycle and upgrade operations for EKS.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **self-managed nodes**
  - **Technical definition:** Self-managed nodes are customer-managed Kubernetes workers registered into EKS without node-group lifecycle automation from AWS.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Fargate profiles**
  - **Technical definition:** Fargate profiles are EKS scheduling definitions that map selected pods into Fargate-based serverless execution.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Amazon EKS pods on AWS Fargate**
  - **Technical definition:** Amazon EKS pods on AWS Fargate is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **add-ons**
  - **Technical definition:** add-ons is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **access entries**
  - **Technical definition:** access entries is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Kubernetes service accounts**
  - **Technical definition:** Kubernetes service accounts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **pod identity associations**
  - **Technical definition:** pod identity associations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Karpenter NodePool resources**
  - **Technical definition:** Karpenter NodePool resources is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **EC2NodeClass resources**
  - **Technical definition:** EC2NodeClass resources is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **NodeClaim resources**
  - **Technical definition:** NodeClaim resources is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.

### AWS Lambda

- **AWS Lambda**
  - **Technical definition:** AWS Lambda is AWS's event-driven serverless compute runtime that executes functions on demand with managed scaling and request-based billing.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **functions**
  - **Technical definition:** Functions are the deployable Lambda code units executed in response to events or direct invocation.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **versions**
  - **Technical definition:** Versions are immutable Lambda snapshots of function code and configuration created for stable deployment references.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **aliases**
  - **Technical definition:** Aliases are mutable Lambda pointers to versions, commonly used for traffic shifting and release management.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **layers**
  - **Technical definition:** Layers are reusable Lambda code or dependency packages mounted into functions at runtime.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **event source mappings**
  - **Technical definition:** Event source mappings are Lambda-managed pollers that connect stream or queue sources to function invocation.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **function URLs**
  - **Technical definition:** function URLs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **execution roles**
  - **Technical definition:** execution roles is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **provisioned concurrency**
  - **Technical definition:** Provisioned concurrency is pre-initialized execution capacity kept ready to reduce cold-start latency for a function version or alias.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **reserved concurrency**
  - **Technical definition:** Reserved concurrency is a per-function capacity reservation and throttle ceiling that isolates execution slots from other functions.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.

### AWS App Runner, AWS Elastic Beanstalk, and Amazon Lightsail

- **AWS App Runner**
  - **Technical definition:** AWS App Runner is AWS's managed application hosting service for containerized or source-based web applications with simplified deployment and autoscaling.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **AWS Elastic Beanstalk**
  - **Technical definition:** AWS Elastic Beanstalk is AWS's managed application orchestration service that provisions and coordinates infrastructure for supported application platforms.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **and Amazon Lightsail**
  - **Technical definition:** and Amazon Lightsail is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **App Runner services**
  - **Technical definition:** App Runner services is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **source configurations**
  - **Technical definition:** source configurations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **VPC connectors**
  - **Technical definition:** VPC connectors is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **custom domains**
  - **Technical definition:** custom domains is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Elastic Beanstalk applications**
  - **Technical definition:** Elastic Beanstalk applications is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **application versions**
  - **Technical definition:** application versions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **environments**
  - **Technical definition:** Environments are the deployed runtime containers of configuration, compute, scaling, and networking state in higher-level app-hosting services.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **platforms**
  - **Technical definition:** platforms is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **deployment policies**
  - **Technical definition:** deployment policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Lightsail instances**
  - **Technical definition:** Lightsail instances is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **blueprints**
  - **Technical definition:** blueprints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **bundles**
  - **Technical definition:** bundles is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **static IPs**
  - **Technical definition:** static IPs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **snapshots**
  - **Technical definition:** snapshots is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **container services**
  - **Technical definition:** container services is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.

### AWS Batch

- **AWS Batch**
  - **Technical definition:** AWS Batch is AWS's managed batch-workload scheduler and compute orchestrator for queued jobs with placement, retry, and environment control.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **jobs**
  - **Technical definition:** Jobs are the submitted execution units placed by AWS Batch onto eligible compute resources.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **job queues**
  - **Technical definition:** Job queues are ordered submission buffers that determine priority and which compute environments may run queued jobs.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **job definitions**
  - **Technical definition:** Job definitions are versioned templates that specify container image, resources, retry behavior, and runtime settings for AWS Batch jobs.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **compute environments**
  - **Technical definition:** Compute environments are the pools of EC2 or Fargate capacity AWS Batch uses to place jobs.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **scheduling policies**
  - **Technical definition:** Scheduling policies are the fairness and priority controls that influence how queued Batch jobs receive compute time.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **array jobs**
  - **Technical definition:** array jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **multi-node parallel jobs**
  - **Technical definition:** multi-node parallel jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **fair share identifiers**
  - **Technical definition:** fair share identifiers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.

### AWS CodeBuild, AWS CodePipeline, AWS CodeDeploy, and AWS CodeArtifact

- **AWS CodeBuild**
  - **Technical definition:** AWS CodeBuild is AWS's managed build-execution service for compiling, testing, packaging, and generating artifacts in isolated build containers.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **AWS CodePipeline**
  - **Technical definition:** AWS CodePipeline is AWS's pipeline orchestration service for sequencing source, build, test, approval, and deployment stages.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **AWS CodeDeploy**
  - **Technical definition:** AWS CodeDeploy is AWS's deployment automation service for rolling versions to EC2, Lambda, ECS, and selected on-premises targets.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **and AWS CodeArtifact**
  - **Technical definition:** and AWS CodeArtifact is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **build projects**
  - **Technical definition:** build projects is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **buildspec files**
  - **Technical definition:** buildspec files is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **reports**
  - **Technical definition:** Reports are AWS Artifact documents such as SOC, PCI, or ISO-related attestations made available for customer review.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **caches**
  - **Technical definition:** caches is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **pipelines**
  - **Technical definition:** pipelines is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **stages**
  - **Technical definition:** stages is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **actions**
  - **Technical definition:** actions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **source actions**
  - **Technical definition:** source actions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **AWS CodeConnections connections (CodeStar connections)**
  - **Technical definition:** AWS CodeConnections connections (CodeStar connections) is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **applications**
  - **Technical definition:** applications is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **deployment groups**
  - **Technical definition:** deployment groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **deployment configurations**
  - **Technical definition:** deployment configurations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **AppSpec files**
  - **Technical definition:** AppSpec files is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **domains**
  - **Technical definition:** domains is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **repositories**
  - **Technical definition:** repositories is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **packages**
  - **Technical definition:** packages is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **package versions**
  - **Technical definition:** package versions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.
- **external connections**
  - **Technical definition:** external connections is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during an audit or incident, this is queried to prove who changed a resource, whether the environment drifted, and whether quota or provider events contributed to failure.

### AWS CloudFormation, AWS Cloud Development Kit (AWS CDK), and AWS Proton

- **AWS CloudFormation**
  - **Technical definition:** AWS CloudFormation is AWS's declarative infrastructure-as-code service for provisioning and updating AWS resources as versioned stacks.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **AWS Cloud Development Kit (AWS CDK)**
  - **Technical definition:** AWS CDK is AWS's higher-level infrastructure-as-code framework that synthesizes programming-language constructs into CloudFormation templates.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **and AWS Proton**
  - **Technical definition:** and AWS Proton is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **stacks**
  - **Technical definition:** Stacks are CloudFormation's deployment units representing a collection of AWS resources managed as one lifecycle boundary.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **stack sets**
  - **Technical definition:** stack sets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **templates**
  - **Technical definition:** templates is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **change sets**
  - **Technical definition:** Change sets are CloudFormation previews of the resource modifications a stack update would perform.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **nested stacks**
  - **Technical definition:** nested stacks is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **AWS SAM templates**
  - **Technical definition:** AWS SAM templates is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **AWS::Serverless resources (serverless resources)**
  - **Technical definition:** AWS::Serverless resources (serverless resources) is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **CDK apps**
  - **Technical definition:** CDK apps is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **constructs**
  - **Technical definition:** Constructs are CDK programming abstractions representing reusable infrastructure building blocks that synthesize to CloudFormation.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **assets**
  - **Technical definition:** assets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **synthesized CloudFormation templates**
  - **Technical definition:** synthesized CloudFormation templates is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **Proton environment templates**
  - **Technical definition:** Proton environment templates is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **service templates**
  - **Technical definition:** Service templates are Proton definitions that standardize how one class of application service is provisioned and deployed.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **environments**
  - **Technical definition:** environments is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **services**
  - **Technical definition:** services is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **service instances**
  - **Technical definition:** Service instances are live Proton-managed deployments of a service template into a target environment.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.
- **components**
  - **Technical definition:** components is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: during a production rollout, this helps the team deploy a new version, scale safely, preserve rollback capability, and detect regressions before wider impact.

### Amazon SageMaker AI (Amazon SageMaker)

- **Amazon SageMaker AI (Amazon SageMaker)**
  - **Technical definition:** Amazon SageMaker is AWS's managed ML platform for data preparation, feature processing, training, registry, deployment, and model monitoring workflows.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **domains (Studio domains)**
  - **Technical definition:** domains (Studio domains) is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **user profiles**
  - **Technical definition:** User profiles are SageMaker identities and workspace records used to assign access and isolate ML users.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **spaces**
  - **Technical definition:** spaces is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **training jobs**
  - **Technical definition:** Training jobs are managed compute runs that execute model-training code against supplied data and hyperparameters.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **processing jobs**
  - **Technical definition:** Processing jobs are SageMaker-managed batch compute jobs for data preparation, transformation, and feature engineering.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **feature groups**
  - **Technical definition:** Feature groups are SageMaker feature-store entities that organize online and offline feature data for ML workloads.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **data flows**
  - **Technical definition:** data flows is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **models**
  - **Technical definition:** Models are SageMaker deployable artifacts combining inference code and model data for serving.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **endpoint configurations**
  - **Technical definition:** Endpoint configurations define the production variant layout, instance types, and traffic settings used by a SageMaker endpoint.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **endpoints**
  - **Technical definition:** Endpoints are SageMaker-managed online inference surfaces that host one or more deployed model variants.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **pipelines**
  - **Technical definition:** Pipelines are SageMaker ML workflow graphs that connect processing, training, registration, and condition steps into one reusable ML flow.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **model package groups**
  - **Technical definition:** Model package groups are SageMaker registry collections that hold versioned model packages for one model lineage.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **model packages**
  - **Technical definition:** model packages is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **monitoring schedules**
  - **Technical definition:** monitoring schedules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **baselines**
  - **Technical definition:** baselines is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **constraints**
  - **Technical definition:** constraints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **experiments**
  - **Technical definition:** experiments is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **trials**
  - **Technical definition:** trials is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **trial components**
  - **Technical definition:** trial components is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **lineage artifacts**
  - **Technical definition:** lineage artifacts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **lineage actions**
  - **Technical definition:** lineage actions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **lineage contexts**
  - **Technical definition:** lineage contexts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### Amazon Bedrock

- **Amazon Bedrock**
  - **Technical definition:** Amazon Bedrock is AWS's managed generative-AI service for invoking foundation models and building controlled AI applications without operating model infrastructure.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **foundation models**
  - **Technical definition:** Foundation models are the provider-hosted generative models callable through Bedrock's managed inference APIs.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **inference profiles**
  - **Technical definition:** Inference profiles are Bedrock invocation abstractions used to route calls across model or Region options with controlled behavior.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **provisioned throughput**
  - **Technical definition:** provisioned throughput is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **agents**
  - **Technical definition:** Agents are Bedrock orchestration entities that combine models, instructions, actions, and retrieval to perform multi-step tasks.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **knowledge bases**
  - **Technical definition:** Knowledge bases are Bedrock retrieval constructs that connect managed retrieval pipelines to external documents or vectorized sources.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **guardrails**
  - **Technical definition:** Guardrails are Bedrock policy layers used to constrain prompts and outputs according to safety or business rules.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **model customization jobs**
  - **Technical definition:** model customization jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **model evaluation jobs**
  - **Technical definition:** model evaluation jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **invocation logs**
  - **Technical definition:** invocation logs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

## Controls

### Expanded Points

- **EC2 instance metadata options**
  - **Technical definition:** EC2 instance metadata options is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM instance profiles**
  - **Technical definition:** IAM instance profiles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **security group rules**
  - **Technical definition:** security group rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **termination protection**
  - **Technical definition:** termination protection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Auto Scaling desired/min/max capacity**
  - **Technical definition:** Auto Scaling desired/min/max capacity is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **health check type**
  - **Technical definition:** health check type is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **termination policies**
  - **Technical definition:** termination policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **mixed instances policy**
  - **Technical definition:** mixed instances policy is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **instance refresh preferences**
  - **Technical definition:** instance refresh preferences is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Amazon ECR repository policies**
  - **Technical definition:** Amazon ECR repository policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **image scanning**
  - **Technical definition:** image scanning is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **tag immutability**
  - **Technical definition:** tag immutability is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region replication**
  - **Technical definition:** cross-Region replication is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Amazon ECS task IAM roles**
  - **Technical definition:** Amazon ECS task IAM roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **execution roles**
  - **Technical definition:** execution roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **deployment configuration**
  - **Technical definition:** deployment configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **service discovery**
  - **Technical definition:** service discovery is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **load balancer integration**
  - **Technical definition:** load balancer integration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **capacity provider strategy**
  - **Technical definition:** capacity provider strategy is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Fargate task sizing**
  - **Technical definition:** Fargate task sizing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Amazon EKS endpoint access**
  - **Technical definition:** Amazon EKS endpoint access is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cluster access management**
  - **Technical definition:** cluster access management is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **managed add-ons**
  - **Technical definition:** managed add-ons is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles for service accounts (IRSA)**
  - **Technical definition:** IAM roles for service accounts (IRSA) is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **EKS Pod Identity**
  - **Technical definition:** EKS Pod Identity is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **security groups for pods**
  - **Technical definition:** security groups for pods is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **upgrade policies**
  - **Technical definition:** upgrade policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Karpenter disruption controls**
  - **Technical definition:** Karpenter disruption controls is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Fargate profile placement**
  - **Technical definition:** Fargate profile placement is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Lambda execution roles**
  - **Technical definition:** AWS Lambda execution roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **environment variables**
  - **Technical definition:** environment variables is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **concurrency limits**
  - **Technical definition:** concurrency limits is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **code signing**
  - **Technical definition:** code signing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **App Runner deployment settings**
  - **Technical definition:** App Runner deployment settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Elastic Beanstalk platform versions**
  - **Technical definition:** Elastic Beanstalk platform versions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Lightsail firewall rules**
  - **Technical definition:** Lightsail firewall rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC egress**
  - **Technical definition:** VPC egress is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **access roles**
  - **Technical definition:** access roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **instance profiles**
  - **Technical definition:** instance profiles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **snapshots**
  - **Technical definition:** snapshots is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Batch compute environment type**
  - **Technical definition:** AWS Batch compute environment type is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **allocation strategy**
  - **Technical definition:** allocation strategy is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **job priority**
  - **Technical definition:** job priority is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **retry strategy**
  - **Technical definition:** retry strategy is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **timeout settings**
  - **Technical definition:** timeout settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Developer tool service roles**
  - **Technical definition:** Developer tool service roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **environment images**
  - **Technical definition:** environment images is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **artifact encryption**
  - **Technical definition:** artifact encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **approval actions**
  - **Technical definition:** approval actions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **stage transitions**
  - **Technical definition:** stage transitions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account actions**
  - **Technical definition:** cross-account actions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **automatic rollback**
  - **Technical definition:** automatic rollback is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **alarms**
  - **Technical definition:** alarms is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **repository policies**
  - **Technical definition:** repository policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **package origin controls**
  - **Technical definition:** package origin controls is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **CloudFormation stack policies**
  - **Technical definition:** CloudFormation stack policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rollback triggers**
  - **Technical definition:** rollback triggers is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **termination protection**
  - **Technical definition:** termination protection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **capabilities**
  - **Technical definition:** capabilities is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **hooks**
  - **Technical definition:** hooks is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **service roles**
  - **Technical definition:** service roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS SAM deployment preferences**
  - **Technical definition:** AWS SAM deployment preferences is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **CDK bootstrap stacks**
  - **Technical definition:** CDK bootstrap stacks is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **synthesizers**
  - **Technical definition:** synthesizers is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Proton template versions**
  - **Technical definition:** Proton template versions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **deployment workflows**
  - **Technical definition:** deployment workflows is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **SageMaker execution roles**
  - **Technical definition:** SageMaker execution roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **network isolation**
  - **Technical definition:** network isolation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **model approval status**
  - **Technical definition:** model approval status is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint deployment configuration**
  - **Technical definition:** endpoint deployment configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data capture**
  - **Technical definition:** data capture is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **bias metrics**
  - **Technical definition:** bias metrics is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage metadata**
  - **Technical definition:** lineage metadata is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **artifact encryption**
  - **Technical definition:** artifact encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **access permissions**
  - **Technical definition:** access permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Bedrock model access**
  - **Technical definition:** Bedrock model access is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrails**
  - **Technical definition:** guardrails is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM permissions**
  - **Technical definition:** IAM permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC endpoints**
  - **Technical definition:** VPC endpoints is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **invocation logging**
  - **Technical definition:** invocation logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Expanded Points

- **Build and roll back golden AMIs**
  - **Technical definition:** Build and roll back golden AMIs is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review EC2 status checks**
  - **Technical definition:** review EC2 status checks is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **replace unhealthy instances**
  - **Technical definition:** replace unhealthy instances is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate Auto Scaling instance refreshes**
  - **Technical definition:** validate Auto Scaling instance refreshes is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Push and promote container images in Amazon ECR while reviewing image scan findings and lifecycle cleanup**
  - **Technical definition:** Push and promote container images in Amazon ECR while reviewing image scan findings and lifecycle cleanup is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Deploy ECS services with rolling or blue/green patterns and troubleshoot failed tasks**
  - **Technical definition:** Deploy ECS services with rolling or blue/green patterns and troubleshoot failed tasks is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **capacity providers**
  - **Technical definition:** capacity providers is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **target registration**
  - **Technical definition:** target registration is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Upgrade EKS clusters**
  - **Technical definition:** Upgrade EKS clusters is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **managed node groups**
  - **Technical definition:** managed node groups is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **add-ons**
  - **Technical definition:** add-ons is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review pod scheduling**
  - **Technical definition:** review pod scheduling is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IRSA**
  - **Technical definition:** IRSA is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **EKS Pod Identity**
  - **Technical definition:** EKS Pod Identity is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Karpenter scale-out behavior**
  - **Technical definition:** Karpenter scale-out behavior is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Deploy Lambda versions and aliases**
  - **Technical definition:** Deploy Lambda versions and aliases is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **investigate cold starts**
  - **Technical definition:** investigate cold starts is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **retries**
  - **Technical definition:** retries is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **event source mapping failures**
  - **Technical definition:** event source mapping failures is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Use CodeBuild**
  - **Technical definition:** Use CodeBuild is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **CodePipeline**
  - **Technical definition:** CodePipeline is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **CodeDeploy**
  - **Technical definition:** CodeDeploy is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **CodeArtifact for build**
  - **Technical definition:** CodeArtifact for build is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **approval**
  - **Technical definition:** approval is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **deployment**
  - **Technical definition:** deployment is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rollback**
  - **Technical definition:** rollback is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **artifact promotion workflows**
  - **Technical definition:** artifact promotion workflows is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Review CloudFormation change sets**
  - **Technical definition:** Review CloudFormation change sets is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **drift**
  - **Technical definition:** drift is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **stack rollback**
  - **Technical definition:** stack rollback is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **CDK diffs**
  - **Technical definition:** CDK diffs is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **SAM serverless deployment**
  - **Technical definition:** SAM serverless deployment is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Proton template rollout**
  - **Technical definition:** Proton template rollout is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Run SageMaker training**
  - **Technical definition:** Run SageMaker training is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **processing**
  - **Technical definition:** processing is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **pipelines**
  - **Technical definition:** pipelines is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **model registration**
  - **Technical definition:** model registration is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint deployment**
  - **Technical definition:** endpoint deployment is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **monitoring review**
  - **Technical definition:** monitoring review is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **inference troubleshooting**
  - **Technical definition:** inference troubleshooting is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Invoke Bedrock models**
  - **Technical definition:** Invoke Bedrock models is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **test prompts**
  - **Technical definition:** test prompts is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate guardrails**
  - **Technical definition:** validate guardrails is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **synchronize knowledge bases**
  - **Technical definition:** synchronize knowledge bases is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **deploy agents**
  - **Technical definition:** deploy agents is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review model evaluations**
  - **Technical definition:** review model evaluations is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 4: Data, Storage, Backup, And Stateful Reliability

## Service And Entity Coverage

### Amazon Simple Storage Service (Amazon S3)

- **Amazon Simple Storage Service (Amazon S3)**
  - **Technical definition:** Amazon S3 is AWS's object storage service for durable, massively scalable object persistence, lifecycle control, replication, and event-driven integration.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **buckets**
  - **Technical definition:** Buckets are the top-level S3 namespace containers that own object keys, policies, lifecycle rules, and access settings.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **objects**
  - **Technical definition:** Objects are the actual data items stored in S3, each addressed by a bucket and key and carrying metadata and optional version state.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **prefixes**
  - **Technical definition:** Prefixes are key-name path-like groupings used for organization, lifecycle targeting, and list/query patterns in S3.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **access points**
  - **Technical definition:** Access points are S3-specific named endpoints and policies used to simplify access control for distinct applications or teams.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Multi-Region Access Points**
  - **Technical definition:** Multi-Region Access Points are global S3 entry points that route requests to buckets in multiple Regions for performance or resilience.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **bucket policies**
  - **Technical definition:** Bucket policies are resource-based JSON access policies attached to an S3 bucket.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **S3 Block Public Access settings**
  - **Technical definition:** Within Amazon S3, S3 Block Public Access settings is an object-storage construct that controls namespace, access, retention, replication, lifecycle, or event-driven behavior.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **S3 Object Lock configurations**
  - **Technical definition:** Within Amazon S3, S3 Object Lock configurations is an object-storage construct that controls namespace, access, retention, replication, lifecycle, or event-driven behavior.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **lifecycle configurations**
  - **Technical definition:** Within Amazon S3, lifecycle configurations is an object-storage construct that controls namespace, access, retention, replication, lifecycle, or event-driven behavior.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication rules**
  - **Technical definition:** Replication rules define how and when S3 objects should be copied to another bucket, account, or Region.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **storage classes**
  - **Technical definition:** Storage classes are S3 durability-and-access-cost tiers that trade retrieval speed and price for different object access patterns.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### Amazon Elastic Block Store (Amazon EBS) and Amazon Elastic File System (Amazon EFS)

- **Amazon Elastic Block Store (Amazon EBS)**
  - **Technical definition:** Amazon EBS is AWS's block-storage service for persistent EC2-attached volumes with provisioned performance and snapshot-based recovery.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Amazon Elastic File System (Amazon EFS)**
  - **Technical definition:** Amazon EFS is AWS's elastic NFS file service for shared POSIX access across instances and Availability Zones.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **EBS volumes**
  - **Technical definition:** EBS volumes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **snapshots**
  - **Technical definition:** Snapshots are incremental point-in-time copies of EBS volumes stored in S3-backed snapshot infrastructure.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **snapshot archives**
  - **Technical definition:** snapshot archives is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **volume types**
  - **Technical definition:** volume types is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **provisioned IOPS**
  - **Technical definition:** provisioned IOPS is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **throughput settings**
  - **Technical definition:** throughput settings is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Fast Snapshot Restore**
  - **Technical definition:** Fast Snapshot Restore is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **EFS file systems**
  - **Technical definition:** EFS file systems is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **mount targets**
  - **Technical definition:** Mount targets are the ENI-backed zonal endpoints through which EFS is mounted inside a VPC.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **access points**
  - **Technical definition:** Access points are EFS application-specific entry paths with enforced POSIX identity and directory scoping.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **lifecycle policies**
  - **Technical definition:** lifecycle policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **throughput modes**
  - **Technical definition:** Throughput modes are EFS controls that determine how file-system throughput is provisioned or burst-managed.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **performance modes**
  - **Technical definition:** Performance modes are EFS behavior profiles that trade off latency and scale characteristics based on workload shape.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backups**
  - **Technical definition:** backups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication configurations**
  - **Technical definition:** replication configurations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### Amazon FSx

- **Amazon FSx**
  - **Technical definition:** Amazon FSx is AWS's family of managed specialized file systems for Windows, Lustre, NetApp ONTAP, and OpenZFS workloads.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **file systems**
  - **Technical definition:** File systems are the top-level FSx deployments representing one managed file-system instance of a selected family.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backups**
  - **Technical definition:** Backups are FSx-managed copies used for recovery, cloning, or lifecycle retention.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **storage virtual machines**
  - **Technical definition:** storage virtual machines is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **volumes**
  - **Technical definition:** Volumes are subdivided storage entities in FSx families that support multi-volume semantics such as ONTAP or OpenZFS.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **shares**
  - **Technical definition:** shares is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **endpoints**
  - **Technical definition:** endpoints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **deployment types**
  - **Technical definition:** deployment types is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **throughput capacity**
  - **Technical definition:** Throughput capacity is the file-system data-transfer capability configured for supported FSx families.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### Amazon Relational Database Service (Amazon RDS) and Amazon Aurora

- **Amazon Relational Database Service (Amazon RDS)**
  - **Technical definition:** Amazon RDS is AWS's managed relational database service that abstracts provisioning, backups, maintenance, and failover for supported engines.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Amazon Aurora**
  - **Technical definition:** Amazon Aurora is AWS's cloud-optimized relational engine with distributed storage, rapid failover behavior, and high read-scale features.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **DB instances**
  - **Technical definition:** DB instances are the compute and engine runtime containers that run RDS-managed database engines.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **DB clusters**
  - **Technical definition:** DB clusters are grouped relational database topologies, especially in Aurora, composed of shared storage and one or more DB instances.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **DB snapshots**
  - **Technical definition:** Within Amazon RDS or Aurora, DB snapshots is a managed relational database construct that influences engine behavior, availability, backup, or network placement.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **automated backups**
  - **Technical definition:** Automated backups are RDS-managed backup and log-capture workflows supporting point-in-time recovery.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **parameter groups**
  - **Technical definition:** Parameter groups are engine configuration sets that control runtime database settings across instances or clusters.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **option groups**
  - **Technical definition:** Option groups are engine-specific feature bundles attached to supported RDS instance types.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **subnet groups**
  - **Technical definition:** Subnet groups are the selected subnets into which RDS places database network interfaces.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **read replicas**
  - **Technical definition:** Read replicas are asynchronously or engine-specifically replicated database copies used for read scale or DR options.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **reserved DB instances**
  - **Technical definition:** Within Amazon RDS or Aurora, reserved DB instances is a managed relational database construct that influences engine behavior, availability, backup, or network placement.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Aurora Replicas**
  - **Technical definition:** Within Amazon RDS or Aurora, Aurora Replicas is a managed relational database construct that influences engine behavior, availability, backup, or network placement.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **cluster endpoints**
  - **Technical definition:** Cluster endpoints are Aurora DNS entry points that abstract writer or reader routing over cluster instances.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **reader endpoints**
  - **Technical definition:** Within Amazon RDS or Aurora, reader endpoints is a managed relational database construct that influences engine behavior, availability, backup, or network placement.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **custom endpoints**
  - **Technical definition:** Within Amazon RDS or Aurora, custom endpoints is a managed relational database construct that influences engine behavior, availability, backup, or network placement.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **global databases**
  - **Technical definition:** Global Databases are Aurora's cross-Region replication topology for low-lag regional read scale and DR.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backtrack windows**
  - **Technical definition:** Within Amazon RDS or Aurora, backtrack windows is a managed relational database construct that influences engine behavior, availability, backup, or network placement.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### Amazon DynamoDB

- **Amazon DynamoDB**
  - **Technical definition:** Amazon DynamoDB is AWS's serverless key-value and document database with partition-based horizontal scale, low-latency access, and configurable consistency behavior.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **tables**
  - **Technical definition:** Tables are DynamoDB's top-level data containers defining keys, throughput mode, indexing, and stream behavior.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **items**
  - **Technical definition:** Within DynamoDB, items is a table, indexing, capacity, or replication construct that directly shapes access patterns, scaling behavior, and consistency tradeoffs.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **attributes**
  - **Technical definition:** Within DynamoDB, attributes is a table, indexing, capacity, or replication construct that directly shapes access patterns, scaling behavior, and consistency tradeoffs.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **partition keys**
  - **Technical definition:** Partition keys are the primary key components that determine which logical partition stores an item and therefore affect scale distribution.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **sort keys**
  - **Technical definition:** Sort keys are the ordered secondary primary-key components used to group related items under one partition key.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **local secondary indexes**
  - **Technical definition:** Within DynamoDB, local secondary indexes is a table, indexing, capacity, or replication construct that directly shapes access patterns, scaling behavior, and consistency tradeoffs.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **global secondary indexes**
  - **Technical definition:** Within DynamoDB, global secondary indexes is a table, indexing, capacity, or replication construct that directly shapes access patterns, scaling behavior, and consistency tradeoffs.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **streams**
  - **Technical definition:** Streams are ordered item-change records emitted from a DynamoDB table for downstream processing or replication.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **global tables**
  - **Technical definition:** Global Tables are DynamoDB's multi-Region active-active replication feature for replicated table state across Regions.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backups**
  - **Technical definition:** Within DynamoDB, backups is a table, indexing, capacity, or replication construct that directly shapes access patterns, scaling behavior, and consistency tradeoffs.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **time to live (TTL)**
  - **Technical definition:** Within DynamoDB, time to live (TTL) is a table, indexing, capacity, or replication construct that directly shapes access patterns, scaling behavior, and consistency tradeoffs.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### Amazon DocumentDB, Amazon Neptune, Amazon Timestream for LiveAnalytics, Amazon ElastiCache, and Amazon MemoryDB

- **Amazon DocumentDB**
  - **Technical definition:** Amazon DocumentDB is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Amazon Neptune**
  - **Technical definition:** Amazon Neptune is AWS's managed graph database service for property graph and RDF workloads.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Amazon Timestream for LiveAnalytics**
  - **Technical definition:** Amazon Timestream for LiveAnalytics is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Amazon ElastiCache**
  - **Technical definition:** Amazon ElastiCache is AWS's managed in-memory caching service for Redis- or Memcached-compatible workloads.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **and Amazon MemoryDB**
  - **Technical definition:** and Amazon MemoryDB is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **DocumentDB clusters**
  - **Technical definition:** DocumentDB clusters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Neptune DB clusters**
  - **Technical definition:** Neptune DB clusters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Timestream databases and tables**
  - **Technical definition:** Timestream databases and tables is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **ElastiCache serverless caches**
  - **Technical definition:** ElastiCache serverless caches is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication groups**
  - **Technical definition:** Replication groups are Redis-family topologies grouping primary and replica nodes under one failover domain.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **shards**
  - **Technical definition:** shards is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **users**
  - **Technical definition:** Users are service-level identities used in engines or services supporting internal authentication constructs.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **user groups**
  - **Technical definition:** User groups are grouped authentication/authorization constructs used to bind users and permissions in supported engines.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **MemoryDB clusters**
  - **Technical definition:** MemoryDB clusters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **access control lists (ACLs)**
  - **Technical definition:** access control lists (ACLs) is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **snapshots**
  - **Technical definition:** Snapshots are service-managed point-in-time state captures used for restore or migration.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **endpoints**
  - **Technical definition:** endpoints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### Amazon OpenSearch Service, Amazon Redshift, and Amazon Athena

- **Amazon OpenSearch Service**
  - **Technical definition:** Amazon OpenSearch Service is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon Redshift**
  - **Technical definition:** Amazon Redshift is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **and Amazon Athena**
  - **Technical definition:** and Amazon Athena is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **OpenSearch domains**
  - **Technical definition:** OpenSearch domains is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **indexes**
  - **Technical definition:** indexes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data nodes**
  - **Technical definition:** data nodes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cluster manager nodes**
  - **Technical definition:** cluster manager nodes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **OpenSearch Dashboards**
  - **Technical definition:** OpenSearch Dashboards is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redshift provisioned clusters**
  - **Technical definition:** Redshift provisioned clusters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **serverless namespaces**
  - **Technical definition:** serverless namespaces is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **serverless workgroups**
  - **Technical definition:** serverless workgroups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **databases**
  - **Technical definition:** databases is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **schemas**
  - **Technical definition:** schemas is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **tables**
  - **Technical definition:** tables is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **materialized views**
  - **Technical definition:** materialized views is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Athena workgroups**
  - **Technical definition:** Athena workgroups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **queries**
  - **Technical definition:** queries is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data catalogs**
  - **Technical definition:** data catalogs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **partitions**
  - **Technical definition:** partitions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **query result locations**
  - **Technical definition:** query result locations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### AWS Glue and AWS Lake Formation

- **AWS Glue**
  - **Technical definition:** AWS Glue is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Lake Formation**
  - **Technical definition:** AWS Lake Formation is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Data Catalog databases**
  - **Technical definition:** Data Catalog databases is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Data Catalog tables**
  - **Technical definition:** Data Catalog tables is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **crawlers**
  - **Technical definition:** crawlers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **jobs**
  - **Technical definition:** jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **triggers**
  - **Technical definition:** triggers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **workflows**
  - **Technical definition:** workflows is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **connections**
  - **Technical definition:** connections is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **schemas**
  - **Technical definition:** schemas is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **classifiers**
  - **Technical definition:** classifiers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data quality rulesets**
  - **Technical definition:** data quality rulesets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Glue DataBrew projects**
  - **Technical definition:** AWS Glue DataBrew projects is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **datasets**
  - **Technical definition:** datasets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **recipes**
  - **Technical definition:** recipes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **profile jobs**
  - **Technical definition:** profile jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Lake Formation data lake administrators**
  - **Technical definition:** Lake Formation data lake administrators is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **registered locations**
  - **Technical definition:** registered locations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **LF-Tags**
  - **Technical definition:** LF-Tags is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **permissions**
  - **Technical definition:** permissions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data filters**
  - **Technical definition:** data filters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource links**
  - **Technical definition:** resource links is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **blueprints**
  - **Technical definition:** blueprints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Amazon Kinesis Data Streams, Amazon Data Firehose, Amazon Managed Streaming for Apache Kafka (Amazon MSK), Amazon Simple Queue Service (Amazon SQS), and Amazon Simple Notification Service (Amazon SNS)

- **Amazon Kinesis Data Streams**
  - **Technical definition:** Amazon Kinesis Data Streams is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon Data Firehose**
  - **Technical definition:** Amazon Data Firehose is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon Managed Streaming for Apache Kafka (Amazon MSK)**
  - **Technical definition:** Amazon Managed Streaming for Apache Kafka (Amazon MSK) is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon Simple Queue Service (Amazon SQS)**
  - **Technical definition:** Amazon Simple Queue Service (Amazon SQS) is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **and Amazon Simple Notification Service (Amazon SNS)**
  - **Technical definition:** and Amazon Simple Notification Service (Amazon SNS) is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Kinesis data streams**
  - **Technical definition:** Kinesis data streams is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **shards**
  - **Technical definition:** shards is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **consumers**
  - **Technical definition:** consumers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **enhanced fan-out consumers**
  - **Technical definition:** enhanced fan-out consumers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Firehose streams**
  - **Technical definition:** Firehose streams is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **sources**
  - **Technical definition:** sources is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **destinations**
  - **Technical definition:** destinations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **MSK clusters**
  - **Technical definition:** MSK clusters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **brokers**
  - **Technical definition:** brokers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **topics**
  - **Technical definition:** topics is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **partitions**
  - **Technical definition:** partitions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **consumer groups**
  - **Technical definition:** consumer groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **connectors**
  - **Technical definition:** connectors is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **SQS standard queues**
  - **Technical definition:** SQS standard queues is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **FIFO queues**
  - **Technical definition:** FIFO queues is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **SNS topics**
  - **Technical definition:** SNS topics is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **subscriptions**
  - **Technical definition:** subscriptions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **filter policies**
  - **Technical definition:** filter policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Amazon EventBridge and AWS Step Functions

- **Amazon EventBridge**
  - **Technical definition:** Amazon EventBridge is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Step Functions**
  - **Technical definition:** AWS Step Functions is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **event buses**
  - **Technical definition:** event buses is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rules**
  - **Technical definition:** rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **targets**
  - **Technical definition:** targets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **event patterns**
  - **Technical definition:** event patterns is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **archives**
  - **Technical definition:** archives is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **replays**
  - **Technical definition:** replays is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **schedules**
  - **Technical definition:** schedules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **connections**
  - **Technical definition:** connections is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **API destinations**
  - **Technical definition:** API destinations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **state machines**
  - **Technical definition:** state machines is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **states**
  - **Technical definition:** states is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **executions**
  - **Technical definition:** executions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **activities**
  - **Technical definition:** activities is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **tasks**
  - **Technical definition:** tasks is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **versions**
  - **Technical definition:** versions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **aliases**
  - **Technical definition:** aliases is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **distributed maps**
  - **Technical definition:** distributed maps is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **execution history**
  - **Technical definition:** execution history is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### AWS Secrets Manager and AWS Key Management Service (AWS KMS)

- **AWS Secrets Manager**
  - **Technical definition:** AWS Secrets Manager is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Key Management Service (AWS KMS)**
  - **Technical definition:** AWS Key Management Service (AWS KMS) is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **secrets**
  - **Technical definition:** secrets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **secret versions**
  - **Technical definition:** secret versions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **version stages**
  - **Technical definition:** version stages is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rotation schedules**
  - **Technical definition:** rotation schedules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rotation Lambda functions**
  - **Technical definition:** rotation Lambda functions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource policies**
  - **Technical definition:** resource policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **replication settings**
  - **Technical definition:** replication settings is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **KMS keys**
  - **Technical definition:** KMS keys is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **aliases**
  - **Technical definition:** aliases is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **key policies**
  - **Technical definition:** key policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **grants**
  - **Technical definition:** grants is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **key material**
  - **Technical definition:** key material is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **multi-Region keys**
  - **Technical definition:** multi-Region keys is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **imported key material**
  - **Technical definition:** imported key material is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **external key stores**
  - **Technical definition:** external key stores is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### AWS Backup and AWS Database Migration Service (AWS DMS)

- **AWS Backup**
  - **Technical definition:** AWS Backup is AWS's centralized backup policy and vaulting service for scheduling, copying, retaining, and restoring supported AWS resources.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **AWS Database Migration Service (AWS DMS)**
  - **Technical definition:** AWS DMS is AWS's managed data replication service for migrating and continuously syncing database state between sources and targets.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backup plans**
  - **Technical definition:** backup plans is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backup rules**
  - **Technical definition:** backup rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backup selections**
  - **Technical definition:** backup selections is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **recovery points**
  - **Technical definition:** recovery points is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backup vaults**
  - **Technical definition:** backup vaults is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **backup vault lock configurations**
  - **Technical definition:** backup vault lock configurations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **copy jobs**
  - **Technical definition:** copy jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **restore jobs**
  - **Technical definition:** restore jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication instances**
  - **Technical definition:** Replication instances are the DMS-managed compute resources that run database migration and replication tasks.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication tasks**
  - **Technical definition:** Replication tasks are the DMS jobs that perform full load, CDC, or combined replication behavior between endpoints.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **endpoints**
  - **Technical definition:** endpoints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication subnet groups**
  - **Technical definition:** replication subnet groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **event subscriptions**
  - **Technical definition:** event subscriptions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **data providers**
  - **Technical definition:** data providers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **migration projects**
  - **Technical definition:** migration projects is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

## Controls

### Expanded Points

- **S3 bucket policies**
  - **Technical definition:** S3 bucket policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Object Ownership**
  - **Technical definition:** Object Ownership is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **versioning**
  - **Technical definition:** versioning is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **default encryption**
  - **Technical definition:** default encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **S3 Object Lock retention modes**
  - **Technical definition:** S3 Object Lock retention modes is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **replication**
  - **Technical definition:** replication is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **access logging**
  - **Technical definition:** access logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **EBS volume encryption**
  - **Technical definition:** EBS volume encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **snapshot sharing**
  - **Technical definition:** snapshot sharing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **delete-on-termination settings**
  - **Technical definition:** delete-on-termination settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **KMS key selection**
  - **Technical definition:** KMS key selection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **EFS file system policies**
  - **Technical definition:** EFS file system policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **security groups**
  - **Technical definition:** security groups is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **access point POSIX identities**
  - **Technical definition:** access point POSIX identities is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle management**
  - **Technical definition:** lifecycle management is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup policies**
  - **Technical definition:** backup policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **RDS and Aurora Multi-AZ deployment**
  - **Technical definition:** RDS and Aurora Multi-AZ deployment is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup retention**
  - **Technical definition:** backup retention is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **storage autoscaling**
  - **Technical definition:** storage autoscaling is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM database authentication**
  - **Technical definition:** IAM database authentication is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **failover priority**
  - **Technical definition:** failover priority is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **parameter alignment**
  - **Technical definition:** parameter alignment is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **DynamoDB capacity modes**
  - **Technical definition:** DynamoDB capacity modes is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **auto scaling**
  - **Technical definition:** auto scaling is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **table classes**
  - **Technical definition:** table classes is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **point-in-time recovery (PITR)**
  - **Technical definition:** point-in-time recovery (PITR) is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Specialized database VPC placement**
  - **Technical definition:** Specialized database VPC placement is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **TLS**
  - **Technical definition:** TLS is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup retention**
  - **Technical definition:** backup retention is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **audit logging**
  - **Technical definition:** audit logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **retention configuration**
  - **Technical definition:** retention configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redis-compatible access control lists**
  - **Technical definition:** Redis-compatible access control lists is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Multi-AZ deployment**
  - **Technical definition:** Multi-AZ deployment is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **OpenSearch fine-grained access control**
  - **Technical definition:** OpenSearch fine-grained access control is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC access**
  - **Technical definition:** VPC access is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **index state management**
  - **Technical definition:** index state management is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redshift workload management (WLM)**
  - **Technical definition:** Redshift workload management (WLM) is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data sharing**
  - **Technical definition:** data sharing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Athena workgroup settings**
  - **Technical definition:** Athena workgroup settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **bytes scanned limits**
  - **Technical definition:** bytes scanned limits is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **result location enforcement**
  - **Technical definition:** result location enforcement is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Glue job bookmarks**
  - **Technical definition:** Glue job bookmarks is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **worker type**
  - **Technical definition:** worker type is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **security configurations**
  - **Technical definition:** security configurations is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Data Catalog permissions**
  - **Technical definition:** Data Catalog permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Lake Formation permissions**
  - **Technical definition:** Lake Formation permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **LF-Tag-based access control**
  - **Technical definition:** LF-Tag-based access control is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **row-level filters**
  - **Technical definition:** row-level filters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **column-level filters**
  - **Technical definition:** column-level filters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account sharing**
  - **Technical definition:** cross-account sharing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Streaming and messaging retention**
  - **Technical definition:** Streaming and messaging retention is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **shard count**
  - **Technical definition:** shard count is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Firehose buffering**
  - **Technical definition:** Firehose buffering is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **broker storage**
  - **Technical definition:** broker storage is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Apache Kafka ACLs**
  - **Technical definition:** Apache Kafka ACLs is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **queue policies**
  - **Technical definition:** queue policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **visibility timeout**
  - **Technical definition:** visibility timeout is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **redrive policies**
  - **Technical definition:** redrive policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **topic policies**
  - **Technical definition:** topic policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **subscription filter policies**
  - **Technical definition:** subscription filter policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **delivery retry policies**
  - **Technical definition:** delivery retry policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **EventBridge event bus policies**
  - **Technical definition:** EventBridge event bus policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **input transformers**
  - **Technical definition:** input transformers is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **retry policies**
  - **Technical definition:** retry policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **schema discovery**
  - **Technical definition:** schema discovery is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Step Functions retry/catch handlers**
  - **Technical definition:** Step Functions retry/catch handlers is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **timeout settings**
  - **Technical definition:** timeout settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **logging**
  - **Technical definition:** logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **tracing**
  - **Technical definition:** tracing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Secrets Manager automatic rotation**
  - **Technical definition:** Secrets Manager automatic rotation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **version staging labels**
  - **Technical definition:** version staging labels is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource policies**
  - **Technical definition:** resource policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **KMS key policies**
  - **Technical definition:** KMS key policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **grants**
  - **Technical definition:** grants is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rotation**
  - **Technical definition:** rotation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **deletion waiting periods**
  - **Technical definition:** deletion waiting periods is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **multi-Region replication**
  - **Technical definition:** multi-Region replication is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Backup schedules**
  - **Technical definition:** AWS Backup schedules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **vault policies**
  - **Technical definition:** vault policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Backup Vault Lock**
  - **Technical definition:** AWS Backup Vault Lock is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region copy**
  - **Technical definition:** cross-Region copy is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account copy**
  - **Technical definition:** cross-account copy is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **legal holds**
  - **Technical definition:** legal holds is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **DMS full-load settings**
  - **Technical definition:** DMS full-load settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **change data capture (CDC)**
  - **Technical definition:** change data capture (CDC) is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **table mappings**
  - **Technical definition:** table mappings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validation settings**
  - **Technical definition:** validation settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint permissions**
  - **Technical definition:** endpoint permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Expanded Points

- **Restore S3 objects**
  - **Technical definition:** Restore S3 objects is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate lifecycle policies**
  - **Technical definition:** validate lifecycle policies is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review replication**
  - **Technical definition:** review replication is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **check public access**
  - **Technical definition:** check public access is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **optimize storage classes**
  - **Technical definition:** optimize storage classes is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Attach and resize EBS volumes**
  - **Technical definition:** Attach and resize EBS volumes is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore snapshots**
  - **Technical definition:** restore snapshots is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate EFS mounts**
  - **Technical definition:** validate EFS mounts is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review throughput**
  - **Technical definition:** review throughput is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **test file-system recovery**
  - **Technical definition:** test file-system recovery is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Validate RDS/Aurora failover**
  - **Technical definition:** Validate RDS/Aurora failover is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **point-in-time recovery (PITR)**
  - **Technical definition:** point-in-time recovery (PITR) is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **replica promotion**
  - **Technical definition:** replica promotion is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **global database switchover**
  - **Technical definition:** global database switchover is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **engine upgrades**
  - **Technical definition:** engine upgrades is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **replica lag**
  - **Technical definition:** replica lag is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Troubleshoot DynamoDB hot partitions**
  - **Technical definition:** Troubleshoot DynamoDB hot partitions is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **secondary indexes**
  - **Technical definition:** secondary indexes is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup restore**
  - **Technical definition:** backup restore is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **streams**
  - **Technical definition:** streams is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **global table replication**
  - **Technical definition:** global table replication is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Review cache hit ratios**
  - **Technical definition:** Review cache hit ratios is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redis-compatible access**
  - **Technical definition:** Redis-compatible access is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **snapshot restore**
  - **Technical definition:** snapshot restore is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **eviction behavior**
  - **Technical definition:** eviction behavior is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **specialized database migration fit**
  - **Technical definition:** specialized database migration fit is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Tune OpenSearch shards**
  - **Technical definition:** Tune OpenSearch shards is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redshift queries**
  - **Technical definition:** Redshift queries is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Athena partitions**
  - **Technical definition:** Athena partitions is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data lake query failures**
  - **Technical definition:** data lake query failures is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Run Glue crawlers and ETL jobs**
  - **Technical definition:** Run Glue crawlers and ETL jobs is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate schemas**
  - **Technical definition:** validate schemas is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review data quality**
  - **Technical definition:** review data quality is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **grant Lake Formation permissions**
  - **Technical definition:** grant Lake Formation permissions is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **troubleshoot governed data sharing**
  - **Technical definition:** troubleshoot governed data sharing is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Monitor stream ingestion**
  - **Technical definition:** Monitor stream ingestion is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **shard scaling**
  - **Technical definition:** shard scaling is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **consumer lag**
  - **Technical definition:** consumer lag is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **MSK connectors**
  - **Technical definition:** MSK connectors is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **SQS dead-letter queue redrive**
  - **Technical definition:** SQS dead-letter queue redrive is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **SNS fanout**
  - **Technical definition:** SNS fanout is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Validate EventBridge routing and replay**
  - **Technical definition:** Validate EventBridge routing and replay is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **then troubleshoot failed Step Functions executions and orchestration rollback**
  - **Technical definition:** then troubleshoot failed Step Functions executions and orchestration rollback is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Rotate secrets**
  - **Technical definition:** Rotate secrets is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **roll back secret versions**
  - **Technical definition:** roll back secret versions is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review KMS grants**
  - **Technical definition:** review KMS grants is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate cross-account key access**
  - **Technical definition:** validate cross-account key access is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **test backups**
  - **Technical definition:** test backups is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore recovery points**
  - **Technical definition:** restore recovery points is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **execute DMS full-load plus CDC cutover**
  - **Technical definition:** execute DMS full-load plus CDC cutover is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 5: Observability, FinOps, And Operational Excellence

## Service And Entity Coverage

### Amazon CloudWatch and AWS X-Ray

- **Amazon CloudWatch**
  - **Technical definition:** Amazon CloudWatch is AWS's metrics, logs, alarms, dashboards, events, and operational monitoring service for AWS resources and applications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **AWS X-Ray**
  - **Technical definition:** AWS X-Ray is AWS's distributed tracing and service-map platform for analyzing request paths, latency, and downstream dependency behavior.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **metrics**
  - **Technical definition:** Metrics are CloudWatch time-series measurements representing AWS resource or application behavior over time.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **namespaces**
  - **Technical definition:** Within CloudWatch or X-Ray, namespaces is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **dimensions**
  - **Technical definition:** Dimensions are CloudWatch metric key/value labels used to identify one time series within a metric namespace.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **alarms**
  - **Technical definition:** Alarms are CloudWatch evaluators that trigger state changes based on metric thresholds or anomaly conditions.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **dashboards**
  - **Technical definition:** Dashboards are CloudWatch visual aggregations of metrics, alarms, and text widgets for operational review.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **log groups**
  - **Technical definition:** Within CloudWatch or X-Ray, log groups is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **log streams**
  - **Technical definition:** Log streams are ordered sequences of log events within a CloudWatch Logs group, usually representing one source instance or execution.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **metric filters**
  - **Technical definition:** Metric filters are CloudWatch Logs pattern matchers that emit custom metrics from log events.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Contributor Insights rules**
  - **Technical definition:** Within CloudWatch or X-Ray, Contributor Insights rules is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **synthetic canaries**
  - **Technical definition:** Within CloudWatch or X-Ray, synthetic canaries is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Application Signals services**
  - **Technical definition:** Within CloudWatch or X-Ray, Application Signals services is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Container Insights clusters**
  - **Technical definition:** Within CloudWatch or X-Ray, Container Insights clusters is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Logs Insights queries**
  - **Technical definition:** Within CloudWatch or X-Ray, Logs Insights queries is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **traces**
  - **Technical definition:** Traces are X-Ray request records showing path, timing, and downstream segment behavior for a transaction.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **segments**
  - **Technical definition:** Segments are X-Ray's primary units of trace data representing one service's work during a request.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **subsegments**
  - **Technical definition:** Subsegments are nested X-Ray trace elements representing downstream calls or internal timing portions inside a segment.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **service maps**
  - **Technical definition:** Within CloudWatch or X-Ray, service maps is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **groups**
  - **Technical definition:** Within CloudWatch or X-Ray, groups is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **sampling rules**
  - **Technical definition:** Sampling rules are X-Ray controls that determine which requests should be fully traced to balance cost and visibility.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **annotations**
  - **Technical definition:** Within CloudWatch or X-Ray, annotations is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **metadata**
  - **Technical definition:** Within CloudWatch or X-Ray, metadata is an observability construct that governs telemetry collection, retention, queryability, or operational signaling.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.

### AWS Distro for OpenTelemetry (ADOT), Amazon Managed Service for Prometheus, and Amazon Managed Grafana

- **AWS Distro for OpenTelemetry (ADOT)**
  - **Technical definition:** AWS Distro for OpenTelemetry is AWS's supported OpenTelemetry distribution for collecting and exporting metrics, logs, and traces from workloads.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Amazon Managed Service for Prometheus**
  - **Technical definition:** Amazon Managed Service for Prometheus is AWS's managed Prometheus-compatible metrics store and query backend.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **and Amazon Managed Grafana**
  - **Technical definition:** and Amazon Managed Grafana is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **ADOT collectors**
  - **Technical definition:** ADOT collectors is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **pipelines**
  - **Technical definition:** pipelines is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **receivers**
  - **Technical definition:** Receivers are collector components that ingest telemetry data from configured protocols or sources.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **processors**
  - **Technical definition:** Processors are collector pipeline stages that transform, batch, filter, or enrich telemetry before export.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **exporters**
  - **Technical definition:** Exporters are collector components that send processed telemetry to backends such as CloudWatch, AMP, or third-party systems.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Prometheus workspaces**
  - **Technical definition:** Prometheus workspaces is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **rule groups namespaces**
  - **Technical definition:** rule groups namespaces is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **scrapers**
  - **Technical definition:** scrapers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **metrics**
  - **Technical definition:** metrics is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **labels**
  - **Technical definition:** labels is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Grafana workspaces**
  - **Technical definition:** Grafana workspaces is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **data sources**
  - **Technical definition:** Data sources are Grafana backend connections used to query metrics, logs, traces, or other observability systems.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **dashboards**
  - **Technical definition:** dashboards is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **folders**
  - **Technical definition:** folders is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **users**
  - **Technical definition:** users is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **teams**
  - **Technical definition:** teams is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **service accounts**
  - **Technical definition:** service accounts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.

### AWS Systems Manager

- **AWS Systems Manager**
  - **Technical definition:** AWS Systems Manager is AWS's fleet-operations platform for remote access, patching, automation, parameters, inventory, and managed-node control.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **managed nodes**
  - **Technical definition:** Managed nodes are instances or servers enrolled into Systems Manager and therefore controllable through SSM features.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **documents**
  - **Technical definition:** documents is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Run Command**
  - **Technical definition:** Run Command is Systems Manager's remote command-execution feature for sending scripts or commands to managed nodes at scale.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Session Manager sessions**
  - **Technical definition:** Session Manager sessions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **State Manager associations**
  - **Technical definition:** State Manager associations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Patch Manager baselines**
  - **Technical definition:** Patch Manager baselines is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Automation runbooks**
  - **Technical definition:** Automation runbooks is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Parameter Store parameters**
  - **Technical definition:** Parameter Store parameters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **OpsItems**
  - **Technical definition:** OpsItems is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.

### AWS Well-Architected Tool and AWS Resilience Hub

- **AWS Well-Architected Tool**
  - **Technical definition:** AWS Well-Architected Tool is AWS's structured workload-review service for evaluating architecture choices against AWS best-practice pillars.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **AWS Resilience Hub**
  - **Technical definition:** AWS Resilience Hub is AWS's resilience-analysis service that models applications, evaluates recovery objectives, and recommends validation work.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **workloads**
  - **Technical definition:** Workloads are the modeled application systems reviewed in AWS Well-Architected or Resilience Hub assessments.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **lenses**
  - **Technical definition:** Lenses are Well-Architected question sets tailored to specific technology or domain patterns.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **milestones**
  - **Technical definition:** Milestones are point-in-time snapshots of a Well-Architected workload review used to track architectural improvement over time.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **improvement plans**
  - **Technical definition:** improvement plans is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **review templates**
  - **Technical definition:** review templates is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **profiles**
  - **Technical definition:** profiles is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **workload shares**
  - **Technical definition:** workload shares is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **applications**
  - **Technical definition:** Applications are the resilience-modeled logical systems in Resilience Hub that combine resources and recovery expectations.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **app components**
  - **Technical definition:** app components is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **resilience policies**
  - **Technical definition:** resilience policies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **assessments**
  - **Technical definition:** assessments is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **resiliency scores**
  - **Technical definition:** resiliency scores is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **recommendations**
  - **Technical definition:** Recommendations are improvement actions generated from architecture or resilience assessments.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **standard operating procedures (SOPs)**
  - **Technical definition:** standard operating procedures (SOPs) is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.

### AWS Cost Explorer, AWS Budgets, AWS Cost Categories, AWS Cost and Usage Reports (AWS CUR), AWS Cost Anomaly Detection, AWS Compute Optimizer, and AWS Trusted Advisor

- **AWS Cost Explorer**
  - **Technical definition:** AWS Cost Explorer is AWS's cost and usage analysis tool for exploring spend patterns, dimensions, and forecast trends.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **AWS Budgets**
  - **Technical definition:** AWS Budgets is AWS's threshold-based cost and usage alerting service tied to defined scopes and owners.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **AWS Cost Categories**
  - **Technical definition:** AWS Cost Categories is AWS's cost-classification feature for mapping raw spend into business-defined reporting dimensions.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **AWS Cost and Usage Reports (AWS CUR)**
  - **Technical definition:** AWS CUR is AWS's detailed billing and usage export for granular FinOps analysis, showback, and chargeback workflows.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **AWS Cost Anomaly Detection**
  - **Technical definition:** AWS Cost Anomaly Detection is AWS's managed spend-anomaly service for flagging unusual cost patterns and routing alerts.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **AWS Compute Optimizer**
  - **Technical definition:** AWS Compute Optimizer is AWS's rightsizing recommendation service based on observed resource utilization patterns.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **and AWS Trusted Advisor**
  - **Technical definition:** and AWS Trusted Advisor is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **cost and usage data**
  - **Technical definition:** cost and usage data is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **dimensions**
  - **Technical definition:** dimensions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **filters**
  - **Technical definition:** filters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **forecasts**
  - **Technical definition:** forecasts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **budgets**
  - **Technical definition:** Budgets are scoped thresholds for cost, usage, reservation, or savings-plan utilization with alerting.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **budget actions**
  - **Technical definition:** budget actions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **alerts**
  - **Technical definition:** alerts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **cost category rules**
  - **Technical definition:** cost category rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **CUR report definitions**
  - **Technical definition:** CUR report definitions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **anomaly monitors**
  - **Technical definition:** Anomaly monitors are the scoped detectors used by AWS Cost Anomaly Detection to watch specific slices of spend.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **anomalies**
  - **Technical definition:** anomalies is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **recommendations**
  - **Technical definition:** Recommendations are rightsizing or optimization suggestions emitted by AWS cost and optimization services.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **checks**
  - **Technical definition:** Checks are Trusted Advisor or related service evaluations against cost, quota, security, or resilience best practices.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.

### Amazon GuardDuty, Amazon Inspector, Amazon Macie, Amazon Detective, and Amazon Security Lake

- **Amazon GuardDuty**
  - **Technical definition:** Amazon GuardDuty is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Amazon Inspector**
  - **Technical definition:** Amazon Inspector is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Amazon Macie**
  - **Technical definition:** Amazon Macie is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Amazon Detective**
  - **Technical definition:** Amazon Detective is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **and Amazon Security Lake**
  - **Technical definition:** and Amazon Security Lake is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **detectors**
  - **Technical definition:** detectors is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **findings**
  - **Technical definition:** findings is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **threat intelligence sets**
  - **Technical definition:** threat intelligence sets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **malware protection plans**
  - **Technical definition:** malware protection plans is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **coverage**
  - **Technical definition:** coverage is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **software bills of materials (SBOMs)**
  - **Technical definition:** software bills of materials (SBOMs) is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **classification jobs**
  - **Technical definition:** classification jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **sensitive data discovery results**
  - **Technical definition:** sensitive data discovery results is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **managed data identifiers**
  - **Technical definition:** managed data identifiers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **behavior graphs**
  - **Technical definition:** behavior graphs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **investigations**
  - **Technical definition:** investigations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **data lakes**
  - **Technical definition:** data lakes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **sources**
  - **Technical definition:** sources is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **subscribers**
  - **Technical definition:** subscribers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **Open Cybersecurity Schema Framework (OCSF) event classes**
  - **Technical definition:** Open Cybersecurity Schema Framework (OCSF) event classes is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.

### Amazon SageMaker AI and Amazon Bedrock

- **Amazon SageMaker AI**
  - **Technical definition:** Amazon SageMaker AI is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Amazon Bedrock**
  - **Technical definition:** Amazon Bedrock is AWS's managed generative-AI service for invoking foundation models and building controlled AI applications without operating model infrastructure.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **SageMaker monitoring schedules**
  - **Technical definition:** SageMaker monitoring schedules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **baselines**
  - **Technical definition:** baselines is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **constraints**
  - **Technical definition:** constraints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **processing jobs**
  - **Technical definition:** Processing jobs are SageMaker-managed batch compute jobs for data preparation, transformation, and feature engineering.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **bias reports**
  - **Technical definition:** bias reports is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **explainability reports**
  - **Technical definition:** explainability reports is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **experiments**
  - **Technical definition:** experiments is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **trials**
  - **Technical definition:** trials is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **lineage artifacts**
  - **Technical definition:** lineage artifacts is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **lineage actions**
  - **Technical definition:** lineage actions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Bedrock model evaluation jobs**
  - **Technical definition:** Bedrock model evaluation jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **evaluation datasets**
  - **Technical definition:** evaluation datasets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **guardrails**
  - **Technical definition:** guardrails is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **guardrail versions**
  - **Technical definition:** guardrail versions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **foundation models**
  - **Technical definition:** foundation models is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **agents**
  - **Technical definition:** agents is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **invocation logs**
  - **Technical definition:** invocation logs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

## Controls

### Expanded Points

- **CloudWatch alarm actions**
  - **Technical definition:** CloudWatch alarm actions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **metric math**
  - **Technical definition:** metric math is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **composite alarms**
  - **Technical definition:** composite alarms is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **log retention**
  - **Technical definition:** log retention is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data protection policies**
  - **Technical definition:** data protection policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account observability**
  - **Technical definition:** cross-account observability is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **dashboards**
  - **Technical definition:** dashboards is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **X-Ray sampling rules**
  - **Technical definition:** X-Ray sampling rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **trace retention**
  - **Technical definition:** trace retention is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **SDK configuration**
  - **Technical definition:** SDK configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **ADOT telemetry routing**
  - **Technical definition:** ADOT telemetry routing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **sampling**
  - **Technical definition:** sampling is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **exporter configuration**
  - **Technical definition:** exporter configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Prometheus workspace policies**
  - **Technical definition:** Prometheus workspace policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **ingestion limits**
  - **Technical definition:** ingestion limits is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **alerting rules**
  - **Technical definition:** alerting rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Grafana authentication providers**
  - **Technical definition:** Grafana authentication providers is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **data source permissions**
  - **Technical definition:** data source permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **workspace roles**
  - **Technical definition:** workspace roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **plugin management**
  - **Technical definition:** plugin management is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Systems Manager instance profiles**
  - **Technical definition:** Systems Manager instance profiles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **session logging**
  - **Technical definition:** session logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **patch baselines**
  - **Technical definition:** patch baselines is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **document permissions**
  - **Technical definition:** document permissions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **parameter policies**
  - **Technical definition:** parameter policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Well-Architected lens selection**
  - **Technical definition:** Well-Architected lens selection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **workload sharing**
  - **Technical definition:** workload sharing is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Resilience Hub recovery time objective/recovery point objective (RTO/RPO) targets**
  - **Technical definition:** Resilience Hub recovery time objective/recovery point objective (RTO/RPO) targets is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **policy mapping**
  - **Technical definition:** policy mapping is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **application import sources**
  - **Technical definition:** application import sources is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **assessment scope**
  - **Technical definition:** assessment scope is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Cost filters**
  - **Technical definition:** Cost filters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **grouping dimensions**
  - **Technical definition:** grouping dimensions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **budget thresholds**
  - **Technical definition:** budget thresholds is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **budget actions**
  - **Technical definition:** budget actions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **split charge allocation**
  - **Technical definition:** split charge allocation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **CUR delivery options**
  - **Technical definition:** CUR delivery options is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **anomaly alert thresholds**
  - **Technical definition:** anomaly alert thresholds is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **recommendation preferences**
  - **Technical definition:** recommendation preferences is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **organizational views**
  - **Technical definition:** organizational views is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Security delegated administrator accounts**
  - **Technical definition:** Security delegated administrator accounts is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **GuardDuty detector features**
  - **Technical definition:** GuardDuty detector features is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **S3 protection**
  - **Technical definition:** S3 protection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **EKS protection**
  - **Technical definition:** EKS protection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon ECR scanning**
  - **Technical definition:** Amazon ECR scanning is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Lambda scanning**
  - **Technical definition:** AWS Lambda scanning is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **finding filters**
  - **Technical definition:** finding filters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **automated sensitive data discovery**
  - **Technical definition:** automated sensitive data discovery is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **graph retention**
  - **Technical definition:** graph retention is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **source collection**
  - **Technical definition:** source collection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **subscriber access**
  - **Technical definition:** subscriber access is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region rollup**
  - **Technical definition:** cross-Region rollup is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **SageMaker data capture**
  - **Technical definition:** SageMaker data capture is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **baseline constraints**
  - **Technical definition:** baseline constraints is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **bias metrics**
  - **Technical definition:** bias metrics is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **explainability configuration**
  - **Technical definition:** explainability configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **experiment metadata**
  - **Technical definition:** experiment metadata is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage metadata**
  - **Technical definition:** lineage metadata is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Bedrock model access**
  - **Technical definition:** Bedrock model access is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrail policies**
  - **Technical definition:** guardrail policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **content filters**
  - **Technical definition:** content filters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **sensitive information filters**
  - **Technical definition:** sensitive information filters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC endpoints**
  - **Technical definition:** VPC endpoints is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **logging**
  - **Technical definition:** logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Expanded Points

- **Review dashboards**
  - **Technical definition:** Review dashboards is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **alarms**
  - **Technical definition:** alarms is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Logs Insights queries**
  - **Technical definition:** Logs Insights queries is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **synthetic checks**
  - **Technical definition:** synthetic checks is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **distributed traces**
  - **Technical definition:** distributed traces is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **dependency maps**
  - **Technical definition:** dependency maps is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **latency root causes**
  - **Technical definition:** latency root causes is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **incident timelines**
  - **Technical definition:** incident timelines is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Roll out ADOT collectors**
  - **Technical definition:** Roll out ADOT collectors is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate Prometheus queries**
  - **Technical definition:** validate Prometheus queries is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **tune metric cardinality**
  - **Technical definition:** tune metric cardinality is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **publish Grafana dashboards**
  - **Technical definition:** publish Grafana dashboards is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **troubleshoot data sources**
  - **Technical definition:** troubleshoot data sources is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Execute fleet commands**
  - **Technical definition:** Execute fleet commands is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **open Session Manager sessions**
  - **Technical definition:** open Session Manager sessions is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review patch compliance**
  - **Technical definition:** review patch compliance is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **run automations**
  - **Technical definition:** run automations is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **track OpsItems**
  - **Technical definition:** track OpsItems is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Perform Well-Architected reviews**
  - **Technical definition:** Perform Well-Architected reviews is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **track risks**
  - **Technical definition:** track risks is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **run Resilience Hub assessments**
  - **Technical definition:** run Resilience Hub assessments is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review resiliency recommendations**
  - **Technical definition:** review resiliency recommendations is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Analyze spend**
  - **Technical definition:** Analyze spend is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review forecasts**
  - **Technical definition:** review forecasts is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **tune budgets**
  - **Technical definition:** tune budgets is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate CUR delivery**
  - **Technical definition:** validate CUR delivery is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **triage cost anomalies**
  - **Technical definition:** triage cost anomalies is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **track rightsizing recommendations**
  - **Technical definition:** track rightsizing recommendations is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Triage GuardDuty findings**
  - **Technical definition:** Triage GuardDuty findings is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Inspector vulnerabilities**
  - **Technical definition:** Inspector vulnerabilities is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Macie sensitive data findings**
  - **Technical definition:** Macie sensitive data findings is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Detective investigations**
  - **Technical definition:** Detective investigations is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Security Lake subscriber delivery**
  - **Technical definition:** Security Lake subscriber delivery is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Review model drift**
  - **Technical definition:** Review model drift is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **bias reports**
  - **Technical definition:** bias reports is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **explainability reports**
  - **Technical definition:** explainability reports is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **model lineage**
  - **Technical definition:** model lineage is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Bedrock evaluations**
  - **Technical definition:** Bedrock evaluations is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrail tests**
  - **Technical definition:** guardrail tests is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **generative AI governance evidence**
  - **Technical definition:** generative AI governance evidence is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 6: DR, Multi-Region, And Migration Execution

## Service And Entity Coverage

### Amazon Application Recovery Controller (ARC) (Route 53 ARC)

- **Amazon Application Recovery Controller (ARC) (Route 53 ARC)**
  - **Technical definition:** Amazon Application Recovery Controller is AWS's readiness and routing-control safety service for zonal and regional failover operations.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **clusters**
  - **Technical definition:** clusters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **routing controls**
  - **Technical definition:** routing controls is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **control panels**
  - **Technical definition:** control panels is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **safety rules**
  - **Technical definition:** safety rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **readiness checks**
  - **Technical definition:** readiness checks is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **recovery groups**
  - **Technical definition:** recovery groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **cells**
  - **Technical definition:** cells is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.
- **resource sets**
  - **Technical definition:** resource sets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS resolution, TLS trust, health checks, and private connectivity all behave as intended.

### AWS Elastic Disaster Recovery (AWS DRS) and AWS Application Migration Service (AWS MGN)

- **AWS Elastic Disaster Recovery (AWS DRS)**
  - **Technical definition:** AWS Elastic Disaster Recovery is AWS's block-level replication and recovery orchestration service for server-based DR into AWS.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **AWS Application Migration Service (AWS MGN)**
  - **Technical definition:** AWS Application Migration Service is AWS's server replication and launch service for machine-based migration into AWS.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **DRS source servers**
  - **Technical definition:** DRS source servers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication servers**
  - **Technical definition:** replication servers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **staging area subnets**
  - **Technical definition:** Staging area subnets are the AWS network locations where replication infrastructure lands before launch or recovery.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **recovery instances**
  - **Technical definition:** recovery instances is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **recovery snapshots**
  - **Technical definition:** recovery snapshots is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **conversion servers**
  - **Technical definition:** conversion servers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **MGN source servers**
  - **Technical definition:** MGN source servers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication agents**
  - **Technical definition:** replication agents is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication settings templates**
  - **Technical definition:** replication settings templates is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **launch settings**
  - **Technical definition:** Launch settings define how recovered or migrated servers are instantiated in AWS, including instance type, subnet, and post-launch actions.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **test instances**
  - **Technical definition:** test instances is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **cutover instances**
  - **Technical definition:** cutover instances is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **waves**
  - **Technical definition:** waves is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### AWS Migration Hub

- **AWS Migration Hub**
  - **Technical definition:** AWS Migration Hub is AWS's portfolio-level migration tracking service for organizing applications, waves, and tool-generated status.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **applications**
  - **Technical definition:** Applications are logical migration groupings in Migration Hub that represent one business or technical system.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **servers**
  - **Technical definition:** servers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **migration waves**
  - **Technical definition:** migration waves is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **discovered resources**
  - **Technical definition:** discovered resources is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **migration tools**
  - **Technical definition:** migration tools is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **home Region**
  - **Technical definition:** home Region is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.
- **migration statuses**
  - **Technical definition:** migration statuses is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: SREs or migration engineers use this to generate evidence, validate recovery objectives, trace faults, optimize cost, or track readiness across a live AWS estate.

### AWS Database Migration Service (AWS DMS) and AWS Schema Conversion Tool (AWS SCT)

- **AWS Database Migration Service (AWS DMS)**
  - **Technical definition:** AWS DMS is AWS's managed data replication service for migrating and continuously syncing database state between sources and targets.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **AWS Schema Conversion Tool (AWS SCT)**
  - **Technical definition:** AWS SCT is AWS's schema and code conversion utility for assessing and transforming source database structures toward target engines.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication instances**
  - **Technical definition:** Replication instances are the DMS-managed compute resources that run database migration and replication tasks.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication tasks**
  - **Technical definition:** Replication tasks are the DMS jobs that perform full load, CDC, or combined replication behavior between endpoints.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **source endpoints**
  - **Technical definition:** Source endpoints are DMS connection definitions for reading from the originating database or data source.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **target endpoints**
  - **Technical definition:** Target endpoints are DMS connection definitions for writing into the destination database or target service.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **replication subnet groups**
  - **Technical definition:** replication subnet groups is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **table mappings**
  - **Technical definition:** Table mappings are DMS rules that select, transform, or rename which source objects are migrated.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **validation reports**
  - **Technical definition:** validation reports is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **SCT migration projects**
  - **Technical definition:** SCT migration projects is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **source schemas**
  - **Technical definition:** source schemas is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **target schemas**
  - **Technical definition:** target schemas is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **assessment reports**
  - **Technical definition:** Assessment reports are SCT outputs that explain schema-conversion complexity, unsupported features, and migration effort.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **conversion rules**
  - **Technical definition:** conversion rules is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **converted schema objects**
  - **Technical definition:** converted schema objects is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### AWS DataSync and AWS Transfer Family

- **AWS DataSync**
  - **Technical definition:** AWS DataSync is AWS's managed high-throughput data-transfer service for moving file and object data between on-premises, AWS, and supported storage systems.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **AWS Transfer Family**
  - **Technical definition:** AWS Transfer Family is AWS's managed file-transfer endpoint service for SFTP, FTPS, FTP, and AS2 protocols backed by AWS storage.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **DataSync agents**
  - **Technical definition:** DataSync agents is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **locations**
  - **Technical definition:** Locations are DataSync source or destination endpoint definitions for file systems, object stores, or AWS services.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **tasks**
  - **Technical definition:** Tasks are DataSync transfer jobs defining source, destination, options, and scheduling behavior.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **task executions**
  - **Technical definition:** Task executions are the actual runs of a DataSync task with status, logs, and throughput metrics.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **manifests**
  - **Technical definition:** manifests is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **filters**
  - **Technical definition:** filters is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **reports**
  - **Technical definition:** reports is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **transfer options**
  - **Technical definition:** transfer options is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **Transfer Family servers**
  - **Technical definition:** Transfer Family servers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **users**
  - **Technical definition:** Users are Transfer Family identities mapped to auth methods and backend storage access paths.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **workflows**
  - **Technical definition:** Workflows are Transfer Family post-upload or processing pipelines applied to transferred files.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **connectors**
  - **Technical definition:** connectors is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **identity providers**
  - **Technical definition:** identity providers is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **protocols**
  - **Technical definition:** protocols is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **host keys**
  - **Technical definition:** host keys is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.
- **access endpoints**
  - **Technical definition:** access endpoints is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: production readiness depends on validating this through backup restore, replication checks, throughput testing, or cutover rehearsal before the workload is declared safe.

### AWS Backup

- **AWS Backup**
  - **Technical definition:** AWS Backup is AWS's centralized backup policy and vaulting service for scheduling, copying, retaining, and restoring supported AWS resources.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup plans**
  - **Technical definition:** backup plans is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup vaults**
  - **Technical definition:** backup vaults is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **recovery points**
  - **Technical definition:** recovery points is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup vault lock configurations**
  - **Technical definition:** backup vault lock configurations is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **copy jobs**
  - **Technical definition:** copy jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore jobs**
  - **Technical definition:** restore jobs is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **legal holds**
  - **Technical definition:** legal holds is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore testing plans**
  - **Technical definition:** restore testing plans is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### AWS Fault Injection Service (AWS FIS)

- **AWS Fault Injection Service (AWS FIS)**
  - **Technical definition:** AWS Fault Injection Service (AWS FIS) is an AWS managed service whose design should be understood in terms of control-plane behavior, data-plane scope, and operational failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **experiment templates**
  - **Technical definition:** experiment templates is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **experiments**
  - **Technical definition:** experiments is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **actions**
  - **Technical definition:** actions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **targets**
  - **Technical definition:** targets is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **stop conditions**
  - **Technical definition:** stop conditions is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **experiment roles**
  - **Technical definition:** experiment roles is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **experiment reports**
  - **Technical definition:** experiment reports is an AWS service entity or operational construct whose technical value depends on scope, lifecycle, policy boundary, and failure implications.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

## DR Dependencies From Earlier Weeks

### Expanded Points

- **Amazon Route 53 DNS failover and health checks are covered in Week 2**
  - **Technical definition:** Amazon Route 53 DNS failover and health checks are covered in Week 2 is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Amazon S3 replication**
  - **Technical definition:** Amazon S3 replication is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon EFS replication**
  - **Technical definition:** Amazon EFS replication is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon Aurora Global Database**
  - **Technical definition:** Amazon Aurora Global Database is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon RDS Read Replicas**
  - **Technical definition:** Amazon RDS Read Replicas is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon DynamoDB global tables are covered in Week 4**
  - **Technical definition:** Amazon DynamoDB global tables are covered in Week 4 is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **IAM emergency access**
  - **Technical definition:** IAM emergency access is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **CloudTrail evidence**
  - **Technical definition:** CloudTrail evidence is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Config drift data**
  - **Technical definition:** AWS Config drift data is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS KMS keys**
  - **Technical definition:** AWS KMS keys is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Backup vault controls are covered in earlier weeks**
  - **Technical definition:** AWS Backup vault controls are covered in earlier weeks is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

## Controls

### Expanded Points

- **ARC routing control state**
  - **Technical definition:** ARC routing control state is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **safety rules**
  - **Technical definition:** safety rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **readiness check scope**
  - **Technical definition:** readiness check scope is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cell design**
  - **Technical definition:** cell design is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **recovery group modeling**
  - **Technical definition:** recovery group modeling is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **DRS/MGN replication settings**
  - **Technical definition:** DRS/MGN replication settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **launch settings**
  - **Technical definition:** launch settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **staging area configuration**
  - **Technical definition:** staging area configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **point-in-time recovery selection**
  - **Technical definition:** point-in-time recovery selection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **failback settings**
  - **Technical definition:** failback settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **post-launch actions**
  - **Technical definition:** post-launch actions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cutover settings**
  - **Technical definition:** cutover settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Migration Hub home Region selection**
  - **Technical definition:** Migration Hub home Region selection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **application grouping**
  - **Technical definition:** application grouping is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **migration status tracking**
  - **Technical definition:** migration status tracking is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **migration tool integration**
  - **Technical definition:** migration tool integration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **DMS full-load settings**
  - **Technical definition:** DMS full-load settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **change data capture (CDC)**
  - **Technical definition:** change data capture (CDC) is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **task settings**
  - **Technical definition:** task settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validation**
  - **Technical definition:** validation is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **transformation rules**
  - **Technical definition:** transformation rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint encryption**
  - **Technical definition:** endpoint encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **SCT compatibility assessment**
  - **Technical definition:** SCT compatibility assessment is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **schema mapping**
  - **Technical definition:** schema mapping is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **conversion settings**
  - **Technical definition:** conversion settings is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **DataSync task options**
  - **Technical definition:** DataSync task options is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **bandwidth limits**
  - **Technical definition:** bandwidth limits is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **verification mode**
  - **Technical definition:** verification mode is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **include/exclude filters**
  - **Technical definition:** include/exclude filters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **schedule configuration**
  - **Technical definition:** schedule configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **Transfer Family protocol selection**
  - **Technical definition:** Transfer Family protocol selection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **identity provider configuration**
  - **Technical definition:** identity provider configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **workflow triggers**
  - **Technical definition:** workflow triggers is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **logging**
  - **Technical definition:** logging is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **AWS Backup schedules**
  - **Technical definition:** AWS Backup schedules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Backup Vault Lock**
  - **Technical definition:** AWS Backup Vault Lock is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region copy**
  - **Technical definition:** cross-Region copy is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account copy**
  - **Technical definition:** cross-account copy is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **vault access policies**
  - **Technical definition:** vault access policies is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore testing configuration**
  - **Technical definition:** restore testing configuration is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **FIS stop conditions**
  - **Technical definition:** FIS stop conditions is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM experiment roles**
  - **Technical definition:** IAM experiment roles is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **target selection**
  - **Technical definition:** target selection is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **action parameters**
  - **Technical definition:** action parameters is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **safety guardrails**
  - **Technical definition:** safety guardrails is an AWS control that should be evaluated for preventive strength, detective value, and operational blast-radius impact.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Expanded Points

- **Execute ARC routing control tests**
  - **Technical definition:** Execute ARC routing control tests is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **readiness checks**
  - **Technical definition:** readiness checks is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **failover rehearsal**
  - **Technical definition:** failover rehearsal is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **game day evidence collection**
  - **Technical definition:** game day evidence collection is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Replicate servers with AWS DRS**
  - **Technical definition:** Replicate servers with AWS DRS is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **run non-disruptive recovery drills**
  - **Technical definition:** run non-disruptive recovery drills is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **launch recovery instances**
  - **Technical definition:** launch recovery instances is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate failback**
  - **Technical definition:** validate failback is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **review replication lag**
  - **Technical definition:** review replication lag is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Use AWS MGN for migration waves**
  - **Technical definition:** Use AWS MGN for migration waves is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **test launches**
  - **Technical definition:** test launches is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cutover launches**
  - **Technical definition:** cutover launches is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rollback planning**
  - **Technical definition:** rollback planning is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **post-launch validation**
  - **Technical definition:** post-launch validation is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Track portfolio migration status in AWS Migration Hub and report wave readiness**
  - **Technical definition:** Track portfolio migration status in AWS Migration Hub and report wave readiness is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Run DMS full-load plus CDC migration**
  - **Technical definition:** Run DMS full-load plus CDC migration is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate data**
  - **Technical definition:** validate data is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **troubleshoot replication lag**
  - **Technical definition:** troubleshoot replication lag is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **convert schemas with AWS SCT**
  - **Technical definition:** convert schemas with AWS SCT is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **rehearse cutover/rollback**
  - **Technical definition:** rehearse cutover/rollback is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Use DataSync for file/object seeding**
  - **Technical definition:** Use DataSync for file/object seeding is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **incremental sync**
  - **Technical definition:** incremental sync is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **transfer validation**
  - **Technical definition:** transfer validation is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **cutover sync**
  - **Technical definition:** cutover sync is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Validate AWS Backup recovery points**
  - **Technical definition:** Validate AWS Backup recovery points is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore testing**
  - **Technical definition:** restore testing is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **immutable backup controls**
  - **Technical definition:** immutable backup controls is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
- **ransomware recovery readiness**
  - **Technical definition:** ransomware recovery readiness is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.

### Expanded Points

- **Run AWS FIS experiments with stop conditions to validate resilience assumptions and blast radius**
  - **Technical definition:** Run AWS FIS experiments with stop conditions to validate resilience assumptions and blast radius is an AWS operational workflow used to reduce risk and validate production behavior.
  - **Concrete production example:** Example: in production, this point matters when teams need secure change control, clear ownership, and testable recovery behavior.
