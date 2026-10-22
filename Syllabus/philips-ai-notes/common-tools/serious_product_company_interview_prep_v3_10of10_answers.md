# Answer Key — Serious Product-Based Company Interview Preparation v3

This file answers the prompts from `serious_product_company_interview_prep_v3_10of10.md` from the perspective of a product-based company platform team.

## Answering Standard

- explicit blast-radius boundaries
- least-privilege identity
- state and environment separation
- rollout safety before speed
- observable failure domains
- rollback and recovery clarity

## Source Base Used

The answers were aligned to official documentation and vendor best-practice guides for AWS, GCP, Azure, Terraform, Kubernetes, Argo CD, Helm, Docker, GitHub Actions, GitLab CI, Jenkins, OpenTelemetry, Prometheus, Redis, Kafka, and PostgreSQL. Where the question asked for design judgment, the answer adds product-company operating patterns on top of the official control model.

## AWS
### Architecture 1 — Production-grade multi-account AWS platform

- **Structure:** Use AWS Organizations with separate management, security, log-archive, shared-services, platform, and workload accounts. Keep prod isolated from non-prod, and separate cluster accounts from workload/data accounts when EKS is shared.
- **Guardrails:** Apply SCPs to deny dangerous actions globally: disabling CloudTrail, deleting centralized logs, creating IAM users, leaving regions ungoverned, or bypassing KMS and tagging policy. Use Config + Security Hub + GuardDuty centrally.
- **Networking:** Use a hub-and-spoke model. Shared ingress/egress, inspection, and DNS live in platform or network accounts; workload accounts own only local subnets and security groups. Default deny east-west traffic except explicitly approved service paths.
- **Identity:** Federate humans through IAM Identity Center. No standing admin. Use short-lived role assumption with permission sets, MFA, device posture, and break-glass isolation.
- **CI/CD:** Pipelines run in isolated runner accounts, assume narrowly scoped deploy roles per environment, and cannot pivot laterally. Prod deploy role is separate from non-prod and protected by approval plus environment conditions.
- **Logging and DR:** Centralize CloudTrail, VPC Flow Logs, ALB logs, EKS control-plane logs, and application telemetry in a log archive account. Define DR per dependency: stateless compute rebuild, stateful services with backup/replication, regional DNS failover, and tested restore paths.

### Architecture 2 — AWS identity without broad admin roles

- **Humans:** Developers get read-only plus scoped power-user roles in dev/test accounts. Platform engineers get platform-admin roles only in platform accounts. Security gets audit plus response roles. No direct long-lived IAM users.
- **Pipelines:** Use OIDC or role-assumption entry roles from CI. The identity chain is: workload identity in CI -> broker role in tooling account -> target deploy role in target account. Attach permission boundaries so even misconfigured roles cannot expand privilege.
- **Emergency access:** Break-glass uses isolated accounts, hardware MFA, strong logging, limited session duration, approval recording, and post-use credential rotation. It must not be the same path as daily admin.
- **Kubernetes/AWS split:** Use EKS access entries plus Kubernetes RBAC for cluster access, and IRSA or EKS Pod Identity for workload-to-AWS access. Never share node instance profile permissions with application pods.
- **Controls:** Deny wildcard `iam:*`, broad `sts:AssumeRole`, and global `kms:Decrypt`. Use resource tags and condition keys to force environment scoping.

### Architecture 3 — Secure EKS platform

- **Ingress:** Terminate external traffic through ALB or NLB plus ingress controller or Gateway API. Separate public and private ingress classes. Make ownership explicit so teams do not mutate shared edge controls.
- **Node isolation:** Use separate managed node groups or Karpenter provisioners by trust level, workload class, and environment. Use taints/tolerations and node selectors to keep privileged agents away from product workloads.
- **Secrets:** Store source of truth in Secrets Manager with IRSA/Pod Identity access. Sync into Kubernetes only where needed, prefer CSI or External Secrets patterns, and scope secret access to namespace/service account.
- **Observability:** Enable control-plane logs, node metrics, kube-state-metrics, app metrics, logs, traces, and ingress telemetry. Correlate request IDs across ALB, app, and downstream services.
- **Rollout safety:** Use readiness and startup probes, PDBs, surge/unavailable controls, canary or progressive delivery, rollback hooks, and freeze windows for platform changes.

### Architecture 4 — AWS DR with strict RTO/RPO

- **Compute:** Keep immutable images and reproducible Terraform for rebuild. Use multi-AZ by default; use pilot-light or warm-standby for regional failure depending on RTO.
- **Data:** Choose service-specific replication: Aurora replicas or global database where justified, S3 versioning plus CRR if needed, DynamoDB global tables only if write locality needs it, and backup restore tested separately from replica failover.
- **Identity and secrets:** Replicate or pre-stage IAM roles, KMS strategy, DNS, ACM certificates, and secrets. DR often fails because compute is recoverable but identity, secrets, or artifact access is not.
- **Delivery path:** CI, artifact registry, Terraform state, and cluster bootstrap assets must also survive primary-region loss. If delivery tooling is single-region, the app is not truly recoverable.
- **Runbook:** Document failover trigger, control-plane sequence, data consistency check, DNS cutover, customer communication, and failback criteria.

### Implementation 1 — Terraform structure for multi-account AWS

- `live/` per environment/account root module; `modules/` for reusable components; `policies/` for IAM JSON/templates; `bootstrap/` for backend and state prerequisites.
- One remote state object per account plus environment plus stack. Never put prod and non-prod in one state key.
- Provider aliases for each account; root modules map to exactly one deploy scope. Role assumption is configured in the provider, not ad hoc in scripts.
- Example split: `network/`, `identity/`, `eks-platform/`, `shared-services/`, `app-base/`. Do not couple networking, data, and app stacks in one apply.

### Implementation 2 — CI/CD IAM model

- **Entry:** CI identity can only assume a broker role from trusted repo/branch/environment claims.
- **Target roles:** Create separate deploy roles per env: `deployer-dev`, `deployer-stage`, `deployer-prod`. Prod requires protected branch/tag plus approval and cannot be assumed by non-prod workflows.
- **Policy shape:** Allow only required resource APIs and only for tagged resources in that environment. Split plan/read from apply/write if review process requires it.
- **Guardrails:** Deny role creation, boundary removal, state bucket deletion, KMS key admin, and arbitrary pass-role. Restrict `iam:PassRole` to explicit execution roles only.

### Implementation 3 — Secure EKS building blocks

