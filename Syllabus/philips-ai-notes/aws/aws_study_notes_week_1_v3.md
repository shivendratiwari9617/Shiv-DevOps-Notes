# AWS Study Notes — Week 1 (Technical Edition)
## Landing Zone, Account Strategy, and Governance

> This version is written as a **technical study README**.  
> It explains **each major Week 1 service and entity clearly**, especially:
> - AWS Organizations and policy types
> - AWS Control Tower
> - IAM and IAM Identity Center
> - Service Catalog, AWS RAM, and StackSets
> - CloudTrail, AWS Config, Security Hub, Audit Manager
> - Supporting governance services: AWS Artifact, AWS Health, Service Quotas

---

# 1. Week 1 Objective

Week 1 is about building a **governed AWS foundation** before workloads are deployed. The technical goal is to create an AWS environment where:

- accounts are structured predictably
- permissions are centrally controlled
- security and compliance logging are enabled from day one
- baseline resources can be rolled out repeatedly
- governance evidence can be produced
- growth across teams and environments remains manageable

This is commonly called a **landing zone**.

A landing zone is not a single AWS service. It is a combination of:
- account structure
- identity model
- policy controls
- logging and audit services
- security services
- deployment baselines
- account provisioning process

---

# 2. AWS Organizations

AWS Organizations is the **central account-governance service** for multi-account AWS environments. It lets you group accounts, apply policy guardrails, manage billing centrally, and integrate organization-wide administration for supported services.

## 2.1 Core entities in AWS Organizations

### Organization
An **organization** is the top-level container that groups AWS accounts under one administrative structure.

Technically, the organization provides:
- centralized account governance
- policy inheritance
- consolidated billing
- integration with organization-aware AWS services

### Management account
The **management account** is the account that creates and owns the organization.

Technical role:
- defines the organization structure
- enables trusted access for integrated services
- attaches policies at the organization root, OU, or account level
- manages consolidated billing

Best practice:
- do not use the management account for routine operations
- keep it heavily restricted and monitored

### Member account
A **member account** is an AWS account inside the organization.

Technical role:
- hosts workloads or platform services
- receives inherited governance from OUs and root-level policies
- can host service-specific delegated administration if supported

### Root
The **root** is the top policy inheritance point inside the organization.

Technical meaning:
- all accounts and OUs ultimately descend from the root
- policies attached at the root affect everything underneath unless otherwise scoped or overridden by stricter denies

### Organizational Unit (OU)
An **OU** is a logical grouping of accounts.

Technical role:
- supports policy inheritance across a collection of accounts
- provides administrative grouping for environments, business units, or functions

Examples:
- Production OU
- Non-Production OU
- Sandbox OU
- Security OU

### Delegated administrator
A **delegated administrator** is a member account authorized to administer a supported AWS service across the organization.

Technical value:
- reduces dependence on the management account
- improves separation of duties
- lets central teams operate services such as Security Hub from a designated security account

## 2.2 Why AWS uses multi-account architecture

A multi-account design is preferred because it creates:
- **blast-radius isolation** — one account failure or compromise does not automatically affect all workloads
- **billing separation** — easier cost ownership and reporting
- **permission separation** — easier to isolate production from development access
- **compliance segmentation** — clearer boundaries for regulated workloads
- **operational clarity** — teams know where specific workloads belong

## 2.3 AWS Organizations policy types

This part must be understood precisely.

### Service Control Policies (SCPs)
An SCP defines the **maximum available permissions** for principals in accounts under a root, OU, or account.

Technical behavior:
- SCPs do **not** grant permissions
- IAM identity policies must still grant access
- the effective permission is bounded by the SCP
- an explicit deny in an SCP overrides an allow elsewhere

**Example**
If a role in a member account has permission to call `ec2:RunInstances`, but an SCP denies EC2 in that OU, the call fails.

