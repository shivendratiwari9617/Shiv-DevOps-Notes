# Terraform Production Issues and Troubleshooting Guide
**Runbook-grade | Product-company focused | Safe production investigation first**

## Principles for Troubleshooting Terraform in Production

Terraform incidents in production are rarely just "Terraform problems." They usually sit at the boundary between:

- infrastructure state
- CI/CD identity
- cloud control plane behavior
- module or provider lifecycle
- platform ownership gaps
- emergency manual changes made under pressure

The goal during a Terraform incident is not to prove Terraform is right. The goal is to:

1. **restore safe delivery**
2. **minimize blast radius**
3. **preserve state integrity**
4. **capture evidence before mutation**
5. **separate investigation from remediation**
6. **avoid making a bad situation worse with rushed state surgery**

In a strong product company, Terraform is part of a larger delivery system. Treat every production issue as both:

- an immediate operational incident
- a signal about platform design, ownership, or governance debt

---

# Rapid Triage Checklist

Before changing anything, answer these questions:

1. **Is production traffic impacted, or is delivery only blocked?**
2. **What changed most recently?**
   - Terraform code
   - provider version
   - module version
   - CI identity or secrets
   - manual console change
   - cloud-side incident
3. **Which environments are affected?**
   - one workspace/state
   - one account/subscription/project
   - one region
   - all production
4. **Is the issue in plan, apply, drift detection, or post-apply behavior?**
5. **Is remote state healthy and reachable?**
6. **Is there an active lock?**
7. **Did a partial apply already mutate infrastructure?**
8. **Who owns the failing layer?**
   - platform
   - security
   - networking
   - app team
   - SRE
9. **Is there an approved rollback or mitigation path?**
10. **Are you inside a production change window or emergency incident process?**

If any answer is unclear, slow down before remediation.

---

# When Not to Touch State

Do **not** mutate Terraform state quickly just because the pipeline is blocked.

Avoid direct state changes when:

- the root cause is still unknown
- cloud resources may have only partially changed
- another apply may still be running
- the provider is returning inconsistent or stale data
- a lock may be legitimate
- the issue may actually be IAM, backend access, or provider regression
- you do not yet know whether GitOps or app deployment systems will re-reconcile the same resources

State mutation is a **last-resort recovery tool**, not a first response.

---

# Evidence to Capture Before Remediation

Capture these before changing infra or state:

- failing pipeline URL and run ID
- exact Terraform version and provider versions
- workspace/environment/account/region
- plan/apply output logs
- current lock details
- recent PRs, module changes, and provider changes
- cloud audit trail around the failing resources
- screenshot or exported details of manual console changes if any
- current drift report
- list of resources potentially changed in the failed run

This evidence is often more valuable than the first attempted fix.

---

# 1) Remote State Backend Unreachable or Corrupted Access Path

## Severity / Blast Radius
**SEV-2 or SEV-1** if production applies and drift checks across multiple stacks are blocked.

## Typical Symptoms

- `terraform init` fails
- `Error loading state`
- backend authentication or KMS decrypt errors
- all plans fail across one environment or multiple repos
- drift detection jobs suddenly fail at the same time

## Likely Root Causes

- backend bucket or storage policy changed
- KMS key permission regression
- storage endpoint/network path issue
- CI identity no longer allowed to read backend
- state bucket/table renamed or replaced incorrectly
- backend configuration drift after bootstrap change

## Ownership

- **Primary:** Platform
- **Consulted:** Security
- **Consulted when network path is private:** Networking

## Step-by-Step Troubleshooting

1. Confirm whether the issue is isolated to one state or all states.
2. Check whether local engineers can reproduce with read-only credentials.
3. Verify backend reachability separately from Terraform.
4. Confirm the CI or runtime identity still has:
   - read/write state access as required
   - KMS decrypt permission
   - lock table access if applicable
5. Review the most recent backend/bootstrap Terraform changes.
6. Check cloud audit logs for denied API calls to:
   - state storage
   - KMS
   - lock table
7. Confirm bucket/table/key names and prefixes still match backend config.
8. If using private endpoints, confirm routing, endpoint policies, and DNS resolution.
9. Validate whether state objects still exist and prior versions are present.
10. Only after confirming backend health, rerun `terraform init`.

