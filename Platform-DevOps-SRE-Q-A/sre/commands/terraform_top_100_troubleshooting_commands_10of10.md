# Terraform Top 100 Production Troubleshooting Commands
**10/10 senior-level runbook pack for Terraform Platform Engineers, DevOps Engineers, and SREs in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic Terraform cheat sheet.

Each command includes:

- the command itself
- a concise technical explanation
- a **risk level**
- a **use when** line so responders know where it fits in incident flow

## Command Risk Levels

- **Low risk** — read-only or generally safe diagnostic action
- **Medium risk** — changes execution behavior or context and should be used deliberately
- **High risk** — can alter real infrastructure or state safety significantly
- **Emergency-only** — use only in tightly controlled recovery situations with strong confirmation

## Fast “First 5 Commands” by Incident Type

### State lock incident
1. `terraform workspace show`
2. `terraform init`
3. `terraform plan -lock-timeout=10m`
4. `terraform state pull > pulled_state.json`
5. `env | grep ^TF_`

### Unexpected destructive plan
1. `terraform workspace show`
2. `terraform init -reconfigure`
3. `terraform plan -var-file=prod.tfvars`
4. `terraform show -json tfplan`
5. `terraform providers`

### Drift or out-of-band infrastructure change
1. `terraform workspace show`
2. `terraform init`
3. `terraform plan -refresh-only`
4. `terraform state list`
5. `terraform state show <resource>`

### Broken module refactor
1. `terraform workspace show`
2. `terraform state list | grep '^module\.'`
3. `terraform graph -type=plan`
4. `terraform console <<< 'module.network.vpc_id'`
5. `terraform state mv <src> <dst>`

### Backend or remote state problem
1. `terraform workspace show`
2. `terraform init -reconfigure`
3. `terraform init -backend=false`
4. `terraform state pull`
5. `terraform plan -lock-timeout=10m`

### Provider or plugin issue
1. `terraform version`
2. `terraform providers`
3. `terraform init -upgrade`
4. `terraform providers lock`
5. `TF_LOG=TRACE terraform plan`

## Recommended Troubleshooting Flow

1. confirm workspace and backend first
2. inspect plan or drift before apply whenever possible
3. pull and preserve state before state repair
4. prefer lock timeout over unsafe unlock
5. use targeted or replacement actions only when blast-radius reduction justifies them
6. verify outputs, state, and next plan after recovery

---

# Environment and Initialization


## 1. `terraform version`


```bash

terraform version

```

**Risk:** Low risk

**Technical explanation:** Prints the Terraform CLI version and provider protocol compatibility so you can rule out local client drift first.

**Use when:** Use when a run behaves differently across laptops, runners, or older bastion hosts.


## 2. `terraform init`


```bash

terraform init

```

**Risk:** Low risk

**Technical explanation:** Initializes modules, providers, and backend access so you can confirm the working directory can bootstrap correctly.

**Use when:** Use when entering a repo, reproducing a pipeline issue locally, or validating a clean execution path.


## 3. `terraform init -upgrade`


```bash

terraform init -upgrade

```

**Risk:** Medium risk

**Technical explanation:** Refreshes provider and module selections, which helps verify whether a defect is tied to stale dependencies.

**Use when:** Use when provider bugs, broken cache state, or outdated modules are suspected.


## 4. `terraform init -reconfigure`


```bash

terraform init -reconfigure

```

**Risk:** Medium risk

**Technical explanation:** Forces backend reconfiguration without migrating state, which helps when backend settings changed or local cache is misleading.

**Use when:** Use when Terraform appears pointed at the wrong backend or cached backend metadata looks stale.


## 5. `terraform init -migrate-state`


```bash

terraform init -migrate-state

```

**Risk:** High risk

**Technical explanation:** Moves state to the currently configured backend, which is essential for controlled backend migrations.

**Use when:** Use when a backend cutover is planned and you must validate state relocation carefully.


## 6. `terraform providers`


```bash

terraform providers

```

**Risk:** Low risk

**Technical explanation:** Lists the providers required by the configuration so you can confirm source, aliases, and inheritance across modules.

**Use when:** Use when provider mismatch or alias wiring may explain plan or apply failures.


## 7. `terraform providers lock`


```bash

terraform providers lock

```