**Use cases**
- restrict allowed Regions
- prevent disabling CloudTrail
- prevent leaving the organization
- block creation of certain services in sandbox or regulated OUs

### Tag policies
Tag policies standardize how tags are named and valued across the organization.

Technical purpose:
- define allowed tag keys
- define acceptable tag value formats
- improve tagging consistency for governance, cost allocation, and automation

**Example**
Enforce tags like:
- `Environment = Dev|Test|Prod`
- `CostCenter = CC-####`
- `Owner = team-name`

Tag policies do not behave like IAM permission policies. They are governance standards for tag usage.

### Backup policies
Backup policies define centralized backup expectations using AWS Backup across accounts in the organization.

Technical purpose:
- assign standardized backup behavior to accounts or OUs
- align backup plans and retention expectations organizationally

**Example**
Production OU receives daily backups with longer retention, while sandbox OU gets lighter retention.

### AI services opt-out policies
These policies allow organization-level governance for certain AI service data-handling settings.

Technical purpose:
- apply centralized controls in specialized service domains
- ensure organization-wide governance of service behavior where supported

### Resource Control Policies (RCPs)
RCPs are an organization-level governance mechanism for supported resources.

Study understanding:
- they extend centralized governance beyond classic SCP-only use
- they should be understood as part of AWS’s broader evolution in organization-wide policy governance

## 2.4 Trusted access

**Trusted access** allows supported AWS services to integrate directly with Organizations.

Technical effect:
- a service can read organization structure and account relationships
- a service can support org-wide setup, aggregation, or delegated administration

Examples:
- Security Hub delegated admin
- Audit Manager multi-account use cases
- StackSets with service-managed permissions

## 2.5 Consolidated billing

Consolidated billing groups charges across member accounts under the organization.

Technical value:
- single payer model
- central finance visibility
- separate usage by account still preserved
- supports chargeback/showback models

## 2.6 Region governance

Organizations-based governance often includes approved-Region strategy.

Technical use:
- combine organization policy and operational process to restrict workloads to approved Regions
- reduce compliance, cost, and sprawl risk

---

# 3. AWS Control Tower

AWS Control Tower is a **landing-zone orchestration service** built on top of AWS Organizations and other AWS governance components.

It does not replace Organizations.  
It operationalizes it.

## 3.1 What Control Tower technically provides

Control Tower helps you:
- establish a landing zone
- enroll and govern accounts
- apply prebuilt controls
- monitor drift
- provision new accounts through Account Factory

## 3.2 Core entities in Control Tower

### Landing zone
A **landing zone** in Control Tower is the governed multi-account environment configured by the service.

Technical contents usually include:
- organization structure
- logging setup
- baseline governance
- shared access patterns
- preconfigured controls

### Registered OU
A **registered OU** is an OU brought under Control Tower governance.

Technical meaning:
- Control Tower monitors and applies its governance model to that OU
- account enrollment behavior and controls depend on this governance scope

### Governed account
A **governed account** is an enrolled account managed under Control Tower governance.

### Account Factory
**Account Factory** is the account provisioning workflow for creating new accounts with standardized setup.

Technical role:
- account vending
- baseline consistency
- standard placement into the right OU
- predefined setup choices

### Controls
Control Tower controls are governance rules.

Three major types:

#### Preventive controls
Stop a disallowed action before it succeeds.

Technical implementation idea:
- usually based on policy enforcement

#### Detective controls
Detect a condition after it exists.

Technical implementation idea:
- typically based on evaluation and compliance detection

#### Proactive controls
Evaluate resources earlier in the deployment lifecycle to reduce noncompliant creation.

### Drift
**Drift** means the current environment differs from the expected governed baseline.

Technical examples:
- required roles removed
- logging path broken
- baseline settings modified outside approved methods

## 3.3 Control Tower technical value

Control Tower is useful because it reduces the need to manually assemble all landing-zone pieces one by one.

It is especially strong for:
- standardization
- governance-first account creation
- organization-scale control application

