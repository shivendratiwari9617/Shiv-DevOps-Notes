# 10 Terraform Production Issues and Troubleshooting Steps — Interview-Ready Answers

This file is written for **interview revision** and **production troubleshooting thinking**.  
Each issue is structured as:

1. **Interview question**
2. **What I would say**
3. **Failure boundary**
4. **Troubleshooting steps**
5. **Redacted log / trace evidence**
6. **Fix**
7. **Prevention**
8. **Open-source references**

> The logs and traces below are **realistic redacted examples** based on common Terraform production failures. They are included to show how to reason from evidence, not as vendor incident transcripts.

---

## 1) Terraform plan wants to recreate a production resource after import

### Interview question
**You imported a production resource into Terraform, but the next plan shows destroy-and-recreate. How would you troubleshoot it?**

### What I would say
If `terraform import` succeeded but the next plan still wants to replace a production resource, I would stop immediately and assume there is a **state-to-configuration mismatch**. My first goal is not to clean up code or refactor modules. It is to get to a plan that is either no-op or fully explained. I would compare the imported resource address, provider context, live API values, and any immutable attributes that force replacement.

### Failure boundary
Typical causes:
- imported to the wrong module path
- wrong provider alias or wrong account/region/project/subscription
- immutable field in code differs from the live resource
- provider-computed defaults missing from config
- code models desired future state, not current production state

### Troubleshooting steps
1. Run `terraform state show` for the imported object.
2. Compare the state to the real cloud API response.
3. Verify the provider alias and target environment.
4. Identify any ForceNew or replacement-causing attributes.
5. Confirm the resource address in state matches the actual module path in code.
6. Remove refactor changes from the same PR if they are mixed with import stabilization.

### Redacted log / trace evidence
```text
terraform plan

-/+ resource "aws_db_instance" "prod" {
      identifier           = "orders-prod-db"
    ~ parameter_group_name = "custom-pg" -> "default.postgres15"
    ~ storage_encrypted    = true -> false # forces replacement
    ~ kms_key_id           = "arn:aws:kms:..." -> null # forces replacement
  }

Plan: 1 to add, 0 to change, 1 to destroy.
```

### Fix
Bring the configuration into alignment with the live resource first. Correct the provider context, preserve immutable fields, and postpone refactors until the imported resource stabilizes under Terraform.

### Prevention
Import by logical stack, freeze manual changes during import, and require the first post-import plan to be no-op before any cleanup refactor.

### Open-source references
- Terraform import: https://developer.hashicorp.com/terraform/cli/import
- Terraform state: https://developer.hashicorp.com/terraform/language/state
- Terraform resource addressing: https://developer.hashicorp.com/terraform/cli/state/resource-addressing

---

## 2) CI/CD pipeline can plan but fails at apply with AccessDenied

### Interview question
**A pipeline can run `terraform plan`, but `terraform apply` fails with permission errors. How would you debug it?**

### What I would say
If plan works but apply fails, I assume the pipeline has **read capability but not the exact write capability needed for mutation**. I would reconstruct the identity chain: CI principal, assumed role or service account, permission boundary, org policy, and the exact denied API call. In production, this often happens when teams widen read permissions for planning but forget that apply needs create, update, tag, attach, or pass-role permissions.

### Failure boundary
Typical causes:
- deploy role missing a specific write action
- trust policy or impersonation target differs between plan and apply stages
- permission boundary or SCP blocks writes
- provider tries a hidden auxiliary API call
- apply runs in a different environment or workspace than expected

### Troubleshooting steps
1. Find the exact denied action from the apply output or audit logs.
2. Compare the identity used in plan vs apply.
3. Check trust policy / impersonation / workload identity chain.
4. Check permission boundaries, SCPs, or org policies.
5. Review provider docs for any dependent API calls like tagging, KMS, IAM pass-role, or attachment operations.
6. Verify the backend/workspace/environment is the intended one.