- Private cluster endpoint or tightly restricted public endpoint, VPC CNI tuned for IP scaling, separate system/application node groups, ingress controller, external DNS, cert automation, cluster autoscaler or Karpenter, metrics/logging agents, admission policy, secret operator, image pull policy, and backup/restore tooling.
- Enable IRSA or Pod Identity, security groups for pods where needed, network policies, Pod Security admission or equivalent baseline/restricted profile, and namespace quotas/limits.

### Implementation 4 — Import manual AWS resources safely

- Freeze writes, inventory real resources, map each one to a target resource address, write minimal exact configuration with non-default arguments, then import into a disposable branch/state copy first.
- After import, run plan and eliminate replacements caused by address mistakes, provider defaults, computed fields, or missing lifecycle rules.
- Use `moved` blocks when refactoring addresses, and split onboarding into low-blast-radius batches. Never import and refactor structure in the same production change window.

### Debugging 1 — EKS intermittent timeouts with healthy nodes

- Debug in traffic order: client -> DNS -> ALB listener/rules/target health -> security groups/NACLs -> service/endpoints/EndpointSlice -> ingress annotations -> pod readiness -> CNI/IP exhaustion -> node conntrack/ephemeral ports -> downstream dependency.
- Confirm whether timeouts happen before reaching pods by comparing ALB target response codes/latency with app request logs. If ALB sees unhealthy targets, inspect readiness, health path, target type, and security group reachability first.
- Check EndpointSlice drift, pod churn, readiness flap under load, DNS TTL or weighted records, and whether pods are `Ready` but blocked by network policy or dependency stalls.

### Debugging 2 — Unexpected Terraform prod changes after import

- First check that imported addresses match the current module path and provider alias. A correct ID in the wrong address still causes replacement.
- Compare actual resource arguments versus provider defaults. Missing explicit non-default settings often create plan churn after import.
- Look for implicit renames, `for_each` key changes, module path changes, and attributes that should be `ignore_changes` only if an external controller truly owns them.

### Debugging 3 — Cross-account deploy fails after hardening

- Validate the STS path end to end: source identity claims, trusted principal in target trust policy, conditions on branch/environment claims, session duration, external ID if used, and permission boundary interaction.
- Check whether SCPs, permission boundaries, or session policies now deny actions even though the inline role policy still allows them.
- Inspect CloudTrail for the `AssumeRole` failure reason before debugging Terraform or `kubectl`; identity denial is earlier in the path.

### Debugging 4 — Cost spike after rollout

- Separate spend into compute, storage, network egress, log ingestion, trace volume, and managed service calls. A rollout often changes payload size, retry rate, or metric cardinality rather than CPU.
- Check ALB/NAT egress, new verbose logs, high-cardinality labels, autoscaling overshoot, persistent volume growth, and synthetic or health-check amplification.

### Review 1 — One CI/CD role deploys everywhere

- Main risks: horizontal compromise, accidental prod mutation from non-prod path, no environment accountability, and impossible blast-radius containment.
- Redesign to per-environment roles, OIDC-claim binding to repo/branch/environment, explicit prod approval, separated state access, and auditable broker-to-target assumption chain.

### Review 2 — Weakly isolated EKS platform

- Split node pools by trust boundary, move secrets access to per-service-account identity, enforce namespace policy and network policy baselines, and centralize ingress ownership with reviewed shared annotations only.
- Remove wildcard cluster roles and shared external-secret access. Tenancy without network, identity, and scheduling isolation is only cosmetic.

### Cross-Tool 1 — Git -> CI/CD -> Terraform -> EKS -> latency rise

- Find the first failing control by building a timeline: merge SHA, pipeline run, image digest, Terraform plan/apply IDs, cluster rollout revision, ALB target changes, and latency onset.
- Diff config before and after impact. The first failure point is the earliest stage where intended state diverged from safe state and passed without being blocked: bad image, unsafe Terraform network change, ingress annotation change, probe drift, or autoscaling regression.

### Cross-Tool 2 — One governed AWS delivery model

- Protected Git branches produce reviewed changes; CI builds immutable artifacts and signs/scans them; Terraform plan is generated with policy checks and manual approval for prod; Kubernetes rollout uses canary/readiness/PDB controls; observability gates verify SLO and key golden signals before promotion.
- Every stage emits an audit artifact: commit SHA, image digest, plan file checksum, deployment revision, and rollout or health evidence.

## GCP
### Architecture 1 — Production-grade GCP platform

- Use organization -> folders -> projects as the hard boundary model. Put platform, security, shared services, and workloads in separate folders/projects. Shared VPC host projects own networks; service projects host GKE and workload resources.
- Apply Org Policies at org/folder to restrict risky services, external IPs, service-account key creation, allowed regions, and resource location. Centralize audit logs and Security Command Center findings.
- Use project-per-environment or project-per-service boundary for prod workloads. Prod and non-prod should not share projects or service accounts.

### Architecture 2 — Least-privilege GCP identity

- Humans authenticate through Cloud Identity or workforce federation and receive role bindings at folder/project scope only where needed. Prefer groups over user bindings.
- Workloads on GKE use Workload Identity Federation for GKE; pipelines use workload identity federation or service account impersonation instead of static keys.
- Separate deploy, runtime, and break-glass identities. Disable broad editor/owner grants in workload projects.

### Architecture 3 — Multi-team GKE platform

- Use separate namespaces with ResourceQuota, LimitRange, RBAC, NetworkPolicy, and dedicated service accounts. For higher trust separation, move teams to separate clusters or projects, not just namespaces.
- Choose ingress ownership model up front: central platform-controlled gateways or ingress classes, team-owned HTTP routes only. Keep secrets externalized and bound through Workload Identity.

### Architecture 4 — GCP DR

- Multi-zonal clusters are baseline, but regional failure requires backup/restore or standby clusters in a secondary region, replicated artifacts, DNS failover, and precreated IAM, policy, and secret dependencies.
- Validate DR for GKE node pools, load balancing, Cloud DNS, Artifact Registry, Secret Manager, and data-service recovery point separately; one healthy cluster without data or identity is not recovery.

### Implementation 1 — Terraform for multi-project GCP

- Use one root module per project, stack, and environment. Backend in GCS with separate prefix per environment and stack. Bootstrap backend separately from workload stacks.
- Use provider impersonation with explicit service accounts per environment; never share one admin service account across prod and non-prod.

### Implementation 2 — CI/CD to GKE without broad static credentials

