# Deep Technical Analysis of AWS Week-Wise Cloud Coverage — v2
## AWS-Focused Technical Study Notes with Comma-Separated and `and`-Separated Service/Entity Expansion

> Rule applied: every comma-separated value is treated as a point, and when **`and` joins two AWS services or two AWS entities**, they are treated as separate points with independent definitions.
> Style: technical definition first, one concrete production example wherever useful, and stronger AWS-specific depth than the prior version.

# AWS Week 1: Landing Zone, Account Strategy, And Governance

## Service And Entity Coverage

### AWS Organizations and AWS Account Management

- **AWS Organizations**
  - **Technical definition:** AWS Organizations is AWS's multi-account governance service for structuring accounts into a hierarchy, applying inherited policy, delegating administration, and consolidating billing.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **AWS Account Management**
  - **Technical definition:** AWS Account Management is the administrative layer for account identity, contacts, regional enablement, support metadata, and other account-scoped operational settings.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **organization**
  - **Technical definition:** An organization is the top-level AWS Organizations object that owns the hierarchy, policy inheritance, and consolidated governance scope.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **root**
  - **Technical definition:** The root is the top hierarchy node of an AWS Organization under which all organizational units and accounts ultimately inherit organization-level policy.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **management account**
  - **Technical definition:** The management account is the organization-administering AWS account with authority over billing, policy, and trusted-service integrations across member accounts.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **member accounts**
  - **Technical definition:** Member accounts are AWS accounts attached to an organization and governed through inherited or directly attached organization controls.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **organizational units (OUs)**
  - **Technical definition:** Organizational units are hierarchy containers inside AWS Organizations used to group accounts for policy inheritance and delegation boundaries.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **delegated administrators**
  - **Technical definition:** Delegated administrators are member accounts explicitly authorized to manage supported AWS services on behalf of the organization without using the management account for all operations.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **service control policies (SCPs)**
  - **Technical definition:** Service control policies are organization-level permission guardrails that define the maximum available AWS API actions for accounts or OUs, regardless of IAM allow policy.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **resource control policies (RCPs)**
  - **Technical definition:** Resource control policies are AWS Organizations policies that constrain supported resource configurations across accounts as part of centralized governance.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **tag policies**
  - **Technical definition:** Tag policies define allowed tag keys, values, and capitalization patterns across accounts to improve governance, automation, and cost allocation consistency.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **backup policies**
  - **Technical definition:** Backup policies are AWS Organizations governance policies that standardize backup-plan behavior for supported resources across accounts.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **AI services opt-out policies**
  - **Technical definition:** AI services opt-out policies are organization-level controls that govern whether certain AWS AI services may retain or use customer content for service-improvement purposes.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **AWS accounts**
  - **Technical definition:** AWS accounts are hard isolation boundaries for IAM, billing, quotas, and many service control planes, making them the primary unit of tenancy in AWS.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **primary contacts**
  - **Technical definition:** Primary contacts are the main administrative contacts attached to an AWS account for operational and communication ownership.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **alternate contacts**
  - **Technical definition:** Alternate contacts are account-level billing, operations, or security contacts used for role-specific account communications.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **AWS Regions**
  - **Technical definition:** AWS Regions are geographically separate AWS deployment areas whose enablement and service availability determine placement, compliance, and DR topology.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **Region opt-in status**
  - **Technical definition:** Region opt-in status is the per-account enablement state that determines whether opt-in AWS Regions may be used by that account.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.

### AWS Control Tower

- **AWS Control Tower**
  - **Technical definition:** AWS Control Tower is AWS's managed landing-zone orchestration service that provisions and governs multi-account AWS environments using prebuilt guardrails and account-vending workflows.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **landing zone**
  - **Technical definition:** A landing zone in AWS Control Tower is the managed multi-account baseline composed of configured accounts, logging, security services, and guardrail governance.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **Account Factory**
  - **Technical definition:** Account Factory is the Control Tower account-vending workflow that provisions new governed accounts from standardized parameters and baselines.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **governed accounts**
  - **Technical definition:** Governed accounts are AWS accounts enrolled in Control Tower so that prescribed baselines and controls apply continuously.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **registered organizational units (OUs)**
  - **Technical definition:** Registered OUs are organizational units explicitly brought under Control Tower governance and account-factory workflows.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **preventive controls**
  - **Technical definition:** Preventive controls are Control Tower guardrails that block noncompliant actions before they succeed, usually through policy-based enforcement.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **detective controls**
  - **Technical definition:** Detective controls are Control Tower guardrails that evaluate and report noncompliant state after resources or actions exist.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **proactive controls**
  - **Technical definition:** Proactive controls are Control Tower checks that validate infrastructure definitions before deployment so noncompliant templates are rejected early.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **drift status**
  - **Technical definition:** Drift status is Control Tower's view of whether enrolled accounts and resources remain aligned with the expected landing-zone baseline.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.

### AWS Identity and Access Management (IAM)

- **AWS Identity and Access Management (IAM)**
  - **Technical definition:** AWS IAM is AWS's core identity and authorization system for users, roles, policies, temporary credentials, and API permission evaluation.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **IAM users**
  - **Technical definition:** IAM users are long-lived human or programmatic identities in an AWS account with directly attached or inherited permission policy.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **IAM groups**
  - **Technical definition:** IAM groups are collections of IAM users used to simplify permission assignment inside one AWS account.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **IAM roles**
  - **Technical definition:** IAM roles are assumable identities that grant temporary credentials after a trust policy authorizes a caller or service to assume them.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **IAM policies**
  - **Technical definition:** IAM policies are JSON authorization documents that define allowed or denied actions, resources, and conditions during AWS permission evaluation.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **instance profiles**
  - **Technical definition:** Instance profiles are EC2 attachments that make an IAM role available to an instance so workloads can obtain temporary credentials via the instance metadata path.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **access keys**
  - **Technical definition:** Access keys are long-lived IAM user credentials for API access and therefore a high-risk credential form compared with role-based temporary credentials.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **MFA devices**
  - **Technical definition:** MFA devices are second-factor authenticators associated with IAM users or root accounts to strengthen login and high-risk action security.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **permissions boundaries**
  - **Technical definition:** Permissions boundaries are maximum-permission policy filters attached to IAM principals that limit what identity-based policies can ultimately allow.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **role trust policies**
  - **Technical definition:** Role trust policies are resource-based policy documents on IAM roles that define which principals may assume the role and under what conditions.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **analyzers**
  - **Technical definition:** IAM Access Analyzer analyzers are analysis scopes that inspect external or public access paths and policy implications across AWS resources.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **findings**
  - **Technical definition:** IAM Access Analyzer findings are results showing public, cross-account, or otherwise notable access paths or policy issues detected by analysis.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **archive rules**
  - **Technical definition:** Archive rules are analyzer filters that automatically suppress repeated findings matching approved conditions or known exceptions.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **access previews**
  - **Technical definition:** Access previews simulate how a proposed resource policy would affect external access before deployment or modification.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **policy validation checks**
  - **Technical definition:** Policy validation checks are static and semantic analyses of IAM policies that flag errors, overly broad access, or best-practice issues before use.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.

### AWS IAM Identity Center

- **AWS IAM Identity Center**
  - **Technical definition:** AWS IAM Identity Center is AWS's workforce federation and permission-assignment service for granting centrally managed human access into AWS accounts and AWS-integrated applications.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **identity sources**
  - **Technical definition:** Identity sources are the authoritative user and group directories—such as internal store or external identity providers—feeding AWS IAM Identity Center.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **users**
  - **Technical definition:** Users are workforce identities represented in IAM Identity Center for assignment into AWS accounts or AWS-managed applications.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **groups**
  - **Technical definition:** Groups are workforce identity collections used to assign AWS account or application access at scale through Identity Center.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **permission sets**
  - **Technical definition:** Permission sets are reusable access bundles in IAM Identity Center that provision IAM roles and associated permissions into target AWS accounts.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **account assignments**
  - **Technical definition:** Account assignments bind users or groups to permission sets for specific AWS accounts, creating the resulting assumable AWS access.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.
- **application assignments**
  - **Technical definition:** Application assignments grant federated users or groups access into supported SSO-integrated applications through Identity Center.
  - **Concrete production example:** Example: a production service or operator receives only temporary, scoped access through role assumption or federated assignment rather than long-lived standing privilege.

### AWS Service Catalog and AWS Resource Access Manager

- **AWS Service Catalog**
  - **Technical definition:** AWS Service Catalog is AWS's governed self-service catalog for publishing approved infrastructure products and restricting how they are launched and changed.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **AWS Resource Access Manager**
  - **Technical definition:** AWS Resource Access Manager is AWS's cross-account and cross-organization sharing service for selected resource types without requiring full account duplication.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **portfolios**
  - **Technical definition:** Portfolios are Service Catalog containers that group and publish approved products to specific principals or accounts.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **products**
  - **Technical definition:** Products are the governed self-service items in Service Catalog, usually backed by infrastructure templates or provisioning workflows.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **provisioning artifacts**
  - **Technical definition:** Provisioning artifacts are versioned implementations of a Service Catalog product, such as specific CloudFormation template versions.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **provisioned products**
  - **Technical definition:** Provisioned products are live deployed instances launched from a Service Catalog product under governance constraints.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **constraints**
  - **Technical definition:** Constraints are Service Catalog policy objects that limit launch roles, allowed parameters, or template behaviors for products.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **launch paths**
  - **Technical definition:** Launch paths are the specific portfolio-product-principal combinations through which a user is allowed to provision a product.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **portfolio access**
  - **Technical definition:** Portfolio access is the authorization relationship that determines which principals or accounts may view or launch products within a portfolio.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **resource shares (AWS RAM shares)**
  - **Technical definition:** RAM shares are sharing constructs that grant selected principals access to supported AWS resources across account boundaries.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **principals**
  - **Technical definition:** Principals are the accounts, OUs, or organizations authorized to consume a shared resource or portfolio.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **resources**
  - **Technical definition:** Resources are the supported AWS entities exposed through RAM sharing rather than duplicated into each account.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **managed permissions**
  - **Technical definition:** Managed permissions are AWS-defined permission templates controlling what a shared-resource consumer may do with a RAM-shared resource.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **customer managed permissions**
  - **Technical definition:** Customer managed permissions are customer-authored RAM permission definitions that tailor consumer actions on shared resources.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **invitations**
  - **Technical definition:** Invitations are the share-acceptance objects used when RAM resource sharing requires an explicit recipient acknowledgment.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.

### AWS CloudFormation StackSets

- **AWS CloudFormation StackSets**
  - **Technical definition:** AWS CloudFormation StackSets is the multi-account, multi-Region deployment layer for centrally distributing CloudFormation-managed stacks with coordinated operations and target selection.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **stack sets (StackSets)**
  - **Technical definition:** StackSets are top-level multi-account CloudFormation deployment collections that define one template and parameter model for many targets.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **stack instances**
  - **Technical definition:** Stack instances are the per-account, per-Region realizations of a StackSet deployment.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **stack operations**
  - **Technical definition:** Stack operations are coordinated create, update, or delete actions executed by StackSets across its target stack instances.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **administrator accounts**
  - **Technical definition:** Administrator accounts are the AWS accounts from which StackSet operations are initiated and centrally managed.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **execution roles**
  - **Technical definition:** Execution roles are target-account IAM roles assumed by StackSets to create or update resources in each target account.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **target accounts**
  - **Technical definition:** Target accounts are the AWS accounts where StackSet instances are deployed.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **target organizational units (OUs)**
  - **Technical definition:** Target OUs are organizational hierarchy scopes used by StackSets for account selection in service-managed deployments.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.
- **operation preferences**
  - **Technical definition:** Operation preferences define concurrency, failure tolerance, Region order, and deployment behavior for StackSet operations.
  - **Concrete production example:** Example: a platform team uses this during new-account onboarding so every production account inherits the same baseline controls, logging paths, and approved deployment model.

### AWS CloudTrail and AWS Config

- **AWS CloudTrail**
  - **Technical definition:** AWS CloudTrail is AWS's API activity audit service for recording management, data, and insight events across accounts and Regions.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **AWS Config**
  - **Technical definition:** AWS Config is AWS's configuration recording and compliance-evaluation service for tracking resource state, change history, rules, and remediation posture.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **trails**
  - **Technical definition:** CloudTrail trails are configurations that deliver selected AWS API events to destinations such as S3 and CloudWatch Logs.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **organization trails**
  - **Technical definition:** Organization trails are CloudTrail trails applied at the AWS Organizations level so member-account events are captured centrally.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **event data stores**
  - **Technical definition:** Event data stores are CloudTrail Lake-backed storage constructs for queryable audit events with configurable retention and selectors.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **management events**
  - **Technical definition:** Management events are CloudTrail records for control-plane API activity such as create, modify, and delete operations on AWS resources.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **data events**
  - **Technical definition:** Data events are higher-volume CloudTrail records for resource-level operations such as S3 object access or Lambda invocation.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **Insights events**
  - **Technical definition:** Insights events are CloudTrail anomaly-detection outputs highlighting unusual API rate or error patterns in management activity.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **configuration recorders**
  - **Technical definition:** Configuration recorders are AWS Config components that capture supported resource configuration state and changes over time.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **delivery channels**
  - **Technical definition:** Delivery channels are AWS Config outputs that specify where configuration snapshots and change notifications are delivered.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **configuration items**
  - **Technical definition:** Configuration items are the versioned AWS Config records representing the state of one resource at one point in time.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **rules**
  - **Technical definition:** AWS Config rules are compliance evaluations that test resource configuration or account settings against desired conditions.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **conformance packs**
  - **Technical definition:** Conformance packs are collections of AWS Config rules and remediation guidance packaged for repeatable governance deployment.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **configuration aggregators**
  - **Technical definition:** Configuration aggregators are AWS Config views that consolidate configuration and compliance data across accounts and Regions.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **remediation actions**
  - **Technical definition:** Remediation actions are AWS Config-integrated automated or manual workflows that correct detected noncompliant state.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.

### AWS Security Hub and AWS Audit Manager

- **AWS Security Hub**
  - **Technical definition:** AWS Security Hub is AWS's centralized security finding aggregation and standards-evaluation service across AWS accounts and Regions.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **AWS Audit Manager**
  - **Technical definition:** AWS Audit Manager is AWS's evidence-oriented assessment service that maps AWS control evidence into audit frameworks and reports.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **security standards**
  - **Technical definition:** Within Security Hub or Audit Manager, security standards is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **controls**
  - **Technical definition:** Within Security Hub or Audit Manager, controls is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **findings**
  - **Technical definition:** Within Security Hub or Audit Manager, findings is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **insights**
  - **Technical definition:** Within Security Hub or Audit Manager, insights is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **automation rules**
  - **Technical definition:** Within Security Hub or Audit Manager, automation rules is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **finding aggregators**
  - **Technical definition:** Within Security Hub or Audit Manager, finding aggregators is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **administrator accounts**
  - **Technical definition:** Within Security Hub or Audit Manager, administrator accounts is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **member accounts**
  - **Technical definition:** Within Security Hub or Audit Manager, member accounts is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **assessments**
  - **Technical definition:** Within Security Hub or Audit Manager, assessments is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **frameworks**
  - **Technical definition:** Within Security Hub or Audit Manager, frameworks is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **control sets**
  - **Technical definition:** Within Security Hub or Audit Manager, control sets is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **evidence folders**
  - **Technical definition:** Within Security Hub or Audit Manager, evidence folders is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **evidence**
  - **Technical definition:** Within Security Hub or Audit Manager, evidence is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **Evidence Finder**
  - **Technical definition:** Within Security Hub or Audit Manager, Evidence Finder is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **assessment reports**
  - **Technical definition:** Within Security Hub or Audit Manager, assessment reports is a standards, finding, evidence, or assessment object used to aggregate security posture and audit support.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.

### AWS Artifact, AWS Health, and AWS Service Quotas

- **AWS Artifact**
  - **Technical definition:** AWS Artifact is AWS's portal for compliance reports, attestations, and contractual artifacts such as agreements and audit documents.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **AWS Health**
  - **Technical definition:** AWS Health is AWS's account- and organization-aware service event feed for provider-side incidents, planned changes, and affected resources.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **and AWS Service Quotas**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, and AWS Service Quotas is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **reports**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, reports is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **agreements**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, agreements is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **notifications**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, notifications is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **AWS Health events**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, AWS Health events is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **affected entities**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, affected entities is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **event status codes**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, event status codes is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **service quotas**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, service quotas is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **applied quota values**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, applied quota values is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **quota increase requests**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, quota increase requests is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **quota request templates**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, quota request templates is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.
- **usage metrics**
  - **Technical definition:** Within AWS Artifact, AWS Health, or Service Quotas, usage metrics is a compliance, provider-event, or limit-management entity relevant to governance and operational readiness.
  - **Concrete production example:** Example: during an audit or incident, the team uses this to prove who changed a resource, whether configuration drift exists, and whether quotas or provider events contributed to risk.

## Controls

### Operational And Control Points