**Risk:** Low risk

**Technical explanation:** Refreshes dependency lock data to keep provider selection consistent across machines and CI systems.

**Use when:** Use when engineers or runners resolve different provider versions for the same code.


## 8. `terraform providers mirror ./provider_mirror`


```bash

terraform providers mirror ./provider_mirror

```

**Risk:** Low risk

**Technical explanation:** Builds a local provider mirror, which is useful in restricted environments or to prove network retrieval is the issue.

**Use when:** Use when provider downloads fail from CI runners, bastions, or isolated networks.


## 9. `terraform fmt -check -recursive`


```bash

terraform fmt -check -recursive

```

**Risk:** Low risk

**Technical explanation:** Checks formatting consistency, which often catches broken merges or malformed HCL before deeper debugging.

**Use when:** Use when a branch was edited manually, rebased heavily, or generated by automation.


## 10. `terraform validate`


```bash

terraform validate

```

**Risk:** Low risk

**Technical explanation:** Performs static validation on the configuration to catch syntax, typing, and structural issues before runtime.

**Use when:** Use when a plan fails early or a module interface may be incorrect.


## 11. `terraform validate -json`


```bash

terraform validate -json

```

**Risk:** Low risk

**Technical explanation:** Returns machine-readable validation diagnostics that CI or helper scripts can parse reliably.

**Use when:** Use when you need exact file-and-line diagnostics from automated validation workflows.


## 12. `terraform force-unlock <LOCK_ID>`


```bash

terraform force-unlock <LOCK_ID>

```

**Risk:** Emergency-only

**Technical explanation:** Removes a state lock manually when a crashed run has left the backend blocked.

**Use when:** Use only after confirming no valid apply or plan still owns the lock.


# Planning and Drift Analysis


## 13. `terraform plan`


```bash

terraform plan

```

**Risk:** Low risk

**Technical explanation:** Computes the proposed infrastructure changes so you can inspect Terraform's understanding before mutation.

**Use when:** Use first in most incidents where the backend is healthy and you need to see current intent.


## 14. `terraform plan -out=tfplan`


```bash

terraform plan -out=tfplan

```

**Risk:** Low risk

**Technical explanation:** Saves the exact reviewed plan to a file so later apply uses the same computed action set.

**Use when:** Use when you want a human-reviewed plan artifact before a production apply.


## 15. `terraform plan -refresh-only`


```bash

terraform plan -refresh-only

```

**Risk:** Low risk

**Technical explanation:** Refreshes state against real infrastructure and proposes only state updates, making it ideal for drift investigation.

**Use when:** Use when production may have changed out-of-band and you want observation before change.


## 16. `terraform plan -destroy`


```bash

terraform plan -destroy

```

**Risk:** Medium risk

**Technical explanation:** Shows what Terraform would delete, which is critical when investigating accidental destroy paths or wrong scope.

**Use when:** Use when a workspace, module path, or backend target may be wrong and deletions look suspicious.


## 17. `terraform plan -target=<resource>`


```bash

terraform plan -target=<resource>

```

**Risk:** Medium risk

**Technical explanation:** Limits graph evaluation to a specific address so you can isolate one failing dependency path.

**Use when:** Use when a large stack is noisy and you need focused triage on a single resource or module.


## 18. `terraform plan -var='key=value'`


```bash

terraform plan -var='key=value'

```

**Risk:** Low risk

**Technical explanation:** Overrides a variable inline to test whether bad runtime inputs are driving unwanted changes.

**Use when:** Use when CI variables, wrapper scripts, or environment injection may be incorrect.


## 19. `terraform plan -var-file=prod.tfvars`


```bash

terraform plan -var-file=prod.tfvars

```

**Risk:** Low risk

**Technical explanation:** Loads an explicit variable file so you can reproduce the exact production input set.

**Use when:** Use when local and pipeline plans differ and you suspect variable-source mismatch.


## 20. `terraform plan -parallelism=1`


```bash

terraform plan -parallelism=1

```

**Risk:** Low risk

**Technical explanation:** Serializes graph operations during planning, which can expose ordering and provider timing problems.

**Use when:** Use when provider rate limits, flaky APIs, or dependency ordering bugs are suspected.


## 21. `terraform plan -lock=false`


```bash

terraform plan -lock=false

```