### Redacted log / trace evidence
```text
Error: creating IAM Role (payments-prod-app-role): AccessDenied: User is not authorized to perform iam:CreateRole

Cloud audit:
principal=arn:aws:sts::123456789012:assumed-role/ci-apply-role/run-8172
eventName=CreateRole
errorCode=AccessDenied
```

### Fix
Grant the minimum missing write permission to the apply identity, and ensure plan/apply use the intended role chain and environment consistently.

### Prevention
Keep separate plan and apply roles documented, test role assumption explicitly in CI, and audit denied actions during non-prod dry runs.

### Open-source references
- Terraform CLI apply: https://developer.hashicorp.com/terraform/cli/commands/apply
- Terraform provider authentication patterns: https://developer.hashicorp.com/terraform/language/providers/configuration

---

## 3) Remote state lock never clears and blocks deployments

### Interview question
**Terraform says the state is locked, but no deployment is actually running. What would you do?**

### What I would say
If the state lock is stuck, I treat that as a **coordination and correctness issue**, not just an inconvenience. I first confirm whether a real apply is still running. If not, I identify the lock owner, verify that the state is not being mutated, and only then consider force-unlocking. In production, unlocking state without proving there is no live writer can create state corruption or conflicting resource changes.

### Failure boundary
Common causes:
- pipeline crashed and left a stale lock
- apply still running in another runner or terminal
- backend outage interrupted lock release
- multiple teams point to the same backend key by mistake

### Troubleshooting steps
1. Identify lock metadata from the error message.
2. Search CI runs, runners, or terminals for an active Terraform process.
3. Verify the backend key is correct and not shared across stacks.
4. Confirm no recent state writes are still in progress.
5. Only then use `terraform force-unlock` if you have proven the lock is stale.
6. Re-run plan before apply after unlocking.

### Redacted log / trace evidence
```text
Error acquiring the state lock

Lock Info:
  ID:        7f2c9c2f-8d5a-1c3d-91d2-2b0f1de2cbe7
  Path:      terraform/prod/network.tfstate
  Operation: OperationTypeApply
  Who:       ci-runner@build-142
  Version:   1.7.5
  Created:   2026-10-04 08:12:41.911 +0000 UTC
```

### Fix
Clear the stale lock only after confirming no real writer exists, then re-run plan and apply in a controlled way.

### Prevention
Use one backend key per stack, serialize applies per environment, and make pipeline cancellation handlers clean up state-locking paths gracefully.

### Open-source references
- State locking: https://developer.hashicorp.com/terraform/language/state/locking
- Force unlock: https://developer.hashicorp.com/terraform/cli/commands/force-unlock

---

## 4) Drift exists in production, but Terraform does not behave the way the team expects

### Interview question
**Someone changed infrastructure manually, and now Terraform behaves unexpectedly. How would you approach drift in production?**

### What I would say
I would treat manual drift as a **source-of-truth conflict**. The first step is to understand whether the live infrastructure should be preserved or reverted. Then I compare Terraform state, configuration, and the live resource to see which side is wrong. In production, the mistake is applying quickly before deciding whether the drift is intentional, emergency remediation, or accidental change.

### Failure boundary
Typical causes:
- emergency manual change never back-ported into code
- console change modified a managed attribute
- unmanaged dependency changed behavior
- provider refresh exposed drift the team did not know existed

### Troubleshooting steps
1. Detect the drift with plan and live API comparison.
2. Determine whether the live change was intentional and must be kept.
3. If the live change is correct, update Terraform code to match.
4. If the live change is wrong, plan the safest way to revert through Terraform.
5. Review whether state refresh changed any assumptions in the provider.
6. Avoid mixing drift correction with unrelated feature changes.

### Redacted log / trace evidence
```text
terraform plan

  # aws_security_group.web will be updated in-place
  ~ resource "aws_security_group" "web" {
      ~ ingress = [
          - {
              from_port   = 22
              to_port     = 22
              protocol    = "tcp"
              cidr_blocks = ["0.0.0.0/0"]
            },
        ]
    }

Note: Objects have changed outside of Terraform
```