## Validation Commands

```bash
terraform version
terraform init -reconfigure

aws s3 ls s3://my-tf-state-bucket/path/
aws dynamodb describe-table --table-name tf-state-locks
aws kms describe-key --key-id <key-id>
aws sts get-caller-identity
```

## Immediate Mitigation

- stop all applies touching the affected backend
- switch incident coordination to platform owner
- restore broken IAM/KMS/backend policy path first
- do not copy state manually unless recovery is formally approved

## Unsafe Fixes to Avoid

- creating a new empty backend and pointing production at it
- manually downloading and editing state without evidence capture
- deleting backend policy statements blindly
- bypassing encryption controls to “get the pipeline moving”

## Permanent Fix / Prevention

- isolate backend bootstrap from regular app/platform stacks
- add backend access smoke checks in CI
- require approval for backend/KMS policy changes
- version and protect backend resources
- enable object versioning and access logging

## Escalation Guidance

Escalate immediately if:

- state objects appear missing
- KMS access is broken in production
- multiple production stacks are blocked
- anyone proposes manual state recreation without restore validation

---

# 2) Stuck State Lock Blocking Production Deployments

## Severity / Blast Radius
**SEV-3 to SEV-2**, depending on whether production rollout is blocked.

## Typical Symptoms

- `Error acquiring the state lock`
- apply jobs queue up behind one state
- no active deployment appears to be running
- teams cannot promote changes

## Likely Root Causes

- crashed apply left stale lock
- long-running apply still active
- concurrent pipelines against same state
- CI cancellation did not release lock
- backend lock table inconsistency

## Ownership

- **Primary:** Platform
- **Consulted:** SRE if production incident underway

## Step-by-Step Troubleshooting

1. Confirm whether an apply is genuinely still running.
2. Find the pipeline/job holding the lock.
3. Check whether the lock timestamp is recent or stale.
4. Verify whether cloud resources are still changing.
5. Inspect lock metadata for owner, operation, and path.
6. Confirm whether multiple pipelines target the same workspace/state.
7. If a real apply is active, do nothing until it finishes or is declared failed.
8. If the lock is stale, follow approved force-unlock procedure.
9. After unlock, run a **read-only** plan first before any apply.
10. Validate whether the previously interrupted apply partially changed resources.

## Validation Commands

```bash
terraform force-unlock -help
terraform plan -refresh-only

aws dynamodb scan --table-name tf-state-locks
aws sts get-caller-identity
```

## Immediate Mitigation

- pause duplicate pipelines
- allow only one incident handler to coordinate unlock
- perform unlock only with evidence the lock is stale
- run `plan -refresh-only` before standard apply

## Unsafe Fixes to Avoid

- force-unlocking while another apply is active
- rerunning many pipelines at once after unlock
- assuming no infra changed because apply failed

## Permanent Fix / Prevention

- serialize applies per state
- add pipeline cancellation cleanup
- enforce one workspace/state per deployment boundary
- alert on locks older than expected threshold

## Escalation Guidance

Escalate if:

- lock ownership is unclear
- resource mutations are still in progress
- partial apply touched shared production infrastructure

---

# 3) Drift After Emergency Hotfix or Manual Console Change

## Severity / Blast Radius
**SEV-2 or SEV-3**, but can become **SEV-1** if security, networking, or routing drift affects customers.

## Typical Symptoms

- drift detected in nightly plans
- production apply wants to revert emergency change
- Terraform proposes deletion/replacement of manually changed resources
- security settings differ from code

## Likely Root Causes

- emergency console hotfix
- untracked manual IAM or networking change
- GitOps/runtime reconciler modified overlapping resources
- incident response bypassed Terraform workflow

## Ownership

- **Primary:** Platform
- **Consulted:** Security / Networking / App team depending on changed asset
- **Consulted:** SRE if hotfix was incident-driven

## Step-by-Step Troubleshooting

1. Identify exactly which resources drifted.
2. Determine whether the drift was:
   - emergency
   - unauthorized
   - expected operational variance
3. Check cloud audit logs for who changed what.
4. Confirm whether the manual change is still needed for service safety.
5. Compare current live config to desired code.
6. Decide whether source of truth should become:
   - Terraform code
   - approved exception record
   - temporary operational allowlist
