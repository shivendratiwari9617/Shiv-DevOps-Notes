# GCP Study Notes — Week 3 v2
## Compute, GKE, Serverless Runtimes, DevOps, Supply Chain, Eventing, and AI Platform

> This is the **9.5+ version** of Week 3.  
> It is written as a **deep service-by-service technical guide** with:
> - technical purpose
> - core entities
> - runtime mechanics
> - architecture usage
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** concise wording, high information density, minimal filler.

---

# 1. Why Week 3 Matters

Week 3 is about **where workloads run and how they are delivered safely**.

If:
- Week 1 = governance and control plane
- Week 2 = networking and connectivity

then Week 3 = **execution, packaging, rollout, and runtime operations**.

This week is where platform decisions become concrete:

- Which execution model fits the workload?
- How much control do you need over kernel, node, runtime, or request lifecycle?
- How are images built, scanned, promoted, and trusted?
- How are rollouts staged and recovered?
- How do eventing and orchestration glue services together?
- How do ML and generative AI workloads fit into the same platform model?

The core idea is:

> **Execution model choice determines your operational burden, scaling behavior, identity model, artifact strategy, and deployment controls.**

---

# 2. Learning Outcomes

By the end of Week 3, you should be able to:

- explain **Compute Engine** instances, machine families, disks, images, machine images, metadata, startup scripts, host maintenance considerations, OS Login, Shielded VM, sole-tenant nodes, and reservations
- explain **managed instance groups (MIGs)**, autoscaling, autohealing, rolling updates, stateful patterns, and instance templates
- explain **GKE** Standard vs Autopilot, node pools, release channels, private clusters, Gateway API, Workload Identity Federation for GKE, and upgrade/disruption considerations
- explain **Cloud Run** services, revisions, jobs, concurrency, min/max instances, ingress, service identity, VPC connectivity considerations, and container contract behavior
- explain where **Cloud Run functions**, **App Engine**, and **Batch** fit
- explain **Artifact Registry**, vulnerability scanning, metadata, cleanup policies, and artifact promotion patterns
- explain **Binary Authorization**, attestors, attestations, and deploy-time policy enforcement
- explain **Cloud Build** triggers, configs, steps, private worker pools, approvals, substitutions, and service accounts
- explain **Cloud Deploy** delivery pipelines, targets, releases, rollouts, and Skaffold-based rendering
- explain **Infrastructure Manager**, **Config Connector**, and **Config Controller**
- explain **Cloud Scheduler**, **Eventarc**, and **Workflows**
- explain key **Vertex AI**, **Gemini via Vertex AI**, **Cloud TPU**, and **BigQuery ML** entities
- troubleshoot startup failures, rollout failures, image pull failures, cluster upgrade issues, broken triggers, and model deployment failures

---

# 3. Architectural Mental Model

Think in layers:

1. **Execution layer**  
   Compute Engine, GKE, Cloud Run, functions, App Engine, Batch

2. **Artifact layer**  
   Artifact Registry, images, packages, tags, metadata, machine images

3. **Trust layer**  
   scanning, provenance, attestations, Binary Authorization

4. **Build layer**  
   Cloud Build

5. **Release layer**  
   Cloud Deploy

6. **Infrastructure definition layer**  
   Infrastructure Manager, Terraform, Config Connector, Config Controller

7. **Event and orchestration layer**  
   Scheduler, Eventarc, Workflows

8. **AI/ML platform layer**  
   Vertex AI, Gemini, TPU, BigQuery ML

When delivery fails, the root cause usually belongs to one of these layers, not all of them.

---

# 4. Execution Model Selection

Before individual services, know the selection logic.

## 4.1 Compute Engine
Choose when you need:
- VM-level OS control
- custom agent/kernel/runtime behavior
- legacy software patterns
- special hardware or specialized licensing

## 4.2 GKE
Choose when you need:
- Kubernetes platform patterns
- multi-service container orchestration
- node/pod scheduling control
- service mesh, policy, and platform standardization patterns

## 4.3 Cloud Run
Choose when you need:
- request-driven containers
- very low ops burden
- fast service deployment
- elastic HTTP/gRPC services or finite jobs

## 4.4 App Engine
Choose mainly for:
- existing App Engine estates
- platform-managed app deployment models that already fit App Engine well

## 4.5 Batch
Choose when you need:
- finite parallel compute jobs
- managed job scheduling/execution semantics
- bursty batch without long-lived infrastructure

## 4.6 Rule of thumb
- **Need host control?** Compute Engine
- **Need Kubernetes?** GKE
- **Need serverless containers?** Cloud Run
- **Need finite large-scale job execution?** Batch
- **Have an App Engine-native app/platform fit?** App Engine

---

# 5. Compute Engine

Compute Engine is the foundational VM service.

## 5.1 Technical purpose

Use Compute Engine for:
- full VM lifecycle control
- OS and agent customization
- infrastructure patterns that require instance-level semantics
- legacy or stateful applications that do not fit serverless/container-first models

## 5.2 Core entities

### VM instance
Running virtual machine.

### Machine family / machine type
CPU-memory profile and hardware class.

### Disk
Persistent storage attached to instance.

### Image
Reusable boot disk template.

### Machine image
Multi-disk and instance-configuration capture object for clone/backup-like scenarios.

### Metadata
Key/value config surface for the instance.

### Startup script
Bootstrap action executed during startup.

### Service account
Google API identity for the workload.

### Reservation
Capacity reservation for future VM usage.

### Sole-tenant node
Dedicated host isolation option.

