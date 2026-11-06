# Production Deployment Projects for a Senior Platform Engineer
**Product-company focused | Terraform-first | Reliability, least-privilege IAM, automated drift detection**

## Executive Summary

This document proposes three production-grade platform engineering projects designed for a strong product company scaling across multiple teams, environments, and compliance expectations. The goal is not just to provision cloud infrastructure, but to create **internal platform capabilities** that improve developer velocity while reducing operational risk.

Across all three projects, the platform strategy is built on the same core principles:

- **Reliability by default** through failure-domain isolation, safe rollouts, observability, and recovery playbooks
- **Least-privilege IAM** with short-lived credentials, permission boundaries, scoped CI/CD identities, and auditable break-glass access
- **Automated drift detection** as an operating discipline, with ownership, SLAs, and remediation workflows
- **Terraform as the infrastructure control plane**, with reusable modules, versioned releases, remote state isolation, policy gates, and controlled promotion
- **Paved roads and self-service contracts** so product teams can ship on approved patterns rather than inventing infrastructure repeatedly
- **Day-2 operational maturity** including upgrades, deprecations, game days, quota management, and post-incident feedback into platform code

This is the kind of work a Director of Platform Engineering or Head of Infrastructure should expect from a senior platform engineer operating in a modern product organization.

---

# 1) Multi-Account AWS EKS Platform with Terraform, GitOps, and Shared Services

## 1.1 Business Context / Problem Statement

A scaling product company has multiple teams deploying customer-facing APIs, internal services, data workers, and scheduled jobs. Teams need Kubernetes flexibility, but the current operating model is fragmented:

- inconsistent cluster provisioning
- broad and unsafe access patterns
- manual console changes
- unreliable add-on lifecycle management
- weak production deployment controls
- limited visibility into drift and cluster health

The platform objective is to provide a **secure, reliable, reusable EKS foundation** that supports product teams through a paved-road model rather than bespoke cluster engineering.

## 1.2 Platform Consumers and Operating Model

### Primary consumers

- **Product engineering teams** deploying services to approved Kubernetes patterns
- **SRE** operating reliability controls, incident readiness, and SLO tooling
- **Security** enforcing IAM, network, runtime, and audit controls
- **Compliance / risk** reviewing evidence for access, changes, encryption, and logging
- **Platform engineering** owning the cluster foundation, service templates, and release lifecycle

### Responsibility split

**Platform team owns:**

- account and environment baseline
- EKS cluster lifecycle
- networking and shared services
- GitOps foundations
- add-on lifecycle and policy controls
- Terraform modules, CI/CD guardrails, and drift management

**Product teams own:**

- application manifests within approved contracts
- service-specific scaling and resource requests
- on-call ownership for their workloads
- SLOs for their product services
- rollout readiness and rollback validation

## 1.3 Target Architecture Overview

The architecture separates concerns by account and environment:

- **Shared services account** for observability, registries, CI identity integration, and DNS control points
- **Production platform account** for production EKS and shared production dependencies
- **Non-production platform account** for dev/stage clusters
- **Security/log archive account** for audit retention and immutable evidence

Core components:

- Amazon EKS across 3 AZs
- managed node groups plus optional Karpenter for elasticity
- private subnets for worker nodes
- ingress via AWS Load Balancer Controller
- workload identity via IRSA
- ExternalDNS with tightly scoped permissions
- GitOps controller for cluster add-ons and workloads
- centralized logs, metrics, traces, and alerting
- policy enforcement for admission and workload posture
- secrets integration without static credentials embedded in workloads

## 1.4 What Terraform Manages vs GitOps vs App Pipelines

### Terraform manages

- VPCs, subnets, routing, endpoints, security groups
- EKS cluster, node groups, OIDC provider, IAM roles
- shared observability sinks and storage backends
- cluster add-on IAM scaffolding
- Route 53 zones and approved DNS integrations
- KMS keys, backend state resources, and baseline policies

### GitOps manages

- Kubernetes add-ons after foundational bootstrap
- namespaces, network policies, resource quotas
- ingress objects, service meshes, policy controllers, and workload release manifests
- per-team approved workload deployment patterns

### Application pipelines manage

- image build, scan, sign, and publish
- application tests and progressive delivery triggers
- release promotion and rollback orchestration
- service-level verification after deployment

### Boundary rule