### Fix
Decide whether to preserve or revert the drift, then update code or apply a controlled correction. The key is to restore one clear source of truth.

### Prevention
Reduce console access, alert on drift, and require emergency production changes to be back-ported into code immediately after the incident.

### Open-source references
- Terraform plan: https://developer.hashicorp.com/terraform/cli/commands/plan
- Terraform state overview: https://developer.hashicorp.com/terraform/language/state

---

## 5) Module refactor causes resources to move or recreate unexpectedly

### Interview question
**A module refactor causes Terraform to think resources moved or need recreation. How would you handle that safely?**

### What I would say
I would treat a module refactor as a **state-address migration problem**, not just a code cleanup. If resource addresses changed, Terraform may think the old resource disappeared and a new one must be created. I would map old addresses to new ones carefully and use state moves or moved blocks rather than trusting Terraform to infer intent.

### Failure boundary
Typical causes:
- resource moved into or out of a module
- `count` changed to `for_each`
- resource names changed
- module nesting changed resource addresses
- refactor mixed with functional changes

### Troubleshooting steps
1. Compare old and new resource addresses.
2. Identify whether the real object is the same object with a new logical address.
3. Use `moved` blocks or state move operations where appropriate.
4. Separate structural refactor from behavioral changes.
5. Run plan repeatedly until it shows address migration instead of replacement.
6. Review any resources with immutable attributes before apply.

### Redacted log / trace evidence
```text
terraform plan

  # module.network.aws_subnet.private[0] must be replaced
  -/+ resource "aws_subnet" "private" {
      ~ id                = "subnet-09af..." -> (known after apply)
      ~ availability_zone = "us-east-1a" -> "us-east-1a"
    }

Plan: 1 to add, 0 to change, 1 to destroy.
```

### Fix
Use explicit moved-state handling so Terraform understands the object identity did not change. Keep refactor-only changes isolated from functional infrastructure changes.

### Prevention
Refactor in small steps, use `moved` blocks where possible, and never combine large module reorganization with critical production changes.

### Open-source references
- Refactoring with moved blocks: https://developer.hashicorp.com/terraform/language/modules/develop/refactoring
- State move command: https://developer.hashicorp.com/terraform/cli/commands/state/mv

---

## 6) Terraform destroys and recreates resources because of `count` or `for_each` mistakes

### Interview question
**A harmless-looking variable change caused many resources to be recreated. What likely happened and how would you troubleshoot it?**

### What I would say
If many resources are suddenly recreated after a small variable change, I suspect a **resource identity problem** caused by `count`, `for_each`, or unstable keys. Terraform does not track resources by human intention. It tracks them by address and key. If those keys change, production resources can churn even when the underlying design did not need to.

### Failure boundary
Typical causes:
- list order changed while using `count`
- `for_each` keys derived from unstable values
- switching between `count` and `for_each`
- filtered collections changed identity unexpectedly

### Troubleshooting steps
1. Identify whether resource addresses or instance keys changed.
2. Check whether list ordering changed between commits.
3. Review how `for_each` keys are constructed.
4. Compare old state addresses with new plan addresses.
5. Convert unstable indexing patterns into stable named keys.
6. Use state moves if the object identity is logically unchanged.

### Redacted log / trace evidence
```text
terraform plan

  # aws_security_group_rule.app[0] must be replaced
  -/+ resource "aws_security_group_rule" "app" {
      ~ cidr_blocks = [
          - "10.1.0.0/16",
          + "10.2.0.0/16",
        ]
    }

Plan: 12 to add, 0 to change, 12 to destroy.
```

### Fix
Use stable keys with `for_each`, avoid relying on list order, and map old addresses to new addresses before applying.

