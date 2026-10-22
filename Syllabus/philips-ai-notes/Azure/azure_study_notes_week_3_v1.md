# Azure Study Notes — Week 3
## Compute, Containers, Application Hosting, CI/CD, IaC, and ML/AI Delivery

> This is the **deep Week 3 Azure guide**, written much closer to a **9.5+ bar** than a normal summary.  
> It is structured as **expert-level technical study material** with:
> - technical purpose
> - core entities
> - control-plane and runtime mechanics
> - architecture fit
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** tight structure, high signal, minimal filler.

---

# 1. Why Week 3 Matters

Week 3 is about **how workloads actually run and how changes reach production safely**.

If Week 1 defined governance and Week 2 defined connectivity, Week 3 defines:
- compute runtime choice
- container platform choice
- deployment mechanics
- immutable artifact flow
- rollout safety
- platform automation
- ML and AI serving patterns

The core idea is:

> **Week 3 is the delivery plane of Azure: build, package, deploy, scale, upgrade, and serve.**

A weak Week 3 platform usually looks like this:
- VMs are treated like pets instead of deployment targets
- scale set upgrade behavior is not understood until production churn
- AKS exists, but node-pool and policy boundaries are unclear
- App Service, Functions, and Container Apps overlap without service-selection logic
- registries are private, but runtime pull identity or network path is broken
- CI/CD exists, but secrets, approvals, and environment controls are weak
- Bicep is used, but module boundaries and what-if discipline are missing
- ML endpoints are deployed, but rollout, quota, and inference mode are poorly matched

A strong Week 3 design makes it easy to answer:
- what runtime should host this workload?
- how is it packaged?
- how does it scale?
- how is it updated?
- what is the rollback model?
- how is drift prevented?
- how are inference and API endpoints exposed safely?

---

# 2. Learning Outcomes

By the end of Week 3, you should be able to:

- explain **Azure Virtual Machines**, **availability constructs**, **managed disks**, **images**, **extensions**, **automatic extension upgrades**, and **Virtual Machine Scale Sets**
- explain **VMSS upgrade policies**, **application health extension**, image/OS upgrade behavior, and how rollout safety depends on probe and health design
- explain **Azure Kubernetes Service (AKS)** cluster architecture, node pools, system vs user pools, workload identity, network policy, Azure Policy integration, and cluster governance patterns
- explain **Azure App Service**, **deployment slots**, **VNet integration**, **Azure Functions** hosting and slots, and **Azure Container Apps** revisions, ingress, secrets, and jobs
- explain **Azure Container Registry (ACR)** registries, repositories, tags, tasks, webhooks, private endpoints, and image pull patterns
- explain **Azure DevOps**, **Azure Pipelines**, service connections, environments, approvals/checks, variable groups, and secure deployment controls
- explain **GitHub Actions** for Azure, especially OIDC-based authentication instead of long-lived secrets
- explain **ARM/Bicep** modules, deployment scope, and what-if-based change preview
- explain **Azure Deployment Environments** projects, catalogs, templates, and self-service environment governance
- explain **Azure Batch** pools, nodes, autoscale, and when Batch is a better fit than AKS or VMSS
- explain **Azure Machine Learning** online endpoints, batch endpoints, deployment strategies, and where **Azure OpenAI** fits in an AI-serving architecture
- troubleshoot VM bootstrap failures, VMSS rollout stalls, AKS node-pool issues, Container Apps revision problems, registry auth failures, pipeline approval blocks, Bicep change drift, and ML endpoint rollout issues

---

# 3. Week 3 Mental Model

Think in layers:

1. **Runtime layer**  
   VM, VMSS, AKS, App Service, Functions, Container Apps, Batch

2. **Artifact layer**  
   images, packages, containers, registries, model artifacts

3. **Deployment layer**  
   pipelines, approvals, slots, revisions, rolling upgrades

4. **Infrastructure-as-code layer**  
   ARM, Bicep, modules, what-if, deployment environments

5. **Serving layer**  
   web endpoints, API paths, online inference endpoints, batch inference

6. **Governance and safety layer**  
   identity, approvals, policy, health signals, rollback path

The Week 3 principle is:

> **Runtime choice, artifact flow, deployment strategy, and rollback strategy must be designed together.**

---

# 4. Runtime Selection Before Service Selection

Avoid product-first bias.

## 4.1 Use Virtual Machines when
- the workload needs OS-level control
- you need custom kernel/driver/agent behavior
- packaging into containers would be forced or high risk
- migration lift-and-shift is still necessary

## 4.2 Use VM Scale Sets when
- the workload still needs VM semantics
- but you want elastic horizontal scale and controlled rolling updates

## 4.3 Use AKS when
- you need Kubernetes APIs and ecosystem control
- workload mix benefits from orchestrated multi-service container scheduling
- platform team can sustain cluster operations discipline

## 4.4 Use App Service when
- the app is a web app/API and the platform should abstract the host heavily
- you want fast PaaS delivery with slots and managed app hosting

## 4.5 Use Functions when
- event-driven execution and short-lived compute are primary
- scaling should follow triggers rather than always-on service replicas

## 4.6 Use Container Apps when
- you want containerized application runtime
- but not full Kubernetes control-plane exposure
- and revisions, KEDA-style scale, or jobs are attractive

## 4.7 Use Batch when
- the core problem is large-scale job execution
- task fan-out and pool autoscale matter more than service-style request handling