---

# 4. AWS IAM

IAM is the **authorization system** of AWS. It controls who can do what on which resource, under what conditions.

## 4.1 Core IAM entities

### IAM user
A long-term identity in an AWS account.

Technical characteristics:
- can have password for console access
- can have access keys for API access
- can have MFA
- can be assigned policies directly or through groups

Modern best practice:
- minimize use of IAM users for workforce access

### IAM group
A collection of IAM users.

Technical role:
- applies shared permissions to many users at once
- used only for IAM users, not roles

### IAM role
A temporary, assumable identity.

Technical characteristics:
- has no long-term password like a user
- is assumed by a trusted principal
- receives temporary credentials via STS
- can be used by AWS services, users, federated identities, and cross-account principals

This is the preferred mechanism for:
- applications
- cross-account access
- service permissions
- workforce access through federation

### IAM policy
A JSON policy document that defines allowed or denied actions.

Technical elements:
- `Effect`
- `Action`
- `Resource`
- `Condition`

### Trust policy
A special policy on a role that defines **who can assume the role**.

Technical distinction:
- trust policy answers **who may enter the role**
- permission policy answers **what the role can do once assumed**

### Permissions boundary
A policy that caps the maximum permissions a principal can receive.

Technical use:
- let teams create roles without letting them exceed a defined privilege ceiling
- enable delegated administration safely

### Instance profile
A container used to attach an IAM role to an EC2 instance.

Technical importance:
- allows applications on EC2 to obtain temporary credentials without storing secrets

### Access key
A long-term credential pair for programmatic access.

Technical risk:
- easy to leak
- often forgotten
- harder to govern than temporary credentials

### MFA device
A second authentication factor associated with a principal.

Technical use:
- protects privileged console access
- should be mandatory for root and high-privilege identities

## 4.2 IAM policy evaluation model

AWS evaluates access using multiple policy layers.

The effective decision can be influenced by:
- identity-based policies
- resource-based policies
- SCPs
- permissions boundaries
- session policies
- explicit denies
- conditions

The key study rule:
> **Access must be allowed by the relevant policies and not denied by any stronger control.**

## 4.3 AWS STS

AWS Security Token Service issues **temporary security credentials**.

Technical role:
- enables role assumption
- enables federation
- supports cross-account access
- reduces reliance on long-lived credentials

## 4.4 Cross-account IAM access

Cross-account access works by combining:
- trust policy in target account
- permission policy on the role
- source principal with ability to assume the role
- STS temporary credential issuance
- organization and SCP constraints

**Example**
A developer group signs in through Identity Center and assumes a read-only production role in a separate account.

## 4.5 IAM Access Analyzer

IAM Access Analyzer is a governance and validation tool for access policies and resource sharing.

### Analyzer
An analyzer defines a trust boundary and scans for access patterns.

### Findings
A finding identifies a potential or actual access condition, such as:
- resource shared externally
- role trusted by an unintended principal
- unused access

### Archive rules
Archive rules suppress expected findings so analysts can focus on meaningful exceptions.

### Access previews
Access previews simulate the effect of a proposed policy before deployment.

### Policy validation
Policy checks identify malformed or risky policy patterns.

Technical value:
- reduces accidental public or cross-account exposure
- improves least-privilege design
- helps validate IAM and resource-based access posture

---

# 5. AWS IAM Identity Center

IAM Identity Center is AWS’s centralized workforce access service for AWS accounts and supported business applications.

## 5.1 Core entities

### Identity source
The source of users and groups.

Technical meaning:
- can be an internal identity store or an integrated external identity provider

### User
A workforce identity managed through the chosen identity source.

### Group
A collection of users used for assignment at scale.

### Permission set
A reusable access definition used to grant access to AWS accounts.

Technical role:
- acts like an access template
- becomes provisioned permissions in target accounts

### Account assignment
Maps a user or group to:
- a permission set
- a target AWS account