Terraform provisions the **platform substrate**. GitOps reconciles the **cluster desired state**. Application pipelines deliver **versioned workloads**. Mixing those boundaries creates unclear ownership and unstable reconciliation loops.

## 1.5 Core Terraform Structure

Use a layered root-module model to keep blast radius small and review scope clear.

### Layering

- `org-bootstrap`
- `network`
- `eks-cluster`
- `platform-addons-bootstrap`
- `shared-services`
- `team-tenancy-baseline`

### Suggested repository

```text
repo/
├── modules/
│   ├── account-baseline/
│   ├── remote-state/
│   ├── vpc/
│   ├── eks-cluster/
│   ├── eks-nodegroup/
│   ├── irsa-role/
│   ├── kms-key/
│   ├── observability-baseline/
│   ├── dns-delegation/
│   ├── team-namespace-baseline/
│   └── policy-baseline/
├── live/
│   ├── prod/
│   │   ├── bootstrap/
│   │   ├── network/
│   │   ├── eks/
│   │   ├── shared-services/
│   │   └── team-baselines/
│   ├── stage/
│   └── dev/
├── policies/
├── tests/
├── docs/
└── .github/workflows/
```

### Remote state design

- S3 backend with versioning, KMS encryption, access logging, and bucket policy restrictions
- DynamoDB locking
- one state file per layer and environment
- state readers restricted to platform and selected CI roles
- no shared monolithic state across unrelated concerns

### Module lifecycle rules

- semantic versioning for every reusable module
- patch releases for fixes without interface change
- minor releases for backward-compatible additions
- major releases for breaking changes only
- deprecation notice published before removing inputs or outputs
- minimum 90-day sunset for deprecated service templates in active use

## 1.6 Self-Service Contract

The platform should expose a narrow, opinionated contract to product teams.

### Example service catalog input

```yaml
service_name: payments-api
runtime_class: web
environment: prod
exposure: internal
scaling_class: medium
data_access:
  - rds:orders-read
  - s3:receipts-write
slo_tier: tier-1
```

### Example outputs inherited automatically

- namespace and service account
- workload identity binding
- default dashboards and alerts
- log retention policy
- pod disruption budget
- network policy baseline
- resource quota and limit range
- standard ingress and TLS wiring

### Intentionally not customizable

- cluster version
- node OS image family
- global CNI settings
- log retention defaults below baseline
- admission policy bypass
- unrestricted egress
- wildcard IAM permissions

That is the core of a strong product-company platform: product teams choose **business-relevant parameters**, not low-level cloud plumbing.

## 1.7 Reliability Design

### Reliability objectives

- production cluster availability target: **99.95%**
- critical ingress success rate target: **99.9% over 30 days**
- platform MTTR target for cluster-level incidents: **< 30 minutes**
- deployment rollback initiation target after bad release detection: **< 10 minutes**
- failed deployment rate target: **< 5% monthly**
- node replacement SLO for standard failures: **< 15 minutes**

### Reliability controls

- multi-AZ clusters and node pool spread
- dedicated system and application node pools
- topology spread constraints and PDBs
- pre-warmed capacity for critical namespaces
- cluster autoscaling with disruption-aware settings
- progressive rollout support for workloads
- synthetic checks for ingress, DNS, and service reachability
- centralized alerting with ownership metadata
- etcd/control plane handled by managed service, reducing operator burden
- backup strategy for Kubernetes state and configuration metadata where appropriate
- DR playbooks for failed upgrades, AZ loss, DNS issues, and admission-control regressions

### Day-2 operational cadence

- weekly production reliability review
- monthly Kubernetes version and add-on patch review
- quarterly cluster recovery game day
- quarterly capacity review on node pools, quotas, and cost hotspots
- every sev-1/sev-2 incident feeds corrective actions into modules, templates, policies, or runbooks

## 1.8 Least-Privilege IAM Strategy

### Design principles

- no long-lived AWS keys in CI
- OIDC federation for CI/CD
- IRSA for workloads
- separate plan and apply roles
- permission boundaries for team-delegated access
- time-bound break-glass access with MFA and ticket reference
- `iam:PassRole` tightly restricted by approved role paths and tags

### Role examples