## 4.8 Use Azure Machine Learning endpoints when
- model serving lifecycle matters
- model deployment, online inference, or batch scoring is part of the product

## 4.9 Use Azure OpenAI when
- you are consuming managed frontier/foundation model capability
- not merely hosting your own model artifact

---

# 5. Azure Virtual Machines and VM Scale Sets

Microsoft documents VM Scale Sets upgrade policies, application health extension, automatic OS image upgrades, and automatic extension upgrade behavior. [1][2][3][4][5]

## 5.1 Azure Virtual Machines

### Technical purpose
Use Azure VMs for:
- full OS control
- legacy or specialized workloads
- agent-heavy applications
- stateful workloads that are not easy PaaS candidates

### Core entities
- VM
- NIC
- managed disk
- image
- extension
- availability construct
- identity
- boot/runtime config

### Best practices
- treat VM provisioning as code, not portal craft
- separate OS disk assumptions from data-disk lifecycle
- keep startup dependency chain documented
- use managed identity where possible
- standardize extension usage and lifecycle

### Common mistakes
- VM built manually and never reproducible
- extensions added ad hoc with no ownership
- data needed for app startup stored only on ephemeral path by mistake
- image drift between environments

---

## 5.2 Managed disks, images, and extension behavior

### Important concept
Operational success on VMs often depends less on the VM resource itself and more on:
- image hygiene
- extension ordering
- disk persistence model
- bootstrap assumptions

### Best practices
- standardize golden image or image-pipeline strategy
- know what survives image update, OS replacement, or reprovisioning
- keep extension dependencies minimal
- validate extension versioning and upgrade impact

### Common mistakes
- relying on custom-script extension for everything
- image update assumed to preserve all runtime state
- extension failure hidden until scale-out event

---

## 5.3 Virtual Machine Scale Sets (VMSS)

### Technical purpose
Scale and manage a set of equivalent VMs with consistent model-driven configuration.

### Core entities
- scale set model
- instances
- upgrade policy
- health model
- orchestration behavior
- image version
- autoscale relationship
- extension set

### Best fit
- horizontally scalable VM-based services
- container hosts without AKS
- transitional modernized workloads still requiring VM semantics

---

## 5.4 Upgrade policy modes

Microsoft documents VMSS upgrade policy modes: automatic, manual, and rolling. [2]

### Automatic
Fastest, least operator-controlled.

### Manual
You choose when instances update.

### Rolling
Controlled batch-wise update with more availability awareness.

### Best practices
- use rolling where production safety matters
- define rollout windows and instance batch expectations
- understand application dependency on load balancer and health state
- validate rollback model before enabling automatic behavior

### Common mistakes
- automatic upgrades used in production with weak health signals
- manual mode used but operational team forgets to apply updates
- rolling mode assumed safe without app-level readiness design

---

## 5.5 Application Health extension

Microsoft documents the Application Health extension for VMSS and VMs. [1][6]

### Technical purpose
Expose application health, not merely VM liveness.

### Best practices
- probe actual service readiness, not process existence only
- align health reporting with load balancer and upgrade behavior
- use health signal as deployment gate for rolling operations

### Common mistakes
- health endpoint always returns healthy
- health extension reports up while dependency chain is broken
- upgrade stalls or causes brownouts because app readiness is misrepresented

---

## 5.6 Automatic OS image upgrades and extension upgrades

Microsoft documents automatic OS image upgrade batch behavior and automatic extension upgrades. [3][4]

### Important concept
OS image upgrade is not a patch in place; it replaces the OS disk using a new image version while retaining data disks. [3]

### Best practices
- design workloads to tolerate image replacement
- preserve state correctly on data disks or external services
- test extension compatibility with new image versions
- ensure load balancer health integration is accurate before rollout

### Common mistakes
- local state unexpectedly lost across image-based upgrade behavior
- extension version change breaks bootstrapping
- teams think “automatic” means “safe without testing”

---

## 5.7 VM / VMSS troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| VM boots but app never comes up | bootstrap/runtime | image config, extensions, managed identity, dependency reachability |
| VMSS rollout stalls | health/upgrade | app health extension, load balancer probe, upgrade policy, failed instance batch |
| New instances fail only during scale-out | image/extension drift | image version, startup scripts, secret retrieval, extension timing |
| Upgrade succeeded but service regressed | rollout design | health endpoint fidelity, rolling batch size, external dependency readiness |
| Extension keeps failing | extension lifecycle | version compatibility, permissions, script assumptions, network path |

---

# 6. Azure Kubernetes Service (AKS)

Microsoft documents AKS node pools, system node pools, network policies, and Azure Policy for Kubernetes. [7][8][9][10]

## 6.1 Technical purpose

Use AKS when you need:
- Kubernetes-native orchestration
- multi-service container scheduling
- cluster-level control
- policy-backed workload governance
- ecosystem flexibility

## 6.2 Core entities

### Cluster
Control boundary for Kubernetes workloads.

### Node pool
Group of nodes with shared characteristics.

### System node pool
Hosts core cluster services and required system pods.

### User node pool
Hosts application workloads.

### Workload identity
Identity mechanism for pods and services to access Azure resources securely.

### Network policy
Pod-to-pod traffic restriction framework.

### Add-on/policy integration
Azure-native governance and operational extensions applied to cluster.