**Risk:** Medium risk

**Technical explanation:** Skips state locking for read-only diagnostic planning where waiting for a stale lock is not acceptable.

**Use when:** Use when you must inspect behavior immediately and have confirmed no active writer should own the lock.


## 22. `terraform plan -detailed-exitcode`


```bash

terraform plan -detailed-exitcode

```

**Risk:** Low risk

**Technical explanation:** Returns separate exit codes for no diff, diff, and error, which is ideal for CI-based drift detection.

**Use when:** Use when building or debugging automation that must distinguish change from failure.


## 23. `terraform show tfplan`


```bash

terraform show tfplan

```

**Risk:** Low risk

**Technical explanation:** Renders a saved plan in readable form so you can review what a pipeline intends to do.

**Use when:** Use when someone hands you a saved plan artifact and you need a human inspection quickly.


## 24. `terraform show -json tfplan`


```bash

terraform show -json tfplan

```

**Risk:** Low risk

**Technical explanation:** Outputs saved plan details as JSON for policy checks, diff tooling, or automated risk review.

**Use when:** Use when you need machine-readable access to planned actions for gates or analysis.


# Apply, Destroy, and Controlled Execution


## 25. `terraform apply`


```bash

terraform apply

```

**Risk:** Medium risk

**Technical explanation:** Executes the planned changes and surfaces runtime provider or cloud API failures that planning alone cannot reveal.

**Use when:** Use when review is complete and you are ready to observe real mutation behavior.


## 26. `terraform apply tfplan`


```bash

terraform apply tfplan

```

**Risk:** Medium risk

**Technical explanation:** Applies a previously saved plan so the executed change set exactly matches the reviewed artifact.

**Use when:** Use when production controls require plan approval before apply.


## 27. `terraform apply -auto-approve`


```bash

terraform apply -auto-approve

```

**Risk:** High risk

**Technical explanation:** Runs an apply without interactive confirmation, which is common in CI but increases accidental-change risk.

**Use when:** Use when reproducing automation behavior or operating in non-interactive recovery workflows.


## 28. `terraform apply -target=<resource>`


```bash

terraform apply -target=<resource>

```

**Risk:** High risk

**Technical explanation:** Applies changes only to a targeted address, reducing scope but risking graph incompleteness if misused.

**Use when:** Use when a focused recovery is justified and full-stack apply is unnecessarily risky.


## 29. `terraform apply -replace=<resource>`


```bash

terraform apply -replace=<resource>

```

**Risk:** High risk

**Technical explanation:** Forces replacement of a specific resource when normal drift resolution will not recreate a broken object.

**Use when:** Use when the resource is unhealthy and replacement is the cleanest safe recovery path.


## 30. `terraform apply -refresh-only`


```bash

terraform apply -refresh-only

```

**Risk:** Low risk

**Technical explanation:** Updates state from real infrastructure without modifying the infrastructure itself.

**Use when:** Use when verifying that drift, not desired config change, explains the incident.


## 31. `terraform apply -parallelism=1`


```bash

terraform apply -parallelism=1

```

**Risk:** Medium risk

**Technical explanation:** Serializes apply operations to reduce concurrency-related provider, API, or dependency issues.

**Use when:** Use when applies fail unpredictably under normal parallel execution.


## 32. `terraform destroy`


```bash

terraform destroy

```

**Risk:** Emergency-only

**Technical explanation:** Deletes managed infrastructure in the current scope and therefore has the largest possible blast radius.

**Use when:** Use only for controlled teardown or disaster-response workflows with explicit approval.


## 33. `terraform destroy -target=<resource>`


```bash

terraform destroy -target=<resource>

```

**Risk:** High risk

**Technical explanation:** Destroys only one targeted resource so a broken object can be cleaned up without broad teardown.

**Use when:** Use when a single corrupted or obsolete object must be removed carefully.


## 34. `terraform destroy -refresh=false`


```bash

terraform destroy -refresh=false

```

**Risk:** High risk

**Technical explanation:** Skips refresh during destroy, which can help during API degradation but increases risk if state is stale.

**Use when:** Use when emergency cleanup is required and you have already validated state sufficiently.


## 35. `terraform apply -lock-timeout=5m`


```bash

terraform apply -lock-timeout=5m

```