- **terraform-plan-prod-eks**: read-only plus minimal state/KMS access
- **terraform-apply-prod-eks**: scoped to EKS layer resources only
- **gitops-controller-role**: namespace or add-on scoped cloud access only
- **developer-read-platform**: logs, metrics, describe APIs, no mutation
- **incident-breakglass-prod**: elevated but time-bound, audited, and approval-gated

### Example IAM pseudocode

```hcl
role "terraform_apply_prod_eks" {
  trust = ["github-oidc:platform-repo-prod-workflow"]
  permissions = [
    "eks:* on approved cluster arns",
    "ec2:* on tagged networking resources for this layer",
    "iam:PassRole on /platform/eks/*",
    "kms:Decrypt on state key and platform keys",
    "s3:GetObject|PutObject on backend bucket prefix"
  ]
  conditions = [
    "aws:RequestedRegion in approved regions",
    "resource tags managed-by = terraform",
    "principal tag env = prod"
  ]
}
```

### Access governance cadence

- quarterly role review for platform and product teams
- semiannual review of exception-based permissions
- immediate review after security incidents or privilege misuse
- access evidence stored for audit and change management

## 1.9 Automated Drift Detection Strategy

### Drift objectives

- production drift detection cadence: **nightly**
- drift triage SLA for production: **same business day**
- unauthorized production drift remediation SLA: **24 hours**
- state hygiene review cadence: **weekly**
- false-positive drift threshold: **< 10% of detections**

### Drift pipeline

```text
1. Checkout Terraform repo
2. Authenticate to read-only drift role via OIDC
3. terraform init against remote backend
4. terraform fmt -check && terraform validate
5. run policy checks
6. terraform plan -detailed-exitcode
7. publish plan artifact and machine-readable summary
8. route non-zero drift to Slack + incident/task system
9. require owner assignment and remediation status
```

### Ownership model

- platform foundation drift -> platform team owns remediation
- shared networking drift -> platform plus security review
- team tenancy drift -> platform assigns back to owning team if within delegated scope
- production unauthorized drift -> incident opened automatically
- expected operational variance -> documented allowlist with expiry

### Weekly drift review agenda

- open drift items by severity
- aging drift > SLA
- repeated manual-change patterns
- state import / moved block hygiene
- exceptions nearing expiry
- top recurring module defects driving drift

## 1.10 CI/CD Workflow

### Release model

- PR validation on fmt, validate, lint, policy, security, and contract tests
- plan attached to PR
- required reviewers for production-impacting changes
- environment promotion path: `dev -> stage -> prod`
- production apply only after approved plan and change window validation
- serialized applies per state
- post-apply smoke checks and platform health checks

### Approval model

**Standard production change**

- author opens PR
- platform code owner reviews
- security review required for IAM/network changes
- approved plan is immutable for apply window
- production apply by pipeline identity only

**Emergency production change**

- incident declared
- incident commander or designated approver authorizes emergency apply
- break-glass use logged automatically
- follow-up reconciliation PR required within 24 hours
- post-incident review captures missing platform control

## 1.11 Security and Compliance Controls

- audit logs for AWS API and control plane events
- encryption in transit and at rest
- private endpoints where possible
- admission controls for privileged pods, image source, and required labels
- egress controls and VPC endpoints to reduce public exposure
- evidence collection for who changed what, when, and through which approval path

## 1.12 Risks, Trade-offs, and Failure Modes

### Trade-offs

- fewer larger clusters lower cost but increase blast radius
- more clusters improve isolation but increase operational overhead
- stricter guardrails improve safety but can slow early adoption if DX is poor

### Common failure modes

- over-broad `iam:PassRole`
- unmanaged add-on drift between Terraform and GitOps
- weak namespace tenancy controls
- manual Route 53 edits for ingress shortcuts
- policy exceptions granted without expiry
- platform becoming ticket-driven instead of self-service-driven

### Success metrics after 6 months

- **80%+** of new Kubernetes services launched through paved-road templates
- production infra change lead time reduced to **< 1 day**
- unauthorized production drift resolved within SLA **95%+** of the time
- cluster-related sev-1 incidents reduced by **30%**
- onboarding a new service to production takes **< 2 days**
- manual cloud console changes in platform accounts reduced materially and tracked

---

# 2) Multi-Region AWS Serverless API Platform with API Gateway, Lambda, DynamoDB, and Route 53 Failover

## 2.1 Business Context / Problem Statement