7. Run `terraform plan` and review replacement risk carefully.
8. If production stability depends on the manual change, do **not** blindly apply.
9. Create reconciliation PR.
10. Apply only after code matches intended steady state.

## Validation Commands

```bash
terraform plan
terraform show
aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceName,AttributeValue=<resource>
```

## Immediate Mitigation

- freeze conflicting applies
- preserve the hotfix if it is protecting production
- document temporary exception with owner and expiry
- reconcile into code before next standard production release

## Unsafe Fixes to Avoid

- applying immediately just to remove drift noise
- editing state to pretend drift is gone
- assuming the manual change was harmless

## Permanent Fix / Prevention

- formal emergency-change workflow with PR reconciliation SLA
- nightly drift detection with owner assignment
- clear Terraform vs GitOps ownership boundaries
- incident templates that require post-incident infra reconciliation

## Escalation Guidance

Escalate if:

- security controls drifted
- public exposure or routing changed
- there is disagreement on intended steady state
- replacement risk is unclear for stateful/shared resources

---

# 4) CI/CD OIDC or Federated Identity Failure

## Severity / Blast Radius
**SEV-2** when deploys are blocked across environments; **SEV-1** if emergency production fix cannot be deployed.

## Typical Symptoms

- CI cannot assume cloud role
- token audience or subject mismatch errors
- STS/WebIdentity failure
- pipeline worked yesterday and now all plans fail

## Likely Root Causes

- trust policy changed
- OIDC provider thumbprint/config rotated
- repo/branch/environment claim mismatch
- CI workflow identity moved to different path
- role session conditions too restrictive

## Ownership

- **Primary:** Platform
- **Consulted:** Security

## Step-by-Step Troubleshooting

1. confirm the failure is identity-related, not backend-related
2. inspect the exact assume-role error
3. compare token claims expected by trust policy:
   - audience
   - subject
   - repository
   - branch/environment
4. review the latest IAM trust policy changes
5. confirm the CI workflow still runs from approved repo/ref/environment
6. verify OIDC provider configuration and certificate/thumbprint if applicable
7. test assumption with a minimal diagnostic workflow
8. confirm the target role still exists and policy boundaries allow required actions
9. restore trust policy or claims alignment
10. rerun plan before apply

## Validation Commands

```bash
aws sts get-caller-identity
aws iam get-role --role-name <role-name>
aws iam get-open-id-connect-provider --open-id-connect-provider-arn <provider-arn>
```

## Immediate Mitigation

- restore last known good trust configuration
- use approved emergency deploy path if production mitigation is urgent
- avoid switching to long-lived static credentials unless formally authorized

## Unsafe Fixes to Avoid

- putting permanent cloud keys into CI secrets as a shortcut
- broadening trust policy to all repos or all branches
- using admin role assumptions temporarily without audit controls

## Permanent Fix / Prevention

- contract-test trust policies in CI
- version-control OIDC provider and IAM role trust configs
- document claim expectations per environment
- alert on trust policy drift

## Escalation Guidance

Escalate if:

- security approves emergency access only
- production break/fix is blocked
- trust model appears broadly over-permissive or broken across multiple repos

---

# 5) Provider Version Mismatch or Provider Regression

## Severity / Blast Radius
**SEV-3 to SEV-2**, but may escalate if production replacements are proposed.

## Typical Symptoms

- plan suddenly changes many unrelated resources
- provider schema errors
- unsupported attribute or deprecation failure
- resource replacement proposed after provider upgrade
- plan differs between local and CI

## Likely Root Causes

- provider version drift between local and CI
- lock file not committed or ignored
- provider bug/regression
- changed default behavior in new provider version
- mixed Terraform versions across runners

## Ownership

- **Primary:** Platform
- **Consulted:** Affected product team if service-specific impact exists

## Step-by-Step Troubleshooting

1. compare Terraform CLI versions across local and CI
2. compare provider lock file and resolved versions
3. identify exactly when the version changed
4. reproduce the failing plan in a controlled non-production environment
5. isolate which resources changed due to provider behavior
6. read provider release notes for breaking/default behavior changes
7. test rollback to last known good provider version
8. verify whether any state migration already happened
9. if regression confirmed, pin provider and halt rollout
10. rerun plan in stage before resuming production

