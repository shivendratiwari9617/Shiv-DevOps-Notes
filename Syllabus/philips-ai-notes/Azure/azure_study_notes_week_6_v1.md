# Azure Study Notes — Week 6
## Disaster Recovery, Resilience Validation, and Migration Execution

> This is the **deep Week 6 Azure guide**, written much closer to a **9.5+ bar** than a normal summary.  
> It is structured as **expert-level technical study material** with:
> - technical purpose
> - core entities
> - control-plane and execution mechanics
> - architecture fit
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** tight structure, high signal, minimal filler.

---

# 1. Why Week 6 Matters

Week 6 is about **getting from “designed” to “survivable and movable.”**

If:
- Week 1 = governance
- Week 2 = networking
- Week 3 = runtime and delivery
- Week 4 = state and data
- Week 5 = operations and evidence

then Week 6 = **continuity and change under stress**.

This is where platform architecture becomes operationally honest:
- can the workload recover in another location?
- can recovery be rehearsed without business harm?
- can migration happen in controlled waves?
- can databases cut over with known rollback criteria?
- can regional moves be executed without hidden dependency traps?
- can large data sets move online or offline with verifiable integrity?
- can hybrid servers be governed consistently during and after migration?
- can resilience claims be validated through load and chaos testing?
- can Well-Architected findings become concrete action plans?

The core idea is:

> **Week 6 is about execution discipline: recover, move, validate, cut over, fail back, and prove resilience with evidence.**

Weak Week 6 architecture usually looks like this:
- DR exists only on diagrams
- Site Recovery replication is enabled but recovery plans are not rehearsed
- test failovers are skipped or treated as optional
- migration assessments exist, but dependency grouping and wave planning are weak
- database migration mode is chosen without downtime truth
- rollback is mentioned but not engineered
- resource moves ignore dependency and quota realities
- bulk data transfer lacks verification and cutover logic
- Arc is installed but not used as an operational control plane
- chaos and load tests are run as demos instead of reliability controls

Strong Week 6 architecture can answer:
- what is the recovery objective and the recovery method?
- what is replicated, restored, rebuilt, or redirected?
- what is the cutover trigger?
- what is the rollback point?
- what dependencies must move together?
- what data must move online versus offline?
- what operational evidence proves the design works?

---

# 2. Learning Outcomes

By the end of Week 6, you should be able to:

- explain **Azure Site Recovery** vaults, replication policies, protected machines, recovery points, recovery plans, test failover, planned failover, unplanned failover, reprotection, and failback
- explain **Azure Migrate** discovery, appliance deployment, assessments, dependency analysis, grouping, business case interpretation, and migration-wave planning
- explain **Azure Database Migration Service** offline vs online migration modes, replication/cutover logic, validation, downtime windows, and rollback planning
- explain **Azure Resource Mover** move collections, dependency validation, prepare, initiate move, commit, and discard
- explain **Azure Data Box** and **Azure Storage Mover**, including offline transfer, agent placement, endpoint design, verification, and cutover patterns
- explain **Azure Arc-enabled servers**, extensions, policy, and machine configuration as hybrid governance controls during migration and post-migration operations
- explain **Azure Chaos Studio** and **Azure Load Testing** as resilience-validation tools, and relate them to monitored stop conditions and blast-radius control
- explain **Azure Well-Architected Review** as a structured assessment and improvement loop rather than a one-time questionnaire
- design recovery rehearsals, migration waves, rollback criteria, and resilience-validation programs with clear operational ownership
- troubleshoot replication gaps, stale assessments, dependency misses, migration-cutover failures, move-collection blockers, agent issues, and invalid resilience-test conclusions

---

# 3. Week 6 Mental Model

Think in six execution planes:

1. **Recovery plane**  
   Site Recovery, recovery points, recovery plans, failover, failback

2. **Migration planning plane**  
   Azure Migrate, discovery, dependency analysis, assessments, wave design

3. **Data move plane**  
   Database Migration Service, Data Box, Storage Mover

4. **Region and platform move plane**  
   Resource Mover, cross-region dependency resolution, target readiness

5. **Hybrid governance plane**  
   Azure Arc, extensions, policy, machine configuration

6. **Resilience validation plane**  
   Load Testing, Chaos Studio, Well-Architected Review, operational evidence

The Week 6 principle is:

> **Recovery, migration, and resilience are not separate topics. They are all exercises in dependency truth, cutover truth, and evidence truth.**

---

# 4. Execution Selection Before Service Selection

## 4.1 Use Azure Site Recovery when
- you need orchestrated disaster recovery
- replicated workload continuity matters more than restore-only recovery
- you need failover, test failover, reprotection, and failback mechanics

## 4.2 Use Azure Migrate when
- you need discovery, assessments, dependency analysis, and structured migration planning
- you want migration waves built from actual inventory and readiness evidence

