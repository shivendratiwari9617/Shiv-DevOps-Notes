# GCP Study Notes — Week 6
## Resilience, Backup and DR, Migration Strategy, Data Transfer, and Cutover Operations

> This is the **high-depth Week 6 guide**, written to sit close to the **9.5+ bar**.  
> It is structured as **expert-level study material** with:
> - technical purpose
> - core entities
> - architecture fit
> - recovery and migration mechanics
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** tight structure, high signal, minimal filler.

---

# 1. Why Week 6 Matters

Week 6 is about **how systems survive failure and how workloads move safely into Google Cloud**.

If:
- Week 1 = governance
- Week 2 = networking
- Week 3 = execution and delivery
- Week 4 = storage and data services
- Week 5 = observability, FinOps, and security operations

then Week 6 = **resilience and change at platform scale**.

This is where cloud architecture stops being static design and becomes:
- backup policy
- recovery discipline
- migration sequencing
- cutover planning
- rollback planning
- data transfer control
- realistic service continuity

The core idea is:

> **A migration is successful only if the target architecture works, the cutover is controlled, the rollback path is known, and the recovery posture is tested.**

Week 6 is not just about “moving workloads.”  
It is about:
- knowing what exists
- grouping dependencies correctly
- selecting the right migration path
- protecting data during and after migration
- minimizing downtime
- proving recoverability

---

# 2. Learning Outcomes

By the end of Week 6, you should be able to:

- explain **backup strategy**, **restore strategy**, **recovery point objective (RPO)**, **recovery time objective (RTO)**, failover, failback, and recovery-tier thinking
- explain **Backup and DR** backup vaults, backup plans, backup rules, data sources, recovery points, restore jobs, reports, and service-agent/KMS implications
- explain **Migration Center** discovery, assessment, grouping, migration preferences, cost estimation, application cataloging, and migration waves
- explain **Migrate to Virtual Machines** replication, test clones, cutover, migration lifecycle, and when image import is not enough
- explain **Migrate to Containers** fit assessment, transformation workflow, generated artifacts, and platform-target tradeoffs
- explain **Database Migration Service (DMS)** connection profiles, migration jobs, full load, CDC patterns, validation, and cutover risks
- explain **Storage Transfer Service** transfer jobs, agent pools, event-driven transfers, and transfer design patterns
- explain **Transfer Appliance** lifecycle, attestation, offline ingestion posture, and chain-of-custody style concerns
- distinguish **backup**, **replication**, **synchronization**, **migration**, and **DR**
- troubleshoot failed backups, restore failures, migration stalls, replication lag, CDC issues, and cutover errors

---

# 3. Architectural Mental Model

Think in five layers:

1. **Recovery design layer**  
   RPO, RTO, backup tiers, restore patterns, failover/failback

2. **Discovery and assessment layer**  
   inventory, grouping, dependencies, fit assessment, cost estimation

3. **Migration execution layer**  
   replication, cloning, conversion, CDC, transfer jobs, appliance import

4. **Cutover control layer**  
   validation, freeze windows, final sync, DNS/app switch, rollback

5. **Operational proof layer**  
   restore tests, job reports, observability, runbooks, game days

The Week 6 principle is:

> **You do not really have a migration plan until you have a cutover plan, a rollback plan, and a validated recovery plan.**

---

# 4. Resilience Design Before Product Selection

Before products, master the concepts.

## 4.1 Backup vs replication vs migration vs DR

### Backup
Creates recoverable copies at points in time.

### Replication
Copies data continuously or periodically to another target.

### Migration
Moves workload state and execution to a new platform or location.

### Disaster recovery
The broader strategy for continuing or restoring service after failure.

### Critical distinction
- replication can propagate corruption
- backup can preserve recoverable points
- migration can use replication, but migration is not DR by itself
- DR includes people, process, cutover, and validation

## 4.2 RPO and RTO

### RPO
How much data loss you can tolerate.

### RTO
How long service can be unavailable.

### Best practices
- define these per service, not globally
- tie them to business impact
- validate whether the chosen product/process actually meets them

### Common mistakes
- one default RPO/RTO for every app
- backup schedule chosen with no restore-speed consideration
- “near-zero downtime” claimed with no cutover evidence

## 4.3 Recovery tiers