## Validation Commands

```bash
terraform version
terraform providers
cat .terraform.lock.hcl
terraform init -upgrade
```

## Immediate Mitigation

- pin last known good provider version
- stop production applies until replacement risk is understood
- require targeted review for resources showing widespread drift

## Unsafe Fixes to Avoid

- `terraform init -upgrade` directly in production during incident response
- applying large unexplained plan changes
- allowing local and CI versions to drift further

## Permanent Fix / Prevention

- commit provider lock files
- maintain monthly provider upgrade review
- test upgrades in sandbox and stage
- keep release notes for platform-wide provider bumps

## Escalation Guidance

Escalate if:

- provider regression affects shared modules or many stacks
- replacements touch databases, clusters, or public routing resources
- rollback path is unclear because state migration already occurred

---

# 6) Partial Apply Left Infrastructure in Unknown State

## Severity / Blast Radius
**SEV-2**, sometimes **SEV-1** if shared production infrastructure was modified.

## Typical Symptoms

- apply failed midway
- some resources exist, others do not
- Terraform wants to recreate or delete unexpectedly on rerun
- product service is degraded after partially completed change

## Likely Root Causes

- API throttling or provider timeout
- IAM denial mid-run
- dependency issue in apply graph
- manual cancellation
- cloud-side transient failure

## Ownership

- **Primary:** Platform
- **Consulted:** App team / SRE depending on impacted service

## Step-by-Step Troubleshooting

1. stop automatic retries
2. capture exact failing resource and last successful operations
3. inspect live infrastructure for partially created or modified resources
4. compare state to live resources
5. determine whether the failed apply affected:
   - stateless resources only
   - shared infra
   - stateful/data-bearing resources
6. run `terraform plan -refresh-only`
7. import missing but valid resources if formally approved and necessary
8. if replacements are proposed, assess service impact before rerun
9. restore consistency between code, state, and real infra
10. only then rerun apply

## Validation Commands

```bash
terraform plan -refresh-only
terraform state list
terraform state show <resource>
terraform import <resource> <id>
```

## Immediate Mitigation

- freeze further deploys to the same state
- restore service manually only if required for production safety and through incident process
- keep one incident owner coordinating reconciliation

## Unsafe Fixes to Avoid

- rerunning apply repeatedly without checking live state
- deleting half-created production resources by hand
- importing resources without verifying exact identity and configuration

## Permanent Fix / Prevention

- better retry/backoff and throttling handling
- smaller apply blast radius through state separation
- pre-apply IAM validation
- explicit dependency modeling
- post-failure reconciliation runbook

## Escalation Guidance

Escalate if:

- databases, KMS keys, routing, or clusters were partially mutated
- stateful resources may be duplicated or replaced
- production traffic is impacted and reconciliation plan is unclear

---

# 7) Hidden Dependency or Graph Ordering Problem

## Severity / Blast Radius
**SEV-3 to SEV-2** depending on whether apply repeatedly fails in production.

## Typical Symptoms

- intermittent apply failures
- resource creation order seems wrong
- IAM role/policy not ready when dependent resource is created
- DNS, networking, or Kubernetes objects fail until rerun

## Likely Root Causes

- implicit dependency not captured
- data source evaluated before resource is actually ready
- cloud eventual consistency
- module output consumed too early
- external system dependency not modeled

## Ownership

- **Primary:** Platform
- **Consulted:** Networking or Security if dependency involves those layers

## Step-by-Step Troubleshooting

1. identify the first failing resource, not just the last error shown
2. inspect whether dependency is explicit or only assumed
3. check whether the failure resolves on rerun
4. review if the cloud provider has eventual consistency delay in this path
5. inspect module boundaries and outputs
6. confirm whether data sources are reading not-yet-ready resources
7. add explicit `depends_on` only where justified
8. break layers apart if hidden cross-layer coupling exists
9. validate in stage with clean apply
10. document the dependency in module contract

## Validation Commands

```bash
terraform graph
terraform plan
terraform apply -target=<resource>   # only for controlled diagnosis, not normal steady state
```

## Immediate Mitigation

- if safe, stage the change in smaller approved layers
- isolate affected resources
- delay or rerun only after understanding dependency issue

## Unsafe Fixes to Avoid