### Prevention
Prefer explicit names over positional indexes and review any `count`/`for_each` change as a production-risk change, not a cosmetic one.

### Open-source references
- `count` meta-argument: https://developer.hashicorp.com/terraform/language/meta-arguments/count
- `for_each` meta-argument: https://developer.hashicorp.com/terraform/language/meta-arguments/for_each

---

## 7) Unknown values at plan time break dependencies or conditional logic

### Interview question
**Terraform fails or behaves unexpectedly because values are only known after apply. How would you explain and debug that?**

### What I would say
If Terraform cannot decide something at plan time because a value is only known after apply, I treat it as a **graph and dependency design issue**. Terraform needs enough information during planning to build the dependency graph safely. I would identify which values are unknown, which expressions depend on them, and whether the module design is forcing conditional logic too early.

### Failure boundary
Common causes:
- conditional resource creation depends on computed values
- `for_each` keys depend on values known only after apply
- outputs from one module are used too dynamically in another
- data sources depend on resources not created yet

### Troubleshooting steps
1. Read the exact expression Terraform says is unknown.
2. Identify whether the unknown value is computed by a resource created in the same apply.
3. Check whether `count`, `for_each`, or conditionals depend on that value.
4. Separate graph creation from later configuration where possible.
5. Replace unstable dynamic keys with explicit inputs.
6. Break the flow into stages only if truly necessary.

### Redacted log / trace evidence
```text
Error: Invalid for_each argument

The "for_each" map includes keys derived from resource attributes that cannot be determined until apply.

module.network.private_subnet_ids will be known only after apply
```

### Fix
Redesign the module boundary so Terraform knows the resource graph at plan time. Use explicit inputs or stable keys instead of apply-time values for collection identity.

### Prevention
Keep module interfaces predictable and avoid using computed values to decide how many resources exist.

### Open-source references
- Expressions and unknown values: https://developer.hashicorp.com/terraform/language/expressions
- Depends on meta-argument: https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on

---

## 8) Provider upgrade changes behavior and breaks production plans

### Interview question
**A provider version upgrade caused unexpected plan changes. How would you approach it?**

### What I would say
If a provider upgrade changes plan behavior, I would assume the issue is **provider semantics changing**, not random Terraform instability. Providers can change defaults, schema behavior, computed attributes, and diff logic. I would pin the version, compare release notes, isolate the changed resources, and test the new provider in a safe environment before any production apply.

### Failure boundary
Typical causes:
- provider default changed
- previously ignored field now appears in diff
- new validation rejects old config
- computed field now normalized differently
- provider bug fixed, exposing old assumptions

### Troubleshooting steps
1. Compare old and new provider versions.
2. Re-run plan with the previous pinned version if possible.
3. Identify which resources changed only because of the upgrade.
4. Read provider release notes and schema changes.
5. Test the upgrade in lower environments with the same state shape.
6. Separate provider upgrade PRs from infrastructure changes.

### Redacted log / trace evidence
```text
terraform init -upgrade

Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 5.0"...
- Installing hashicorp/aws v5.45.0...

terraform plan
~ resource "aws_s3_bucket" "logs" {
    acl = "private" -> null
  }

Plan: 0 to add, 1 to change, 0 to destroy.
```

### Fix
Pin the known-good version if needed, then adapt the code deliberately to the new provider behavior in a controlled rollout.

### Prevention
Use provider version pinning, run upgrade tests in non-prod first, and isolate provider upgrades into dedicated reviewable changes.

### Open-source references
- Terraform provider requirements: https://developer.hashicorp.com/terraform/language/providers/requirements
- Dependency lock file: https://developer.hashicorp.com/terraform/language/files/dependency-lock

---

## 9) Data source reads the wrong object and the plan targets the wrong environment

### Interview question
**Terraform is reading the wrong VPC, subnet, or image and planning against the wrong environment. How would you troubleshoot it?**