A practical tiering model:
- **Tier 0:** mission-critical, strict RPO/RTO, tested failover
- **Tier 1:** important production, fast restore, regular validation
- **Tier 2:** standard restore acceptable
- **Tier 3:** archive/compliance or rebuildable systems

### Best practices
- map apps to tiers explicitly
- align backup frequency and restore design to tier
- keep rebuildable systems from consuming Tier 0 budget

### Common mistakes
- every service labeled critical
- no tier-linked testing frequency

## 4.4 Failover and failback

### Failover
Move service to recovery environment.

### Failback
Return service to the primary environment after stabilization.

### Best practices
- define both
- document data reconciliation after failover
- rehearse application-dependent failback steps

### Common mistakes
- failover playbook exists; failback does not
- data divergence after failover not considered

---

# 5. Backup and DR

Google documents Backup and DR around backup vaults, backup plans, backup rules, data sources, restore jobs, job monitoring, reports, and deployment planning. [1][2][3][4][5][6][7][8][9][10]

## 5.1 Technical purpose

Use Backup and DR to:
- define centralized protection policy
- store immutable or indelible vaulted backups
- automate backup schedules and retention
- restore protected workloads through managed workflows
- monitor backup/restore activity

## 5.2 Core entities

### Backup vault
Logical storage/control boundary for vaulted backups.

### Backup plan
Policy object linking workloads to one or more backup rules and a vault.

### Backup rule
Frequency, window, and retention logic.

### Data source
Specific protected resource represented in the vault model.

### Backup
Discrete point-in-time recovery point child resource.

### Restore job
Recovery operation from selected backup to target environment.

### Service agent
Managed identity that needs appropriate access, including KMS access when CMEK is involved.

## 5.3 Backup vault model

Google documents backup vaults as holding data sources and backups, with backup as a point-in-time child resource. [2]

### Why it matters
This is a resource model, not just storage.
Operators need to understand:
- which workload maps to which data source
- which plan applies
- where restore points live
- how IAM and service-agent access affect restore

## 5.4 Backup plans and rules

Google documents backup plans as centralized management tools containing rules for frequency, windows, and retention. [1][3]

### Best practices
- separate plan design by recovery tier
- avoid one catch-all plan for all workloads
- define retention to match business and compliance needs
- review backup windows against production load windows

### Common mistakes
- every workload attached to same frequency/retention policy
- backup windows overlap peak periods
- retention too short for actual recovery needs

## 5.5 Restore thinking

### Important concept
A successful backup posture is measured by **reliable restore**, not backup completion rate alone.

### Best practices
- define restore targets and dependencies
- test representative restores
- validate app startup after data restore
- document whether restore is in-place, new target, or target-environment-specific

### Common mistakes
- backups “green” but restore path never tested
- restore job succeeds technically but app not usable
- DNS, config, or credentials not aligned after restore

## 5.6 Job monitoring and reports

Google documents job monitoring, backup/restore job logs in Cloud Logging, and prebuilt reports in BigQuery/Looker Studio-style reporting surfaces. [6][7][8]

### Best practices
- monitor failed/skipped backup jobs daily
- use reports for trend review, not only incident response
- route backup-failure alerts to workload owners and platform owners

### Common mistakes
- backup success reviewed monthly only
- restore failures discovered only during disaster
- job logs exist but no operational dashboard

## 5.7 KMS and service-agent implications

Google documents that backup and restore operations may require granting roles to the backup vault service agent for key access. [5]

### Best practices
- review CMEK dependencies before backup rollout
- test restore of encrypted workloads
- document service-agent access model per project

### Common mistakes
- backup succeeds for some resources, restore fails due to missing key access
- security hardening removes service-agent permissions with no recovery test

## 5.8 Backup and DR troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Backups failing for encrypted resources | IAM/KMS | service-agent access, CMEK permissions, vault project vs workload project |
| Backup completed but restore unusable | app recovery | app dependencies, credentials, config, startup validation |
| Restore job fails unexpectedly | target environment | restore target config, quotas, target permissions, network assumptions |
| Backup cost higher than expected | policy design | frequency, retention, too-broad plan scope, tier mismatch |
| Job dashboard shows skipped backups | scheduling/state | backup window overlap, resource state, plan assignment |

---

# 6. Migration Center

