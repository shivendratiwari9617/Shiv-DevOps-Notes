# AWS Study Notes — Week 6
## Disaster Recovery, Multi-Region, and Migration Execution

> This is the **deep technical Week 6 guide**.  
> It is written as a **service-by-service study README** with:
> - technical explanations
> - key entities
> - controls
> - operations
> - examples
> - best practices
> - common mistakes
> - troubleshooting cues
> - official references  
>
> **Noise control:** concise wording, high density, minimal filler.  
>
> **Naming note:** AWS documentation currently refers to the former **AWS Application Migration Service (AWS MGN)** as **AWS Transform MGN** in several places as of **October 3, 2026**. This guide uses **MGN** for continuity with your Week 6 scope, while noting the current documentation name where relevant. [1]

---

# 1. Why Week 6 Matters

Week 6 is about **surviving failure and executing change safely**.

If earlier weeks covered:
- governance
- networking
- compute
- data
- observability and operations

then Week 6 covers the hardest operational questions:

- What happens when a Region fails?
- What happens when a workload must fail over now?
- How do we prove recovery readiness before a disaster?
- How do we migrate servers, databases, and file data without losing control?
- How do we test resilience instead of just assuming it?
- How do we back up, restore, and validate at enterprise scale?

The core Week 6 idea is:

> **Recovery is not a document. It is a tested control system across routing, replication, backup, migration waves, validation, and failback.**

This week spans five domains:

1. **Multi-Region and failover control**
2. **Server and application migration**
3. **Database migration and schema conversion**
4. **Data transfer, backup, and restore validation**
5. **Resilience experiments and game days**

---

# 2. Week 6 Learning Outcomes

By the end of this week, you should be able to:

- explain **Amazon Application Recovery Controller (ARC)** clusters, routing controls, control panels, safety rules, readiness checks, recovery groups, cells, and resource sets
- explain **AWS Elastic Disaster Recovery (DRS)** source servers, replication servers, staging area subnets, recovery instances, recovery snapshots, failback, and drills
- explain **MGN / AWS Transform MGN** source servers, replication agents, launch settings, test instances, cutover instances, templates, and waves
- explain **Migration Hub** applications, discovered resources, migration tools, statuses, waves, and home Region behavior
- explain **AWS DMS** replication instances, serverless/project components where relevant, endpoints, tasks, subnet groups, task settings, and validation
- explain **schema conversion** entities such as migration projects, assessment reports, conversion rules, and converted objects
- explain **DataSync** agents, locations, tasks, executions, manifests, filters, reports, options, and validation flow
- explain **Transfer Family** servers, users, workflows, connectors, identity providers, protocols, host keys, and endpoints
- explain **AWS Backup** plans, vaults, recovery points, vault lock, copy jobs, restore jobs, legal holds, and restore testing
- explain **AWS Fault Injection Service (FIS)** experiment templates, experiments, actions, targets, stop conditions, roles, and reports
- connect DR controls, migration waves, and recovery testing into one operating model

---

# 3. Architectural Mental Model for Week 6

Think in layers:

1. **Failover control layer**  
   ARC, Route 53 failover dependencies, readiness modeling

2. **Server migration and DR replication layer**  
   DRS, MGN

3. **Portfolio execution layer**  
   Migration Hub, waves, status tracking, cutover governance

4. **Database and schema migration layer**  
   DMS, schema conversion, CDC, validation

5. **Data movement and partner transfer layer**  
   DataSync, Transfer Family

6. **Backup and restore assurance layer**  
   AWS Backup, Vault Lock, legal holds, restore testing

7. **Resilience validation layer**  
   FIS, failover rehearsal, game days

A mature Week 6 design connects all seven.

---

# 4. Recovery Strategy First: RTO, RPO, and Recovery Patterns

Before the services, define recovery intent.

## 4.1 RTO
**Recovery Time Objective** = maximum acceptable downtime.

## 4.2 RPO
**Recovery Point Objective** = maximum acceptable data loss window.

## 4.3 Common DR patterns
- **Backup and restore**
- **Pilot light**
- **Warm standby**
- **Active/passive multi-Region**
- **Active/active multi-Region**

## 4.4 Design rule
Do not choose tools first. Choose:
- business criticality
- RTO
- RPO
- failover authority
- test frequency
- failback requirements  
then choose services.

## 4.5 Common mistake
Calling a workload “DR-ready” because replication exists, without proving:
- traffic steering
- dependency readiness
- identity and secret availability
- data consistency handling
- restore or failback runbooks

---

# 5. Amazon Application Recovery Controller (ARC)

Amazon Application Recovery Controller helps assess readiness and control failover routing for multi-Region recovery. AWS states ARC provides readiness insights and helps move traffic across Regions or away from Availability Zones for disaster recovery. [2]

## 5.1 Technical purpose
ARC provides **recovery readiness analysis** and **manual failover control primitives**.

It does not replace application architecture.  
It strengthens and operationalizes it.

## 5.2 Core entities

### Cluster
A set of regional endpoints supporting routing control operations.

### Routing control
A simple on/off control that affects associated Route 53 health-check-based routing behavior.

