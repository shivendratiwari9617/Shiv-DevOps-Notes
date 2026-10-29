# Senior Terraform Platform, DevOps, and SRE Interview Answers

## Core model and modules

### 1. How does Terraform construct a dependency graph, and how do resource references, explicit dependencies, and unknown values affect planning and apply order?

Terraform builds a graph from references, providers, and lifecycle relationships; `depends_on` adds edges when implicit references do not capture an ordering constraint. Unknown values defer some decisions to apply but do not inherently create ordering edges. Read a plan for replacements and unknown attributes, and add explicit dependencies only when required by a real side effect. Avoid unnecessary graph serialization because it slows changes. Validate resulting resource readiness, not just apply order.

### 2. How do configuration, state, provider behavior, and live infrastructure interact during refresh, plan, and apply?

Configuration defines desired resources; state tracks Terraform's recorded object addresses and provider IDs; the provider reads and mutates live infrastructure. A normal plan refreshes live attributes before comparing them with configuration and state, subject to provider behavior and API consistency. Apply executes the plan and updates state, but partial failures can leave uncertainty. Compare all three before making incident changes; a successful plan does not prove a service works for customers.

### 3. What lifecycle and addressing risks differ between `count` and `for_each`? How would you safely migrate an existing resource between addresses?

`count` indexes by position, so reordering or removing list elements can map existing addresses to different intended resources; `for_each` uses stable keys. Choose keys that represent durable business identity. Before migration, map old to new addresses and use `moved` blocks or `terraform state mv` with locking, then inspect plans in each environment for zero unintended replacements. Back up state and rehearse a representative migration. Do not equate no resource change with no state risk.

### 4. How would you design a reusable module that enforces necessary guardrails without hiding important operational choices from teams?

Expose decisions that affect reliability, data durability and cost, such as instance class and backup policy, while enforcing organization-required encryption, ownership and network controls. Provide clear defaults, validation and outputs with explicit sensitivity. Test upgrades and failure cases with representative teams and document who owns resources created by the module. Avoid a giant wrapper that prevents product teams from understanding replacement risk. Measure adoption and support effort, not just module downloads.

### 5. How would you version Terraform, providers, and modules across multiple teams while testing compatibility and planning upgrades?

Constrain Terraform and provider versions, commit the dependency lockfile, and publish versioned modules with a supported upgrade window. Test representative states and provider APIs against a candidate version, inspect real plans and stage low-risk environments first. Delay major upgrades that produce unexplained replacements and document downgrade or state-schema limitations. Track lagging consumers and give a compatible transition path. Reproducible planning matters more than having every team on the newest provider immediately.

### 6. A module change would replace a production resource. How would you identify why, evaluate alternatives, and stage a safe migration?

Inspect the `-/+` plan attribute, provider schema and module change to distinguish immutable-field replacement, address move and drift. Check service dependencies, data persistence, quota, endpoint changes and whether a moved block or phased new resource avoids destruction. Canary in an equivalent environment, plan against current production state, back up data and obtain explicit owner approval. Do not apply a replacement under a routine upgrade label. Verify customer paths and rollback feasibility after cutover.

## State and collaboration

### 7. How would you choose state boundaries across environments, accounts, shared infrastructure, and application teams to limit blast radius and coupling?

Use separate state and credentials for production versus lower environments and for independently owned resources; split very large states when change contention and blast radius justify it. Shared network and application resources should have clear interfaces rather than co-owning one address. Balance too many state dependencies against one state that locks every team. Model access, plan time, recovery and cross-state contract changes. Test an application team's identity cannot destroy shared production infrastructure.

### 8. A state lock remains after an interrupted deployment. What evidence must you gather before deciding whether to force-unlock, and how would you prevent concurrent changes?

Identify lock ID, holder, timestamp, runner process, backend lease and cloud API activity; lost connectivity does not prove the apply stopped. Contact the runner owner and stop any still-active operation, then compare state and live resources before a controlled force-unlock. Only one authorized operator should resume changes. Preserve state version and plan evidence. Prevent recurrence with a locked remote backend, runner cancellation protocol and a tested recovery runbook.