- **AWS Organizations policy inheritance**
  - **Technical definition:** AWS Organizations policy inheritance is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **trusted access**
  - **Technical definition:** trusted access is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **delegated administration**
  - **Technical definition:** delegated administration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **consolidated billing**
  - **Technical definition:** consolidated billing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **account contact ownership**
  - **Technical definition:** account contact ownership is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Region opt-in/opt-out**
  - **Technical definition:** Region opt-in/opt-out is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **account metadata governance**
  - **Technical definition:** account metadata governance is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Control Tower account baselines**
  - **Technical definition:** AWS Control Tower account baselines is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **control enablement**
  - **Technical definition:** control enablement is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **drift detection**
  - **Technical definition:** drift detection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **landing-zone versioning**
  - **Technical definition:** landing-zone versioning is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **control remediation**
  - **Technical definition:** control remediation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **IAM identity-based policies**
  - **Technical definition:** IAM identity-based policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **role assumption**
  - **Technical definition:** role assumption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **multi-factor authentication (MFA)**
  - **Technical definition:** multi-factor authentication (MFA) is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **attribute-based access control (ABAC)**
  - **Technical definition:** attribute-based access control (ABAC) is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Security Token Service (AWS STS)**
  - **Technical definition:** AWS Security Token Service (AWS STS) is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM Access Analyzer findings**
  - **Technical definition:** IAM Access Analyzer findings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS IAM Identity Center workforce identity federation**
  - **Technical definition:** AWS IAM Identity Center workforce identity federation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **permission-set provisioning**
  - **Technical definition:** permission-set provisioning is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **session duration**
  - **Technical definition:** session duration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **account assignment review**
  - **Technical definition:** account assignment review is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Service Catalog launch constraints**
  - **Technical definition:** AWS Service Catalog launch constraints is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **template constraints**
  - **Technical definition:** template constraints is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **tag update constraints**
  - **Technical definition:** tag update constraints is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **portfolio sharing**
  - **Technical definition:** portfolio sharing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **launch permissions**
  - **Technical definition:** launch permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Resource Access Manager resource-share permissions**
  - **Technical definition:** AWS Resource Access Manager resource-share permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS CloudFormation StackSets service-managed permissions**
  - **Technical definition:** AWS CloudFormation StackSets service-managed permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **self-managed permissions**
  - **Technical definition:** self-managed permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **automatic deployments**
  - **Technical definition:** automatic deployments is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **account filters**
  - **Technical definition:** account filters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Region targeting**
  - **Technical definition:** Region targeting is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **failure tolerance**
  - **Technical definition:** failure tolerance is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **concurrency**
  - **Technical definition:** concurrency is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS CloudTrail multi-Region trails**
  - **Technical definition:** AWS CloudTrail multi-Region trails is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **organization-wide logging**
  - **Technical definition:** organization-wide logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **log file integrity validation**
  - **Technical definition:** log file integrity validation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **retention**
  - **Technical definition:** retention is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Config organization aggregation**
  - **Technical definition:** AWS Config organization aggregation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Security Hub delegated administration**
  - **Technical definition:** AWS Security Hub delegated administration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region aggregation**
  - **Technical definition:** cross-Region aggregation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Foundational Security Best Practices**
  - **Technical definition:** AWS Foundational Security Best Practices is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **CIS AWS Foundations Benchmark**
  - **Technical definition:** CIS AWS Foundations Benchmark is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Audit Manager assessment scoping**
  - **Technical definition:** AWS Audit Manager assessment scoping is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Artifact report access**
  - **Technical definition:** AWS Artifact report access is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **agreement acceptance**
  - **Technical definition:** agreement acceptance is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Health organizational view**
  - **Technical definition:** AWS Health organizational view is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Service Quotas baselines**
  - **Technical definition:** Service Quotas baselines is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **utilization monitoring**
  - **Technical definition:** utilization monitoring is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **quota-risk review**
  - **Technical definition:** quota-risk review is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Operational And Control Points

- **Create a landing-zone account model with management account**
  - **Technical definition:** Create a landing-zone account model with management account is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **shared services accounts**
  - **Technical definition:** shared services accounts is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **workload accounts**
  - **Technical definition:** workload accounts is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **environment-specific OUs**
  - **Technical definition:** environment-specific OUs is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Design an SCP model for preventive guardrails without blocking required platform operations**
  - **Technical definition:** Design an SCP model for preventive guardrails without blocking required platform operations is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Enroll accounts and OUs into AWS Control Tower and review drift remediation paths**
  - **Technical definition:** Enroll accounts and OUs into AWS Control Tower and review drift remediation paths is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Review IAM role trust policies**
  - **Technical definition:** Review IAM role trust policies is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **access keys**
  - **Technical definition:** access keys is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **unused access findings**
  - **Technical definition:** unused access findings is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Provision AWS IAM Identity Center permission sets and validate account assignments**
  - **Technical definition:** Provision AWS IAM Identity Center permission sets and validate account assignments is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Use AWS CloudFormation StackSets for multi-account baseline deployment and rollback**
  - **Technical definition:** Use AWS CloudFormation StackSets for multi-account baseline deployment and rollback is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Investigate API activity with AWS CloudTrail and validate drift/compliance with AWS Config**
  - **Technical definition:** Investigate API activity with AWS CloudTrail and validate drift/compliance with AWS Config is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Triage AWS Security Hub findings and prepare AWS Audit Manager evidence for compliance review**
  - **Technical definition:** Triage AWS Security Hub findings and prepare AWS Audit Manager evidence for compliance review is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Validate landing-zone readiness before migration by checking identity**
  - **Technical definition:** Validate landing-zone readiness before migration by checking identity is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **audit**
  - **Technical definition:** audit is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **account ownership**
  - **Technical definition:** account ownership is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **quotas**
  - **Technical definition:** quotas is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **health events**
  - **Technical definition:** health events is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 2: Networking, Private Connectivity, And Edge Traffic

## Service And Entity Coverage

### Amazon Virtual Private Cloud (Amazon VPC)

- **Amazon Virtual Private Cloud (Amazon VPC)**
  - **Technical definition:** Amazon VPC is AWS's logically isolated network boundary for IP address management, segmentation, routing, security attachment, and private service connectivity.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **VPCs**
  - **Technical definition:** Within Amazon VPC, VPCs is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **subnets**
  - **Technical definition:** Within Amazon VPC, subnets is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **route tables**
  - **Technical definition:** Within Amazon VPC, route tables is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **internet gateways**
  - **Technical definition:** Within Amazon VPC, internet gateways is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **egress-only internet gateways**
  - **Technical definition:** Within Amazon VPC, egress-only internet gateways is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **NAT gateways**
  - **Technical definition:** Within Amazon VPC, NAT gateways is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **elastic IP addresses**
  - **Technical definition:** Within Amazon VPC, elastic IP addresses is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **elastic network interfaces**
  - **Technical definition:** Within Amazon VPC, elastic network interfaces is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **prefix lists**
  - **Technical definition:** Within Amazon VPC, prefix lists is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **DHCP option sets**
  - **Technical definition:** Within Amazon VPC, DHCP option sets is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **security groups**
  - **Technical definition:** Within Amazon VPC, security groups is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **network ACLs**
  - **Technical definition:** Within Amazon VPC, network ACLs is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **VPC Flow Logs**
  - **Technical definition:** Within Amazon VPC, VPC Flow Logs is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Amazon VPC Reachability Analyzer**
  - **Technical definition:** Within Amazon VPC, Amazon VPC Reachability Analyzer is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Amazon VPC Network Access Analyzer**
  - **Technical definition:** Within Amazon VPC, Amazon VPC Network Access Analyzer is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Amazon VPC IP Address Manager (IPAM)**
  - **Technical definition:** Within Amazon VPC, Amazon VPC IP Address Manager (IPAM) is a network construct that determines address allocation, attachment behavior, traffic routing, isolation, or path visibility inside AWS virtual networking.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### AWS Transit Gateway and AWS Cloud WAN

- **AWS Transit Gateway**
  - **Technical definition:** AWS Transit Gateway is AWS's regional network transit hub for connecting VPCs, VPNs, and Direct Connect paths under centrally managed route domains.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **AWS Cloud WAN**
  - **Technical definition:** AWS Cloud WAN is AWS's managed global network control plane for building segmented wide-area connectivity across Regions and sites.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **transit gateways**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, transit gateways is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **transit gateway attachments**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, transit gateway attachments is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **transit gateway route tables**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, transit gateway route tables is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **route table associations**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, route table associations is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **route table propagations**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, route table propagations is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **peering attachments**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, peering attachments is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **global networks**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, global networks is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **core networks**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, core networks is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **core network policies**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, core network policies is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **segments**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, segments is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **attachments**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, attachments is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **network function groups**
  - **Technical definition:** Within AWS Transit Gateway or Cloud WAN, network function groups is a transit-network entity that shapes attachment topology, route-domain behavior, segmentation, or global propagation.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### Amazon VPC Peering, Amazon VPC Endpoints, and AWS PrivateLink

- **Amazon VPC Peering**
  - **Technical definition:** Amazon VPC Peering is AWS's direct private routing relationship between VPCs without a transit gateway or internet traversal.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Amazon VPC Endpoints**
  - **Technical definition:** Amazon VPC Endpoints provide private access from a VPC to supported AWS services without using public internet paths.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **and AWS PrivateLink**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, and AWS PrivateLink is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **VPC peering connections**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, VPC peering connections is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **requester VPCs**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, requester VPCs is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **accepter VPCs**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, accepter VPCs is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **interface endpoints**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, interface endpoints is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **gateway endpoints**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, gateway endpoints is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Gateway Load Balancer endpoints**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, Gateway Load Balancer endpoints is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **endpoint services**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, endpoint services is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **endpoint network interfaces**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, endpoint network interfaces is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **endpoint policies**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, endpoint policies is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **private DNS names**
  - **Technical definition:** Within VPC peering, endpoints, or PrivateLink, private DNS names is a private-connectivity entity that controls how one VPC or service reaches another without public internet exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### Amazon VPC Lattice and AWS Verified Access

- **Amazon VPC Lattice**
  - **Technical definition:** Amazon VPC Lattice is AWS's application networking layer for service-to-service connectivity, auth policy, and request routing across VPC and account boundaries.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **AWS Verified Access**
  - **Technical definition:** AWS Verified Access is AWS's identity- and device-context-aware application access service for replacing coarse network trust with policy-driven private application access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **service networks**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, service networks is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **services**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, services is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **target groups**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, target groups is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **listeners**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, listeners is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **rules**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, rules is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **auth policies**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, auth policies is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **resource configurations**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, resource configurations is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Verified Access instances**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, Verified Access instances is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Verified Access groups**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, Verified Access groups is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Verified Access endpoints**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, Verified Access endpoints is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **trust providers**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, trust providers is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **access policies**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, access policies is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **application domains**
  - **Technical definition:** Within Amazon VPC Lattice or AWS Verified Access, application domains is an application-connectivity or policy object that controls service exposure, request routing, or identity-aware access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### AWS Network Firewall and AWS Firewall Manager

- **AWS Network Firewall**
  - **Technical definition:** AWS Network Firewall is AWS's managed stateful and stateless network inspection service for VPC traffic filtering, domain controls, and TLS-aware inspection.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **AWS Firewall Manager**
  - **Technical definition:** AWS Firewall Manager is AWS's organization-scale policy management service for centrally applying and auditing supported security controls such as WAF and network firewall policies.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **firewalls**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, firewalls is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **firewall policies**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, firewall policies is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **rule groups**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, rule groups is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **stateless rules**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, stateless rules is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **stateful rules**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, stateful rules is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **TLS inspection configurations**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, TLS inspection configurations is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **alert logs**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, alert logs is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **flow logs**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, flow logs is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Firewall Manager policies**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, Firewall Manager policies is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **policy scopes**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, policy scopes is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **resource sets**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, resource sets is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **compliance status**
  - **Technical definition:** Within AWS Network Firewall or Firewall Manager, compliance status is a policy or telemetry construct that determines inspection, enforcement scope, or security compliance status.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### Elastic Load Balancing and AWS Certificate Manager

- **Elastic Load Balancing**
  - **Technical definition:** Elastic Load Balancing is AWS's managed traffic distribution system for L4/L7 request routing, target health evaluation, and service exposure.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **AWS Certificate Manager**
  - **Technical definition:** AWS Certificate Manager is AWS's managed certificate lifecycle service for issuing, importing, renewing, and binding TLS certificates to AWS-integrated services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Application Load Balancers**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, Application Load Balancers is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Network Load Balancers**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, Network Load Balancers is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Gateway Load Balancers**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, Gateway Load Balancers is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **target groups**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, target groups is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **listeners**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, listeners is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **listener rules**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, listener rules is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **health checks**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, health checks is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **load balancer attributes**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, load balancer attributes is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **public certificates**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, public certificates is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **private certificates**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, private certificates is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **imported certificates**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, imported certificates is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **domain names**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, domain names is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **subject alternative names (SANs)**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, subject alternative names (SANs) is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **validation options**
  - **Technical definition:** Within Elastic Load Balancing or AWS Certificate Manager, validation options is a traffic-distribution, TLS, or target-evaluation object that affects service exposure and request handling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### AWS Site-to-Site VPN, AWS Client VPN, and AWS Direct Connect

- **AWS Site-to-Site VPN**
  - **Technical definition:** AWS Site-to-Site VPN is AWS's IPSec-based hybrid network connectivity service between AWS and customer-managed gateway devices.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **AWS Client VPN**
  - **Technical definition:** AWS Client VPN is AWS's managed remote-user VPN service that terminates user tunnels into AWS VPC environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **and AWS Direct Connect**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, and AWS Direct Connect is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **customer gateways**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, customer gateways is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **virtual private gateways**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, virtual private gateways is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **VPN connections**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, VPN connections is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **VPN tunnels**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, VPN tunnels is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **tunnel options**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, tunnel options is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **customer gateway devices**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, customer gateway devices is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Client VPN endpoints**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, Client VPN endpoints is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **target network associations**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, target network associations is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **authorization rules**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, authorization rules is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Direct Connect connections**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, Direct Connect connections is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **link aggregation groups (LAGs)**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, link aggregation groups (LAGs) is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **private virtual interfaces**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, private virtual interfaces is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **public virtual interfaces**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, public virtual interfaces is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **transit virtual interfaces**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, transit virtual interfaces is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Direct Connect gateways**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, Direct Connect gateways is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **hosted connections**
  - **Technical definition:** Within AWS VPN, Client VPN, or Direct Connect, hosted connections is a connectivity entity that controls tunnel establishment, route exchange, private-path characteristics, or hybrid-network access.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### Amazon Route 53 and Amazon Route 53 Resolver

- **Amazon Route 53**
  - **Technical definition:** Amazon Route 53 is AWS's DNS service for authoritative zones, health-aware routing, domain registration, and traffic steering.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Amazon Route 53 Resolver**
  - **Technical definition:** Amazon Route 53 Resolver is AWS's recursive and hybrid DNS component for inbound/outbound forwarding, rule-based resolution, and DNS firewall enforcement.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **hosted zones**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, hosted zones is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **records**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, records is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **routing policies**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, routing policies is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **health checks**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, health checks is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **reusable delegation sets**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, reusable delegation sets is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **domains**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, domains is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **key-signing keys (KSKs)**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, key-signing keys (KSKs) is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **inbound Resolver endpoints**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, inbound Resolver endpoints is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **outbound Resolver endpoints**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, outbound Resolver endpoints is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **Resolver rules**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, Resolver rules is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **rule associations**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, rule associations is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **query logging configurations**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, query logging configurations is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **DNS Firewall rule groups**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, DNS Firewall rule groups is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **domain lists**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, domain lists is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### Amazon CloudFront, AWS WAF, AWS Shield, and AWS Global Accelerator

- **Amazon CloudFront**
  - **Technical definition:** Amazon CloudFront is AWS's global content delivery and edge request-processing network for cached and dynamic HTTP(S) traffic.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **AWS WAF**
  - **Technical definition:** AWS WAF is AWS's managed application-layer filtering service for HTTP request inspection, rate controls, and rule-driven request blocking or labeling.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **AWS Shield**
  - **Technical definition:** AWS Shield is AWS's DDoS protection service for AWS resources, with Shield Advanced adding enhanced detection, response support, and additional protections.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **and AWS Global Accelerator**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, and AWS Global Accelerator is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **distributions**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, distributions is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **origins**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, origins is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **cache behaviors**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, cache behaviors is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **cache policies**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, cache policies is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **origin request policies**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, origin request policies is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **response headers policies**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, response headers policies is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **invalidations**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, invalidations is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **key groups**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, key groups is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **origin access controls**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, origin access controls is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **web ACLs**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, web ACLs is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **rules**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, rules is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **rule groups**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, rule groups is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **managed rule groups**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, managed rule groups is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **IP sets**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, IP sets is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **labels**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, labels is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **protections**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, protections is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **protection groups**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, protection groups is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **accelerators**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, accelerators is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **listeners**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, listeners is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **endpoint groups**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, endpoint groups is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **endpoints**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, endpoints is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **static anycast IP addresses**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, static anycast IP addresses is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **endpoint weights**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, endpoint weights is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **traffic dials**
  - **Technical definition:** Within CloudFront, AWS WAF, Shield, or Global Accelerator, traffic dials is an edge-routing, cache, traffic-protection, or acceleration construct exposed at AWS's global ingress layer.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### Amazon API Gateway