### Control panel
Logical grouping of routing controls.

### Safety rule
Guardrail rule that prevents unsafe routing-control state combinations.

### Readiness check
Assessment that evaluates whether resources are configured in a way that supports recovery.

### Recovery group
Logical collection representing an application or portion of it for readiness assessment.

### Cell
A partition or recovery unit representing an application slice or environment component.

### Resource set
Named collection of resources associated with readiness checks.

## 5.3 Technical role

ARC is strongest when:
- workloads already have multi-Region replicas
- Route 53 DNS failover is part of the architecture
- operators need explicit failover controls
- teams need evidence that a recovery topology is actually ready

## 5.4 Routing controls

AWS documents routing controls as on/off switches integrated with specific Route 53 health checks used to reroute traffic during failover. [3]

### Example
You have:
- primary Region endpoint
- secondary Region endpoint
- DNS failover records fronting both

Routing controls let operators explicitly switch traffic state rather than relying only on passive health behavior.

### Best practices
- define clear primary/secondary control naming
- document who may flip routing controls
- test routing state changes in game days
- connect control changes to incident command process

### Common mistakes
- no human process around routing authority
- routing controls exist, but app/data dependencies are not actually ready
- DNS TTL/client caching behavior not accounted for

---

## 5.5 Safety rules

### Technical purpose
Prevent unsafe combinations such as:
- both regions off
- both regions on when architecture expects single active
- too few healthy cells serving traffic

### Best practices
- use safety rules aggressively in critical failover designs
- model allowed control states explicitly

### Common mistakes
- no safety rules on critical routing controls
- safety rules too loose to be useful
- safety rules too strict and untested, blocking emergency action

---

## 5.6 Readiness checks, recovery groups, cells, and resource sets

AWS documents readiness checks, recovery groups, cells, and resource sets as core constructs for assessing recovery readiness and monitoring health for failover decisions. [4]

### Technical role
These model whether the resources needed for recovery are configured and available.

### Example
A recovery group for `checkout-app` might include:
- ALB in secondary Region
- target compute capacity
- database replica/global construct
- key dependencies
- Route 53 records and health relationships

Cells could represent:
- regional app slice A
- regional app slice B

### Best practices
- scope resource sets to actual recovery dependencies
- align cells with real failure domains
- keep recovery group definitions current as architecture changes
- collect evidence from readiness checks during rehearsal

### Common mistakes
- abstract modeling with no mapping to actual infrastructure
- forgetting supporting dependencies like KMS, secrets, identity, networking, or observability
- assuming readiness check coverage equals end-to-end business recovery proof

---

## 5.7 ARC operations

Typical operations:
- create routing controls and control panels
- define safety rules
- map readiness checks to recovery groups/resource sets
- rehearse failover
- gather game-day evidence
- update models when application topology changes

### Troubleshooting cues
- routing control change did not move traffic → inspect Route 53 integration, health checks, DNS behavior, client caching
- readiness issue persists → inspect resource set scope, underlying unsupported/misconfigured resources, stale modeling
- failover worked partially → dependency graph likely incomplete, not routing alone

---

# 6. AWS Elastic Disaster Recovery (AWS DRS)

AWS DRS is the service for disaster recovery replication and rapid recovery of servers and applications. AWS documents that it minimizes downtime and data loss using affordable storage, minimal compute, and point-in-time recovery. [5]

## 6.1 Technical purpose
Provide **disaster recovery replication and recovery** for source servers.

## 6.2 Core entities

### Source server
Replicated server protected by DRS.

### Replication agent
Agent installed on the source server to replicate disk changes.

### Replication server
Lightweight service-managed replication infrastructure used during staging/replication operations.

### Staging area subnet
Subnet where replication-related staging resources operate.

### Recovery instance
Launched server in AWS during test or disaster recovery.

### Recovery snapshot
Recovery point / snapshot basis used for recovery launches.

### Recovery point in time
Selectable point for recovery launch depending on available replicated data history.

### Failback settings
Configuration and workflow for returning recovered workloads back to original or alternate environments.

### Post-launch actions
Automated actions run after recovery instance launch.

## 6.3 How DRS works technically

High-level flow:
1. Initialize DRS in target Region
2. Install replication agent on source server
3. Continuous block-level replication into AWS staging area
4. Launch test recovery or actual recovery instance
5. Validate application
6. Perform failback if needed

AWS’s documented getting-started flow includes initializing in target Region, installing the replication agent, waiting for initial sync, and then launching recovery workflows. [6]

## 6.4 Example
An on-prem Windows application server is protected by DRS:
- replication agent installed
- continuous replication to AWS staging subnet
- quarterly non-disruptive recovery drill
- post-launch action joins recovery instance to target domain
- failback procedure documented

## 6.5 Best practices
- isolate staging area subnets and control access tightly
- define post-launch actions for app readiness tasks
- run regular non-disruptive recovery drills
- monitor replication lag and source health
- document failback, not just failover

## 6.6 Common mistakes
- agent installed but no recovery drill ever performed
- no dependency mapping between replicated servers
- underestimating DNS, secrets, license, or middleware dependencies after launch
- assuming replicated infrastructure alone equals application recovery