Google documents Migration Center around discovery, assessment, grouping, migration preferences, cost estimation, offline assessment, and migration waves. [11][12][13][14][15][16]

## 6.1 Technical purpose

Use Migration Center to:
- discover infrastructure
- assess migration candidates
- group servers and databases by application or objective
- compare migration scenarios
- estimate cost
- plan migration waves

## 6.2 Core entities

### Discovery client
Collector deployed in source environments.

### Inventory
Imported server and database data set.

### Assessment
Fit or readiness evaluation for migration paths.

### Migration preferences
Scenario assumptions used for recommendation/cost comparison.

### Application catalog
Logical grouping of related workloads.

### Migration wave
Planned execution cohort.

### Cost estimate
Scenario-based projected target cost model.

## 6.3 Discovery and assessment

Google documents discovery clients and infrastructure assessment as the basis for grouping workloads and comparing scenarios. [11][12]

### Best practices
- complete discovery before choosing migration tooling
- validate collected data quality
- identify business dependencies, not just technical host lists
- use offline assessment where online integration is not possible

### Common mistakes
- migrate by server list only
- dependency mapping ignored
- incomplete inventory leads to surprise cutover blockers

## 6.4 Migration preferences and cost estimation

Google documents migration preferences and cost-estimation scenarios. [12][14]

### Best practices
- compare multiple target options
- document sizing assumptions
- involve platform and finance stakeholders early
- treat estimate as planning input, not guaranteed outcome

### Common mistakes
- cost estimate taken as exact bill
- rightsizing assumptions never validated in performance tests

## 6.5 Migration waves

Google documents migration-wave planning and application cataloging. [15][16]

### Best practices
- wave by dependency and risk, not only by size
- separate pilot, medium-risk, and high-criticality waves
- define entry/exit criteria per wave
- keep rollback boundary clear for each wave

### Common mistakes
- giant “big bang” waves
- app tiers split across different waves without dependency strategy
- no success criteria before moving to next wave

## 6.6 Migration Center troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Migration plan misses major dependency | discovery/grouping | application catalog quality, dependency mapping, inventory completeness |
| Cost estimate unrealistic | assumption model | preference set, machine sizing, storage/network assumptions |
| Wave plan looks neat on paper but risky in practice | planning model | shared services, identity dependencies, data coupling, rollback scope |
| Offline assessment differs from later reality | data quality | stale metrics, incomplete source inventory, changed workload behavior |

---

# 7. Migrate to Virtual Machines

Google documents Migrate to Virtual Machines around lifecycle, architecture, replication, test clones, and cutover. [17][18][19][20][21]

## 7.1 Technical purpose

Use Migrate to Virtual Machines when you need:
- lift-and-shift style migration of VMs or VM disks
- reduced downtime through replication-based migration
- testing before final cutover
- migration from supported source environments such as vSphere, AWS, Azure, or VMware Engine

## 7.2 Core entities

### Source VM
Original migrated workload.

### Replication
Continuous disk-data replication into Google Cloud.

### VM clone / test clone
Bootable target copy used for testing.

### Cutover
Final move to production target.

### Migration job / group
Organized migration execution unit.

### Target VM or Persistent Disk
Final Google Cloud resource produced by migration.

## 7.3 Replication-based lifecycle

Google documents that Migrate to Virtual Machines continuously replicates disk data without source downtime, then supports test clones and predictable cutover. [18]

### Why it matters
This is not just image import.  
It supports:
- replication
- test clone creation
- iterative validation
- lower-risk cutover

## 7.4 Test clones and cutover

### Best practices
- create test clones early
- validate boot, app startup, dependencies, licensing, and monitoring
- document final synchronization and freeze window
- define rollback criteria before cutover begins

### Common mistakes
- test clone boots, but no application validation performed
- networking or IAM changed only at cutover
- source writes continue during final sync with no freeze strategy

## 7.5 Image import vs machine-image-like migration strategy

Google documents migration-strategy differences between image import and machine image import for complex multi-disk VMs. [21]

### Best practices
- use migration method that matches VM complexity
- consider disk layout, OS dependencies, and app state
- do not reduce complex application migration to a simplistic disk import mindset

### Common mistakes
- underestimating multi-disk or service-chain dependencies
- assuming imported disk equals production-ready VM