- **Amazon API Gateway**
  - **Technical definition:** Amazon API Gateway is AWS's managed API front-door for REST, HTTP, and WebSocket APIs with auth, throttling, staging, and backend integration.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **REST APIs**
  - **Technical definition:** Within Amazon API Gateway, REST APIs is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **HTTP APIs**
  - **Technical definition:** Within Amazon API Gateway, HTTP APIs is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **WebSocket APIs**
  - **Technical definition:** Within Amazon API Gateway, WebSocket APIs is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **resources**
  - **Technical definition:** Within Amazon API Gateway, resources is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **routes**
  - **Technical definition:** Within Amazon API Gateway, routes is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **methods**
  - **Technical definition:** Within Amazon API Gateway, methods is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **integrations**
  - **Technical definition:** Within Amazon API Gateway, integrations is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **stages**
  - **Technical definition:** Within Amazon API Gateway, stages is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **deployments**
  - **Technical definition:** Within Amazon API Gateway, deployments is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **authorizers**
  - **Technical definition:** Within Amazon API Gateway, authorizers is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **usage plans**
  - **Technical definition:** Within Amazon API Gateway, usage plans is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **API keys**
  - **Technical definition:** Within Amazon API Gateway, API keys is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **custom domain names**
  - **Technical definition:** Within Amazon API Gateway, custom domain names is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **VPC links**
  - **Technical definition:** Within Amazon API Gateway, VPC links is an API publication, integration, deployment, or access-control object that affects how clients reach backend services.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

## Controls

### Operational And Control Points

- **VPC CIDR allocation**
  - **Technical definition:** VPC CIDR allocation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **subnet routing**
  - **Technical definition:** subnet routing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **route table associations**
  - **Technical definition:** route table associations is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **security group rules**
  - **Technical definition:** security group rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **network ACL rules**
  - **Technical definition:** network ACL rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **flow log delivery**
  - **Technical definition:** flow log delivery is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IPAM pool rules**
  - **Technical definition:** IPAM pool rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **overlapping CIDR detection**
  - **Technical definition:** overlapping CIDR detection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Transit Gateway route domains**
  - **Technical definition:** Transit Gateway route domains is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **attachment acceptance**
  - **Technical definition:** attachment acceptance is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **route propagation**
  - **Technical definition:** route propagation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **appliance mode**
  - **Technical definition:** appliance mode is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Resource Access Manager sharing**
  - **Technical definition:** AWS Resource Access Manager sharing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Cloud WAN segment isolation**
  - **Technical definition:** Cloud WAN segment isolation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **attachment policies**
  - **Technical definition:** attachment policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **route sharing**
  - **Technical definition:** route sharing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **VPC peering non-overlapping CIDRs**
  - **Technical definition:** VPC peering non-overlapping CIDRs is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint policies**
  - **Technical definition:** endpoint policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **private DNS**
  - **Technical definition:** private DNS is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **allowed principals**
  - **Technical definition:** allowed principals is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **acceptance settings**
  - **Technical definition:** acceptance settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **private service exposure boundaries**
  - **Technical definition:** private service exposure boundaries is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **VPC Lattice service network associations**
  - **Technical definition:** VPC Lattice service network associations is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **routing rules**
  - **Technical definition:** routing rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **TLS settings**
  - **Technical definition:** TLS settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **target group health checks**
  - **Technical definition:** target group health checks is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Verified Access identity trust**
  - **Technical definition:** Verified Access identity trust is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **device trust**
  - **Technical definition:** device trust is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **policy-based access**
  - **Technical definition:** policy-based access is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **access log delivery**
  - **Technical definition:** access log delivery is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Network Firewall centralized inspection**
  - **Technical definition:** AWS Network Firewall centralized inspection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Suricata-compatible rules**
  - **Technical definition:** Suricata-compatible rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **domain list rules**
  - **Technical definition:** domain list rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **default actions**
  - **Technical definition:** default actions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Firewall Manager policies**
  - **Technical definition:** AWS Firewall Manager policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **security group policies**
  - **Technical definition:** security group policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Elastic Load Balancing TLS security policies**
  - **Technical definition:** Elastic Load Balancing TLS security policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **certificates**
  - **Technical definition:** certificates is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **target group attributes**
  - **Technical definition:** target group attributes is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-zone load balancing**
  - **Technical definition:** cross-zone load balancing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **access logs**
  - **Technical definition:** access logs is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Certificate Manager managed renewal**
  - **Technical definition:** AWS Certificate Manager managed renewal is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **VPN IKE/IPsec parameters**
  - **Technical definition:** VPN IKE/IPsec parameters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **BGP routing**
  - **Technical definition:** BGP routing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **tunnel inside CIDRs**
  - **Technical definition:** tunnel inside CIDRs is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **pre-shared keys**
  - **Technical definition:** pre-shared keys is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Client VPN authorization rules**
  - **Technical definition:** Client VPN authorization rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Direct Connect BGP authentication**
  - **Technical definition:** Direct Connect BGP authentication is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **allowed prefixes**
  - **Technical definition:** allowed prefixes is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **MACsec**
  - **Technical definition:** MACsec is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **resiliency model**
  - **Technical definition:** resiliency model is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Route 53 TTLs**
  - **Technical definition:** Route 53 TTLs is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **routing policies**
  - **Technical definition:** routing policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **health-check routing**
  - **Technical definition:** health-check routing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **DNSSEC signing**
  - **Technical definition:** DNSSEC signing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **delegation**
  - **Technical definition:** delegation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Route 53 Resolver conditional forwarding**
  - **Technical definition:** Route 53 Resolver conditional forwarding is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **DNS Firewall rules**
  - **Technical definition:** DNS Firewall rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **query logging**
  - **Technical definition:** query logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **CloudFront viewer protocol policies**
  - **Technical definition:** CloudFront viewer protocol policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **signed URLs**
  - **Technical definition:** signed URLs is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **signed cookies**
  - **Technical definition:** signed cookies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **origin access control**
  - **Technical definition:** origin access control is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS WAF rate-based rules**
  - **Technical definition:** AWS WAF rate-based rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Bot Control**
  - **Technical definition:** Bot Control is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Shield Advanced**
  - **Technical definition:** AWS Shield Advanced is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Global Accelerator traffic dials**
  - **Technical definition:** Global Accelerator traffic dials is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint weights**
  - **Technical definition:** endpoint weights is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **API Gateway authorizers**
  - **Technical definition:** API Gateway authorizers is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource policies**
  - **Technical definition:** resource policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **mutual TLS**
  - **Technical definition:** mutual TLS is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **stage variables**
  - **Technical definition:** stage variables is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **request validation**
  - **Technical definition:** request validation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS WAF associations**
  - **Technical definition:** AWS WAF associations is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Operational And Control Points

- **Validate VPC packet paths with route tables**
  - **Technical definition:** Validate VPC packet paths with route tables is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **security groups**
  - **Technical definition:** security groups is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **network ACLs**
  - **Technical definition:** network ACLs is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Reachability Analyzer**
  - **Technical definition:** Reachability Analyzer is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Network Access Analyzer**
  - **Technical definition:** Network Access Analyzer is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Review NAT gateway saturation**
  - **Technical definition:** Review NAT gateway saturation is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IPAM allocation**
  - **Technical definition:** IPAM allocation is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **route drift**
  - **Technical definition:** route drift is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **overlapping CIDRs before migration**
  - **Technical definition:** overlapping CIDRs before migration is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Onboard Transit Gateway attachments**
  - **Technical definition:** Onboard Transit Gateway attachments is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate route propagation**
  - **Technical definition:** validate route propagation is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **test segmentation between network domains**
  - **Technical definition:** test segmentation between network domains is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Publish or consume private services through VPC endpoints and AWS PrivateLink without internet exposure**
  - **Technical definition:** Publish or consume private services through VPC endpoints and AWS PrivateLink without internet exposure is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Investigate dropped flows in AWS Network Firewall and tune rule groups safely**
  - **Technical definition:** Investigate dropped flows in AWS Network Firewall and tune rule groups safely is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Review Elastic Load Balancing target health**
  - **Technical definition:** Review Elastic Load Balancing target health is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **listener rules**
  - **Technical definition:** listener rules is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **TLS certificates**
  - **Technical definition:** TLS certificates is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **traffic shifting**
  - **Technical definition:** traffic shifting is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Test Site-to-Site VPN tunnel failover and Direct Connect backup routing**
  - **Technical definition:** Test Site-to-Site VPN tunnel failover and Direct Connect backup routing is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Execute DNS cutover with Route 53 records**
  - **Technical definition:** Execute DNS cutover with Route 53 records is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Resolver rules**
  - **Technical definition:** Resolver rules is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **rollback TTL planning**
  - **Technical definition:** rollback TTL planning is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Tune CloudFront cache behavior**
  - **Technical definition:** Tune CloudFront cache behavior is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **origin failover**
  - **Technical definition:** origin failover is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS WAF false positives**
  - **Technical definition:** AWS WAF false positives is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Global Accelerator failover**
  - **Technical definition:** Global Accelerator failover is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Deploy and troubleshoot API Gateway stages**
  - **Technical definition:** Deploy and troubleshoot API Gateway stages is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **private integrations**
  - **Technical definition:** private integrations is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **access logs**
  - **Technical definition:** access logs is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **canary releases**
  - **Technical definition:** canary releases is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 3: Compute, Kubernetes, CI/CD, And Platform Delivery

## Service And Entity Coverage

### Amazon Elastic Compute Cloud (Amazon EC2) and Amazon EC2 Auto Scaling

- **Amazon Elastic Compute Cloud (Amazon EC2)**
  - **Technical definition:** Amazon EC2 is AWS's virtual machine service for provisioning instance-based compute with customer-managed OS and runtime control.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Amazon EC2 Auto Scaling**
  - **Technical definition:** Amazon EC2 Auto Scaling is AWS's fleet-scaling and instance-refresh control plane for adjusting EC2 capacity to match policy and demand.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **instances**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, instances is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Amazon Machine Images (AMIs)**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, Amazon Machine Images (AMIs) is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **instance types**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, instance types is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **key pairs**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, key pairs is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **elastic network interfaces**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, elastic network interfaces is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **instance profiles**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, instance profiles is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **user data**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, user data is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Elastic IP addresses**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, Elastic IP addresses is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **EC2 Image Builder image pipelines**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, EC2 Image Builder image pipelines is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Auto Scaling groups**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, Auto Scaling groups is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **launch templates**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, launch templates is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **scaling policies**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, scaling policies is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **scheduled actions**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, scheduled actions is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **lifecycle hooks**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, lifecycle hooks is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **warm pools**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, warm pools is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **instance refreshes**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, instance refreshes is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **health checks**
  - **Technical definition:** Within EC2 or EC2 Auto Scaling, health checks is a compute or fleet-management entity that shapes instance lifecycle, scaling behavior, or capacity placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### Amazon Elastic Container Registry (Amazon ECR)

- **Amazon Elastic Container Registry (Amazon ECR)**
  - **Technical definition:** Amazon ECR is AWS's managed OCI-compatible image registry for storing, replicating, and controlling access to container images and artifacts.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **private repositories**
  - **Technical definition:** Within Amazon ECR, private repositories is an image-registry entity that affects image storage, promotion, security, or distribution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **public repositories**
  - **Technical definition:** Within Amazon ECR, public repositories is an image-registry entity that affects image storage, promotion, security, or distribution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **images**
  - **Technical definition:** Within Amazon ECR, images is an image-registry entity that affects image storage, promotion, security, or distribution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **tags**
  - **Technical definition:** Within Amazon ECR, tags is an image-registry entity that affects image storage, promotion, security, or distribution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **manifests**
  - **Technical definition:** Within Amazon ECR, manifests is an image-registry entity that affects image storage, promotion, security, or distribution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **lifecycle policies**
  - **Technical definition:** Within Amazon ECR, lifecycle policies is an image-registry entity that affects image storage, promotion, security, or distribution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **pull through cache rules**
  - **Technical definition:** Within Amazon ECR, pull through cache rules is an image-registry entity that affects image storage, promotion, security, or distribution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **replication configurations**
  - **Technical definition:** Within Amazon ECR, replication configurations is an image-registry entity that affects image storage, promotion, security, or distribution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### Amazon Elastic Container Service (Amazon ECS) and AWS Fargate

- **Amazon Elastic Container Service (Amazon ECS)**
  - **Technical definition:** Amazon ECS is AWS's container orchestration service for scheduling tasks and services across EC2 or Fargate capacity.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AWS Fargate**
  - **Technical definition:** AWS Fargate is AWS's serverless container compute engine that runs ECS tasks or EKS pods without customer-managed worker nodes.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **clusters**
  - **Technical definition:** Within ECS or Fargate, clusters is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **services**
  - **Technical definition:** Within ECS or Fargate, services is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **task definitions**
  - **Technical definition:** Within ECS or Fargate, task definitions is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **tasks**
  - **Technical definition:** Within ECS or Fargate, tasks is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **capacity providers**
  - **Technical definition:** Within ECS or Fargate, capacity providers is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **container instances**
  - **Technical definition:** Within ECS or Fargate, container instances is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **service deployments**
  - **Technical definition:** Within ECS or Fargate, service deployments is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **task sets**
  - **Technical definition:** Within ECS or Fargate, task sets is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Amazon ECS tasks on AWS Fargate**
  - **Technical definition:** Within ECS or Fargate, Amazon ECS tasks on AWS Fargate is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Amazon ECS services on AWS Fargate**
  - **Technical definition:** Within ECS or Fargate, Amazon ECS services on AWS Fargate is a container-orchestration construct that determines task placement, service availability, runtime identity, or scaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### Amazon Elastic Kubernetes Service (Amazon EKS) and Karpenter

- **Amazon Elastic Kubernetes Service (Amazon EKS)**
  - **Technical definition:** Amazon EKS is AWS's managed Kubernetes control plane service for running upstream Kubernetes clusters integrated with AWS identity, networking, and storage.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Karpenter**
  - **Technical definition:** Karpenter is a Kubernetes node provisioning controller commonly used with Amazon EKS to create right-sized EC2 capacity directly from pending pod requirements.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **clusters**
  - **Technical definition:** Within EKS or Karpenter, clusters is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **control planes**
  - **Technical definition:** Within EKS or Karpenter, control planes is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **managed node groups**
  - **Technical definition:** Within EKS or Karpenter, managed node groups is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **self-managed nodes**
  - **Technical definition:** Within EKS or Karpenter, self-managed nodes is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Fargate profiles**
  - **Technical definition:** Within EKS or Karpenter, Fargate profiles is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Amazon EKS pods on AWS Fargate**
  - **Technical definition:** Within EKS or Karpenter, Amazon EKS pods on AWS Fargate is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **add-ons**
  - **Technical definition:** Within EKS or Karpenter, add-ons is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **access entries**
  - **Technical definition:** Within EKS or Karpenter, access entries is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Kubernetes service accounts**
  - **Technical definition:** Within EKS or Karpenter, Kubernetes service accounts is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **pod identity associations**
  - **Technical definition:** Within EKS or Karpenter, pod identity associations is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Karpenter NodePool resources**
  - **Technical definition:** Within EKS or Karpenter, Karpenter NodePool resources is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **EC2NodeClass resources**
  - **Technical definition:** Within EKS or Karpenter, EC2NodeClass resources is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **NodeClaim resources**
  - **Technical definition:** Within EKS or Karpenter, NodeClaim resources is a Kubernetes or node-provisioning entity that influences cluster behavior, pod scheduling, or AWS capacity allocation.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### AWS Lambda

- **AWS Lambda**
  - **Technical definition:** AWS Lambda is AWS's event-driven serverless compute runtime that scales function execution automatically and bills on invocation and execution duration.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **functions**
  - **Technical definition:** Within AWS Lambda, functions is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **versions**
  - **Technical definition:** Within AWS Lambda, versions is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **aliases**
  - **Technical definition:** Within AWS Lambda, aliases is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **layers**
  - **Technical definition:** Within AWS Lambda, layers is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **event source mappings**
  - **Technical definition:** Within AWS Lambda, event source mappings is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **function URLs**
  - **Technical definition:** Within AWS Lambda, function URLs is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **execution roles**
  - **Technical definition:** Within AWS Lambda, execution roles is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **provisioned concurrency**
  - **Technical definition:** Within AWS Lambda, provisioned concurrency is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **reserved concurrency**
  - **Technical definition:** Within AWS Lambda, reserved concurrency is a serverless runtime, packaging, scaling, or invocation-control construct that shapes function execution behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### AWS App Runner, AWS Elastic Beanstalk, and Amazon Lightsail

- **AWS App Runner**
  - **Technical definition:** AWS App Runner is AWS's managed application hosting service for container or source-based web services with simplified deployment and autoscaling.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AWS Elastic Beanstalk**
  - **Technical definition:** AWS Elastic Beanstalk is AWS's application deployment orchestration service that provisions and manages underlying infrastructure for supported application stacks.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **and Amazon Lightsail**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, and Amazon Lightsail is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **App Runner services**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, App Runner services is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **source configurations**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, source configurations is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **VPC connectors**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, VPC connectors is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **custom domains**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, custom domains is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Elastic Beanstalk applications**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, Elastic Beanstalk applications is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **application versions**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, application versions is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **environments**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, environments is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **platforms**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, platforms is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **deployment policies**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, deployment policies is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Lightsail instances**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, Lightsail instances is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **blueprints**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, blueprints is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **bundles**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, bundles is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **static IPs**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, static IPs is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **snapshots**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, snapshots is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **container services**
  - **Technical definition:** Within App Runner, Elastic Beanstalk, or Lightsail, container services is a higher-level hosting entity that defines application packaging, environment behavior, or managed platform exposure.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### AWS Batch