- Use GitHub, GitLab, or Jenkins OIDC/workload identity federation to impersonate a narrowly scoped deploy service account. Bind service-account permissions only to Artifact Registry, GKE deploy, and related read operations in the target project.
- Deploy manifests through a dedicated namespace-scoped RBAC identity when possible rather than cluster-admin.

### Implementation 3 — Import manual GCP resources

- Discover actual resource names, self links, labels, location, and project. Write matching Terraform config first, then import one address at a time. Stabilize before any module refactor.
- Beware server-populated fields, API defaults, and resource move semantics across projects and regions.

### Implementation 4 — Secure GKE platform building blocks

- Private cluster where feasible, Workload Identity Federation for GKE, NetworkPolicy-enabled CNI, managed ingress or Gateway, cert management, logging and metrics pipeline, policy controller or admission policy, and restricted node or service-account strategy.
- Keep platform add-ons in dedicated namespaces and service accounts; do not let app teams patch shared ingress or controllers directly.

### Debugging 1 — Healthy pods, customer latency

- Walk path: DNS -> global or regional load balancer -> backend service and NEG health -> Ingress or Gateway -> Service -> Pod -> HPA or node scale -> downstream.
- Pods can be green while NEGs are stale, health checks are wrong, connection draining is incomplete, or autoscaling is lagging behind request burst.

### Debugging 2 — Service-account change breaks prod

- Check IAM binding inheritance from org, folder, and project, service-account impersonation grants, Workload Identity principal mapping, and whether hidden dependency service accounts lost roles such as artifact pull or secret access.
- Audit logs usually identify the denied principal and exact permission earlier than app logs do.

### Debugging 3 — Terraform destructive plans after refactor

- Check moved resource addresses, `for_each` key changes, project/provider alias changes, import state after module moves, and whether a resource became managed by a different root module.
- Never combine import, module split, and naming changes in one step.

### Review 1 — Broad service-account permissions

- Reduce one-to-many service accounts into per-workload runtime service accounts, per-pipeline deploy service accounts, and scoped platform service accounts. Disable key creation and require impersonation or federation.

### Review 2 — Weak GKE tenant isolation

- Fix namespace RBAC, network policy, ingress ownership, shared add-on permissions, and secret scoping. If two teams need conflicting trust domains, move them to different clusters or projects.

### Cross-Tool 1 — Governed GCP delivery path

- Commit -> review -> build -> sign and scan -> publish artifact -> Terraform plan with policy checks -> approved apply -> GKE deployment -> rollout observation -> SLO gate -> promote. Persist commit SHA, image digest, plan artifact, applied revision, and rollout evidence end to end.

### Cross-Tool 2 — CI looked healthy but real traffic failed

- Correlate pipeline success with runtime evidence: NEG health, Gateway or Ingress config, HPA reaction time, error budget burn, trace outliers, and customer-region-specific failures. CI validates artifact quality; production validates service behavior under real routing and dependencies.

## Azure
### Architecture 1 — Production-grade Azure platform

- Use management groups for policy hierarchy and subscriptions as hard governance and scale boundaries. Separate platform, connectivity, security, prod workloads, and non-prod workloads into different subscriptions.
- Centralize policy, Defender, logging, and identity governance. VNets or subnets follow hub-and-spoke or virtual WAN pattern; shared ingress and egress plus private connectivity are platform-owned.
- AKS clusters live in workload or platform subscriptions depending on tenancy model, but prod and non-prod never share subscription or identity path.

### Architecture 2 — Azure RBAC and identity boundaries

- Humans authenticate through Microsoft Entra ID with PIM/JIT elevation, group-based RBAC, and no standing Owner on subscriptions. Platform engineering has scoped admin on platform subscriptions only.
- Pipelines use federated credentials or managed identities; runtime uses managed identities or Entra Workload ID for AKS. Break-glass accounts are cloud-only, MFA-protected, monitored, and not reused for daily work.

### Architecture 3 — Secure AKS platform

- Use dedicated node pools by workload class, network policies and CNI controls, egress control via firewall/NAT, ingress ownership separation, Key Vault integration, policy enforcement, workload identity, and pod/service isolation.
- Do not let teams share an unrestricted ingress controller or a broad kube-system privileged path.

### Architecture 4 — Azure DR

- Define failure domains for AKS, data stores, Key Vault, ingress, DNS, and CI/CD. Pair regions for backup and warm standby where needed. Keep IaC, images, and secrets availability in the DR plan, not only the application workloads.
- DR completion criteria must include successful identity issuance, secret retrieval, traffic cutover, and runbook-tested failback.

### Implementation 1 — Terraform for multi-subscription Azure

- Use separate backend containers or keys per subscription plus environment plus stack. Keep provider aliases explicit for each subscription and tenant combination.
- Structure roots by `platform/`, `connectivity/`, `aks/`, `shared/`, `workload-foundation/`; modules stay generic and roots bind them to one scope only.

### Implementation 2 — AKS delivery pipeline access

- Pipeline identity should have only cluster deploy and dependency read rights in the target scope. For AKS, use federated credential or managed identity to obtain Azure access, then Kubernetes RBAC for namespace deployment where possible.
- Prod pipelines use a different identity and approval path from non-prod.

### Implementation 3 — Import manual Azure resources

- Inventory exact resource IDs, API versions, names, and immutable properties. Write minimal matching config, import into correct resource address, then remove replacement causes before any refactor.
- Be careful with resources that surface server-populated fields, renamed blocks, or nested dependencies that Terraform models differently from ARM.

### Implementation 4 — Secure AKS components

- Private API endpoint or restricted access, CNI and network policy, ingress controller or managed ingress, cert management, Key Vault or CSI or operator-based secret integration, monitoring agents, policy engine, backup tooling, node pool strategy, workload identity, and baseline quotas and limits.

### Debugging 1 — AKS intermittent failure with normal cluster health

- Follow path: DNS -> ingress/load balancer -> service/endpoints -> pod readiness -> workload identity/Key Vault access -> node pool saturation -> SNAT or egress -> downstream dependency.
- Cluster health is necessary but insufficient; many AKS issues are data-plane, identity, or network path failures with a healthy control plane.

### Debugging 2 — Hardening change breaks deployments

- Inspect RBAC role assignments at management-group, subscription, and resource-group scope, inherited denies, federated credential subject and audience conditions, and whether the pipeline identity lost `AcrPull`, AKS access, or Key Vault secret rights.

### Debugging 3 — Terraform wants to replace prod Azure resources

- Check address drift, import to wrong provider alias or subscription, immutable property changes, resource group or name changes, and module refactor without `moved` tracking.