## 7.6 Best practices
- migrate by app dependency set, not VM count alone
- validate agent, startup, and observability behavior in target
- plan DNS, IP, and firewall changes explicitly
- keep source rollback path intact until business validation completes

## 7.7 Migrate to VMs troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Replication lags unexpectedly | source/transport | source load, network path, replication settings, disk churn rate |
| Test clone boots but app fails | app dependency | startup services, hardcoded IPs, licenses, secrets, external dependency reachability |
| Cutover succeeds technically but users fail | environment switch | DNS, firewall, IAM, load balancer registration, monitoring agents |
| Rollback takes too long | rollback design | source preservation, data divergence, final-sync strategy, runbook completeness |

---

# 8. Migrate to Containers

Google documents Migrate to Containers, CLI-based modernization, generated artifacts, target deployment to GKE or Cloud Run, CLI architecture, and planning best practices. [22][23][24][25][26]

## 8.1 Technical purpose

Use Migrate to Containers when you need:
- transform VM-based application components into containers
- modernize rather than merely lift-and-shift
- generate artifacts for GKE, GKE Autopilot, or Cloud Run patterns where supported

## 8.2 Core entities

### Transformation
Process of converting VM-based workload into containerized form.

### Fit assessment
Evaluation of suitability for containerization.

### Generated artifacts
Deployment descriptors, container specs, and related assets created by the tooling.

### Target platform
GKE, Autopilot, or Cloud Run depending on workload fit and generated output.

### Maintenance step
Post-transformation lifecycle of the migrated container workload.

## 8.3 Fit assessment and planning

### Best practices
- assess workload assumptions: init system, storage, local state, host dependencies
- classify workloads as container-friendly, container-possible-with-change, or poor fit
- disable or remove hardware/environment-specific services where appropriate

Google documents planning best practices including automatic disabling of certain hardware- or environment-specific services. [26]

### Common mistakes
- treating every VM workload as automatically container-ready
- ignoring host-level dependencies
- underestimating stateful local filesystem assumptions

## 8.4 Generated artifacts and target choice

Google documents that the CLI generates artifacts deployable to GKE or Cloud Run. [22][24]

### Best practices
- review generated artifacts instead of assuming they are production-ready as-is
- align target choice to runtime characteristics
- validate startup, health checks, persistence, and service identity after transformation

### Common mistakes
- generated YAML or config deployed with no engineering review
- Cloud Run chosen for workload needing host-like behavior or long-lived assumptions
- GKE chosen when app could have been simplified further first

## 8.5 Modernization mindset

### Important concept
Containerization is not success by itself.  
Operational success requires:
- observability
- config externalization
- secret management
- stateless design where possible
- health and restart behavior that matches platform expectations

## 8.6 Migrate to Containers troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Transformation completes but app crashes in container | app/runtime assumptions | filesystem paths, init/process model, missing OS service, permissions |
| Generated artifacts deploy but app is unhealthy | platform fit | health probes, ports, config injection, service identity, persistence assumptions |
| Containerized app performs worse than VM | modernization design | resource requests/limits, I/O assumptions, image composition, target platform mismatch |
| Team stuck in endless fixes | fit assessment | workload was poor container candidate, hidden dependencies, insufficient redesign budget |

---

# 9. Database Migration Service (DMS)

Google documents Database Migration Service around overview, connection profiles, migration jobs, regionality, conversion workspaces, and SQL conversion assistance. [27][28][29][30][31][32][33]

## 9.1 Technical purpose

Use DMS when you need:
- managed database migration into Google Cloud services
- full load plus ongoing replication/CDC-style migration patterns where supported
- safer controlled database cutover
- heterogeneous conversion support in supported scenarios

## 9.2 Core entities

### Connection profile
Saved source or destination connection definition.

### Migration job
The managed data-movement process binding source and destination profiles.

### Full load
Initial bulk copy of source data.

### CDC / ongoing replication
Continual transfer of changes after initial load in supported scenarios.

### Conversion workspace
Schema/code conversion workspace for heterogeneous migrations.

### Regional service model
Source/destination profiles, jobs, and workspaces reside in one region.

## 9.3 Regionality and connection profiles

Google documents DMS as fully regional, with related entities kept in a single region. [29]

### Best practices
- choose region deliberately
- keep connectivity, latency, and support considerations aligned
- version or document connection-profile changes carefully