A product company needs a resilient public API platform that can handle traffic spikes, regional failures, and frequent releases without the operational overhead of managing clusters for every service. The current environment has inconsistent deployment patterns, uneven IAM hygiene, and failover plans that exist in slides rather than rehearsed operations.

The goal is to build a **multi-region serverless platform** that provides strong reliability, clear ownership, low operational burden, and a safe path for product teams to launch APIs rapidly.

## 2.2 Platform Consumers and Operating Model

### Primary consumers

- backend product teams shipping APIs
- SRE responsible for reliability targets and recovery posture
- security reviewing WAF, IAM, and secrets controls
- platform team owning common modules, routing patterns, and release governance

### Responsibility split

**Platform owns:**

- multi-region routing patterns
- Terraform modules for API, Lambda, tables, alarms, and failover
- IAM guardrails and CI/CD controls
- standard observability and runtime posture
- DR testing framework and runbooks

**Product teams own:**

- handler logic and business behavior
- API-level SLOs
- idempotency semantics and retry correctness
- feature flags and release safety checks
- on-call for service-specific incidents

## 2.3 Target Architecture Overview

- API Gateway in two AWS regions
- Lambda functions deployed immutably and promoted across environments
- DynamoDB global tables or a consciously chosen primary/replica strategy
- Route 53 health-check-based failover
- regional alarms plus centralized observability
- WAF and throttling controls
- DLQs, retries, and event replay patterns
- KMS-backed secrets and encrypted environment variables

This platform is appropriate for customer-facing APIs, webhooks, and event-driven product capabilities that need high availability with lower infrastructure management overhead.

## 2.4 What Terraform Manages vs App Pipelines

### Terraform manages

- API Gateway stages, custom domains, WAF associations
- Lambda infrastructure configuration
- DynamoDB tables and replication settings
- Route 53 failover records and health checks
- log groups, alarms, retention, KMS keys, and IAM roles
- CI identities and deployment permissions

### Application pipelines manage

- build, test, scan, and package Lambda artifacts
- publish immutable versions
- progressive rollout using aliases / weighted traffic
- health-gated rollback decisions
- business smoke tests and contract tests

### Boundary rule

Terraform owns **service infrastructure and safety rails**. Application pipelines own **artifact release**. Runtime config changes outside that model are treated as drift.

## 2.5 Core Terraform Structure

### Layering

- `global-foundation`
- `regional-platform-primary`
- `regional-platform-secondary`
- `shared-ci-and-artifacts`
- `service-stacks`

### Suggested repository

```text
repo/
├── modules/
│   ├── api-gateway/
│   ├── lambda-service/
│   ├── lambda-alias-rollout/
│   ├── dynamodb-global/
│   ├── route53-failover/
│   ├── waf/
│   ├── alarms/
│   ├── kms/
│   └── service-boundary/
├── live/
│   ├── prod/
│   │   ├── global/
│   │   ├── us-east-1/
│   │   └── us-west-2/
│   ├── stage/
│   └── dev/
├── policies/
├── tests/
└── .github/workflows/
```

### State design

- separate state for global and each region
- no mixing of primary and secondary region resources in one state
- backend encryption and locking mandatory
- regional apply pipelines isolated to reduce simultaneous blast radius

## 2.6 Self-Service Contract

### Example service catalog input

```yaml
service_name: billing-public-api
api_type: public-json
regions:
  - us-east-1
  - us-west-2
availability_tier: tier-1
auth_mode: jwt
data_pattern: global-table
rollout_strategy: canary
waf_profile: public-standard
```

### Outputs inherited automatically

- regional API Gateway stages
- Lambda roles and log groups
- alarms for error rate, latency, and throttling
- WAF attachment
- Route 53 records and failover policy
- default dashboards and structured logging format
- deployment alias strategy and rollback hooks

### Not customizable

- disabling DLQ for asynchronous flows
- reducing log retention below baseline
- bypassing WAF for public APIs
- wildcard IAM on execution role
- skipping health checks for tier-1 services

## 2.7 Reliability Design

### Reliability targets

- public API availability target: **99.95%**
- p95 latency target for critical APIs: **< 300 ms** excluding downstream dependency degradation
- change failure rate target: **< 7%**
- MTTR target for regional routing incidents: **< 15 minutes**
- rollback initiation after canary failure: **< 5 minutes**
- quarterly failover success target: **100% test completion**

### Reliability controls