- normalizing `-target` as production workflow
- piling on arbitrary `depends_on` everywhere
- accepting flaky reruns as “good enough”

## Permanent Fix / Prevention

- clearer layer separation
- module contract tests
- explicit outputs and readiness assumptions
- fewer cross-cutting modules that hide lifecycle order

## Escalation Guidance

Escalate if:

- the dependency involves shared networking, IAM, or external platform control planes
- repeat failures indicate structural module design issue affecting many teams

---

# 8) Module Upgrade Regression Broke Shared Platform Behavior

## Severity / Blast Radius
**SEV-2** and potentially broad if many services consume the module.

## Typical Symptoms

- many stacks show unexpected changes after module bump
- new defaults alter security groups, tags, IAM, or routing
- services fail after rollout of supposedly minor change
- downstream teams lose trust in platform releases

## Likely Root Causes

- breaking behavior shipped in minor version
- undocumented default change
- missing compatibility tests
- upgrade path not exercised in stage
- module consumer assumptions were not preserved

## Ownership

- **Primary:** Platform
- **Consulted:** Affected product teams
- **Consulted:** Security if control posture changed

## Step-by-Step Troubleshooting

1. identify exact module version delta
2. compare changelog, release notes, and code diff
3. determine whether interface break or behavior break occurred
4. isolate affected resources and teams
5. test rollback to previous module version
6. validate whether any irreversible state/resource migration occurred
7. if rollback is safe, revert and republish pinned version guidance
8. if rollback is unsafe, produce migration plan and controlled remediation
9. freeze further adoption of broken module release
10. capture missing tests and compatibility guarantees

## Validation Commands

```bash
git diff <old-tag> <new-tag>
terraform plan
terraform providers
```

## Immediate Mitigation

- stop further rollouts of the module
- pin consumers to last known good version
- open platform incident if shared production behavior changed

## Unsafe Fixes to Avoid

- force-upgrading all consumers “for consistency”
- releasing hotfix without compatibility validation
- silently editing module outputs used by many teams

## Permanent Fix / Prevention

- semantic versioning discipline
- required changelog and migration notes
- contract and integration tests
- canary adoption by internal platform-owned services first
- quarterly review of deprecated module usage

## Escalation Guidance

Escalate if:

- many production services depend on the module
- rollback may break already-migrated consumers
- security or network posture changed broadly

---

# 9) EKS / GitOps / Terraform Ownership Mismatch

## Severity / Blast Radius
**SEV-2**, often high-noise and high-risk in Kubernetes platforms.

## Typical Symptoms

- Terraform plan keeps changing Kubernetes-related resources
- GitOps reverts Terraform-managed changes or vice versa
- namespaces/add-ons/ingress objects oscillate
- production deploys become unpredictable

## Likely Root Causes

- overlapping ownership between Terraform and GitOps
- cluster add-ons not clearly split between bootstrap and reconciliation
- manual kubectl changes during incident
- Helm/chart values changed outside agreed path

## Ownership

- **Primary:** Platform
- **Consulted:** SRE
- **Consulted:** App team if workload manifests involved

## Step-by-Step Troubleshooting

1. identify the exact resource being fought over
2. determine current source of truth:
   - Terraform
   - GitOps
   - Helm release process
   - manual kubectl change
3. inspect GitOps sync history and Terraform plan history
4. review whether the resource should be bootstrap-only or continuously reconciled
5. stop conflicting applies/syncs if they threaten production stability
6. preserve the configuration currently required for service health
7. move ownership to one control plane only
8. update documentation and module boundaries
9. reconcile state/code to the chosen source of truth
10. re-enable normal automation carefully

## Validation Commands

```bash
terraform plan
kubectl get <resource> -A -o yaml
kubectl describe <resource> <name> -n <namespace>
```

## Immediate Mitigation

- pause conflicting automation if safe and approved
- keep the live config that preserves availability
- avoid simultaneous Terraform apply and forced GitOps sync

## Unsafe Fixes to Avoid

- letting both systems continue fighting
- importing volatile Kubernetes objects into Terraform casually
- manual kubectl patching without reconciliation plan

## Permanent Fix / Prevention

- strict boundary: Terraform bootstraps infra, GitOps reconciles cluster objects
- documented ownership matrix
- incident workflow for temporary kubectl changes with reconciliation SLA
- module contracts that do not overlap with GitOps scope