- **AWS Batch**
  - **Technical definition:** AWS Batch is AWS's batch-workload scheduler and compute orchestrator for queued, containerized, or EC2-backed jobs.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **jobs**
  - **Technical definition:** Within AWS Batch, jobs is a queueing, scheduling, compute, or job-execution object that affects batch workload throughput and placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **job queues**
  - **Technical definition:** Within AWS Batch, job queues is a queueing, scheduling, compute, or job-execution object that affects batch workload throughput and placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **job definitions**
  - **Technical definition:** Within AWS Batch, job definitions is a queueing, scheduling, compute, or job-execution object that affects batch workload throughput and placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **compute environments**
  - **Technical definition:** Within AWS Batch, compute environments is a queueing, scheduling, compute, or job-execution object that affects batch workload throughput and placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **scheduling policies**
  - **Technical definition:** Within AWS Batch, scheduling policies is a queueing, scheduling, compute, or job-execution object that affects batch workload throughput and placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **array jobs**
  - **Technical definition:** Within AWS Batch, array jobs is a queueing, scheduling, compute, or job-execution object that affects batch workload throughput and placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **multi-node parallel jobs**
  - **Technical definition:** Within AWS Batch, multi-node parallel jobs is a queueing, scheduling, compute, or job-execution object that affects batch workload throughput and placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **fair share identifiers**
  - **Technical definition:** Within AWS Batch, fair share identifiers is a queueing, scheduling, compute, or job-execution object that affects batch workload throughput and placement.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### AWS CodeBuild, AWS CodePipeline, AWS CodeDeploy, and AWS CodeArtifact

- **AWS CodeBuild**
  - **Technical definition:** AWS CodeBuild is AWS's managed build-execution service for compiling, testing, packaging, and generating artifacts in isolated build environments.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AWS CodePipeline**
  - **Technical definition:** AWS CodePipeline is AWS's release orchestration service for stage-based software delivery pipelines with integrated source, build, test, and deploy actions.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AWS CodeDeploy**
  - **Technical definition:** AWS CodeDeploy is AWS's deployment automation service for rolling software out to compute targets such as EC2, Lambda, and on-prem instances.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **and AWS CodeArtifact**
  - **Technical definition:** Within AWS developer tools, and AWS CodeArtifact is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **build projects**
  - **Technical definition:** Within AWS developer tools, build projects is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **buildspec files**
  - **Technical definition:** Within AWS developer tools, buildspec files is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **reports**
  - **Technical definition:** Within AWS developer tools, reports is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **caches**
  - **Technical definition:** Within AWS developer tools, caches is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **pipelines**
  - **Technical definition:** Within AWS developer tools, pipelines is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **stages**
  - **Technical definition:** Within AWS developer tools, stages is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **actions**
  - **Technical definition:** Within AWS developer tools, actions is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **source actions**
  - **Technical definition:** Within AWS developer tools, source actions is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AWS CodeConnections connections (CodeStar connections)**
  - **Technical definition:** Within AWS developer tools, AWS CodeConnections connections (CodeStar connections) is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **applications**
  - **Technical definition:** Within AWS developer tools, applications is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **deployment groups**
  - **Technical definition:** Within AWS developer tools, deployment groups is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **deployment configurations**
  - **Technical definition:** Within AWS developer tools, deployment configurations is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AppSpec files**
  - **Technical definition:** Within AWS developer tools, AppSpec files is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **domains**
  - **Technical definition:** Within AWS developer tools, domains is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **repositories**
  - **Technical definition:** Within AWS developer tools, repositories is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **packages**
  - **Technical definition:** Within AWS developer tools, packages is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **package versions**
  - **Technical definition:** Within AWS developer tools, package versions is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **external connections**
  - **Technical definition:** Within AWS developer tools, external connections is a build, package, release, or deployment entity that shapes CI/CD flow and artifact provenance.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### AWS CloudFormation, AWS Cloud Development Kit (AWS CDK), and AWS Proton

- **AWS CloudFormation**
  - **Technical definition:** AWS CloudFormation is AWS's declarative infrastructure-as-code service for provisioning and managing resources as versioned stacks.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AWS Cloud Development Kit (AWS CDK)**
  - **Technical definition:** AWS CDK is AWS's infrastructure-as-code framework that synthesizes higher-level programming language constructs into CloudFormation templates.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **and AWS Proton**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, and AWS Proton is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **stacks**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, stacks is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **stack sets**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, stack sets is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **templates**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, templates is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **change sets**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, change sets is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **nested stacks**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, nested stacks is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AWS SAM templates**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, AWS SAM templates is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **AWS::Serverless resources (serverless resources)**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, AWS::Serverless resources (serverless resources) is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **CDK apps**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, CDK apps is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **constructs**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, constructs is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **assets**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, assets is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **synthesized CloudFormation templates**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, synthesized CloudFormation templates is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **Proton environment templates**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, Proton environment templates is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **service templates**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, service templates is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **environments**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, environments is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **services**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, services is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **service instances**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, service instances is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.
- **components**
  - **Technical definition:** Within CloudFormation, CDK, or Proton, components is an infrastructure or platform-delivery construct that defines desired state, packaging, or service-standardization behavior.
  - **Concrete production example:** Example: during a production release, this helps the team scale compute, deploy a new version, validate health, and preserve rollback capability if the rollout regresses.

### Amazon SageMaker AI (Amazon SageMaker)

- **Amazon SageMaker AI (Amazon SageMaker)**
  - **Technical definition:** Amazon SageMaker is AWS's managed machine learning platform for data preparation, training, model registry, deployment, and monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **domains (Studio domains)**
  - **Technical definition:** Within Amazon SageMaker, domains (Studio domains) is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **user profiles**
  - **Technical definition:** Within Amazon SageMaker, user profiles is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **spaces**
  - **Technical definition:** Within Amazon SageMaker, spaces is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **training jobs**
  - **Technical definition:** Within Amazon SageMaker, training jobs is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **processing jobs**
  - **Technical definition:** Within Amazon SageMaker, processing jobs is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **feature groups**
  - **Technical definition:** Within Amazon SageMaker, feature groups is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **data flows**
  - **Technical definition:** Within Amazon SageMaker, data flows is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **models**
  - **Technical definition:** Within Amazon SageMaker, models is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint configurations**
  - **Technical definition:** Within Amazon SageMaker, endpoint configurations is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoints**
  - **Technical definition:** Within Amazon SageMaker, endpoints is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **pipelines**
  - **Technical definition:** Within Amazon SageMaker, pipelines is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **model package groups**
  - **Technical definition:** Within Amazon SageMaker, model package groups is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **model packages**
  - **Technical definition:** Within Amazon SageMaker, model packages is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **monitoring schedules**
  - **Technical definition:** Within Amazon SageMaker, monitoring schedules is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **baselines**
  - **Technical definition:** Within Amazon SageMaker, baselines is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **constraints**
  - **Technical definition:** Within Amazon SageMaker, constraints is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **experiments**
  - **Technical definition:** Within Amazon SageMaker, experiments is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **trials**
  - **Technical definition:** Within Amazon SageMaker, trials is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **trial components**
  - **Technical definition:** Within Amazon SageMaker, trial components is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage artifacts**
  - **Technical definition:** Within Amazon SageMaker, lineage artifacts is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage actions**
  - **Technical definition:** Within Amazon SageMaker, lineage actions is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage contexts**
  - **Technical definition:** Within Amazon SageMaker, lineage contexts is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Amazon Bedrock

- **Amazon Bedrock**
  - **Technical definition:** Amazon Bedrock is AWS's managed generative AI platform for invoking foundation models and building controlled AI applications without directly managing model infrastructure.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **foundation models**
  - **Technical definition:** Within Amazon Bedrock, foundation models is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **inference profiles**
  - **Technical definition:** Within Amazon Bedrock, inference profiles is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **provisioned throughput**
  - **Technical definition:** Within Amazon Bedrock, provisioned throughput is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **agents**
  - **Technical definition:** Within Amazon Bedrock, agents is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **knowledge bases**
  - **Technical definition:** Within Amazon Bedrock, knowledge bases is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrails**
  - **Technical definition:** Within Amazon Bedrock, guardrails is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **model customization jobs**
  - **Technical definition:** Within Amazon Bedrock, model customization jobs is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **model evaluation jobs**
  - **Technical definition:** Within Amazon Bedrock, model evaluation jobs is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **invocation logs**
  - **Technical definition:** Within Amazon Bedrock, invocation logs is a generative-AI invocation, guardrail, knowledge, or model-management entity exposed through AWS-managed foundation-model workflows.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Controls

### Operational And Control Points

- **EC2 instance metadata options**
  - **Technical definition:** EC2 instance metadata options is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM instance profiles**
  - **Technical definition:** IAM instance profiles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **security group rules**
  - **Technical definition:** security group rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **termination protection**
  - **Technical definition:** termination protection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Auto Scaling desired/min/max capacity**
  - **Technical definition:** Auto Scaling desired/min/max capacity is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **health check type**
  - **Technical definition:** health check type is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **termination policies**
  - **Technical definition:** termination policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **mixed instances policy**
  - **Technical definition:** mixed instances policy is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **instance refresh preferences**
  - **Technical definition:** instance refresh preferences is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Amazon ECR repository policies**
  - **Technical definition:** Amazon ECR repository policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **image scanning**
  - **Technical definition:** image scanning is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **tag immutability**
  - **Technical definition:** tag immutability is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region replication**
  - **Technical definition:** cross-Region replication is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Amazon ECS task IAM roles**
  - **Technical definition:** Amazon ECS task IAM roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **execution roles**
  - **Technical definition:** execution roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **deployment configuration**
  - **Technical definition:** deployment configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **service discovery**
  - **Technical definition:** service discovery is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **load balancer integration**
  - **Technical definition:** load balancer integration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **capacity provider strategy**
  - **Technical definition:** capacity provider strategy is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Fargate task sizing**
  - **Technical definition:** Fargate task sizing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Amazon EKS endpoint access**
  - **Technical definition:** Amazon EKS endpoint access is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cluster access management**
  - **Technical definition:** cluster access management is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **managed add-ons**
  - **Technical definition:** managed add-ons is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles for service accounts (IRSA)**
  - **Technical definition:** IAM roles for service accounts (IRSA) is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **EKS Pod Identity**
  - **Technical definition:** EKS Pod Identity is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **security groups for pods**
  - **Technical definition:** security groups for pods is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **upgrade policies**
  - **Technical definition:** upgrade policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Karpenter disruption controls**
  - **Technical definition:** Karpenter disruption controls is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Fargate profile placement**
  - **Technical definition:** Fargate profile placement is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Lambda execution roles**
  - **Technical definition:** AWS Lambda execution roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **environment variables**
  - **Technical definition:** environment variables is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **concurrency limits**
  - **Technical definition:** concurrency limits is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **code signing**
  - **Technical definition:** code signing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **App Runner deployment settings**
  - **Technical definition:** App Runner deployment settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Elastic Beanstalk platform versions**
  - **Technical definition:** Elastic Beanstalk platform versions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Lightsail firewall rules**
  - **Technical definition:** Lightsail firewall rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC egress**
  - **Technical definition:** VPC egress is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **access roles**
  - **Technical definition:** access roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **instance profiles**
  - **Technical definition:** instance profiles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **snapshots**
  - **Technical definition:** snapshots is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Batch compute environment type**
  - **Technical definition:** AWS Batch compute environment type is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **allocation strategy**
  - **Technical definition:** allocation strategy is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **job priority**
  - **Technical definition:** job priority is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **retry strategy**
  - **Technical definition:** retry strategy is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **timeout settings**
  - **Technical definition:** timeout settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Developer tool service roles**
  - **Technical definition:** Developer tool service roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **environment images**
  - **Technical definition:** environment images is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **artifact encryption**
  - **Technical definition:** artifact encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **approval actions**
  - **Technical definition:** approval actions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **stage transitions**
  - **Technical definition:** stage transitions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account actions**
  - **Technical definition:** cross-account actions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **automatic rollback**
  - **Technical definition:** automatic rollback is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **alarms**
  - **Technical definition:** alarms is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **repository policies**
  - **Technical definition:** repository policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **package origin controls**
  - **Technical definition:** package origin controls is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **CloudFormation stack policies**
  - **Technical definition:** CloudFormation stack policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **rollback triggers**
  - **Technical definition:** rollback triggers is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **termination protection**
  - **Technical definition:** termination protection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **capabilities**
  - **Technical definition:** capabilities is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **hooks**
  - **Technical definition:** hooks is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **service roles**
  - **Technical definition:** service roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS SAM deployment preferences**
  - **Technical definition:** AWS SAM deployment preferences is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **CDK bootstrap stacks**
  - **Technical definition:** CDK bootstrap stacks is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **synthesizers**
  - **Technical definition:** synthesizers is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Proton template versions**
  - **Technical definition:** Proton template versions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **deployment workflows**
  - **Technical definition:** deployment workflows is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **SageMaker execution roles**
  - **Technical definition:** SageMaker execution roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **network isolation**
  - **Technical definition:** network isolation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **model approval status**
  - **Technical definition:** model approval status is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint deployment configuration**
  - **Technical definition:** endpoint deployment configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **data capture**
  - **Technical definition:** data capture is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **bias metrics**
  - **Technical definition:** bias metrics is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage metadata**
  - **Technical definition:** lineage metadata is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **artifact encryption**
  - **Technical definition:** artifact encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **access permissions**
  - **Technical definition:** access permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Bedrock model access**
  - **Technical definition:** Bedrock model access is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrails**
  - **Technical definition:** guardrails is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM permissions**
  - **Technical definition:** IAM permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC endpoints**
  - **Technical definition:** VPC endpoints is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **invocation logging**
  - **Technical definition:** invocation logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Operational And Control Points

- **Build and roll back golden AMIs**
  - **Technical definition:** Build and roll back golden AMIs is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review EC2 status checks**
  - **Technical definition:** review EC2 status checks is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **replace unhealthy instances**
  - **Technical definition:** replace unhealthy instances is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate Auto Scaling instance refreshes**
  - **Technical definition:** validate Auto Scaling instance refreshes is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Push and promote container images in Amazon ECR while reviewing image scan findings and lifecycle cleanup**
  - **Technical definition:** Push and promote container images in Amazon ECR while reviewing image scan findings and lifecycle cleanup is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Deploy ECS services with rolling or blue/green patterns and troubleshoot failed tasks**
  - **Technical definition:** Deploy ECS services with rolling or blue/green patterns and troubleshoot failed tasks is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **capacity providers**
  - **Technical definition:** capacity providers is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **target registration**
  - **Technical definition:** target registration is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Upgrade EKS clusters**
  - **Technical definition:** Upgrade EKS clusters is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **managed node groups**
  - **Technical definition:** managed node groups is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **add-ons**
  - **Technical definition:** add-ons is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review pod scheduling**
  - **Technical definition:** review pod scheduling is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IRSA**
  - **Technical definition:** IRSA is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **EKS Pod Identity**
  - **Technical definition:** EKS Pod Identity is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Karpenter scale-out behavior**
  - **Technical definition:** Karpenter scale-out behavior is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Deploy Lambda versions and aliases**
  - **Technical definition:** Deploy Lambda versions and aliases is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **investigate cold starts**
  - **Technical definition:** investigate cold starts is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **retries**
  - **Technical definition:** retries is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **event source mapping failures**
  - **Technical definition:** event source mapping failures is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Use CodeBuild**
  - **Technical definition:** Use CodeBuild is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **CodePipeline**
  - **Technical definition:** CodePipeline is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **CodeDeploy**
  - **Technical definition:** CodeDeploy is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **CodeArtifact for build**
  - **Technical definition:** CodeArtifact for build is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **approval**
  - **Technical definition:** approval is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **deployment**
  - **Technical definition:** deployment is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **rollback**
  - **Technical definition:** rollback is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **artifact promotion workflows**
  - **Technical definition:** artifact promotion workflows is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Review CloudFormation change sets**
  - **Technical definition:** Review CloudFormation change sets is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **drift**
  - **Technical definition:** drift is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **stack rollback**
  - **Technical definition:** stack rollback is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **CDK diffs**
  - **Technical definition:** CDK diffs is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **SAM serverless deployment**
  - **Technical definition:** SAM serverless deployment is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Proton template rollout**
  - **Technical definition:** Proton template rollout is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Run SageMaker training**
  - **Technical definition:** Run SageMaker training is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **processing**
  - **Technical definition:** processing is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **pipelines**
  - **Technical definition:** pipelines is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **model registration**
  - **Technical definition:** model registration is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint deployment**
  - **Technical definition:** endpoint deployment is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **monitoring review**
  - **Technical definition:** monitoring review is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **inference troubleshooting**
  - **Technical definition:** inference troubleshooting is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Invoke Bedrock models**
  - **Technical definition:** Invoke Bedrock models is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **test prompts**
  - **Technical definition:** test prompts is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate guardrails**
  - **Technical definition:** validate guardrails is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **synchronize knowledge bases**
  - **Technical definition:** synchronize knowledge bases is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **deploy agents**
  - **Technical definition:** deploy agents is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review model evaluations**
  - **Technical definition:** review model evaluations is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 4: Data, Storage, Backup, And Stateful Reliability