- regional independence with minimal shared mutable state
- Route 53 failover or active/active weighting based on service tier
- Lambda reserved concurrency for critical functions
- throttling and circuit-breaking patterns
- DLQ and retry design for async paths
- idempotency keys for write operations
- DynamoDB PITR and replication monitoring
- synthetic probes from multiple regions
- feature-flag-aware rollback behavior
- immutable release artifacts

### Day-2 operating cadence

- weekly review of API error budgets and failed deployments
- monthly runtime and dependency patch review
- quarterly regional failover exercise
- quarterly capacity and throttle-limit review
- after every incident, update modules or service contract defaults if a control was missing

## 2.8 Least-Privilege IAM Strategy

### IAM principles

- one execution role per service or service boundary
- deployment role can modify only approved APIs, functions, tables, and records
- no shared admin role for all serverless services
- Route 53 updates restricted to named failover records
- KMS policies limited to relevant roles and service principals
- read-only diagnostics role for support teams

### Example IAM pseudocode

```hcl
role "lambda_billing_exec" {
  permissions = [
    "dynamodb:GetItem on billing table",
    "dynamodb:PutItem on idempotency table",
    "logs:CreateLogStream",
    "logs:PutLogEvents",
    "xray:PutTraceSegments",
    "kms:Decrypt on billing service key"
  ]
}
```

### Governance

- quarterly access review
- immediate review after security findings
- approval required for any new cross-service access
- exception-based permissions expire automatically unless renewed

## 2.9 Automated Drift Detection Strategy

### Drift targets

- nightly production drift plans per region
- route and security drift triaged within **4 business hours**
- non-production drift reviewed weekly
- execution-role drift or public endpoint drift treated as high severity
- stale drift item age target: **< 14 days** outside production

### Drift scope

- Route 53 failover policy
- API Gateway stages and domain settings
- Lambda environment and concurrency settings
- WAF associations and rule groups
- log retention and alarm presence
- DynamoDB PITR, billing mode, and replication config

### Remediation workflow

- production routing/security drift -> auto-open incident
- configuration drift introduced during emergency response -> reconcile by PR within 24 hours
- repeated service-specific drift -> create platform backlog item to tighten contract or module defaults
- expected operational variance documented with owner, reason, and expiry

## 2.10 CI/CD Workflow

- build once, promote artifact across environments
- static analysis, dependency scan, unit tests, and contract tests on PR
- Terraform plan plus policy checks for infra changes
- deployment alias canary with health-gated progression
- rollback to last known good alias if alarms fire
- apply permissions isolated by environment and region

### Release cadence

- platform module release every 2 weeks if changes exist
- emergency patch release path for security or reliability defects
- deprecated module or template announced with migration guidance and sunset date
- major version upgrades tested in sandbox and stage before production promotion

## 2.11 Security and Compliance Controls

- JWT/IAM/verified identity auth only on approved patterns
- WAF baseline mandatory for public services
- encrypted configuration and secrets
- request/response logging with redaction rules
- no unmanaged public origins
- audit evidence for API policy, release approvals, and failover tests

## 2.12 Risks, Trade-offs, and Failure Modes

### Trade-offs

- active/active reduces failover time but adds consistency and debugging complexity
- active/passive simplifies control but may underuse capacity
- stronger release gates reduce incident risk but add process overhead

### Common failure modes

- missing idempotency on retryable writes
- shared Lambda execution roles
- manual API Gateway stage edits
- untested Route 53 failover paths
- stale environment variables changed in console
- WAF detached under pressure and not restored

### Success metrics after 6 months

- **70%+** of new external APIs launched on the standard platform contract
- production release rollback time reduced to **< 10 minutes**
- no unresolved high-severity routing drift older than **24 hours**
- quarterly failover drill completion at **100%**
- public API security exception count trending down quarter-over-quarter
- platform-provided rollout defaults adopted by most critical services

---

# 3) Terraform-Based Internal Developer Platform Foundation with Shared VPC Networking, Self-Service Catalog Patterns, and Policy Enforcement

## 3.1 Business Context / Problem Statement

As engineering organizations scale, teams often slow themselves by repeatedly solving the same infrastructure problems: networking, IAM, observability, environment bootstrapping, service deployment scaffolding, and compliance controls. This creates long lead times, inconsistent reliability, and expensive review cycles.