### 9. How would you secure, back up, and restore remote state? How would you verify that restored state corresponds to live infrastructure?

Use encrypted, access-scoped, versioned remote storage with locking, audit and protected backups, remembering state can hold secret values. Restore into an isolated recovery workflow first, compare lineage/serial and resource IDs with live infrastructure, and run a refreshed plan before any apply. A restored older snapshot can omit resources created since its capture. Reconcile imports or removals deliberately and ensure no concurrent runner. Measure recovery time to a safe, usable deployment, not just state download.

### 10. What risks arise from exposing outputs through remote state, and what alternatives would you consider for cross-team contracts?

Access to remote state often exposes more than the chosen output, including sensitive resource attributes. Prefer a scoped service-discovery API, parameter store or published versioned output with restricted consumers when teams need only a few values. Establish owner, contract version, freshness and rotation rules. If remote state is necessary, grant the narrowest backend access and audit it. Test that application teams cannot read production secrets or mutate shared network state.

### 11. How would you compare workspaces, separate state files, and separate repositories for production environment isolation and access control?

Workspaces select different states within the same configuration and are convenient for similar environments, but do not inherently provide separate credentials or review boundaries. Separate backends/state and identities make production isolation explicit; repos may separate ownership but can add duplication. Choose based on risk, configuration divergence, team access and deployment independence. Test an operator cannot select production accidentally and that a nonproduction change cannot reach production APIs.

### 12. An apply partially succeeds before failing. How would you determine what changed, preserve service, and reconcile configuration, state, and live resources?

Pause retries and preserve run logs, state version and provider operation IDs. Inspect each changed resource in both state and cloud API; an API timeout may have created an object even if the runner recorded failure. Protect customer traffic and data first, then refresh and plan, importing or correcting state only with verified IDs and lock ownership. Make a narrow reconciled apply and check actual service health. Blind re-apply can duplicate or destroy resources.

## Change management and automation

### 13. An emergency console change resolved an incident but created drift. How would you reconcile the fix with configuration and decide whether to retain or revert it?

Record the exact resource, actor, time, reason and customer impact, then refresh a plan to identify all drift. If the change is the safe desired state, codify it and run a reviewed apply that preserves it; otherwise schedule a rollback with the incident owner and customer health gate. Do not automatically revert a mitigation during an active outage. Prevent competing controllers and document the emergency change path for future audits.

### 14. How do `create_before_destroy`, `prevent_destroy`, and `ignore_changes` affect operational safety? When can they conceal risk?

`create_before_destroy` reduces cutover gaps only if quotas, unique names and dependency behavior permit coexistence. `prevent_destroy` blocks a planned deletion but is not a data backup; removing the guard can still destroy data. `ignore_changes` suppresses diffs for specified fields, including harmful drift such as missing required tags. Use each with an owner, documented exception and a separate drift check. Test a representative replacement and restore rather than trusting lifecycle flags alone.

### 15. How would you import existing infrastructure and refactor resource addresses without causing unintended replacements?

Inventory live IDs and ownership, write matching configuration, and import each resource to its correct address under a state lock. Use `moved` blocks for later address refactors and compare refreshed plans for no unintended creates or destroys. Back up state first and test a nonproduction instance or state copy. Check sensitive attributes and provider normalization that may cause changes after import. Only then enable automated apply on the resource.

### 16. How would you design a plan-and-apply pipeline with scoped identities, policy checks, approvals, and protection against applying a stale or unreviewed plan?

Run plan with pinned Terraform/provider versions and read-scoped identity where feasible, validate policy and destructive changes, and attach source revision, state version and artifact hash to the reviewed saved plan. Apply that exact plan in a single locked workflow with appropriately scoped short-lived identity; if state or source changed, replan and seek review again. Keep approvals tied to the actual artifact. Verify deployment and customer health afterward; a plan approval is not a runtime health gate.