### Common mistakes
- wrong region chosen relative to target or source connectivity
- profile changed without impact review
- source network path not stable enough for CDC phase

## 9.4 Migration jobs: full load and ongoing sync

Google documents migration jobs as the managed data-movement unit and emphasizes combining connection profiles with job settings and finalization steps. [28]

### Best practices
- validate schema compatibility before final load
- monitor full-load performance separately from CDC lag
- schedule cutover only after acceptable replication lag and application validation
- freeze writes or coordinate app quiescing for final cutover where required

### Common mistakes
- full load complete assumed equal to ready for cutover
- CDC lag ignored
- destination validation skipped because tables exist

## 9.5 Conversion workspaces

Google documents conversion workspaces and Gemini-assisted SQL conversion for Oracle/SQL Server to PostgreSQL scenarios. [30][31][32][33]

### Best practices
- treat converted schema/code as draft needing engineering review
- validate unsupported objects and behavior differences
- budget time for procedural SQL rewrites and application query changes
- track unresolved conversion issues explicitly

### Common mistakes
- automatic conversion assumed production-ready
- object-level incompatibilities found only during cutover week
- missing primary-key assumptions or object edits not reviewed

## 9.6 DMS troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Full load succeeds but app queries fail on target | schema compatibility | converted objects, indexes, collation, procedural code, grants |
| CDC lag grows | source/change rate | network path, source load, target apply rate, replication health |
| Migration job cannot start | connectivity/region | connection profile, region mismatch, source access, target permissions |
| Final cutover causes data mismatch | process control | write freeze, lag at cutover, validation queries, app retry traffic |

---

# 10. Storage Transfer Service

Google documents Storage Transfer Service around transfer jobs, sources and sinks, agents and agent pools, event-driven transfers, and performance/routing considerations. [34][35][36][37]

## 10.1 Technical purpose

Use Storage Transfer Service when you need:
- move data between storage systems and Cloud Storage
- recurring or one-time data transfer jobs
- managed transfer scheduling
- event-driven bucket copy patterns
- agent-based transfer from on-prem or supported endpoints

## 10.2 Core entities

### Transfer job
Defined source-to-destination movement process.

### Source / sink
Origin and destination storage locations.

### Agent pool
Managed group of agents for supported transfer types.

### Agent
Self-hosted component with access to source/destination.

### Manifest
Enumerated transfer input in supported scenarios.

### Event-driven transfer
Trigger-driven copy workflow based on bucket events.

## 10.3 Agent pools and network control

Google documents that some transfer types require agent pools and installed agents. [35]

### Best practices
- place agents close to data sources
- manage bandwidth and routing intentionally
- size agent pools to source performance and migration window
- monitor failures and retries

### Common mistakes
- one underpowered agent for large transfer
- no network path or firewall review
- transfer window shorter than real throughput allows

## 10.4 Event-driven transfers

Google documents event-driven transfers as distinct from ordinary replication and useful for separate namespaces and controlled data copying. [36][37]

### Best practices
- use when you need controlled copy semantics, not simple mirrored access
- define permissions for Pub/Sub subscriber service identity carefully
- document replay or duplicate-event expectations in downstream workflow

### Common mistakes
- confusing event-driven transfer with native bucket replication
- missing Pub/Sub permissions
- no lifecycle/retention plan for copied objects

## 10.5 Storage Transfer troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Transfer job slow | throughput/agent | agent placement, agent count, source read speed, network bandwidth |
| Event-driven transfer not firing | eventing/IAM | Pub/Sub permissions, service agent, source bucket events, filters |
| Data copied but namespace messy | design | prefix strategy, overwrite policy, destination layout |
| Job failures intermittent | source stability | network interruptions, source throttling, agent health |

---

# 11. Transfer Appliance

Google documents Transfer Appliance as a secure high-capacity device for offline data transfer to Cloud Storage, with attestation and operational procedure guidance. [38][39][40][41]

## 11.1 Technical purpose

Use Transfer Appliance when:
- datasets are too large for practical online transfer windows
- network bandwidth is constrained
- bulk offline ingestion is the best migration path

## 11.2 Core entities

### Appliance
Physical secure storage device.

### Attestation
Validation step to confirm device integrity on receipt.

### Shipment lifecycle
Order, receive, validate, load data, return, ingest to Cloud Storage.