## 4.3 Use Azure Database Migration Service when
- database migration needs controlled online or offline movement
- cutover timing and replication mode matter

## 4.4 Use Resource Mover when
- Azure resources must move across regions with dependency-aware orchestration

## 4.5 Use Data Box when
- data volume, bandwidth, or transfer window makes offline transfer the right answer

## 4.6 Use Storage Mover when
- file and folder migrations to Azure Storage need managed, agent-based, low-downtime execution

## 4.7 Use Azure Arc when
- hybrid or migrated servers need centralized governance, extensions, policy, and configuration audit/remediation

## 4.8 Use Load Testing and Chaos Studio when
- resilience and performance claims need validation under controlled stress, not assumption

## 4.9 Use Well-Architected Review when
- workload-level improvement needs structured assessment and exported recommendations

---

# 5. Azure Site Recovery (ASR)

Microsoft documents Site Recovery, recovery plans, failover/failback, test failover, monitoring, and a failover-readiness issue if no successful test failover exists within 180 days. [1][2][3][4][5][6]

## 5.1 Technical purpose

Use Site Recovery for:
- orchestrated disaster recovery
- replicated machine continuity
- controlled failover and failback
- multi-machine application recovery sequencing
- DR drills that validate actual recovery path

## 5.2 Core entities

### Recovery Services vault
Control plane for replication and recovery metadata.

### Replication policy
Defines replication behavior, retention, and recovery-point characteristics.

### Protected machine / replicated item
Workload under replication protection.

### Recovery point
Usable restore/failover point in time.

### Recovery plan
Orchestrated failover sequence across multiple machines.

### Test failover
Isolated disaster-recovery drill.

### Planned failover
Controlled failover with source-side coordination.

### Unplanned failover
Emergency failover under disruption conditions.

### Reprotection / failback
Reverse direction and return-to-primary operations after recovery.

---

## 5.3 Recovery plans

Microsoft documents recovery plans as the way to define failover order, startup sequence, and grouped orchestration for up to 100 protected instances per plan. [2]

### Best practices
- create recovery plans by application boundary, not by random infrastructure grouping
- model startup sequencing explicitly
- include operational instructions and checkpoints
- validate plan assumptions against actual dependencies such as DNS, load balancers, secrets, and databases

### Common mistakes
- recovery plan mirrors resource group structure instead of application dependency structure
- too many unrelated systems in one plan
- recovery plan exists but no one has tested whether the app is usable after startup

### Expert nuance
A recovery plan is not just a boot order.  
It is:
- dependency order
- validation order
- operator-decision order
- business-recovery order

---

## 5.4 Test failover

Microsoft documents that a test failover validates replication and DR strategy without affecting ongoing replication or production, and recommends at least one successful test failover every 180 days or Site Recovery raises a failover-readiness health issue. [3][4]

### Best practices
- run test failovers on a schedule, not only before audits
- use isolated test networks deliberately
- capture evidence and lessons from every drill
- validate application outcome, not only VM boot success

### Common mistakes
- test failover treated as optional
- drill ends when VM starts, even though application dependencies are broken
- no cleanup or documentation after test run
- test network design accidentally overlaps production assumptions

---

## 5.5 Planned failover, unplanned failover, reprotection, and failback

Microsoft documents failover stages, recovery-point options, reprotection, and failback in modernized ASR guidance. [5]

### Best practices
- document which failover type is appropriate for which incident
- define recovery-point choice criteria
- validate networking, identity, and DNS behavior after failover
- plan reprotection and failback before disaster, not after recovery day

### Common mistakes
- recovery plan stops at failover; no one planned the steady-state run in recovered region
- failback assumed to be symmetrical and easy
- teams cannot explain when to choose latest processed versus app-consistent recovery point
- replicated compute recovered, but dependent stateful services were never part of the continuity model

---

## 5.6 Monitoring and readiness

Microsoft documents built-in monitoring for replication health, test-failover status, and issues affecting configuration and replication. [6]

### Best practices
- monitor replication health continuously
- route readiness issues to responsible owners
- track last successful test failover date
- correlate ASR alerts with app-specific recovery tests

### Common mistakes
- replication issues discovered only during event
- built-in health not connected to operations workflow
- teams confuse protected status with proven recoverability

---

## 5.7 Site Recovery troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Replication healthy but app not recoverable | dependency model | DNS, identity, load balancer, database dependencies, recovery plan sequencing |
| Test failover “worked” but business test failed | validation quality | isolated network design, app validation checklist, post-boot dependency readiness |
| Failover blocked or confusing | recovery state | chosen recovery point, replication health, plan ordering, target capacity/network readiness |
| Recovered environment unstable | post-failover ops | reprotection state, DNS cutover, secrets, monitoring, app config drift |
| Readiness warning appears unexpectedly | drill cadence | last successful test failover date, 180-day readiness expectation, monitoring workflow |