## 6.7 Troubleshooting cues
- replication lag high → inspect agent health, bandwidth, source I/O behavior, staging resources
- recovery instance boots but app fails → inspect post-launch actions, networking, credentials, hostname assumptions, external dependencies
- failback complexity high → original environment changed beyond what failback design assumed

---

# 7. MGN / AWS Transform MGN

AWS documentation now refers to the service as **AWS Transform MGN**, but the operational concepts remain those of **AWS Application Migration Service (MGN)** in many migration programs. AWS states it is a highly automated lift-and-shift rehost solution. [1]

## 7.1 Technical purpose
Lift-and-shift server migration to AWS with test launch and cutover workflows.

## 7.2 Core entities

### Source server
Server being migrated.

### Replication agent
Agent that replicates source server data.

### Replication settings template
Reusable template for replication behavior across source servers.

### Launch settings
General launch settings and EC2 launch template settings that determine how test and cutover instances are launched. AWS documents launch settings as having these two sections. [7]

### Test instance
Temporary launch used for validation before cutover.

### Cutover instance
Production migration launch used during final migration.

### Wave
Logical migration grouping for execution planning.

### Post-launch actions
Automation run after launch.

### Migration lifecycle status
State of the source server through test and cutover process.

## 7.3 How MGN works technically

Typical flow:
1. Install replication agent
2. Replicate source server to AWS
3. Configure replication template and launch settings
4. Launch test instances
5. Validate application
6. Mark wave or server ready for cutover
7. Launch cutover instances
8. Finalize cutover
9. Execute rollback/fallback plan if necessary

AWS documents wave actions such as test launches and marking servers as ready for cutover, and cutover launches that transition lifecycle state to cutover in progress. [8][9]

## 7.4 Waves

### Technical role
Waves group servers by migration dependency, business unit, application set, or cutover schedule.

### Example
Wave 1:
- DNS utility server
- license server
- shared app gateway

Wave 2:
- app servers
- batch workers

Wave 3:
- reporting nodes

### Best practices
- build waves by dependency order, not org chart alone
- include rollback criteria per wave
- validate shared service dependencies before app waves
- track readiness with business and technical owners

### Common mistakes
- waves too large to manage safely
- no business freeze window planning
- cutting over app servers before shared dependencies are ready

---

## 7.5 Launch settings

AWS documents launch settings as general launch settings plus an EC2 launch template section. [7]

### Technical role
Control:
- instance launch behavior
- networking
- instance type
- tags
- boot settings
- test vs cutover behavior

### Best practices
- review launch settings per server class
- do not assume source server hardware profile maps directly to AWS defaults
- separate test launch validation from final cutover assumptions

### Common mistakes
- wrong subnet/security group during test launch
- unsuitable instance type mapping
- no launch-template review for production cutover

---

## 7.6 Test launches and cutover launches

AWS documents both test launch and cutover launch workflows, including cutover lifecycle progression and finalization. [8][9]

### Example
A finance app server:
- runs test launch in isolated subnet
- passes validation
- enters ready-for-cutover state
- cutover launch happens during weekend freeze window
- DNS updated after application validation

### Best practices
- always perform test launches before cutover
- document validation checklist
- define explicit rollback point and stop/go authority
- preserve evidence from test results

### Common mistakes
- no meaningful test launch, only “server booted”
- test environment too different from cutover environment
- no rollback window or dependency freeze during cutover

---

# 8. AWS Migration Hub

Migration Hub provides centralized migration tracking across multiple tools and partner solutions. AWS documents it as a single place to track migration tasks and project status. [10]

## 8.1 Technical purpose
Track migration portfolio status across tools, apps, waves, and discovered resources.

## 8.2 Core entities

### Home Region
Central Region where Migration Hub metadata and tracking are managed.

### Application
Logical grouping of servers/resources for migration tracking.

### Server
Discovered or migrated server entity.

### Migration tool integration
Connection to source migration tooling such as MGN, DRS, DMS, and partner tools.

### Migration status
State reported for tracked migration resources.

### Discovered resource
Server or database discovered through connected assessment/discovery tools.

### Wave
Execution grouping for migration program planning.

## 8.3 Example
A portfolio migration office tracks:
- 120 servers
- 8 apps
- 6 waves
- MGN for rehosting
- DMS for DB migration  
from one home Region dashboard.

## 8.4 Best practices
- choose the home Region early and keep it stable
- group resources into applications, not just raw server lists
- align wave naming with migration governance
- use it as program control, not as the only technical truth

## 8.5 Common mistakes
- no consistent application grouping
- tool integrations enabled without process ownership
- status tracking that does not reflect real cutover readiness

## 8.6 Troubleshooting cues
- migration status mismatch → inspect source tool integration or stale updates
- application grouping confusion → governance issue more than service issue
- stakeholders cannot report wave readiness → migration metadata model likely weak

---

# 9. AWS DMS and Schema Conversion

Week 6 includes both data movement and schema conversion. AWS documents DMS as a service that can discover sources, convert schemas, and migrate data. [11]