### 17. An external controller changes a field Terraform also manages. How would you establish ownership and avoid repeated or destructive changes?

Observe live changes, Terraform diffs and controller events to determine who writes the field and whether its value is computed or business-critical. Assign one owner: move it out of Terraform, constrain the controller, or use narrow `ignore_changes` with an independent policy check if the controller legitimately owns it. Avoid broad ignores that hide security drift. Test restart and re-apply cycles for convergence and ensure neither system repeatedly resets customer-affecting settings.

### 18. How would you coordinate changes when one state depends on outputs from another state but the teams deploy independently?

Publish a versioned, minimal output contract with owner, compatibility window and access control. Change producers first with backward-compatible values, validate consumer plans, then migrate consumers before removing old outputs. Serialize only the critical transition, not every independent team deployment. Avoid unrestricted remote-state access if a scoped parameter or service API suffices. Test an upstream rollback and stale consumer value so the dependency cannot silently break customer traffic.

### 19. A plan becomes slow after a small change. How would you investigate provider calls, refresh behavior, graph structure, state size, and external API limits?

Measure time spent on backend locking, refresh, provider reads and graph execution using sanitized debug traces and provider API metrics. Check state/resource growth, excessive `depends_on`, rate limits, retries and newly slow endpoints. Compare the same state and version with a prior run without disabling refresh as a permanent workaround. Split state or tune provider/API pressure only after locating the bottleneck. Track p95 plan time and correctness across teams after change.

### 20. How would you roll out a breaking module change across many teams and verify that no critical environments are left behind?

Inventory module versions and resource owners, publish a versioned migration guide and compatible adapter where possible, and test plans on representative states. Canary lower-risk consumers, then schedule critical environments with owners and rollback requirements; do not silently repoint versions. Track adoption by environment, unexplained replacements, apply failures and exception expiry. Keep old versions supported long enough for urgent fixes while removing insecure variants on a defined schedule.

## Security, reliability, and platform design

### 21. How would you manage cloud credentials and sensitive values in Terraform workflows? What risks remain in plans, state, logs, and outputs?

Use short-lived scoped workload identity, isolate production execution and restrict backend and plan artifact access. Mark variables/outputs sensitive to reduce display, but understand raw state and saved plans can still contain secrets; encrypt, audit and set retention accordingly. Avoid printing provider debug logs or passing secrets in command arguments. Prefer secret references over embedding secret values where supported. Test a low-privilege team cannot retrieve another team's secret from state.

### 22. How would you implement multi-account or multi-region provider configuration without creating accidental privilege escalation or ambiguous ownership?

Declare explicit provider aliases with account/region and separate short-lived roles, then pass each alias deliberately into modules. Guard against implicit default-provider use, verify actual account identity in CI, and segregate production permissions from nonproduction. Review state ownership and cross-region dependencies, particularly during failover. Test a misconfigured role cannot plan or apply to the wrong account. Labels in a module do not enforce cloud permission boundaries.

### 23. What controls would you apply to reduce accidental destructive changes, and how would you handle an authorized emergency change?

Use protected state and locks, scoped identities, saved reviewed plans, policy gates for critical resource deletion and data backups with tested restore. `prevent_destroy` can be a guard, not a substitute for review or recovery. Emergency changes require explicit incident owner approval, bounded scope, audit and follow-up drift reconciliation. Stop an unexpected replacement first and verify live impact before proceeding. Measure destructive-plan escapes and mean time to safe emergency mitigation.

### 24. How would you compare local execution with a managed remote-execution platform for access control, state, policy, availability, and operational responsibility?

Local runs offer flexibility but require careful control of credentials, versions, logs and concurrent execution; remote platforms centralize locking, policy, approvals and audit but introduce availability and vendor dependencies. Test protected production isolation, state restore, provider connectivity and emergency access in each model. Include platform cost and on-call responsibility. Do not move to remote execution assuming it eliminates accidental destructive plans; reviewers still need context and real health gates.