---

## 6.3 Node pool architecture

Microsoft documents multiple node pools and system vs user node pools. [8][11]

### Best practices
- isolate system workloads from application workloads
- use separate user pools for materially different workload classes
- align pool design to OS, SKU, scaling pattern, and scheduling constraints
- keep node-pool sprawl under control

### Common mistakes
- all workloads on default pool
- system services and noisy app workloads compete for same nodes
- node pools created for every team with no scheduling model

### Expert nuance
Node pools are not just scaling buckets.  
They are:
- operational boundaries
- cost boundaries
- upgrade boundaries
- sometimes blast-radius boundaries

---

## 6.4 Workload identity

### Technical purpose
Allow Kubernetes workloads to access Azure resources without static secrets.

### Best practices
- prefer workload identity over embedded credentials
- document service-account-to-identity mapping
- grant least privilege at target Azure scope
- validate token and role assumptions during deployment

### Common mistakes
- pod identity model not understood by application teams
- identity exists, but RBAC scope is wrong
- teams fall back to connection strings because workload identity troubleshooting is weak

---

## 6.5 Network policy

Microsoft documents AKS network policies to restrict pod traffic using least privilege. [7]

### Best practices
- define default-deny posture where appropriate
- model app dependency flows explicitly
- test policy changes in nonproduction with realistic traffic
- keep platform and app team responsibilities clear

### Common mistakes
- network policies enabled with no service map
- DNS or control-plane-related flows unintentionally blocked
- policy exists but nobody can explain why a pod can or cannot connect

---

## 6.6 Azure Policy for Kubernetes

Microsoft documents Azure Policy integration using OPA/Rego and built-in definitions for AKS. [9][10]

### Technical purpose
Govern cluster and workload configuration through policy.

### Best practices
- use built-ins first where suitable
- separate preventive and audit policy intent
- review policy impact on developer workflow before broad enforcement
- align cluster governance with landing-zone governance

### Common mistakes
- policy enabled without exception process
- developers discover policy only when deploys fail
- cluster governance and Azure Policy ownership split across teams with no coordination

---

## 6.7 AKS troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Pod can’t access Azure resource | identity/authorization | workload identity mapping, token audience, Azure RBAC scope |
| App traffic broken after security hardening | network policy | allowed flows, DNS egress, namespace/pod selector logic |
| Cluster upgrades hurt workload stability | node-pool design | system vs user pool separation, disruption tolerance, scheduling constraints |
| Policy blocks deployment unexpectedly | governance | Azure Policy assignment, built-in definition, admission behavior, exception path |
| Scaling works unevenly | pool design | node-pool sizing, system-pool pressure, workload placement, quotas |

---

# 7. App Service, Functions, and Container Apps

These services overlap enough that selection quality matters.

Microsoft documents App Service deployment slots, App Service VNet integration, Functions deployment slots and networking options, and Azure Container Apps revisions, ingress, secrets, jobs, and Functions on Container Apps. [12][13][14][15][16][17][18][19][20]

## 7.1 App Service

### Technical purpose
Managed hosting for web apps and APIs with strong PaaS abstraction.

### Core strengths
- deployment slots
- managed platform operations
- integrated web-app hosting model
- reduced host-management burden compared with VMs or Kubernetes

### Best practices
- use slots for safe deployment promotion
- externalize config
- understand networking mode and outbound behavior
- document slot-specific and sticky settings carefully

### Common mistakes
- slot swap used without understanding configuration stickiness
- web app depends on local mutable state
- VNet integration assumed to behave like inbound private placement

---

## 7.2 Deployment slots

Microsoft documents App Service and Functions deployment slots as low-downtime staging/swap patterns. [12][14]

### Technical purpose
Support staged rollout, validation, and rollback.

### Best practices
- warm and validate staging before swap
- understand slot settings vs shared settings
- use slots as rollout control, not as long-term environment sprawl
- monitor both source and target slot health before swap

### Common mistakes
- slot tested with different config than production-equivalent settings
- swap introduces runtime break due to secrets, hostname, or connection behavior
- staging slot becomes semi-permanent parallel environment with drift

---

## 7.3 VNet integration

Microsoft documents App Service VNet integration and Functions networking options. [13][15]

### Important concept
Outbound VNet integration is not the same as inbound private hosting.

### Best practices
- understand whether you are solving outbound reachability, inbound privacy, or both
- map private endpoint and DNS strategy separately if needed
- test egress to dependencies from the app runtime, not only from network diagrams

### Common mistakes
- app can reach VNet resources, but users still enter via public ingress unexpectedly
- teams confuse VNet integration with App Service Environment style isolation assumptions

---

## 7.4 Azure Functions

### Technical purpose
Event-driven or trigger-driven execution model.

### Best fit
- event handling
- queue/timer/http trigger workloads
- bursty execution patterns
- function-level scaling model

### Best practices
- keep function boundaries clean
- understand hosting and networking mode
- use deployment slots where supported and appropriate
- monitor cold-start or initialization sensitivity for user-facing patterns

### Common mistakes
- long-running workload forced into Functions
- slot strategy ignored until production deployment risk appears
- networking expectations borrowed from ordinary web app model

---

## 7.5 Azure Container Apps

### Technical purpose
Managed container application platform with:
- revisions
- traffic splitting
- scale-to-zero patterns
- secrets
- ingress control
- jobs for finite-duration execution