---

## 9.1 AWS Database Migration Service (DMS)

### Technical purpose
Migrate and replicate relational, warehouse, NoSQL, and other supported data stores using full load and/or CDC.

### Core entities

### Replication instance
Managed compute used for migration tasks in classic DMS mode.

### Source endpoint
Connection definition for source database or datastore.

### Target endpoint
Connection definition for target database or datastore.

### Replication task
Migration job that performs full load, CDC, or both.

### Replication subnet group
Subnet selection for DMS network placement.

### Table mappings
Rules that select or transform source objects.

### Task settings
Operational settings for buffering, validation, control tables, logging, error handling, and more.

### Validation report
Evidence that migrated data matches source under configured validation scope.

## 9.2 Common migration modes
- full load
- CDC
- full load + CDC

## 9.3 Example
Oracle to Aurora migration:
1. Create replication instance and subnet group
2. Configure source and target endpoints
3. Create task with full load + CDC
4. Apply table mappings and transformation rules
5. Validate data
6. Monitor lag
7. Freeze writes and cut over when lag is acceptable

## 9.4 Best practices
- validate endpoint connectivity and permissions first
- use full load + CDC for low-downtime cutovers where supported
- size replication instance according to throughput and LOB complexity
- enable validation where business criticality requires it
- review task settings, not just defaults

## 9.5 Common mistakes
- using default task settings without understanding behavior
- no rehearsal for CDC cutover timing
- underestimating LOB handling, charset, or transformation complexity
- no rollback/fallback decision point

## 9.6 Troubleshooting cues
- replication lag high → inspect source load, network throughput, target write performance, task tuning
- task errors → inspect endpoint auth, unsupported datatypes, transformation rules, target constraints
- validation mismatch → inspect charset, type mapping, null semantics, row filtering, timing windows

---

## 9.7 Schema conversion

AWS documentation now presents schema conversion capabilities within DMS migration workflows and related migration projects, including assessment reports, conversion rules, and converted schema objects. [11][12]

### Core entities

### Migration project
Container for migration planning and schema conversion work.

### Source schema
Schema discovered from source database.

### Target schema
Schema generated or mapped for target engine.

### Assessment report
Compatibility and effort report for conversion.

### Conversion rule
Rule controlling how source objects map to target objects.

### Converted schema object
Generated target-side object after conversion.

### Validation/conversion output
Reports showing manual action areas and conversion quality.

## 9.8 Technical role
Schema conversion handles:
- engine compatibility analysis
- SQL/object conversion
- manual remediation planning
- schema mapping before data migration

## 9.9 Example
SQL Server to PostgreSQL:
- assessment report identifies unsupported stored procedure patterns
- conversion rules adjust naming and type mapping
- converted schema is reviewed and manually corrected
- DMS handles data movement after schema is prepared

## 9.10 Best practices
- run assessment report early
- separate schema conversion from data cutover decisions
- track manual remediation items explicitly
- validate application SQL compatibility, not just schema objects

## 9.11 Common mistakes
- assuming automated conversion means zero manual work
- no application test against converted schema
- underestimating stored procedure/function rewrite effort

---

# 10. AWS DataSync

AWS DataSync is a high-speed managed file/object data transfer service for moving data to, from, and between AWS storage services and other locations. AWS documents that it supports secure data movement across on-premises, AWS storage, and other clouds. [13][14]

## 10.1 Technical purpose
Move file and object data efficiently for seeding, incremental sync, migration, and cutover.

## 10.2 Core entities

### Agent
Virtual or physical software component used when source or destination requires it.

### Location
Configured endpoint such as NFS, SMB, S3, EFS, FSx, or other supported stores.

### Task
Definition of source, destination, options, filters, and transfer behavior.

### Task execution
Actual run of a task.

### Manifest
Optional object/list-driven selection model for targeted transfer sets.

### Filter
Include/exclude rules for path selection.

### Report
Transfer execution reporting output.

### Transfer options
Behavior settings such as verification mode, overwrite mode, throttling, and metadata handling.

## 10.3 How DataSync works technically
- define source location
- define destination location
- create task with options and filters
- execute task manually or on schedule
- validate reports and cutover results

## 10.4 Example
NAS to FSx migration:
- agent deployed on-prem
- location points to SMB share
- destination points to FSx
- initial full sync runs
- nightly incremental sync continues
- final cutover sync runs during freeze window

## 10.5 Best practices
- perform initial seed before cutover window
- use filters to reduce unnecessary data transfer
- choose verification mode intentionally
- monitor execution reports and failures
- plan bandwidth limits where production networks are sensitive

## 10.6 Common mistakes
- treating DataSync as one-shot copy with no incremental/cutover planning
- transferring junk and stale data because no filters were designed
- not accounting for cross-Region data transfer cost
- no validation after final sync

## 10.7 Troubleshooting cues
- slow transfer → inspect network path, bandwidth limits, source storage performance, small-file patterns
- skipped data → inspect manifests, filters, task options, permissions
- metadata mismatch → inspect POSIX/SMB/object semantics and transfer option settings

---

# 11. AWS Transfer Family