### Application assignment
Grants user/group access to an integrated application.

## 5.2 Technical value of Identity Center

It centralizes workforce access so organizations do not need to create local IAM users in every account.

Benefits:
- centralized sign-in
- group-based access
- better onboarding/offboarding
- standardized account access
- better privileged-access control

## 5.3 Session duration

Permission sets can define session duration.

Technical importance:
- shorter sessions for privileged roles reduce risk
- longer sessions may be allowed for lower-risk work

---

# 6. AWS Service Catalog

AWS Service Catalog provides **governed self-service deployment**.

It lets platform teams publish approved products for users to deploy.

## 6.1 Core entities

### Portfolio
A collection of products offered to users or accounts.

### Product
A deployable approved offering, such as:
- a baseline VPC
- a standard EC2 build
- a database template
- a platform account blueprint

### Provisioning artifact
A version of a product, often backed by infrastructure code.

### Provisioned product
A deployed instance of a product.

### Constraint
A rule that limits or governs how a product can be launched or managed.

Examples:
- launch constraints
- template constraints
- notification constraints

### Launch path
The route through which a user is permitted to launch a product.

## 6.2 Why Service Catalog matters technically

It separates:
- **what users are allowed to deploy**
from
- **how the platform team defines approved infrastructure**

This is useful for controlled self-service and platform standardization.

---

# 7. AWS Resource Access Manager (AWS RAM)

AWS RAM shares supported AWS resources across accounts.

## 7.1 Core entities

### Resource share
A container that defines what resources are shared and with whom.

### Principal
The consumer of the shared resource.

Can be:
- account
- OU
- organization

### Resource
The supported AWS resource being shared.

### Managed permission
An AWS-defined permission set controlling how the shared resource can be used.

### Customer managed permission
A customized permission model for supported sharing scenarios.

### Invitation
A share acceptance step required in certain sharing scenarios.

## 7.2 Technical purpose

RAM is used when a resource should remain centrally owned but be consumable by other accounts.

Examples:
- shared networking components
- shared resolver rules
- shared Transit Gateway-related access where supported
- centralized platform resources

---

# 8. AWS CloudFormation StackSets

StackSets extend CloudFormation to multi-account, multi-Region deployments.

## 8.1 Core entities

### StackSet
The top-level definition for deploying a CloudFormation stack repeatedly across targets.

### Stack instance
A deployed copy of the StackSet in a specific account and Region.

### Stack operation
An action such as create, update, or delete performed across stack instances.

### Administrator account
The account from which the StackSet is managed.

### Execution role
The role assumed in target accounts to deploy resources.

### Target account / target OU
The deployment scope.

## 8.2 Permission models

### Service-managed permissions
Integrated with Organizations.

Technical value:
- simpler org-wide deployment
- good for centralized governance rollouts

### Self-managed permissions
You explicitly manage administrator and execution roles.

Technical value:
- more manual control
- useful in some specialized governance models

## 8.3 Why StackSets matter

They allow standard resources to be deployed consistently at scale.

Examples:
- baseline IAM roles
- Config rules
- SNS topics
- logging resources
- operational automation components

---

# 9. AWS CloudTrail

CloudTrail records AWS API activity and account events.

It is one of the most important audit services in AWS.

## 9.1 Core entities

### Trail
A configuration that delivers CloudTrail events to a chosen destination.

### Organization trail
A trail applied across the AWS organization.

Technical value:
- central audit visibility
- consistent logging across accounts

### Event data store
A more flexible event storage construct for advanced retention and analysis use cases.

### Management events
Control-plane operations such as:
- creating resources
- modifying IAM
- updating security settings

### Data events
High-volume resource-level events such as:
- S3 object access
- Lambda invocation activity
- other supported data-plane operations

### Insights events
Events generated when unusual API activity patterns are detected.

## 9.2 What CloudTrail technically answers