### Shielded VM
Hardening feature set for boot integrity and VM protection.

### OS Login
Identity-based SSH and login management model.

## 5.3 Machine families and workload fit

### General-purpose
Good default for most apps.

### Compute-optimized
Better for CPU-heavy stateless services.

### Memory-optimized
Better for in-memory systems, caches, and memory-heavy analytics.

### Accelerator-backed patterns
Used when GPU/TPU or specialized acceleration is genuinely needed.

### Best practices
- right-size from actual profiling
- separate workload classes
- review sustained under/over-utilization regularly

### Common mistakes
- choosing by habit
- lifting old VM sizes unchanged
- overprovisioning because no autoscaling or performance test exists

---

## 5.4 Disks, images, and machine images

### Persistent disks
Primary long-lived block storage model.

### Local SSD
High-performance ephemeral local storage where supported and justified.

### Image
Used to create boot disks repeatedly.

### Machine image
Captures broader instance shape including multiple disks and metadata-oriented cloning/recovery scenarios.

### Best practices
- standardize hardened images
- use image pipelines
- separate boot configuration from mutable app data
- understand what is ephemeral versus persistent

### Common mistakes
- storing critical state only in ephemeral local constructs
- manual VM drift after provisioning
- unclear golden-image ownership

---

## 5.5 Metadata and startup scripts

### Technical role
Metadata is a config/control surface; startup scripts handle bootstrap.

### Good pattern
Image contains most runtime dependencies.  
Startup script does only final environment-specific actions.

### Bad pattern
Startup script:
- installs everything
- downloads code live
- fetches secrets insecurely
- branches by environment unpredictably

### Best practices
- keep startup deterministic and short
- move heavy config into image pipeline
- never put secrets casually in metadata
- version startup logic

### Common mistakes
- mutable boot behavior
- hidden dependency on public downloads
- different instances drifting because bootstrap is nondeterministic

---

## 5.6 Service accounts, OS Login, and Shielded VM

### Service accounts
Provide workload identity.

### OS Login
Use centralized identity-based SSH/login instead of unmanaged instance-level key sprawl.

### Shielded VM
Use when you want stronger integrity defaults and boot trust.

### Best practices
- least privilege service accounts
- prefer OS Login over unmanaged SSH keys
- enable Shielded VM where compatible
- separate admin identity from workload identity

### Common mistakes
- one broad service account for many apps
- instance metadata SSH key sprawl
- security features disabled without real compatibility reason

---

## 5.7 Reservations and sole-tenant nodes

### Reservations
Protect capacity availability.

### Sole-tenant nodes
Use for licensing, noisy-neighbor sensitivity, or dedicated isolation requirements.

### Best practice
Use only when requirement is real.  
Both features add operational and financial design implications.

---

## 5.8 Compute Engine troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| VM boots but app never starts | bootstrap/runtime | startup script, service account permissions, logs, missing package/runtime |
| VM unreachable over SSH | identity/network | OS Login, firewall rules, instance status, IAP or bastion path |
| Instance recreated repeatedly in MIG | health/boot | health check, app readiness, startup timing, port binding |
| New image works in dev but fails in prod | config drift | metadata differences, missing secrets path, network dependencies, org policy |
| Capacity error during deploy | capacity/quota | quota, reservation coverage, zonal availability, machine type availability |

---

# 6. Managed Instance Groups (MIGs)

Google documents MIGs as managed fleets of identical VMs with autoscaling, autohealing, load balancing, and multi-zone support. [1]

## 6.1 Technical purpose

MIGs are the VM fleet control plane for:
- stateless app tiers
- standardized VM rollout
- autoscaled compute workers
- regional high-availability VM services

## 6.2 Core entities

### Instance template
Reusable VM definition.

### Managed instance group
Fleet manager for identical instances.

### Regional MIG
Spans multiple zones.

### Zonal MIG
Single-zone fleet.

### Autoscaler
Elastic instance-count controller.

### Autohealing policy
Health-check-based recreation policy.

### Update policy
Controls replacement during rollout.

### Stateful configuration
Stateful disk/IP preservation options in supported MIG patterns.

## 6.3 Instance templates

Google documents instance templates as reusable VM configuration definitions and notes deterministic instance templates and regional/global distinctions. [2][3]

### Best practices
- use immutable template versions
- name by app + version + environment
- keep templates deterministic
- never mutate fleet config by hand on instances

### Common mistakes
- one template reused for unrelated apps
- template names with no version meaning
- manual per-instance change after rollout

---

## 6.4 Autoscaling

### Signals
Can be based on utilization or other app-relevant signals depending on design.

### Best practices
- scale on the metric that reflects saturation
- use CPU only when CPU is actually the bottleneck
- pair autoscaling with capacity ceilings and backend readiness

### Common mistakes
- CPU autoscaling for queue workloads
- scaling policy without observing dependency bottlenecks
- no load test before production traffic

---

## 6.5 Autohealing

### Technical role
Replace unhealthy instances based on health checks.

### Best practices
- health check should reflect real readiness
- avoid app endpoints that return healthy before dependencies are ready
- tune initialization delays appropriately

### Common mistakes
- health checks trigger before boot completes
- autohealing loops caused by startup timing
- health endpoint always returns OK, hiding real failure

---

## 6.6 Rolling updates

### Technical role
Replace instances safely according to update policy.

### Best practices
- use staged rollout
- control surge and unavailable counts
- observe real backend health during update
- connect rollout to monitoring and rollback criteria