AWS Transfer Family provides managed transfer endpoints for SFTP, FTPS, FTP, and AS2 and stores data in S3 or EFS. AWS documents support for these protocols plus workflows, connectors, identity providers, and secure endpoints. [15]

## 11.1 Technical purpose
Modernize or host partner and file-transfer workflows on AWS without self-managing transfer servers.

## 11.2 Core entities

### Server
Managed transfer endpoint for one or more supported protocols.

### User
Transfer identity with role and home directory/path mapping.

### Workflow
Managed post-upload or transfer processing sequence.

### Connector
Managed outbound connection capability, including SFTP connector patterns.

### Identity provider
Service-managed, custom, or directory-backed identity source.

### Protocol
SFTP, FTPS, FTP, or AS2 depending on server configuration.

### Host key
Server identity key used for secure protocol validation.

### Access endpoint
Internet-facing or VPC-related endpoint model depending on design.

## 11.3 Example
A B2B integration platform uses:
- SFTP server on Transfer Family
- custom identity provider
- S3 backend
- managed workflow to decrypt, validate, and route uploaded files
- outbound connector for partner delivery

AWS documents managed workflows and connector features, including SFTP connectors and workflow-based file handling. [16][17]

## 11.4 Best practices
- choose protocol based on partner requirement, not habit
- use least-privilege IAM roles per user or group model
- log and monitor transfers
- treat workflows as production integration logic with error handling
- manage host keys and endpoint exposure carefully

## 11.5 Common mistakes
- weak identity provider design
- one oversized shared role for all users
- no workflow/error handling for downstream processing
- no distinction between inbound partner transfer and outbound connector use cases

## 11.6 Troubleshooting cues
- login failure → inspect identity provider response, SSH key/user config, role trust, path mapping
- file not delivered → inspect workflow steps, connector settings, destination auth, CloudWatch logs
- protocol issue → inspect endpoint exposure, TLS/cipher expectations, partner compatibility

---

# 12. AWS Backup

AWS Backup centralizes backup and restore policy across supported services. AWS documents it as a fully managed backup service that centralizes and automates backups across AWS and some on-premises environments. [18]

## 12.1 Technical purpose
Provide policy-driven centralized backup, copy, immutability, and restore assurance.

## 12.2 Core entities

### Backup plan
Policy object containing schedules and lifecycle rules.

### Backup rule
Specific schedule/lifecycle definition in a plan.

### Backup selection
Resource assignment to a backup plan.

### Backup vault
Storage boundary for recovery points.

### Recovery point
Specific restorable backup artifact.

### Backup Vault Lock
Immutability control for backup vaults.

### Copy job
Backup copy to another Region or account.

### Restore job
Recovery operation from a recovery point.

### Legal hold
Administrative hold preventing deletion or lifecycle transition of covered recovery points.

### Restore testing plan
Automated periodic restore viability assessment plan.

## 12.3 Vault Lock

AWS documents Vault Lock governance and compliance modes. Governance mode can be removed by sufficiently privileged users; compliance mode becomes immutable after the grace/cooling-off period expires. [19]

### Best practices
- use Vault Lock for critical immutable backup requirements
- understand retention before enabling compliance mode
- validate cost impact of long or infinite retention

### Common mistakes
- enabling permanent retention accidentally
- misunderstanding governance vs compliance mode
- assuming Vault Lock alone proves restore readiness

---

## 12.4 Legal holds

AWS documents legal holds as protecting individual recovery points from deletion and delaying lifecycle transitions while the hold exists. [20]

### Best fit
- investigations
- litigation hold
- ransomware response preservation
- executive hold scenarios

### Common mistake
Confusing vault immutability with per-recovery-point legal hold behavior.

---

## 12.5 Restore testing

AWS documents restore testing as automated periodic evaluation of restore viability and restore job duration. [21]

### Technical role
This is one of the most important Week 6 features.

### Example
Monthly restore testing plan validates:
- one RDS recovery point
- one EFS recovery point
- one DynamoDB backup  
and reports duration and status.

### Best practices
- test restores regularly, not just backup creation
- include multiple resource types
- capture evidence for auditors and DR reviews
- compare actual restore duration to RTO assumptions

### Common mistakes
- backup success treated as recovery proof
- no testing of IAM/KMS/network permissions for restore path
- restore testing not aligned to business-critical workloads

---

## 12.6 Cross-Region and cross-account backup

### Best practices
- use a separate recovery account where appropriate
- validate KMS access for copied backups
- control vault policies tightly
- align copy frequency with RPO

### Common mistakes
- copied recovery points that cannot actually be restored because permissions or keys were not validated

---

# 13. AWS Fault Injection Service (FIS)

AWS FIS is a managed fault-injection service for resilience testing. AWS documents it as a service for conducting fault injection experiments on AWS workloads. [22]

## 13.1 Technical purpose
Validate resilience assumptions by deliberately injecting faults under controlled conditions.

## 13.2 Core entities

### Experiment template
Reusable experiment definition.

### Experiment
A runtime execution of a template.

### Action
Specific fault or perturbation applied.