---

# 6. Azure Migrate

Microsoft documents Azure Migrate discovery and assessment, dependency analysis, migration-wave planning, and migration planning in the Cloud Adoption Framework. [7][8][9][10][11][12]

## 6.1 Technical purpose

Use Azure Migrate for:
- inventory discovery
- assessment and right-sizing
- dependency visualization
- business-case and readiness input
- migration grouping and wave sequencing
- execution planning for server and app migration

## 6.2 Core entities

### Project
Top-level migration planning boundary.

### Appliance
Discovery engine deployed into source environment.

### Discovered server
Inventory object with collected metadata.

### Assessment
Sizing, readiness, and target recommendation artifact.

### Group
Logical set of machines assessed or planned together.

### Dependency analysis
Observed communication relationship evidence.

### Migration wave
Structured batch for execution sequencing.

---

## 6.3 Appliance and discovery strategy

### Best practices
- scope appliance deployment deliberately
- validate discovery coverage before trusting assessment output
- coordinate credentials and access carefully
- understand that stale or partial discovery leads to false confidence

### Common mistakes
- appliance deployed but discovery scope too narrow
- inventory looks complete but critical edge systems were excluded
- credentials permit inventory but not the richer data needed for dependency analysis

---

## 6.4 Dependency analysis

Microsoft documents both dependency analysis and agentless dependency analysis, with current limitations in the group view. [8][9]

### Technical purpose
Identify real communication dependencies across servers.

### Best practices
- use dependency evidence to shape groups and waves
- validate critical dependencies manually for crown-jewel apps
- revisit dependency maps after observation windows that include peak business behavior
- treat lack of detected dependency as “not observed,” not automatically “does not exist”

### Common mistakes
- one short observation window treated as complete truth
- dependencies mapped only for app tier, not admin, auth, batch, or integration flows
- teams trust group dependency map blindly without understanding current limitations

### Expert nuance
Dependency analysis reduces unknowns, but it does not eliminate architectural judgment.

---

## 6.5 Assessments and migration waves

Microsoft documents migration waves, including manual and recommended waves based on workload inventory, dependencies, criticality, readiness, and planning conditions. [10][11]

### Best practices
- use waves to reduce risk and improve learning between batches
- group workloads by business dependency and rollback capability
- keep early waves intentionally informative, not merely easy
- align wave boundaries to cutover windows and operational capacity

### Common mistakes
- wave 1 chosen by political visibility instead of learnability and risk balance
- app and shared dependency land in different waves accidentally
- too many servers in one wave for rollback practicality
- assessment output treated as migration decision without business context

---

## 6.6 Azure Migrate troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Assessment looks unrealistic | discovery/assumption | appliance scope, performance-history window, readiness assumptions, sizing basis |
| Dependency map misses expected systems | observation model | collection window, access coverage, unsupported/undetected flows, analysis limitations |
| Migration wave feels unmanageable | planning model | wave size, shared dependencies, rollback ability, business blackout constraints |
| Project confidence low late in program | governance | stale discovery, missing ownership, no wave acceptance criteria, weak rehearsal |

---

# 7. Azure Database Migration Service (DMS)

Microsoft documents DMS as a fully managed service for online and offline database migrations with minimal downtime, and notes that online SQL Server migrations use Premium tier while offline migration downtime starts when migration starts. [13][14][15]

## 7.1 Technical purpose

Use DMS for:
- structured database migration
- online or offline database movement
- controlled cutover
- heterogeneous or engine-aligned migration scenarios depending on path

## 7.2 Core entities

### Source endpoint
Origin database connection context.

### Target endpoint
Destination database context.

### Migration activity
Execution unit tracking a migration job.

### Offline migration
Data movement with downtime beginning at migration start.

### Online migration
Continuous or near-continuous synchronization with downtime concentrated at cutover.

### Cutover
Authoritative switch to target database.

---

## 7.3 Offline vs online migration

Microsoft documents the key difference: offline migration begins downtime when migration starts, while online migration limits downtime to the cutover window at the end. [14]

### Best practices
- choose mode from business downtime truth, not technical preference
- rehearse offline duration honestly
- use online only when operational complexity is justified
- define cutover freeze, validation, and rollback criteria in advance

### Common mistakes
- offline chosen with unrealistic downtime expectation
- online migration selected, but teams do not plan replication lag monitoring and cutover choreography
- “minimal downtime” interpreted as “no business coordination needed”

---

## 7.4 Cutover and rollback design

### Best practices
- define authoritative success criteria before cutover
- freeze schema-change or high-risk app changes during migration window
- validate app connectivity, login behavior, and core transactions immediately after cutover
- keep rollback decision time-boxed and owned