### Core entities
- container app
- revision
- environment
- ingress
- secret
- job
- scaling rule
- runtime identity/networking context

---

## 7.6 Revisions and traffic splitting

Microsoft documents Container Apps revisions and revision management. [16][17]

### Technical purpose
Controlled rollout and rollback for containerized services.

### Best practices
- treat revisions as rollout primitives
- use traffic splitting deliberately
- document revision-retention and cleanup patterns
- validate environment variables and secret references per revision

### Common mistakes
- new revision created, but no one knows which traffic percentage is active
- secrets changed but revision behavior not revalidated
- rollout assumed safe because platform supports revisions, while app readiness is weak

---

## 7.7 Ingress, secrets, and jobs

Microsoft documents Container Apps ingress, secret management, and jobs. [18][19][20]

### Best practices
- align ingress mode with service exposure model
- govern secrets centrally and rotate intentionally
- use jobs for finite tasks, not long-running service semantics
- separate app-runtime concerns from batch-processing concerns

### Common mistakes
- service-style workload implemented as a job
- secrets updated but stale revision still active
- ingress open broader than intended
- Functions-on-Container-Apps chosen without understanding both abstractions

---

## 7.8 App Service / Functions / Container Apps troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Slot swap caused outage | config/runtime | slot settings, warm-up behavior, hostname/dependency assumptions |
| App can’t reach private dependency | networking | VNet integration, DNS, private endpoint path, outbound restrictions |
| New Container Apps revision healthy but traffic fails | ingress/revision | active revision mode, traffic split, probe/readiness, secret refs |
| Function deployment works but runtime calls fail | hosting/networking | trigger config, app settings, identity, networking mode |
| Job completes unexpectedly or not at all | workload fit | Container Apps jobs config, finite-duration logic, scaling trigger design |

---

# 8. Azure Container Registry (ACR)

Microsoft documents ACR tasks, webhooks, and private endpoints. [21][22][23][24]

## 8.1 Technical purpose

Use ACR for:
- container image and artifact storage
- secure private registry patterns
- build/task automation
- event-driven downstream reactions
- private access to registry endpoints

## 8.2 Core entities

### Registry
Top-level ACR resource.

### Repository
Logical image grouping.

### Tag
Mutable or semantically meaningful reference.

### Task
Server-side build/automation capability.

### Webhook
Event hook for push/delete and related registry actions.

### Private endpoint
Private registry access path.

---

## 8.3 Registry architecture and pull path

### Best practices
- separate registry access design from runtime deployment design
- use managed identity or trusted auth path for pull where possible
- define repository and tagging strategy clearly
- use private endpoints when registry access must remain private

### Common mistakes
- image can be pushed from CI but runtime cannot pull
- registry private endpoint exists, but DNS/network path for nodes is broken
- latest-tag-only strategy destroys deployment traceability

---

## 8.4 ACR Tasks and webhooks

Microsoft documents ACR Tasks and webhooks. [21][22]

### Technical purpose
Automate build or downstream actions when registry events happen.

### Best practices
- use tasks for standardized build logic close to registry lifecycle
- use webhooks to trigger downstream deployment or audit logic carefully
- keep build provenance and artifact immutability in view

### Common mistakes
- webhook chain creates accidental deployment storms
- image tag mutation reused with no content traceability
- tasks treated as full CI replacement when governance needs more context

---

## 8.5 ACR troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Runtime can’t pull image | auth/network | managed identity or secret path, private endpoint DNS, firewall/network path |
| Image pull works in one environment only | governance | repo permissions, VNet/DNS differences, private endpoint linkage |
| Build succeeded but wrong image deployed | artifact discipline | tag strategy, digest tracking, webhook/task trigger logic |
| Registry access breaks after privatization | DNS/private link | private zone records, client resolver path, allowed network path |

---

# 9. Azure DevOps, Azure Pipelines, and GitHub Actions

Week 3 is not only runtime. It is also change control.

Microsoft documents Azure Pipelines service connections, environments, approvals/checks, variable groups, resources, and GitHub Actions Azure authentication with OIDC or secrets. [25][26][27][28][29][30][31]

## 9.1 Azure DevOps and Azure Pipelines

### Technical purpose
Provide:
- CI/CD orchestration
- approval and deployment governance
- environment tracking
- resource-based controls
- reusable pipeline infrastructure

### Core entities
- pipeline
- service connection
- environment
- approval/check
- variable group
- agent pool
- secure file
- resource

---

## 9.2 Service connections

Microsoft documents service connections as secure connectivity objects to external systems. [25]

### Best practices
- minimize privilege of service connections
- separate build and deploy identities where useful
- treat service connections as governed resources, not pipeline-local conveniences
- review ownership regularly

### Common mistakes
- one highly privileged Azure service connection reused everywhere
- no rotation or lifecycle review
- teams cannot tell which pipelines rely on which connection

---

## 9.3 Environments, approvals, and checks

Microsoft documents deployment approvals/checks and environments. [26][27]

### Technical purpose
Move deployment control from YAML alone into governed resources.

### Best practices
- use environments for deployment-history visibility and target governance
- attach approvals/checks at resource level
- align approval path to environment criticality
- use environment protections as part of release architecture

### Common mistakes
- approvals embedded only in pipeline code, bypassing resource-owner control
- nonproduction and production share same service connection and no staged checks
- too many manual approvals create bottlenecks and emergency bypass habits