The objective is to build an **internal developer platform foundation** that provides self-service capabilities with clear contracts, while enforcing production-grade defaults for security, reliability, governance, and auditability.

## 3.2 Platform Product Framing

This is not a collection of modules. It is a **product for internal engineering teams**.

### Platform consumers

- product teams launching services
- data teams provisioning approved data jobs and access paths
- SRE consuming telemetry, ownership metadata, and runbooks
- security consuming control evidence and exception records
- finance / engineering leadership consuming cost allocation and environment efficiency views

### Core promises of the platform

- provision a production-ready service foundation in hours, not weeks
- inherit safe defaults automatically
- use a narrow, stable self-service interface
- provide clear escalation paths and ownership boundaries
- prevent the need for bespoke infrastructure unless genuinely necessary

### Support model

- business-hours support for standard requests
- on-call support for sev-1/sev-2 platform incidents
- published SLA for provisioning and drift triage
- office hours and upgrade guidance for consuming teams
- documented exceptions process, not ad hoc Slack approvals

## 3.3 Target Architecture Overview

Core platform capabilities:

- shared VPC and network baseline with subnet segmentation
- service catalog for approved archetypes: web service, worker, scheduled job, batch job
- standard observability, secret integration, alert routing, and ownership metadata
- Terraform-based environment provisioning with policy checks
- tenancy boundaries by account, project, namespace, or workload identity
- cost tags, budgets, and quota controls by default
- release and lifecycle management for templates and modules

This platform can sit under EKS, ECS, Lambda, or mixed runtime strategies. The key is the **platform contract**, not the underlying runtime alone.

## 3.4 What Terraform Manages vs Runtime-Specific Delivery Systems

### Terraform manages

- org baseline, remote state, IAM boundaries
- networking, DNS, endpoints, service discovery
- standard service catalog resources
- observability sinks and alerting integration
- budgets, tags, quotas, and ownership metadata
- security controls and policy baselines

### Runtime-specific delivery systems manage

- deployment artifacts
- service rollout mechanics
- app-specific runtime config within approved scope
- health verification and rollback

### Boundary rule

Terraform defines the **foundation and approved interfaces**. Teams consume those interfaces. Direct mutation of shared infrastructure outside those interfaces is a platform policy violation.

## 3.5 Core Terraform Structure

### Layering

- `foundation`
- `network`
- `platform-services`
- `service-catalog`
- `team-environments`
- `governance-and-evidence`

### Suggested repository

```text
repo/
├── modules/
│   ├── remote-state/
│   ├── shared-vpc/
│   ├── team-boundary/
│   ├── service-web/
│   ├── service-worker/
│   ├── service-cron/
│   ├── observability-baseline/
│   ├── secret-integration/
│   ├── policy-baseline/
│   ├── budget-alarms/
│   └── evidence-export/
├── catalog/
│   ├── web-service/
│   ├── worker-service/
│   ├── scheduled-job/
│   └── batch-job/
├── live/
│   ├── foundation/
│   ├── prod/
│   ├── stage/
│   └── dev/
├── policies/
├── docs/
└── .github/workflows/
```

### Release and compatibility rules

- semantic versioning for all service catalog modules
- no breaking input changes without major release
- deprecated patterns remain supported for at least one quarter unless security risk requires faster removal
- migration guides required for every major release
- rollback instructions required for releases that alter defaults or interfaces

## 3.6 Self-Service Contract

### Example web-service input

```yaml
service_name: checkout
service_class: web
environment: prod
runtime: kubernetes
availability_tier: tier-1
exposure: internal
scaling_class: large
data_dependencies:
  - postgres:orders-readwrite
owner_team: commerce-platform
```

### Example outputs

- environment and namespace/project allocation
- IAM or workload identity binding
- dashboards, alerts, and runbook link placeholders
- standard ingress / routing integration
- budget tag set and owner metadata
- quota assignment and default autoscaling envelope
- audit labels and compliance evidence hooks

### What is intentionally not customizable

- removing owner tags
- disabling encryption
- bypassing policy baseline
- changing approved network segmentation directly
- skipping dashboards and alerts for production services
- opting out of backup requirements for stateful classes without exception approval

## 3.7 Reliability Design

### Reliability targets

- self-service environment provisioning lead time: **< 4 hours**
- paved-road service deployment lead time: **< 1 business day**
- platform availability target for provisioning workflows: **99.9%**
- change failure rate for platform-managed templates: **< 5%**
- MTTR for platform provisioning incidents: **< 60 minutes**
- service ownership metadata coverage: **100% for production services**