### Target
Resource or resource set affected by the action.

### Stop condition
CloudWatch-alarm-driven safety condition that halts the experiment if performance is unacceptable.

### Experiment role
IAM role used by FIS to execute actions.

### Report / results
Experiment observations and execution output.

AWS documents experiment templates as being composed of actions, targets, stop conditions, and experiment role. [23]

## 13.3 Example
An experiment template:
- target = one Auto Scaling group
- action = instance stop or network impairment
- stop condition = checkout latency alarm  
The experiment validates whether autoscaling, retries, and failover work as expected.

## 13.4 Best practices
- define a steady-state hypothesis first
- always use stop conditions
- start with narrow blast radius
- run in lower environments first, then controlled production exercises
- document expected behavior and rollback path before execution

AWS explicitly documents stop conditions as CloudWatch alarms that stop experiments when application performance becomes unacceptable. [24]

## 13.5 Common mistakes
- chaos experiment with no measurable objective
- broad target selection
- missing or weak stop conditions
- running experiments without observability baselines
- treating FIS as a stunt rather than controlled engineering validation

## 13.6 Troubleshooting cues
- experiment ended early → stop condition likely triggered; inspect CloudWatch alarm context
- no meaningful result → wrong target scope, weak hypothesis, or observability gaps
- excessive blast radius → target selection/tagging model too broad

---

# 14. End-to-End Week 6 Execution Patterns

Week 6 is strongest when the services are connected.

## 14.1 Multi-Region failover pattern
- data already replicated using earlier-week services
- ARC readiness checks validate secondary Region readiness
- routing controls gate failover
- safety rules prevent unsafe states
- game day proves operator procedure

## 14.2 Rehost migration pattern
- Migration Hub tracks portfolio
- MGN replicates source servers
- waves define execution groups
- test launch validates application behavior
- cutover launch executes final migration
- rollback criteria defined in advance

## 14.3 DR server recovery pattern
- DRS protects critical servers
- quarterly drills launch recovery instances
- post-launch actions restore app readiness
- failback is tested after selected drills

## 14.4 Database migration pattern
- schema conversion project runs first
- assessment report identifies incompatibilities
- DMS full load + CDC migrates data
- validation report confirms correctness
- app cutover occurs only after lag and validation thresholds are acceptable

## 14.5 File and partner migration pattern
- DataSync seeds large file stores
- final incremental sync aligns cutover
- Transfer Family handles ongoing partner exchange after platform move

## 14.6 Backup and ransomware recovery pattern
- AWS Backup plans protect critical resources
- Vault Lock enforces immutability
- legal hold preserves key recovery points when needed
- restore testing proves recoverability
- FIS tests service resilience assumptions around degraded dependencies

---

# 15. Operational Playbooks

## 15.1 Failover rehearsal playbook
1. Review readiness checks
2. Confirm control-panel authority and safety rules
3. Verify current routing state
4. Trigger planned routing-control change
5. Validate secondary Region health
6. Capture timings, evidence, and user impact
7. Revert or continue as planned
8. Update lessons learned

## 15.2 MGN wave cutover playbook
1. Confirm all servers passed test launch
2. Freeze upstream changes
3. Validate launch settings
4. Confirm dependency order
5. Launch cutover instances
6. Validate app function, DNS, integrations
7. Finalize cutover or invoke rollback decision

## 15.3 DRS drill playbook
1. Verify replication health
2. Launch recovery instances non-disruptively
3. Run app validation checklist
4. Measure recovery time
5. Test failback on selected drills
6. Record evidence and gaps

## 15.4 Database migration cutover playbook
1. Confirm schema conversion complete
2. Check DMS task health and lag
3. Run validation
4. Pause writes or enter maintenance mode
5. Wait for acceptable CDC catch-up
6. Redirect application
7. Monitor post-cutover correctness and performance

## 15.5 Backup recovery playbook
1. Confirm recovery point availability
2. Check vault and KMS access
3. Run restore job
4. Validate restored resource function
5. Measure duration against RTO
6. Record evidence in audit/runbook system

---

# 16. Service Selection Guide

## 16.1 ARC vs Route 53 failover alone
- **Route 53 failover alone**: DNS failover mechanism
- **ARC**: failover control + safety rules + readiness modeling layered on top

## 16.2 DRS vs MGN
- **DRS**: disaster recovery replication and recovery
- **MGN**: migration/rehost execution with test and cutover workflows

## 16.3 DMS vs schema conversion
- **Schema conversion**: assess and convert structure/code
- **DMS**: move data and CDC

## 16.4 DataSync vs Transfer Family
- **DataSync**: bulk or incremental managed storage-to-storage data movement
- **Transfer Family**: managed partner/user file transfer endpoints and workflows

## 16.5 Backup vs replication vs legal hold
- **Backup**: historical recoverable copy
- **Replication**: near-current copy for availability or migration
- **Legal hold**: prevent deletion/lifecycle change for protected backups

## 16.6 FIS vs DR drill
- **FIS**: controlled fault injection to validate resilience assumptions
- **DR drill**: explicit recovery/failover execution rehearsal  
Use both.

---