---

## 9.4 Variable groups and resource model

Microsoft documents variable groups and pipeline resources as protected resources with auditability. [28][29]

### Best practices
- treat secret material as protected resources
- centralize shared configuration where governance benefits outweigh convenience cost
- scope variables and secrets carefully
- document dependency between pipelines and protected resources

### Common mistakes
- variable group becomes a dumping ground
- production secrets exposed to pipelines that do not need them
- teams debug release issues without checking protected-resource checks

---

## 9.5 GitHub Actions for Azure

Microsoft documents authenticating from GitHub Actions to Azure by OIDC or by secret, and Key Vault integration. [30][31][32][33]

### Best practices
- prefer OIDC over long-lived client secrets
- retrieve secrets from Key Vault rather than embedding them in workflow secrets when feasible
- align environment protection rules with deployment criticality
- separate repository-level trust from environment-level trust

### Common mistakes
- long-lived service principal secret retained because “it already works”
- GitHub environment protections weaker than Azure-side expectations
- workflow can deploy but cannot perform runtime checks because RBAC scope is incomplete

---

## 9.6 Pipeline troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Pipeline can build but can’t deploy | auth/authorization | service connection, OIDC federation, Azure RBAC scope |
| Deployment blocked unexpectedly | governance | environment checks, approval rules, resource ownership |
| Secret retrieval fails in workflow | secret path | Key Vault access, federated identity, variable-group or env protection scope |
| Prod release bypass pressure grows | process design | too many approvals, unclear ownership, missing progressive delivery model |
| Same YAML behaves differently by environment | protected resources | variable groups, service connections, environment checks, target identity |

---

# 10. ARM Templates, Bicep, and Deployment Discipline

Microsoft documents Bicep modules and what-if change preview. [34][35][36]

## 10.1 Technical purpose

Use ARM/Bicep to:
- declare desired infrastructure state
- make deployment repeatable
- standardize module boundaries
- preview change impact
- reduce manual drift

## 10.2 Core entities

### Template / Bicep file
Declarative deployment specification.

### Module
Composable deployment unit.

### Parameter
Deployment-time input.

### Scope
Tenant, management group, subscription, or resource group deployment target.

### What-if
Predicted deployment changes before apply.

---

## 10.3 Modules

Microsoft documents Bicep modules for encapsulating complex details and improving readability. [34]

### Best practices
- define module boundaries around reusable platform concepts
- keep inputs/outputs stable and meaningful
- avoid giant monolithic Bicep files
- version module contracts intentionally

### Common mistakes
- every deployment becomes one huge file
- module boundaries reflect repo ownership, not platform design
- too many parameters with weak validation and no defaults strategy

---

## 10.4 What-if discipline

Microsoft documents what-if for previewing deployment changes. [35][37]

### Technical purpose
Reduce surprise before deployment.

### Best practices
- use what-if routinely before production changes
- review destructive or unexpected change types explicitly
- understand what-if limitations and do not treat it as perfect oracle
- include what-if output in deployment review flow where practical

### Common mistakes
- what-if skipped on “small” change
- operator sees change but does not understand cascade implications
- what-if result ignored because it looks noisy

---

## 10.5 Deployment scope and platform design

### Best practices
- choose deployment scope intentionally
- align Bicep deployment structure to Week 1 hierarchy decisions
- separate platform baseline modules from workload modules
- document which team owns which deployment scope

### Common mistakes
- workload repo tries to manage shared platform scope
- subscription or management-group level deployments hidden in app pipeline
- resource drift blamed on Azure when it is actually scope confusion

---

## 10.6 IaC troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Deployment preview looks wrong | module/scope | parameter values, target scope, module defaults, resource IDs |
| Same module works in dev not prod | governance/scope | policy, RBAC, naming constraints, existing-state differences |
| Infra drift keeps returning | process model | portal/manual changes, missing IaC ownership, weak deployment discipline |
| Prod change caused deletion risk | what-if review | change type, module refactor side effects, scope mismatch |

---

# 11. Azure Deployment Environments (ADE)

Microsoft documents Azure Deployment Environments, its overview, key concepts, quickstarts, and environment management. [38][39][40][41][42]

## 11.1 Technical purpose

Use ADE to provide:
- self-service environment provisioning
- project-based developer environments
- catalog-backed templates
- governance with platform-approved infrastructure definitions

## 11.2 Core entities

### Dev center
Organizing control plane for development infrastructure experience.

### Project
Boundary for teams or scenarios consuming environments.

### Catalog
Source of approved IaC templates.

### Environment definition
Template-backed environment specification.

### Environment
Instantiated deployment for developer or team use.

## 11.3 Best practices
- use ADE to standardize self-service, not bypass governance
- keep catalog quality high
- align environment templates with platform standards
- define who can request, approve, modify, and delete environments

## 11.4 Common mistakes
- catalog turns into ungoverned template pile
- developers expect ADE to replace all CI/CD and platform workflows
- template governance weaker than landing-zone policy model

## 11.5 ADE troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Developer can’t provision environment | access/catalog | project permissions, catalog config, template availability |
| Environment template behaves inconsistently | template governance | version drift, missing standards, parameter assumptions |
| Self-service causes governance drift | operating model | weak catalog review, no platform ownership, unclear lifecycle policy |