## Escalation Guidance

Escalate if:

- ingress, DNS, certs, or policy controllers are unstable
- multiple teams disagree on source of truth
- cluster-level add-ons are involved in production outage

---

# 10) Secrets or Sensitive Values Leaking Through State, Plan, or CI Logs

## Severity / Blast Radius
**SEV-1** if production secrets are exposed.

## Typical Symptoms

- secret appears in state file
- plan output reveals sensitive values
- CI log contains tokens or credentials
- engineers paste plan output into chat/tickets with secrets visible

## Likely Root Causes

- secrets passed directly as Terraform variables
- sensitive output not marked appropriately
- provider/resource stores secret in state by design
- CI logs too verbose
- remote state access too broad

## Ownership

- **Primary:** Platform
- **Accountable:** Security
- **Consulted:** App team if application secret rotated
- **Consulted:** SRE if live service rotation needed

## Step-by-Step Troubleshooting

1. treat as a security incident immediately
2. identify exactly what secret or sensitive value leaked
3. determine exposure scope:
   - state backend
   - CI logs
   - PR comments
   - chat/ticket systems
4. restrict access to affected artifacts immediately
5. rotate exposed credentials/secrets according to severity
6. review whether state history/object versions also contain the secret
7. assess whether state readers need emergency access reduction
8. remove secret from code path and migrate to secret-reference pattern
9. clean up logs/artifacts where retention policy allows
10. document incident and prevention changes

## Validation Commands

```bash
terraform output
terraform show
aws s3api get-bucket-versioning --bucket my-tf-state-bucket
aws sts get-caller-identity
```

## Immediate Mitigation

- revoke/rotate leaked secret
- restrict backend/log access
- stop sharing affected logs or plan output
- engage security incident process

## Unsafe Fixes to Avoid

- assuming `sensitive = true` means secret is absent from state
- leaving leaked credentials active while discussing cleanup
- deleting evidence before security review

## Permanent Fix / Prevention

- avoid passing raw secrets into Terraform when possible
- use secret references and external secret managers
- reduce plan/log verbosity exposure
- restrict backend access sharply
- review provider behavior for sensitive fields before adoption

## Escalation Guidance

Escalate immediately if:

- production credentials, tokens, signing keys, or database secrets leaked
- state backend exposure scope is unclear
- leaked values may already have been used externally

---

# Patterns Behind Repeated Terraform Incidents

Repeated Terraform incidents usually come from a small set of systemic problems:

- unclear ownership between Terraform, GitOps, and app delivery
- backend and state treated as implementation detail instead of critical platform asset
- weak CI identity governance
- module changes released without compatibility testing
- providers upgraded without staged rollout
- emergency manual changes not reconciled back into code
- oversized states with large blast radius
- no explicit drift ownership
- platform team operating through tickets instead of self-service contracts

If the same incident class happens twice, treat it as a platform design problem, not just operator error.

---

# Operational Habits That Reduce Terraform Pages

These habits reduce both incident count and incident severity:

- one owner per state and clear state boundaries
- serialized applies for production
- nightly drift checks with owner assignment
- monthly provider and module upgrade review
- quarterly access review
- emergency-change reconciliation SLA
- semantic versioning and migration notes for modules
- read-only validation before mutation after failed apply
- clear production approval model
- evidence capture before remediation

---

# Signals Your Terraform Platform Needs Rework

Your Terraform platform likely needs architectural rework if:

- production drift is normal and mostly ignored
- engineers use cloud consoles for routine changes
- shared states block many teams at once
- provider upgrades are feared because rollback is unreliable
- module releases repeatedly surprise consumers
- GitOps and Terraform overlap on the same resources
- state surgery is a common operational skill
- CI relies on broad static credentials
- platform incidents recur without module, policy, or workflow changes
- delivery speed depends on manual platform intervention rather than paved roads

---

# Closing Note

A strong product company does not judge Terraform maturity by whether code exists. It judges it by whether production changes are **safe, observable, recoverable, and governable**.

The best Terraform troubleshooting culture is one where incidents become rarer over time because each one improves:

- platform boundaries
- module quality
- CI identity controls
- release discipline
- drift management
- operational ownership