### Common mistakes
- replacing too many instances at once
- rollout with no app-level validation
- boot-time assumptions invalid in one zone only

---

## 6.7 Stateful MIG patterns

### Best fit
Use only if stateful VM semantics are truly needed.

### Common mistake
Using stateful MIGs when workload should be externalized to managed storage/data services.

---

## 6.8 MIG troubleshooting examples

### Symptom
Instances are created but immediately marked unhealthy.
**Inspect**
- startup script completion
- correct app port
- health-check path
- firewall path from load balancer/health-check source

### Symptom
Regional MIG scales unevenly across zones.
**Inspect**
- per-zone capacity
- subnet exhaustion
- quota
- image/boot dependency locality

### Symptom
Rolling update stalls.
**Inspect**
- health thresholds
- max unavailable / surge
- backend dependency failure
- app startup time regression

---

# 7. Google Kubernetes Engine (GKE)

GKE is the managed Kubernetes platform.

## 7.1 Technical purpose

Use GKE for:
- Kubernetes-native application platforms
- multi-service container scheduling
- service discovery and platform standardization
- teams that need Kubernetes-level abstractions rather than VM or simple serverless abstraction

## 7.2 Core entities

### Cluster
Top-level Kubernetes boundary.

### Control plane
Managed Kubernetes API/control components.

### Standard cluster
More node and infrastructure control.

### Autopilot cluster
More opinionated and managed operational model.

### Node pool
Group of worker nodes with shared characteristics.

### Pod
Smallest deployment/scheduling unit.

### Deployment / StatefulSet / DaemonSet / Job
Kubernetes workload controllers.

### Kubernetes Service
Stable internal access abstraction.

### Ingress
HTTP(S) exposure path in supported patterns.

### Gateway API
More expressive traffic model for Gateways and routes.

### Release channel
Upgrade/stability track.

### Maintenance policy
Upgrade timing control.

### Workload Identity Federation for GKE
Preferred workload-to-Google-API identity path.

---

## 7.3 Standard vs Autopilot

Google documents Autopilot as the more managed mode and notes Autopilot clusters are enrolled in release channels by default. [4]

### Standard
Use when you need:
- node-pool control
- host-level customization
- special daemon or runtime assumptions
- more custom scheduling/infra behavior

### Autopilot
Use when you want:
- less cluster infrastructure burden
- stronger opinionated platform defaults
- faster platform operations

### Best practice
Default to **Autopilot unless Standard is truly justified**.

### Common mistakes
- choosing Standard for prestige/control rather than need
- underestimating the operational burden of node pools, upgrades, networking, and disruption handling

---

## 7.4 Release channels and upgrades

Google documents release channels and notes non-enrolled clusters are deprecated and support removal is planned for **June 14, 2027**. [5]

### Technical role
Balance new features against stability.

### Best practices
- use release channels
- test in lower env first
- align channel to workload tolerance
- define maintenance windows and disruption expectations

### Common mistakes
- cluster upgrades without app-level validation
- unmanaged version drift
- no maintenance policy

---

## 7.5 Node pools, scheduling, and disruption

### Node pools
Use to separate workload classes.

### Scheduling signals
- labels
- taints/tolerations
- resource requests/limits
- affinity/anti-affinity

### Best practices
- keep node pools purposeful
- use taints for isolation where needed
- validate PodDisruptionBudgets and readiness before upgrades
- avoid excessive node-pool sprawl

### Common mistakes
- too many tiny node pools
- wrong workloads land on wrong nodes
- upgrade causes widespread restarts because disruption policy was weak

---

## 7.6 Workload Identity Federation for GKE

Google documents this as the way for GKE workloads to authenticate to Google APIs. [6]

### Technical purpose
Map Kubernetes service accounts to Google Cloud access without distributing long-lived service account keys.

### Best practices
- use by default
- separate service accounts per workload
- validate mapping in CI and staging
- remove node-level broad permissions where possible

### Common mistakes
- one Kubernetes service account reused across many apps
- troubleshooting `403` without checking identity mapping
- leaving workloads dependent on node credentials

---

## 7.7 Private clusters

### Technical purpose
Reduce exposed control-plane access and tighten admin connectivity patterns.

### Best practices
- design admin access path before cluster creation
- verify CI/CD, GitOps, and platform tooling reachability
- document how emergency access works

### Common mistakes
- private cluster created with no operator access path
- platform tools cannot reach cluster after networking change

---

## 7.8 Gateway API and Ingress

Google documents Gateway deployment on GKE and notes prerequisites and troubleshooting such as proxy-only subnet issues and unhealthy upstreams. [7]

### Technical purpose
Expose services using Kubernetes-native traffic resources.

### Best practices
- use Gateway API for newer traffic designs where it fits
- validate underlying LB prerequisites
- test host/path logic and backend health
- separate internal and external gateway intent cleanly

### Common mistakes
- treating Gateway or Ingress as magic abstractions with no underlying LB/network needs
- route conflicts not tested
- missing subnet prerequisites

---

## 7.9 GKE troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Pods Pending | scheduling/capacity | requests/limits, node pool size, taints, quota, secondary IP space |
| ImagePullBackOff | artifact/auth | Artifact Registry path, permissions, image tag/digest, Binary Authorization |
| Upgrade caused outages | disruption/readiness | PDBs, readiness probes, maintenance policy, node-pool rollout |
| Pod gets 403 from Google API | identity | Workload Identity mapping, IAM, KSA namespace/name |
| Gateway exists but no traffic works | network/LB | proxy-only subnet, backend service health, route binding, LB resource provisioning |