# 17. Troubleshooting Framework for Week 6

## 17.1 Failover did not work
- readiness model incomplete?
- routing control state changed but DNS/clients not aligned?
- dependency missing in secondary Region?
- safety rule blocked action?
- data replication healthy but app dependency not ready?

## 17.2 Migration launch failed
- replication agent healthy?
- launch settings wrong?
- security group/subnet incorrect?
- app dependency missing?
- test launch not representative?

## 17.3 DMS migration underperforming
- replication instance undersized?
- source/target endpoints misconfigured?
- CDC lag from source workload?
- LOB or datatype complexity?
- transformation rules causing issues?

## 17.4 File migration inconsistent
- filters/manifests wrong?
- source permissions incomplete?
- metadata semantics mismatch?
- final incremental cutover not executed?

## 17.5 Restore failed
- vault access denied?
- KMS permissions missing?
- copied recovery point not usable in target account/region?
- restore testing never validated this path?

## 17.6 Experiment unsafe or low-value
- target selection too broad?
- stop conditions missing?
- no steady-state metric?
- no observability to interpret outcome?

---

# 18. Common Week 6 Mistakes

- calling replication “DR”
- no RTO/RPO alignment before design
- no failback plan
- no failover drills
- test launches that only verify boot, not application function
- migration waves too large
- schema conversion assumed fully automatic
- DMS validation skipped to save time
- DataSync used without incremental cutover strategy
- Transfer Family identity model left weak
- backups created but restores never tested
- Vault Lock enabled without retention/cost review
- FIS experiments with no stop conditions or measurable hypothesis

---

# 19. Week 6 Revision Checklist

You should be able to explain clearly:

- how ARC routing controls, control panels, safety rules, readiness checks, recovery groups, cells, and resource sets work together
- how DRS source servers, replication agents, staging resources, recovery instances, recovery points, and failback fit into a DR workflow
- how MGN source servers, replication settings templates, launch settings, test launches, cutover launches, and waves fit into a migration workflow
- what Migration Hub home Region, applications, discovered resources, tool integrations, and migration statuses do
- how DMS replication instances, endpoints, tasks, subnet groups, table mappings, task settings, and validation support migration
- how schema conversion assessment reports, migration projects, conversion rules, and converted objects fit before DMS cutover
- how DataSync agents, locations, tasks, executions, manifests, filters, reports, and options fit into data migration
- how Transfer Family servers, users, identity providers, protocols, workflows, connectors, host keys, and access endpoints fit into managed transfer operations
- how AWS Backup plans, vaults, recovery points, vault lock, legal holds, copy jobs, restore jobs, and restore testing differ and connect
- how FIS experiment templates, targets, actions, stop conditions, roles, and reports validate resilience assumptions

---

# 20. Memory Anchors

- **ARC** = recovery readiness + failover controls
- **Routing control** = manual failover switch
- **Safety rule** = prevent unsafe failover state
- **Readiness check** = prove recovery configuration readiness
- **DRS** = disaster recovery replication and recovery
- **Recovery instance** = launched DR server
- **MGN** = rehost migration with test and cutover
- **Wave** = migration execution group
- **Migration Hub** = migration portfolio tracker
- **DMS** = database data movement and CDC
- **Schema conversion** = convert structure/code before data move
- **DataSync** = high-speed managed storage transfer
- **Transfer Family** = managed SFTP/FTPS/FTP/AS2 transfer
- **AWS Backup** = centralized backup and restore governance
- **Vault Lock** = immutable vault control
- **Legal hold** = recovery point deletion hold
- **Restore testing** = prove recoverability
- **FIS** = controlled fault injection for resilience validation

---

# 21. Official AWS Documentation References

## Recovery control
- Amazon Application Recovery Controller (ARC) Documentation  
  https://docs.aws.amazon.com/amazonarc/

- ARC readiness checks  
  https://docs.aws.amazon.com/r53recovery/latest/dg/recovery-readiness.html

- ARC routing controls  
  https://docs.aws.amazon.com/r53recovery/latest/dg/routing-control.html

## Server migration and DR
- AWS Elastic Disaster Recovery Documentation  
  https://docs.aws.amazon.com/drs/

- AWS Transform MGN Documentation  
  https://docs.aws.amazon.com/mgn/

- Manage selected waves in MGN  
  https://docs.aws.amazon.com/mgn/latest/ug/wave-actions-menu.html

- Launch settings in MGN  
  https://docs.aws.amazon.com/mgn/latest/ug/launch-settings.html

- Starting cutover in MGN  
  https://docs.aws.amazon.com/mgn/latest/ug/starting-cutover.html

## Migration portfolio
- AWS Migration Hub Documentation  
  https://docs.aws.amazon.com/migrationhub/

## Database migration
- AWS Database Migration Service Documentation  
  https://docs.aws.amazon.com/dms/

- What is AWS DMS?  
  https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html

- Schema conversion in DMS  
  https://docs.aws.amazon.com/dms/latest/userguide/CHAP_SchemaConversion.html

## Data movement and transfer
- AWS DataSync Documentation  
  https://docs.aws.amazon.com/datasync/