### Reliability controls

- resilient shared networking with segmented subnet tiers
- default observability and alert routing for every service class
- quota-aware provisioning to prevent hidden shared-platform exhaustion
- standard backup hooks for stateful services
- owner metadata and runbook requirements attached to catalog entries
- platform-wide dashboards for quota, subnet utilization, IAM exception load, and drift trends
- game days for shared networking, identity boundaries, and provisioning rollback

### Day-2 cadence

- weekly review of provisioning failures and support tickets
- monthly provider and module upgrade review
- quarterly DR or game day focused on shared platform dependencies
- quarterly service-catalog pruning and deprecated-pattern review
- monthly cost and quota review across teams and environments

## 3.8 Least-Privilege IAM Strategy

### IAM design

- team roles constrained by permission boundaries
- service templates create only approved identities
- no broad infra-admin rights for product teams by default
- product teams can deploy only into delegated environments
- platform team controls shared infrastructure mutation
- security can audit broadly but mutates only through controlled workflows

### Example permission boundary pseudocode

```hcl
permission_boundary "team_boundary" {
  deny = [
    "iam:* outside approved role paths",
    "kms:* outside team-approved keys",
    "ec2:* outside delegated resources",
    "route53:* outside team subdomains",
    "deletion of shared platform resources"
  ]
}
```

### Break-glass standard

- owner
- reason
- ticket / incident reference
- compensating controls
- automatic expiry
- post-use review

## 3.9 Automated Drift Detection Strategy

### Drift targets

- nightly drift checks for production and shared foundation
- weekly drift review for lower environments
- production foundation drift triaged within **4 business hours**
- unauthorized shared-platform drift remediated within **24 hours**
- drift older than **14 days** requires explicit risk acceptance
- target of **> 90%** drift items with clear owner and due date

### Drift scope

- foundation resources
- team environment baselines
- tag and metadata compliance
- observability and backup attachments
- network segmentation rules
- IAM boundary attachment and exception records

### Remediation ownership

- shared foundation drift -> platform owner
- policy drift -> security and platform joint review
- team-scoped drift within delegated area -> assigned to owning team with SLA
- repeated drift pattern -> platform backlog item to improve contract or automation

### Weekly drift review checklist

- open production drift
- exceptions expiring this week
- unmanaged resources discovered
- teams with repeated template bypass attempts
- modules causing recurring import/move issues
- stale states and decomission candidates

## 3.10 CI/CD Workflow

### Pipeline expectations

- format, validate, lint, static policy, and contract tests on every PR
- integration test for critical service templates
- preview plan for all changed environments
- approval required for shared foundation and production changes
- serialized apply per state
- post-apply conformance and evidence export
- release notes published for module and template consumers

### Promotion model

- new service template or module tested in sandbox
- promoted to dev, then stage, then prod
- production use of new major version gated behind explicit adoption plan
- rollback path documented before release approval

## 3.11 Governance and Exception Workflow

### Policy exception record

Every exception should include:

- exception owner
- linked team or service
- exact control being bypassed
- business reason
- compensating controls
- approval authority
- expiry date
- review checkpoint
- retirement plan

### Evidence expectations

- all production applies are traceable to PR, approver, pipeline run, and identity
- access review outcomes stored centrally
- drift and exception records exportable for audit
- service ownership and criticality tags mandatory

## 3.12 RACI-Style Ownership Model

| Capability | Platform | Product Team | Security | SRE |
|---|---|---|---|---|
| Terraform modules and service catalog | **R/A** | C | C | C |
| Service adoption and app config | C | **R/A** | C | C |
| IAM guardrails and boundaries | R | C | **A** | C |
| Shared network and environment baseline | **R/A** | C | C | C |
| Product service SLOs | C | **R/A** | C | **C** |
| Platform observability baseline | **R/A** | C | C | C |
| Incident response for platform outage | **R** | C | C | **A** |
| Drift remediation for shared foundation | **R/A** | C | C | C |
| Exception approval | C | C | **A** | C |

**R = Responsible, A = Accountable, C = Consulted**

## 3.13 Risks, Trade-offs, and Failure Modes

### Trade-offs