**Risk:** Low risk

**Technical explanation:** Waits for a backend lock instead of failing immediately or disabling locking unsafely.

**Use when:** Use when another legitimate run may still be finishing.


## 36. `terraform apply -json`


```bash

terraform apply -json

```

**Risk:** Medium risk

**Technical explanation:** Streams apply output in machine-readable form for advanced pipeline debugging and event correlation.

**Use when:** Use when automation tooling needs structured apply events rather than terminal text.


# State Inspection and Repair


## 37. `terraform state list`


```bash

terraform state list

```

**Risk:** Low risk

**Technical explanation:** Lists tracked resource addresses so you can confirm what Terraform believes it manages.

**Use when:** Use when inventory seems wrong or you suspect missing or duplicate state coverage.


## 38. `terraform state show <resource>`


```bash

terraform state show <resource>

```

**Risk:** Low risk

**Technical explanation:** Displays stored attributes for one resource so you can compare state to cloud reality.

**Use when:** Use when a single object behaves strangely and you need Terraform's exact view.


## 39. `terraform state pull`


```bash

terraform state pull

```

**Risk:** Low risk

**Technical explanation:** Downloads raw state from the backend before any manual repair so you preserve evidence safely.

**Use when:** Use before state surgery, backend recovery, or postmortem analysis.


## 40. `terraform state push state.json`


```bash

terraform state push state.json

```

**Risk:** Emergency-only

**Technical explanation:** Uploads a local state file back to the backend, which can repair state but also damage lineage if mishandled.

**Use when:** Use only in controlled recovery after taking verified backups and peer review.


## 41. `terraform state mv <src> <dst>`


```bash

terraform state mv <src> <dst>

```

**Risk:** High risk

**Technical explanation:** Moves a tracked address in state to align refactors without recreating live infrastructure.

**Use when:** Use when modules, names, or paths changed but the real object must stay untouched.


## 42. `terraform state rm <resource>`


```bash

terraform state rm <resource>

```

**Risk:** High risk

**Technical explanation:** Removes a resource from state while leaving the real object intact, breaking Terraform's management link intentionally.

**Use when:** Use when state is wrong and you need to stop Terraform from acting on a live object temporarily or permanently.


## 43. `terraform state replace-provider <old> <new>`


```bash

terraform state replace-provider <old> <new>

```

**Risk:** High risk

**Technical explanation:** Rewrites provider references stored in state, which is vital during provider source migration.

**Use when:** Use when provider namespace or source changes would otherwise orphan state objects.


## 44. `terraform state list | grep <pattern>`


```bash

terraform state list | grep <pattern>

```

**Risk:** Low risk

**Technical explanation:** Filters the state inventory quickly so you can find related resources in very large stacks.

**Use when:** Use when triaging by naming convention, module prefix, or resource family.


## 45. `terraform show`


```bash

terraform show

```

**Risk:** Low risk

**Technical explanation:** Prints the current state in human-readable form, including outputs and tracked objects.

**Use when:** Use when you need a broad snapshot without editing raw state JSON.


## 46. `terraform show -json`


```bash

terraform show -json

```

**Risk:** Low risk

**Technical explanation:** Exports current state as JSON for scripting, audits, or deeper structural inspection.

**Use when:** Use when you need automated analysis rather than terminal-readable output.


## 47. `terraform taint <resource>`


```bash

terraform taint <resource>

```

**Risk:** Medium risk

**Technical explanation:** Marks a resource for recreation in legacy-style workflows when you know replacement is required.

**Use when:** Use when older workflows still rely on taint semantics and a rebuild is intentional.


## 48. `terraform untaint <resource>`


```bash

terraform untaint <resource>

```

**Risk:** Low risk

**Technical explanation:** Removes a taint marker to prevent unwanted recreation.

**Use when:** Use when forced replacement is no longer the correct recovery path.


## 49. `terraform import <resource> <id>`


```bash

terraform import <resource> <id>

```

**Risk:** High risk

**Technical explanation:** Associates an existing real object with state so Terraform can manage it going forward.

**Use when:** Use when production infrastructure exists but state coverage was lost or never created.


## 50. `terraform state mv 'module.old.aws_instance.web' 'module.new.aws_instance.web'`


```bash

terraform state mv 'module.old.aws_instance.web' 'module.new.aws_instance.web'

```