### 25. A backend outage prevents production deployment during an incident. How would you assess safe mitigation and avoid corrupting or diverging state?

Identify whether a locked run is still active and whether customer recovery can use a documented manual change or independent deployment path. Do not copy stale state locally and apply concurrently; that risks divergent histories and duplicate resources. For an approved break-glass action, record live IDs and intended state, restrict other runs, and reconcile drift once the backend recovers. Verify customer recovery and state consistency before reopening normal delivery. Rehearse this failure path.

### 26. How would you test whether infrastructure can be recreated after a regional failure, including data, identity, network dependencies, and state recovery?

Restore protected state or reconstruct desired infrastructure in an isolated recovery region with independent credentials, DNS, network, secrets and artifact access. Ensure data restore or replication is application-consistent and the old writer is fenced before cutover. Execute a real product transaction and measure RPO from durable business events and RTO from usable service. Test failback and account for provider quotas and region-specific resources. `terraform apply` success alone is not disaster recovery.

### 27. How would you use Terraform to enable cost allocation and guardrails without claiming that configuration alone proves actual cost or savings?

Require accountable team/service tags, allowed instance families and budgets with approved exceptions, and validate planned changes before apply. Join resource inventory to billing and actual usage after deployment, including shared network and storage costs. `ignore_changes` on tags may hide removal of required allocation labels. Measure cost per customer transaction alongside reliability and headroom. Attribute observed savings only after comparing real bills and service performance.

### 28. How would you manage Kubernetes resources with Terraform when controllers and admission systems may mutate those resources?

Use Terraform for stable cluster primitives and resources with clear lifecycle ownership; leave high-churn workload fields to a Kubernetes-native reconciler when appropriate. Inspect provider refresh and admission mutations before using narrow `ignore_changes` for controller-owned fields. Avoid two controllers repeatedly writing the same object or field. Test plans after a deployment and policy mutation to ensure no accidental replacement or traffic reset. Document which system is authoritative per resource.

### 29. Tell me about a Terraform platform decision where standardization conflicted with team autonomy. How did you handle exceptions and measure outcomes?

Use an actual decision, naming product-team constraints, the guardrail you needed, pilot feedback and why any exception was time-bound. Quantify adoption, unsafe plans avoided, deployment lead time and support burden rather than asserting everyone agreed. Describe your own contribution and where the standard needed revision. Do not invent personal experience. Show a process teams could use during an urgent customer incident without bypassing all controls.

### 30. During a production incident, a plan proposes deleting shared infrastructure unexpectedly. What would you do in the first 15 minutes, and what evidence would be required before proceeding?

Stop apply, secure the state lock and notify incident and shared-resource owners. Preserve the plan, source revision, state version and live resource status; inspect whether a module/address refactor, drift, provider upgrade or wrong workspace caused deletion. Protect current customer traffic and use an approved manual mitigation if necessary. Proceed only after a new reviewed plan proves intended changes and restore/failover risks are understood. A deadline is not evidence that deletion is safe.

## Evidence-based exercises

### 31. A plan proposes destroying and recreating three instances after a list used with `count` is reordered. How would you investigate resource addresses and plan a safe refactor without unintended replacement?

Compare old `count` indices, list order and actual instance IDs against the new desired mapping; an index refers to position, not stable identity. Stop the destructive apply and choose durable `for_each` keys. Add explicit `moved` mappings or locked `state mv` actions for each instance, then inspect refreshed plans for zero unintended replacements in every state. Back up state and verify host identities and customer traffic after the refactor.

### 32. A Terraform run reports a state lock, and the last known runner lost connectivity ten minutes ago. What evidence would you require to determine whether the run is still active, and how would you coordinate any recovery?

