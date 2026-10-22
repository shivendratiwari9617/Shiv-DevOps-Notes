# Deep Technical Analysis of AWS Week-Wise Cloud Coverage
## Technical Study Notes Expanded from Every Comma-Separated Point

> Scope: AWS-focused technical study notes for platform engineering, solutions architecture, SRE, DevOps, security, disaster recovery, and migration.
> Method: every comma-separated point in the uploaded file is treated as its own study point. Definitions are concise, technical, and paired with one concrete production example wherever useful.

# AWS Week 1: Landing Zone, Account Strategy, And Governance

## Service And Entity Coverage

### AWS Organizations and AWS Account Management

- **AWS Organizations and AWS Account Management**
  - **Technical definition:** AWS Organizations and AWS Account Management form the multi-account control plane for structuring accounts, applying inherited governance, delegating administration, and centralizing billing and metadata at enterprise scale.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **organization**
  - **Technical definition:** organization is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **root**
  - **Technical definition:** root is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **management account**
  - **Technical definition:** management account is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **member accounts**
  - **Technical definition:** member accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **organizational units (OUs)**
  - **Technical definition:** organizational units (OUs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **delegated administrators**
  - **Technical definition:** delegated administrators is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **service control policies (SCPs)**
  - **Technical definition:** service control policies (SCPs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource control policies (RCPs)**
  - **Technical definition:** resource control policies (RCPs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tag policies**
  - **Technical definition:** tag policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backup policies**
  - **Technical definition:** backup policies are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **AI services opt-out policies**
  - **Technical definition:** AI services opt-out policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS accounts**
  - **Technical definition:** AWS accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **primary contacts**
  - **Technical definition:** primary contacts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **alternate contacts**
  - **Technical definition:** alternate contacts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Regions**
  - **Technical definition:** AWS Regions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Region opt-in status**
  - **Technical definition:** Region opt-in status is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Control Tower

- **AWS Control Tower**
  - **Technical definition:** AWS Control Tower is AWS's managed landing-zone orchestration layer that applies an opinionated multi-account baseline using AWS Organizations, guardrails, account vending, and continuous drift visibility.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **landing zone**
  - **Technical definition:** landing zone is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Account Factory**
  - **Technical definition:** Account Factory is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **governed accounts**
  - **Technical definition:** governed accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **registered organizational units (OUs)**
  - **Technical definition:** registered organizational units (OUs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **preventive controls**
  - **Technical definition:** preventive controls is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **detective controls**
  - **Technical definition:** detective controls is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **proactive controls**
  - **Technical definition:** proactive controls is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **drift status**
  - **Technical definition:** drift status is the divergence between intended AWS configuration state and live deployed state, usually requiring investigation and remediation.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Identity and Access Management (IAM)

- **AWS Identity and Access Management (IAM)**
  - **Technical definition:** AWS Identity and Access Management (IAM) is AWS's authorization and identity-policy system for controlling who or what can call AWS APIs, assume roles, or access resources under explicitly evaluated policy.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **IAM users**
  - **Technical definition:** IAM users are human or service identities represented directly in an AWS identity system and evaluated against attached or inherited access policy.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **IAM groups**
  - **Technical definition:** IAM groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS identity principal or trust construct used to obtain temporary permissions through assumption rather than long-lived static credentials.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **IAM policies**
  - **Technical definition:** IAM policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **instance profiles**
  - **Technical definition:** instance profiles are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access keys**
  - **Technical definition:** access keys are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **MFA devices**
  - **Technical definition:** MFA devices is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **role trust policies**
  - **Technical definition:** role trust policies is an AWS identity principal or trust construct used to obtain temporary permissions through assumption rather than long-lived static credentials.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **analyzers**
  - **Technical definition:** analyzers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **findings**
  - **Technical definition:** findings are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **archive rules**
  - **Technical definition:** archive rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **access previews**
  - **Technical definition:** access previews is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **policy validation checks**
  - **Technical definition:** policy validation checks is an AWS policy construct used to constrain, permit, inherit, or validate behavior against a defined authorization or governance boundary.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS IAM Identity Center

- **AWS IAM Identity Center**
  - **Technical definition:** AWS IAM Identity Center is AWS's workforce identity and account-access federation service used to map enterprise identities and permission sets into AWS account sessions and application access.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **identity sources**
  - **Technical definition:** identity sources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **users**
  - **Technical definition:** users are human or service identities represented directly in an AWS identity system and evaluated against attached or inherited access policy.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **groups**
  - **Technical definition:** groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **permission sets**
  - **Technical definition:** permission sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **account assignments**
  - **Technical definition:** account assignments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **application assignments**
  - **Technical definition:** application assignments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Service Catalog and AWS Resource Access Manager

- **AWS Service Catalog and AWS Resource Access Manager**
  - **Technical definition:** AWS Service Catalog and AWS Resource Access Manager together expose governed self-service products and controlled cross-account resource sharing as platform primitives.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **portfolios**
  - **Technical definition:** portfolios is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **products**
  - **Technical definition:** products is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **provisioning artifacts**
  - **Technical definition:** provisioning artifacts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **provisioned products**
  - **Technical definition:** provisioned products is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **constraints**
  - **Technical definition:** constraints is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **launch paths**
  - **Technical definition:** launch paths is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **portfolio access**
  - **Technical definition:** portfolio access is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource shares (AWS RAM shares)**
  - **Technical definition:** resource shares (AWS RAM shares) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **principals**
  - **Technical definition:** principals is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resources**
  - **Technical definition:** resources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **managed permissions**
  - **Technical definition:** managed permissions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **customer managed permissions**
  - **Technical definition:** customer managed permissions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **invitations**
  - **Technical definition:** invitations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS CloudFormation StackSets

- **AWS CloudFormation StackSets**
  - **Technical definition:** AWS CloudFormation StackSets provide centrally managed multi-account, multi-Region infrastructure deployment using a single template and coordinated stack-instance operations.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stack sets (StackSets)**
  - **Technical definition:** stack sets (StackSets) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stack instances**
  - **Technical definition:** stack instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stack operations**
  - **Technical definition:** stack operations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **administrator accounts**
  - **Technical definition:** administrator accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **execution roles**
  - **Technical definition:** execution roles is an AWS identity principal or trust construct used to obtain temporary permissions through assumption rather than long-lived static credentials.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **target accounts**
  - **Technical definition:** target accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **target organizational units (OUs)**
  - **Technical definition:** target organizational units (OUs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **operation preferences**
  - **Technical definition:** operation preferences is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS CloudTrail and AWS Config

- **AWS CloudTrail and AWS Config**
  - **Technical definition:** AWS CloudTrail and AWS Config together provide API audit evidence and continuously evaluated configuration-state visibility across AWS resources and accounts.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **trails**
  - **Technical definition:** trails is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **organization trails**
  - **Technical definition:** organization trails is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **event data stores**
  - **Technical definition:** event data stores is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **management events**
  - **Technical definition:** management events is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **data events**
  - **Technical definition:** data events is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Insights events**
  - **Technical definition:** Insights events are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **configuration recorders**
  - **Technical definition:** configuration recorders are DNS namespace, answer, or recursive-resolution constructs that control how names are published and resolved in Route 53 workflows.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **delivery channels**
  - **Technical definition:** delivery channels is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **configuration items**
  - **Technical definition:** configuration items is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **rules**
  - **Technical definition:** rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **conformance packs**
  - **Technical definition:** conformance packs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **configuration aggregators**
  - **Technical definition:** configuration aggregators is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **remediation actions**
  - **Technical definition:** remediation actions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Security Hub and AWS Audit Manager

- **AWS Security Hub and AWS Audit Manager**
  - **Technical definition:** AWS Security Hub and AWS Audit Manager provide centralized security finding aggregation and evidence-oriented compliance assessment workflows across AWS environments.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **security standards**
  - **Technical definition:** security standards is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **controls**
  - **Technical definition:** controls is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **findings**
  - **Technical definition:** findings are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **insights**
  - **Technical definition:** insights are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **automation rules**
  - **Technical definition:** automation rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **finding aggregators**
  - **Technical definition:** finding aggregators are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **administrator accounts**
  - **Technical definition:** administrator accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **member accounts**
  - **Technical definition:** member accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **assessments**
  - **Technical definition:** assessments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **frameworks**
  - **Technical definition:** frameworks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **control sets**
  - **Technical definition:** control sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **evidence folders**
  - **Technical definition:** evidence folders are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **evidence**
  - **Technical definition:** evidence are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Evidence Finder**
  - **Technical definition:** Evidence Finder are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **assessment reports**
  - **Technical definition:** assessment reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Artifact, AWS Health, and AWS Service Quotas

- **AWS Artifact, AWS Health, and AWS Service Quotas**
  - **Technical definition:** AWS Artifact, AWS Health, and AWS Service Quotas expose compliance documents, provider-side service event visibility, and enforceable resource-limit management needed for governed operations.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **reports**
  - **Technical definition:** reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **agreements**
  - **Technical definition:** agreements is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **notifications**
  - **Technical definition:** notifications is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Health events**
  - **Technical definition:** AWS Health events is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **affected entities**
  - **Technical definition:** affected entities is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **event status codes**
  - **Technical definition:** event status codes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **service quotas**
  - **Technical definition:** service quotas are enforceable AWS service limits that cap scale, recovery, and migration concurrency until increased or redesigned around.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **applied quota values**
  - **Technical definition:** applied quota values are enforceable AWS service limits that cap scale, recovery, and migration concurrency until increased or redesigned around.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **quota increase requests**
  - **Technical definition:** quota increase requests are enforceable AWS service limits that cap scale, recovery, and migration concurrency until increased or redesigned around.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **quota request templates**
  - **Technical definition:** quota request templates are enforceable AWS service limits that cap scale, recovery, and migration concurrency until increased or redesigned around.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **usage metrics**
  - **Technical definition:** usage metrics are measured telemetry series or aggregations used to represent resource, platform, or application behavior over time.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

## Controls

### AWS Organizations policy inheritance

- **AWS Organizations policy inheritance**
  - **Technical definition:** AWS Organizations policy inheritance is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **trusted access**
  - **Technical definition:** trusted access is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **delegated administration**
  - **Technical definition:** delegated administration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **consolidated billing**
  - **Technical definition:** consolidated billing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **account contact ownership**
  - **Technical definition:** account contact ownership is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Region opt-in/opt-out**
  - **Technical definition:** Region opt-in/opt-out is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **account metadata governance**
  - **Technical definition:** account metadata governance is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Control Tower account baselines

- **AWS Control Tower account baselines**
  - **Technical definition:** AWS Control Tower account baselines is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **control enablement**
  - **Technical definition:** control enablement is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **drift detection**
  - **Technical definition:** drift detection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **landing-zone versioning**
  - **Technical definition:** landing-zone versioning is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **control remediation**
  - **Technical definition:** control remediation is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### IAM identity-based policies

- **IAM identity-based policies**
  - **Technical definition:** IAM identity-based policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **role assumption**
  - **Technical definition:** role assumption is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **multi-factor authentication (MFA)**
  - **Technical definition:** multi-factor authentication (MFA) is a strong-authentication control used to reduce credential-compromise risk for privileged or federated access into AWS environments.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **attribute-based access control (ABAC)**
  - **Technical definition:** attribute-based access control (ABAC) is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Security Token Service (AWS STS)**
  - **Technical definition:** AWS Security Token Service (AWS STS) is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **IAM Access Analyzer findings**
  - **Technical definition:** IAM Access Analyzer findings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.

### AWS IAM Identity Center workforce identity federation

- **AWS IAM Identity Center workforce identity federation**
  - **Technical definition:** AWS IAM Identity Center workforce identity federation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **permission-set provisioning**
  - **Technical definition:** permission-set provisioning is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **session duration**
  - **Technical definition:** session duration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **account assignment review**
  - **Technical definition:** account assignment review is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Service Catalog launch constraints

- **AWS Service Catalog launch constraints**
  - **Technical definition:** AWS Service Catalog launch constraints is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **template constraints**
  - **Technical definition:** template constraints is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tag update constraints**
  - **Technical definition:** tag update constraints is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **portfolio sharing**
  - **Technical definition:** portfolio sharing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **launch permissions**
  - **Technical definition:** launch permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **AWS Resource Access Manager resource-share permissions**
  - **Technical definition:** AWS Resource Access Manager resource-share permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.

### AWS CloudFormation StackSets service-managed permissions

- **AWS CloudFormation StackSets service-managed permissions**
  - **Technical definition:** AWS CloudFormation StackSets service-managed permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **self-managed permissions**
  - **Technical definition:** self-managed permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **automatic deployments**
  - **Technical definition:** automatic deployments is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **account filters**
  - **Technical definition:** account filters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Region targeting**
  - **Technical definition:** Region targeting is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **failure tolerance**
  - **Technical definition:** failure tolerance is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **concurrency**
  - **Technical definition:** concurrency is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS CloudTrail multi-Region trails

- **AWS CloudTrail multi-Region trails**
  - **Technical definition:** AWS CloudTrail multi-Region trails is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **organization-wide logging**
  - **Technical definition:** organization-wide logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **log file integrity validation**
  - **Technical definition:** log file integrity validation is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **retention**
  - **Technical definition:** retention is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Config organization aggregation**
  - **Technical definition:** AWS Config organization aggregation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### AWS Security Hub delegated administration

- **AWS Security Hub delegated administration**
  - **Technical definition:** AWS Security Hub delegated administration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-Region aggregation**
  - **Technical definition:** cross-Region aggregation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Foundational Security Best Practices**
  - **Technical definition:** AWS Foundational Security Best Practices is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **CIS AWS Foundations Benchmark**
  - **Technical definition:** CIS AWS Foundations Benchmark is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Audit Manager assessment scoping**
  - **Technical definition:** AWS Audit Manager assessment scoping is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### AWS Artifact report access

- **AWS Artifact report access**
  - **Technical definition:** AWS Artifact report access is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **agreement acceptance**
  - **Technical definition:** agreement acceptance is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Health organizational view**
  - **Technical definition:** AWS Health organizational view is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Service Quotas baselines**
  - **Technical definition:** Service Quotas baselines is a capacity-governance control that determines whether AWS environments can scale, fail over, or migrate safely under peak conditions.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **utilization monitoring**
  - **Technical definition:** utilization monitoring is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **quota-risk review**
  - **Technical definition:** quota-risk review is a capacity-governance control that determines whether AWS environments can scale, fail over, or migrate safely under peak conditions.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

## Operations

### Create a landing-zone account model with management account

- **Create a landing-zone account model with management account**
  - **Technical definition:** Create a landing-zone account model with management account is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **shared services accounts**
  - **Technical definition:** shared services accounts is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **workload accounts**
  - **Technical definition:** workload accounts is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **environment-specific OUs**
  - **Technical definition:** environment-specific OUs is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Design an SCP model for preventive guardrails without blocking required platform operations

- **Design an SCP model for preventive guardrails without blocking required platform operations**
  - **Technical definition:** Design an SCP model for preventive guardrails without blocking required platform operations is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Enroll accounts and OUs into AWS Control Tower and review drift remediation paths

- **Enroll accounts and OUs into AWS Control Tower and review drift remediation paths**
  - **Technical definition:** Enroll accounts and OUs into AWS Control Tower and review drift remediation paths is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Review IAM role trust policies

- **Review IAM role trust policies**
  - **Technical definition:** Review IAM role trust policies is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **access keys**
  - **Technical definition:** access keys is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **unused access findings**
  - **Technical definition:** unused access findings is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Provision AWS IAM Identity Center permission sets and validate account assignments

- **Provision AWS IAM Identity Center permission sets and validate account assignments**
  - **Technical definition:** Provision AWS IAM Identity Center permission sets and validate account assignments is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Use AWS CloudFormation StackSets for multi-account baseline deployment and rollback

- **Use AWS CloudFormation StackSets for multi-account baseline deployment and rollback**
  - **Technical definition:** Use AWS CloudFormation StackSets for multi-account baseline deployment and rollback is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Investigate API activity with AWS CloudTrail and validate drift/compliance with AWS Config

- **Investigate API activity with AWS CloudTrail and validate drift/compliance with AWS Config**
  - **Technical definition:** Investigate API activity with AWS CloudTrail and validate drift/compliance with AWS Config is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Triage AWS Security Hub findings and prepare AWS Audit Manager evidence for compliance review

- **Triage AWS Security Hub findings and prepare AWS Audit Manager evidence for compliance review**
  - **Technical definition:** Triage AWS Security Hub findings and prepare AWS Audit Manager evidence for compliance review is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Validate landing-zone readiness before migration by checking identity

- **Validate landing-zone readiness before migration by checking identity**
  - **Technical definition:** Validate landing-zone readiness before migration by checking identity is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **audit**
  - **Technical definition:** audit is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **account ownership**
  - **Technical definition:** account ownership is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **quotas**
  - **Technical definition:** quotas is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **health events**
  - **Technical definition:** health events is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

# AWS Week 2: Networking, Private Connectivity, And Edge Traffic

## Service And Entity Coverage

### Amazon Virtual Private Cloud (Amazon VPC)

- **Amazon Virtual Private Cloud (Amazon VPC)**
  - **Technical definition:** Amazon VPC is AWS's logically isolated software-defined network boundary for compute, routing, segmentation, and private connectivity within a Region.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **VPCs**
  - **Technical definition:** VPCs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **subnets**
  - **Technical definition:** subnets are VPC address subdivisions that scope routing, availability-zone placement, and network-control attachment for AWS resources.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **route tables**
  - **Technical definition:** route tables are routing constructs that determine how traffic or requests are directed between AWS resources, networks, services, or recovery targets.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **internet gateways**
  - **Technical definition:** internet gateways is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **egress-only internet gateways**
  - **Technical definition:** egress-only internet gateways is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **NAT gateways**
  - **Technical definition:** NAT gateways is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **elastic IP addresses**
  - **Technical definition:** elastic IP addresses is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **elastic network interfaces**
  - **Technical definition:** elastic network interfaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **prefix lists**
  - **Technical definition:** prefix lists are core Amazon S3 namespace or data abstractions that define how object data is stored, addressed, secured, and processed.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **DHCP option sets**
  - **Technical definition:** DHCP option sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **security groups**
  - **Technical definition:** security groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **network ACLs**
  - **Technical definition:** network ACLs are stateless subnet-level packet filters that evaluate ordered allow/deny rules for ingress and egress traffic.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPC Flow Logs**
  - **Technical definition:** VPC Flow Logs are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **Amazon VPC Reachability Analyzer**
  - **Technical definition:** Amazon VPC Reachability Analyzer is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **Amazon VPC Network Access Analyzer**
  - **Technical definition:** Amazon VPC Network Access Analyzer is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **Amazon VPC IP Address Manager (IPAM)**
  - **Technical definition:** Amazon VPC IP Address Manager (IPAM) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.

### AWS Transit Gateway and AWS Cloud WAN

- **AWS Transit Gateway and AWS Cloud WAN**
  - **Technical definition:** AWS Transit Gateway and AWS Cloud WAN are AWS network-transit control planes for building centralized routing domains, segmentation, and wide-area connectivity across accounts and Regions.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **transit gateways**
  - **Technical definition:** transit gateways is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **transit gateway attachments**
  - **Technical definition:** transit gateway attachments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **transit gateway route tables**
  - **Technical definition:** transit gateway route tables are routing constructs that determine how traffic or requests are directed between AWS resources, networks, services, or recovery targets.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **route table associations**
  - **Technical definition:** route table associations are routing constructs that determine how traffic or requests are directed between AWS resources, networks, services, or recovery targets.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **route table propagations**
  - **Technical definition:** route table propagations are routing constructs that determine how traffic or requests are directed between AWS resources, networks, services, or recovery targets.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **peering attachments**
  - **Technical definition:** peering attachments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **global networks**
  - **Technical definition:** global networks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **core networks**
  - **Technical definition:** core networks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **core network policies**
  - **Technical definition:** core network policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **segments**
  - **Technical definition:** segments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **attachments**
  - **Technical definition:** attachments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **network function groups**
  - **Technical definition:** network function groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon VPC Peering, Amazon VPC Endpoints, and AWS PrivateLink

- **Amazon VPC Peering, Amazon VPC Endpoints, and AWS PrivateLink**
  - **Technical definition:** Amazon VPC Peering, VPC Endpoints, and AWS PrivateLink are private connectivity mechanisms for east-west VPC communication and private consumption or publication of services without traversing the public internet.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **VPC peering connections**
  - **Technical definition:** VPC peering connections is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **requester VPCs**
  - **Technical definition:** requester VPCs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **accepter VPCs**
  - **Technical definition:** accepter VPCs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **interface endpoints**
  - **Technical definition:** interface endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **gateway endpoints**
  - **Technical definition:** gateway endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **Gateway Load Balancer endpoints**
  - **Technical definition:** Gateway Load Balancer endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **endpoint services**
  - **Technical definition:** endpoint services are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **endpoint network interfaces**
  - **Technical definition:** endpoint network interfaces are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **endpoint policies**
  - **Technical definition:** endpoint policies are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **private DNS names**
  - **Technical definition:** private DNS names is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.

### Amazon VPC Lattice and AWS Verified Access

- **Amazon VPC Lattice and AWS Verified Access**
  - **Technical definition:** Amazon VPC Lattice and AWS Verified Access are application-connectivity and zero-trust access services that shift service exposure and authorization from raw network reachability toward policy-driven access.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **service networks**
  - **Technical definition:** service networks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **services**
  - **Technical definition:** services is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **target groups**
  - **Technical definition:** target groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **listeners**
  - **Technical definition:** listeners are traffic entry definitions that bind ports, protocols, and routing behavior to AWS load-balancing or edge services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rules**
  - **Technical definition:** rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **auth policies**
  - **Technical definition:** auth policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource configurations**
  - **Technical definition:** resource configurations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **Verified Access instances**
  - **Technical definition:** Verified Access instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Verified Access groups**
  - **Technical definition:** Verified Access groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Verified Access endpoints**
  - **Technical definition:** Verified Access endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **trust providers**
  - **Technical definition:** trust providers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access policies**
  - **Technical definition:** access policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **application domains**
  - **Technical definition:** application domains is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Network Firewall and AWS Firewall Manager

- **AWS Network Firewall and AWS Firewall Manager**
  - **Technical definition:** AWS Network Firewall and AWS Firewall Manager provide centralized packet- and domain-level traffic inspection plus organization-wide policy rollout for network security governance.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **firewalls**
  - **Technical definition:** firewalls is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **firewall policies**
  - **Technical definition:** firewall policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rule groups**
  - **Technical definition:** rule groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stateless rules**
  - **Technical definition:** stateless rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stateful rules**
  - **Technical definition:** stateful rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **TLS inspection configurations**
  - **Technical definition:** TLS inspection configurations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **alert logs**
  - **Technical definition:** alert logs are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **flow logs**
  - **Technical definition:** flow logs are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **Firewall Manager policies**
  - **Technical definition:** Firewall Manager policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **policy scopes**
  - **Technical definition:** policy scopes is an AWS policy construct used to constrain, permit, inherit, or validate behavior against a defined authorization or governance boundary.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource sets**
  - **Technical definition:** resource sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **compliance status**
  - **Technical definition:** compliance status is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Elastic Load Balancing and AWS Certificate Manager

- **Elastic Load Balancing and AWS Certificate Manager**
  - **Technical definition:** Elastic Load Balancing and AWS Certificate Manager together provide request distribution, health-aware traffic routing, TLS termination, and certificate lifecycle automation at AWS service edges.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Application Load Balancers**
  - **Technical definition:** Application Load Balancers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Network Load Balancers**
  - **Technical definition:** Network Load Balancers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Gateway Load Balancers**
  - **Technical definition:** Gateway Load Balancers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **target groups**
  - **Technical definition:** target groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **listeners**
  - **Technical definition:** listeners are traffic entry definitions that bind ports, protocols, and routing behavior to AWS load-balancing or edge services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **listener rules**
  - **Technical definition:** listener rules are traffic entry definitions that bind ports, protocols, and routing behavior to AWS load-balancing or edge services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **load balancer attributes**
  - **Technical definition:** load balancer attributes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **public certificates**
  - **Technical definition:** public certificates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **private certificates**
  - **Technical definition:** private certificates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **imported certificates**
  - **Technical definition:** imported certificates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **domain names**
  - **Technical definition:** domain names is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **subject alternative names (SANs)**
  - **Technical definition:** subject alternative names (SANs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **validation options**
  - **Technical definition:** validation options is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Site-to-Site VPN, AWS Client VPN, and AWS Direct Connect

- **AWS Site-to-Site VPN, AWS Client VPN, and AWS Direct Connect**
  - **Technical definition:** AWS Site-to-Site VPN, AWS Client VPN, and AWS Direct Connect are AWS connectivity primitives for hybrid, remote-user, and dedicated private network access into AWS environments.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **customer gateways**
  - **Technical definition:** customer gateways is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **virtual private gateways**
  - **Technical definition:** virtual private gateways is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPN connections**
  - **Technical definition:** VPN connections is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPN tunnels**
  - **Technical definition:** VPN tunnels is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tunnel options**
  - **Technical definition:** tunnel options is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **customer gateway devices**
  - **Technical definition:** customer gateway devices is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Client VPN endpoints**
  - **Technical definition:** Client VPN endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **target network associations**
  - **Technical definition:** target network associations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **authorization rules**
  - **Technical definition:** authorization rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Direct Connect connections**
  - **Technical definition:** Direct Connect connections is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **link aggregation groups (LAGs)**
  - **Technical definition:** link aggregation groups (LAGs) are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **private virtual interfaces**
  - **Technical definition:** private virtual interfaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **public virtual interfaces**
  - **Technical definition:** public virtual interfaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **transit virtual interfaces**
  - **Technical definition:** transit virtual interfaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Direct Connect gateways**
  - **Technical definition:** Direct Connect gateways is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **hosted connections**
  - **Technical definition:** hosted connections is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon Route 53 and Amazon Route 53 Resolver

- **Amazon Route 53 and Amazon Route 53 Resolver**
  - **Technical definition:** Amazon Route 53 and Route 53 Resolver form AWS's authoritative DNS and recursive-hybrid DNS control plane for naming, failover, and conditional forwarding.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **hosted zones**
  - **Technical definition:** hosted zones are DNS namespace, answer, or recursive-resolution constructs that control how names are published and resolved in Route 53 workflows.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **records**
  - **Technical definition:** records are DNS namespace, answer, or recursive-resolution constructs that control how names are published and resolved in Route 53 workflows.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **routing policies**
  - **Technical definition:** routing policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **reusable delegation sets**
  - **Technical definition:** reusable delegation sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **domains**
  - **Technical definition:** domains is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **key-signing keys (KSKs)**
  - **Technical definition:** key-signing keys (KSKs) are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **inbound Resolver endpoints**
  - **Technical definition:** inbound Resolver endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **outbound Resolver endpoints**
  - **Technical definition:** outbound Resolver endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **Resolver rules**
  - **Technical definition:** Resolver rules are DNS namespace, answer, or recursive-resolution constructs that control how names are published and resolved in Route 53 workflows.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rule associations**
  - **Technical definition:** rule associations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **query logging configurations**
  - **Technical definition:** query logging configurations are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **DNS Firewall rule groups**
  - **Technical definition:** DNS Firewall rule groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **domain lists**
  - **Technical definition:** domain lists is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon CloudFront, AWS WAF, AWS Shield, and AWS Global Accelerator

- **Amazon CloudFront, AWS WAF, AWS Shield, and AWS Global Accelerator**
  - **Technical definition:** Amazon CloudFront, AWS WAF, AWS Shield, and AWS Global Accelerator provide AWS edge delivery, request filtering, DDoS protection, and global anycast traffic acceleration.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **distributions**
  - **Technical definition:** distributions are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **origins**
  - **Technical definition:** origins are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cache behaviors**
  - **Technical definition:** cache behaviors are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cache policies**
  - **Technical definition:** cache policies are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **origin request policies**
  - **Technical definition:** origin request policies are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **response headers policies**
  - **Technical definition:** response headers policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **invalidations**
  - **Technical definition:** invalidations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **key groups**
  - **Technical definition:** key groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **origin access controls**
  - **Technical definition:** origin access controls are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **web ACLs**
  - **Technical definition:** web ACLs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rules**
  - **Technical definition:** rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rule groups**
  - **Technical definition:** rule groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **managed rule groups**
  - **Technical definition:** managed rule groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **IP sets**
  - **Technical definition:** IP sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **labels**
  - **Technical definition:** labels is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **protections**
  - **Technical definition:** protections is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **protection groups**
  - **Technical definition:** protection groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **accelerators**
  - **Technical definition:** accelerators is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **listeners**
  - **Technical definition:** listeners are traffic entry definitions that bind ports, protocols, and routing behavior to AWS load-balancing or edge services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoint groups**
  - **Technical definition:** endpoint groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **endpoints**
  - **Technical definition:** endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **static anycast IP addresses**
  - **Technical definition:** static anycast IP addresses is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoint weights**
  - **Technical definition:** endpoint weights are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **traffic dials**
  - **Technical definition:** traffic dials is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon API Gateway

- **Amazon API Gateway**
  - **Technical definition:** Amazon API Gateway is AWS's managed API front-door service for REST, HTTP, and WebSocket APIs with authentication, throttling, deployment staging, and integration orchestration.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **REST APIs**
  - **Technical definition:** REST APIs are API exposure or access-control constructs used to publish, secure, meter, and version application interfaces on AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **HTTP APIs**
  - **Technical definition:** HTTP APIs are API exposure or access-control constructs used to publish, secure, meter, and version application interfaces on AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **WebSocket APIs**
  - **Technical definition:** WebSocket APIs are API exposure or access-control constructs used to publish, secure, meter, and version application interfaces on AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resources**
  - **Technical definition:** resources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **routes**
  - **Technical definition:** routes are routing constructs that determine how traffic or requests are directed between AWS resources, networks, services, or recovery targets.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **methods**
  - **Technical definition:** methods is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **integrations**
  - **Technical definition:** integrations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stages**
  - **Technical definition:** stages is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **deployments**
  - **Technical definition:** deployments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **authorizers**
  - **Technical definition:** authorizers are API exposure or access-control constructs used to publish, secure, meter, and version application interfaces on AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **usage plans**
  - **Technical definition:** usage plans are API exposure or access-control constructs used to publish, secure, meter, and version application interfaces on AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **API keys**
  - **Technical definition:** API keys are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **custom domain names**
  - **Technical definition:** custom domain names is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPC links**
  - **Technical definition:** VPC links is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.

## Controls

### VPC CIDR allocation

- **VPC CIDR allocation**
  - **Technical definition:** VPC CIDR allocation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **subnet routing**
  - **Technical definition:** subnet routing is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **route table associations**
  - **Technical definition:** route table associations is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **security group rules**
  - **Technical definition:** security group rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **network ACL rules**
  - **Technical definition:** network ACL rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **flow log delivery**
  - **Technical definition:** flow log delivery is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **IPAM pool rules**
  - **Technical definition:** IPAM pool rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **overlapping CIDR detection**
  - **Technical definition:** overlapping CIDR detection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Transit Gateway route domains

- **Transit Gateway route domains**
  - **Technical definition:** Transit Gateway route domains is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **attachment acceptance**
  - **Technical definition:** attachment acceptance is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **route propagation**
  - **Technical definition:** route propagation is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **appliance mode**
  - **Technical definition:** appliance mode is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Resource Access Manager sharing**
  - **Technical definition:** AWS Resource Access Manager sharing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Cloud WAN segment isolation**
  - **Technical definition:** Cloud WAN segment isolation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **attachment policies**
  - **Technical definition:** attachment policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **route sharing**
  - **Technical definition:** route sharing is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.

### VPC peering non-overlapping CIDRs

- **VPC peering non-overlapping CIDRs**
  - **Technical definition:** VPC peering non-overlapping CIDRs is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **endpoint policies**
  - **Technical definition:** endpoint policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **private DNS**
  - **Technical definition:** private DNS is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **allowed principals**
  - **Technical definition:** allowed principals is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **acceptance settings**
  - **Technical definition:** acceptance settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **private service exposure boundaries**
  - **Technical definition:** private service exposure boundaries is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### VPC Lattice service network associations

- **VPC Lattice service network associations**
  - **Technical definition:** VPC Lattice service network associations is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **routing rules**
  - **Technical definition:** routing rules is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **TLS settings**
  - **Technical definition:** TLS settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **target group health checks**
  - **Technical definition:** target group health checks is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Verified Access identity trust**
  - **Technical definition:** Verified Access identity trust is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **device trust**
  - **Technical definition:** device trust is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **policy-based access**
  - **Technical definition:** policy-based access is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access log delivery**
  - **Technical definition:** access log delivery is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### AWS Network Firewall centralized inspection

- **AWS Network Firewall centralized inspection**
  - **Technical definition:** AWS Network Firewall centralized inspection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Suricata-compatible rules**
  - **Technical definition:** Suricata-compatible rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **domain list rules**
  - **Technical definition:** domain list rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **default actions**
  - **Technical definition:** default actions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Firewall Manager policies**
  - **Technical definition:** AWS Firewall Manager policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **security group policies**
  - **Technical definition:** security group policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Elastic Load Balancing TLS security policies

- **Elastic Load Balancing TLS security policies**
  - **Technical definition:** Elastic Load Balancing TLS security policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **certificates**
  - **Technical definition:** certificates is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **target group attributes**
  - **Technical definition:** target group attributes is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-zone load balancing**
  - **Technical definition:** cross-zone load balancing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access logs**
  - **Technical definition:** access logs is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **AWS Certificate Manager managed renewal**
  - **Technical definition:** AWS Certificate Manager managed renewal is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### VPN IKE/IPsec parameters

- **VPN IKE/IPsec parameters**
  - **Technical definition:** VPN IKE/IPsec parameters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **BGP routing**
  - **Technical definition:** BGP routing is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tunnel inside CIDRs**
  - **Technical definition:** tunnel inside CIDRs is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **pre-shared keys**
  - **Technical definition:** pre-shared keys is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Client VPN authorization rules**
  - **Technical definition:** Client VPN authorization rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Direct Connect BGP authentication**
  - **Technical definition:** Direct Connect BGP authentication is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **allowed prefixes**
  - **Technical definition:** allowed prefixes is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **MACsec**
  - **Technical definition:** MACsec is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resiliency model**
  - **Technical definition:** resiliency model is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Route 53 TTLs

- **Route 53 TTLs**
  - **Technical definition:** Route 53 TTLs is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **routing policies**
  - **Technical definition:** routing policies is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **health-check routing**
  - **Technical definition:** health-check routing is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **DNSSEC signing**
  - **Technical definition:** DNSSEC signing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **delegation**
  - **Technical definition:** delegation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Route 53 Resolver conditional forwarding**
  - **Technical definition:** Route 53 Resolver conditional forwarding is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **DNS Firewall rules**
  - **Technical definition:** DNS Firewall rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **query logging**
  - **Technical definition:** query logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### CloudFront viewer protocol policies

- **CloudFront viewer protocol policies**
  - **Technical definition:** CloudFront viewer protocol policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **signed URLs**
  - **Technical definition:** signed URLs is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **signed cookies**
  - **Technical definition:** signed cookies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **origin access control**
  - **Technical definition:** origin access control is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS WAF rate-based rules**
  - **Technical definition:** AWS WAF rate-based rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Bot Control**
  - **Technical definition:** Bot Control is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Shield Advanced**
  - **Technical definition:** AWS Shield Advanced is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Global Accelerator traffic dials**
  - **Technical definition:** Global Accelerator traffic dials is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoint weights**
  - **Technical definition:** endpoint weights is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **health checks**
  - **Technical definition:** health checks is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### API Gateway authorizers

- **API Gateway authorizers**
  - **Technical definition:** API Gateway authorizers is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource policies**
  - **Technical definition:** resource policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **throttling**
  - **Technical definition:** throttling is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **mutual TLS**
  - **Technical definition:** mutual TLS is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stage variables**
  - **Technical definition:** stage variables is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **request validation**
  - **Technical definition:** request validation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS WAF associations**
  - **Technical definition:** AWS WAF associations is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

## Operations

### Validate VPC packet paths with route tables

- **Validate VPC packet paths with route tables**
  - **Technical definition:** Validate VPC packet paths with route tables is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **security groups**
  - **Technical definition:** security groups is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **network ACLs**
  - **Technical definition:** network ACLs is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Reachability Analyzer**
  - **Technical definition:** Reachability Analyzer is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Network Access Analyzer**
  - **Technical definition:** Network Access Analyzer is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Review NAT gateway saturation

- **Review NAT gateway saturation**
  - **Technical definition:** Review NAT gateway saturation is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **IPAM allocation**
  - **Technical definition:** IPAM allocation is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **route drift**
  - **Technical definition:** route drift is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **overlapping CIDRs before migration**
  - **Technical definition:** overlapping CIDRs before migration is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Onboard Transit Gateway attachments

- **Onboard Transit Gateway attachments**
  - **Technical definition:** Onboard Transit Gateway attachments is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate route propagation**
  - **Technical definition:** validate route propagation is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **test segmentation between network domains**
  - **Technical definition:** test segmentation between network domains is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Publish or consume private services through VPC endpoints and AWS PrivateLink without internet exposure

- **Publish or consume private services through VPC endpoints and AWS PrivateLink without internet exposure**
  - **Technical definition:** Publish or consume private services through VPC endpoints and AWS PrivateLink without internet exposure is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Investigate dropped flows in AWS Network Firewall and tune rule groups safely

- **Investigate dropped flows in AWS Network Firewall and tune rule groups safely**
  - **Technical definition:** Investigate dropped flows in AWS Network Firewall and tune rule groups safely is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Review Elastic Load Balancing target health

- **Review Elastic Load Balancing target health**
  - **Technical definition:** Review Elastic Load Balancing target health is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **listener rules**
  - **Technical definition:** listener rules is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **TLS certificates**
  - **Technical definition:** TLS certificates is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **traffic shifting**
  - **Technical definition:** traffic shifting is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Test Site-to-Site VPN tunnel failover and Direct Connect backup routing

- **Test Site-to-Site VPN tunnel failover and Direct Connect backup routing**
  - **Technical definition:** Test Site-to-Site VPN tunnel failover and Direct Connect backup routing is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Execute DNS cutover with Route 53 records

- **Execute DNS cutover with Route 53 records**
  - **Technical definition:** Execute DNS cutover with Route 53 records is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **health checks**
  - **Technical definition:** health checks is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Resolver rules**
  - **Technical definition:** Resolver rules is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **rollback TTL planning**
  - **Technical definition:** rollback TTL planning is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Tune CloudFront cache behavior

- **Tune CloudFront cache behavior**
  - **Technical definition:** Tune CloudFront cache behavior is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **origin failover**
  - **Technical definition:** origin failover is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **AWS WAF false positives**
  - **Technical definition:** AWS WAF false positives is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Global Accelerator failover**
  - **Technical definition:** Global Accelerator failover is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Deploy and troubleshoot API Gateway stages

- **Deploy and troubleshoot API Gateway stages**
  - **Technical definition:** Deploy and troubleshoot API Gateway stages is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **private integrations**
  - **Technical definition:** private integrations is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **throttling**
  - **Technical definition:** throttling is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **access logs**
  - **Technical definition:** access logs is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **canary releases**
  - **Technical definition:** canary releases is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

# AWS Week 3: Compute, Kubernetes, CI/CD, And Platform Delivery

## Service And Entity Coverage

### Amazon Elastic Compute Cloud (Amazon EC2) and Amazon EC2 Auto Scaling

- **Amazon Elastic Compute Cloud (Amazon EC2) and Amazon EC2 Auto Scaling**
  - **Technical definition:** Amazon EC2 and EC2 Auto Scaling provide virtual machine compute plus policy-driven fleet scaling, refresh, and lifecycle orchestration for instance-based workloads.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **instances**
  - **Technical definition:** instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Amazon Machine Images (AMIs)**
  - **Technical definition:** Amazon Machine Images (AMIs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **instance types**
  - **Technical definition:** instance types are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **key pairs**
  - **Technical definition:** key pairs are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **elastic network interfaces**
  - **Technical definition:** elastic network interfaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **instance profiles**
  - **Technical definition:** instance profiles are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **user data**
  - **Technical definition:** user data are human or service identities represented directly in an AWS identity system and evaluated against attached or inherited access policy.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Elastic IP addresses**
  - **Technical definition:** Elastic IP addresses is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **EC2 Image Builder image pipelines**
  - **Technical definition:** EC2 Image Builder image pipelines are ordered execution graphs that transform source state into build, deploy, data-move, or ML outcomes under managed orchestration.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **Auto Scaling groups**
  - **Technical definition:** Auto Scaling groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **launch templates**
  - **Technical definition:** launch templates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **scaling policies**
  - **Technical definition:** scaling policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **scheduled actions**
  - **Technical definition:** scheduled actions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lifecycle hooks**
  - **Technical definition:** lifecycle hooks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **warm pools**
  - **Technical definition:** warm pools is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **instance refreshes**
  - **Technical definition:** instance refreshes are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon Elastic Container Registry (Amazon ECR)

- **Amazon Elastic Container Registry (Amazon ECR)**
  - **Technical definition:** Amazon ECR is AWS's OCI-compatible container registry for image storage, lifecycle management, replication, and pull-time distribution control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **private repositories**
  - **Technical definition:** private repositories is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **public repositories**
  - **Technical definition:** public repositories is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **images**
  - **Technical definition:** images is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tags**
  - **Technical definition:** tags is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **manifests**
  - **Technical definition:** manifests is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lifecycle policies**
  - **Technical definition:** lifecycle policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **pull through cache rules**
  - **Technical definition:** pull through cache rules are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **replication configurations**
  - **Technical definition:** replication configurations are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### Amazon Elastic Container Service (Amazon ECS) and AWS Fargate

- **Amazon Elastic Container Service (Amazon ECS) and AWS Fargate**
  - **Technical definition:** Amazon ECS and AWS Fargate provide AWS-native container scheduling and serverless container execution without requiring direct node fleet management for all workloads.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **clusters**
  - **Technical definition:** clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **services**
  - **Technical definition:** services is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **task definitions**
  - **Technical definition:** task definitions are discrete executable or replicative work units scheduled or managed by an AWS service control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tasks**
  - **Technical definition:** tasks are discrete executable or replicative work units scheduled or managed by an AWS service control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **capacity providers**
  - **Technical definition:** capacity providers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **container instances**
  - **Technical definition:** container instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **service deployments**
  - **Technical definition:** service deployments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **task sets**
  - **Technical definition:** task sets are discrete executable or replicative work units scheduled or managed by an AWS service control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Amazon ECS tasks on AWS Fargate**
  - **Technical definition:** Amazon ECS tasks on AWS Fargate are discrete executable or replicative work units scheduled or managed by an AWS service control plane.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **Amazon ECS services on AWS Fargate**
  - **Technical definition:** Amazon ECS services on AWS Fargate is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.

### Amazon Elastic Kubernetes Service (Amazon EKS) and Karpenter

- **Amazon Elastic Kubernetes Service (Amazon EKS) and Karpenter**
  - **Technical definition:** Amazon EKS and Karpenter provide managed Kubernetes control planes and dynamic node provisioning for pod-driven capacity allocation in AWS.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **clusters**
  - **Technical definition:** clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **control planes**
  - **Technical definition:** control planes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **managed node groups**
  - **Technical definition:** managed node groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **self-managed nodes**
  - **Technical definition:** self-managed nodes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Fargate profiles**
  - **Technical definition:** Fargate profiles is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Amazon EKS pods on AWS Fargate**
  - **Technical definition:** Amazon EKS pods on AWS Fargate is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **add-ons**
  - **Technical definition:** add-ons is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access entries**
  - **Technical definition:** access entries is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Kubernetes service accounts**
  - **Technical definition:** Kubernetes service accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **pod identity associations**
  - **Technical definition:** pod identity associations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **Karpenter NodePool resources**
  - **Technical definition:** Karpenter NodePool resources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **EC2NodeClass resources**
  - **Technical definition:** EC2NodeClass resources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **NodeClaim resources**
  - **Technical definition:** NodeClaim resources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Lambda

- **AWS Lambda**
  - **Technical definition:** AWS Lambda is AWS's event-driven serverless compute runtime that executes functions on demand with managed scaling and per-request billing.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **functions**
  - **Technical definition:** functions are serverless code execution units whose scaling, event handling, and concurrency behavior are managed by AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **versions**
  - **Technical definition:** versions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **aliases**
  - **Technical definition:** aliases is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **layers**
  - **Technical definition:** layers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **event source mappings**
  - **Technical definition:** event source mappings is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **function URLs**
  - **Technical definition:** function URLs are serverless code execution units whose scaling, event handling, and concurrency behavior are managed by AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **execution roles**
  - **Technical definition:** execution roles is an AWS identity principal or trust construct used to obtain temporary permissions through assumption rather than long-lived static credentials.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **provisioned concurrency**
  - **Technical definition:** provisioned concurrency is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **reserved concurrency**
  - **Technical definition:** reserved concurrency is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS App Runner, AWS Elastic Beanstalk, and Amazon Lightsail

- **AWS App Runner, AWS Elastic Beanstalk, and Amazon Lightsail**
  - **Technical definition:** AWS App Runner, Elastic Beanstalk, and Lightsail are higher-level application hosting services that trade off control for faster app deployment and simpler operational models.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **App Runner services**
  - **Technical definition:** App Runner services is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **source configurations**
  - **Technical definition:** source configurations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **VPC connectors**
  - **Technical definition:** VPC connectors is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **custom domains**
  - **Technical definition:** custom domains is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Elastic Beanstalk applications**
  - **Technical definition:** Elastic Beanstalk applications is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **application versions**
  - **Technical definition:** application versions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **environments**
  - **Technical definition:** environments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **platforms**
  - **Technical definition:** platforms is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **deployment policies**
  - **Technical definition:** deployment policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Lightsail instances**
  - **Technical definition:** Lightsail instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **blueprints**
  - **Technical definition:** blueprints is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **bundles**
  - **Technical definition:** bundles is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **static IPs**
  - **Technical definition:** static IPs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **snapshots**
  - **Technical definition:** snapshots are point-in-time persisted state captures used for backup, copy, restore, and migration workflows across AWS storage or database services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **container services**
  - **Technical definition:** container services is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Batch

- **AWS Batch**
  - **Technical definition:** AWS Batch is AWS's batch scheduling and compute orchestration service for containerized or EC2-backed batch jobs with queueing and placement policies.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **jobs**
  - **Technical definition:** jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **job queues**
  - **Technical definition:** job queues is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **job definitions**
  - **Technical definition:** job definitions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **compute environments**
  - **Technical definition:** compute environments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **scheduling policies**
  - **Technical definition:** scheduling policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **array jobs**
  - **Technical definition:** array jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **multi-node parallel jobs**
  - **Technical definition:** multi-node parallel jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **fair share identifiers**
  - **Technical definition:** fair share identifiers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS CodeBuild, AWS CodePipeline, AWS CodeDeploy, and AWS CodeArtifact

- **AWS CodeBuild, AWS CodePipeline, AWS CodeDeploy, and AWS CodeArtifact**
  - **Technical definition:** AWS CodeBuild, CodePipeline, CodeDeploy, and CodeArtifact form AWS's build, release, deployment, and package-distribution toolchain for CI/CD workflows.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **build projects**
  - **Technical definition:** build projects is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **buildspec files**
  - **Technical definition:** buildspec files is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **reports**
  - **Technical definition:** reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **caches**
  - **Technical definition:** caches are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **pipelines**
  - **Technical definition:** pipelines are ordered execution graphs that transform source state into build, deploy, data-move, or ML outcomes under managed orchestration.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stages**
  - **Technical definition:** stages is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **actions**
  - **Technical definition:** actions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **source actions**
  - **Technical definition:** source actions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS CodeConnections connections (CodeStar connections)**
  - **Technical definition:** AWS CodeConnections connections (CodeStar connections) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **applications**
  - **Technical definition:** applications is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **deployment groups**
  - **Technical definition:** deployment groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **deployment configurations**
  - **Technical definition:** deployment configurations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **AppSpec files**
  - **Technical definition:** AppSpec files is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **domains**
  - **Technical definition:** domains is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **repositories**
  - **Technical definition:** repositories is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **packages**
  - **Technical definition:** packages is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **package versions**
  - **Technical definition:** package versions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **external connections**
  - **Technical definition:** external connections is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS CloudFormation, AWS Cloud Development Kit (AWS CDK), and AWS Proton

- **AWS CloudFormation, AWS Cloud Development Kit (AWS CDK), and AWS Proton**
  - **Technical definition:** AWS CloudFormation, AWS CDK, and AWS Proton provide infrastructure and platform delivery abstractions ranging from declarative stacks to higher-level service templates.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stacks**
  - **Technical definition:** stacks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stack sets**
  - **Technical definition:** stack sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **templates**
  - **Technical definition:** templates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **change sets**
  - **Technical definition:** change sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **nested stacks**
  - **Technical definition:** nested stacks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS SAM templates**
  - **Technical definition:** AWS SAM templates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS::Serverless resources (serverless resources)**
  - **Technical definition:** AWS::Serverless resources (serverless resources) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **CDK apps**
  - **Technical definition:** CDK apps is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **constructs**
  - **Technical definition:** constructs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **assets**
  - **Technical definition:** assets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **synthesized CloudFormation templates**
  - **Technical definition:** synthesized CloudFormation templates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Proton environment templates**
  - **Technical definition:** Proton environment templates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **service templates**
  - **Technical definition:** service templates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **environments**
  - **Technical definition:** environments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **services**
  - **Technical definition:** services is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **service instances**
  - **Technical definition:** service instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **components**
  - **Technical definition:** components is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon SageMaker AI (Amazon SageMaker)

- **Amazon SageMaker AI (Amazon SageMaker)**
  - **Technical definition:** Amazon SageMaker is AWS's managed ML platform for training, processing, feature engineering, model registry, deployment, and monitoring workflows.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **domains (Studio domains)**
  - **Technical definition:** domains (Studio domains) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **user profiles**
  - **Technical definition:** user profiles are human or service identities represented directly in an AWS identity system and evaluated against attached or inherited access policy.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **spaces**
  - **Technical definition:** spaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **training jobs**
  - **Technical definition:** training jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **processing jobs**
  - **Technical definition:** processing jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **feature groups**
  - **Technical definition:** feature groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **data flows**
  - **Technical definition:** data flows is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **models**
  - **Technical definition:** models is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoint configurations**
  - **Technical definition:** endpoint configurations are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **endpoints**
  - **Technical definition:** endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **pipelines**
  - **Technical definition:** pipelines are ordered execution graphs that transform source state into build, deploy, data-move, or ML outcomes under managed orchestration.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **model package groups**
  - **Technical definition:** model package groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **model packages**
  - **Technical definition:** model packages is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **monitoring schedules**
  - **Technical definition:** monitoring schedules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **baselines**
  - **Technical definition:** baselines is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **constraints**
  - **Technical definition:** constraints is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **experiments**
  - **Technical definition:** experiments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **trials**
  - **Technical definition:** trials is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **trial components**
  - **Technical definition:** trial components is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lineage artifacts**
  - **Technical definition:** lineage artifacts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lineage actions**
  - **Technical definition:** lineage actions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lineage contexts**
  - **Technical definition:** lineage contexts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon Bedrock

- **Amazon Bedrock**
  - **Technical definition:** Amazon Bedrock is AWS's managed foundation-model access and generative AI platform for model invocation, guarded integration, and higher-level application building.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **foundation models**
  - **Technical definition:** foundation models is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **inference profiles**
  - **Technical definition:** inference profiles is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **provisioned throughput**
  - **Technical definition:** provisioned throughput is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **agents**
  - **Technical definition:** agents is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **knowledge bases**
  - **Technical definition:** knowledge bases is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **guardrails**
  - **Technical definition:** guardrails is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **model customization jobs**
  - **Technical definition:** model customization jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **model evaluation jobs**
  - **Technical definition:** model evaluation jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **invocation logs**
  - **Technical definition:** invocation logs are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

## Controls

### EC2 instance metadata options

- **EC2 instance metadata options**
  - **Technical definition:** EC2 instance metadata options is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **IAM instance profiles**
  - **Technical definition:** IAM instance profiles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **security group rules**
  - **Technical definition:** security group rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **termination protection**
  - **Technical definition:** termination protection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Auto Scaling desired/min/max capacity**
  - **Technical definition:** Auto Scaling desired/min/max capacity is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **health check type**
  - **Technical definition:** health check type is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **termination policies**
  - **Technical definition:** termination policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **mixed instances policy**
  - **Technical definition:** mixed instances policy is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **instance refresh preferences**
  - **Technical definition:** instance refresh preferences is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon ECR repository policies

- **Amazon ECR repository policies**
  - **Technical definition:** Amazon ECR repository policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **image scanning**
  - **Technical definition:** image scanning is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tag immutability**
  - **Technical definition:** tag immutability is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-Region replication**
  - **Technical definition:** cross-Region replication is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.

### Amazon ECS task IAM roles

- **Amazon ECS task IAM roles**
  - **Technical definition:** Amazon ECS task IAM roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **execution roles**
  - **Technical definition:** execution roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **deployment configuration**
  - **Technical definition:** deployment configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **service discovery**
  - **Technical definition:** service discovery is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **load balancer integration**
  - **Technical definition:** load balancer integration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **capacity provider strategy**
  - **Technical definition:** capacity provider strategy is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Fargate task sizing**
  - **Technical definition:** Fargate task sizing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon EKS endpoint access

- **Amazon EKS endpoint access**
  - **Technical definition:** Amazon EKS endpoint access is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **cluster access management**
  - **Technical definition:** cluster access management is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **managed add-ons**
  - **Technical definition:** managed add-ons is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **IAM roles for service accounts (IRSA)**
  - **Technical definition:** IAM roles for service accounts (IRSA) is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **EKS Pod Identity**
  - **Technical definition:** EKS Pod Identity is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **security groups for pods**
  - **Technical definition:** security groups for pods is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **upgrade policies**
  - **Technical definition:** upgrade policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Karpenter disruption controls**
  - **Technical definition:** Karpenter disruption controls is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Fargate profile placement**
  - **Technical definition:** Fargate profile placement is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Lambda execution roles

- **AWS Lambda execution roles**
  - **Technical definition:** AWS Lambda execution roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **environment variables**
  - **Technical definition:** environment variables is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **concurrency limits**
  - **Technical definition:** concurrency limits is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **code signing**
  - **Technical definition:** code signing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### App Runner deployment settings

- **App Runner deployment settings**
  - **Technical definition:** App Runner deployment settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Elastic Beanstalk platform versions**
  - **Technical definition:** Elastic Beanstalk platform versions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Lightsail firewall rules**
  - **Technical definition:** Lightsail firewall rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **health checks**
  - **Technical definition:** health checks is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPC egress**
  - **Technical definition:** VPC egress is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **access roles**
  - **Technical definition:** access roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **instance profiles**
  - **Technical definition:** instance profiles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **snapshots**
  - **Technical definition:** snapshots is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Batch compute environment type

- **AWS Batch compute environment type**
  - **Technical definition:** AWS Batch compute environment type is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **allocation strategy**
  - **Technical definition:** allocation strategy is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **job priority**
  - **Technical definition:** job priority is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **retry strategy**
  - **Technical definition:** retry strategy is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **timeout settings**
  - **Technical definition:** timeout settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.

### Developer tool service roles

- **Developer tool service roles**
  - **Technical definition:** Developer tool service roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **environment images**
  - **Technical definition:** environment images is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **artifact encryption**
  - **Technical definition:** artifact encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **approval actions**
  - **Technical definition:** approval actions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stage transitions**
  - **Technical definition:** stage transitions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-account actions**
  - **Technical definition:** cross-account actions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **automatic rollback**
  - **Technical definition:** automatic rollback is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **alarms**
  - **Technical definition:** alarms is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **repository policies**
  - **Technical definition:** repository policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **package origin controls**
  - **Technical definition:** package origin controls is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### CloudFormation stack policies

- **CloudFormation stack policies**
  - **Technical definition:** CloudFormation stack policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rollback triggers**
  - **Technical definition:** rollback triggers is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **termination protection**
  - **Technical definition:** termination protection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **capabilities**
  - **Technical definition:** capabilities is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **hooks**
  - **Technical definition:** hooks is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **service roles**
  - **Technical definition:** service roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **AWS SAM deployment preferences**
  - **Technical definition:** AWS SAM deployment preferences is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **CDK bootstrap stacks**
  - **Technical definition:** CDK bootstrap stacks is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **synthesizers**
  - **Technical definition:** synthesizers is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **permissions boundaries**
  - **Technical definition:** permissions boundaries is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **Proton template versions**
  - **Technical definition:** Proton template versions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **deployment workflows**
  - **Technical definition:** deployment workflows is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### SageMaker execution roles

- **SageMaker execution roles**
  - **Technical definition:** SageMaker execution roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **network isolation**
  - **Technical definition:** network isolation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPC configuration**
  - **Technical definition:** VPC configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **model approval status**
  - **Technical definition:** model approval status is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoint deployment configuration**
  - **Technical definition:** endpoint deployment configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **data capture**
  - **Technical definition:** data capture is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **bias metrics**
  - **Technical definition:** bias metrics is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lineage metadata**
  - **Technical definition:** lineage metadata is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **artifact encryption**
  - **Technical definition:** artifact encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access permissions**
  - **Technical definition:** access permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.

### Bedrock model access

- **Bedrock model access**
  - **Technical definition:** Bedrock model access is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **guardrails**
  - **Technical definition:** guardrails is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **IAM permissions**
  - **Technical definition:** IAM permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **VPC endpoints**
  - **Technical definition:** VPC endpoints is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **invocation logging**
  - **Technical definition:** invocation logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

## Operations

### Build and roll back golden AMIs

- **Build and roll back golden AMIs**
  - **Technical definition:** Build and roll back golden AMIs is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review EC2 status checks**
  - **Technical definition:** review EC2 status checks is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **replace unhealthy instances**
  - **Technical definition:** replace unhealthy instances is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate Auto Scaling instance refreshes**
  - **Technical definition:** validate Auto Scaling instance refreshes is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Push and promote container images in Amazon ECR while reviewing image scan findings and lifecycle cleanup

- **Push and promote container images in Amazon ECR while reviewing image scan findings and lifecycle cleanup**
  - **Technical definition:** Push and promote container images in Amazon ECR while reviewing image scan findings and lifecycle cleanup is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Deploy ECS services with rolling or blue/green patterns and troubleshoot failed tasks

- **Deploy ECS services with rolling or blue/green patterns and troubleshoot failed tasks**
  - **Technical definition:** Deploy ECS services with rolling or blue/green patterns and troubleshoot failed tasks is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **capacity providers**
  - **Technical definition:** capacity providers is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **target registration**
  - **Technical definition:** target registration is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Upgrade EKS clusters

- **Upgrade EKS clusters**
  - **Technical definition:** Upgrade EKS clusters is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **managed node groups**
  - **Technical definition:** managed node groups is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **add-ons**
  - **Technical definition:** add-ons is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review pod scheduling**
  - **Technical definition:** review pod scheduling is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **IRSA**
  - **Technical definition:** IRSA is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **EKS Pod Identity**
  - **Technical definition:** EKS Pod Identity is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Karpenter scale-out behavior**
  - **Technical definition:** Karpenter scale-out behavior is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Deploy Lambda versions and aliases

- **Deploy Lambda versions and aliases**
  - **Technical definition:** Deploy Lambda versions and aliases is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **investigate cold starts**
  - **Technical definition:** investigate cold starts is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **throttling**
  - **Technical definition:** throttling is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **retries**
  - **Technical definition:** retries is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **event source mapping failures**
  - **Technical definition:** event source mapping failures is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Use CodeBuild

- **Use CodeBuild**
  - **Technical definition:** Use CodeBuild is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **CodePipeline**
  - **Technical definition:** CodePipeline is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **CodeDeploy**
  - **Technical definition:** CodeDeploy is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **CodeArtifact for build**
  - **Technical definition:** CodeArtifact for build is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **approval**
  - **Technical definition:** approval is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **deployment**
  - **Technical definition:** deployment is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **rollback**
  - **Technical definition:** rollback is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **artifact promotion workflows**
  - **Technical definition:** artifact promotion workflows is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Review CloudFormation change sets

- **Review CloudFormation change sets**
  - **Technical definition:** Review CloudFormation change sets is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **drift**
  - **Technical definition:** drift is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **stack rollback**
  - **Technical definition:** stack rollback is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **CDK diffs**
  - **Technical definition:** CDK diffs is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **SAM serverless deployment**
  - **Technical definition:** SAM serverless deployment is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Proton template rollout**
  - **Technical definition:** Proton template rollout is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Run SageMaker training

- **Run SageMaker training**
  - **Technical definition:** Run SageMaker training is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **processing**
  - **Technical definition:** processing is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **pipelines**
  - **Technical definition:** pipelines is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **model registration**
  - **Technical definition:** model registration is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **endpoint deployment**
  - **Technical definition:** endpoint deployment is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **monitoring review**
  - **Technical definition:** monitoring review is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **inference troubleshooting**
  - **Technical definition:** inference troubleshooting is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Invoke Bedrock models

- **Invoke Bedrock models**
  - **Technical definition:** Invoke Bedrock models is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **test prompts**
  - **Technical definition:** test prompts is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate guardrails**
  - **Technical definition:** validate guardrails is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **synchronize knowledge bases**
  - **Technical definition:** synchronize knowledge bases is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **deploy agents**
  - **Technical definition:** deploy agents is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review model evaluations**
  - **Technical definition:** review model evaluations is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

# AWS Week 4: Data, Storage, Backup, And Stateful Reliability

## Service And Entity Coverage

### Amazon Simple Storage Service (Amazon S3)

- **Amazon Simple Storage Service (Amazon S3)**
  - **Technical definition:** Amazon S3 is AWS's object storage system for durable, regionally distributed object persistence with policy, lifecycle, eventing, and large-scale data access patterns.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **buckets**
  - **Technical definition:** buckets are core Amazon S3 namespace or data abstractions that define how object data is stored, addressed, secured, and processed.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **objects**
  - **Technical definition:** objects are core Amazon S3 namespace or data abstractions that define how object data is stored, addressed, secured, and processed.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **prefixes**
  - **Technical definition:** prefixes are core Amazon S3 namespace or data abstractions that define how object data is stored, addressed, secured, and processed.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access points**
  - **Technical definition:** access points is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Multi-Region Access Points**
  - **Technical definition:** Multi-Region Access Points is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **bucket policies**
  - **Technical definition:** bucket policies are core Amazon S3 namespace or data abstractions that define how object data is stored, addressed, secured, and processed.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **S3 Block Public Access settings**
  - **Technical definition:** S3 Block Public Access settings is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **S3 Object Lock configurations**
  - **Technical definition:** S3 Object Lock configurations are core Amazon S3 namespace or data abstractions that define how object data is stored, addressed, secured, and processed.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **lifecycle configurations**
  - **Technical definition:** lifecycle configurations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **replication rules**
  - **Technical definition:** replication rules are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **storage classes**
  - **Technical definition:** storage classes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon Elastic Block Store (Amazon EBS) and Amazon Elastic File System (Amazon EFS)

- **Amazon Elastic Block Store (Amazon EBS) and Amazon Elastic File System (Amazon EFS)**
  - **Technical definition:** Amazon EBS and Amazon EFS are AWS block and elastic file storage services for instance-attached persistent volumes and shared POSIX file access respectively.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **EBS volumes**
  - **Technical definition:** EBS volumes are persistent storage units with defined durability, performance, and attachment behavior used by compute or file services in AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **snapshots**
  - **Technical definition:** snapshots are point-in-time persisted state captures used for backup, copy, restore, and migration workflows across AWS storage or database services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **snapshot archives**
  - **Technical definition:** snapshot archives are point-in-time persisted state captures used for backup, copy, restore, and migration workflows across AWS storage or database services.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **volume types**
  - **Technical definition:** volume types are persistent storage units with defined durability, performance, and attachment behavior used by compute or file services in AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **provisioned IOPS**
  - **Technical definition:** provisioned IOPS is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **throughput settings**
  - **Technical definition:** throughput settings is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Fast Snapshot Restore**
  - **Technical definition:** Fast Snapshot Restore are point-in-time persisted state captures used for backup, copy, restore, and migration workflows across AWS storage or database services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **EFS file systems**
  - **Technical definition:** EFS file systems is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **mount targets**
  - **Technical definition:** mount targets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access points**
  - **Technical definition:** access points is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lifecycle policies**
  - **Technical definition:** lifecycle policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **throughput modes**
  - **Technical definition:** throughput modes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **performance modes**
  - **Technical definition:** performance modes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backups**
  - **Technical definition:** backups are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **replication configurations**
  - **Technical definition:** replication configurations are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### Amazon FSx

- **Amazon FSx**
  - **Technical definition:** Amazon FSx is AWS's managed family of specialized file-system offerings for Windows, Lustre, NetApp ONTAP, and OpenZFS-style workloads.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **file systems**
  - **Technical definition:** file systems is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backups**
  - **Technical definition:** backups are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **storage virtual machines**
  - **Technical definition:** storage virtual machines is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **volumes**
  - **Technical definition:** volumes are persistent storage units with defined durability, performance, and attachment behavior used by compute or file services in AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **shares**
  - **Technical definition:** shares is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoints**
  - **Technical definition:** endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **deployment types**
  - **Technical definition:** deployment types is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **throughput capacity**
  - **Technical definition:** throughput capacity is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon Relational Database Service (Amazon RDS) and Amazon Aurora

- **Amazon Relational Database Service (Amazon RDS) and Amazon Aurora**
  - **Technical definition:** Amazon RDS and Amazon Aurora are AWS managed relational database platforms that abstract patching, backup, failover, and provisioning while exposing engine-specific data behavior.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **DB instances**
  - **Technical definition:** DB instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **DB clusters**
  - **Technical definition:** DB clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **DB snapshots**
  - **Technical definition:** DB snapshots are point-in-time persisted state captures used for backup, copy, restore, and migration workflows across AWS storage or database services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **automated backups**
  - **Technical definition:** automated backups are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **parameter groups**
  - **Technical definition:** parameter groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **option groups**
  - **Technical definition:** option groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **subnet groups**
  - **Technical definition:** subnet groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **read replicas**
  - **Technical definition:** read replicas are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **reserved DB instances**
  - **Technical definition:** reserved DB instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Aurora Replicas**
  - **Technical definition:** Aurora Replicas are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **cluster endpoints**
  - **Technical definition:** cluster endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **reader endpoints**
  - **Technical definition:** reader endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **custom endpoints**
  - **Technical definition:** custom endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **global databases**
  - **Technical definition:** global databases is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backtrack windows**
  - **Technical definition:** backtrack windows is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon DynamoDB

- **Amazon DynamoDB**
  - **Technical definition:** Amazon DynamoDB is AWS's serverless key-value and document database designed for low-latency access, partition-based horizontal scale, and policy-controlled capacity behavior.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **tables**
  - **Technical definition:** tables are logical state containers that define access patterns, indexing, throughput, and replication semantics for database workloads.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **items**
  - **Technical definition:** items is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **attributes**
  - **Technical definition:** attributes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **partition keys**
  - **Technical definition:** partition keys is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **sort keys**
  - **Technical definition:** sort keys is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **local secondary indexes**
  - **Technical definition:** local secondary indexes are alternate access structures that optimize specific query patterns at the cost of additional write, storage, or consistency overhead.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **global secondary indexes**
  - **Technical definition:** global secondary indexes are alternate access structures that optimize specific query patterns at the cost of additional write, storage, or consistency overhead.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **streams**
  - **Technical definition:** streams are ordered or near-real-time change or telemetry feeds used for downstream processing, replication, observability, or event-driven design.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **global tables**
  - **Technical definition:** global tables are logical state containers that define access patterns, indexing, throughput, and replication semantics for database workloads.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backups**
  - **Technical definition:** backups are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **time to live (TTL)**
  - **Technical definition:** time to live (TTL) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon DocumentDB, Amazon Neptune, Amazon Timestream for LiveAnalytics, Amazon ElastiCache, and Amazon MemoryDB

- **Amazon DocumentDB, Amazon Neptune, Amazon Timestream for LiveAnalytics, Amazon ElastiCache, and Amazon MemoryDB**
  - **Technical definition:** Amazon DocumentDB, Amazon Neptune, Amazon Timestream for LiveAnalytics, Amazon ElastiCache, and Amazon MemoryDB are ordered or near-real-time change or telemetry feeds used for downstream processing, replication, observability, or event-driven design.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **DocumentDB clusters**
  - **Technical definition:** DocumentDB clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Neptune DB clusters**
  - **Technical definition:** Neptune DB clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Timestream databases and tables**
  - **Technical definition:** Timestream databases and tables are logical state containers that define access patterns, indexing, throughput, and replication semantics for database workloads.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **ElastiCache serverless caches**
  - **Technical definition:** ElastiCache serverless caches are content-delivery constructs that shape edge routing, cache behavior, request forwarding, and origin protection in CloudFront.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **replication groups**
  - **Technical definition:** replication groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **shards**
  - **Technical definition:** shards is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **users**
  - **Technical definition:** users are human or service identities represented directly in an AWS identity system and evaluated against attached or inherited access policy.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **user groups**
  - **Technical definition:** user groups are human or service identities represented directly in an AWS identity system and evaluated against attached or inherited access policy.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **MemoryDB clusters**
  - **Technical definition:** MemoryDB clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access control lists (ACLs)**
  - **Technical definition:** access control lists (ACLs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **snapshots**
  - **Technical definition:** snapshots are point-in-time persisted state captures used for backup, copy, restore, and migration workflows across AWS storage or database services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoints**
  - **Technical definition:** endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.

### Amazon OpenSearch Service, Amazon Redshift, and Amazon Athena

- **Amazon OpenSearch Service, Amazon Redshift, and Amazon Athena**
  - **Technical definition:** Amazon OpenSearch Service, Amazon Redshift, and Amazon Athena is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **OpenSearch domains**
  - **Technical definition:** OpenSearch domains is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **indexes**
  - **Technical definition:** indexes are alternate access structures that optimize specific query patterns at the cost of additional write, storage, or consistency overhead.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **data nodes**
  - **Technical definition:** data nodes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cluster manager nodes**
  - **Technical definition:** cluster manager nodes are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **OpenSearch Dashboards**
  - **Technical definition:** OpenSearch Dashboards are curated operational views that aggregate telemetry into human-readable status, trend, and investigation context.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **Redshift provisioned clusters**
  - **Technical definition:** Redshift provisioned clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **serverless namespaces**
  - **Technical definition:** serverless namespaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **serverless workgroups**
  - **Technical definition:** serverless workgroups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **databases**
  - **Technical definition:** databases is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **schemas**
  - **Technical definition:** schemas is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tables**
  - **Technical definition:** tables are logical state containers that define access patterns, indexing, throughput, and replication semantics for database workloads.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **materialized views**
  - **Technical definition:** materialized views is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Athena workgroups**
  - **Technical definition:** Athena workgroups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **queries**
  - **Technical definition:** queries is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **data catalogs**
  - **Technical definition:** data catalogs are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **partitions**
  - **Technical definition:** partitions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **query result locations**
  - **Technical definition:** query result locations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Glue and AWS Lake Formation

- **AWS Glue and AWS Lake Formation**
  - **Technical definition:** AWS Glue and AWS Lake Formation is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Data Catalog databases**
  - **Technical definition:** Data Catalog databases are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **Data Catalog tables**
  - **Technical definition:** Data Catalog tables are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **crawlers**
  - **Technical definition:** crawlers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **jobs**
  - **Technical definition:** jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **triggers**
  - **Technical definition:** triggers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **workflows**
  - **Technical definition:** workflows is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **connections**
  - **Technical definition:** connections is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **schemas**
  - **Technical definition:** schemas is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **classifiers**
  - **Technical definition:** classifiers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **data quality rulesets**
  - **Technical definition:** data quality rulesets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Glue DataBrew projects**
  - **Technical definition:** AWS Glue DataBrew projects is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **datasets**
  - **Technical definition:** datasets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **recipes**
  - **Technical definition:** recipes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **profile jobs**
  - **Technical definition:** profile jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Lake Formation data lake administrators**
  - **Technical definition:** Lake Formation data lake administrators is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **registered locations**
  - **Technical definition:** registered locations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **LF-Tags**
  - **Technical definition:** LF-Tags is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **permissions**
  - **Technical definition:** permissions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **data filters**
  - **Technical definition:** data filters is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource links**
  - **Technical definition:** resource links is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **blueprints**
  - **Technical definition:** blueprints is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon Kinesis Data Streams, Amazon Data Firehose, Amazon Managed Streaming for Apache Kafka (Amazon MSK), Amazon Simple Queue Service (Amazon SQS), and Amazon Simple Notification Service (Amazon SNS)

- **Amazon Kinesis Data Streams, Amazon Data Firehose, Amazon Managed Streaming for Apache Kafka (Amazon MSK), Amazon Simple Queue Service (Amazon SQS), and Amazon Simple Notification Service (Amazon SNS)**
  - **Technical definition:** Amazon Kinesis Data Streams, Amazon Data Firehose, Amazon Managed Streaming for Apache Kafka (Amazon MSK), Amazon Simple Queue Service (Amazon SQS), and Amazon Simple Notification Service (Amazon SNS) are ordered or near-real-time change or telemetry feeds used for downstream processing, replication, observability, or event-driven design.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Kinesis data streams**
  - **Technical definition:** Kinesis data streams are ordered or near-real-time change or telemetry feeds used for downstream processing, replication, observability, or event-driven design.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **shards**
  - **Technical definition:** shards is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **consumers**
  - **Technical definition:** consumers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **enhanced fan-out consumers**
  - **Technical definition:** enhanced fan-out consumers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Firehose streams**
  - **Technical definition:** Firehose streams are ordered or near-real-time change or telemetry feeds used for downstream processing, replication, observability, or event-driven design.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **sources**
  - **Technical definition:** sources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **destinations**
  - **Technical definition:** destinations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **MSK clusters**
  - **Technical definition:** MSK clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **brokers**
  - **Technical definition:** brokers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **topics**
  - **Technical definition:** topics is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **partitions**
  - **Technical definition:** partitions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **consumer groups**
  - **Technical definition:** consumer groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **connectors**
  - **Technical definition:** connectors is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **SQS standard queues**
  - **Technical definition:** SQS standard queues is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **FIFO queues**
  - **Technical definition:** FIFO queues is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **SNS topics**
  - **Technical definition:** SNS topics is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **subscriptions**
  - **Technical definition:** subscriptions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **filter policies**
  - **Technical definition:** filter policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon EventBridge and AWS Step Functions

- **Amazon EventBridge and AWS Step Functions**
  - **Technical definition:** Amazon EventBridge and AWS Step Functions are serverless code execution units whose scaling, event handling, and concurrency behavior are managed by AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **event buses**
  - **Technical definition:** event buses is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rules**
  - **Technical definition:** rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **targets**
  - **Technical definition:** targets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **event patterns**
  - **Technical definition:** event patterns is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **archives**
  - **Technical definition:** archives is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **replays**
  - **Technical definition:** replays is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **schedules**
  - **Technical definition:** schedules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **connections**
  - **Technical definition:** connections is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **API destinations**
  - **Technical definition:** API destinations are API exposure or access-control constructs used to publish, secure, meter, and version application interfaces on AWS.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **state machines**
  - **Technical definition:** state machines is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **states**
  - **Technical definition:** states is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **executions**
  - **Technical definition:** executions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **activities**
  - **Technical definition:** activities is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tasks**
  - **Technical definition:** tasks are discrete executable or replicative work units scheduled or managed by an AWS service control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **versions**
  - **Technical definition:** versions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **aliases**
  - **Technical definition:** aliases is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **distributed maps**
  - **Technical definition:** distributed maps is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **execution history**
  - **Technical definition:** execution history is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Secrets Manager and AWS Key Management Service (AWS KMS)

- **AWS Secrets Manager and AWS Key Management Service (AWS KMS)**
  - **Technical definition:** AWS Secrets Manager and AWS Key Management Service (AWS KMS) are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **secrets**
  - **Technical definition:** secrets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **secret versions**
  - **Technical definition:** secret versions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **version stages**
  - **Technical definition:** version stages is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rotation schedules**
  - **Technical definition:** rotation schedules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rotation Lambda functions**
  - **Technical definition:** rotation Lambda functions are serverless code execution units whose scaling, event handling, and concurrency behavior are managed by AWS.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **resource policies**
  - **Technical definition:** resource policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **replication settings**
  - **Technical definition:** replication settings are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **KMS keys**
  - **Technical definition:** KMS keys are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **aliases**
  - **Technical definition:** aliases is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **key policies**
  - **Technical definition:** key policies are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **grants**
  - **Technical definition:** grants is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **key material**
  - **Technical definition:** key material are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **multi-Region keys**
  - **Technical definition:** multi-Region keys are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **imported key material**
  - **Technical definition:** imported key material are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **external key stores**
  - **Technical definition:** external key stores are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Backup and AWS Database Migration Service (AWS DMS)

- **AWS Backup and AWS Database Migration Service (AWS DMS)**
  - **Technical definition:** AWS Backup and AWS Database Migration Service (AWS DMS) are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **backup plans**
  - **Technical definition:** backup plans are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **backup rules**
  - **Technical definition:** backup rules are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **backup selections**
  - **Technical definition:** backup selections are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **recovery points**
  - **Technical definition:** recovery points is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backup vaults**
  - **Technical definition:** backup vaults are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **backup vault lock configurations**
  - **Technical definition:** backup vault lock configurations are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **copy jobs**
  - **Technical definition:** copy jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **restore jobs**
  - **Technical definition:** restore jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **replication instances**
  - **Technical definition:** replication instances are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **replication tasks**
  - **Technical definition:** replication tasks are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **endpoints**
  - **Technical definition:** endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **replication subnet groups**
  - **Technical definition:** replication subnet groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **event subscriptions**
  - **Technical definition:** event subscriptions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **data providers**
  - **Technical definition:** data providers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **migration projects**
  - **Technical definition:** migration projects is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

## Controls

### S3 bucket policies

- **S3 bucket policies**
  - **Technical definition:** S3 bucket policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **Object Ownership**
  - **Technical definition:** Object Ownership is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **versioning**
  - **Technical definition:** versioning is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **default encryption**
  - **Technical definition:** default encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **S3 Object Lock retention modes**
  - **Technical definition:** S3 Object Lock retention modes is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **replication**
  - **Technical definition:** replication is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **access logging**
  - **Technical definition:** access logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### EBS volume encryption

- **EBS volume encryption**
  - **Technical definition:** EBS volume encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **snapshot sharing**
  - **Technical definition:** snapshot sharing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **delete-on-termination settings**
  - **Technical definition:** delete-on-termination settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **KMS key selection**
  - **Technical definition:** KMS key selection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **EFS file system policies**
  - **Technical definition:** EFS file system policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **security groups**
  - **Technical definition:** security groups is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access point POSIX identities**
  - **Technical definition:** access point POSIX identities is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lifecycle management**
  - **Technical definition:** lifecycle management is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backup policies**
  - **Technical definition:** backup policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.

### RDS and Aurora Multi-AZ deployment

- **RDS and Aurora Multi-AZ deployment**
  - **Technical definition:** RDS and Aurora Multi-AZ deployment is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **backup retention**
  - **Technical definition:** backup retention is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **storage autoscaling**
  - **Technical definition:** storage autoscaling is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **IAM database authentication**
  - **Technical definition:** IAM database authentication is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **failover priority**
  - **Technical definition:** failover priority is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **parameter alignment**
  - **Technical definition:** parameter alignment is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### DynamoDB capacity modes

- **DynamoDB capacity modes**
  - **Technical definition:** DynamoDB capacity modes is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **auto scaling**
  - **Technical definition:** auto scaling is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **table classes**
  - **Technical definition:** table classes is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **point-in-time recovery (PITR)**
  - **Technical definition:** point-in-time recovery (PITR) is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource-based policies**
  - **Technical definition:** resource-based policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **deletion protection**
  - **Technical definition:** deletion protection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Specialized database VPC placement

- **Specialized database VPC placement**
  - **Technical definition:** Specialized database VPC placement is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **TLS**
  - **Technical definition:** TLS is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backup retention**
  - **Technical definition:** backup retention is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **audit logging**
  - **Technical definition:** audit logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **retention configuration**
  - **Technical definition:** retention configuration is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **Redis-compatible access control lists**
  - **Technical definition:** Redis-compatible access control lists is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Multi-AZ deployment**
  - **Technical definition:** Multi-AZ deployment is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### OpenSearch fine-grained access control

- **OpenSearch fine-grained access control**
  - **Technical definition:** OpenSearch fine-grained access control is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **VPC access**
  - **Technical definition:** VPC access is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **index state management**
  - **Technical definition:** index state management is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Redshift workload management (WLM)**
  - **Technical definition:** Redshift workload management (WLM) is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **data sharing**
  - **Technical definition:** data sharing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Athena workgroup settings**
  - **Technical definition:** Athena workgroup settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **bytes scanned limits**
  - **Technical definition:** bytes scanned limits is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **result location enforcement**
  - **Technical definition:** result location enforcement is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Glue job bookmarks

- **Glue job bookmarks**
  - **Technical definition:** Glue job bookmarks is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **worker type**
  - **Technical definition:** worker type is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **security configurations**
  - **Technical definition:** security configurations is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **Data Catalog permissions**
  - **Technical definition:** Data Catalog permissions is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **Lake Formation permissions**
  - **Technical definition:** Lake Formation permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **LF-Tag-based access control**
  - **Technical definition:** LF-Tag-based access control is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **row-level filters**
  - **Technical definition:** row-level filters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **column-level filters**
  - **Technical definition:** column-level filters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-account sharing**
  - **Technical definition:** cross-account sharing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Streaming and messaging retention

- **Streaming and messaging retention**
  - **Technical definition:** Streaming and messaging retention is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **shard count**
  - **Technical definition:** shard count is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Firehose buffering**
  - **Technical definition:** Firehose buffering is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **broker storage**
  - **Technical definition:** broker storage is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Apache Kafka ACLs**
  - **Technical definition:** Apache Kafka ACLs is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **queue policies**
  - **Technical definition:** queue policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **visibility timeout**
  - **Technical definition:** visibility timeout is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **redrive policies**
  - **Technical definition:** redrive policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **topic policies**
  - **Technical definition:** topic policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **subscription filter policies**
  - **Technical definition:** subscription filter policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **delivery retry policies**
  - **Technical definition:** delivery retry policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### EventBridge event bus policies

- **EventBridge event bus policies**
  - **Technical definition:** EventBridge event bus policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **input transformers**
  - **Technical definition:** input transformers is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **retry policies**
  - **Technical definition:** retry policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **dead-letter queues**
  - **Technical definition:** dead-letter queues is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **schema discovery**
  - **Technical definition:** schema discovery is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Step Functions retry/catch handlers**
  - **Technical definition:** Step Functions retry/catch handlers is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **timeout settings**
  - **Technical definition:** timeout settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **logging**
  - **Technical definition:** logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **tracing**
  - **Technical definition:** tracing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Secrets Manager automatic rotation

- **Secrets Manager automatic rotation**
  - **Technical definition:** Secrets Manager automatic rotation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **version staging labels**
  - **Technical definition:** version staging labels is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource policies**
  - **Technical definition:** resource policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **KMS key policies**
  - **Technical definition:** KMS key policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **grants**
  - **Technical definition:** grants is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **rotation**
  - **Technical definition:** rotation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **deletion waiting periods**
  - **Technical definition:** deletion waiting periods is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **multi-Region replication**
  - **Technical definition:** multi-Region replication is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.

### AWS Backup schedules

- **AWS Backup schedules**
  - **Technical definition:** AWS Backup schedules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **vault policies**
  - **Technical definition:** vault policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Backup Vault Lock**
  - **Technical definition:** AWS Backup Vault Lock is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **cross-Region copy**
  - **Technical definition:** cross-Region copy is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-account copy**
  - **Technical definition:** cross-account copy is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **legal holds**
  - **Technical definition:** legal holds is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **DMS full-load settings**
  - **Technical definition:** DMS full-load settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **change data capture (CDC)**
  - **Technical definition:** change data capture (CDC) is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **table mappings**
  - **Technical definition:** table mappings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **validation settings**
  - **Technical definition:** validation settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoint permissions**
  - **Technical definition:** endpoint permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.

## Operations

### Restore S3 objects

- **Restore S3 objects**
  - **Technical definition:** Restore S3 objects is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate lifecycle policies**
  - **Technical definition:** validate lifecycle policies is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review replication**
  - **Technical definition:** review replication is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **check public access**
  - **Technical definition:** check public access is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **optimize storage classes**
  - **Technical definition:** optimize storage classes is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Attach and resize EBS volumes

- **Attach and resize EBS volumes**
  - **Technical definition:** Attach and resize EBS volumes is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **restore snapshots**
  - **Technical definition:** restore snapshots is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate EFS mounts**
  - **Technical definition:** validate EFS mounts is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review throughput**
  - **Technical definition:** review throughput is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **test file-system recovery**
  - **Technical definition:** test file-system recovery is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Validate RDS/Aurora failover

- **Validate RDS/Aurora failover**
  - **Technical definition:** Validate RDS/Aurora failover is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **point-in-time recovery (PITR)**
  - **Technical definition:** point-in-time recovery (PITR) is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **replica promotion**
  - **Technical definition:** replica promotion is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **global database switchover**
  - **Technical definition:** global database switchover is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **engine upgrades**
  - **Technical definition:** engine upgrades is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **replica lag**
  - **Technical definition:** replica lag is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Troubleshoot DynamoDB hot partitions

- **Troubleshoot DynamoDB hot partitions**
  - **Technical definition:** Troubleshoot DynamoDB hot partitions is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **throttling**
  - **Technical definition:** throttling is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **secondary indexes**
  - **Technical definition:** secondary indexes is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **backup restore**
  - **Technical definition:** backup restore is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **streams**
  - **Technical definition:** streams is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **global table replication**
  - **Technical definition:** global table replication is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Review cache hit ratios

- **Review cache hit ratios**
  - **Technical definition:** Review cache hit ratios is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Redis-compatible access**
  - **Technical definition:** Redis-compatible access is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **snapshot restore**
  - **Technical definition:** snapshot restore is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **eviction behavior**
  - **Technical definition:** eviction behavior is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **specialized database migration fit**
  - **Technical definition:** specialized database migration fit is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Tune OpenSearch shards

- **Tune OpenSearch shards**
  - **Technical definition:** Tune OpenSearch shards is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Redshift queries**
  - **Technical definition:** Redshift queries is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Athena partitions**
  - **Technical definition:** Athena partitions is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **data lake query failures**
  - **Technical definition:** data lake query failures is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Run Glue crawlers and ETL jobs

- **Run Glue crawlers and ETL jobs**
  - **Technical definition:** Run Glue crawlers and ETL jobs is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate schemas**
  - **Technical definition:** validate schemas is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review data quality**
  - **Technical definition:** review data quality is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **grant Lake Formation permissions**
  - **Technical definition:** grant Lake Formation permissions is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **troubleshoot governed data sharing**
  - **Technical definition:** troubleshoot governed data sharing is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Monitor stream ingestion

- **Monitor stream ingestion**
  - **Technical definition:** Monitor stream ingestion is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **shard scaling**
  - **Technical definition:** shard scaling is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **consumer lag**
  - **Technical definition:** consumer lag is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **MSK connectors**
  - **Technical definition:** MSK connectors is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **SQS dead-letter queue redrive**
  - **Technical definition:** SQS dead-letter queue redrive is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **SNS fanout**
  - **Technical definition:** SNS fanout is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Validate EventBridge routing and replay

- **Validate EventBridge routing and replay**
  - **Technical definition:** Validate EventBridge routing and replay is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **then troubleshoot failed Step Functions executions and orchestration rollback**
  - **Technical definition:** then troubleshoot failed Step Functions executions and orchestration rollback is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Rotate secrets

- **Rotate secrets**
  - **Technical definition:** Rotate secrets is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **roll back secret versions**
  - **Technical definition:** roll back secret versions is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review KMS grants**
  - **Technical definition:** review KMS grants is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate cross-account key access**
  - **Technical definition:** validate cross-account key access is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **test backups**
  - **Technical definition:** test backups is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **restore recovery points**
  - **Technical definition:** restore recovery points is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **execute DMS full-load plus CDC cutover**
  - **Technical definition:** execute DMS full-load plus CDC cutover is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

# AWS Week 5: Observability, FinOps, And Operational Excellence

## Service And Entity Coverage

### Amazon CloudWatch and AWS X-Ray

- **Amazon CloudWatch and AWS X-Ray**
  - **Technical definition:** Amazon CloudWatch and AWS X-Ray provide AWS-native metrics, logs, alarms, synthetic monitoring, and distributed trace visibility for infrastructure and application telemetry.
  - **Concrete production example:** Example: an SRE traces a latency spike from dashboard symptom to dependency root cause using correlated metrics, logs, and traces.
- **metrics**
  - **Technical definition:** metrics are measured telemetry series or aggregations used to represent resource, platform, or application behavior over time.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **namespaces**
  - **Technical definition:** namespaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **dimensions**
  - **Technical definition:** dimensions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **alarms**
  - **Technical definition:** alarms are threshold- or expression-driven operational control objects that convert telemetry into notification or automated response in AWS monitoring workflows.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **dashboards**
  - **Technical definition:** dashboards are curated operational views that aggregate telemetry into human-readable status, trend, and investigation context.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **log groups**
  - **Technical definition:** log groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **log streams**
  - **Technical definition:** log streams are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **metric filters**
  - **Technical definition:** metric filters are measured telemetry series or aggregations used to represent resource, platform, or application behavior over time.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Contributor Insights rules**
  - **Technical definition:** Contributor Insights rules are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **synthetic canaries**
  - **Technical definition:** synthetic canaries is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Application Signals services**
  - **Technical definition:** Application Signals services is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Container Insights clusters**
  - **Technical definition:** Container Insights clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Logs Insights queries**
  - **Technical definition:** Logs Insights queries are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **traces**
  - **Technical definition:** traces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **segments**
  - **Technical definition:** segments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **subsegments**
  - **Technical definition:** subsegments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **service maps**
  - **Technical definition:** service maps is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **groups**
  - **Technical definition:** groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **sampling rules**
  - **Technical definition:** sampling rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **annotations**
  - **Technical definition:** annotations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **metadata**
  - **Technical definition:** metadata is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Distro for OpenTelemetry (ADOT), Amazon Managed Service for Prometheus, and Amazon Managed Grafana

- **AWS Distro for OpenTelemetry (ADOT), Amazon Managed Service for Prometheus, and Amazon Managed Grafana**
  - **Technical definition:** ADOT, Amazon Managed Service for Prometheus, and Amazon Managed Grafana provide open telemetry collection, Prometheus-compatible metrics storage, and Grafana-based visualization in AWS.
  - **Concrete production example:** Example: an SRE traces a latency spike from dashboard symptom to dependency root cause using correlated metrics, logs, and traces.
- **ADOT collectors**
  - **Technical definition:** ADOT collectors is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **pipelines**
  - **Technical definition:** pipelines are ordered execution graphs that transform source state into build, deploy, data-move, or ML outcomes under managed orchestration.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **receivers**
  - **Technical definition:** receivers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **processors**
  - **Technical definition:** processors is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **exporters**
  - **Technical definition:** exporters is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Prometheus workspaces**
  - **Technical definition:** Prometheus workspaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: an SRE traces a latency spike from dashboard symptom to dependency root cause using correlated metrics, logs, and traces.
- **rule groups namespaces**
  - **Technical definition:** rule groups namespaces are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **scrapers**
  - **Technical definition:** scrapers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **metrics**
  - **Technical definition:** metrics are measured telemetry series or aggregations used to represent resource, platform, or application behavior over time.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **labels**
  - **Technical definition:** labels is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Grafana workspaces**
  - **Technical definition:** Grafana workspaces is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: an SRE traces a latency spike from dashboard symptom to dependency root cause using correlated metrics, logs, and traces.
- **data sources**
  - **Technical definition:** data sources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **dashboards**
  - **Technical definition:** dashboards are curated operational views that aggregate telemetry into human-readable status, trend, and investigation context.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **folders**
  - **Technical definition:** folders is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **users**
  - **Technical definition:** users are human or service identities represented directly in an AWS identity system and evaluated against attached or inherited access policy.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **teams**
  - **Technical definition:** teams is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **service accounts**
  - **Technical definition:** service accounts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Systems Manager

- **AWS Systems Manager**
  - **Technical definition:** AWS Systems Manager is AWS's fleet operations control plane for managed instances, remote execution, session access, patching, parameters, and automation workflows.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **managed nodes**
  - **Technical definition:** managed nodes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **documents**
  - **Technical definition:** documents is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Run Command**
  - **Technical definition:** Run Command is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Session Manager sessions**
  - **Technical definition:** Session Manager sessions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **State Manager associations**
  - **Technical definition:** State Manager associations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Patch Manager baselines**
  - **Technical definition:** Patch Manager baselines is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Automation runbooks**
  - **Technical definition:** Automation runbooks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Parameter Store parameters**
  - **Technical definition:** Parameter Store parameters is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **OpsItems**
  - **Technical definition:** OpsItems is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Well-Architected Tool and AWS Resilience Hub

- **AWS Well-Architected Tool and AWS Resilience Hub**
  - **Technical definition:** AWS Well-Architected Tool and AWS Resilience Hub provide structured architecture review and resilience assessment workflows tied to AWS operational best practices and recovery objectives.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **workloads**
  - **Technical definition:** workloads is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lenses**
  - **Technical definition:** lenses is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **milestones**
  - **Technical definition:** milestones is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **improvement plans**
  - **Technical definition:** improvement plans is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **review templates**
  - **Technical definition:** review templates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **profiles**
  - **Technical definition:** profiles is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **workload shares**
  - **Technical definition:** workload shares is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **applications**
  - **Technical definition:** applications is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **app components**
  - **Technical definition:** app components is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resilience policies**
  - **Technical definition:** resilience policies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **assessments**
  - **Technical definition:** assessments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resiliency scores**
  - **Technical definition:** resiliency scores is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **recommendations**
  - **Technical definition:** recommendations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **standard operating procedures (SOPs)**
  - **Technical definition:** standard operating procedures (SOPs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Cost Explorer, AWS Budgets, AWS Cost Categories, AWS Cost and Usage Reports (AWS CUR), AWS Cost Anomaly Detection, AWS Compute Optimizer, and AWS Trusted Advisor

- **AWS Cost Explorer, AWS Budgets, AWS Cost Categories, AWS Cost and Usage Reports (AWS CUR), AWS Cost Anomaly Detection, AWS Compute Optimizer, and AWS Trusted Advisor**
  - **Technical definition:** AWS Cost Explorer, Budgets, CUR, Cost Categories, Cost Anomaly Detection, Compute Optimizer, and Trusted Advisor form AWS's FinOps and optimization analysis toolkit for spend visibility and rightsizing.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cost and usage data**
  - **Technical definition:** cost and usage data is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **dimensions**
  - **Technical definition:** dimensions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **filters**
  - **Technical definition:** filters is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **forecasts**
  - **Technical definition:** forecasts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **budgets**
  - **Technical definition:** budgets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **budget actions**
  - **Technical definition:** budget actions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **alerts**
  - **Technical definition:** alerts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cost category rules**
  - **Technical definition:** cost category rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **CUR report definitions**
  - **Technical definition:** CUR report definitions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **anomaly monitors**
  - **Technical definition:** anomaly monitors is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **anomalies**
  - **Technical definition:** anomalies is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **recommendations**
  - **Technical definition:** recommendations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **checks**
  - **Technical definition:** checks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon GuardDuty, Amazon Inspector, Amazon Macie, Amazon Detective, and Amazon Security Lake

- **Amazon GuardDuty, Amazon Inspector, Amazon Macie, Amazon Detective, and Amazon Security Lake**
  - **Technical definition:** Amazon GuardDuty, Amazon Inspector, Amazon Macie, Amazon Detective, and Amazon Security Lake is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **detectors**
  - **Technical definition:** detectors is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **findings**
  - **Technical definition:** findings are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **threat intelligence sets**
  - **Technical definition:** threat intelligence sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **malware protection plans**
  - **Technical definition:** malware protection plans is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **coverage**
  - **Technical definition:** coverage is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **software bills of materials (SBOMs)**
  - **Technical definition:** software bills of materials (SBOMs) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **classification jobs**
  - **Technical definition:** classification jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **sensitive data discovery results**
  - **Technical definition:** sensitive data discovery results is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **managed data identifiers**
  - **Technical definition:** managed data identifiers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **behavior graphs**
  - **Technical definition:** behavior graphs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **investigations**
  - **Technical definition:** investigations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **data lakes**
  - **Technical definition:** data lakes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **sources**
  - **Technical definition:** sources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **subscribers**
  - **Technical definition:** subscribers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Open Cybersecurity Schema Framework (OCSF) event classes**
  - **Technical definition:** Open Cybersecurity Schema Framework (OCSF) event classes is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Amazon SageMaker AI and Amazon Bedrock

- **Amazon SageMaker AI and Amazon Bedrock**
  - **Technical definition:** Amazon SageMaker AI and Amazon Bedrock is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **SageMaker monitoring schedules**
  - **Technical definition:** SageMaker monitoring schedules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **baselines**
  - **Technical definition:** baselines is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **constraints**
  - **Technical definition:** constraints is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **processing jobs**
  - **Technical definition:** processing jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **bias reports**
  - **Technical definition:** bias reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **explainability reports**
  - **Technical definition:** explainability reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **experiments**
  - **Technical definition:** experiments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **trials**
  - **Technical definition:** trials is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lineage artifacts**
  - **Technical definition:** lineage artifacts is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lineage actions**
  - **Technical definition:** lineage actions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Bedrock model evaluation jobs**
  - **Technical definition:** Bedrock model evaluation jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **evaluation datasets**
  - **Technical definition:** evaluation datasets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **guardrails**
  - **Technical definition:** guardrails is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **guardrail versions**
  - **Technical definition:** guardrail versions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **foundation models**
  - **Technical definition:** foundation models is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **agents**
  - **Technical definition:** agents is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **invocation logs**
  - **Technical definition:** invocation logs are persisted telemetry or audit records used for debugging, forensics, and compliance evidence in AWS operational systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

## Controls

### CloudWatch alarm actions

- **CloudWatch alarm actions**
  - **Technical definition:** CloudWatch alarm actions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: an SRE traces a latency spike from dashboard symptom to dependency root cause using correlated metrics, logs, and traces.
- **metric math**
  - **Technical definition:** metric math is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **composite alarms**
  - **Technical definition:** composite alarms is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **log retention**
  - **Technical definition:** log retention is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **data protection policies**
  - **Technical definition:** data protection policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-account observability**
  - **Technical definition:** cross-account observability is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **dashboards**
  - **Technical definition:** dashboards is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **X-Ray sampling rules**
  - **Technical definition:** X-Ray sampling rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: an SRE traces a latency spike from dashboard symptom to dependency root cause using correlated metrics, logs, and traces.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **trace retention**
  - **Technical definition:** trace retention is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **SDK configuration**
  - **Technical definition:** SDK configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### ADOT telemetry routing

- **ADOT telemetry routing**
  - **Technical definition:** ADOT telemetry routing is a network-boundary control that shapes traffic isolation, propagation, and blast radius across AWS networking domains.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **sampling**
  - **Technical definition:** sampling is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **exporter configuration**
  - **Technical definition:** exporter configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **Prometheus workspace policies**
  - **Technical definition:** Prometheus workspace policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: an SRE traces a latency spike from dashboard symptom to dependency root cause using correlated metrics, logs, and traces.
- **ingestion limits**
  - **Technical definition:** ingestion limits is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **alerting rules**
  - **Technical definition:** alerting rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Grafana authentication providers**
  - **Technical definition:** Grafana authentication providers is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: an SRE traces a latency spike from dashboard symptom to dependency root cause using correlated metrics, logs, and traces.
- **data source permissions**
  - **Technical definition:** data source permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **workspace roles**
  - **Technical definition:** workspace roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **plugin management**
  - **Technical definition:** plugin management is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Systems Manager instance profiles

- **Systems Manager instance profiles**
  - **Technical definition:** Systems Manager instance profiles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **session logging**
  - **Technical definition:** session logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **patch baselines**
  - **Technical definition:** patch baselines is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **maintenance windows**
  - **Technical definition:** maintenance windows is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **document permissions**
  - **Technical definition:** document permissions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **parameter policies**
  - **Technical definition:** parameter policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Well-Architected lens selection

- **Well-Architected lens selection**
  - **Technical definition:** Well-Architected lens selection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **workload sharing**
  - **Technical definition:** workload sharing is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Resilience Hub recovery time objective/recovery point objective (RTO/RPO) targets**
  - **Technical definition:** Resilience Hub recovery time objective/recovery point objective (RTO/RPO) targets is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **policy mapping**
  - **Technical definition:** policy mapping is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **application import sources**
  - **Technical definition:** application import sources is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **assessment scope**
  - **Technical definition:** assessment scope is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Cost filters

- **Cost filters**
  - **Technical definition:** Cost filters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **grouping dimensions**
  - **Technical definition:** grouping dimensions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **budget thresholds**
  - **Technical definition:** budget thresholds is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **budget actions**
  - **Technical definition:** budget actions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **split charge allocation**
  - **Technical definition:** split charge allocation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **CUR delivery options**
  - **Technical definition:** CUR delivery options is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **anomaly alert thresholds**
  - **Technical definition:** anomaly alert thresholds is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **recommendation preferences**
  - **Technical definition:** recommendation preferences is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **organizational views**
  - **Technical definition:** organizational views is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Security delegated administrator accounts

- **Security delegated administrator accounts**
  - **Technical definition:** Security delegated administrator accounts is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **GuardDuty detector features**
  - **Technical definition:** GuardDuty detector features is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **S3 protection**
  - **Technical definition:** S3 protection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **EKS protection**
  - **Technical definition:** EKS protection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **Amazon ECR scanning**
  - **Technical definition:** Amazon ECR scanning is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Lambda scanning**
  - **Technical definition:** AWS Lambda scanning is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.
- **finding filters**
  - **Technical definition:** finding filters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **automated sensitive data discovery**
  - **Technical definition:** automated sensitive data discovery is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **graph retention**
  - **Technical definition:** graph retention is a lifecycle control that determines how long AWS telemetry, backups, or state are preserved for recovery, audit, or investigation.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **source collection**
  - **Technical definition:** source collection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **subscriber access**
  - **Technical definition:** subscriber access is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-Region rollup**
  - **Technical definition:** cross-Region rollup is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### SageMaker data capture

- **SageMaker data capture**
  - **Technical definition:** SageMaker data capture is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **baseline constraints**
  - **Technical definition:** baseline constraints is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **bias metrics**
  - **Technical definition:** bias metrics is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **explainability configuration**
  - **Technical definition:** explainability configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **experiment metadata**
  - **Technical definition:** experiment metadata is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **lineage metadata**
  - **Technical definition:** lineage metadata is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Bedrock model access**
  - **Technical definition:** Bedrock model access is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **guardrail policies**
  - **Technical definition:** guardrail policies is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **content filters**
  - **Technical definition:** content filters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **sensitive information filters**
  - **Technical definition:** sensitive information filters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **VPC endpoints**
  - **Technical definition:** VPC endpoints is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **logging**
  - **Technical definition:** logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

## Operations

### Review dashboards

- **Review dashboards**
  - **Technical definition:** Review dashboards is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **alarms**
  - **Technical definition:** alarms is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Logs Insights queries**
  - **Technical definition:** Logs Insights queries is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **synthetic checks**
  - **Technical definition:** synthetic checks is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **distributed traces**
  - **Technical definition:** distributed traces is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **dependency maps**
  - **Technical definition:** dependency maps is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **latency root causes**
  - **Technical definition:** latency root causes is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **incident timelines**
  - **Technical definition:** incident timelines is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Roll out ADOT collectors

- **Roll out ADOT collectors**
  - **Technical definition:** Roll out ADOT collectors is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate Prometheus queries**
  - **Technical definition:** validate Prometheus queries is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **tune metric cardinality**
  - **Technical definition:** tune metric cardinality is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **publish Grafana dashboards**
  - **Technical definition:** publish Grafana dashboards is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **troubleshoot data sources**
  - **Technical definition:** troubleshoot data sources is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Execute fleet commands

- **Execute fleet commands**
  - **Technical definition:** Execute fleet commands is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **open Session Manager sessions**
  - **Technical definition:** open Session Manager sessions is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review patch compliance**
  - **Technical definition:** review patch compliance is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **run automations**
  - **Technical definition:** run automations is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **track OpsItems**
  - **Technical definition:** track OpsItems is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Perform Well-Architected reviews

- **Perform Well-Architected reviews**
  - **Technical definition:** Perform Well-Architected reviews is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **track risks**
  - **Technical definition:** track risks is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **run Resilience Hub assessments**
  - **Technical definition:** run Resilience Hub assessments is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review resiliency recommendations**
  - **Technical definition:** review resiliency recommendations is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Analyze spend

- **Analyze spend**
  - **Technical definition:** Analyze spend is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review forecasts**
  - **Technical definition:** review forecasts is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **tune budgets**
  - **Technical definition:** tune budgets is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate CUR delivery**
  - **Technical definition:** validate CUR delivery is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **triage cost anomalies**
  - **Technical definition:** triage cost anomalies is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **track rightsizing recommendations**
  - **Technical definition:** track rightsizing recommendations is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Triage GuardDuty findings

- **Triage GuardDuty findings**
  - **Technical definition:** Triage GuardDuty findings is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Inspector vulnerabilities**
  - **Technical definition:** Inspector vulnerabilities is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Macie sensitive data findings**
  - **Technical definition:** Macie sensitive data findings is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Detective investigations**
  - **Technical definition:** Detective investigations is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Security Lake subscriber delivery**
  - **Technical definition:** Security Lake subscriber delivery is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Review model drift

- **Review model drift**
  - **Technical definition:** Review model drift is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **bias reports**
  - **Technical definition:** bias reports is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **explainability reports**
  - **Technical definition:** explainability reports is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **model lineage**
  - **Technical definition:** model lineage is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **Bedrock evaluations**
  - **Technical definition:** Bedrock evaluations is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **guardrail tests**
  - **Technical definition:** guardrail tests is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **generative AI governance evidence**
  - **Technical definition:** generative AI governance evidence is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

# AWS Week 6: DR, Multi-Region, And Migration Execution

## Service And Entity Coverage

### Amazon Application Recovery Controller (ARC) (Route 53 ARC)

- **Amazon Application Recovery Controller (ARC) (Route 53 ARC)**
  - **Technical definition:** Amazon Application Recovery Controller is AWS's readiness and routing-control service for zonal and regional recovery orchestration with safety checks and operator-controlled failover.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **clusters**
  - **Technical definition:** clusters are grouped runtime or control-plane resources that provide coordinated capacity, failover, or scheduling behavior across multiple nodes or instances.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **routing controls**
  - **Technical definition:** routing controls is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **control panels**
  - **Technical definition:** control panels is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **safety rules**
  - **Technical definition:** safety rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **readiness checks**
  - **Technical definition:** readiness checks is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **recovery groups**
  - **Technical definition:** recovery groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cells**
  - **Technical definition:** cells is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **resource sets**
  - **Technical definition:** resource sets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Elastic Disaster Recovery (AWS DRS) and AWS Application Migration Service (AWS MGN)

- **AWS Elastic Disaster Recovery (AWS DRS) and AWS Application Migration Service (AWS MGN)**
  - **Technical definition:** AWS DRS and AWS MGN provide block-level replication and server migration workflows for disaster recovery and cloud migration of machine-based workloads.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **DRS source servers**
  - **Technical definition:** DRS source servers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **replication servers**
  - **Technical definition:** replication servers are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **staging area subnets**
  - **Technical definition:** staging area subnets are VPC address subdivisions that scope routing, availability-zone placement, and network-control attachment for AWS resources.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **recovery instances**
  - **Technical definition:** recovery instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **recovery snapshots**
  - **Technical definition:** recovery snapshots are point-in-time persisted state captures used for backup, copy, restore, and migration workflows across AWS storage or database services.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **conversion servers**
  - **Technical definition:** conversion servers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **MGN source servers**
  - **Technical definition:** MGN source servers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **replication agents**
  - **Technical definition:** replication agents are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **replication settings templates**
  - **Technical definition:** replication settings templates are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **launch settings**
  - **Technical definition:** launch settings is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **test instances**
  - **Technical definition:** test instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cutover instances**
  - **Technical definition:** cutover instances are runtime or management units representing provisioned compute, database, migration, or service capacity inside an AWS-managed control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **waves**
  - **Technical definition:** waves is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Migration Hub

- **AWS Migration Hub**
  - **Technical definition:** AWS Migration Hub is AWS's portfolio-level migration tracking and grouping service for applications, waves, and tool-integrated status aggregation.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **applications**
  - **Technical definition:** applications is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **servers**
  - **Technical definition:** servers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **migration waves**
  - **Technical definition:** migration waves is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **discovered resources**
  - **Technical definition:** discovered resources is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **migration tools**
  - **Technical definition:** migration tools is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **home Region**
  - **Technical definition:** home Region is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **migration statuses**
  - **Technical definition:** migration statuses is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Database Migration Service (AWS DMS) and AWS Schema Conversion Tool (AWS SCT)

- **AWS Database Migration Service (AWS DMS) and AWS Schema Conversion Tool (AWS SCT)**
  - **Technical definition:** AWS DMS and AWS SCT provide data replication plus schema assessment and conversion workflows for heterogeneous and homogeneous database migration programs.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **replication instances**
  - **Technical definition:** replication instances are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **replication tasks**
  - **Technical definition:** replication tasks are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **source endpoints**
  - **Technical definition:** source endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **target endpoints**
  - **Technical definition:** target endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **replication subnet groups**
  - **Technical definition:** replication subnet groups are organizational or authorization grouping objects used to simplify assignment of permissions, routing, or shared configuration to multiple principals or resources.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **table mappings**
  - **Technical definition:** table mappings are logical state containers that define access patterns, indexing, throughput, and replication semantics for database workloads.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **validation reports**
  - **Technical definition:** validation reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **SCT migration projects**
  - **Technical definition:** SCT migration projects is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **source schemas**
  - **Technical definition:** source schemas is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **target schemas**
  - **Technical definition:** target schemas is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **assessment reports**
  - **Technical definition:** assessment reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **conversion rules**
  - **Technical definition:** conversion rules is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **converted schema objects**
  - **Technical definition:** converted schema objects are core Amazon S3 namespace or data abstractions that define how object data is stored, addressed, secured, and processed.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS DataSync and AWS Transfer Family

- **AWS DataSync and AWS Transfer Family**
  - **Technical definition:** AWS DataSync and AWS Transfer Family provide managed bulk data movement and file-transfer endpoint services for migration and operational ingestion patterns.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **DataSync agents**
  - **Technical definition:** DataSync agents is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **locations**
  - **Technical definition:** locations is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **tasks**
  - **Technical definition:** tasks are discrete executable or replicative work units scheduled or managed by an AWS service control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **task executions**
  - **Technical definition:** task executions are discrete executable or replicative work units scheduled or managed by an AWS service control plane.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **manifests**
  - **Technical definition:** manifests is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **filters**
  - **Technical definition:** filters is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **reports**
  - **Technical definition:** reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **transfer options**
  - **Technical definition:** transfer options is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **Transfer Family servers**
  - **Technical definition:** Transfer Family servers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **users**
  - **Technical definition:** users are human or service identities represented directly in an AWS identity system and evaluated against attached or inherited access policy.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **workflows**
  - **Technical definition:** workflows is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **connectors**
  - **Technical definition:** connectors is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **identity providers**
  - **Technical definition:** identity providers is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **protocols**
  - **Technical definition:** protocols is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **host keys**
  - **Technical definition:** host keys are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **access endpoints**
  - **Technical definition:** access endpoints are service or network termination points where traffic, identity, or API calls enter a managed AWS component or private-access path.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.

### AWS Backup

- **AWS Backup**
  - **Technical definition:** AWS Backup is AWS's centralized backup-orchestration service for policy-driven backup scheduling, vaulting, copy, and restore workflows across supported services.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **backup plans**
  - **Technical definition:** backup plans are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **backup vaults**
  - **Technical definition:** backup vaults are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **recovery points**
  - **Technical definition:** recovery points is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **backup vault lock configurations**
  - **Technical definition:** backup vault lock configurations are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **copy jobs**
  - **Technical definition:** copy jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **restore jobs**
  - **Technical definition:** restore jobs is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **legal holds**
  - **Technical definition:** legal holds is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **restore testing plans**
  - **Technical definition:** restore testing plans is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### AWS Fault Injection Service (AWS FIS)

- **AWS Fault Injection Service (AWS FIS)**
  - **Technical definition:** AWS Fault Injection Service (AWS FIS) is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **experiment templates**
  - **Technical definition:** experiment templates is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **experiments**
  - **Technical definition:** experiments is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **actions**
  - **Technical definition:** actions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **targets**
  - **Technical definition:** targets is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **stop conditions**
  - **Technical definition:** stop conditions is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **experiment roles**
  - **Technical definition:** experiment roles is an AWS identity principal or trust construct used to obtain temporary permissions through assumption rather than long-lived static credentials.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **experiment reports**
  - **Technical definition:** experiment reports is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

## DR Dependencies From Earlier Weeks

### Amazon Route 53 DNS failover and health checks are covered in Week 2

- **Amazon Route 53 DNS failover and health checks are covered in Week 2**
  - **Technical definition:** Amazon Route 53 DNS failover and health checks are covered in Week 2 are routing constructs that determine how traffic or requests are directed between AWS resources, networks, services, or recovery targets.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.

### Amazon S3 replication

- **Amazon S3 replication**
  - **Technical definition:** Amazon S3 replication are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **Amazon EFS replication**
  - **Technical definition:** Amazon EFS replication are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **Amazon Aurora Global Database**
  - **Technical definition:** Amazon Aurora Global Database is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **Amazon RDS Read Replicas**
  - **Technical definition:** Amazon RDS Read Replicas are mechanisms or target resources used to duplicate state across zones, Regions, or services for scale, recovery, or migration.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **Amazon DynamoDB global tables are covered in Week 4**
  - **Technical definition:** Amazon DynamoDB global tables are covered in Week 4 are logical state containers that define access patterns, indexing, throughput, and replication semantics for database workloads.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.

### IAM emergency access

- **IAM emergency access**
  - **Technical definition:** IAM emergency access is an AWS service entity, control surface, or operational construct that should be understood in terms of control-plane behavior, security boundaries, scaling model, and failure implications.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **CloudTrail evidence**
  - **Technical definition:** CloudTrail evidence are security, compliance, or audit-analysis outputs that summarize detected issues, controls, or supporting proof in AWS governance systems.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **AWS Config drift data**
  - **Technical definition:** AWS Config drift data is the divergence between intended AWS configuration state and live deployed state, usually requiring investigation and remediation.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **AWS KMS keys**
  - **Technical definition:** AWS KMS keys are cryptographic or identity material used to authenticate, sign, encrypt, or validate access and therefore require strict lifecycle, rotation, and audit control.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Backup vault controls are covered in earlier weeks**
  - **Technical definition:** AWS Backup vault controls are covered in earlier weeks are retained recovery copies or policy objects that define how AWS data is protected, copied, and restored under failure or compliance requirements.
  - **Concrete production example:** Example: a customer-facing service scales out during peak demand while platform teams verify identity, health checks, and deployment rollback still behave correctly.

## Controls

### ARC routing control state

- **ARC routing control state**
  - **Technical definition:** ARC routing control state is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **safety rules**
  - **Technical definition:** safety rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **readiness check scope**
  - **Technical definition:** readiness check scope is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cell design**
  - **Technical definition:** cell design is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **recovery group modeling**
  - **Technical definition:** recovery group modeling is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### DRS/MGN replication settings

- **DRS/MGN replication settings**
  - **Technical definition:** DRS/MGN replication settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **launch settings**
  - **Technical definition:** launch settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **staging area configuration**
  - **Technical definition:** staging area configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **point-in-time recovery selection**
  - **Technical definition:** point-in-time recovery selection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **failback settings**
  - **Technical definition:** failback settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **post-launch actions**
  - **Technical definition:** post-launch actions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cutover settings**
  - **Technical definition:** cutover settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### Migration Hub home Region selection

- **Migration Hub home Region selection**
  - **Technical definition:** Migration Hub home Region selection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **application grouping**
  - **Technical definition:** application grouping is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **migration status tracking**
  - **Technical definition:** migration status tracking is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **migration tool integration**
  - **Technical definition:** migration tool integration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### DMS full-load settings

- **DMS full-load settings**
  - **Technical definition:** DMS full-load settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **change data capture (CDC)**
  - **Technical definition:** change data capture (CDC) is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **task settings**
  - **Technical definition:** task settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **validation**
  - **Technical definition:** validation is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **transformation rules**
  - **Technical definition:** transformation rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **endpoint encryption**
  - **Technical definition:** endpoint encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: a migration wave is blocked until packet paths, name resolution, and private service reachability all validate in the destination environment.
- **SCT compatibility assessment**
  - **Technical definition:** SCT compatibility assessment is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **schema mapping**
  - **Technical definition:** schema mapping is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **conversion settings**
  - **Technical definition:** conversion settings is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

### DataSync task options

- **DataSync task options**
  - **Technical definition:** DataSync task options is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during rehearsal, the team validates replication lag, launch settings, and rollback paths before approving final migration cutover.
- **bandwidth limits**
  - **Technical definition:** bandwidth limits is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **verification mode**
  - **Technical definition:** verification mode is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **include/exclude filters**
  - **Technical definition:** include/exclude filters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **encryption**
  - **Technical definition:** encryption is an AWS security control that governs how data, logs, backups, or traffic are cryptographically protected and how key usage is enforced.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **schedule configuration**
  - **Technical definition:** schedule configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.
- **Transfer Family protocol selection**
  - **Technical definition:** Transfer Family protocol selection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **identity provider configuration**
  - **Technical definition:** identity provider configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **IAM roles**
  - **Technical definition:** IAM roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **workflow triggers**
  - **Technical definition:** workflow triggers is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **logging**
  - **Technical definition:** logging is an evidence and observability control that determines whether AWS control-plane or runtime activity can be reconstructed during incident response or audit.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### AWS Backup schedules

- **AWS Backup schedules**
  - **Technical definition:** AWS Backup schedules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **lifecycle rules**
  - **Technical definition:** lifecycle rules is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **AWS Backup Vault Lock**
  - **Technical definition:** AWS Backup Vault Lock is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: the platform tests point-in-time recovery, replica promotion, or object restore before declaring the workload production-ready.
- **cross-Region copy**
  - **Technical definition:** cross-Region copy is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **cross-account copy**
  - **Technical definition:** cross-account copy is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **vault access policies**
  - **Technical definition:** vault access policies is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **restore testing configuration**
  - **Technical definition:** restore testing configuration is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: during an incident review, the team reconstructs who changed a resource, when the change occurred, and whether the live state drifted from baseline.

### FIS stop conditions

- **FIS stop conditions**
  - **Technical definition:** FIS stop conditions is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **IAM experiment roles**
  - **Technical definition:** IAM experiment roles is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: a production API assumes a scoped role at runtime so it can read only one secret path and write only its own application logs.
- **target selection**
  - **Technical definition:** target selection is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **action parameters**
  - **Technical definition:** action parameters is an AWS governance, security, reliability, or operational control that should be evaluated for prevention value, blast-radius impact, and auditability.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.
- **safety guardrails**
  - **Technical definition:** safety guardrails is a governance control used to constrain AWS resource configuration or access so platform standards are enforced before incidents occur.
  - **Concrete production example:** Example: in production, this point becomes relevant when teams need secure change control, reliable scaling, clear ownership, and testable recovery behavior.

## Operations

### Execute ARC routing control tests

- **Execute ARC routing control tests**
  - **Technical definition:** Execute ARC routing control tests is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **readiness checks**
  - **Technical definition:** readiness checks is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **failover rehearsal**
  - **Technical definition:** failover rehearsal is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **game day evidence collection**
  - **Technical definition:** game day evidence collection is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Replicate servers with AWS DRS

- **Replicate servers with AWS DRS**
  - **Technical definition:** Replicate servers with AWS DRS is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **run non-disruptive recovery drills**
  - **Technical definition:** run non-disruptive recovery drills is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **launch recovery instances**
  - **Technical definition:** launch recovery instances is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate failback**
  - **Technical definition:** validate failback is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **review replication lag**
  - **Technical definition:** review replication lag is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Use AWS MGN for migration waves

- **Use AWS MGN for migration waves**
  - **Technical definition:** Use AWS MGN for migration waves is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **test launches**
  - **Technical definition:** test launches is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **cutover launches**
  - **Technical definition:** cutover launches is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **rollback planning**
  - **Technical definition:** rollback planning is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **post-launch validation**
  - **Technical definition:** post-launch validation is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Track portfolio migration status in AWS Migration Hub and report wave readiness

- **Track portfolio migration status in AWS Migration Hub and report wave readiness**
  - **Technical definition:** Track portfolio migration status in AWS Migration Hub and report wave readiness is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Run DMS full-load plus CDC migration

- **Run DMS full-load plus CDC migration**
  - **Technical definition:** Run DMS full-load plus CDC migration is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **validate data**
  - **Technical definition:** validate data is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **troubleshoot replication lag**
  - **Technical definition:** troubleshoot replication lag is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **convert schemas with AWS SCT**
  - **Technical definition:** convert schemas with AWS SCT is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **rehearse cutover/rollback**
  - **Technical definition:** rehearse cutover/rollback is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Use DataSync for file/object seeding

- **Use DataSync for file/object seeding**
  - **Technical definition:** Use DataSync for file/object seeding is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **incremental sync**
  - **Technical definition:** incremental sync is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **transfer validation**
  - **Technical definition:** transfer validation is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **cutover sync**
  - **Technical definition:** cutover sync is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Validate AWS Backup recovery points

- **Validate AWS Backup recovery points**
  - **Technical definition:** Validate AWS Backup recovery points is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **restore testing**
  - **Technical definition:** restore testing is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **immutable backup controls**
  - **Technical definition:** immutable backup controls is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
- **ransomware recovery readiness**
  - **Technical definition:** ransomware recovery readiness is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.

### Run AWS FIS experiments with stop conditions to validate resilience assumptions and blast radius

- **Run AWS FIS experiments with stop conditions to validate resilience assumptions and blast radius**
  - **Technical definition:** Run AWS FIS experiments with stop conditions to validate resilience assumptions and blast radius is an AWS operational activity or troubleshooting workflow used to validate configuration, recover service, reduce risk, or produce deployment and incident evidence.
  - **Concrete production example:** Example: in production, an operator performs this activity during rollout, incident response, or readiness validation to confirm the AWS environment behaves as designed.