**Risk:** High risk

**Technical explanation:** Demonstrates a real refactor-safe state move between module paths to preserve a production object.

**Use when:** Use when a module reorganization would otherwise cause destructive recreation.


# Console, Expressions, and Outputs


## 51. `terraform console`


```bash

terraform console

```

**Risk:** Low risk

**Technical explanation:** Opens an interactive evaluator for variables, locals, functions, and expressions.

**Use when:** Use when module logic, type conversion, or computed values are unclear.


## 52. `terraform console <<< 'var.environment'`


```bash

terraform console <<< 'var.environment'

```

**Risk:** Low risk

**Technical explanation:** Evaluates one variable non-interactively for quick shell-based debugging.

**Use when:** Use when you need fast confirmation of a specific input value.


## 53. `terraform console <<< 'local.tags'`


```bash

terraform console <<< 'local.tags'

```

**Risk:** Low risk

**Technical explanation:** Prints a local value so you can inspect merged maps or computed metadata logic.

**Use when:** Use when tagging, policy docs, or merged locals are behaving unexpectedly.


## 54. `terraform console <<< 'jsonencode(var.policy)'`


```bash

terraform console <<< 'jsonencode(var.policy)'

```

**Risk:** Low risk

**Technical explanation:** Shows the exact rendered JSON payload for policies or API-bound documents.

**Use when:** Use when provider errors suggest malformed JSON or escaping issues.


## 55. `terraform console <<< 'length(module.vpc.private_subnets)'`


```bash

terraform console <<< 'length(module.vpc.private_subnets)'

```

**Risk:** Low risk

**Technical explanation:** Checks computed collection sizes to debug count, for_each, or module output shape.

**Use when:** Use when loops, indexes, or subnet counts differ from expectation.


## 56. `terraform output`


```bash

terraform output

```

**Risk:** Low risk

**Technical explanation:** Lists root outputs so you can verify what downstream systems should be consuming.

**Use when:** Use when another stack, app, or pipeline claims Terraform outputs are wrong.


## 57. `terraform output <name>`


```bash

terraform output <name>

```

**Risk:** Low risk

**Technical explanation:** Returns one named output directly for targeted dependency checks.

**Use when:** Use when validating a single connection string, ID, or endpoint quickly.


## 58. `terraform output -json`


```bash

terraform output -json

```

**Risk:** Low risk

**Technical explanation:** Exports outputs in JSON for automation-friendly validation or diffing.

**Use when:** Use when scripts or CI jobs consume Terraform outputs programmatically.


## 59. `terraform console <<< 'can(var.optional_key)'`


```bash

terraform console <<< 'can(var.optional_key)'

```

**Risk:** Low risk

**Technical explanation:** Tests whether an expression can be evaluated safely without hard failure.

**Use when:** Use when optional objects or nested attributes may not exist.


## 60. `terraform console <<< 'try(var.obj.missing, "fallback")'`


```bash

terraform console <<< 'try(var.obj.missing, "fallback")'

```

**Risk:** Low risk

**Technical explanation:** Evaluates fallback logic used in complex module interfaces or optional attribute chains.

**Use when:** Use when null handling or fallback expressions may explain bad plans.


# Workspace and Environment Isolation


## 61. `terraform workspace list`


```bash

terraform workspace list

```

**Risk:** Low risk

**Technical explanation:** Lists available workspaces so you can confirm which environment partitions exist.

**Use when:** Use when backend targeting or missing environment state is in doubt.


## 62. `terraform workspace show`


```bash

terraform workspace show

```

**Risk:** Low risk

**Technical explanation:** Prints the current workspace, which is one of the fastest checks against wrong-environment execution.

**Use when:** Use before any production plan, apply, import, or state repair.


## 63. `terraform workspace select prod`


```bash

terraform workspace select prod

```

**Risk:** Medium risk

**Technical explanation:** Switches the current context to the production workspace.

**Use when:** Use when reproducing production behavior locally and you must confirm scope explicitly.


## 64. `terraform workspace new staging`


```bash

terraform workspace new staging

```

**Risk:** Medium risk

**Technical explanation:** Creates a new workspace, helping test isolation or recover a missing environment partition.

**Use when:** Use when bootstrapping a new environment partition or recovering from workspace absence.


