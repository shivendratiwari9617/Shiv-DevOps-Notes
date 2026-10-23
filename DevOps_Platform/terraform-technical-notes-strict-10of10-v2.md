# Terraform Technical Notes

## 1. Core Mental Model

Terraform is a declarative infrastructure-as-code tool used to define, plan, and manage infrastructure through configuration files. Its real power is not only resource creation, but the controlled reconciliation of desired state against tracked state and actual remote infrastructure.

A strong working model is:

- **configuration** = desired infrastructure definition
- **state** = Terraform’s recorded binding between configuration and real infrastructure
- **plan** = proposed actions required to reconcile desired and current state
- **apply** = execution of that plan against real systems

Terraform is not just a provisioning script runner. It is a **stateful graph-driven reconciliation engine**, and many advanced edge cases come from misunderstanding state, graph dependencies, provider behavior, concurrency, and workflow security.

---

## 2. Terraform Workflow Model

Terraform typically moves through a repeated lifecycle:

1. write configuration
2. initialize providers and modules
3. inspect backend and state context
4. create a plan
5. review the plan carefully
6. apply approved changes
7. monitor for drift and operational fallout

```sh
terraform init
terraform fmt -check
terraform validate
terraform plan -out=tfplan
terraform apply tfplan
```

- `terraform init` initializes the backend, providers, and modules.
- `terraform fmt -check` verifies formatting without changing files.
- `terraform validate` checks configuration syntax and internal consistency.
- `terraform plan -out=tfplan` saves a reviewed execution plan for deterministic apply.
- `terraform apply tfplan` applies exactly the reviewed plan artifact.

### 2.1 Declarative, But Not Magic

Terraform declares what infrastructure should look like, but providers still map those declarations to APIs with their own constraints, defaults, eventual consistency delays, idempotency quirks, and replacement rules.

### 2.2 Graph-Based Execution

Terraform builds a dependency graph of resources and evaluates what can be created, changed, or destroyed in parallel. Most subtle behavior around ordering comes from explicit references, implicit dependencies, lifecycle rules, and provider-level readiness gaps.

### 2.3 Plan Is a Safety Boundary

A Terraform plan is not just an informational preview. In mature environments it is a change-control artifact that should be reviewed as carefully as code.

---

## 3. Configuration Structure and Evaluation Nuance

Terraform configurations are composed from blocks such as providers, resources, data sources, variables, locals, outputs, and modules.

### 3.1 Resource Blocks

Resources declare managed infrastructure objects.

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "example-logs-bucket"
}
```

### 3.2 Data Sources

Data sources read existing information from providers without managing that object’s lifecycle.

```hcl
data "aws_caller_identity" "current" {}

data "aws_ami" "base" {
  most_recent = true

  owners = ["self"]

  filter {
    name   = "name"
    values = ["app-base-*"]
  }
}
```

#### edge case

Data sources can make plans sensitive to changing external conditions. A data source that changes unexpectedly can alter downstream plans even when your configuration files did not change.

#### timing caveat

A data source may resolve successfully during one plan and fail later because of permissions, regional differences, eventual consistency, or upstream object lifecycle changes.

### 3.3 Variables, Locals, and Outputs

Variables parameterize configuration. Locals centralize derived values. Outputs expose useful values after apply and across module boundaries.

```hcl
variable "environment" {
  type = string
}

locals {
  name_prefix = "payments-${var.environment}"
}

output "bucket_name" {
  value     = aws_s3_bucket.logs.id
  sensitive = false
}
```

### 3.4 Unknown Values at Plan Time

Some values are unknown until apply because they depend on provider-generated identifiers or results of resources not yet created.

```hcl
resource "aws_security_group" "app" {
  name = "${local.name_prefix}-sg"
}

resource "aws_instance" "app" {
  ami           = data.aws_ami.base.id
  instance_type = "t3.micro"

  vpc_security_group_ids = [aws_security_group.app.id]
}
```

#### edge case

Expressions depending on unknown values may force more conservative planning behavior, especially in `for_each`, dynamic blocks, conditionals, and chained module outputs.

### 3.5 `count` vs `for_each`

`count` is index-based and can cause destructive address shifts when element ordering changes. `for_each` is key-based and is usually safer for long-lived infrastructure collections.

```hcl
# riskier for identity stability
resource "aws_iam_user" "count_users" {
  count = length(var.usernames)
  name  = var.usernames[count.index]
}