---

# 12. Azure Batch

Microsoft documents Azure Batch pools, nodes, and autoscale behavior and events. [43][44][45][46]

## 12.1 Technical purpose

Use Azure Batch for:
- high-scale parallel job execution
- compute-heavy task orchestration
- finite work queues
- elastic pool-based processing

## 12.2 Core entities

### Account
Top-level Batch boundary.

### Pool
Collection of compute nodes.

### Node
Worker VM for task processing.

### Job
Logical group of tasks.

### Task
Unit of work.

### Start task
Preparation command on nodes before workload tasks.

### Autoscale formula
Expression controlling pool size dynamically.

## 12.3 Best practices
- distinguish service workload from job workload
- use autoscale based on real workload signals
- keep start-task assumptions minimal and reproducible
- review pool-image and dependency behavior regularly

## 12.4 Common mistakes
- Batch chosen for long-lived web-service semantics
- autoscale formula too aggressive or too slow
- start task becomes hidden configuration management layer
- cost surprises due to idle node retention

## 12.5 Batch troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Jobs queue but don’t progress | pool capacity | autoscale formula, pool allocation, quotas, node health |
| Tasks fail only on fresh nodes | bootstrap | start task, image dependencies, package availability |
| Autoscale behaves strangely | formula/model | evaluation interval, signals, min/max logic, event logs |
| Batch cost too high | scaling discipline | idle nodes, formula aggressiveness, workload batching pattern |

---

# 13. Azure Machine Learning and Azure OpenAI

Microsoft documents Azure Machine Learning endpoints, online endpoints, batch endpoints, deployment guidance, and Azure OpenAI quota, limits, networking, and content filters. [47][48][49][50][51][52][53][54]

## 13.1 Azure Machine Learning

### Technical purpose
Platform for model lifecycle and managed inference deployment.

### Core entities
- workspace
- model
- endpoint
- deployment
- online endpoint
- batch endpoint
- compute/managed runtime context
- model registry or artifact flow depending on architecture

---

## 13.2 Online endpoints

Microsoft documents online endpoints as stable real-time inference URLs. [47][48][50]

### Technical purpose
Low-latency or request/response inference.

### Best practices
- use online endpoints for real-time serving
- treat deployment rollout like application rollout
- validate traffic split or blue/green pattern where supported
- align identity, network, and quota expectations before launch

### Common mistakes
- online endpoint used for heavy asynchronous scoring
- model deploys, but dependency packaging or auth path fails at inference time
- no rollback plan for endpoint revision change

---

## 13.3 Batch endpoints

Microsoft documents batch endpoints for asynchronous large-volume inferencing. [49][51]

### Technical purpose
Long-running or large-volume scoring.

### Best practices
- use when throughput and volume matter more than interactive latency
- separate online-serving and batch-processing economics
- validate data-input and output path carefully
- monitor batch backlog and completion semantics

### Common mistakes
- batch endpoint chosen for user-facing low-latency API
- output storage and downstream consumption not designed
- online/batch endpoint distinction ignored in product requirements

---

## 13.4 Azure OpenAI

Microsoft documents Azure OpenAI quota and limits, content filters, and private networking guidance. [52][53][54]

### Technical purpose
Managed frontier-model access for generative AI applications.

### Core concerns
- quota and rate limits
- responsible AI/content filters
- network exposure model
- deployment naming and environment discipline
- application-level fallback behavior

### Best practices
- design quota and concurrency around expected usage
- understand content-filter behavior and operational implications
- use private networking where security model requires it
- monitor model usage and deployment-specific limits

### Common mistakes
- one deployment used for every workload with no quota planning
- application treats filtered or rate-limited response as generic outage
- private endpoint enabled but app networking and DNS path not validated

---

## 13.5 ML / AI troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Endpoint deployed but inference fails | packaging/auth | model dependencies, scoring environment, endpoint identity, network path |
| Real-time latency too high | endpoint fit | online vs batch mismatch, scaling, model size, downstream calls |
| Batch jobs complete but outputs unusable | data contract | input schema, output location, downstream expectations |
| Azure OpenAI requests throttled | quota | deployment quota, concurrency, retry design, traffic shaping |
| AI app “fails randomly” | safety/network | content filters, private endpoint DNS, quota bursts, client retry logic |

---

# 14. End-to-End Platform Delivery Example

An enterprise platform supports:
- legacy web app still on VMs
- new APIs on App Service
- event-driven jobs on Functions
- internal microservices on AKS
- experimental containerized workers on Container Apps
- all images in ACR
- Bicep for infra
- Azure DevOps for controlled production releases
- GitHub Actions for app-repo CI
- Azure ML for recommendation endpoint
- Azure OpenAI for assistant features

A strong Week 3 design could look like this:

### Runtime choice
- VMSS for horizontally scaled VM-based legacy service
- App Service for core public API
- Functions for event triggers
- AKS for cluster-native services
- Container Apps jobs for finite worker tasks

### Artifact flow
- images built into ACR
- immutable tag + digest discipline
- runtime pull via managed identity/private path where required

### Deployment
- GitHub Actions for build/test and artifact publish
- Azure DevOps environments and approvals for promoted production rollout
- slots for App Service/Functions
- revisions for Container Apps
- rolling policy with health extension for VMSS

### IaC
- Bicep modules by platform domain
- what-if in pre-prod and prod approval flow
- ADE for developer self-service environments