---

# 8. Cloud Run

Cloud Run is the managed runtime for serverless containers.

Google documents services, jobs, concurrency, min/max instances, environment variables, and the container contract. [8][9][10][11][12][13]

## 8.1 Technical purpose

Use Cloud Run for:
- HTTP/gRPC request-driven apps
- internal or public APIs
- event targets
- finite background jobs using Cloud Run jobs

## 8.2 Core entities

### Service
Request-serving deployment unit.

### Revision
Immutable deployed version of service config.

### Traffic split
Traffic percentage by revision.

### Job
Finite task execution abstraction.

### Execution
A concrete run of a job.

### Concurrency
Requests per instance.

### Min instances
Warm baseline instances.

### Max instances
Upper scale cap.

### Service account
Runtime identity.

### Ingress
Who can call the service and from where.

## 8.3 Revision model

Any significant config change creates a new revision.

### Why it matters
- revision history is deployment history
- rollback is revision traffic control
- config drift is visible through revisions
- deploy safety depends on revision discipline

### Best practices
- use labeled revisions
- canary before 100%
- keep deployment notes tied to revision changes

### Common mistakes
- config changes treated as informal tweaks
- direct 100% rollout
- weak revision observability

---

## 8.4 Concurrency and instance controls

Google documents concurrency and min/max scaling behavior, including regional and resource quota effects. [10][11][12]

### Concurrency
Higher concurrency can reduce cost but may hurt CPU-bound latency.

### Min instances
Good for latency-sensitive services to reduce cold starts.

### Max instances
Protect downstream dependencies and budget.

### Best practices
- CPU-bound service → lower concurrency
- I/O-bound service → concurrency can often be higher
- protect databases with max instances
- measure p95/p99 before changing settings

### Common mistakes
- concurrency tuned by guesswork
- no max-instance cap on fragile downstreams
- no min instances on user-facing latency-critical apps

---

## 8.5 Request timeout, cold starts, and startup behavior

### Technical role
Cloud Run startup and per-request runtime behavior directly affect user latency.

### Best practices
- minimize startup work
- preload only what is necessary
- use min instances where latency matters
- use jobs for finite background processing instead of request-holding hacks

### Common mistakes
- loading huge models/libraries during cold start without mitigation
- blocking requests on expensive initialization
- using long-running background loops inside request-serving services

---

## 8.6 Ingress and service-to-service patterns

Google documents services as private by default unless unauthenticated access is explicitly allowed. [8]

### Best practices
- internal service → private + authenticated
- public service → front with appropriate edge/API controls
- use dedicated service accounts and explicit caller auth
- align ingress with ALB/API Gateway/Apigee pattern

### Common mistakes
- public exposure by convenience
- unclear internal auth model
- mixing public and private assumptions in one service

---

## 8.7 Environment variables and secrets

Google explicitly recommends not using environment variables to store secrets. [13]

### Best practices
- env vars for non-secret config
- Secret Manager for secrets
- keep env changes under revision control

### Common mistakes
- secrets in env vars
- operational teams unable to trace which config changed with which revision

---

## 8.8 Jobs

Google documents that jobs run tasks and exit, unlike services which handle requests. [14]

### Best fit
- maintenance tasks
- data transformations
- one-off or scheduled runs
- finite batch workloads

### Common mistake
Using services to fake batch execution.

---

## 8.9 Cloud Run troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Revision never becomes ready | container/runtime | correct port, startup logs, container exits, startup latency |
| p95 spikes on burst traffic | scaling/runtime | cold starts, min instances, concurrency, CPU/memory sizing |
| DB overload after deploy | scaling/dependency | max instances, concurrency, connection pooling, downstream capacity |
| Internal caller gets 401/403 | auth/ingress | caller identity, invoker permissions, ingress restriction |
| Job starts then hangs | app logic/runtime | process exit behavior, retries, job semantics vs service assumptions |

---

# 9. Cloud Run Functions, App Engine, and Batch

These are adjacent execution models.

---

## 9.1 Cloud Run functions

### Technical purpose
Function-oriented development model on Cloud Run infrastructure.

### Best fit
- event handlers
- small HTTP handlers
- teams preferring function packaging

### Best practices
- keep single purpose
- use least-privilege service accounts
- design idempotent event handling

### Common mistakes
- turning functions into mini-monoliths
- retries without idempotency

---

## 9.2 App Engine

### Technical purpose
Managed app platform with service/version model.

### Best fit
- existing App Engine applications
- teams already aligned to App Engine deployment/runtime model

### Common mistakes
- assuming App Engine and Cloud Run share the same runtime, scaling, and deploy semantics

---

## 9.3 Batch

### Technical purpose
Managed finite job execution at scale.

### Best fit
- parallel finite tasks
- compute bursts
- batch pipelines

### Best practices
- use when tasks are finite and parallel
- define retries/timeouts
- right-size allocation policy

### Common mistakes
- running bursty batch on always-on VM fleets
- no job retry design

---

# 10. Artifact Registry

Artifact Registry is the artifact storage and governance layer.

## 10.1 Technical purpose

Store and manage:
- container images
- language packages
- build outputs
- metadata and attachments
- lifecycle and cleanup policies

## 10.2 Core entities
- repository
- artifact
- image
- package
- tag
- version
- attachment
- cleanup policy

## 10.3 Artifact Analysis and vulnerability scanning

Google documents vulnerability scanning and metadata concepts such as notes and occurrences. [15]