# safer for stable identity
resource "aws_iam_user" "each_users" {
  for_each = toset(var.usernames)
  name     = each.key
}
```

#### production caveat

Using `count` with lists that later reorder can force unwanted destroys and recreations. Stable keys matter for infrastructure identity.

---

## 4. Providers, Versions, and Plugin Behavior

Providers are the translation layer between Terraform configuration and external APIs.

### 4.1 Provider Pinning

Provider versions should be constrained intentionally.

```hcl
terraform {
  required_version = ">= 1.6.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

#### caveat

Provider upgrades can change defaults, schema behavior, diff rendering, validation logic, and replacement behavior. A small provider bump can materially change plans.

### 4.2 Provider Aliases

Aliases allow multiple configurations of the same provider, such as multi-region or multi-account deployments.

```hcl
provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "dr"
  region = "us-west-2"
}

resource "aws_s3_bucket" "primary" {
  bucket = "example-primary-bucket"
}

resource "aws_s3_bucket" "replica" {
  provider = aws.dr
  bucket   = "example-replica-bucket"
}
```

#### advanced caveat

Provider alias handling across modules can become subtle. If a child module expects a provider configuration and the caller passes the wrong alias or forgets to pass one, Terraform may plan against the wrong account or region.

```hcl
module "network_dr" {
  source = "./modules/network"

  providers = {
    aws = aws.dr
  }
}
```

### 4.3 Eventual Consistency and Provider Retries

Cloud APIs are not always immediately consistent. Terraform providers may retry, but failures can still occur when dependent resources are evaluated before the platform is truly ready.

#### edge case

A successful apply can still leave downstream systems operationally incomplete if the API reports success before the service is fully usable.

---

## 5. State: The Center of Terraform Behavior

State is Terraform’s mapping between configuration and real infrastructure objects. Most advanced Terraform safety concerns ultimately relate to how state is stored, updated, protected, and interpreted.

### 5.1 Why State Exists

Terraform needs state to know:

- which real object corresponds to each resource address
- what attributes are known from the last apply
- what must be updated, replaced, or destroyed
- how dependencies connect across managed objects

### 5.2 Local State

Local state is stored in a local file, commonly `terraform.tfstate`.

#### caveat

Local state is fine for individual experimentation but weak for team collaboration. It is easy to lose, overwrite, expose, or diverge.

### 5.3 Sensitive Data in State

State can contain sensitive values, including generated credentials, connection details, resource metadata, and provider-returned secrets.

```hcl
resource "random_password" "db" {
  length  = 24
  special = true
}

output "db_password" {
  value     = random_password.db.result
  sensitive = true
}
```

#### security caveat

Marking an output as sensitive affects display behavior, not whether the value exists in state. State security must be designed intentionally.

### 5.4 State Corruption Risk

Interrupted runs, manual edits, partial applies, and backend misconfiguration can produce confusing or dangerous state conditions.

#### operational note

Terraform state is not something to clean up casually. Manual edits and forceful fixes should be rare, deliberate, reviewed, and backed up.

### 5.5 State Snapshot Mismatch

State is not the same thing as live infrastructure. It is Terraform’s last trusted record plus refresh behavior, and that means stale assumptions are possible until a new plan or refresh reconciles them.

---

## 6. Remote State Backends

Remote state is the normal approach for team Terraform use. It centralizes state, improves collaboration, and usually enables locking and stronger access control.

### 6.1 Why Remote State Matters

Remote backends provide:

- shared source of truth
- better team coordination
- state durability
- access control
- auditability in some environments
- locking support in many workflows

### 6.2 Backend Configuration Example

```hcl
terraform {
  backend "s3" {
    bucket         = "org-terraform-state"
    key            = "payments/prod/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "org-terraform-locks"
    encrypt        = true
  }
}
```

This pattern centralizes state in object storage and uses a locking service to prevent concurrent modification.

### 6.3 Backend Initialization Nuance

Backends are initialized during `terraform init`. Changing backend configuration often requires migration or reinitialization steps.

```sh
terraform init -reconfigure
terraform init -migrate-state
```

- `terraform init -reconfigure` forces Terraform to forget previous backend initialization assumptions.
- `terraform init -migrate-state` migrates current state into the newly configured backend.

#### edge case

A backend change can appear harmless in code review but actually relocate the authoritative state store. State migration should always be explicit and verified.

### 6.4 Backend IAM Separation

A strong remote state design often separates permissions for:

- state readers
- state writers
- lock management
- backend administrators
- break-glass recovery operators

#### security value

This limits blast radius if a CI job, automation token, or human credential is compromised.

### 6.5 State Versioning and Recovery

Backends with version history are significantly safer operationally because they provide a recovery path from accidental overwrites or damaging runs.

#### caveat

Recovery still requires caution. Rolling back state incorrectly can create a mismatch between state and real infrastructure.

### 6.6 Remote State Data Source Caveat

Using one Terraform stack to read outputs from another through remote state can be useful, but it creates coupling.

```hcl
data "terraform_remote_state" "network" {
  backend = "s3"

  config = {
    bucket = "org-terraform-state"
    key    = "network/prod/terraform.tfstate"
    region = "us-east-1"
  }
}

resource "aws_security_group" "app" {
  name   = "payments-prod-app"
  vpc_id = data.terraform_remote_state.network.outputs.vpc_id
}
```

#### advanced caveat

Cross-state output consumption can create brittle dependency chains, unclear ownership boundaries, and hidden change propagation between foundational and application stacks.

---

## 7. State Locking and Concurrency

State locking prevents multiple Terraform operations from modifying the same state at the same time. This is one of the most important protections in collaborative workflows.

### 7.1 Why Locking Exists

Without locking, concurrent applies can race, overwrite each other, or produce inconsistent state transitions.

### 7.2 Locking Behavior

Many remote backends or managed platforms support state locking. Terraform acquires the lock before operations that may write state.

```sh
terraform plan
terraform apply
terraform force-unlock <LOCK_ID>
```

- `terraform plan` may acquire backend coordination depending on backend behavior and refresh interactions.
- `terraform apply` acquires the lock before making state-changing operations.
- `terraform force-unlock <LOCK_ID>` removes a lock only after you confirm no valid operation still owns it.

### 7.3 Lock Contention

Lock contention usually means another operation is already in progress or a previous run left a stale lock behind.

#### operational caveat

A stale lock is annoying, but force-unlocking blindly is risky. You should first confirm that no legitimate apply is still running.

### 7.4 `force-unlock` Risk

`terraform force-unlock` should be treated as an emergency coordination action, not routine hygiene.

#### edge case

If one operator force-unlocks while another apply is still active, both processes may believe they can safely proceed. That can create state corruption or conflicting infrastructure changes.

### 7.5 Locking Is Not Full Isolation

State locking reduces concurrency at the state-file level, not at the organization level. If too many unrelated systems share one state, team throughput suffers and blast radius grows.

### 7.6 Long-Running Applies

Long applies increase the chance of contention, expired credentials, partially updated infrastructure, and retry ambiguity.

#### workflow implication

If applies are routinely slow, the design problem may be state size, graph complexity, provider/API bottlenecks, or environment coupling.

---

## 8. State Topology and Workspace Strategy

One of the most important design decisions is how you partition state.

### 8.1 Small, Focused State Is Safer

A single giant state file increases:

- lock contention
- plan noise
- review difficulty
- accidental destroy blast radius
- dependency entanglement

### 8.2 Split by Lifecycle or Ownership

A good state boundary often follows one or more of:

- team ownership
- environment
- system boundary
- deployment lifecycle
- blast radius tolerance

### 8.3 Workspaces

Workspaces provide multiple state instances for one configuration.

```sh
terraform workspace list
terraform workspace new dev
terraform workspace select prod
```

- `terraform workspace list` shows available workspace-scoped state instances.
- `terraform workspace new dev` creates a new isolated workspace state.
- `terraform workspace select prod` switches the CLI context to another workspace.

#### caveat

Workspaces are often misunderstood as a complete environment-separation strategy. They can help, but they do not replace strong account or project isolation, backend separation, secret isolation, or environment-specific review discipline.

### 8.4 Workspace Edge Case

Using one configuration with many conditionals and workspaces can create brittle logic, hidden differences, and risky assumptions about parity between environments.

### 8.5 Directory vs Workspace Strategy

Separate directories or repositories per environment can improve review clarity and boundary control, while workspaces can reduce duplication.

#### tradeoff

The right choice depends on how strongly you need isolation, how much configuration divergence exists, and how much organizational risk comes from accidental cross-environment actions.

---

## 9. Drift Detection

Drift is the difference between Terraform’s desired configuration and the actual remote infrastructure state.

### 9.1 How Drift Happens

Drift commonly appears when:

- resources are changed manually in the cloud console
- other automation modifies the same objects
- provider defaults change over time
- external systems mutate managed resources
- partial failures leave infrastructure in unexpected shape

### 9.2 Drift Categories

Not all drift is equally dangerous.

#### configuration drift

The live resource differs from the declared Terraform configuration.

#### state drift

Terraform’s state no longer accurately reflects the real remote object.

#### provider drift

A provider upgrade, API schema change, or default-behavior shift changes what Terraform now perceives.

#### imported mismatch drift

An imported object exists, but the current configuration does not fully match how it was really built.

### 9.3 Why Drift Matters

Drift weakens trust in plans, increases surprise during apply, and can cause Terraform to attempt corrections at dangerous times.

### 9.4 Detecting Drift

Common drift detection methods include:

- running `terraform plan` regularly
- using refresh-only plans for state reconciliation visibility
- comparing plans in CI
- platform-native audit logging and configuration monitoring

```sh
terraform plan
terraform plan -refresh-only
terraform apply -refresh-only
```

- `terraform plan` compares desired state with tracked and refreshed infrastructure reality.
- `terraform plan -refresh-only` shows state reconciliation differences without config-driven mutation intent.
- `terraform apply -refresh-only` writes refreshed infrastructure reality into state.

### 9.5 `-refresh-only`

A refresh-only plan or apply updates state to reflect real infrastructure without applying configuration-driven changes.

#### caveat

Refresh-only is not the same as fixing drift. It aligns Terraform’s recorded understanding with reality, which may or may not be what you want.

### 9.6 Benign vs Dangerous Drift

Some drift is noisy and expected, such as externally updated timestamps or operational metadata. Other drift is security-critical or availability-critical, such as network exposure, IAM changes, or resource deletion.

### 9.7 Drift Detection in CI

Regular `terraform plan` in CI can surface unexpected changes, but it must be designed carefully.

#### edge case

A plan job with overly broad permissions can become a security liability. A plan job with insufficient permissions can produce misleading or incomplete results.

---

## 10. Plan Interpretation and Safe Review

A Terraform plan is not just something changed. It is the most important safety checkpoint in the workflow.

### 10.1 Read Plans Semantically

Reviewers should ask:

- what is being created, changed, replaced, or destroyed
- why is this happening now
- is the diff expected from the code change
- are any resources being replaced unexpectedly
- are any provider-computed attributes causing noise

### 10.2 Replacement Triggers

Some attribute changes force replacement rather than in-place update.

#### caveat

A seemingly minor field change can destroy and recreate a critical resource if the provider marks it as replacement-triggering behavior.

### 10.3 Saved Plans

Saving a plan artifact can improve review and apply consistency.

```sh
terraform plan -out=tfplan
terraform show tfplan
terraform apply tfplan
```

- `terraform plan -out=tfplan` stores a deterministic plan for later approval and execution.
- `terraform show tfplan` lets reviewers inspect the saved plan content.
- `terraform apply tfplan` executes the exact reviewed plan rather than recalculating it.

#### security note

Saved plan files may contain sensitive information and should be handled as sensitive artifacts.

### 10.4 Plan/Apply Consistency

Using a reviewed saved plan reduces the risk that new changes or environment differences alter the apply outcome between approval and execution.

### 10.5 Targeted Operations Warning

`-target` can be useful for emergency recovery or careful graph slicing, but it should not become a normal workflow.

```sh
terraform plan -target=aws_instance.example
terraform apply -target=aws_instance.example
```

- `terraform plan -target=...` narrows planning to part of the graph.
- `terraform apply -target=...` applies only a selected subset of the dependency graph.

#### serious caveat

Routine targeted plans or applies can bypass the full dependency picture and leave infrastructure in a misleading partially reconciled state.

---

## 11. Secure Terraform Workflows

Terraform automation touches cloud credentials, secrets, state, and destructive operations. Secure workflows require more than valid syntax.

### 11.1 Principle of Least Privilege

Terraform runners, CI jobs, and human operators should have only the permissions required for the intended scope.

#### caveat

Overly broad credentials make automation convenient but dangerous. A bug in code or pipeline logic can become an organization-wide incident.

### 11.2 Secret Handling

Do not hardcode secrets in configuration, variable files, shell history, or source control.

Better patterns include:

- secret managers
- short-lived credentials
- workload identity or federated identity
- environment injection with strict controls

### 11.3 `.tfvars` and Secret Leakage

Variable files are convenient but easy to mishandle.

```gitignore
*.tfvars
*.tfvars.json
.terraform/
terraform.tfstate*
```

#### security caveat

A `.tfvars` file in source control or in an unsecured CI artifact path is a common secret exposure path.

### 11.4 State as Sensitive Material

Even if configuration is clean, state can still expose sensitive values. Access to state should be stricter than ordinary source code access in many environments.

### 11.5 Credential Scope Separation

Plan jobs, apply jobs, and break-glass admin actions may need different credential profiles. Separating those reduces accidental misuse and improves auditability.

### 11.6 Human vs Automation Identity Boundary

Humans should not normally share the same credentials or permission scope as CI/CD automation.

#### value

This supports better audit trails, more predictable workflows, and safer emergency escalation.

### 11.7 Identity Federation Patterns

Federated identity or workload identity reduces reliance on long-lived static cloud keys in CI.

#### security value

This lowers credential leakage risk and makes key rotation less operationally painful.

### 11.8 Secure CI Flow Pattern

A strong workflow usually separates:

- code authoring
- plan generation
- plan review
- approval
- apply execution

```yaml
# conceptual pipeline shape
stages:
  - validate
  - plan
  - review
  - apply
```

This reduces the chance of unreviewed direct applies and improves traceability.

---

## 12. Modules and Reuse Edge Cases

Modules are the main Terraform reuse mechanism, but reuse without discipline can increase complexity.

### 12.1 Good Module Use

Modules are useful when they encapsulate a stable pattern with a clear interface.

### 12.2 Bad Module Smells

Common module problems include:

- too many inputs
- too many conditional branches
- provider assumptions hidden inside
- exposing low-level details without abstraction value
- mixing unrelated infrastructure domains

### 12.3 Version Pinning for Modules

Remote modules should be versioned intentionally. Floating references make reproducibility and rollback much harder.

```hcl
module "vpc" {
  source  = "git::https://example.com/terraform-modules.git//aws/vpc?ref=v2.4.1"
  cidr    = "10.10.0.0/16"
  env     = "prod"
}
```

### 12.4 Module Upgrade Risk

A module upgrade can introduce behavior changes even if your own root configuration barely changed.

#### edge case

When a module changes defaults, lifecycle rules, resource naming, or internal addressing, the resulting plan can include replacements that surprise reviewers.

### 12.5 Cross-Module Dependency Confusion

Modules should usually depend on explicit outputs rather than hidden assumptions about resource names or provider-side discovery.

---

## 13. Dependency Management and Graph Edge Cases

Terraform usually infers dependencies from references, but sometimes that is not enough.

### 13.1 Implicit Dependencies

Terraform creates dependencies when one resource references attributes from another.

### 13.2 `depends_on`

`depends_on` can add explicit dependency edges when ordering matters but value references do not exist directly.

```hcl
resource "aws_iam_role_policy_attachment" "app" {
  role       = aws_iam_role.app.name
  policy_arn = aws_iam_policy.app.arn
}

resource "aws_instance" "app" {
  ami           = data.aws_ami.base.id
  instance_type = "t3.micro"

  depends_on = [aws_iam_role_policy_attachment.app]
}
```

#### caveat

`depends_on` should be used carefully. Overusing it can hide design problems and unnecessarily serialize graph execution.

### 13.3 Hidden Operational Dependencies

Some resources depend on external readiness that Terraform cannot infer from configuration references alone.

#### edge case

A resource may technically exist from the provider’s perspective but not be operationally ready for downstream use. This is common with IAM propagation, DNS readiness, or eventually consistent control planes.

### 13.4 `terraform graph` Is Descriptive, Not Perfect

Graph visualization can help reason about dependency structure, but it will not fully explain provider runtime behavior, hidden readiness gaps, or external system side effects.

```sh
terraform graph
```

- `terraform graph` emits a dependency graph that helps inspect resource relationships.

---

## 14. Lifecycle Meta-Arguments and Change Control

Lifecycle settings can help manage replacement behavior, but they must be used intentionally.

### 14.1 `create_before_destroy`

This attempts to create the replacement before destroying the existing resource.

```hcl
resource "aws_launch_template" "app" {
  name_prefix = "payments-app-"

  lifecycle {
    create_before_destroy = true
  }
}
```

#### caveat

It can reduce downtime, but it may fail if the platform forbids duplicate names, quotas are exhausted, or dependent resources cannot tolerate overlap.

### 14.2 `prevent_destroy`

This blocks resource destruction through Terraform unless explicitly overridden.

```hcl
resource "aws_db_instance" "primary" {
  identifier = "payments-prod-db"

  lifecycle {
    prevent_destroy = true
  }
}
```

#### value

It is useful for especially critical resources such as foundational state storage, core networking, or irreplaceable data systems.

### 14.3 `ignore_changes`

This tells Terraform to ignore specific attribute changes when comparing configuration and state.

```hcl
resource "aws_instance" "app" {
  ami           = data.aws_ami.base.id
  instance_type = "t3.micro"

  lifecycle {
    ignore_changes = [tags["LastPatchedAt"]]
  }
}
```

#### serious caveat

`ignore_changes` can silence useful drift signals. It should be used narrowly and documented clearly, not as a blanket fix for noisy plans.

### 14.4 `replace_triggered_by`

This can force replacement when related objects change, which is useful in selected dependency patterns.

```hcl
resource "terraform_data" "schema_version" {
  input = var.schema_version
}

resource "aws_instance" "app" {
  ami           = data.aws_ami.base.id
  instance_type = "t3.micro"

  lifecycle {
    replace_triggered_by = [terraform_data.schema_version]
  }
}
```

### 14.5 Taint and Forced Replacement Thinking

Forced replacement is sometimes necessary when a resource is unhealthy even if configuration has not changed.

```sh
terraform taint aws_instance.app
terraform untaint aws_instance.app
```

- `terraform taint aws_instance.app` marks a resource for forced replacement on the next apply.
- `terraform untaint aws_instance.app` removes that forced-replacement mark.

#### caveat

Any forced replacement decision should be reviewed like a destroy-and-recreate action, because operational impact may be larger than the command syntax suggests.

---

## 15. Import, Move, and Refactor Safety

Real Terraform code evolves. Refactors are often more dangerous than initial creation.

### 15.1 Importing Existing Resources

Import connects an existing remote object to a Terraform resource address.

```sh
terraform import aws_s3_bucket.logs example-logs-bucket
```

- `terraform import ...` binds an existing remote resource into Terraform state.

#### caveat

Import does not automatically generate correct configuration. If configuration does not match the imported resource well enough, the next plan may propose unexpected changes.

### 15.2 Address Changes and Moves

Renaming resources, changing module paths, or switching from `count` to `for_each` changes resource addresses.

#### edge case

A logical refactor can look to Terraform like old resources should be destroyed and brand-new ones created unless the move is handled correctly.

### 15.3 Moved Blocks

Moved blocks help Terraform understand address changes during refactors.

```hcl
moved {
  from = aws_instance.app
  to   = aws_instance.web
}
```

#### value

They reduce accidental destroys during rename or restructuring operations.

### 15.4 State Moves

Manual state moves are sometimes necessary in advanced refactors.

```sh
terraform state list
terraform state mv aws_instance.app module.web.aws_instance.app
terraform state rm aws_instance.legacy
```

- `terraform state list` lists resource addresses tracked in state.
- `terraform state mv ...` relocates a state binding during refactors.
- `terraform state rm ...` removes a resource from state without destroying the remote object.

#### operational caveat

Direct state manipulation is powerful and risky. It should be reviewed, backed up, and usually tested outside production first.

### 15.5 Destroy Risk During Refactors

Refactors should be reviewed specifically for accidental destroy intent.

#### practical rule

Any structural change involving resource addresses, module paths, collection keys, or provider aliases deserves extra plan scrutiny.

---

## 16. Partial Failure and Recovery

Terraform applies can fail halfway through.

### 16.1 Partial Apply Reality

Terraform may successfully create or modify some resources before another resource fails. The infrastructure and state may be partially updated.

### 16.2 Recovery Approach

After failure, do not assume rerunning blindly is safe. First inspect:

- current infrastructure reality
- state contents
- provider or API error cause
- whether side effects happened outside Terraform awareness

### 16.3 Refresh Before Fixing

A refresh-only plan can help clarify what Terraform currently believes versus what actually exists.

### 16.4 Provider/API Retry Ambiguity

Sometimes a provider call times out or returns uncertainty even though the remote platform eventually completed the operation. This creates some of the hardest reconciliation problems.

### 16.5 Manual Repair Before Reconciliation

In some incidents, the safest path is to manually stabilize the platform first, then reconcile Terraform state and configuration deliberately.

#### caution

Manual repair should be exceptional and documented, because it introduces drift that must later be understood and resolved.

---

## 17. Policy, Validation, and Guardrails

Infrastructure workflows benefit from multiple safety layers before apply.

### 17.1 Formatting and Validation

Basic hygiene checks should run everywhere.

```sh
terraform fmt
terraform fmt -check
terraform validate
```

- `terraform fmt` rewrites files into canonical Terraform style.
- `terraform fmt -check` verifies formatting without modifying files.
- `terraform validate` checks the configuration for internal validity.

### 17.2 Static Analysis and Security Scanning

Terraform code can be analyzed for risky patterns such as:

- public exposure
- overly broad IAM permissions
- missing encryption
- weak network segmentation
- unsafe lifecycle settings

### 17.3 Policy as Code

Organizations often enforce rules around what may be provisioned, how it must be tagged, and which patterns are forbidden.

### 17.4 Review Guardrails

Human review should check both code and plan. Reviewing only HCL without the plan misses provider behavior; reviewing only the plan misses structural code risk.

---

## 18. CI/CD and Team Collaboration Patterns

Terraform collaboration workflows matter as much as configuration correctness.

### 18.1 Recommended Team Flow

A strong workflow usually looks like:

1. change configuration in branch
2. run format and validation checks
3. generate plan in CI
4. review code and plan together
5. approve explicitly
6. apply from trusted automation
7. monitor outputs and cloud-side effects

### 18.2 Avoid Direct Human Apply to Shared Environments

Direct local apply against shared production environments weakens auditability and increases inconsistency risk.

### 18.3 Separate Plan and Apply Permissions

Plan and apply often deserve different execution controls.

#### edge case

If anyone who can open a pull request can also trigger production apply with broad credentials, the workflow is not meaningfully protected.

### 18.4 One State, One Pipeline Ownership Pattern

A state boundary should usually map cleanly to one responsible pipeline and one ownership domain. Shared unclear ownership creates dangerous coordination gaps.

### 18.5 Promotion Between Environments

Promoting changes from dev to stage to prod should be explicit and reviewable.

#### caveat

Implicitly assuming that the same code is the same risk everywhere ignores data gravity, environment drift, quotas, and privilege differences.

---

## 19. Advanced Commands and One-Line Explanations

```sh
terraform init
```
Initializes the working directory, downloads providers/modules, and configures the backend.

```sh
terraform init -reconfigure
```
Reinitializes the backend and provider setup without assuming previous initialization settings are still correct.

```sh
terraform init -migrate-state
```
Reinitializes Terraform while migrating existing state to a newly configured backend.

```sh
terraform fmt
```
Rewrites Terraform files into standard canonical formatting.

```sh
terraform fmt -check
```
Checks whether Terraform files are properly formatted without changing them.

```sh
terraform validate
```
Validates Terraform configuration syntax and internal consistency.

```sh
terraform plan
```
Builds an execution plan showing what Terraform would change without applying it.

```sh
terraform plan -out=tfplan
```
Creates and saves a specific reviewed plan artifact for later controlled apply.

```sh
terraform plan -refresh-only
```
Shows how state would be updated to match current remote reality without proposing config-driven changes.

```sh
terraform plan -target=aws_instance.example
```
Plans changes for a narrowed part of the graph, which is useful in emergencies but risky as routine practice.

```sh
terraform apply
```
Executes the planned infrastructure changes against real systems.

```sh
terraform apply tfplan
```
Applies a previously saved plan so execution matches the reviewed artifact.

```sh
terraform apply -refresh-only
```
Writes refreshed remote reality into state without reconciling toward configuration changes.

```sh
terraform destroy
```
Destroys all resources tracked by the current configuration and state.

```sh
terraform show
```
Displays human-readable details about the current state or a saved plan file.

```sh
terraform show -json
```
Outputs state or plan data in JSON form for programmatic inspection and tooling.

```sh
terraform output
```
Displays output values from the current state.

```sh
terraform output -json
```
Prints outputs in JSON format for automation or downstream systems.

```sh
terraform workspace list
```
Lists available workspaces for the current configuration and backend.

```sh
terraform workspace new dev
```
Creates a new workspace with its own state instance.

```sh
terraform workspace select prod
```
Switches the current working context to a different workspace.

```sh
terraform import aws_s3_bucket.logs example-logs-bucket
```
Binds an existing remote object to a Terraform resource address in state.

```sh
terraform state list
```
Lists all resource addresses currently tracked in the state.

```sh
terraform state show aws_s3_bucket.logs
```
Displays the state-recorded attributes for a specific tracked resource.

```sh
terraform state mv old.addr new.addr
```
Moves a resource binding in state from one address to another during refactors.

```sh
terraform state rm addr
```
Removes a resource binding from state without destroying the real remote object.

```sh
terraform force-unlock <LOCK_ID>
```
Manually removes a state lock when you have confirmed no legitimate operation still holds it.

```sh
terraform providers
```
Shows which providers the current configuration and state require.

```sh
terraform graph
```
Generates a dependency graph representation of the Terraform configuration.

```sh
terraform console
```
Opens an interactive console for evaluating Terraform expressions and inspecting values.

```sh
terraform taint aws_instance.example
```
Marks a resource as needing replacement on the next apply.

```sh
terraform untaint aws_instance.example
```
Removes a taint mark so the resource is no longer forced to be replaced.

```sh
terraform version
```
Displays the installed Terraform version and provider version summary context.

---

## 20. Common Anti-Patterns

### 20.1 One giant state for everything

This increases blast radius, lock contention, and review complexity.

### 20.2 Treating workspaces as full environment isolation

Workspaces are useful, but they do not replace strong account or project separation and access control.

### 20.3 Ignoring plan output

Applying without careful plan review defeats one of Terraform’s main safety mechanisms.

### 20.4 Hardcoding secrets

This creates avoidable long-lived secret exposure risk.

### 20.5 Overusing `ignore_changes`

This can hide real drift and weaken trust in Terraform as the source of truth.

### 20.6 Manual cloud console edits on managed resources

This creates drift and surprise reconciliation behavior.

### 20.7 Running concurrent applies against the same state

This is an avoidable path to state corruption or conflicting infrastructure changes.

### 20.8 Using `count` where stable keys are required

List reordering can cause destructive identity churn.

### 20.9 Blindly force-unlocking

This can turn a stale-lock nuisance into a real concurrency incident.

### 20.10 Relying on targeted apply as normal workflow

This creates partial graph reconciliation and often hides broader dependency effects.

### 20.11 Sharing broad production credentials across humans and CI

This weakens accountability and expands blast radius during mistakes or compromise.

---

## 21. Final Framing

Terraform is safest and most effective when treated as a controlled reconciliation system rather than a simple provisioning convenience tool. Most serious incidents come from a small set of misunderstandings:

- weak state protection
- poor state boundaries
- blind applies
- unsafe concurrency
- hidden drift
- overly broad credentials
- manual mutations outside workflow
- insecure secret handling
- sloppy refactors
- misuse of targeted operations

Strong Terraform practice therefore emphasizes:

- secure remote state
- reliable locking
- disciplined drift detection
- careful plan review
- least-privilege automation
- explicit state boundaries
- cautious lifecycle settings
- safe refactor and recovery workflows
- audited, review-driven infrastructure delivery