CloudTrail helps answer:
- who made the API call
- what action was called
- when it happened
- from where it was called
- which resource was targeted
- whether the call succeeded or failed

## 9.3 Why organization trails matter

In multi-account environments, local per-account logging is not enough.

An organization trail provides:
- consistent organization-wide audit collection
- simpler forensic review
- stronger governance baseline

## 9.4 Example use case

A security group was modified unexpectedly in production.

CloudTrail can show:
- which role made the change
- whether it was assumed through SSO
- the source IP
- the exact API call and timestamp

---

# 10. AWS Config

AWS Config records resource configuration state and evaluates compliance.

If CloudTrail tells you **who changed something**, Config tells you **what the resource configuration is and whether it matches policy**.

## 10.1 Core entities

### Configuration recorder
Captures supported resource configuration changes.

### Delivery channel
Sends configuration snapshots/history to the configured destination.

### Configuration item
A point-in-time record of a resource’s configuration.

### Config rule
A compliance rule that evaluates whether a resource matches a condition.

### Conformance pack
A packaged collection of Config rules and compliance expectations.

### Configuration aggregator
Aggregates compliance and configuration state across accounts and Regions.

### Remediation action
An automated or guided action to fix noncompliant resources.

## 10.2 Technical behavior

Config can:
- track how resource settings change over time
- evaluate resources continuously or periodically
- provide compliance snapshots
- support organization-wide compliance posture via aggregation

## 10.3 Example use cases

- detect public S3 buckets
- verify encryption is enabled
- ensure approved instance types are used
- validate logging configuration exists

## 10.4 CloudTrail vs Config

A strong Week 1 understanding requires this distinction:

- **CloudTrail** = API event history
- **Config** = resource state and compliance history

---

# 11. AWS Security Hub

Security Hub is the **central security findings aggregation and posture management service**.

## 11.1 Core entities

### Standard
A published set of security best-practice controls.

### Control
An individual security requirement/check.

### Finding
A security-relevant result or issue detected by AWS or integrated services.

### Insight
A way to group and analyze findings.

### Automation rule
A rule to update or route findings automatically.

### Finding aggregator
Aggregates findings across Regions.

### Administrator account
The delegated admin account operating Security Hub centrally.

### Member account
Accounts whose findings contribute to the centralized view.

## 11.2 Technical role

Security Hub:
- ingests findings from supported AWS security services and integrations
- normalizes them into a common model
- allows centralized analysis and workflow

It does not replace CloudTrail or Config. It sits above them in the governance stack.

## 11.3 Example flow

1. Config detects a noncompliant resource  
2. a related security finding is surfaced  
3. Security Hub centralizes and prioritizes the finding  
4. security team investigates and routes remediation

---

# 12. AWS Audit Manager

Audit Manager organizes evidence for audits and compliance assessments.

## 12.1 Core entities

### Assessment
A structured audit project based on a framework.

### Framework
A collection of controls aligned to a compliance objective.

### Control set
A group of related controls within an assessment.

### Evidence folder
The container storing evidence for a control.

### Evidence
A collected proof item relevant to a control.

### Evidence Finder
A search capability for locating relevant evidence.

### Assessment report
A generated report summarizing the assessment.

## 12.2 Technical role

Audit Manager helps move from:
- raw governance signals
to
- organized evidence suitable for audit and compliance workflows

It is not primarily a detection service.  
It is an **evidence and assessment management service**.

---

# 13. Supporting Governance Services

## 13.1 AWS Artifact

AWS Artifact provides:
- AWS compliance reports
- security/compliance documents
- agreement management

### Technical use
- retrieve formal AWS assurance documents
- support audit and customer due-diligence requests

## 13.2 AWS Health

AWS Health provides account-specific and organization-relevant service event information.

### Core entities
- health event
- affected entity
- event status code
- organizational view

### Technical value
It tells you not just that AWS has an issue, but whether **your resources/accounts** are affected.

## 13.3 AWS Service Quotas