## Service And Entity Coverage

### Amazon Simple Storage Service (Amazon S3)

- **Amazon Simple Storage Service (Amazon S3)**
  - **Technical definition:** Amazon S3 is AWS's object storage service for durable, massively scalable object persistence, lifecycle control, eventing, and policy-based access.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **buckets**
  - **Technical definition:** Within Amazon S3, buckets is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **objects**
  - **Technical definition:** Within Amazon S3, objects is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **prefixes**
  - **Technical definition:** Within Amazon S3, prefixes is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **access points**
  - **Technical definition:** Within Amazon S3, access points is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Multi-Region Access Points**
  - **Technical definition:** Within Amazon S3, Multi-Region Access Points is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **bucket policies**
  - **Technical definition:** Within Amazon S3, bucket policies is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **S3 Block Public Access settings**
  - **Technical definition:** Within Amazon S3, S3 Block Public Access settings is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **S3 Object Lock configurations**
  - **Technical definition:** Within Amazon S3, S3 Object Lock configurations is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **lifecycle configurations**
  - **Technical definition:** Within Amazon S3, lifecycle configurations is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication rules**
  - **Technical definition:** Within Amazon S3, replication rules is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **storage classes**
  - **Technical definition:** Within Amazon S3, storage classes is an object-storage namespace, policy, lifecycle, replication, or eventing construct that shapes durability, access, and data movement.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### Amazon Elastic Block Store (Amazon EBS) and Amazon Elastic File System (Amazon EFS)

- **Amazon Elastic Block Store (Amazon EBS)**
  - **Technical definition:** Amazon EBS is AWS's block-storage service for EC2-attached persistent volumes with provisioned performance and snapshot-based backup.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon Elastic File System (Amazon EFS)**
  - **Technical definition:** Amazon EFS is AWS's elastic, managed NFS file service for shared POSIX access across multiple instances and Availability Zones.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **EBS volumes**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, EBS volumes is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **snapshots**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, snapshots is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **snapshot archives**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, snapshot archives is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **volume types**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, volume types is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **provisioned IOPS**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, provisioned IOPS is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **throughput settings**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, throughput settings is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Fast Snapshot Restore**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, Fast Snapshot Restore is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **EFS file systems**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, EFS file systems is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **mount targets**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, mount targets is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **access points**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, access points is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **lifecycle policies**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, lifecycle policies is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **throughput modes**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, throughput modes is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **performance modes**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, performance modes is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backups**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, backups is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication configurations**
  - **Technical definition:** Within Amazon EBS or Amazon EFS, replication configurations is a persistent-storage entity that determines attachment, performance, snapshot, lifecycle, or shared-access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### Amazon FSx

- **Amazon FSx**
  - **Technical definition:** Amazon FSx is AWS's family of managed high-performance and specialized file systems for Windows, Lustre, NetApp ONTAP, and OpenZFS use cases.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **file systems**
  - **Technical definition:** Within Amazon FSx, file systems is a file-system, volume, throughput, backup, or access construct specific to AWS-managed specialized file storage.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backups**
  - **Technical definition:** Within Amazon FSx, backups is a file-system, volume, throughput, backup, or access construct specific to AWS-managed specialized file storage.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **storage virtual machines**
  - **Technical definition:** Within Amazon FSx, storage virtual machines is a file-system, volume, throughput, backup, or access construct specific to AWS-managed specialized file storage.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **volumes**
  - **Technical definition:** Within Amazon FSx, volumes is a file-system, volume, throughput, backup, or access construct specific to AWS-managed specialized file storage.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **shares**
  - **Technical definition:** Within Amazon FSx, shares is a file-system, volume, throughput, backup, or access construct specific to AWS-managed specialized file storage.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **endpoints**
  - **Technical definition:** Within Amazon FSx, endpoints is a file-system, volume, throughput, backup, or access construct specific to AWS-managed specialized file storage.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **deployment types**
  - **Technical definition:** Within Amazon FSx, deployment types is a file-system, volume, throughput, backup, or access construct specific to AWS-managed specialized file storage.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **throughput capacity**
  - **Technical definition:** Within Amazon FSx, throughput capacity is a file-system, volume, throughput, backup, or access construct specific to AWS-managed specialized file storage.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### Amazon Relational Database Service (Amazon RDS) and Amazon Aurora

- **Amazon Relational Database Service (Amazon RDS)**
  - **Technical definition:** Amazon RDS is AWS's managed relational database service for common engines with integrated backup, maintenance, and failover features.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon Aurora**
  - **Technical definition:** Amazon Aurora is AWS's cloud-optimized relational database engine with distributed storage, fast failover behavior, and high read-scaling options.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **DB instances**
  - **Technical definition:** Within Amazon RDS or Aurora, DB instances is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **DB clusters**
  - **Technical definition:** Within Amazon RDS or Aurora, DB clusters is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **DB snapshots**
  - **Technical definition:** Within Amazon RDS or Aurora, DB snapshots is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **automated backups**
  - **Technical definition:** Within Amazon RDS or Aurora, automated backups is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **parameter groups**
  - **Technical definition:** Within Amazon RDS or Aurora, parameter groups is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **option groups**
  - **Technical definition:** Within Amazon RDS or Aurora, option groups is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **subnet groups**
  - **Technical definition:** Within Amazon RDS or Aurora, subnet groups is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **read replicas**
  - **Technical definition:** Within Amazon RDS or Aurora, read replicas is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **reserved DB instances**
  - **Technical definition:** Within Amazon RDS or Aurora, reserved DB instances is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Aurora Replicas**
  - **Technical definition:** Within Amazon RDS or Aurora, Aurora Replicas is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **cluster endpoints**
  - **Technical definition:** Within Amazon RDS or Aurora, cluster endpoints is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **reader endpoints**
  - **Technical definition:** Within Amazon RDS or Aurora, reader endpoints is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **custom endpoints**
  - **Technical definition:** Within Amazon RDS or Aurora, custom endpoints is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **global databases**
  - **Technical definition:** Within Amazon RDS or Aurora, global databases is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backtrack windows**
  - **Technical definition:** Within Amazon RDS or Aurora, backtrack windows is a database lifecycle, availability, storage, replication, or maintenance construct that determines managed relational behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### Amazon DynamoDB

- **Amazon DynamoDB**
  - **Technical definition:** Amazon DynamoDB is AWS's serverless NoSQL database for key-value and document workloads with partition-based scale and configurable consistency behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **tables**
  - **Technical definition:** Within DynamoDB, tables is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **items**
  - **Technical definition:** Within DynamoDB, items is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **attributes**
  - **Technical definition:** Within DynamoDB, attributes is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **partition keys**
  - **Technical definition:** Within DynamoDB, partition keys is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **sort keys**
  - **Technical definition:** Within DynamoDB, sort keys is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **local secondary indexes**
  - **Technical definition:** Within DynamoDB, local secondary indexes is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **global secondary indexes**
  - **Technical definition:** Within DynamoDB, global secondary indexes is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **streams**
  - **Technical definition:** Within DynamoDB, streams is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **global tables**
  - **Technical definition:** Within DynamoDB, global tables is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backups**
  - **Technical definition:** Within DynamoDB, backups is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **time to live (TTL)**
  - **Technical definition:** Within DynamoDB, time to live (TTL) is a table, indexing, throughput, partitioning, or replication construct that defines NoSQL scale and access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### Amazon DocumentDB, Amazon Neptune, Amazon Timestream for LiveAnalytics, Amazon ElastiCache, and Amazon MemoryDB

- **Amazon DocumentDB**
  - **Technical definition:** Amazon DocumentDB is AWS's managed document database service with MongoDB compatibility-oriented interfaces and managed infrastructure behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon Neptune**
  - **Technical definition:** Amazon Neptune is AWS's managed graph database service for property-graph and RDF workloads.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon Timestream for LiveAnalytics**
  - **Technical definition:** Amazon Timestream for LiveAnalytics is AWS's managed time-series database optimized for high-ingest, time-window, and operational telemetry queries.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon ElastiCache**
  - **Technical definition:** Amazon ElastiCache is AWS's managed in-memory caching service for Redis or Memcached-compatible low-latency data access.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **and Amazon MemoryDB**
  - **Technical definition:** Within AWS specialty data services, and Amazon MemoryDB is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **DocumentDB clusters**
  - **Technical definition:** Within AWS specialty data services, DocumentDB clusters is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Neptune DB clusters**
  - **Technical definition:** Within AWS specialty data services, Neptune DB clusters is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Timestream databases and tables**
  - **Technical definition:** Within AWS specialty data services, Timestream databases and tables is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **ElastiCache serverless caches**
  - **Technical definition:** Within AWS specialty data services, ElastiCache serverless caches is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication groups**
  - **Technical definition:** Within AWS specialty data services, replication groups is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **shards**
  - **Technical definition:** Within AWS specialty data services, shards is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **users**
  - **Technical definition:** Within AWS specialty data services, users is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **user groups**
  - **Technical definition:** Within AWS specialty data services, user groups is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **MemoryDB clusters**
  - **Technical definition:** Within AWS specialty data services, MemoryDB clusters is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **access control lists (ACLs)**
  - **Technical definition:** Within AWS specialty data services, access control lists (ACLs) is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **snapshots**
  - **Technical definition:** Within AWS specialty data services, snapshots is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **endpoints**
  - **Technical definition:** Within AWS specialty data services, endpoints is a data-model, performance, or operational construct specific to the managed database family in scope.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### Amazon OpenSearch Service, Amazon Redshift, and Amazon Athena

- **Amazon OpenSearch Service**
  - **Technical definition:** Amazon OpenSearch Service is AWS's managed search and analytics service for OpenSearch-compatible indexing, search, and log-analysis workloads.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon Redshift**
  - **Technical definition:** Amazon Redshift is AWS's managed analytical data warehouse for large-scale SQL analytics, columnar storage, and distributed query execution.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **and Amazon Athena**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, and Amazon Athena is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **OpenSearch domains**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, OpenSearch domains is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **indexes**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, indexes is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **data nodes**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, data nodes is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **cluster manager nodes**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, cluster manager nodes is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **OpenSearch Dashboards**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, OpenSearch Dashboards is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Redshift provisioned clusters**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, Redshift provisioned clusters is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **serverless namespaces**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, serverless namespaces is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **serverless workgroups**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, serverless workgroups is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **databases**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, databases is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **schemas**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, schemas is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **tables**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, tables is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **materialized views**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, materialized views is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Athena workgroups**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, Athena workgroups is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **queries**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, queries is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **data catalogs**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, data catalogs is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **partitions**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, partitions is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **query result locations**
  - **Technical definition:** Within OpenSearch, Redshift, or Athena, query result locations is an indexing, warehouse, query, or cluster-level construct that determines analytical execution and data access behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### AWS Glue and AWS Lake Formation

- **AWS Glue**
  - **Technical definition:** AWS Glue is AWS's managed data integration and ETL platform for crawlers, jobs, and metadata-driven transformation pipelines.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **AWS Lake Formation**
  - **Technical definition:** AWS Lake Formation is AWS's governance layer for centrally controlling data lake permissions, sharing, and metadata access over AWS analytics services.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Data Catalog databases**
  - **Technical definition:** Within AWS Glue or Lake Formation, Data Catalog databases is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Data Catalog tables**
  - **Technical definition:** Within AWS Glue or Lake Formation, Data Catalog tables is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **crawlers**
  - **Technical definition:** Within AWS Glue or Lake Formation, crawlers is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **jobs**
  - **Technical definition:** Within AWS Glue or Lake Formation, jobs is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **triggers**
  - **Technical definition:** Within AWS Glue or Lake Formation, triggers is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **workflows**
  - **Technical definition:** Within AWS Glue or Lake Formation, workflows is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **connections**
  - **Technical definition:** Within AWS Glue or Lake Formation, connections is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **schemas**
  - **Technical definition:** Within AWS Glue or Lake Formation, schemas is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **classifiers**
  - **Technical definition:** Within AWS Glue or Lake Formation, classifiers is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **data quality rulesets**
  - **Technical definition:** Within AWS Glue or Lake Formation, data quality rulesets is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **AWS Glue DataBrew projects**
  - **Technical definition:** Within AWS Glue or Lake Formation, AWS Glue DataBrew projects is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **datasets**
  - **Technical definition:** Within AWS Glue or Lake Formation, datasets is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **recipes**
  - **Technical definition:** Within AWS Glue or Lake Formation, recipes is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **profile jobs**
  - **Technical definition:** Within AWS Glue or Lake Formation, profile jobs is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Lake Formation data lake administrators**
  - **Technical definition:** Within AWS Glue or Lake Formation, Lake Formation data lake administrators is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **registered locations**
  - **Technical definition:** Within AWS Glue or Lake Formation, registered locations is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **LF-Tags**
  - **Technical definition:** Within AWS Glue or Lake Formation, LF-Tags is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **permissions**
  - **Technical definition:** Within AWS Glue or Lake Formation, permissions is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **data filters**
  - **Technical definition:** Within AWS Glue or Lake Formation, data filters is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **resource links**
  - **Technical definition:** Within AWS Glue or Lake Formation, resource links is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **blueprints**
  - **Technical definition:** Within AWS Glue or Lake Formation, blueprints is a metadata, ETL, permission, or governance construct that shapes data-lake processing and access control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### Amazon Kinesis Data Streams, Amazon Data Firehose, Amazon Managed Streaming for Apache Kafka (Amazon MSK), Amazon Simple Queue Service (Amazon SQS), and Amazon Simple Notification Service (Amazon SNS)

- **Amazon Kinesis Data Streams**
  - **Technical definition:** Amazon Kinesis Data Streams is AWS's ordered streaming data service for producer-consumer event pipelines with shard-based throughput control.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon Data Firehose**
  - **Technical definition:** Amazon Data Firehose is AWS's managed delivery service for buffering, transforming, and landing streaming data into storage and analytics destinations.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon Managed Streaming for Apache Kafka (Amazon MSK)**
  - **Technical definition:** Amazon MSK is AWS's managed Apache Kafka service for running Kafka clusters with AWS-integrated networking, monitoring, and operations.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Amazon Simple Queue Service (Amazon SQS)**
  - **Technical definition:** Amazon SQS is AWS's managed message-queue service for asynchronous decoupling, buffering, and worker consumption patterns.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **and Amazon Simple Notification Service (Amazon SNS)**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, and Amazon Simple Notification Service (Amazon SNS) is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Kinesis data streams**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, Kinesis data streams is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **shards**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, shards is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **consumers**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, consumers is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **enhanced fan-out consumers**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, enhanced fan-out consumers is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **Firehose streams**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, Firehose streams is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **sources**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, sources is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **destinations**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, destinations is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **MSK clusters**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, MSK clusters is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **brokers**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, brokers is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **topics**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, topics is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **partitions**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, partitions is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **consumer groups**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, consumer groups is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **connectors**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, connectors is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **SQS standard queues**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, SQS standard queues is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **FIFO queues**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, FIFO queues is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **dead-letter queues**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, dead-letter queues is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **SNS topics**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, SNS topics is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **subscriptions**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, subscriptions is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **filter policies**
  - **Technical definition:** Within Kinesis, Firehose, MSK, SQS, or SNS, filter policies is a streaming, queueing, retention, or delivery construct for asynchronous event-driven systems.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### Amazon EventBridge and AWS Step Functions

- **Amazon EventBridge**
  - **Technical definition:** Amazon EventBridge is AWS's event-routing bus for SaaS, AWS, and custom events with rule-based target delivery.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **AWS Step Functions**
  - **Technical definition:** AWS Step Functions is AWS's workflow orchestration service for stateful, multi-step application logic with retries, branching, and service integration.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **event buses**
  - **Technical definition:** Within EventBridge or Step Functions, event buses is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **rules**
  - **Technical definition:** Within EventBridge or Step Functions, rules is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **targets**
  - **Technical definition:** Within EventBridge or Step Functions, targets is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **event patterns**
  - **Technical definition:** Within EventBridge or Step Functions, event patterns is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **archives**
  - **Technical definition:** Within EventBridge or Step Functions, archives is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replays**
  - **Technical definition:** Within EventBridge or Step Functions, replays is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **schedules**
  - **Technical definition:** Within EventBridge or Step Functions, schedules is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **connections**
  - **Technical definition:** Within EventBridge or Step Functions, connections is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **API destinations**
  - **Technical definition:** Within EventBridge or Step Functions, API destinations is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **state machines**
  - **Technical definition:** Within EventBridge or Step Functions, state machines is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **states**
  - **Technical definition:** Within EventBridge or Step Functions, states is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **executions**
  - **Technical definition:** Within EventBridge or Step Functions, executions is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **activities**
  - **Technical definition:** Within EventBridge or Step Functions, activities is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **tasks**
  - **Technical definition:** Within EventBridge or Step Functions, tasks is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **versions**
  - **Technical definition:** Within EventBridge or Step Functions, versions is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **aliases**
  - **Technical definition:** Within EventBridge or Step Functions, aliases is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **distributed maps**
  - **Technical definition:** Within EventBridge or Step Functions, distributed maps is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **execution history**
  - **Technical definition:** Within EventBridge or Step Functions, execution history is an event-routing or workflow-state construct that controls orchestration, branching, and downstream invocation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### AWS Secrets Manager and AWS Key Management Service (AWS KMS)