### What I would say
If Terraform reads the wrong object through a data source, I assume the problem is **selection ambiguity**. Data sources feel safe because they are read-only, but they can still point the whole plan at the wrong infrastructure. I would inspect the filters, provider alias, region or subscription context, and naming assumptions behind the lookup.

### Failure boundary
Typical causes:
- data source filter is too broad
- provider alias points at the wrong account/region/project
- duplicated names exist across environments
- tags are inconsistent
- workspace or variable values selected the wrong target

### Troubleshooting steps
1. Inspect the exact data source filters in code.
2. Verify provider alias, region, and credentials.
3. Check whether names or tags are unique enough.
4. Print or output the selected object IDs in the plan path.
5. Confirm workspace and variable files are correct.
6. Tighten filters before allowing apply.

### Redacted log / trace evidence
```text
data.aws_vpc.selected: Reading...
data.aws_vpc.selected: Read complete after 1s [id=vpc-0c1dprodshared]

Outputs:
selected_vpc_id = "vpc-0c1dprodshared"

Expected:
staging VPC = vpc-09abstaging
```

### Fix
Tighten the data source filter, correct the provider context, and make environment selection explicit rather than implicit.

### Prevention
Use unique naming and tagging standards, and expose selected IDs in plans so reviewers can detect wrong-target reads before apply.

### Open-source references
- Terraform data sources: https://developer.hashicorp.com/terraform/language/data-sources
- Provider configuration: https://developer.hashicorp.com/terraform/language/providers/configuration

---

## 10) Apply partly succeeded, then failed, leaving production in an uncertain state

### Interview question
**Terraform apply created some resources, then failed halfway. How would you recover safely?**

### What I would say
A partial apply is a **reconciliation problem**. I would not immediately rerun apply blindly. First, I would identify what Terraform successfully created, what failed, whether state recorded everything correctly, and whether any partially created resources have side effects. In production, safe recovery depends on restoring a consistent view of reality before the next mutation.

### Failure boundary
Typical causes:
- provider/API error after partial creation
- quota exhaustion mid-run
- timeout on long-running operation
- network interruption between Terraform and backend/provider
- dependency created successfully, downstream attachment failed

### Troubleshooting steps
1. Read the apply output to find the last successful resource.
2. Compare Terraform state with live infrastructure.
3. Inspect whether partially created resources need import, cleanup, or retry.
4. Re-run plan before any further apply.
5. Decide whether the safest path is retry, targeted recovery, import, or manual cleanup followed by reconciliation.
6. Avoid `-target` unless the recovery plan is explicit and justified.

### Redacted log / trace evidence
```text
aws_iam_role.app: Creation complete after 2s [id=payments-prod-app-role]
aws_kms_key.app: Creation complete after 8s [id=1f2a...]
aws_db_instance.prod: Still creating... [10m elapsed]
Error: creating RDS instance: InvalidParameterCombination: Storage type io1 is not supported for engine version 15.3
```

### Fix
Reconcile state with reality first, then apply the smallest safe correction. That may mean importing a partially created resource, deleting a failed partial object, or adjusting the configuration before retrying.

### Prevention
Keep plans reviewable, avoid huge applies, test quotas and provider constraints in lower environments, and design modules so failures are easier to isolate and retry.

### Open-source references
- Terraform apply: https://developer.hashicorp.com/terraform/cli/commands/apply
- Terraform state commands: https://developer.hashicorp.com/terraform/cli/commands/state
- Targeting warning and graph implications: https://developer.hashicorp.com/terraform/cli/commands/plan#resource-targeting

---

# How to answer Terraform incidents in interviews

Use this sequence:

1. **State the symptom**
2. **Name the failure boundary**
3. **Give the first troubleshooting steps in order**
4. **Use one plan/apply/log/audit snippet as evidence**
5. **State the safest fix**
6. **End with prevention**

That answer shape sounds strong because it shows you understand **state, graph, provider behavior, identity, and production safety**, not just Terraform commands.