Service Quotas manages limits and quota increase workflows.

### Core entities
- service quota
- applied quota value
- usage metric
- quota increase request
- quota request template

### Technical importance
Quotas are part of platform readiness. Many migrations and launches fail because quotas were never reviewed in advance.

---

# 14. Relationship Between the Services

This is the most important conceptual view for Week 1.

- **AWS Organizations** provides the multi-account hierarchy and policy scope.
- **Control Tower** operationalizes the landing zone.
- **IAM** controls authorization.
- **IAM Identity Center** centralizes workforce access.
- **Service Catalog** publishes approved deployable products.
- **AWS RAM** shares centrally owned supported resources.
- **StackSets** deploy baseline resources across many accounts/Regions.
- **CloudTrail** records API activity.
- **AWS Config** records resource state and compliance.
- **Security Hub** centralizes security findings.
- **Audit Manager** organizes evidence.
- **Artifact** provides AWS assurance documents.
- **AWS Health** shows operational events affecting your environment.
- **Service Quotas** protects rollout readiness and scalability.

---

# 15. Technical Example: Public S3 Bucket Incident

Suppose a bucket becomes public in a production account.

- **IAM / Identity Center** determine which user/role had access to make the change.
- **CloudTrail** shows who changed the bucket policy, when, and via which API call.
- **AWS Config** records that the bucket configuration changed and may mark it noncompliant.
- **Security Hub** aggregates the resulting finding for central review.
- **Audit Manager** can link the evidence to a control assessment.
- **Organizations / SCPs** might have prevented the action if designed to restrict that pattern.
- **Control Tower** may show related governance posture depending on enabled controls.

This is the Week 1 mindset: understand not only each service individually, but how they work together.

---

# 16. Week 1 Revision Checklist

Make sure you can clearly explain:

- what an organization, root, OU, management account, and member account are
- how SCPs technically affect permissions
- how Control Tower differs from Organizations
- what a trust policy does
- what a permissions boundary does
- what STS does
- what IAM Access Analyzer findings represent
- what a permission set is in Identity Center
- how Service Catalog differs from AWS RAM
- what StackSets deploy
- what CloudTrail management vs data events are
- what Config rules and aggregators do
- what Security Hub findings and standards are
- what Audit Manager evidence is
- why AWS Artifact, Health, and Service Quotas matter operationally

---

# 17. Official AWS Documentation References

- AWS Organizations  
  https://docs.aws.amazon.com/organizations/
- AWS Control Tower  
  https://docs.aws.amazon.com/controltower/
- IAM User Guide  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/
- IAM Access Analyzer  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html
- IAM Identity Center  
  https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html
- AWS Service Catalog  
  https://docs.aws.amazon.com/servicecatalog/
- AWS RAM  
  https://docs.aws.amazon.com/ram/
- AWS CloudFormation StackSets  
  https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html
- AWS CloudTrail  
  https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html
- AWS Config  
  https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html
- AWS Security Hub  
  https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html
- AWS Audit Manager  
  https://docs.aws.amazon.com/audit-manager/latest/userguide/what-is.html
- AWS Artifact  
  https://docs.aws.amazon.com/artifact/
- AWS Health  
  https://docs.aws.amazon.com/health/
- AWS Service Quotas  
  https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html

---

# 18. Final Summary

Week 1 is the **governance control layer** of AWS.

The correct technical understanding is:

- Organizations gives the **account governance model**
- Control Tower gives the **landing-zone implementation model**
- IAM and Identity Center give the **access-control model**
- Service Catalog, RAM, and StackSets give the **platform rollout model**
- CloudTrail, Config, Security Hub, and Audit Manager give the **audit/compliance/security visibility model**
- Artifact, Health, and Service Quotas give the **supporting assurance and readiness model**

When you can explain each service, each entity, and how the layers connect, your Week 1 understanding becomes platform-grade rather than just exam-grade.