- more abstraction improves consistency but can hide important cloud mechanics
- stronger control reduces risk but can create platform friction if UX is weak
- shared services improve efficiency but increase the need for careful blast-radius design

### Failure signals

- new teams bypass the platform because provisioning is too slow
- exceptions increase every month without retirement
- module upgrades stall because compatibility guarantees are weak
- platform support becomes ticket-heavy instead of self-service-heavy
- drift keeps reappearing in the same areas
- service ownership metadata is incomplete or stale
- cost allocation is inconsistent across teams

### Success metrics after 6 months

- **75%+** of new services launched through the platform catalog
- provisioning lead time reduced from weeks to **< 4 hours** for standard environments
- production services with complete owner, cost, and criticality tags at **100%**
- drift items with owner and due date at **> 90%**
- policy exceptions reduced quarter-over-quarter
- platform-generated service baselines adopted by the majority of tier-1 and tier-2 services

---

# Shared Production Terraform Practices Across All Three Projects

## State Management

- remote state with encryption, versioning, logging, and locking
- state isolation by environment and layer
- state reader access tightly restricted
- imports and moved blocks reviewed as part of normal hygiene
- orphaned state entries removed through controlled decommission workflows

## Module Engineering

- narrow, opinionated interfaces
- semantic versioning
- examples and contract tests
- explicit upgrade notes
- deprecation policy with sunset windows
- no hidden side effects or implicit cross-stack coupling

## Policy and Compliance

- policy checks before apply
- enforcement on IAM, encryption, public exposure, logging, tags, and ownership metadata
- exceptions require owner, reason, compensating control, and expiry
- audit evidence exportable for compliance and risk review

## Testing

- `terraform fmt`, `validate`, lint, and security scan on every PR
- policy tests and contract tests for reusable modules
- integration tests for critical paths
- upgrade tests before provider/module promotions
- game days and DR tests for critical production patterns

## Observability

- logs, metrics, traces, and alerts wired by default
- dashboard standards per service class
- owner and environment metadata required
- error-budget and change-failure review included in operating cadence

## Delivery and Promotion

- build once, promote safely
- separate plan from apply
- required approvals for production
- short-lived federated CI identities
- immutable plan/apply relationship for production where practical
- post-apply verification and rollback criteria documented

---

# Operating Cadence for a Strong Product Company

## Weekly

- production drift review
- provisioning failure and incident review
- top platform support themes
- aging exceptions and manual-change patterns

## Monthly

- provider and module upgrade review
- platform release train for templates/modules
- cost, quota, and capacity review
- deprecated pattern adoption review

## Quarterly

- access review
- DR or game day for critical platform paths
- platform KPI review with engineering leadership
- service-catalog cleanup and retirement decisions

---

# What Good Looks Like After 6 Months

A strong product company should expect outcomes like:

- **70-80%+** of new workloads launched through paved-road platform contracts
- infrastructure change lead time reduced materially, with standard patterns delivered in hours or a single business day
- unauthorized production drift detected nightly and remediated within SLA
- clear reduction in manual console changes across production accounts
- improved production reliability through better rollback speed, ownership metadata, and observability consistency
- platform support load shifting from manual tickets to self-service consumption
- access reviews, exception records, and apply evidence consistently available for audit
- platform module releases shipping on a predictable cadence with documented compatibility and rollback paths

---

# Signals That the Platform Is Failing

These are measurable warning signs leadership should treat seriously:

- adoption of paved-road templates remains below **50%**
- exceptions rise faster than service count
- production drift is detected repeatedly in the same areas
- module upgrades are routinely delayed because consumers do not trust compatibility
- teams rely on direct cloud access for normal delivery work
- ownership metadata is missing for production services
- change failure rate is flat or worsening despite increased platform investment
- platform backlog is dominated by bespoke one-off provisioning rather than reusable product improvements
- platform incidents repeat without resulting module, policy, or template changes

---

# Closing Assessment

These three projects represent high-quality senior platform engineering work for a strong product company because they do more than provision cloud resources. They establish:

- a **reliable platform operating model**
- clear **team and system boundaries**
- **least-privilege IAM** at both human and workload layers
- **automated drift detection** with ownership and SLAs
- a realistic **Terraform governance model**
- safe **promotion, release, and rollback practices**
- measurable **business and engineering outcomes**
- strong **day-2 lifecycle management**

This is the difference between infrastructure automation and platform engineering leadership.