### Common mistakes
- rollback exists only as vague statement
- source continues receiving writes because freeze discipline was weak
- validation focuses on connection test instead of business transaction path
- stakeholders disagree on when to declare migration successful

---

## 7.5 DMS troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Migration progresses but cutover confidence is low | execution design | validation checklist, replication lag view, freeze controls, rollback threshold |
| Downtime larger than expected | migration mode fit | offline vs online choice, data volume, pre-stage completeness, cutover choreography |
| Target works technically but app fails | post-cutover integration | connection strings, auth, compatibility, jobs, dependent services |
| Rollback impossible in practice | governance | no write-freeze discipline, no rollback point, unclear source/target authority |

---

# 8. Azure Resource Mover

Microsoft documents Resource Mover, the move process, dependency validation, supported scenarios, and the CLI move-collection lifecycle including prepare, initiate-move, commit, and discard. [16][17][18][19]

## 8.1 Technical purpose

Use Resource Mover for:
- moving supported Azure resources across regions
- dependency-aware orchestration
- repeatable region-move process
- structured target-region preparation

## 8.2 Core entities

### Move collection
Container for resources and their move state.

### Move resource
Tracked item inside the move collection.

### Dependency validation
Check for unresolved dependent resources.

### Prepare
Stage target-side resources.

### Initiate move
Begin move operation.

### Commit
Finalize move to target region.

### Discard
Cancel staged move state where appropriate.

---

## 8.3 Move lifecycle

Microsoft documents that resources go through a summarized move-region process and that unresolved dependencies must be resolved before moving. [17]

### Best practices
- resolve dependencies early
- validate target-region quota and support matrix first
- move by service boundary, not random resource grouping
- rehearse move order for stateful resources and networking dependencies

### Common mistakes
- move collection assembled before target readiness is validated
- operators discover unsupported or missing dependent resources late
- prepare succeeds but workload still not usable because app dependencies were not modeled

---

## 8.4 Dependency and target-region readiness

### Best practices
- check quotas in destination subscription
- validate region support and resource constraints
- separate infra move success from workload cutover success
- keep fallback checkpoint before commit

### Common mistakes
- commit treated as reversible convenience
- network, identity, DNS, and monitoring dependencies not validated in target region
- teams assume Resource Mover replaces application cutover planning

---

## 8.5 Resource Mover troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Move blocked by dependencies | move design | unresolved dependency list, alternate target resources, unsupported resources |
| Prepare succeeded but app still fails in target | workload dependency | DNS, identity, app config, shared services, target quota/limits |
| Team unsure whether to commit | rollback checkpoint | validation evidence, fallback readiness, business signoff, monitoring state |
| Region move feels risky | planning model | support matrix, quotas, wave size, target readiness testing |

---

# 9. Data Box and Azure Storage Mover

Microsoft documents Data Box security and overview, notes the Data Box 80 TB device is being retired in regions where next-generation devices are available, and documents Storage Mover as a fully managed hybrid migration service using agents that should be placed near source storage. [20][21][22][23][24][25]

## 9.1 Azure Data Box

### Technical purpose
Offline transfer of large data volumes into or out of Azure when network transfer is impractical.

### Core entities
- order
- device
- import/export workflow
- transfer logs
- chain-of-custody and security controls

### Best practices
- use Data Box when bandwidth, duration, or risk profile justify physical transfer
- validate target storage structure before device arrival
- include checksum/integrity validation in runbook
- plan delta-sync or final cutover after bulk copy

### Common mistakes
- Data Box chosen without downstream ingest and validation plan
- chain-of-custody and handoff steps are poorly documented
- offline transfer completes, but no final delta strategy exists

### Expert nuance
Offline transfer shortens bulk move time, but it does not remove the need for:
- final synchronization
- namespace validation
- permission validation
- application cutover choreography

---

## 9.2 Data Box security

Microsoft documents security features for device, service, and data, including tracking and event logging through the solution flow. [20]

### Best practices
- treat device handling as security workflow
- document who can receive, unlock, copy, and return devices
- preserve transfer logs and audit trail
- validate data deletion and handling obligations where relevant

### Common mistakes
- security viewed only as device encryption, not full chain-of-custody
- no one owns post-transfer verification

---

## 9.3 Azure Storage Mover

Microsoft documents Storage Mover as a fully managed migration service for files and folders from on-premises or AWS S3 to Azure Storage, using agents that run as VM-based migration appliances placed near the source storage. [22][23]

### Technical purpose
Managed online or staged migration for unstructured file/folder data.

### Core entities
- project
- source endpoint
- target endpoint
- agent
- job definition
- job run

### Best practices
- place agents as near as possible to source storage
- treat endpoints and job definitions as controlled migration artifacts
- use Storage Mover when repeated, verifiable file migration matters more than one-off copy scripts
- document cutover and final-sync approach