### Review 1 — One service principal across prod and non-prod

- Replace with separate federated identities per environment and least-privilege scopes. One cross-environment principal destroys blast-radius control and audit clarity.

### Review 2 — Weak AKS identity, policy, and ingress ownership

- Standardize on Entra Workload ID, namespace-specific identities, baseline or restricted pod policy, reviewed ingress classes, and central edge policy ownership. Remove shared secrets and implicit admin paths.

### Cross-Tool 1 — Azure-safe delivery path

- Git review -> build, scan, sign -> artifact publish -> Terraform plan and policy evaluation -> approved apply -> AKS deployment with rollout controls -> metrics/log gate -> promotion. Every stage must preserve the same release identity: commit, artifact digest, plan checksum, cluster revision.

### Cross-Tool 2 — Policy or identity change breaks provisioning and runtime

- Locate the earliest denied control: Terraform provider auth, ARM authorization, AKS access, or runtime managed identity token retrieval. Fix the first blocked control instead of chasing later application symptoms.

## Terraform
### Architecture 1 — Terraform operating model

- State boundary equals blast-radius boundary. Separate state by environment, account/subscription/project, and major stack type. Never let one apply mutate unrelated teams' networking, IAM, app, and data together.
- Modules define standardized building blocks; root modules define ownership and deployment scope. CI/CD must run fmt/validate/static checks, plan, policy checks, approval, apply, and post-apply verification.
- State backend must support locking, versioning, encryption, auditability, and least-privilege access. Secrets should not enter state unless unavoidable, and then access is tightly limited.

### Architecture 2 — Terraform ownership

- Networking, identity, platform, and workload foundation belong to separate roots and often separate repos or pipelines. Teams consume outputs/contracts, not shared write access to the same state.
- Use interface boundaries: subnet IDs, cluster endpoints, IAM role names, secret references. Coupling everything in one state creates unsafe coordination debt.

### Architecture 3 — Reusable module strategy

- Keep modules opinionated on invariants and flexible on workload-specific values. Expose only stable inputs and outputs; avoid passing entire provider or environment policy through deep module chains.
- Version modules, publish change logs, and add migration notes for breaking changes.

### Architecture 4 — Terraform DR

- Protect state objects with versioning or soft delete and test restore. Replicate or rebuild CI runners, maintain provider credentials or secrets path in DR, and document how to recover from state corruption versus cloud failure.
- Rollback means either restoring last known good config/state pairing or applying forward corrective config; not every provider supports a literal reverse apply.

### Implementation 1 — Production repository

- `modules/`, `roots/`, `policies/`, `examples/`, `bootstrap/`, and CI pipeline definitions. `roots/` contains one deployable unit each with backend, providers, variables, and environment mapping.
- Generate provider lock files, pin versions, and keep environment values outside modules.

### Implementation 2 — Providers, aliases, environments

- One provider instance per target boundary, aliased explicitly. Root modules wire the correct alias to child modules. Environment separation is by root and backend key, not only by Terraform workspace.

### Implementation 3 — CI/CD stages

- format -> validate -> init -> lint/static scan -> plan -> policy evaluation -> cost/risk summary -> manual approval for protected envs -> apply -> drift/reporting -> post-apply smoke verification.
- Store the plan artifact immutably; prod apply should consume the reviewed plan, not regenerate a fresh one.

### Implementation 4 — Import-and-stabilize

- Write config first, import one resource or related batch, plan, eliminate unexpected change, then commit. Keep a rollback state backup with `state pull` before any repair.
- Do not normalize names, split modules, or change lifecycle in the same import step.

### Debugging 1 — First apply after import recreates infra

- Check resource address, provider alias, ID format, immutable attributes, missing required non-default args, module path, and ownership collisions with another state/controller.

### Debugging 2 — Locking and slow applies

- Decide whether the problem is oversized state, excessive provider refresh, serialized long-running resources, shared runners, or remote backend contention. Fix by reducing state scope, parallelizing safe stacks, and improving runner isolation rather than disabling locks.

### Debugging 3 — Module refactor plan churn

- Use `moved` blocks for address changes, keep `for_each` keys stable, avoid output renames without consumer migration, and ensure only one dimension changes at a time.

### Debugging 4 — Secrets in state or logs

- Contain by restricting backend access, rotating exposed secrets, scrubbing CI artifacts, and reworking design to reference secrets rather than materialize them. Marking values sensitive only hides display; it does not remove them from state if the provider stores them.

### Review 1 — One state for many teams

- Split by blast radius immediately. One monolithic state makes every plan noisy, increases lock contention, and turns minor mistakes into platform-wide risk.

### Review 2 — Secrets exposure and unsafe defaults

- Pin provider versions, verify plugin trust, move credentials to short-lived identity, deny broad provider credentials in runners, and review every data source or resource that can echo secrets into outputs or logs.

### Cross-Tool 1 — Safe Terraform delivery

- Git review ensures intent quality, CI verifies syntax and policy, cloud IAM restricts actor power, state backend enforces locking and audit, and observability correlates apply events with cloud and runtime signals.

### Cross-Tool 2 — Terraform plus policy plus Kubernetes change conflict

- Trace the earliest policy violation or network/identity change that made the rollout unsafe. Recovery usually means halting further applies, restoring traffic path or identity first, then reconciling desired state.

## Kubernetes
### Architecture 1 — Multi-team Kubernetes platform

- Use namespaces as the minimum tenancy unit, but combine them with RBAC, ResourceQuota, LimitRange, NetworkPolicy, Pod Security, separate service accounts, and controlled ingress classes. Shared components live in dedicated namespaces with restricted admin paths.
- Define platform-owned versus team-owned APIs: teams can deploy Deployments, Services, and route objects in their namespace; they cannot mutate cluster-wide security, storage classes, or core controllers.

### Architecture 2 — Multi-tenancy without cluster-admin normalization

- Humans never get cluster-admin for routine work. Use aggregated roles, namespace roles, and audited break-glass only. Default service account is not used by apps.
- Separate privileged operators from application namespaces; if a tenant needs privileged DaemonSets or unsafe host access, that is a signal for separate cluster placement.

### Architecture 3 — Kubernetes DR

- Treat cluster state, app manifests, secrets, PV-backed data, registry access, and DNS as separate recovery items. GitOps rebuilds cluster objects; it does not restore data or external secret stores by itself.
- Test cluster recreation, secret/operator bootstrap, storage restore, and ingress cutover with measured RTO.