### ML/AI
- online endpoint for real-time model scoring
- batch endpoint for scheduled large-volume jobs
- Azure OpenAI deployment with quota-aware app design and content-filter-aware handling

This is the Week 3 principle:

> compute, artifacts, rollout, and governance are one system.

---

# 15. Troubleshooting Flow

When Week 3 delivery systems fail, inspect in this order:

1. **Runtime truth**  
   Which platform is actually hosting the workload?

2. **Artifact truth**  
   Which image/package/model version was deployed?

3. **Identity truth**  
   Which runtime identity is calling which Azure dependency?

4. **Deployment truth**  
   Was this change delivered through slot, revision, rolling upgrade, direct deploy, or pipeline stage?

5. **Governance truth**  
   Did approval, policy, or environment protection alter the rollout?

6. **Health truth**  
   What probe, extension, revision health, or endpoint readiness signal gated traffic?

7. **Rollback truth**  
   Is the rollback model slot-swap, revision traffic shift, prior image redeploy, previous model deployment, or scale-set model rollback?

---

# 16. Common Week 3 Mistakes

- runtime chosen by team familiarity instead of workload fit
- VM bootstrapping hidden in extensions and scripts with no reproducibility
- VMSS health design weaker than rollout automation
- AKS node-pool sprawl with no scheduling model
- workload identity ignored in favor of secrets
- App Service slots used without config discipline
- Container Apps revisions created without traffic strategy
- ACR tags mutable and deployment traceability weak
- highly privileged pipeline identities reused everywhere
- GitHub Actions still using static secrets instead of OIDC
- Bicep modules too monolithic or too fragmented
- what-if skipped on production changes
- self-service environments created without template governance
- batch and online inference use cases mixed together
- Azure OpenAI quota and filtering behavior not designed into app logic

---

# 17. Revision Checklist

You should be able to explain clearly:

- when to use VMs vs VMSS vs AKS vs App Service vs Functions vs Container Apps vs Batch
- how VMSS upgrade policy and health design affect availability
- how AKS node pools, workload identity, network policy, and Azure Policy fit together
- how App Service slots and Container Apps revisions support rollout safety
- how ACR pull, private access, tasks, and webhook behavior affect artifact delivery
- how Azure DevOps environments, approvals, service connections, and variable groups support deployment governance
- how GitHub Actions OIDC improves Azure authentication posture
- how Bicep modules, deployment scope, and what-if support safe infrastructure delivery
- how ADE supports self-service without surrendering governance
- how online endpoints, batch endpoints, and Azure OpenAI differ in runtime and operational model

---

# 18. Memory Anchors

- **VM** = full OS control
- **VMSS** = horizontally scaled VM fleet
- **rolling upgrade** = controlled batch VMSS update
- **application health extension** = app-level VM/VMSS health signal
- **AKS** = Kubernetes platform
- **system node pool** = core cluster services
- **user node pool** = application workloads
- **workload identity** = pod-to-Azure identity without static secrets
- **App Service** = managed web/API hosting
- **slot** = staged deployment target
- **Functions** = event-driven compute
- **Container Apps** = managed container runtime with revisions
- **job** = finite-duration container task
- **ACR** = private registry and artifact hub
- **service connection** = pipeline credential boundary
- **environment** = deployment target and approval boundary
- **OIDC** = secretless federated auth from GitHub Actions
- **Bicep module** = reusable IaC unit
- **what-if** = deployment preview
- **ADE** = self-service environment provisioning with governed templates
- **Batch** = large-scale task execution
- **online endpoint** = real-time ML inference
- **batch endpoint** = asynchronous large-volume inference
- **Azure OpenAI** = managed foundation-model serving with quota and safety controls

---

# 19. Official References

## Virtual Machines and VM Scale Sets
- Use Application Health extension with VM Scale Sets  
  https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-health-extension

- Upgrade policy modes for VM Scale Sets  
  https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-policy

- Automatic OS image upgrades for VM Scale Sets  
  https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade

- Automatic Extension Upgrade for VMs and scale sets  
  https://learn.microsoft.com/en-us/azure/virtual-machines/automatic-extension-upgrade

- Application Health extension for Azure VMs  
  https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/health-extension

## AKS
- Create multiple node pools in AKS  
  https://learn.microsoft.com/en-us/azure/aks/create-node-pools

- Manage system node pools in AKS  
  https://learn.microsoft.com/en-us/azure/aks/use-system-pools

- Secure pod traffic with network policies in AKS  
  https://learn.microsoft.com/en-us/azure/aks/use-network-policies

- Azure Policy for Kubernetes  
  https://learn.microsoft.com/en-us/azure/governance/policy/concepts/policy-for-kubernetes

- AKS policy built-in definitions  
  https://learn.microsoft.com/en-us/azure/aks/policy-reference

## App Service, Functions, Container Apps
- App Service deployment slots  
  https://learn.microsoft.com/en-us/azure/app-service/deploy-staging-slots

- Enable VNet integration in App Service  
  https://learn.microsoft.com/en-us/azure/app-service/configure-vnet-integration-enable

- Azure Functions deployment slots  
  https://learn.microsoft.com/en-us/azure/azure-functions/functions-deployment-slots

- Azure Functions networking options  
  https://learn.microsoft.com/en-us/azure/azure-functions/functions-networking-options