Read lock ID, holder and lease status, runner/job heartbeat, provider API activity and state serial before contacting the runner owner. A lost connection ten minutes ago does not prove the apply stopped; a cloud request may still be completing. Freeze competing runs, terminate the original run safely if necessary, then force-unlock only with an agreed operator and verified inactivity. Refresh and reconcile live changes before a new plan or apply.

### 33. A reviewed plan proposes no destructive changes, but a second team modifies the same infrastructure before apply. How would you evaluate whether the saved plan remains safe and prevent conflicting changes?

Stop and determine whether the second team's change affected the same state or live resources and whether the saved plan's state version is stale. Terraform should reject a saved plan when its state has changed, but out-of-band changes may require refresh and a new review. Use one locked apply workflow with ownership of shared resources, and invalidate approvals when source or state changes. Verify resulting live infrastructure and customer health after a newly reviewed plan.

### 34. An apply creates a cloud resource but fails before the team confirms that state was updated. How would you inspect both state and the provider, avoid creating a duplicate, and reconcile the configuration?

Pause all runners, confirm the previous apply is no longer active, and preserve its log, remote-state version/serial and lock ID. Under an exclusive lock, inspect the address with `terraform state show` or `terraform state list`, and independently query the cloud API for resource ID, owner tags and creation operation; an API timeout is not proof the create failed. If the object exists but is absent from state, use a reviewed import block or controlled `terraform import` with the verified ID, then refresh and inspect a new full plan. If state already records it, verify provider attributes and dependencies instead of importing twice. If no object exists, only then consider a new apply. Do not edit the state JSON or force-unlock an active runner; gate reconciliation on healthy customer traffic and preserve the resource ID in the release audit trail.

## Product-company practical exercises

### 35. A resource uses `lifecycle { ignore_changes = [tags] }`; a required cost-allocation tag is then removed manually, but the next plan shows no change. How would you investigate the ownership decision, restore accountability, and avoid disrupting other tag managers?

Confirm live tags, Terraform config, module policy and which external controller, if any, owns specific tags. `ignore_changes` masks the diff, so restore the required tag via its designated owner and add independent compliance detection. Narrow the ignore to controller-owned keys where supported by the provider/resource semantics, or establish a single tag owner if field-level ignores are too coarse. Test a plan after both systems reconcile and verify billing attribution without a tag tug-of-war.

### 36. A plan for a shared network includes `-/+` on a critical gateway, but the release deadline is in two hours. How would you determine whether the replacement is necessary, communicate risk, and decide whether to proceed?

Stop and inspect the exact force-replacement attribute, provider version, module change and live gateway ownership. Estimate failover path, cutover downtime, dependent teams, and rollback limitations; try a compatible configuration or address move if evidence supports it. Get shared-network and incident-owner approval for any unavoidable replacement with a tested recovery plan. Do not let the deadline override unknown customer blast radius. Verify dependent product transactions after the change.

### 37. A product platform serves 50 teams using different versions of the same Terraform module. How would you inventory adoption, introduce a breaking change safely, and measure reliability and migration progress?

Inventory each team's pinned version, state, critical resources and exceptions. Release a new major version with migration steps and compatibility bridge where feasible; run plans and canaries on representative states before scheduling high-risk environments. Keep urgent fixes possible on a supported previous version and require review for destructive diffs. Track adoption by environment, unexpected replacements, apply failure and customer SLOs. Retire old versions only after agreed support and exception expiry.

### 38. A provider upgrade changes plans across 20 production states, including replacements for shared resources. How would you isolate the provider behavior, prioritize affected states, stage safe validation and rollout, and prove that unrelated environments remain unchanged?

Pin the previous provider lockfile and stop broad applies; compare old and new plans on the same state/config to identify schema normalization, API refresh or real desired change. Prioritize shared-network and data-bearing replacements by blast radius; test a low-risk state and provider migration notes before any production upgrade. Roll out with saved reviewed plans and per-state locks, measuring actual resource IDs and customer SLIs. Audit plans of unaffected states for zero unexpected diff before claiming isolation.