### Technical role
Support:
- artifact risk review
- metadata-driven governance
- secure deployment policy

## 10.4 Promotion model

Good pattern:
1. build image
2. push immutable digest
3. scan
4. attest
5. promote
6. deploy

Bad pattern:
- push `latest`
- deploy directly
- scan later
- no provenance

## 10.5 Cleanup policies

Google documents configurable cleanup policies. [16]

### Best practices
- clean up stale artifacts
- preserve release tags/digests
- separate dev/test/prod repositories logically
- avoid mutable release tags

### Common mistakes
- only using `latest`
- no cleanup = high cost and operational confusion
- artifact ownership unclear

---

## 10.6 Artifact Registry troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Image pull fails in GKE | auth/path | repository path, Workload Identity or node auth, image digest/tag |
| Cloud Run deploy says image not found | artifact/reference | region/repo mismatch, wrong tag, wrong project |
| Vulnerability review missing | security pipeline | scanning enabled, metadata flow, repository policy |
| Repo cost grows endlessly | lifecycle | cleanup policy, tag strategy, stale versions |

---

# 11. Binary Authorization

Google documents Binary Authorization as deploy-time policy enforcement for supported runtimes including GKE and Cloud Run. [17][18]

## 11.1 Technical purpose
Block deployment of images that do not meet trust policy.

## 11.2 Core entities
- policy
- attestor
- attestation
- trusted image rule set

## 11.3 Good deployment-trust chain
1. Cloud Build builds image
2. image stored in Artifact Registry
3. vulnerability/scanning and policy checks run
4. attestation created
5. Binary Authorization allows deploy

## 11.4 Best practices
- enforce strongest in production first
- tie attestations to immutable digests
- keep attestor ownership clear
- provide developer-visible feedback on policy failures

## 11.5 Common mistakes
- enabling enforcement before trust pipeline exists
- attesting mutable tags instead of digests
- too many manual exceptions

---

## 11.6 Binary Authorization troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Deploy blocked only in prod | policy scope | env-specific policy, missing attestation, allowed registry config |
| Valid image still rejected | trust metadata | digest mismatch, wrong attestor, stale policy, wrong project/repo |
| Developers bypass policy informally | process | exception path too weak or release flow poorly designed |

---

# 12. Cloud Build

Cloud Build is the build and CI execution layer.

## 12.1 Technical purpose
Build, test, package, sign, and hand off deployable artifacts.

## 12.2 Core entities
- build
- step
- build config
- trigger
- substitutions
- worker pool
- service account
- approval config

## 12.3 Build configs and steps

### Best practices
- small, explicit steps
- reproducible toolchain images
- cache strategy where appropriate
- separate build/test from deploy logic when possible

### Common mistakes
- giant opaque build files
- shell scripts doing everything invisibly
- prod deploys wired directly to feature branches

---

## 12.4 Triggers

### Technical role
Automate builds from source or event changes.

### Best practices
- map triggers to branch/environment policy
- label ownership clearly
- keep release branches distinct from feature branches

### Common mistakes
- trigger sprawl
- overlapping triggers causing duplicate builds
- wrong branch deploys due to weak trigger filters

---

## 12.5 Worker pools

Use private worker pools when:
- build needs private network access
- egress must be controlled
- isolation/compliance requires dedicated environment

### Common mistakes
- default workers used for builds that need private network reachability
- no network path validation for private dependency access

---

## 12.6 Approvals and service accounts

### Best practices
- least privilege build identities
- approval gates for prod-sensitive changes
- separate build identity from deploy identity when appropriate

### Common mistakes
- broad default service accounts
- no approval gate on production promotions
- secrets exposed to all build contexts

---

## 12.7 Cloud Build troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Build passes locally, fails in Cloud Build | environment | build image/toolchain, private dependency access, service account |
| Build can push artifact but not deploy | identity | deploy permissions, target auth, Binary Authorization |
| Trigger didn’t fire | source automation | trigger filter, branch mapping, repo connection |
| Private dependency unreachable | network | worker pool config, VPC connectivity, DNS, firewall |

---

# 13. Cloud Deploy

Cloud Deploy is the release orchestration layer.

## 13.1 Technical purpose
Promote artifacts across environments with controlled rollout tracking.

## 13.2 Core entities
- delivery pipeline
- target
- release
- rollout
- phase/approval model
- Skaffold configuration
- custom target type

## 13.3 Delivery pipeline model

Google documents delivery pipelines and targets as distinct resources. [19]

### Mental model
- build produces artifact
- release packages render intent
- rollout applies that release to a target
- promotion moves the release forward

## 13.4 Skaffold integration

Google documents Skaffold integration and also notes its removal from gcloud CLI after **January 15, 2027**. [20][21]

### Best practices
- keep render logic separate from environment policy
- define targets cleanly
- use progressive approvals
- document rollback procedure

### Common mistakes
- Cloud Build and Cloud Deploy responsibilities mixed together
- environment-specific logic duplicated everywhere
- releases created without rollout review

---

## 13.5 Cloud Deploy troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Release created, rollout failed | target/deploy auth | target config, cluster access, namespace/resources, Binary Authorization |
| Render works locally but not in pipeline | render config | Skaffold profile, substitutions, environment assumptions |
| Promotion blocked unexpectedly | control flow | approval gate, target policy, rollout health |
| Rollout succeeds but app fails | runtime | health checks, config drift, dependency access, service identity |

---