### Architecture 4 — Workload security

- Per-app service accounts, RBAC minimum necessary, Pod Security restricted or baseline, admission validation for images and labels, network policy default deny, signed images, non-root containers, read-only filesystems where possible, and runtime restrictions like seccomp and capability drop.

### Implementation 1 — Manifest set for production service

- Deployment with resource requests and limits, readiness, liveness, and startup probes, rolling update config, anti-affinity or topology spread, Service, Ingress or Gateway object, HPA, PDB, ServiceAccount, ConfigMap and Secret references, NetworkPolicy, and monitoring annotations.

### Implementation 2 — Secure namespace RBAC pattern

- One service account per workload, one Role per namespace for required verbs only, RoleBinding to that service account, no secret list or watch unless essential, and a separate deploy role for CI with namespace-scoped patch/apply.

### Implementation 3 — Safe rollout controls

- Use startup probes to protect slow boot, readiness to gate traffic, PDB to preserve capacity, maxSurge and maxUnavailable tuned to service tolerance, rollout status monitoring, and immediate rollback criteria tied to SLO or error rate.

### Implementation 4 — GitOps/pipeline promotion

- Separate config per environment, immutable image tags or digests, promotion by pull request rather than in-cluster mutation, policy validation before sync, and environment-specific approval for prod.

### Debugging 1 — K8s healthy, customers failing

- Follow external to internal: DNS -> ingress/Gateway -> load balancer health checks -> Service and EndpointSlice -> NetworkPolicy -> pod readiness -> app logs and traces -> downstream dependency. Kubernetes object `Ready` never proves customer path health.

### Debugging 2 — Pods restart only under real traffic

- Check OOMKilled, CPU throttling, probe timeouts under load, thread or connection exhaustion, GC pauses, conntrack saturation, file descriptor limits, and dependency timeout amplification.

### Debugging 3 — Cluster upgrade selective outages

- Inspect removed APIs, admission policy incompatibility, CNI or CSI version changes, kube-proxy mode change, PodSecurity behavior, and workloads pinned to deprecated fields or node features.

### Debugging 4 — Intermittent authorization failures

- Trace the exact principal: user, CI identity, or service account. Verify token audience and expiry, RoleBinding namespace, aggregated ClusterRole, webhook authorizers, and external secret or identity dependencies.

### Review 1 — Many teams use cluster-admin/default service account

- Replace with namespace-specific service accounts and roles immediately. Default service account plus cluster-admin means any pod escape or CI compromise becomes cluster compromise.

### Review 2 — Inconsistent ingress, secrets, and policy

- Standardize on one ingress ownership contract, one secrets integration model, and one minimum policy baseline per namespace. Variation at these layers causes outage patterns that are hard to debug and audit.

### Cross-Tool 1 — End-to-end Kubernetes delivery

- Git review -> image build and scan -> signed artifact -> config change -> CI or GitOps sync -> admission/policy validation -> rollout controls -> observability-based verification -> promotion or rollback.

### Cross-Tool 2 — Terraform network + NetworkPolicy break traffic

- Determine whether the cloud path failed before the pod path. Test LB to node or pod reachability first, then namespace policy. Recovery usually restores cloud path or relaxes policy only enough to reestablish known-good traffic, then reapplies narrowed controls.

## Linux, Python, and Bash
### Architecture 1 — Linux/Python/shell standards

- All operational tools must be idempotent, observable, and safe by default: explicit timeouts, retries with backoff, structured logs, dry-run mode, input validation, and exit codes that CI can trust.
- Shell is for orchestration and thin wrappers; Python is for nontrivial control flow, API interaction, parsing, and concurrency limits. Critical logic should not live in opaque bash one-liners.

### Architecture 2 — Hardening and operability standards

- Ephemeral CI agents, minimal host packages, separated admin users, audit logs, pinned tool versions, immutable images where possible, and documented debug toolset. Troubleshooting environments must not become shadow-production servers.

### Implementation 1 — Python utility structure

- Use `main()` plus argument parser plus config loader plus API/client layer plus validation layer plus action executor plus structured output. Add retries, timeouts, and circuit handling around remote calls and map failures to actionable exit codes.
- Emit machine-readable JSON for pipelines and concise human logs for operators.

### Implementation 2 — Shell script structure

- Use `set -euo pipefail`, controlled `IFS`, explicit tempdir cleanup traps, argument validation, dependency checks, and quoted variables everywhere. Avoid parsing JSON with grep or sed when `jq` or Python is available.

### Implementation 3 — Linux host validation checklist

- Check CPU steal/load, memory pressure and swap policy, filesystem capacity and inodes, disk latency, mount options, conntrack, ephemeral ports, DNS resolution, clock sync, ulimits, open files, kernel logs, service unit health, and security baselines.

### Implementation 4 — Package, test, distribute internal Python tools

- Build versioned packages, pin dependencies, unit-test parsing and logic, integration-test API interactions, publish to internal package index or artifact registry, and sign artifacts if supply-chain controls require it.

### Debugging 1 — Python automation fails at scale

- Check API throttling, pagination, retry storms, unbounded concurrency, large in-memory collections, timeout layering, and serialization hot spots. Scale bugs often come from control-plane assumptions that were safe only in dev.

### Debugging 2 — Shell step differs across environments

- Compare shell, coreutils version, env vars, locale, path order, quoting, glob expansion, working directory, and secret injection method. Scripts that rely on implicit environment shape are fragile.

### Debugging 3 — Linux checks for slow service

- Run CPU, memory, IO, network, and DNS checks in that order, then file descriptor and socket state, then kernel logs. Cloud dashboards may miss node-local bottlenecks like conntrack exhaustion or blocked IO.

### Debugging 4 — Node looks healthy but workloads flap

- Inspect `dmesg`, kubelet logs, container runtime logs, disk pressure, image GC, conntrack table, CNI errors, MTU mismatch, and intermittent DNS failures.

### Review 1 — Unsafe credentials and missing idempotency

- Remove secrets from args, env, and logs where possible, switch to workload identity or secret reference, add dry-run and state checks, and make repeated execution safe.

### Review 2 — Weak Linux operational model

- Introduce role separation, centralized logging, audited sudo, immutable or reproducible host config, and standard runbooks. Manual SSH-only operations do not scale safely.

### Cross-Tool 1 — Fit Linux, Python, shell into platform

- Linux provides substrate signals, shell glues deterministic steps, and Python handles reliable automation. All three should surface the same release IDs and incident correlation IDs used across cloud, Terraform, and Kubernetes.