### Transfer logs
Operational records for copy success/failure and ingest coordination.

## 11.3 Attestation and chain-of-custody style thinking

Google documents validation of the appliance before network connection using the attestation application. [39]

### Best practices
- validate appliance before use
- control who handles the device
- document data-loaded scope, encryption posture, and shipment timing
- preserve transfer logs and receipts

### Common mistakes
- appliance connected without attestation
- no operational ownership for shipping/receiving
- dataset selection or directory layout changed ad hoc during loading

## 11.4 When to choose Transfer Appliance vs Storage Transfer Service

### Transfer Appliance
Best for:
- large offline bulk ingestion
- constrained WAN
- one-time bulk seeding

### Storage Transfer Service
Best for:
- recurring transfer
- online accessible source
- scheduled or event-driven data movement

### Common mistake
Choosing online transfer for petabyte-scale constrained-network data with unrealistic timelines.

## 11.5 Transfer Appliance troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Upload timeline unrealistic | migration planning | dataset size, local copy speed, shipping timeline, ingest expectations |
| Integrity concerns on receipt | operational process | attestation result, shipping records, handling control |
| Missing files after ingest | copy process | transfer logs, source selection, directory layout, failed-copy review |

---

# 12. Cutover, Rollback, and Validation

Week 6 is not complete without disciplined cutover mechanics.

## 12.1 Cutover checklist

A real cutover plan should define:
- freeze window
- last replication or sync point
- validation queries or app checks
- traffic-switch method
- success criteria
- rollback trigger
- rollback deadline
- communications path

## 12.2 Validation types

### Technical validation
Does it boot? Is data present? Are services reachable?

### Functional validation
Does the application behave correctly?

### Business validation
Can users complete critical transactions?

### Best practice
Require all three.

## 12.3 Rollback design

### Best practices
- define rollback before migration starts
- preserve source or prior state long enough
- know whether rollback is technical, operational, or data-reconciliation heavy
- set a point after which rollback becomes too costly and alternate recovery path is needed

### Common mistakes
- “rollback” means “we hope source is still there”
- bidirectional changes after cutover make rollback impossible quickly
- no written rollback criteria

## 12.4 Post-cutover stabilization

### Best practices
- heightened monitoring window
- track latency, errors, throughput, cost, and data correctness
- hold change freeze during stabilization where appropriate
- document lessons for next wave

### Common mistakes
- next migration wave begins before previous wave is stable
- cost or performance regressions noticed weeks later

---

# 13. End-to-End Migration Example

An enterprise is moving a three-tier order platform from on-premises:
- web tier on VMs
- app tier on VMs
- PostgreSQL database
- 200 TB file/object-like archive data
- strict recovery obligations

A strong design could be:

### Discovery and planning
- Migration Center discovery client
- application catalog built by service dependency
- migration preferences compared for VM lift-and-shift vs partial modernization
- wave 1 for low-risk supporting services
- wave 2 for core app
- wave 3 for database and critical file workflows

### Recovery design
- Backup and DR plans by service tier
- restore tests before migration weekend
- runbook for failover and rollback

### Compute migration
- Migrate to Virtual Machines for web/app tiers
- replication and test clones
- final cutover with freeze window

### Database migration
- DMS full load + ongoing sync
- schema validation and application test
- controlled final cutover once lag acceptable

### Data transfer
- Transfer Appliance for initial bulk 200 TB archive seed
- Storage Transfer Service for delta or recurring movement

### Modernization opportunity
- selected stateless app component evaluated with Migrate to Containers for later phase, not forced into first-wave risk

This is the Week 6 principle:

> successful migration depends on dependency-aware planning, validated recovery, and disciplined cutover control.

---

# 14. Troubleshooting Flow

When migration or recovery work goes wrong, inspect in this order:

1. **Inventory truth**
   - Did we identify the right systems, dependencies, and data?

2. **Protection truth**
   - Do we have valid recovery points and restore paths?

3. **Connectivity truth**
   - Can source, target, agents, and services actually reach each other?

4. **Data-state truth**
   - Is replication current? Is CDC lag acceptable? Was final sync complete?

5. **Execution truth**
   - Did cutover steps happen in the right order?

6. **Validation truth**
   - Did we prove technical, functional, and business correctness?