- **AWS Secrets Manager**
  - **Technical definition:** AWS Secrets Manager is AWS's managed secrets storage and rotation service for credentials, API keys, and sensitive application values.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **AWS Key Management Service (AWS KMS)**
  - **Technical definition:** AWS KMS is AWS's centralized cryptographic key management service for encryption, decryption, signing, and key-policy governance.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **secrets**
  - **Technical definition:** Within Secrets Manager or AWS KMS, secrets is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **secret versions**
  - **Technical definition:** Within Secrets Manager or AWS KMS, secret versions is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **version stages**
  - **Technical definition:** Within Secrets Manager or AWS KMS, version stages is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **rotation schedules**
  - **Technical definition:** Within Secrets Manager or AWS KMS, rotation schedules is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **rotation Lambda functions**
  - **Technical definition:** Within Secrets Manager or AWS KMS, rotation Lambda functions is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **resource policies**
  - **Technical definition:** Within Secrets Manager or AWS KMS, resource policies is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication settings**
  - **Technical definition:** Within Secrets Manager or AWS KMS, replication settings is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **KMS keys**
  - **Technical definition:** Within Secrets Manager or AWS KMS, KMS keys is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **aliases**
  - **Technical definition:** Within Secrets Manager or AWS KMS, aliases is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **key policies**
  - **Technical definition:** Within Secrets Manager or AWS KMS, key policies is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **grants**
  - **Technical definition:** Within Secrets Manager or AWS KMS, grants is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **key material**
  - **Technical definition:** Within Secrets Manager or AWS KMS, key material is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **multi-Region keys**
  - **Technical definition:** Within Secrets Manager or AWS KMS, multi-Region keys is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **imported key material**
  - **Technical definition:** Within Secrets Manager or AWS KMS, imported key material is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **external key stores**
  - **Technical definition:** Within Secrets Manager or AWS KMS, external key stores is a secrets or cryptographic control-plane entity that governs secret retrieval, key usage, encryption, or rotation.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### AWS Backup and AWS Database Migration Service (AWS DMS)

- **AWS Backup**
  - **Technical definition:** AWS Backup is AWS's centralized backup policy and vaulting service for supported AWS resources across accounts and Regions.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **AWS Database Migration Service (AWS DMS)**
  - **Technical definition:** AWS DMS is AWS's managed data replication and migration service for moving and continuously replicating database state between heterogeneous or homogeneous engines.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backup plans**
  - **Technical definition:** Within AWS Backup or AWS DMS, backup plans is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backup rules**
  - **Technical definition:** Within AWS Backup or AWS DMS, backup rules is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backup selections**
  - **Technical definition:** Within AWS Backup or AWS DMS, backup selections is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **recovery points**
  - **Technical definition:** Within AWS Backup or AWS DMS, recovery points is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backup vaults**
  - **Technical definition:** Within AWS Backup or AWS DMS, backup vaults is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backup vault lock configurations**
  - **Technical definition:** Within AWS Backup or AWS DMS, backup vault lock configurations is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **copy jobs**
  - **Technical definition:** Within AWS Backup or AWS DMS, copy jobs is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **restore jobs**
  - **Technical definition:** Within AWS Backup or AWS DMS, restore jobs is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication instances**
  - **Technical definition:** Within AWS Backup or AWS DMS, replication instances is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication tasks**
  - **Technical definition:** Within AWS Backup or AWS DMS, replication tasks is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **endpoints**
  - **Technical definition:** Within AWS Backup or AWS DMS, endpoints is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication subnet groups**
  - **Technical definition:** Within AWS Backup or AWS DMS, replication subnet groups is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **event subscriptions**
  - **Technical definition:** Within AWS Backup or AWS DMS, event subscriptions is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **data providers**
  - **Technical definition:** Within AWS Backup or AWS DMS, data providers is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **migration projects**
  - **Technical definition:** Within AWS Backup or AWS DMS, migration projects is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

## Controls

### Operational And Control Points

- **S3 bucket policies**
  - **Technical definition:** S3 bucket policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Object Ownership**
  - **Technical definition:** Object Ownership is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **versioning**
  - **Technical definition:** versioning is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **default encryption**
  - **Technical definition:** default encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **S3 Object Lock retention modes**
  - **Technical definition:** S3 Object Lock retention modes is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **replication**
  - **Technical definition:** replication is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **access logging**
  - **Technical definition:** access logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **EBS volume encryption**
  - **Technical definition:** EBS volume encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **snapshot sharing**
  - **Technical definition:** snapshot sharing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **delete-on-termination settings**
  - **Technical definition:** delete-on-termination settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **KMS key selection**
  - **Technical definition:** KMS key selection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **EFS file system policies**
  - **Technical definition:** EFS file system policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **security groups**
  - **Technical definition:** security groups is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **access point POSIX identities**
  - **Technical definition:** access point POSIX identities is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle management**
  - **Technical definition:** lifecycle management is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup policies**
  - **Technical definition:** backup policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **RDS and Aurora Multi-AZ deployment**
  - **Technical definition:** RDS and Aurora Multi-AZ deployment is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup retention**
  - **Technical definition:** backup retention is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **storage autoscaling**
  - **Technical definition:** storage autoscaling is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM database authentication**
  - **Technical definition:** IAM database authentication is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **failover priority**
  - **Technical definition:** failover priority is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **parameter alignment**
  - **Technical definition:** parameter alignment is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **DynamoDB capacity modes**
  - **Technical definition:** DynamoDB capacity modes is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **auto scaling**
  - **Technical definition:** auto scaling is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **table classes**
  - **Technical definition:** table classes is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **point-in-time recovery (PITR)**
  - **Technical definition:** point-in-time recovery (PITR) is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Specialized database VPC placement**
  - **Technical definition:** Specialized database VPC placement is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **TLS**
  - **Technical definition:** TLS is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup retention**
  - **Technical definition:** backup retention is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **audit logging**
  - **Technical definition:** audit logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **retention configuration**
  - **Technical definition:** retention configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redis-compatible access control lists**
  - **Technical definition:** Redis-compatible access control lists is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Multi-AZ deployment**
  - **Technical definition:** Multi-AZ deployment is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **OpenSearch fine-grained access control**
  - **Technical definition:** OpenSearch fine-grained access control is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC access**
  - **Technical definition:** VPC access is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **index state management**
  - **Technical definition:** index state management is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redshift workload management (WLM)**
  - **Technical definition:** Redshift workload management (WLM) is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **data sharing**
  - **Technical definition:** data sharing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Athena workgroup settings**
  - **Technical definition:** Athena workgroup settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **bytes scanned limits**
  - **Technical definition:** bytes scanned limits is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **result location enforcement**
  - **Technical definition:** result location enforcement is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Glue job bookmarks**
  - **Technical definition:** Glue job bookmarks is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **worker type**
  - **Technical definition:** worker type is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **security configurations**
  - **Technical definition:** security configurations is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Data Catalog permissions**
  - **Technical definition:** Data Catalog permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Lake Formation permissions**
  - **Technical definition:** Lake Formation permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **LF-Tag-based access control**
  - **Technical definition:** LF-Tag-based access control is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **row-level filters**
  - **Technical definition:** row-level filters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **column-level filters**
  - **Technical definition:** column-level filters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account sharing**
  - **Technical definition:** cross-account sharing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Streaming and messaging retention**
  - **Technical definition:** Streaming and messaging retention is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **shard count**
  - **Technical definition:** shard count is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Firehose buffering**
  - **Technical definition:** Firehose buffering is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **broker storage**
  - **Technical definition:** broker storage is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Apache Kafka ACLs**
  - **Technical definition:** Apache Kafka ACLs is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **queue policies**
  - **Technical definition:** queue policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **visibility timeout**
  - **Technical definition:** visibility timeout is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **redrive policies**
  - **Technical definition:** redrive policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **topic policies**
  - **Technical definition:** topic policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **subscription filter policies**
  - **Technical definition:** subscription filter policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **delivery retry policies**
  - **Technical definition:** delivery retry policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **EventBridge event bus policies**
  - **Technical definition:** EventBridge event bus policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **input transformers**
  - **Technical definition:** input transformers is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **retry policies**
  - **Technical definition:** retry policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **schema discovery**
  - **Technical definition:** schema discovery is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Step Functions retry/catch handlers**
  - **Technical definition:** Step Functions retry/catch handlers is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **timeout settings**
  - **Technical definition:** timeout settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **logging**
  - **Technical definition:** logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **tracing**
  - **Technical definition:** tracing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Secrets Manager automatic rotation**
  - **Technical definition:** Secrets Manager automatic rotation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **version staging labels**
  - **Technical definition:** version staging labels is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **resource policies**
  - **Technical definition:** resource policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **KMS key policies**
  - **Technical definition:** KMS key policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **grants**
  - **Technical definition:** grants is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **rotation**
  - **Technical definition:** rotation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **deletion waiting periods**
  - **Technical definition:** deletion waiting periods is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **multi-Region replication**
  - **Technical definition:** multi-Region replication is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Backup schedules**
  - **Technical definition:** AWS Backup schedules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **vault policies**
  - **Technical definition:** vault policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Backup Vault Lock**
  - **Technical definition:** AWS Backup Vault Lock is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region copy**
  - **Technical definition:** cross-Region copy is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account copy**
  - **Technical definition:** cross-account copy is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **legal holds**
  - **Technical definition:** legal holds is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **DMS full-load settings**
  - **Technical definition:** DMS full-load settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **change data capture (CDC)**
  - **Technical definition:** change data capture (CDC) is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **table mappings**
  - **Technical definition:** table mappings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validation settings**
  - **Technical definition:** validation settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint permissions**
  - **Technical definition:** endpoint permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Operational And Control Points

- **Restore S3 objects**
  - **Technical definition:** Restore S3 objects is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate lifecycle policies**
  - **Technical definition:** validate lifecycle policies is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review replication**
  - **Technical definition:** review replication is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **check public access**
  - **Technical definition:** check public access is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **optimize storage classes**
  - **Technical definition:** optimize storage classes is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Attach and resize EBS volumes**
  - **Technical definition:** Attach and resize EBS volumes is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore snapshots**
  - **Technical definition:** restore snapshots is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate EFS mounts**
  - **Technical definition:** validate EFS mounts is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review throughput**
  - **Technical definition:** review throughput is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **test file-system recovery**
  - **Technical definition:** test file-system recovery is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Validate RDS/Aurora failover**
  - **Technical definition:** Validate RDS/Aurora failover is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **point-in-time recovery (PITR)**
  - **Technical definition:** point-in-time recovery (PITR) is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **replica promotion**
  - **Technical definition:** replica promotion is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **global database switchover**
  - **Technical definition:** global database switchover is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **engine upgrades**
  - **Technical definition:** engine upgrades is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **replica lag**
  - **Technical definition:** replica lag is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Troubleshoot DynamoDB hot partitions**
  - **Technical definition:** Troubleshoot DynamoDB hot partitions is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **secondary indexes**
  - **Technical definition:** secondary indexes is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **backup restore**
  - **Technical definition:** backup restore is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **streams**
  - **Technical definition:** streams is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **global table replication**
  - **Technical definition:** global table replication is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Review cache hit ratios**
  - **Technical definition:** Review cache hit ratios is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redis-compatible access**
  - **Technical definition:** Redis-compatible access is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **snapshot restore**
  - **Technical definition:** snapshot restore is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **eviction behavior**
  - **Technical definition:** eviction behavior is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **specialized database migration fit**
  - **Technical definition:** specialized database migration fit is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Tune OpenSearch shards**
  - **Technical definition:** Tune OpenSearch shards is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Redshift queries**
  - **Technical definition:** Redshift queries is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Athena partitions**
  - **Technical definition:** Athena partitions is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **data lake query failures**
  - **Technical definition:** data lake query failures is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Run Glue crawlers and ETL jobs**
  - **Technical definition:** Run Glue crawlers and ETL jobs is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate schemas**
  - **Technical definition:** validate schemas is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review data quality**
  - **Technical definition:** review data quality is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **grant Lake Formation permissions**
  - **Technical definition:** grant Lake Formation permissions is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **troubleshoot governed data sharing**
  - **Technical definition:** troubleshoot governed data sharing is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Monitor stream ingestion**
  - **Technical definition:** Monitor stream ingestion is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **shard scaling**
  - **Technical definition:** shard scaling is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **consumer lag**
  - **Technical definition:** consumer lag is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **MSK connectors**
  - **Technical definition:** MSK connectors is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **SQS dead-letter queue redrive**
  - **Technical definition:** SQS dead-letter queue redrive is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **SNS fanout**
  - **Technical definition:** SNS fanout is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Validate EventBridge routing and replay**
  - **Technical definition:** Validate EventBridge routing and replay is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **then troubleshoot failed Step Functions executions and orchestration rollback**
  - **Technical definition:** then troubleshoot failed Step Functions executions and orchestration rollback is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Rotate secrets**
  - **Technical definition:** Rotate secrets is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **roll back secret versions**
  - **Technical definition:** roll back secret versions is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review KMS grants**
  - **Technical definition:** review KMS grants is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate cross-account key access**
  - **Technical definition:** validate cross-account key access is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **test backups**
  - **Technical definition:** test backups is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore recovery points**
  - **Technical definition:** restore recovery points is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **execute DMS full-load plus CDC cutover**
  - **Technical definition:** execute DMS full-load plus CDC cutover is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 5: Observability, FinOps, And Operational Excellence

## Service And Entity Coverage

### Amazon CloudWatch and AWS X-Ray

- **Amazon CloudWatch**
  - **Technical definition:** Amazon CloudWatch is AWS's metrics, logs, alarms, dashboards, and operational event monitoring service for AWS and application telemetry.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **AWS X-Ray**
  - **Technical definition:** AWS X-Ray is AWS's distributed tracing service for request path visibility, latency analysis, and service-map generation.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **metrics**
  - **Technical definition:** Within CloudWatch or X-Ray, metrics is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **namespaces**
  - **Technical definition:** Within CloudWatch or X-Ray, namespaces is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **dimensions**
  - **Technical definition:** Within CloudWatch or X-Ray, dimensions is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **alarms**
  - **Technical definition:** Within CloudWatch or X-Ray, alarms is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **dashboards**
  - **Technical definition:** Within CloudWatch or X-Ray, dashboards is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **log groups**
  - **Technical definition:** Within CloudWatch or X-Ray, log groups is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **log streams**
  - **Technical definition:** Within CloudWatch or X-Ray, log streams is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **metric filters**
  - **Technical definition:** Within CloudWatch or X-Ray, metric filters is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Contributor Insights rules**
  - **Technical definition:** Within CloudWatch or X-Ray, Contributor Insights rules is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **synthetic canaries**
  - **Technical definition:** Within CloudWatch or X-Ray, synthetic canaries is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Application Signals services**
  - **Technical definition:** Within CloudWatch or X-Ray, Application Signals services is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Container Insights clusters**
  - **Technical definition:** Within CloudWatch or X-Ray, Container Insights clusters is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Logs Insights queries**
  - **Technical definition:** Within CloudWatch or X-Ray, Logs Insights queries is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **traces**
  - **Technical definition:** Within CloudWatch or X-Ray, traces is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **segments**
  - **Technical definition:** Within CloudWatch or X-Ray, segments is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **subsegments**
  - **Technical definition:** Within CloudWatch or X-Ray, subsegments is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **service maps**
  - **Technical definition:** Within CloudWatch or X-Ray, service maps is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **groups**
  - **Technical definition:** Within CloudWatch or X-Ray, groups is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **sampling rules**
  - **Technical definition:** Within CloudWatch or X-Ray, sampling rules is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **annotations**
  - **Technical definition:** Within CloudWatch or X-Ray, annotations is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **metadata**
  - **Technical definition:** Within CloudWatch or X-Ray, metadata is a telemetry, alerting, dashboarding, or trace-analysis entity used to observe AWS and application runtime behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### AWS Distro for OpenTelemetry (ADOT), Amazon Managed Service for Prometheus, and Amazon Managed Grafana

- **AWS Distro for OpenTelemetry (ADOT)**
  - **Technical definition:** AWS Distro for OpenTelemetry is AWS's supported distribution of OpenTelemetry components for collecting and exporting telemetry from applications and infrastructure.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon Managed Service for Prometheus**
  - **Technical definition:** Amazon Managed Service for Prometheus is AWS's managed Prometheus-compatible metrics storage and query backend.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **and Amazon Managed Grafana**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, and Amazon Managed Grafana is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **ADOT collectors**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, ADOT collectors is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **pipelines**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, pipelines is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **receivers**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, receivers is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **processors**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, processors is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **exporters**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, exporters is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Prometheus workspaces**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, Prometheus workspaces is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **rule groups namespaces**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, rule groups namespaces is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **scrapers**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, scrapers is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **metrics**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, metrics is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **labels**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, labels is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Grafana workspaces**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, Grafana workspaces is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **data sources**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, data sources is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **dashboards**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, dashboards is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **folders**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, folders is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **users**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, users is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **teams**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, teams is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **service accounts**
  - **Technical definition:** Within ADOT, managed Prometheus, or managed Grafana, service accounts is a metrics or visualization construct that supports open observability pipelines on AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### AWS Systems Manager