## Docker, Helm, and Argo CD / GitOps
### Architecture 1 — Image/chart/GitOps standards

- Images must be reproducible, minimal, non-root, and pinned. Charts must have deterministic defaults, explicit values schema, and no hidden environment-specific logic. GitOps must define clear source of truth, promotion path, and drift ownership.
- Promotion moves an immutable artifact reference between environment configs; it does not rebuild artifacts differently per environment.

### Architecture 2 — GitOps governance

- Teams self-serve through reviewed app manifests or values in approved repos. Argo CD projects, repo allowlists, namespace restrictions, and sync policies prevent bypassing environment and ownership boundaries.

### Implementation 1 — Production Dockerfile

- Use multi-stage build, pin base image digest, install only runtime dependencies, create non-root user, set explicit workdir and entrypoint, copy only needed artifacts, and avoid secrets in build context or layers.

### Implementation 2 — Production Helm chart

- Chart contains templates for deployment, service, ingress, HPA, PDB, service account, and network policy as needed, values schema/defaults, helper templates, and environment overrides outside the chart. Avoid templating arbitrary YAML blobs that bypass review.

### Implementation 3 — Argo CD promotion model

- Keep separate environment overlays or repos. Dev auto-sync can be enabled; prod should require reviewed PR and often manual sync or controlled promotion. AppProject boundaries restrict destination clusters or namespaces and source repos.

### Implementation 4 — CI/CD controls for image promotion

- Build -> SBOM/provenance -> vulnerability scan -> sign -> push -> chart or manifest update with immutable digest -> promotion PR -> Argo sync -> rollout verify -> rollback if SLO breached.

### Debugging 1 — Works locally, fails in production container

- Check UID/GID permissions, filesystem paths, missing runtime files, env injection, entrypoint PID1 behavior, DNS/cert bundle, architecture mismatch, and reliance on tools present locally but absent in the minimal image.

### Debugging 2 — Helm release succeeds but app broken

- Render templates locally and compare applied manifests, verify values-source precedence, secret injection, readiness paths, config map reload behavior, and whether Helm success only means Kubernetes accepted manifests, not that the service works.

### Debugging 3 — Argo CD drift or sync issues

- Determine whether drift is real desired-state divergence, generated-field noise, controller permission failure, Helm or Kustomize render difference, or out-of-band mutation by another operator.

### Review 1 — Bad Dockerfile

- Remove root execution, pin base image, drop build tools from runtime, clean package caches, eliminate copied secrets, and add explicit user, permissions, and health semantics.

### Review 2 — Weak GitOps model

- Separate ownership, secrets, and promotion. Unclear source of truth plus shared write paths means no one can explain why production changed.

### Cross-Tool 1 — Git to image to Argo to observability

- Use commit SHA to produce signed image digest; chart or manifest references that digest; Argo syncs reviewed desired state; rollout status and SLO telemetry determine completion. That linkage must be queryable during incidents.

## Jenkins, Git, and GitHub Actions or GitLab CI
### Architecture 1 — CI/CD operating model

- Git is the policy gate, pipeline is the execution gate, and cloud/Kubernetes/IaC permissions are the blast-radius gate. Shared libraries/templates are versioned and rollout-controlled like production code.
- Use ephemeral agents/runners, centralized secretless auth where possible, protected environments, and immutable artifact promotion.

### Architecture 2 — Self-serve governance

- Teams inherit approved pipeline building blocks but cannot bypass status checks, code owners, environment protections, or deployment approvals. Templates expose safe extension points only.

### Implementation 1 — Secure production pipeline stages

- checkout -> dependency restore -> unit/integration tests -> static analysis -> build -> image scan/SBOM/sign -> publish artifact -> infra/app plan -> approval -> deploy -> smoke or SLO checks -> promote or rollback.

### Implementation 2 — Branch, tag, promotion model

- App code merges to trunk/main, release artifacts are immutable and tagged, infra code follows reviewed mainline, and environment promotion occurs by PR or release object rather than mutable local state.

### Implementation 3 — Credentials and promotion without long-lived secrets

- Use OIDC/federation from pipeline to cloud, secret managers for short-lived runtime secrets, environment protection rules, and manual approval only at boundary transitions such as prod promotion—not as a substitute for missing controls.

### Debugging 1 — Shared library/template breaks many pipelines

- Freeze rollout, pin consumers back to last known good version, test the library in a representative matrix, and add staged adoption. Shared pipeline code is a platform dependency and needs release discipline.

### Debugging 2 — Bad merge caused impact

- Classify which gate failed: inadequate review ownership, missing test coverage, pipeline not checking deployment safety, or lack of runtime verification after rollout.

### Debugging 3 — Same template, different repo fails

- Compare repo permissions, secret or environment scopes, runner labels/capabilities, branch protections, and repository-specific required inputs. 'Same template' does not mean same execution context.

### Review 1 — Weak Jenkins setup

- Replace long-lived controller-stored broad credentials with scoped credentials or federation, use ephemeral agents, restrict shared-library mutation, and harden the controller because compromise there is fleet compromise.

### Review 2 — Weak Git workflow

- Standardize protected branches, CODEOWNERS, signed tags/releases where required, release notes, and environment promotion by reviewed change, not force-push or ad hoc branch magic.

### Cross-Tool 1 — Connect Git status checks to deploy safety

- Status checks prove code/build quality; Terraform/Kubernetes/cloud permissions prove bounded execution; observability proves customer-safe completion. All three are required for a production-ready pipeline.

## Observability
### Architecture 1 — Product-platform observability

- Collect the four core signal families: metrics, logs, traces, and events/change records. Tie every alert to a service owner and escalation path. Use SLO-based alerting for customer-impacting services and symptom-based dashboards for triage.
- Keep infrastructure, control-plane, and application telemetry correlated through consistent service, environment, cluster, version, and team metadata.

### Architecture 2 — OpenTelemetry and ELK design

- Standardize resource attributes, keep cardinality bounded, sample traces intentionally, parse logs to structured form at source or collector, and align retention to use case: hot troubleshooting, warm compliance, cold audit.

### Implementation 1 — Alerts and dashboard views

- Alerts: availability or error rate, latency (p50/p95/p99), saturation, queue depth, dependency failure, rollout health, certificate/secret expiry, and error-budget burn. Dashboards: golden signals, dependency map, rollout overlay, infra saturation, and top customer-path endpoints.

### Implementation 2 — Metric and label strategy