## 65. `terraform workspace delete dev`


```bash

terraform workspace delete dev

```

**Risk:** High risk

**Technical explanation:** Deletes an unused workspace reference, which can be disruptive if the wrong workspace is targeted.

**Use when:** Use when cleaning up obsolete environment partitions after verifying they are no longer needed.


## 66. `terraform workspace select default`


```bash

terraform workspace select default

```

**Risk:** Low risk

**Technical explanation:** Returns to the default workspace to avoid accidental continued work in a sensitive environment.

**Use when:** Use after production debugging or when resetting local context.


## 67. `TF_WORKSPACE=prod terraform plan`


```bash

TF_WORKSPACE=prod terraform plan

```

**Risk:** Low risk

**Technical explanation:** Pins workspace selection via environment variable so automation and shell commands target the intended partition.

**Use when:** Use when reproducing CI behavior that does not call workspace select explicitly.


## 68. `terraform init -backend-config=backend-prod.hcl`


```bash

terraform init -backend-config=backend-prod.hcl

```

**Risk:** Medium risk

**Technical explanation:** Loads backend settings from a file so you can reproduce environment-specific backend wiring accurately.

**Use when:** Use when local and CI runs differ because backend config is injected externally.


## 69. `terraform plan -var-file=environments/prod.tfvars`


```bash

terraform plan -var-file=environments/prod.tfvars

```

**Risk:** Low risk

**Technical explanation:** Forces the production variable set so the plan matches the intended environment inputs.

**Use when:** Use when local tests must mirror the exact production variable source.


## 70. `env | grep ^TF_`


```bash

env | grep ^TF_

```

**Risk:** Low risk

**Technical explanation:** Lists Terraform-related environment variables that may silently alter behavior.

**Use when:** Use when wrapper scripts, CI runners, or shell sessions produce inconsistent results.


# Modules and Dependency Troubleshooting


## 71. `terraform get -update`


```bash

terraform get -update

```

**Risk:** Medium risk

**Technical explanation:** Downloads and refreshes child modules in older workflows so you can detect stale source checkout issues.

**Use when:** Use when module code changes are not reflected locally or cached module content looks wrong.


## 72. `terraform providers schema -json`


```bash

terraform providers schema -json

```

**Risk:** Low risk

**Technical explanation:** Outputs provider schemas in JSON so you can inspect expected arguments and computed attributes.

**Use when:** Use when field-level provider behavior is unclear or tooling needs schema access.


## 73. `terraform graph`


```bash

terraform graph

```

**Risk:** Low risk

**Technical explanation:** Builds the dependency graph so you can inspect hidden relationships and ordering.

**Use when:** Use when complex implicit dependencies or cycles are suspected.


## 74. `terraform graph -type=plan`


```bash

terraform graph -type=plan

```

**Risk:** Low risk

**Technical explanation:** Builds the plan-time dependency graph to show evaluation relationships before apply.

**Use when:** Use when planning appears blocked by hidden edges or cycles.


## 75. `terraform graph -type=apply`


```bash

terraform graph -type=apply

```

**Risk:** Low risk

**Technical explanation:** Builds the apply-time graph so you can compare executed dependency behavior with planned relationships.

**Use when:** Use when apply ordering differs from what the team expects.


## 76. `terraform graph | dot -Tsvg > graph.svg`


```bash

terraform graph | dot -Tsvg > graph.svg

```

**Risk:** Low risk

**Technical explanation:** Converts the graph into an SVG for easier review in large infrastructures.

**Use when:** Use when text output is insufficient for module-heavy dependency debugging.


## 77. `terraform console <<< 'module.network.vpc_id'`


```bash

terraform console <<< 'module.network.vpc_id'

```

**Risk:** Low risk

**Technical explanation:** Evaluates a module output directly so you can verify cross-module wiring.

**Use when:** Use when one module depends on another and IDs or outputs appear incorrect.


## 78. `terraform state list | grep '^module\.'`


```bash

terraform state list | grep '^module\.'

```

**Risk:** Low risk

**Technical explanation:** Filters state to module-managed resources so you can isolate module impact quickly.

**Use when:** Use when a large estate needs module-scoped triage.


## 79. `terraform providers | grep module`


```bash

terraform providers | grep module

```