7. **Rollback truth**
   - Can we still go back safely, and for how long?

---

# 15. Common Week 6 Mistakes

- migration planning without dependency mapping
- backup success confused with restore readiness
- no recovery-tier model
- one backup policy for everything
- test clone created but not functionally validated
- cutover plan with no rollback plan
- DMS full load mistaken for migration completion
- CDC lag ignored before switchover
- Transfer Appliance chosen with weak handling process
- Storage Transfer Service throughput assumptions unrealistic
- generated container artifacts treated as production-ready with no review
- next wave started before previous wave stabilization ends

---

# 16. Revision Checklist

You should be able to explain clearly:

- how backup, replication, migration, and DR differ
- how RPO and RTO change product and process choice
- how Backup and DR vaults, plans, rules, data sources, backups, and restore jobs fit together
- how Migration Center discovery, assessment, migration preferences, cost estimation, and waves fit together
- how Migrate to Virtual Machines replication, test clones, and cutover reduce migration risk
- how Migrate to Containers fit assessment and generated artifacts affect modernization outcomes
- how DMS connection profiles, migration jobs, full load, CDC, and conversion workspaces work
- when to use Storage Transfer Service vs Transfer Appliance
- how to structure cutover, validation, rollback, and stabilization

---

# 17. Memory Anchors

- **RPO** = tolerated data loss
- **RTO** = tolerated downtime
- **backup** = recoverable copy
- **replication** = copied ongoing state
- **migration** = move workload/platform
- **DR** = recover service under failure
- **backup vault** = vaulted backup boundary
- **backup plan** = policy linking resource to rules and vault
- **data source** = protected resource in vault model
- **restore job** = recovery operation
- **Migration Center** = discovery and planning
- **migration preferences** = scenario assumptions
- **migration wave** = grouped execution batch
- **Migrate to VMs** = replication-based VM migration
- **test clone** = migration rehearsal target
- **Migrate to Containers** = VM-to-container transformation
- **DMS** = managed database migration
- **connection profile** = source/destination definition
- **migration job** = DMS data-movement workflow
- **conversion workspace** = heterogeneous schema/code conversion area
- **Storage Transfer Service** = managed online data transfer
- **agent pool** = self-hosted transfer agent group
- **Transfer Appliance** = offline bulk ingest device
- **cutover** = production switch
- **rollback** = revert to prior service state

---

# 18. Official References

## Backup and DR
- Backup and DR overview  
  https://docs.cloud.google.com/backup-disaster-recovery/docs/concepts/backup-dr

- Backup plans overview  
  https://docs.cloud.google.com/backup-disaster-recovery/docs/concepts/backup-plan

- Backup vaults  
  https://docs.cloud.google.com/backup-disaster-recovery/docs/concepts/backup-vault

- Define backup policies  
  https://cloud.google.com/backup-disaster-recovery/docs/concepts/policy-overview

- Manage backup plans  
  https://docs.cloud.google.com/backup-disaster-recovery/docs/cloud-console/backup-plan-create

- Plan your deployment  
  https://docs.cloud.google.com/backup-disaster-recovery/docs/deployment/deployment-plan

- Monitor jobs in console  
  https://docs.cloud.google.com/backup-disaster-recovery/docs/monitor-reports/monitor-jobs-console

- Backup and restore job logs  
  https://docs.cloud.google.com/backup-disaster-recovery/docs/monitor-reports/backupvault-backup-recovery-jobs

- Backup and restore job reports  
  https://docs.cloud.google.com/backup-disaster-recovery/docs/monitor-reports/backup-restore-backupvault-report

- Restore method reference  
  https://cloud.google.com/backup-disaster-recovery/docs/reference/rest/v1/projects.locations.backupVaults.dataSources.backups/restore

## Migration Center
- Discovery and assessment overview  
  https://docs.cloud.google.com/migration-center/docs/discovery-and-assessment-overview

- Discovery client overview  
  https://docs.cloud.google.com/migration-center/docs/discovery-client-overview

- Run an offline assessment  
  https://cloud.google.com/migration-center/docs/run-offline-assessment

- Cost estimation overview  
  https://cloud.google.com/migration-center/docs/estimate/overview

- Plan migration waves  
  https://docs.cloud.google.com/migration-center/docs/plan-migration-waves