# 14. Infrastructure Manager, Terraform, Config Connector, and Config Controller

Week 3 includes how platform resources themselves are delivered.

---

## 14.1 Infrastructure Manager

### Technical purpose
Managed Terraform execution model on Google Cloud.

### Core entities
- deployment
- revision
- configuration
- variables
- outputs
- execution identity

### Best practices
- review plans
- isolate deployment identities
- keep state and variable governance strong
- treat infra changes as release artifacts

### Common mistakes
- apply without plan review
- broad deployment identities
- drift outside code

---

## 14.2 Config Connector

Google documents Config Connector resources as Kubernetes CRDs under `cnrm.cloud.google.com`. [22][23]

### Technical purpose
Manage GCP resources declaratively from Kubernetes.

### Best fit
Platform teams using Kubernetes as a control plane for infrastructure.

### Best practices
- clear namespace-to-project mapping
- Git review for changes
- separate app manifests from platform resource manifests
- limit controller privileges

### Common mistakes
- no ownership boundary
- ad hoc use instead of platform pattern
- permission issues hidden behind reconciliation failures

---

## 14.3 Config Controller

### Technical purpose
Hosted control plane for Config Connector-style management.

### Best fit
Teams wanting managed lifecycle for Kubernetes-style infra control.

### Common mistakes
- no tenancy design
- ambiguous project mappings
- control-plane sprawl

---

## 14.4 Infra-control troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Terraform plan okay, apply fails | policy/identity | service account, APIs enabled, org policy |
| Config Connector resource stuck reconciling | controller/API | permissions, unsupported field, resourceID, parent references |
| Drift keeps reappearing | ownership/process | out-of-band changes, weak source-of-truth discipline |

---

# 15. Cloud Scheduler, Eventarc, and Workflows

These services provide control flow.

---

## 15.1 Cloud Scheduler

### Technical purpose
Cron-style job triggering.

### Best fit
Scheduled HTTP or event invocations.

### Common mistakes
- using app-side cron on long-lived VMs when Scheduler is cleaner
- no idempotency or retry design

---

## 15.2 Eventarc

Google documents Eventarc as managed event routing with CloudEvents delivery format. [24][25]

### Technical purpose
Connect event sources to destinations such as Cloud Run and Workflows.

### Core entities
- trigger
- provider
- destination
- event filter
- delivery identity

### Best practices
- keep triggers narrow
- assume retry and duplicate-delivery possibilities
- build idempotent consumers
- validate region and destination permissions

### Common mistakes
- broad catch-all triggers
- assuming strict ordering
- weak event schema handling

---

## 15.3 Workflows

Google documents Workflows as managed orchestration that can combine Google Cloud services, HTTP APIs, and connectors. [26]

### Technical purpose
Coordinate multi-step business or platform flows.

### Core entities
- workflow
- execution
- step
- connector
- callback
- runtime arguments

### Best practices
- explicit retries/timeouts
- structured state passing
- modular workflow design
- use for orchestration, not as a replacement for every app flow

### Common mistakes
- giant opaque workflows
- no timeout policy
- no idempotency around triggered workflows

---

## 15.4 Eventing troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Event source active, no target invocation | trigger config | filters, region, destination auth, service account |
| Workflow receives unexpected payload shape | event schema | CloudEvents mapping, destination type, argument parsing |
| Duplicate processing | event semantics | retries, idempotency, downstream state handling |
| Scheduled job fires but target errors | target/app | auth, endpoint reachability, app assumptions |

---

# 16. Vertex AI, Gemini via Vertex AI, Cloud TPU, and BigQuery ML

Week 3 also includes the AI platform layer.

---

## 16.1 Vertex AI

### Technical purpose
Unified ML platform for:
- training
- tuning
- pipelines
- model registry
- online serving
- batch prediction
- experiment and metadata patterns

### Core entities
- dataset
- training job
- custom job
- model
- Model Registry
- endpoint
- batch prediction job
- pipeline
- metadata/lineage

## 16.2 Training vs serving

### Training
Often bursty, experiment-heavy, compute-oriented.

### Serving
Latency-sensitive, versioned, reliability-oriented.

### Best practice
Treat them as separate operational domains.

### Common mistake
Using training assumptions to size or govern production serving.

---

## 16.3 Model Registry and endpoints

### Technical role
- registry = controlled model versions
- endpoint = online serving surface

### Best practices
- register before serving
- use staging endpoints
- promote through environments
- track model versions and rollback path

### Common mistakes
- deploying ad hoc model artifact directly
- no endpoint scale/performance validation
- no rollback plan between model versions

---

## 16.4 Vertex AI Pipelines

Google documents pipeline components and pipeline building as the ML orchestration model. [27][28]

### Best practices
- explicit step boundaries
- version components
- track lineage and parameters
- separate experimental from production pipelines

### Common mistakes
- hidden side effects in components
- mixing data prep, model training, and serving promotion without clear gates

---

## 16.5 Gemini via Vertex AI

Google documents Workflow-based access to Vertex AI models including Gemini-style publisher endpoints. [29]

### Technical purpose
Controlled access to generative models under Vertex AI governance.

### Best practices
- control by service account and environment
- test prompt/output quality before production
- log usage according to policy
- separate prototypes from production-approved prompts and models

### Common mistakes
- no evaluation loop
- region/model mismatch
- production use with no governance or output review

---

## 16.6 Cloud TPU

Google documents Vertex AI support for TPU training with selected TPU VM types such as v2, v3, and v5e in documented contexts. [30]