- **AWS Systems Manager**
  - **Technical definition:** AWS Systems Manager is AWS's fleet operations service for remote management, patching, automation, session access, parameter storage, and node state control.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **managed nodes**
  - **Technical definition:** Within AWS Systems Manager, managed nodes is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **documents**
  - **Technical definition:** Within AWS Systems Manager, documents is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Run Command**
  - **Technical definition:** Within AWS Systems Manager, Run Command is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Session Manager sessions**
  - **Technical definition:** Within AWS Systems Manager, Session Manager sessions is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **State Manager associations**
  - **Technical definition:** Within AWS Systems Manager, State Manager associations is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Patch Manager baselines**
  - **Technical definition:** Within AWS Systems Manager, Patch Manager baselines is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Automation runbooks**
  - **Technical definition:** Within AWS Systems Manager, Automation runbooks is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Parameter Store parameters**
  - **Technical definition:** Within AWS Systems Manager, Parameter Store parameters is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **OpsItems**
  - **Technical definition:** Within AWS Systems Manager, OpsItems is a fleet-management, automation, patching, or remote-operations construct that controls managed-node behavior.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### AWS Well-Architected Tool and AWS Resilience Hub

- **AWS Well-Architected Tool**
  - **Technical definition:** AWS Well-Architected Tool is AWS's structured architecture review service for evaluating workloads against AWS best-practice pillars.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **AWS Resilience Hub**
  - **Technical definition:** AWS Resilience Hub is AWS's resilience-assessment service that maps applications, recommends tests, and evaluates readiness against recovery objectives.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **workloads**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, workloads is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **lenses**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, lenses is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **milestones**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, milestones is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **improvement plans**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, improvement plans is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **review templates**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, review templates is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **profiles**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, profiles is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **workload shares**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, workload shares is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **applications**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, applications is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **app components**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, app components is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **resilience policies**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, resilience policies is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **assessments**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, assessments is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **resiliency scores**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, resiliency scores is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **recommendations**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, recommendations is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **standard operating procedures (SOPs)**
  - **Technical definition:** Within AWS Well-Architected or Resilience Hub, standard operating procedures (SOPs) is an assessment, workload-modeling, or recommendation entity used to evaluate architecture quality and recovery readiness.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### AWS Cost Explorer, AWS Budgets, AWS Cost Categories, AWS Cost and Usage Reports (AWS CUR), AWS Cost Anomaly Detection, AWS Compute Optimizer, and AWS Trusted Advisor

- **AWS Cost Explorer**
  - **Technical definition:** AWS Cost Explorer is AWS's interactive cost and usage analysis service for exploring spend dimensions, trends, and forecast views.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **AWS Budgets**
  - **Technical definition:** AWS Budgets is AWS's threshold-based cost and usage alerting service tied to defined scopes and escalation rules.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **AWS Cost Categories**
  - **Technical definition:** AWS Cost Categories is AWS's cost-classification feature for grouping spend into business-defined reporting dimensions.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Cost and Usage Reports (AWS CUR)**
  - **Technical definition:** AWS CUR is AWS's detailed usage and billing export for granular FinOps analysis, chargeback, and downstream processing.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **AWS Cost Anomaly Detection**
  - **Technical definition:** AWS Cost Anomaly Detection is AWS's managed service for identifying unexpected spend patterns and notifying owners based on anomaly monitors.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Compute Optimizer**
  - **Technical definition:** AWS Compute Optimizer is AWS's recommendation service for rightsizing compute and related resources using observed usage data.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **and AWS Trusted Advisor**
  - **Technical definition:** Within AWS FinOps tooling, and AWS Trusted Advisor is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **cost and usage data**
  - **Technical definition:** Within AWS FinOps tooling, cost and usage data is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **dimensions**
  - **Technical definition:** Within AWS FinOps tooling, dimensions is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **filters**
  - **Technical definition:** Within AWS FinOps tooling, filters is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **forecasts**
  - **Technical definition:** Within AWS FinOps tooling, forecasts is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **budgets**
  - **Technical definition:** Within AWS FinOps tooling, budgets is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **budget actions**
  - **Technical definition:** Within AWS FinOps tooling, budget actions is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **alerts**
  - **Technical definition:** Within AWS FinOps tooling, alerts is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **cost category rules**
  - **Technical definition:** Within AWS FinOps tooling, cost category rules is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **CUR report definitions**
  - **Technical definition:** Within AWS FinOps tooling, CUR report definitions is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **anomaly monitors**
  - **Technical definition:** Within AWS FinOps tooling, anomaly monitors is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **anomalies**
  - **Technical definition:** Within AWS FinOps tooling, anomalies is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **recommendations**
  - **Technical definition:** Within AWS FinOps tooling, recommendations is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **checks**
  - **Technical definition:** Within AWS FinOps tooling, checks is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### Amazon GuardDuty, Amazon Inspector, Amazon Macie, Amazon Detective, and Amazon Security Lake

- **Amazon GuardDuty**
  - **Technical definition:** Amazon GuardDuty is AWS's threat-detection service that analyzes AWS telemetry sources for suspicious activity and compromise indicators.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Amazon Inspector**
  - **Technical definition:** Amazon Inspector is AWS's vulnerability and code-scanning service for supported compute and workload surfaces.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Amazon Macie**
  - **Technical definition:** Amazon Macie is AWS's data-security service for discovering, classifying, and alerting on sensitive data in Amazon S3.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Amazon Detective**
  - **Technical definition:** Amazon Detective is AWS's security-investigation service for graph-based exploration of security findings and related telemetry.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **and Amazon Security Lake**
  - **Technical definition:** Within AWS FinOps tooling, and Amazon Security Lake is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **detectors**
  - **Technical definition:** Within AWS FinOps tooling, detectors is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **findings**
  - **Technical definition:** Within AWS FinOps tooling, findings is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **threat intelligence sets**
  - **Technical definition:** Within AWS FinOps tooling, threat intelligence sets is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **malware protection plans**
  - **Technical definition:** Within AWS FinOps tooling, malware protection plans is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **coverage**
  - **Technical definition:** Within AWS FinOps tooling, coverage is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **software bills of materials (SBOMs)**
  - **Technical definition:** Within AWS FinOps tooling, software bills of materials (SBOMs) is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **classification jobs**
  - **Technical definition:** Within AWS FinOps tooling, classification jobs is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **sensitive data discovery results**
  - **Technical definition:** Within AWS FinOps tooling, sensitive data discovery results is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **managed data identifiers**
  - **Technical definition:** Within AWS FinOps tooling, managed data identifiers is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **behavior graphs**
  - **Technical definition:** Within AWS FinOps tooling, behavior graphs is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **investigations**
  - **Technical definition:** Within AWS FinOps tooling, investigations is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **data lakes**
  - **Technical definition:** Within AWS FinOps tooling, data lakes is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **sources**
  - **Technical definition:** Within AWS FinOps tooling, sources is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **subscribers**
  - **Technical definition:** Within AWS FinOps tooling, subscribers is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Open Cybersecurity Schema Framework (OCSF) event classes**
  - **Technical definition:** Within AWS FinOps tooling, Open Cybersecurity Schema Framework (OCSF) event classes is a cost-analysis, anomaly, allocation, or optimization construct used to understand and improve AWS spending.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### Amazon SageMaker AI and Amazon Bedrock

- **Amazon SageMaker AI**
  - **Technical definition:** Within Amazon SageMaker, Amazon SageMaker AI is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon Bedrock**
  - **Technical definition:** Amazon Bedrock is AWS's managed generative AI platform for invoking foundation models and building controlled AI applications without directly managing model infrastructure.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **SageMaker monitoring schedules**
  - **Technical definition:** Within Amazon SageMaker, SageMaker monitoring schedules is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **baselines**
  - **Technical definition:** Within Amazon SageMaker, baselines is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **constraints**
  - **Technical definition:** Within Amazon SageMaker, constraints is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **processing jobs**
  - **Technical definition:** Within Amazon SageMaker, processing jobs is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **bias reports**
  - **Technical definition:** Within Amazon SageMaker, bias reports is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **explainability reports**
  - **Technical definition:** Within Amazon SageMaker, explainability reports is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **experiments**
  - **Technical definition:** Within Amazon SageMaker, experiments is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **trials**
  - **Technical definition:** Within Amazon SageMaker, trials is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage artifacts**
  - **Technical definition:** Within Amazon SageMaker, lineage artifacts is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage actions**
  - **Technical definition:** Within Amazon SageMaker, lineage actions is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Bedrock model evaluation jobs**
  - **Technical definition:** Within Amazon SageMaker, Bedrock model evaluation jobs is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **evaluation datasets**
  - **Technical definition:** Within Amazon SageMaker, evaluation datasets is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrails**
  - **Technical definition:** Within Amazon SageMaker, guardrails is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrail versions**
  - **Technical definition:** Within Amazon SageMaker, guardrail versions is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **foundation models**
  - **Technical definition:** Within Amazon SageMaker, foundation models is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **agents**
  - **Technical definition:** Within Amazon SageMaker, agents is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **invocation logs**
  - **Technical definition:** Within Amazon SageMaker, invocation logs is an ML workflow entity that controls data preparation, training, model lifecycle, deployment, or monitoring.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Controls

### Operational And Control Points

- **CloudWatch alarm actions**
  - **Technical definition:** CloudWatch alarm actions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **metric math**
  - **Technical definition:** metric math is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **composite alarms**
  - **Technical definition:** composite alarms is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **log retention**
  - **Technical definition:** log retention is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **data protection policies**
  - **Technical definition:** data protection policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account observability**
  - **Technical definition:** cross-account observability is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **dashboards**
  - **Technical definition:** dashboards is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **X-Ray sampling rules**
  - **Technical definition:** X-Ray sampling rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **trace retention**
  - **Technical definition:** trace retention is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **SDK configuration**
  - **Technical definition:** SDK configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **ADOT telemetry routing**
  - **Technical definition:** ADOT telemetry routing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **sampling**
  - **Technical definition:** sampling is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **exporter configuration**
  - **Technical definition:** exporter configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Prometheus workspace policies**
  - **Technical definition:** Prometheus workspace policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **ingestion limits**
  - **Technical definition:** ingestion limits is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **alerting rules**
  - **Technical definition:** alerting rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Grafana authentication providers**
  - **Technical definition:** Grafana authentication providers is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **data source permissions**
  - **Technical definition:** data source permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **workspace roles**
  - **Technical definition:** workspace roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **plugin management**
  - **Technical definition:** plugin management is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Systems Manager instance profiles**
  - **Technical definition:** Systems Manager instance profiles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **session logging**
  - **Technical definition:** session logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **patch baselines**
  - **Technical definition:** patch baselines is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **document permissions**
  - **Technical definition:** document permissions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **parameter policies**
  - **Technical definition:** parameter policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Well-Architected lens selection**
  - **Technical definition:** Well-Architected lens selection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **workload sharing**
  - **Technical definition:** workload sharing is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Resilience Hub recovery time objective/recovery point objective (RTO/RPO) targets**
  - **Technical definition:** Resilience Hub recovery time objective/recovery point objective (RTO/RPO) targets is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **policy mapping**
  - **Technical definition:** policy mapping is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **application import sources**
  - **Technical definition:** application import sources is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **assessment scope**
  - **Technical definition:** assessment scope is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Cost filters**
  - **Technical definition:** Cost filters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **grouping dimensions**
  - **Technical definition:** grouping dimensions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **budget thresholds**
  - **Technical definition:** budget thresholds is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **budget actions**
  - **Technical definition:** budget actions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **split charge allocation**
  - **Technical definition:** split charge allocation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **CUR delivery options**
  - **Technical definition:** CUR delivery options is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **anomaly alert thresholds**
  - **Technical definition:** anomaly alert thresholds is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **recommendation preferences**
  - **Technical definition:** recommendation preferences is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **organizational views**
  - **Technical definition:** organizational views is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Security delegated administrator accounts**
  - **Technical definition:** Security delegated administrator accounts is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **GuardDuty detector features**
  - **Technical definition:** GuardDuty detector features is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **S3 protection**
  - **Technical definition:** S3 protection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **EKS protection**
  - **Technical definition:** EKS protection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon ECR scanning**
  - **Technical definition:** Amazon ECR scanning is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Lambda scanning**
  - **Technical definition:** AWS Lambda scanning is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **finding filters**
  - **Technical definition:** finding filters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **automated sensitive data discovery**
  - **Technical definition:** automated sensitive data discovery is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **graph retention**
  - **Technical definition:** graph retention is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **source collection**
  - **Technical definition:** source collection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **subscriber access**
  - **Technical definition:** subscriber access is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region rollup**
  - **Technical definition:** cross-Region rollup is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **SageMaker data capture**
  - **Technical definition:** SageMaker data capture is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **baseline constraints**
  - **Technical definition:** baseline constraints is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **bias metrics**
  - **Technical definition:** bias metrics is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **explainability configuration**
  - **Technical definition:** explainability configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **experiment metadata**
  - **Technical definition:** experiment metadata is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lineage metadata**
  - **Technical definition:** lineage metadata is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Bedrock model access**
  - **Technical definition:** Bedrock model access is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrail policies**
  - **Technical definition:** guardrail policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **content filters**
  - **Technical definition:** content filters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **sensitive information filters**
  - **Technical definition:** sensitive information filters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **VPC endpoints**
  - **Technical definition:** VPC endpoints is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **logging**
  - **Technical definition:** logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Operational And Control Points

- **Review dashboards**
  - **Technical definition:** Review dashboards is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **alarms**
  - **Technical definition:** alarms is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Logs Insights queries**
  - **Technical definition:** Logs Insights queries is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **synthetic checks**
  - **Technical definition:** synthetic checks is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **distributed traces**
  - **Technical definition:** distributed traces is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **dependency maps**
  - **Technical definition:** dependency maps is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **latency root causes**
  - **Technical definition:** latency root causes is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **incident timelines**
  - **Technical definition:** incident timelines is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Roll out ADOT collectors**
  - **Technical definition:** Roll out ADOT collectors is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate Prometheus queries**
  - **Technical definition:** validate Prometheus queries is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **tune metric cardinality**
  - **Technical definition:** tune metric cardinality is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **publish Grafana dashboards**
  - **Technical definition:** publish Grafana dashboards is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **troubleshoot data sources**
  - **Technical definition:** troubleshoot data sources is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Execute fleet commands**
  - **Technical definition:** Execute fleet commands is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **open Session Manager sessions**
  - **Technical definition:** open Session Manager sessions is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review patch compliance**
  - **Technical definition:** review patch compliance is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **run automations**
  - **Technical definition:** run automations is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **track OpsItems**
  - **Technical definition:** track OpsItems is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Perform Well-Architected reviews**
  - **Technical definition:** Perform Well-Architected reviews is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **track risks**
  - **Technical definition:** track risks is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **run Resilience Hub assessments**
  - **Technical definition:** run Resilience Hub assessments is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review resiliency recommendations**
  - **Technical definition:** review resiliency recommendations is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Analyze spend**
  - **Technical definition:** Analyze spend is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review forecasts**
  - **Technical definition:** review forecasts is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **tune budgets**
  - **Technical definition:** tune budgets is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate CUR delivery**
  - **Technical definition:** validate CUR delivery is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **triage cost anomalies**
  - **Technical definition:** triage cost anomalies is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **track rightsizing recommendations**
  - **Technical definition:** track rightsizing recommendations is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Triage GuardDuty findings**
  - **Technical definition:** Triage GuardDuty findings is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Inspector vulnerabilities**
  - **Technical definition:** Inspector vulnerabilities is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Macie sensitive data findings**
  - **Technical definition:** Macie sensitive data findings is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Detective investigations**
  - **Technical definition:** Detective investigations is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Security Lake subscriber delivery**
  - **Technical definition:** Security Lake subscriber delivery is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Review model drift**
  - **Technical definition:** Review model drift is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **bias reports**
  - **Technical definition:** bias reports is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **explainability reports**
  - **Technical definition:** explainability reports is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **model lineage**
  - **Technical definition:** model lineage is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Bedrock evaluations**
  - **Technical definition:** Bedrock evaluations is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **guardrail tests**
  - **Technical definition:** guardrail tests is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **generative AI governance evidence**
  - **Technical definition:** generative AI governance evidence is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

# AWS Week 6: DR, Multi-Region, And Migration Execution

## Service And Entity Coverage

### Amazon Application Recovery Controller (ARC) (Route 53 ARC)

- **Amazon Application Recovery Controller (ARC) (Route 53 ARC)**
  - **Technical definition:** Amazon Application Recovery Controller is AWS's recovery-readiness and routing-safety service for zonal or regional failover operations.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **clusters**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, clusters is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **routing controls**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, routing controls is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **control panels**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, control panels is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **safety rules**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, safety rules is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **readiness checks**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, readiness checks is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **recovery groups**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, recovery groups is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **cells**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, cells is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.