### Common mistakes
- agent deployed on distant or constrained network path
- jobs run successfully but namespace/permission outcome not validated
- Storage Mover used without understanding source and target semantics

---

## 9.4 Data transfer troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Bulk transfer finished but workload can’t cut over | cutover design | final delta sync, permissions, namespace mapping, app path updates |
| Storage Mover slow or inconsistent | agent placement | network locality, source bottlenecks, endpoint config, agent health |
| Data Box order completed but validation fails | transfer verification | logs, target path mapping, checksum/integrity process, missing delta data |
| Team uncertain which transfer tool to use | method fit | data volume, time window, bandwidth, repeatability, source/target type |

---

# 10. Azure Arc-enabled Servers and Machine Configuration

Microsoft documents Arc-enabled server extensions, extension security, policy built-ins, Machine Configuration, and governance/policy patterns for Arc-enabled servers. [26][27][28][29][30]

## 10.1 Technical purpose

Use Azure Arc-enabled servers for:
- hybrid server inventory and governance
- extension-based operational tooling
- centralized policy and machine-level configuration audit/remediation
- consistent control plane across Azure and non-Azure machines

## 10.2 Core entities

### Connected machine
Hybrid or non-Azure server registered to Azure control plane.

### Extension
Operational add-on for monitoring, patching, scripts, and other post-deployment tasks.

### Policy
Azure Policy applied to Arc-enabled machines.

### Machine Configuration
Audit/configure OS settings as code.

---

## 10.3 Extensions and post-migration operations

Microsoft documents Arc VM extensions as optional add-ons published through Marketplace-backed mechanisms and used for monitoring, patching, and script execution. [26][27]

### Best practices
- use extensions intentionally, not as uncontrolled drift layer
- standardize approved extensions
- validate extension prerequisites and lifecycle ownership
- use Arc to normalize post-migration operations for hybrid estates

### Common mistakes
- Arc connected, but operational tooling is still inconsistent by environment
- too many extensions used as ad hoc remote-management shortcuts
- security review of third-party or optional extension use is weak

---

## 10.4 Policy and machine configuration

Microsoft documents built-in Arc policies and Machine Configuration for auditing or configuring OS settings as code. [28][29][30]

### Best practices
- use policy and machine configuration to turn “migration complete” into governed steady state
- define audit versus configure intent explicitly
- route policy findings to server-owner workflows
- use Arc governance to reduce post-migration configuration drift

### Common mistakes
- Arc onboarded as inventory only
- policy assigned with no exception process
- guest state and Azure resource state are treated as same thing operationally

---

## 10.5 Arc troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Server connected to Arc but not operationally useful | governance gap | missing extensions, no policy assignments, no machine config workflow |
| Extension deployment fails or behaves inconsistently | extension lifecycle | prerequisites, permissions, network path, publisher support |
| Post-migration drift grows | control model | machine configuration, policy ownership, exception handling, manual changes |
| Hybrid estate feels fragmented | operating model | Arc scope, RBAC, tagging, extension standards, update integration |

---

# 11. Azure Load Testing and Azure Chaos Studio

Microsoft documents Azure Load Testing, key concepts, server-side metrics capture, results dashboards, and the mission-critical guide for continuous validation with Load Testing and Chaos Studio. Microsoft also documents that Chaos Studio now uses **Workspaces** as the current resource model in public preview, while **Experiments (classic)** remains the generally available legacy model and Workspaces is replacing it. [31][32][33][34][35][36][37]

## 11.1 Azure Load Testing

### Technical purpose
Generate controlled load to validate performance, bottlenecks, and scaling behavior.

### Core entities
- load test
- test engine
- test run
- script or quick URL test
- metrics and server-side component capture

### Best practices
- define hypothesis before running load
- correlate client-side and server-side metrics
- include realistic request mix and data patterns
- align pass/fail thresholds to SLOs, not raw curiosity

### Common mistakes
- tests measure peak throughput but not user-critical latency percentiles
- test scripts are unrealistic compared with production behavior
- teams celebrate successful load run without checking backend saturation or throttling

---

## 11.2 Server-side metrics and result interpretation

Microsoft documents attaching Azure components and capturing server-side metrics during tests. [33]

### Best practices
- always collect server-side metrics for bottleneck attribution
- compare runs across controlled baselines
- evaluate latency, error rate, resource saturation, and retry behavior together
- keep test results as evidence artifacts, not screenshots only

### Common mistakes
- only client-side success rate reviewed
- one noisy run treated as stable truth
- tests repeated without environment baseline consistency

---

## 11.3 Azure Chaos Studio

Microsoft documents Chaos Studio overview, Workspaces as the current preview model, and Experiments (classic) as the legacy GA resource model. It also documents targets and capabilities as key blast-radius controls. [34][35][36][37]