### Best fit
Specialized training where TPU economics/performance fit the framework and workload.

### Common mistake
Choosing TPU without operator/framework readiness.

---

## 16.7 BigQuery ML

### Technical purpose
SQL-native ML close to warehouse data.

### Best fit
Analytics-centric models where data gravity favors BigQuery.

### Common mistake
Externalizing small warehouse-native ML problems unnecessarily.

---

## 16.8 AI platform troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Training job fails early | identity/config | service account, data path, container/image, accelerator settings |
| Model deploy succeeds but inference errors | serving/runtime | endpoint config, model version, request schema, scaling |
| Pipeline step fails intermittently | orchestration | retries, component side effects, permissions, regional resource access |
| Gemini call fails from Workflow | API/auth | Vertex AI permissions, publisher endpoint, request payload, region/model |

---

# 17. End-to-End Platform Delivery Example

A platform team needs:
- VM legacy apps
- Kubernetes services
- request-driven APIs
- image trust enforcement
- staged releases
- event-driven workflows
- ML model serving
- controlled generative AI access

A strong design:

### Execution
- Compute Engine + MIG for OS-specific legacy tiers
- GKE Autopilot for most containerized services
- Standard GKE only for workloads needing deeper node control
- Cloud Run for request-driven services and jobs

### Artifact and trust
- Artifact Registry for all images and packages
- vulnerability scanning
- Binary Authorization in prod

### Build and release
- Cloud Build for build/test/package/sign
- Cloud Deploy for target progression and rollout
- revision- and digest-based promotion

### Infrastructure
- Infrastructure Manager for Terraform-controlled shared resources
- Config Connector/Config Controller for platform-native infra management

### Eventing
- Scheduler for timed tasks
- Eventarc for event routing
- Workflows for multi-step orchestration

### AI
- Vertex AI for training/registry/serving
- Gemini via Vertex AI for governed gen-AI use cases

This is the Week 3 principle:

> runtime choice, artifact trust, rollout control, and orchestration must be designed as one delivery system.

---

# 18. Troubleshooting Flow

When something breaks, use this order:

1. **Artifact**  
   Was the right image/model/artifact built and referenced?

2. **Identity**  
   Does the runtime have the permissions it expects?

3. **Runtime contract**  
   Does the app obey the platform’s execution rules?

4. **Capacity and scaling**  
   Did the workload have enough instances/nodes/quota?

5. **Release policy**  
   Did rollout, Binary Authorization, or approval control block or distort deployment?

6. **Dependency path**  
   Can the runtime actually reach its dependencies?

7. **Observability evidence**  
   What do logs, events, health checks, and rollout status say?

---

# 19. Common Week 3 Mistakes

- choosing the heaviest runtime by default
- manual VM drift
- startup scripts doing too much
- weak MIG health checks
- using Standard GKE without real node-control need
- no release channel policy
- broad workload identity permissions
- Cloud Run configs tuned by guesswork
- secrets in environment variables
- no artifact promotion discipline
- Binary Authorization enabled before trust pipeline exists
- Cloud Build identities too broad
- Cloud Deploy pipeline not separated from build concerns
- Eventarc consumers not idempotent
- Workflows with no retry/timeout model
- Vertex AI serving with no registry or promotion path
- generative AI endpoints used without governance or evaluation

---

# 20. Revision Checklist

You should be able to explain clearly:

- when Compute Engine is better than GKE or Cloud Run
- how machine images differ from ordinary images
- how metadata, startup scripts, Shielded VM, and OS Login affect VM operations
- how MIG templates, autoscaling, autohealing, and rolling updates work together
- how Standard and Autopilot GKE differ operationally
- what release channels and maintenance windows control
- how Workload Identity Federation for GKE works
- how Gateway API differs from older ingress patterns
- how Cloud Run revisions, concurrency, min/max instances, ingress, and jobs interact
- why Cloud Run services and jobs are different abstractions
- how Artifact Registry, scanning, attestations, and Binary Authorization fit together
- what Cloud Build does vs Cloud Deploy
- how Infrastructure Manager differs from Config Connector
- how Scheduler, Eventarc, and Workflows fit together
- how Vertex AI training, registry, endpoints, and pipelines fit together
- how Gemini via Vertex AI differs from an ungoverned direct model integration

---

# 21. Memory Anchors

- **Compute Engine** = VM control
- **machine image** = broader VM capture
- **instance template** = reusable VM definition
- **MIG** = managed VM fleet
- **autoscaling** = elastic fleet size
- **autohealing** = recreate unhealthy VM
- **GKE Standard** = more node control
- **GKE Autopilot** = less ops burden
- **release channel** = upgrade track
- **Workload Identity for GKE** = pod-to-Google-API identity
- **Cloud Run service** = request-serving container
- **Cloud Run revision** = immutable version
- **Cloud Run job** = finite container task
- **Artifact Registry** = artifact store
- **Binary Authorization** = deploy trust enforcement
- **Cloud Build** = build/test/package
- **Cloud Deploy** = promotion and rollout
- **Infrastructure Manager** = managed Terraform deploy
- **Config Connector** = Kubernetes-style infra control
- **Scheduler** = cron trigger
- **Eventarc** = event routing
- **Workflows** = orchestration
- **Vertex AI** = ML platform
- **Gemini via Vertex AI** = governed generative AI access
- **Cloud TPU** = training accelerator
- **BigQuery ML** = SQL-native ML

---

# 22. Official References

## Compute Engine
- Instance groups  
  https://docs.cloud.google.com/compute/docs/instance-groups