- Azure Functions on Azure Container Apps overview  
  https://learn.microsoft.com/en-us/azure/container-apps/functions-overview

- Azure Container Apps revisions  
  https://learn.microsoft.com/en-us/azure/container-apps/revisions

- Manage revisions in Azure Container Apps  
  https://learn.microsoft.com/en-us/azure/container-apps/revisions-manage

- Manage secrets in Azure Container Apps  
  https://learn.microsoft.com/en-us/azure/container-apps/manage-secrets

- Azure Container Apps ingress overview  
  https://learn.microsoft.com/en-us/azure/container-apps/ingress-overview

- Azure Container Apps jobs  
  https://learn.microsoft.com/en-us/azure/container-apps/jobs

## Container Registry
- Azure Container Registry documentation  
  https://learn.microsoft.com/en-us/azure/container-registry/

- Set up private endpoint with Private Link for ACR  
  https://learn.microsoft.com/en-us/azure/container-registry/container-registry-private-endpoints

- ACR webhooks  
  https://learn.microsoft.com/en-us/azure/container-registry/container-registry-webhook

## CI/CD and IaC
- Service connections in Azure Pipelines  
  https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops

- Create and target Azure DevOps environments  
  https://learn.microsoft.com/en-us/azure/devops/pipelines/process/environments?view=azure-devops

- Pipeline deployment approvals and checks  
  https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals?view=azure-devops

- Variable groups in Azure Pipelines  
  https://learn.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups?view=azure-devops

- Resources in Azure Pipelines  
  https://learn.microsoft.com/en-us/azure/devops/pipelines/process/about-resources?view=azure-devops

- Authenticate to Azure from GitHub Actions by OIDC  
  https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure-openid-connect

- Authenticate to Azure from GitHub Actions by a secret  
  https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure-secret

- Use Azure Key Vault secrets in GitHub Actions  
  https://learn.microsoft.com/en-us/azure/developer/github/github-actions-key-vault

- Bicep documentation  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/

- Bicep modules  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/modules

- Bicep what-if  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deploy-what-if

- Deployment stacks what-if  
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deployment-stacks-what-if

## Deployment Environments and Batch
- What is Azure Deployment Environments?  
  https://learn.microsoft.com/en-us/azure/deployment-environments/overview-what-is-azure-deployment-environments

- Key concepts and roles for Deployment Environments  
  https://learn.microsoft.com/en-us/azure/deployment-environments/concept-environments-key-concepts

- Quickstart: set up Azure Deployment Environments  
  https://learn.microsoft.com/en-us/azure/deployment-environments/quickstart-create-and-configure-devcenter

- Create a Deployment Environment  
  https://learn.microsoft.com/en-us/azure/deployment-environments/quickstart-create-access-environments

- Manage your environment  
  https://learn.microsoft.com/en-us/azure/deployment-environments/how-to-manage-environments

- Azure Batch documentation  
  https://learn.microsoft.com/en-us/azure/batch/

- Nodes and pools in Azure Batch  
  https://learn.microsoft.com/en-us/azure/batch/nodes-and-pools

- Autoscale compute nodes in an Azure Batch pool  
  https://learn.microsoft.com/en-us/azure/batch/batch-automatic-scaling

- Batch pool autoscale event  
  https://learn.microsoft.com/en-us/azure/batch/batch-pool-autoscale-event

## Azure Machine Learning and Azure OpenAI
- Endpoints for inference in Azure Machine Learning  
  https://learn.microsoft.com/en-us/azure/machine-learning/concept-endpoints?view=azureml-api-2

- Online endpoints for real-time inference  
  https://learn.microsoft.com/en-us/azure/machine-learning/concept-endpoints-online?view=azureml-api-2

- Deploy models to online endpoints  
  https://learn.microsoft.com/en-us/azure/machine-learning/how-to-deploy-online-endpoints?view=azureml-api-2

- Batch endpoints overview  
  https://learn.microsoft.com/en-us/azure/machine-learning/concept-endpoints-batch?view=azureml-api-2

- Deploy models to batch endpoints  
  https://learn.microsoft.com/en-us/azure/machine-learning/how-to-use-batch-model-deployments?view=azureml-api-2

- Manage Azure OpenAI quota  
  https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/quota

- Azure OpenAI quotas and limits  
  https://learn.microsoft.com/en-us/azure/foundry/openai/quotas-limits

- Configure Azure OpenAI content filters  
  https://learn.microsoft.com/en-us/azure/foundry-classic/openai/how-to/content-filters

- Azure OpenAI private networking guidance  
  https://learn.microsoft.com/en-us/azure/foundry-classic/openai/how-to/network

---

# 20. Final Summary

Week 3 is about **running Azure workloads on the right platform and delivering change with control**.

The strongest mental model is:

- **VM + VMSS** = OS-level compute and scaled VM fleets
- **AKS** = Kubernetes control and policy-aware container orchestration
- **App Service + Functions + Container Apps** = managed application runtimes
- **ACR** = image and artifact control point
- **Azure DevOps + GitHub Actions** = change orchestration and delivery governance
- **ARM/Bicep + ADE** = repeatable environment creation
- **Batch + Azure ML + Azure OpenAI** = job execution and AI/ML serving models

When you can explain the **runtime model, artifact model, rollout model, identity model, and inference model** together, Azure Week 3 is understood architecturally.