### Technical purpose
Inject faults deliberately to validate resilience assumptions.

### Core entities
- workspace or experiment model
- scenario / experiment
- target
- capability
- fault
- step / branch / sequencing
- stop condition and monitoring evidence

### Best practices
- start with low-blast-radius experiments
- choose faults that test real resilience hypotheses
- define stop conditions before injection
- run chaos with observability and incident staff prepared
- capture evidence for improvement, not entertainment

### Common mistakes
- chaos run with no rollback or stop criteria
- faults selected for drama rather than learning
- resource target permissions/capabilities poorly understood
- success declared because system did not fully crash, even though SLO was violated

### Expert nuance
Chaos is not “breaking things.”  
It is controlled falsification of resilience assumptions.

---

## 11.4 Continuous validation

Microsoft’s architecture guidance recommends regular integration of load and chaos validation into the development cycle for mission-critical workloads. [32]

### Best practices
- run load and chaos on cadence
- tie experiments to architecture risks
- use preconditions, monitoring, and post-test reviews
- feed lessons into backlog, runbooks, and Well-Architected actions

### Common mistakes
- tests run once before go-live only
- no ownership for fixing findings
- load and chaos isolated from release governance

---

## 11.5 Resilience-testing troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Load test results inconsistent across runs | baseline control | environment drift, data set, script randomness, backend dependency state |
| System survives load but user experience still poor | metric choice | percentile latency, error rate, retries, downstream bottlenecks |
| Chaos experiment too risky to approve | experiment design | blast radius, target/capability scope, stop conditions, observability readiness |
| Chaos test “passed” but confidence unchanged | learning quality | hypothesis clarity, measurable outcomes, evidence capture, follow-up actions |

---

# 12. Azure Well-Architected Review (WAR)

Microsoft documents the Azure Well-Architected Framework, workload concept, and the Well-Architected Review assessment that produces recommendations and exports for continuous improvement. [38][39][40][41]

## 12.1 Technical purpose

Use Well-Architected Review to:
- assess workload quality across the five pillars
- expose structural risks and technical debt
- generate actionable recommendations
- create improvement plans with evidence

## 12.2 Core entities

### Workload
Collection of application resources, code, data, AI models, and supporting infrastructure that achieve business outcomes.

### Assessment
Structured review across reliability, security, cost optimization, operational excellence, and performance efficiency.

### Recommendation
Actionable output mapped to gaps and guidance.

### Improvement plan
Operational backlog or roadmap created from findings.

---

## 12.3 Assessment operating model

Microsoft documents that the review contains multiple subassessments and recommends selecting the **Core Well-Architected Review** to evaluate the full workload. [40]

### Best practices
- assess one real workload at a time
- involve workload team and centralized stakeholders together
- export recommendations into engineering workflow
- re-run review after major architectural change

### Common mistakes
- assessment done by one architect in isolation
- recommendations exported but not prioritized
- review treated as architecture ceremony rather than improvement program

---

## 12.4 Well-Architected Review troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Review output feels generic | assessment quality | wrong scope, incomplete stakeholders, shallow workload context |
| Recommendations don’t get implemented | operating model | no owner mapping, no backlog integration, no follow-up cadence |
| Team disagrees with findings | requirement clarity | business priorities, nonfunctional requirements, evidence base |
| Review repeated but little changes | governance loop | action-plan tracking, ownership, architecture-decision follow-through |

---

# 13. End-to-End Reference Architecture for DR and Migration Execution

An enterprise must:
- protect critical VMware and physical workloads
- migrate application estates to Azure in waves
- move relational databases with bounded downtime
- transfer petabyte-scale files
- standardize hybrid governance
- validate resilience claims before executive signoff

A strong Week 6 design could look like this:

### DR
- Azure Site Recovery for machine replication and recovery plans
- test failovers on isolated networks every defined cycle
- explicit post-failover validation checklist covering DNS, identity, app transactions, and monitoring

### Migration planning
- Azure Migrate appliance with validated discovery scope
- dependency analysis informing groups and waves
- Cloud Adoption Framework migration-wave planning used to align technical and business sequencing

### Database cutover
- DMS migration mode chosen from downtime truth
- cutover rehearsal with rollback threshold and authoritative business validation

### Region moves
- Resource Mover used only after target quotas, support matrix, and shared-service dependencies are validated

### Bulk and file data movement
- Data Box for large offline bulk transfer when bandwidth is limiting
- Storage Mover for managed staged migration of file/folder estates
- final sync and permission validation before production cutover

### Hybrid governance
- Arc onboarded for migrated and remaining hybrid servers
- approved extensions, policy, and machine configuration enforce steady-state operations