**Risk:** Low risk

**Technical explanation:** Shows module-related provider associations, helping diagnose alias inheritance mistakes.

**Use when:** Use when child modules appear to use the wrong provider instance.


## 80. `terraform plan -target=module.<name>`


```bash

terraform plan -target=module.<name>

```

**Risk:** Medium risk

**Technical explanation:** Narrows planning to one module path so you can isolate a failing area in a large monorepo.

**Use when:** Use when a single module is broken and full-stack plans are too noisy or risky.


# Logging, Debugging, and Automation


## 81. `TF_LOG=TRACE terraform plan`


```bash

TF_LOG=TRACE terraform plan

```

**Risk:** Low risk

**Technical explanation:** Enables very verbose Terraform and provider logs so you can trace graph evaluation and API calls deeply.

**Use when:** Use when a plan fails mysteriously and normal output is not enough.


## 82. `TF_LOG=DEBUG terraform apply`


```bash

TF_LOG=DEBUG terraform apply

```

**Risk:** Medium risk

**Technical explanation:** Turns on debug logging during apply to expose provider request flow and runtime failure context.

**Use when:** Use when apply fails inside provider operations with vague messages.


## 83. `TF_LOG_PATH=terraform.log terraform plan`


```bash

TF_LOG_PATH=terraform.log terraform plan

```

**Risk:** Low risk

**Technical explanation:** Writes logs to a file so debugging evidence is preserved and shareable.

**Use when:** Use when terminal logs are too noisy or you need artifacts for incident review.


## 84. `TF_INPUT=0 terraform apply`


```bash

TF_INPUT=0 terraform apply

```

**Risk:** Low risk

**Technical explanation:** Disables interactive prompts, helping you reproduce CI behavior exactly.

**Use when:** Use when debugging differences between local manual runs and non-interactive automation.


## 85. `TF_CLI_ARGS_plan='-lock-timeout=5m' terraform plan`


```bash

TF_CLI_ARGS_plan='-lock-timeout=5m' terraform plan

```

**Risk:** Low risk

**Technical explanation:** Injects default plan flags through environment variables to mirror hidden automation behavior.

**Use when:** Use when CI wrappers or environment defaults may explain plan differences.


## 86. `TF_CLI_ARGS_apply='-parallelism=1' terraform apply`


```bash

TF_CLI_ARGS_apply='-parallelism=1' terraform apply

```

**Risk:** Medium risk

**Technical explanation:** Injects default apply flags via environment variables to reproduce automation-sensitive concurrency behavior.

**Use when:** Use when production applies only fail under the platform wrapper or CI job.


## 87. `terraform plan -json`


```bash

terraform plan -json

```

**Risk:** Low risk

**Technical explanation:** Streams machine-readable plan output so policy engines and tooling can inspect changes structurally.

**Use when:** Use when debugging automated plan parsing or custom gates.


## 88. `terraform show -json tfplan | jq '.resource_changes[] | {address, actions: .change.actions}'`


```bash

terraform show -json tfplan | jq '.resource_changes[] | {address, actions: .change.actions}'

```

**Risk:** Low risk

**Technical explanation:** Extracts resource action summaries from a saved plan so you can quickly spot replacements or deletes.

**Use when:** Use when you need a fast risk review of a large plan.


## 89. `terraform show -json | jq '.values.root_module.resources[]?.address'`


```bash

terraform show -json | jq '.values.root_module.resources[]?.address'

```

**Risk:** Low risk

**Technical explanation:** Lists resource addresses from state JSON for auditing or tooling checks.

**Use when:** Use when building or debugging automated state inventory logic.


## 90. `terraform validate -json | jq '.diagnostics[]?'`


```bash

terraform validate -json | jq '.diagnostics[]?'

```

**Risk:** Low risk

**Technical explanation:** Parses validation diagnostics cleanly so pipelines can surface exact HCL problems.

**Use when:** Use when CI reports validation failure but the raw output is hard to consume.


## 91. `terraform output -json | jq`


```bash

terraform output -json | jq

```

**Risk:** Low risk

**Technical explanation:** Pretty-prints Terraform outputs for rapid inspection and handoff validation.

**Use when:** Use when downstream jobs or humans need readable structured output.


## 92. `terraform providers schema -json | jq '.provider_schemas | keys'`