- How AWS DataSync works  
  https://docs.aws.amazon.com/datasync/latest/userguide/how-datasync-transfer-works.html

- AWS Transfer Family Documentation  
  https://docs.aws.amazon.com/transfer/

- Transfer Family managed workflows  
  https://docs.aws.amazon.com/transfer/latest/userguide/transfer-workflows.html

- Transfer Family connectors  
  https://docs.aws.amazon.com/transfer/latest/userguide/creating-connectors.html

## Backup and restore
- AWS Backup Documentation  
  https://docs.aws.amazon.com/aws-backup/

- AWS Backup Vault Lock  
  https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html

- AWS Backup legal holds  
  https://docs.aws.amazon.com/aws-backup/latest/devguide/legalhold.html

- AWS Backup restore testing  
  https://docs.aws.amazon.com/aws-backup/latest/devguide/restore-testing.html

## Resilience experiments
- AWS Fault Injection Service Documentation  
  https://docs.aws.amazon.com/fis/

- FIS experiment template components  
  https://docs.aws.amazon.com/fis/latest/userguide/experiment-templates.html

- FIS stop conditions  
  https://docs.aws.amazon.com/fis/latest/userguide/stop-conditions.html

---

# 22. Final Summary

Week 6 is about **proving that workloads can migrate, recover, and fail over under pressure**.

The strongest mental model is:

- **ARC** controls and validates failover readiness
- **DRS** recovers servers for disaster scenarios
- **MGN** migrates servers through test and cutover waves
- **Migration Hub** tracks the portfolio execution state
- **DMS and schema conversion** move and reshape database platforms
- **DataSync and Transfer Family** move file and partner-transfer workloads
- **AWS Backup** protects and validates restore paths
- **FIS** tests resilience assumptions before real failures do

When you can explain not only how to replicate or copy data, but how to **test, fail over, cut over, validate, and fail back**, Week 6 is understood architecturally.

---

## Reference Notes

[1] As of October 3, 2026, AWS documentation uses the name “AWS Transform MGN” in the current docs for the former AWS Application Migration Service. Source: AWS MGN docs.  
[2] AWS documents ARC as providing recovery readiness insights and helping move traffic across AWS Regions or away from Availability Zones for disaster recovery. Source: ARC docs.  
[3] AWS documents routing controls as on/off switches integrated with Route 53 routing-control health checks for failover routing. Source: ARC routing-control docs.  
[4] AWS documents recovery groups, cells, resource sets, and readiness checks as core readiness constructs in ARC. Source: ARC readiness and API action docs.  
[5] AWS documents DRS as minimizing downtime and data loss with point-in-time recovery and minimal compute. Source: DRS docs.  
[6] AWS documents the DRS getting-started flow as initialize target Region, install replication agent, wait for sync, and launch recovery workflows. Source: DRS getting-started docs.  
[7] AWS documents MGN launch settings as two sections: general launch settings and EC2 launch template. Source: MGN launch settings docs.  
[8] AWS documents wave actions such as launching test instances and marking servers ready for cutover. Source: MGN wave actions docs.  
[9] AWS documents cutover launch lifecycle progression and finalization in current MGN docs. Source: MGN cutover docs.  
[10] AWS documents Migration Hub as a single place to track migration tasks across AWS tools and partner solutions. Source: Migration Hub docs.  
[11] AWS documents DMS as supporting source discovery, schema conversion, and data migration. Source: AWS DMS docs.  
[12] AWS documents migration projects, assessment reports, conversion rules, and converted schema workflows within DMS schema conversion guidance. Source: DMS schema conversion docs.  
[13] AWS documents DataSync as a secure, reliable, high-speed data transfer service. Source: DataSync docs.  
[14] AWS documents DataSync as supporting transfer between on-premises storage, AWS storage services, and other clouds. Source: DataSync “how it works” docs.  
[15] AWS documents Transfer Family as supporting SFTP, FTPS, FTP, and AS2 with storage in S3 or EFS. Source: Transfer Family docs.  
[16] AWS documents managed workflows for file processing within Transfer Family. Source: Transfer Family workflow docs.  
[17] AWS documents SFTP connectors and service-managed or VPC egress models for Transfer Family connectors. Source: Transfer Family connector docs.  
[18] AWS documents AWS Backup as a centralized and automated backup service across AWS services and some on-premises data. Source: AWS Backup docs.  
[19] AWS documents Vault Lock governance and compliance modes and their immutability behavior. Source: AWS Backup Vault Lock docs.  
[20] AWS documents legal holds as preventing backup deletion and delaying lifecycle transitions while the hold remains. Source: AWS Backup legal hold docs.  
[21] AWS documents restore testing as automated periodic restore viability and duration evaluation. Source: AWS Backup restore testing docs.  
[22] AWS documents FIS as a managed fault-injection experiment service. Source: FIS docs.  
[23] AWS documents experiment templates as composed of actions, targets, stop conditions, and experiment role. Source: FIS experiment template docs.  
[24] AWS documents stop conditions as CloudWatch alarm-based experiment stop triggers. Source: FIS stop condition docs.