### Resilience validation
- Load Testing for scale and latency evidence
- Chaos Studio for resilience hypothesis testing
- Well-Architected Review converts findings into engineering action plan

This is the Week 6 principle:

> recovery, migration, and resilience become credible only when rehearsed, measured, and tied to rollback-capable execution plans.

---

# 14. Troubleshooting Flow

When Week 6 execution fails, inspect in this order:

1. **Objective truth**  
   Was the goal recovery, relocation, migration, or validation?

2. **Dependency truth**  
   Which systems had to move, fail over, or validate together?

3. **Cutover truth**  
   What was the exact switch point, and who owned it?

4. **Rollback truth**  
   Was rollback technically possible, or only described?

5. **Evidence truth**  
   What logs, metrics, migration status, or test artifacts prove the current state?

6. **Steady-state truth**  
   After recovery or migration, can the system operate normally, or only boot?

7. **Governance truth**  
   Were ownership, approvals, and post-event actions defined clearly enough to act?

---

# 15. Common Week 6 Mistakes

- DR plan exists, but no recent test failover evidence
- Site Recovery plan is infrastructure-centric rather than application-centric
- migration assessment treated as migration strategy
- dependency analysis observation window too short
- migration waves too large for rollback
- online vs offline database migration mode chosen without business downtime agreement
- rollback described but not engineered
- Resource Mover used before destination readiness is verified
- Data Box transfer completed without final delta or integrity process
- Storage Mover jobs succeed but permissions and namespace semantics break cutover
- Arc connected machines not brought under policy and extension standards
- chaos testing run without stop criteria
- load testing used for vanity throughput instead of SLO evidence
- Well-Architected Review findings exported but not operationalized

---

# 16. Revision Checklist

You should be able to explain clearly:

- how Site Recovery replication, recovery plans, test failover, failover, reprotection, and failback fit together
- why a successful boot is not the same as successful disaster recovery
- how Azure Migrate discovery, dependency analysis, assessments, and migration waves fit together
- how to choose DMS online vs offline migration modes
- how Resource Mover’s prepare/initiate/commit flow differs from workload cutover planning
- how to choose between Data Box and Storage Mover for large data movement
- how Arc extensions, policy, and machine configuration turn hybrid servers into governed assets
- how Load Testing and Chaos Studio validate resilience claims differently
- how Well-Architected Review findings become a continuous improvement backlog

---

# 17. Memory Anchors

- **ASR** = orchestrated disaster recovery
- **recovery plan** = ordered recovery unit for an application
- **test failover** = isolated DR drill without production impact
- **reprotection** = reverse replication after failover
- **Azure Migrate** = discovery, assessment, dependency, wave planning
- **migration wave** = controlled execution batch
- **DMS** = structured database migration and cutover
- **offline migration** = downtime starts at migration start
- **online migration** = downtime concentrated at cutover
- **Resource Mover** = dependency-aware cross-region move orchestration
- **Data Box** = offline bulk data transfer
- **Storage Mover** = managed file/folder migration service
- **Arc-enabled server** = hybrid machine in Azure control plane
- **Machine Configuration** = guest/OS state audit and config as code
- **Load Testing** = performance validation under load
- **Chaos Studio** = controlled fault injection for resilience validation
- **Well-Architected Review** = workload assessment and recommendation engine

---

# 18. Official References

## Azure Site Recovery
- Azure Site Recovery documentation  
  https://learn.microsoft.com/en-us/azure/site-recovery/

- About recovery plans in Azure Site Recovery  
  https://learn.microsoft.com/en-us/azure/site-recovery/recovery-plan-overview

- Run a test failover to Azure  
  https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-test-failover-to-azure

- Run a failover during disaster recovery  
  https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-failover

- About failover and failback in Azure Site Recovery - Modernized  
  https://learn.microsoft.com/en-us/azure/site-recovery/failover-failback-overview-modernized

- Monitor and troubleshoot Site Recovery  
  https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-monitor-and-troubleshoot

## Azure Migrate and DMS
- Azure Migrate documentation  
  https://learn.microsoft.com/en-us/azure/migrate/

- Dependency analysis in Azure Migrate  
  https://learn.microsoft.com/en-us/azure/migrate/concepts-dependency-visualization?view=migrate

- Agentless dependency analysis in Azure Migrate  
  https://learn.microsoft.com/en-us/azure/migrate/how-to-create-group-machine-dependencies-agentless?view=migrate

- Create migration waves in Azure Migrate  
  https://learn.microsoft.com/en-us/azure/migrate/how-to-plan-create-waves?view=migrate

- Plan your migration  
  https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/migrate/plan-migration

- Migration wave planning  
  https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/migrate/migration-wave-planning

- Azure Database Migration Service documentation  
  https://learn.microsoft.com/en-us/azure/dms/