```bash

terraform providers schema -json | jq '.provider_schemas | keys'

```

**Risk:** Low risk

**Technical explanation:** Lists provider schema keys so you can verify which providers are actually loaded.

**Use when:** Use when aliased, inherited, or unexpected provider sources are confusing the run.


# Cloud, Backend, and Lock Troubleshooting


## 93. `terraform login`


```bash

terraform login

```

**Risk:** Low risk

**Technical explanation:** Authenticates the CLI with HCP Terraform or Terraform Cloud so remote operations can be tested properly.

**Use when:** Use when remote runs, remote state, or cloud-backed CLI actions fail with auth errors.


## 94. `terraform logout`


```bash

terraform logout

```

**Risk:** Low risk

**Technical explanation:** Clears stored Terraform Cloud credentials to remove stale or wrong authentication context.

**Use when:** Use when token confusion or credential switching is causing inconsistent behavior.


## 95. `terraform init -backend=false`


```bash

terraform init -backend=false

```

**Risk:** Low risk

**Technical explanation:** Initializes the configuration without touching the backend so you can debug code and modules even if state service is down.

**Use when:** Use when backend outages block simple syntax or module troubleshooting.


## 96. `terraform plan -lock-timeout=10m`


```bash

terraform plan -lock-timeout=10m

```

**Risk:** Low risk

**Technical explanation:** Waits longer for a backend lock instead of failing or forcing an unlock too early.

**Use when:** Use when another legitimate run may still be finalizing state operations.


## 97. `terraform force-unlock -force <LOCK_ID>`


```bash

terraform force-unlock -force <LOCK_ID>

```

**Risk:** Emergency-only

**Technical explanation:** Forcefully removes a backend lock without prompt, which can be dangerous if the lock is still valid.

**Use when:** Use only after positive confirmation that the original run is dead and state writes have stopped.


## 98. `terraform state pull > pulled_state.json`


```bash

terraform state pull > pulled_state.json

```

**Risk:** Low risk

**Technical explanation:** Captures remote state to a local file before repair so you retain evidence and rollback material.

**Use when:** Use before any manual state correction or backend recovery step.


## 99. `terraform show pulled_state.json`


```bash

terraform show pulled_state.json

```

**Risk:** Low risk

**Technical explanation:** Renders a pulled state file for safe offline inspection before pushing any repair.

**Use when:** Use when you need to inspect repaired or archived state without touching the backend.


## 100. `terraform state push -force repaired_state.json`


```bash

terraform state push -force repaired_state.json

```

**Risk:** Emergency-only

**Technical explanation:** Pushes a repaired state file even when lineage differs, which can recover state but is highly dangerous.

**Use when:** Use only in peer-reviewed emergency repair after backups and validation.


# Recovery Verification by Scenario

## After state repair
Re-run:

- `terraform workspace show`
- `terraform state list`
- `terraform show`
- `terraform plan`

Verify that state inventory looks complete, rendered state is sane, and the next plan is controlled.

## After forced unlock
Re-run:

- `terraform workspace show`
- `terraform plan -lock-timeout=10m`
- `terraform state pull > pulled_state.json`

Verify that no legitimate writer still exists and the backend lock path is healthy again.

## After backend migration or reconfiguration
Re-run:

- `terraform init`
- `terraform workspace list`
- `terraform state pull`
- `terraform plan`

Verify that the expected state is present in the intended backend and that the next plan does not suggest accidental recreation.

## After import
Re-run:

- `terraform state show <resource>`
- `terraform plan`
- `terraform output`

Verify that the imported object is mapped correctly, no surprise replacement is planned, and outputs still align with reality.

## After targeted apply or replace
Re-run:

- `terraform plan`
- `terraform state show <resource>`
- `terraform output`
- `terraform show -json tfplan`

Verify that the focused recovery did not leave hidden follow-on changes elsewhere in the graph.

# Troubleshooting Discipline

Strong Terraform incident response is not about knowing the most commands. It is about:

- knowing which commands are safe
- preserving evidence before repair
- minimizing state and infrastructure blast radius
- recognizing when targeted actions are appropriate
- verifying that Terraform, state, and real infrastructure agree again

That is the standard expected from a strong senior Terraform Platform, DevOps, or SRE engineer.