- **resource sets**
  - **Technical definition:** Within Route 53 or Route 53 Resolver, resource sets is a DNS namespace, routing, health, or recursive-resolution construct that shapes name answer behavior in AWS and hybrid environments.
  - **Concrete production example:** Example: before a migration cutover, the team validates this to confirm packet path, DNS behavior, TLS posture, health checks, and private connectivity all match the intended design.

### AWS Elastic Disaster Recovery (AWS DRS) and AWS Application Migration Service (AWS MGN)

- **AWS Elastic Disaster Recovery (AWS DRS)**
  - **Technical definition:** AWS Elastic Disaster Recovery is AWS's block-level replication and recovery orchestration service for server-based disaster recovery into AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **AWS Application Migration Service (AWS MGN)**
  - **Technical definition:** AWS Application Migration Service is AWS's lift-and-shift migration service for replicating and launching source servers into AWS.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **DRS source servers**
  - **Technical definition:** Within AWS DRS or AWS MGN, DRS source servers is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **replication servers**
  - **Technical definition:** Within AWS DRS or AWS MGN, replication servers is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **staging area subnets**
  - **Technical definition:** Within AWS DRS or AWS MGN, staging area subnets is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **recovery instances**
  - **Technical definition:** Within AWS DRS or AWS MGN, recovery instances is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **recovery snapshots**
  - **Technical definition:** Within AWS DRS or AWS MGN, recovery snapshots is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **conversion servers**
  - **Technical definition:** Within AWS DRS or AWS MGN, conversion servers is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **MGN source servers**
  - **Technical definition:** Within AWS DRS or AWS MGN, MGN source servers is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **replication agents**
  - **Technical definition:** Within AWS DRS or AWS MGN, replication agents is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **replication settings templates**
  - **Technical definition:** Within AWS DRS or AWS MGN, replication settings templates is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **launch settings**
  - **Technical definition:** Within AWS DRS or AWS MGN, launch settings is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **test instances**
  - **Technical definition:** Within AWS DRS or AWS MGN, test instances is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **cutover instances**
  - **Technical definition:** Within AWS DRS or AWS MGN, cutover instances is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **waves**
  - **Technical definition:** Within AWS DRS or AWS MGN, waves is a replication, launch, test, or cutover construct that affects machine migration and disaster-recovery execution.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### AWS Migration Hub

- **AWS Migration Hub**
  - **Technical definition:** AWS Migration Hub is AWS's migration program tracking service for grouping applications, waves, and tool-integrated status in one view.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **applications**
  - **Technical definition:** Within AWS Migration Hub, applications is a portfolio, wave, application, or progress-tracking entity used to manage migration program state.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **servers**
  - **Technical definition:** Within AWS Migration Hub, servers is a portfolio, wave, application, or progress-tracking entity used to manage migration program state.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **migration waves**
  - **Technical definition:** Within AWS Migration Hub, migration waves is a portfolio, wave, application, or progress-tracking entity used to manage migration program state.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **discovered resources**
  - **Technical definition:** Within AWS Migration Hub, discovered resources is a portfolio, wave, application, or progress-tracking entity used to manage migration program state.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **migration tools**
  - **Technical definition:** Within AWS Migration Hub, migration tools is a portfolio, wave, application, or progress-tracking entity used to manage migration program state.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **home Region**
  - **Technical definition:** Within AWS Migration Hub, home Region is a portfolio, wave, application, or progress-tracking entity used to manage migration program state.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **migration statuses**
  - **Technical definition:** Within AWS Migration Hub, migration statuses is a portfolio, wave, application, or progress-tracking entity used to manage migration program state.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### AWS Database Migration Service (AWS DMS) and AWS Schema Conversion Tool (AWS SCT)

- **AWS Database Migration Service (AWS DMS)**
  - **Technical definition:** AWS DMS is AWS's managed data replication and migration service for moving and continuously replicating database state between heterogeneous or homogeneous engines.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **AWS Schema Conversion Tool (AWS SCT)**
  - **Technical definition:** AWS SCT is AWS's schema and code conversion utility for assessing and transforming database objects toward target engines.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **replication instances**
  - **Technical definition:** Within AWS Backup or AWS DMS, replication instances is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication tasks**
  - **Technical definition:** Within AWS Backup or AWS DMS, replication tasks is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **source endpoints**
  - **Technical definition:** Within AWS Backup or AWS DMS, source endpoints is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **target endpoints**
  - **Technical definition:** Within AWS Backup or AWS DMS, target endpoints is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **replication subnet groups**
  - **Technical definition:** Within AWS Backup or AWS DMS, replication subnet groups is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **table mappings**
  - **Technical definition:** Within AWS Backup or AWS DMS, table mappings is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **validation reports**
  - **Technical definition:** Within AWS Backup or AWS DMS, validation reports is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **SCT migration projects**
  - **Technical definition:** Within AWS Backup or AWS DMS, SCT migration projects is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **source schemas**
  - **Technical definition:** Within AWS Backup or AWS DMS, source schemas is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **target schemas**
  - **Technical definition:** Within AWS Backup or AWS DMS, target schemas is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **assessment reports**
  - **Technical definition:** Within AWS Backup or AWS DMS, assessment reports is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **conversion rules**
  - **Technical definition:** Within AWS Backup or AWS DMS, conversion rules is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **converted schema objects**
  - **Technical definition:** Within AWS Backup or AWS DMS, converted schema objects is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### AWS DataSync and AWS Transfer Family

- **AWS DataSync**
  - **Technical definition:** AWS DataSync is AWS's managed high-throughput data-transfer service for moving file and object data between on-premises, AWS, and other storage endpoints.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **AWS Transfer Family**
  - **Technical definition:** AWS Transfer Family is AWS's managed file-transfer endpoint service for SFTP, FTPS, FTP, and AS2 workflows backed by AWS storage services.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **DataSync agents**
  - **Technical definition:** Within AWS DataSync or Transfer Family, DataSync agents is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **locations**
  - **Technical definition:** Within AWS DataSync or Transfer Family, locations is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **tasks**
  - **Technical definition:** Within AWS DataSync or Transfer Family, tasks is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **task executions**
  - **Technical definition:** Within AWS DataSync or Transfer Family, task executions is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **manifests**
  - **Technical definition:** Within AWS DataSync or Transfer Family, manifests is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **filters**
  - **Technical definition:** Within AWS DataSync or Transfer Family, filters is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **reports**
  - **Technical definition:** Within AWS DataSync or Transfer Family, reports is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **transfer options**
  - **Technical definition:** Within AWS DataSync or Transfer Family, transfer options is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **Transfer Family servers**
  - **Technical definition:** Within AWS DataSync or Transfer Family, Transfer Family servers is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **users**
  - **Technical definition:** Within AWS DataSync or Transfer Family, users is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **workflows**
  - **Technical definition:** Within AWS DataSync or Transfer Family, workflows is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **connectors**
  - **Technical definition:** Within AWS DataSync or Transfer Family, connectors is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **identity providers**
  - **Technical definition:** Within AWS DataSync or Transfer Family, identity providers is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **protocols**
  - **Technical definition:** Within AWS DataSync or Transfer Family, protocols is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **host keys**
  - **Technical definition:** Within AWS DataSync or Transfer Family, host keys is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **access endpoints**
  - **Technical definition:** Within AWS DataSync or Transfer Family, access endpoints is a transfer-endpoint, task, user, or data-movement construct used in migration and integration workflows.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

### AWS Backup

- **AWS Backup**
  - **Technical definition:** AWS Backup is AWS's centralized backup policy and vaulting service for supported AWS resources across accounts and Regions.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backup plans**
  - **Technical definition:** Within AWS Backup or AWS DMS, backup plans is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backup vaults**
  - **Technical definition:** Within AWS Backup or AWS DMS, backup vaults is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **recovery points**
  - **Technical definition:** Within AWS Backup or AWS DMS, recovery points is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **backup vault lock configurations**
  - **Technical definition:** Within AWS Backup or AWS DMS, backup vault lock configurations is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **copy jobs**
  - **Technical definition:** Within AWS Backup or AWS DMS, copy jobs is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **restore jobs**
  - **Technical definition:** Within AWS Backup or AWS DMS, restore jobs is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **legal holds**
  - **Technical definition:** Within AWS Backup or AWS DMS, legal holds is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.
- **restore testing plans**
  - **Technical definition:** Within AWS Backup or AWS DMS, restore testing plans is a recovery, vaulting, copy, or replication construct that affects data protection and migration behavior.
  - **Concrete production example:** Example: the team validates this in production readiness by testing backup restore, replication, throughput behavior, access control, or data correctness under failure and cutover conditions.

### AWS Fault Injection Service (AWS FIS)

- **AWS Fault Injection Service (AWS FIS)**
  - **Technical definition:** AWS Fault Injection Service is AWS's managed chaos engineering service for controlled failure injection against selected AWS resources and systems.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **experiment templates**
  - **Technical definition:** Within AWS Fault Injection Service, experiment templates is a chaos-experiment definition, target selector, stop condition, or action used to validate resilience under controlled failure.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **experiments**
  - **Technical definition:** Within AWS Fault Injection Service, experiments is a chaos-experiment definition, target selector, stop condition, or action used to validate resilience under controlled failure.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **actions**
  - **Technical definition:** Within AWS Fault Injection Service, actions is a chaos-experiment definition, target selector, stop condition, or action used to validate resilience under controlled failure.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **targets**
  - **Technical definition:** Within AWS Fault Injection Service, targets is a chaos-experiment definition, target selector, stop condition, or action used to validate resilience under controlled failure.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **stop conditions**
  - **Technical definition:** Within AWS Fault Injection Service, stop conditions is a chaos-experiment definition, target selector, stop condition, or action used to validate resilience under controlled failure.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **experiment roles**
  - **Technical definition:** Within AWS Fault Injection Service, experiment roles is a chaos-experiment definition, target selector, stop condition, or action used to validate resilience under controlled failure.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.
- **experiment reports**
  - **Technical definition:** Within AWS Fault Injection Service, experiment reports is a chaos-experiment definition, target selector, stop condition, or action used to validate resilience under controlled failure.
  - **Concrete production example:** Example: SREs or migration engineers rely on this to generate telemetry, assess resilience, rehearse failure, track migration progress, or prove recovery objectives with evidence.

## DR Dependencies From Earlier Weeks

### Operational And Control Points

- **Amazon Route 53 DNS failover and health checks are covered in Week 2**
  - **Technical definition:** Amazon Route 53 DNS failover and health checks are covered in Week 2 is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Amazon S3 replication**
  - **Technical definition:** Amazon S3 replication is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon EFS replication**
  - **Technical definition:** Amazon EFS replication is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon Aurora Global Database**
  - **Technical definition:** Amazon Aurora Global Database is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon RDS Read Replicas**
  - **Technical definition:** Amazon RDS Read Replicas is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Amazon DynamoDB global tables are covered in Week 4**
  - **Technical definition:** Amazon DynamoDB global tables are covered in Week 4 is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **IAM emergency access**
  - **Technical definition:** IAM emergency access is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **CloudTrail evidence**
  - **Technical definition:** CloudTrail evidence is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Config drift data**
  - **Technical definition:** AWS Config drift data is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS KMS keys**
  - **Technical definition:** AWS KMS keys is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Backup vault controls are covered in earlier weeks**
  - **Technical definition:** AWS Backup vault controls are covered in earlier weeks is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Controls

### Operational And Control Points

- **ARC routing control state**
  - **Technical definition:** ARC routing control state is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **safety rules**
  - **Technical definition:** safety rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **readiness check scope**
  - **Technical definition:** readiness check scope is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cell design**
  - **Technical definition:** cell design is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **recovery group modeling**
  - **Technical definition:** recovery group modeling is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **DRS/MGN replication settings**
  - **Technical definition:** DRS/MGN replication settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **launch settings**
  - **Technical definition:** launch settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **staging area configuration**
  - **Technical definition:** staging area configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **point-in-time recovery selection**
  - **Technical definition:** point-in-time recovery selection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **failback settings**
  - **Technical definition:** failback settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **post-launch actions**
  - **Technical definition:** post-launch actions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cutover settings**
  - **Technical definition:** cutover settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Migration Hub home Region selection**
  - **Technical definition:** Migration Hub home Region selection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **application grouping**
  - **Technical definition:** application grouping is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **migration status tracking**
  - **Technical definition:** migration status tracking is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **migration tool integration**
  - **Technical definition:** migration tool integration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **DMS full-load settings**
  - **Technical definition:** DMS full-load settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **change data capture (CDC)**
  - **Technical definition:** change data capture (CDC) is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **task settings**
  - **Technical definition:** task settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validation**
  - **Technical definition:** validation is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **transformation rules**
  - **Technical definition:** transformation rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **endpoint encryption**
  - **Technical definition:** endpoint encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **SCT compatibility assessment**
  - **Technical definition:** SCT compatibility assessment is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **schema mapping**
  - **Technical definition:** schema mapping is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **conversion settings**
  - **Technical definition:** conversion settings is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **DataSync task options**
  - **Technical definition:** DataSync task options is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **bandwidth limits**
  - **Technical definition:** bandwidth limits is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **verification mode**
  - **Technical definition:** verification mode is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **include/exclude filters**
  - **Technical definition:** include/exclude filters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **schedule configuration**
  - **Technical definition:** schedule configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **Transfer Family protocol selection**
  - **Technical definition:** Transfer Family protocol selection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **identity provider configuration**
  - **Technical definition:** identity provider configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **workflow triggers**
  - **Technical definition:** workflow triggers is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **logging**
  - **Technical definition:** logging is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **AWS Backup schedules**
  - **Technical definition:** AWS Backup schedules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **AWS Backup Vault Lock**
  - **Technical definition:** AWS Backup Vault Lock is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-Region copy**
  - **Technical definition:** cross-Region copy is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cross-account copy**
  - **Technical definition:** cross-account copy is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **vault access policies**
  - **Technical definition:** vault access policies is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore testing configuration**
  - **Technical definition:** restore testing configuration is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **FIS stop conditions**
  - **Technical definition:** FIS stop conditions is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **IAM experiment roles**
  - **Technical definition:** IAM experiment roles is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **target selection**
  - **Technical definition:** target selection is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **action parameters**
  - **Technical definition:** action parameters is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **safety guardrails**
  - **Technical definition:** safety guardrails is an AWS control that should be evaluated for preventive strength, detective value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

## Operations

### Operational And Control Points

- **Execute ARC routing control tests**
  - **Technical definition:** Execute ARC routing control tests is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **readiness checks**
  - **Technical definition:** readiness checks is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **failover rehearsal**
  - **Technical definition:** failover rehearsal is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **game day evidence collection**
  - **Technical definition:** game day evidence collection is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Replicate servers with AWS DRS**
  - **Technical definition:** Replicate servers with AWS DRS is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **run non-disruptive recovery drills**
  - **Technical definition:** run non-disruptive recovery drills is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **launch recovery instances**
  - **Technical definition:** launch recovery instances is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate failback**
  - **Technical definition:** validate failback is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **review replication lag**
  - **Technical definition:** review replication lag is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Use AWS MGN for migration waves**
  - **Technical definition:** Use AWS MGN for migration waves is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **test launches**
  - **Technical definition:** test launches is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cutover launches**
  - **Technical definition:** cutover launches is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **rollback planning**
  - **Technical definition:** rollback planning is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **post-launch validation**
  - **Technical definition:** post-launch validation is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Track portfolio migration status in AWS Migration Hub and report wave readiness**
  - **Technical definition:** Track portfolio migration status in AWS Migration Hub and report wave readiness is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Run DMS full-load plus CDC migration**
  - **Technical definition:** Run DMS full-load plus CDC migration is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **validate data**
  - **Technical definition:** validate data is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **troubleshoot replication lag**
  - **Technical definition:** troubleshoot replication lag is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **convert schemas with AWS SCT**
  - **Technical definition:** convert schemas with AWS SCT is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **rehearse cutover/rollback**
  - **Technical definition:** rehearse cutover/rollback is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Use DataSync for file/object seeding**
  - **Technical definition:** Use DataSync for file/object seeding is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **incremental sync**
  - **Technical definition:** incremental sync is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **transfer validation**
  - **Technical definition:** transfer validation is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **cutover sync**
  - **Technical definition:** cutover sync is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Validate AWS Backup recovery points**
  - **Technical definition:** Validate AWS Backup recovery points is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **restore testing**
  - **Technical definition:** restore testing is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **immutable backup controls**
  - **Technical definition:** immutable backup controls is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
- **ransomware recovery readiness**
  - **Technical definition:** ransomware recovery readiness is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.

### Operational And Control Points

- **Run AWS FIS experiments with stop conditions to validate resilience assumptions and blast radius**
  - **Technical definition:** Run AWS FIS experiments with stop conditions to validate resilience assumptions and blast radius is an AWS operational activity used to validate configuration, support production change, or reduce incident risk.
  - **Concrete production example:** Example: in production, this becomes important when teams need secure change control, clear ownership, and testable recovery behavior.