- Create a managed instance group  
  https://docs.cloud.google.com/compute/docs/quickstart-mig

- Instance templates  
  https://docs.cloud.google.com/compute/docs/instance-templates

- Create instance templates  
  https://docs.cloud.google.com/compute/docs/instance-templates/create-instance-templates

- Machine images  
  https://docs.cloud.google.com/compute/docs/machine-images

## GKE
- GKE Autopilot overview  
  https://docs.cloud.google.com/kubernetes-engine/docs/concepts/autopilot-overview

- Workload Identity Federation for GKE  
  https://docs.cloud.google.com/kubernetes-engine/docs/how-to/workload-identity

- Release channels  
  https://docs.cloud.google.com/kubernetes-engine/docs/how-to/release-channels

- Deploying Gateways  
  https://docs.cloud.google.com/kubernetes-engine/docs/how-to/deploying-gateways

## Cloud Run
- Configure Cloud Run services  
  https://docs.cloud.google.com/run/docs/configuring

- Container runtime contract  
  https://docs.cloud.google.com/run/docs/container-contract

- About concurrency  
  https://docs.cloud.google.com/run/docs/about-concurrency

- Set minimum instances  
  https://docs.cloud.google.com/run/docs/configuring/min-instances

- Set maximum instances  
  https://docs.cloud.google.com/run/docs/configuring/max-instances

- About maximum instances limits  
  https://docs.cloud.google.com/run/docs/configuring/max-instances-limits

- Configure service environment variables  
  https://docs.cloud.google.com/run/docs/configuring/services/environment-variables

- Configure job environment variables  
  https://docs.cloud.google.com/run/docs/configuring/jobs/environment-variables

- Create jobs  
  https://docs.cloud.google.com/run/docs/create-jobs

## Artifacts and supply chain
- Artifact Analysis and vulnerability scanning  
  https://docs.cloud.google.com/artifact-registry/docs/analysis

- Protect artifacts  
  https://docs.cloud.google.com/artifact-registry/docs/protect-artifacts

- Configure cleanup policies  
  https://docs.cloud.google.com/artifact-registry/docs/repositories/cleanup-policy

- Secure deployments / Binary Authorization  
  https://docs.cloud.google.com/artifact-registry/docs/secure-deployments

## Build and release
- Deploy to Cloud Run using Cloud Build  
  https://docs.cloud.google.com/build/docs/deploying-builds/deploy-cloud-run

- Deploy to Compute Engine with Cloud Build  
  https://docs.cloud.google.com/build/docs/deploying-builds/deploy-compute-engine

- Cloud Build ApprovalConfig  
  https://cloud.google.com/build/docs/api/reference/rest/v1/ApprovalConfig

- Use Skaffold with Cloud Deploy  
  https://docs.cloud.google.com/deploy/docs/using-skaffold

- Get started with Skaffold in Cloud Deploy  
  https://docs.cloud.google.com/deploy/docs/using-skaffold/getting-started-skaffold

- Create delivery pipeline and targets  
  https://docs.cloud.google.com/deploy/docs/create-pipeline-targets

- Custom targets  
  https://cloud.google.com/deploy/docs/custom-targets

## Infrastructure and config management
- Config Connector controller types  
  https://docs.cloud.google.com/config-connector/docs/concepts/controller-types

- About Config Connector CRDs  
  https://docs.cloud.google.com/config-connector/docs/concepts/resources

- Config Connector resources  
  https://docs.cloud.google.com/config-connector/docs/reference/overview

## Eventing and orchestration
- Eventarc overview  
  https://docs.cloud.google.com/eventarc/docs/overview

- Manage Eventarc triggers  
  https://docs.cloud.google.com/eventarc/docs/managing-triggers

- Event format  
  https://docs.cloud.google.com/eventarc/docs/event-format

- Trigger Workflows with Eventarc  
  https://docs.cloud.google.com/workflows/docs/trigger-workflow-eventarc

- Workflows overview  
  https://docs.cloud.google.com/workflows/docs/overview

## AI/ML
- Vertex AI API usage overview  
  https://cloud.google.com/vertex-ai/docs/reference

- Vertex AI Pipelines interfaces  
  https://docs.cloud.google.com/vertex-ai/docs/pipelines/interfaces

- Google Cloud Pipeline Components  
  https://docs.cloud.google.com/vertex-ai/docs/pipelines/components-introduction

- Build a pipeline  
  https://cloud.google.com/vertex-ai/docs/pipelines/build-pipeline

- Workflows with Vertex AI models  
  https://docs.cloud.google.com/workflows/docs/tutorials/use-vertex-ai-models

- Training with TPU accelerators  
  https://cloud.google.com/vertex-ai/docs/training/training-with-tpu-vm

---

# 23. Final Summary

Week 3 is about **how applications run, scale, and ship on Google Cloud**.

The strongest mental model is:

- **Compute Engine and MIGs** = VM execution and fleet control
- **GKE** = Kubernetes platform execution
- **Cloud Run** = serverless container execution
- **Artifact Registry + Binary Authorization** = artifact trust and deploy-time control
- **Cloud Build + Cloud Deploy** = build and release separation
- **Infrastructure Manager + Config Connector** = reproducible infra delivery
- **Scheduler + Eventarc + Workflows** = event and orchestration control
- **Vertex AI + Gemini** = ML and generative AI platform execution

When you can explain the **runtime model, identity model, artifact model, rollout model, and failure model** together, Week 3 is understood architecturally.