- Labels must identify environment, service, cluster, namespace, method or route where bounded, and owner. Do not label by user ID, request ID, pod UID, or other high-cardinality fields. Split unbounded values into logs or traces instead.

### Implementation 3 — Collector/pipeline pattern

- Agents or SDKs emit to collectors; collectors enrich with environment metadata, drop noisy attributes, batch and retry, route to metrics/logs/traces backends separately, and apply backpressure or queueing so backend incidents do not directly crash apps.

### Implementation 4 — Datadog/Dynatrace ownership model

- Enforce tagging for service, team, environment, runtime, and criticality. Dashboards, monitors, and SLOs map back to owners; shared components still need named owners, not `platform` as a catch-all.

### Debugging 1 — Intermittent latency spikes

- Check whether latency is ingress, app, DB, cache, queue, or external API. Use RED + USE signals, correlate exemplars/traces with p99 spikes, and compare good versus bad requests by route, dependency, region, and release version.

### Debugging 2 — Dashboard healthy, customers failing

- Validate whether telemetry measures internal health rather than user path. Missing synthetic checks, edge-region metrics, client-side errors, or dependency-specific SLIs can create a false-green platform.

### Debugging 3 — Telemetry cost rises, signal falls

- Look for cardinality explosions, verbose logs, unbounded trace capture, duplicate collection, and metrics emitted at too-fine dimensions. Cost control is part of observability design, not cleanup after the fact.

### Debugging 4 — ELK search collapses during incident

- Protect ingestion first: throttle noisy sources, prioritize critical indices, reduce parsing cost, and preserve most important logs. A perfect ingestion policy that takes the cluster down is worse than selective shedding.

### Review 1 — Weak labels, duplicate dashboards, alert fatigue

- Create one canonical service dashboard, one ownership model, and alerts tied to actionability. Deduplicate monitors and move informational conditions out of paging paths.

### Review 2 — Uncontrolled OTel/logging model

- Introduce schema, retention classes, parser ownership, and cardinality review. Without these, telemetry becomes expensive data exhaust rather than an incident tool.

### Cross-Tool 1 — One incident timeline across tools

- Every Git merge, pipeline run, Terraform apply, cluster rollout, and edge config change must land as an event annotation in the observability system so operators can see change-to-impact causality.

## NGINX / Ingress, Kafka, Redis, and SQL Reliability Basics
### Architecture 1 — Operational boundaries for ingress, messaging, caching, and DB

- Treat ingress, queue, cache, and DB as first-class reliability dependencies with explicit ownership, SLOs, and scaling policy. App teams must know dependency behavior, but platform must provide safe defaults and operational guardrails.

### Architecture 2 — Observability, incident, DR model

- Expose per-dependency saturation, latency, error, replication, and backlog signals. DR plans must say what happens to routing state, message durability, cache warmup, and database consistency—not just VM or pod replacement.

### Implementation 1 — NGINX/ingress configuration areas

- Review TLS policy, cert rotation, host/path rules, header forwarding, request/body size, keepalive, timeout and retry policy, upstream health checks, connection limits, and whether proxy buffering or HTTP version behavior matches the app.

### Implementation 2 — Kafka, Redis, SQL operational controls

- Kafka: replication factor, ISR health, partitioning strategy, consumer lag, idempotent producers where needed, retention, and rebalancing impact. Redis: persistence mode, replica health, eviction policy, memory headroom, failover strategy. SQL: backup/PITR, replication health, connection pool sizing, slow queries, lock contention, and schema-change safety.

### Debugging 1 — Requests fail only through ingress

- Test DNS, TLS handshake, listener/rule match, upstream selection, headers, body limits, timeouts, readiness health path, and network policy. If direct pod or service access works, the fault is at edge or edge-to-upstream contract.

### Debugging 2 — Queue lag, cache instability, or DB saturation

- Identify the bottleneck owner first: producer rate, consumer throughput, cache memory/eviction, DB lock or pool starvation. Do not scale every layer at once; fix the constrained resource and verify backlog drain rate.

### Debugging 3 — Rollout increases latency without code errors

- Check connection pool mismatch, lock amplification, cache-key miss ratio, changed request fan-out, timeouts/retries, and ingress buffering or keepalive interactions.

### Review 1 — Service treats dependencies as implementation detail

- Refactor to make dependency contracts explicit: timeout budgets, retry policy, idempotency, backpressure behavior, fail-open/fail-closed choice, and observability per dependency.

### Cross-Tool 1 — Identify edge vs service vs dependency failure

- Start with edge telemetry and request-path traces, then compare app saturation and dependency timings. The slowest layer by itself is not always the root cause; often the root is changed retry behavior at the edge or app.

## Cross-Tool Product-Company Rounds
### Architecture 1 — End-to-end serious product-company platform

- Cloud accounts/projects/subscriptions define hard governance boundaries; Terraform defines reproducible infra; CI/CD produces signed immutable artifacts; registry stores digests; Kubernetes runs workloads with policy; observability correlates code, infra, and runtime.
- Identity is federated and least-privilege at every layer. Prod and non-prod are separated in identity, state, runtime, and approval path.

### Architecture 2 — Platform DR including platform tooling

- Recoverability must cover state backends, artifact registry, CI runners/control plane, cluster bootstrap config, secrets/PKI, and telemetry history needed to verify recovery. If only the app is recoverable, the platform is not.

### Architecture 3 — Migration from inconsistent tooling

- Standardize control points first: identity, artifact immutability, state boundaries, ingress ownership, and observability taxonomy. Migrate teams through paved-road templates and policy checks rather than one-time big-bang rewrites.

### Implementation 1 — End-to-end delivery sequence

- Commit -> review -> build/test -> sign/scan -> publish artifact -> Terraform plan -> policy check -> approved apply -> cluster/config deploy -> rollout verify -> observability gate -> release record.

### Implementation 2 — Minimum implementation standards

- Protected repos, CODEOWNERS, ephemeral CI identity, immutable artifacts, pinned module/chart/image versions, environment-separated state, rollout probes/PDBs, namespace RBAC, basic alerts and dashboards, and audited rollback path.

### Implementation 3 — Required production-safe release evidence

- Commit SHA, artifact digest, SBOM or scan result, plan checksum, approval record, deployment revision, namespace or cluster target, smoke or SLO evidence, and audit trail of who/what/when.

### Debugging 1 — Incident spans every tool

- Build one timeline, assign one owner per layer, and start from customer symptom backward until you find the first unsafe state change or first confirmed service regression. Tool health pages are secondary to request-path evidence.