## Migrate to Virtual Machines
- Migrate to Virtual Machines docs  
  https://docs.cloud.google.com/migrate/virtual-machines/docs/5.0

- VM migration process lifecycle  
  https://docs.cloud.google.com/migrate/virtual-machines/docs/5.0/discover/lifecycle

- Migration journey  
  https://docs.cloud.google.com/migrate/virtual-machines/docs/5.0/discover/migrating-vms-migrate-for-compute-engine-getting-started

- Migrate individual VMs  
  https://docs.cloud.google.com/migrate/virtual-machines/docs/5.0/migrate/migrating-vms

- Architecture  
  https://docs.cloud.google.com/migrate/virtual-machines/docs/5.0/discover/architecture

- Best practices  
  https://docs.cloud.google.com/migrate/virtual-machines/docs/5.0/discover/migrating-vms-migrate-for-compute-engine-best-practices

- Image import versus machine image import  
  https://docs.cloud.google.com/migrate/virtual-machines/docs/5.0/migrate/migration-strategy

## Migrate to Containers
- Migrate to Containers docs  
  https://docs.cloud.google.com/migrate/containers/docs

- Overview / getting started  
  https://docs.cloud.google.com/migrate/containers/docs/getting-started

- CLI architecture  
  https://docs.cloud.google.com/migrate/containers/docs/m2c-cli/architecture

- Prepare for migration  
  https://docs.cloud.google.com/migrate/containers/docs/m2c-cli/prepare-for-migration

- Planning best practices  
  https://docs.cloud.google.com/migrate/containers/docs/planning-best-practices

- Downloads  
  https://docs.cloud.google.com/migrate/containers/docs/downloads

## Database Migration Service
- DMS overview  
  https://docs.cloud.google.com/database-migration/docs/overview

- Manage migration jobs  
  https://docs.cloud.google.com/database-migration/docs/manage-jobs

- Manage connection profiles  
  https://docs.cloud.google.com/database-migration/docs/manage-profiles

- About connection profiles and regionality  
  https://docs.cloud.google.com/database-migration/docs/sqlserver-to-csql-pgsql/about-connection-profiles

- Conversion workspaces  
  https://cloud.google.com/database-migration/docs/oracle-to-alloydb/about-conversion-workspaces

- SQL conversion with DMS  
  https://docs.cloud.google.com/database-migration/docs/convert-sql-with-dms

- Conversion workspace actions  
  https://docs.cloud.google.com/database-migration/docs/sqlserver-to-csql-pgsql/work-with-conversion-workspaces

## Data transfer
- Storage Transfer Service overview  
  https://docs.cloud.google.com/storage-transfer/docs/overview

- Create transfers  
  https://docs.cloud.google.com/storage-transfer/docs/create-transfers

- Event-driven transfers  
  https://docs.cloud.google.com/storage-transfer/docs/event-driven-transfers

- Event-driven transfers from Cloud Storage  
  https://docs.cloud.google.com/storage-transfer/docs/event-driven-google

- Storage Transfer API reference  
  https://docs.cloud.google.com/storage-transfer/docs/reference/rest

## Transfer Appliance
- Transfer Appliance overview  
  https://docs.cloud.google.com/transfer-appliance/docs/4.0/overview

- High-level overview / procedure guide  
  https://docs.cloud.google.com/transfer-appliance/docs/4.0/procedure-guide

- Receive and validate appliance  
  https://docs.cloud.google.com/transfer-appliance/docs/4.0/receive-appliance

- Transfer data  
  https://docs.cloud.google.com/transfer-appliance/docs/4.0/transfer-data

- Release notes  
  https://cloud.google.com/transfer-appliance/docs/release-notes

---

# 19. Final Summary

Week 6 is about **keeping workloads recoverable and moving them safely into Google Cloud**.

The strongest mental model is:

- **Backup and DR** = protection policy and recoverability
- **Migration Center** = discovery, assessment, and wave planning
- **Migrate to VMs** = replication-based VM relocation
- **Migrate to Containers** = modernization transformation
- **DMS** = managed database migration
- **Storage Transfer Service** = managed online data transfer
- **Transfer Appliance** = offline bulk ingest
- **cutover + rollback + validation** = where migration success is actually decided

When you can explain the **recovery model, migration model, cutover model, and rollback model** together, Week 6 is understood architecturally.