- What is Azure Database Migration Service?  
  https://learn.microsoft.com/en-us/azure/dms/dms-overview

- Azure DMS FAQ  
  https://learn.microsoft.com/en-us/azure/dms/faq

## Resource Mover
- Azure Resource Mover documentation  
  https://learn.microsoft.com/en-us/azure/resource-mover/

- About the move process in Azure Resource Mover  
  https://learn.microsoft.com/en-us/azure/resource-mover/about-move-process

- Move resources across regions using PowerShell in Resource Mover  
  https://learn.microsoft.com/en-us/azure/resource-mover/tutorial-move-region-powershell

- Azure CLI move-collection reference  
  https://learn.microsoft.com/en-us/cli/azure/resource-mover/move-collection?view=azure-cli-latest

## Data Box and Storage Mover
- Azure Data Box documentation  
  https://learn.microsoft.com/en-us/azure/databox/

- Azure Data Box overview  
  https://learn.microsoft.com/en-us/azure/databox/data-box-overview

- Azure Data Box security overview  
  https://learn.microsoft.com/en-us/azure/databox/data-box-security

- Azure Data Box Disk overview  
  https://learn.microsoft.com/en-us/azure/databox/data-box-disk-overview

- Azure Storage Mover documentation  
  https://learn.microsoft.com/en-us/azure/storage-mover/

- Introduction to Azure Storage Mover  
  https://learn.microsoft.com/en-us/azure/storage-mover/service-overview

- Deploy an Azure Storage Mover agent  
  https://learn.microsoft.com/en-us/azure/storage-mover/agent-deploy

## Azure Arc
- Azure Arc-enabled servers policy reference  
  https://learn.microsoft.com/en-us/azure/azure-arc/servers/policy-reference

- VM extension management with Azure Arc-enabled servers  
  https://learn.microsoft.com/en-us/azure/azure-arc/servers/manage-vm-extensions

- Extensions security for Azure Arc-enabled servers  
  https://learn.microsoft.com/en-us/azure/azure-arc/servers/security-extensions

- Cloud-native governance and policy with Azure Arc-enabled servers  
  https://learn.microsoft.com/en-us/azure/azure-arc/servers/cloud-native/governance-policy

- Azure Machine Configuration documentation  
  https://learn.microsoft.com/en-us/azure/governance/machine-configuration/

## Resilience validation and review
- Azure Load Testing overview  
  https://learn.microsoft.com/en-us/azure/app-testing/load-testing/overview-what-is-azure-load-testing

- Azure Load Testing concepts  
  https://learn.microsoft.com/en-us/azure/app-testing/load-testing/concept-load-testing-concepts

- Create and run a load test  
  https://learn.microsoft.com/en-us/azure/app-testing/load-testing/quickstart-create-and-run-load-test

- Monitor server-side metrics in Azure Load Testing  
  https://learn.microsoft.com/en-us/azure/app-testing/load-testing/how-to-monitor-server-side-metrics

- Understand Azure Load Testing results dashboard  
  https://learn.microsoft.com/en-us/azure/app-testing/load-testing/how-to-understand-test-run-results-dashboard

- Continuous validation with Azure Load Testing and Azure Chaos Studio  
  https://learn.microsoft.com/en-us/azure/architecture/guide/testing/mission-critical-deployment-testing

- Azure Chaos Studio overview  
  https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-overview

- Chaos Studio Workspaces documentation  
  https://learn.microsoft.com/en-us/azure/chaos-studio/

- Experiments (classic) overview  
  https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-chaos-experiments

- Targets and capabilities for Experiments (classic)  
  https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-targets-capabilities

- Azure Well-Architected Framework  
  https://learn.microsoft.com/en-us/azure/well-architected/

- Azure Well-Architected Framework workloads  
  https://learn.microsoft.com/en-us/azure/well-architected/workloads

- Complete an Azure Well-Architected Review assessment  
  https://learn.microsoft.com/en-us/azure/well-architected/design-guides/implementing-recommendations

- Azure Well-Architected Review assessment  
  https://learn.microsoft.com/en-us/assessments/azure-architecture-review/

---

# 19. Final Summary

Week 6 is about **making Azure workloads survivable, movable, and operationally honest under disruption and change**.

The strongest mental model is:

- **Site Recovery** = replicated disaster-recovery execution
- **Azure Migrate** = discovery, assessment, and wave planning
- **DMS** = database cutover execution
- **Resource Mover** = structured cross-region resource relocation
- **Data Box + Storage Mover** = bulk and file-data transfer execution
- **Arc** = hybrid governance continuity during and after migration
- **Load Testing + Chaos Studio** = resilience validation with evidence
- **Well-Architected Review** = structured improvement loop

When you can explain the **recovery model, migration model, cutover model, rollback model, and validation model** together, Azure Week 6 is understood architecturally.