### Debugging 2 — Emergency rollback fails

- Stabilize traffic first, identify which changes are reversible independently, and choose the lowest-risk recovery path: route rollback, config rollback, infra rollback, or scale mitigation. If no clean rollback exists, fix-forward with blast-radius containment.

### Debugging 3 — Every tool healthy individually

- The missing layer is often ownership, integration contract, or observability gap. Healthy components can still compose into a broken customer path if contracts changed silently.

### Review 1 — Strong tools, slow incidents

- Usually the gap is missing change correlation, unclear ownership, inconsistent runbooks, broad access that hides accountability, and too many mutable control points.

### Cross-Tool 1 — Lead severe war-room

- Set incident commander, define customer impact, freeze nonessential changes, assign owners by layer, request evidence not opinions, keep one timeline, declare mitigation criteria, verify recovery from user path, and capture follow-up actions before closure.

### Cross-Tool 2 — Redesign after major incident

- Convert incident learnings into control points: tighter identity, smaller blast radius, safer defaults, better telemetry, deterministic promotion, and rehearsed recovery. The redesign target is faster detection and narrower failure scope, not theoretical elegance.

## Hands-On Mini Tasks
### Task 1 — Terraform layout for one service stack

- Split `network-foundation`, `runtime-platform`, and `service-stack` roots. Keep separate state and least-privilege credentials; expose outputs instead of sharing state writes. Add plan approval, policy checks, and remote-state encryption and versioning.

### Task 2 — Production Kubernetes package

- Include Deployment, Service, Ingress or Gateway, ServiceAccount, HPA, PDB, probes, requests/limits, NetworkPolicy, and rollout or rollback criteria. Keep namespace-scoped identity and explicit ingress ownership.

### Task 3 — Secure Dockerfile + CI/CD flow

- Use multi-stage build, pinned base, non-root runtime, scan/sign artifact, publish immutable digest, promote by reviewed config change, and block deploy on failing scan or health gate.

### Task 4 — Shell + Python operational workflow

- Shell wrapper validates environment and sequencing; Python client performs API or query logic with retries/timeouts and emits structured output. Both support dry-run and deterministic exit codes.

### Task 5 — Alert set + dashboard outline

- Alert on error-budget burn, high latency, elevated error rate, dependency saturation, and rollout regression. Dashboard shows golden signals, dependency panels, rollout annotations, and capacity trend.

### Task 6 — Namespace-isolated RBAC + network policy

- Namespace Role/RoleBinding for CI and workload service account, deny-all ingress/egress baseline, allow only required same-namespace, ingress-controller, DNS, and dependency destinations.

### Task 7 — Safe CI/CD IAM or service-account pattern

- Use repo/environment-bound federated identity -> scoped deploy identity -> namespace/cloud-role permissions only for target environment.

## Mini Debugging Cases
### Case 1 — Import succeeded, plan wants recreation: first six checks

- resource address, provider alias or scope, imported ID correctness, immutable args mismatch, missing non-default config, and conflicting ownership by another state/controller.

### Case 2 — Pods healthy, users fail: traffic-path order

- DNS -> load balancer or ingress -> listener/rules -> Service and EndpointSlice -> network policy -> readiness/app -> downstream.

### Case 3 — CI passed but latency doubled: rollback vs fix-forward

- Use customer impact, blast radius, reversibility, and confidence. Roll back if latest change is strongly causal and rollback is clean; fix forward only when rollback risk exceeds mitigation risk or data/schema compatibility blocks rollback.

### Case 4 — Host looks normal, app unstable: Linux first checks

- CPU throttling/load, memory pressure/OOM, disk latency, DNS, network drops/retransmits, fd/socket exhaustion, and kernel logs.

### Case 5 — Everything green, customers failing

- Prove whether monitoring measures the actual user path: synthetic checks, regional edge metrics, client errors, auth failures, and dependency-specific SLIs.

### Case 6 — GitOps sync successful, service broken

- Separate render success from runtime success: compare desired values, rendered manifests, secret materialization, admission mutations, rollout status, and ingress/service behavior.

## Mock Interview Rounds
### Round 1 — 45-minute architecture round

- Answer in layers: boundary model -> identity -> network -> delivery -> runtime policy -> observability -> DR. Always define who owns each shared control and where prod isolation lives.

### Round 2 — 45-minute debugging round

- Start with customer symptom, build request path, identify earliest failing control, confirm with evidence, mitigate blast radius, then explain permanent prevention.

### Round 3 — 60-minute implementation round

- Write repo layout, state boundaries, pipeline stages, image/build controls, Kubernetes manifests, and baseline telemetry. Keep decisions explicit and minimal; avoid broad shared roles or monolithic state.

### Round 4 — 45-minute review round

- Prioritize findings by blast radius: shared admin, shared state, weak secret handling, uncontrolled ingress, missing rollout gates, and missing observability ownership.

### Round 5 — War-room leadership round

- Lead by impact, evidence, ownership, timeline, mitigation, verification, and prevention. Serious product companies evaluate clarity and sequencing as much as raw technical knowledge.

## Reference Themes to Reuse in Interviews

- Separate prod from non-prod in identity, state, and runtime.
- Prefer short-lived federated identity over static secrets.
- State boundary should match blast-radius boundary.
- Cluster health is not customer-path health.
- Git success and CI success do not prove production safety.
- Rollback is a designed capability, not an improvised command.
- Observability must correlate changes with impact.
- Shared platform controls need explicit ownership contracts.
- DR must include delivery tooling, identity, artifacts, and secrets—not only compute and data.

## Official Documentation Used as Ground Truth

- AWS Well-Architected Framework; Amazon EKS Best Practices; AWS Organizations and IAM guidance.
- Google Cloud resource hierarchy and IAM documentation; Shared VPC and GKE Workload Identity Federation documentation.
- Microsoft Azure Cloud Adoption Framework landing zones; AKS networking, ingress, and workload identity documentation.
- HashiCorp Terraform language/backend/import/state documentation.
- Kubernetes documentation for RBAC, NetworkPolicy, PodDisruptionBudget, rolling updates, and rollout management.
- Helm chart template and hooks documentation; Argo CD sync phases and waves documentation.
- Docker multi-stage build documentation.
- GitHub Actions OIDC documentation; GitLab CI OIDC documentation; Jenkins credentials, agents, and best-practices documentation.
- OpenTelemetry, Prometheus, and Alertmanager documentation.
- Redis replication and persistence documentation; Apache Kafka configuration/docs; PostgreSQL replication documentation.
