# Azure Study Notes — Week 4
## Storage, Databases, Messaging, Analytics, Data Governance, Search, and Recovery

> This is the **deep Week 4 Azure guide**, written much closer to a **9.5+ bar** than a normal summary.  
> It is structured as **expert-level technical study material** with:
> - technical purpose
> - core entities
> - control-plane and data-plane mechanics
> - architecture fit
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** tight structure, high signal, minimal filler.

---

# 1. Why Week 4 Matters

Week 4 is about **where state lives, how it moves, how it is queried, and how it is protected**.

If:
- Week 1 = governance
- Week 2 = networking
- Week 3 = runtime and delivery

then Week 4 = **data and persistence architecture**.

This is where platform design stops being about “where code runs” and becomes:
- how data is stored
- how it scales
- how it is protected from accidental deletion
- how it is replicated or restored
- how events move between systems
- how analytics estates are fed
- how governance and lineage are preserved
- how secrets and encryption roots are controlled

The core idea is:

> **Week 4 is about choosing the correct state model: object, file, block, relational, NoSQL, event stream, message queue, analytical lakehouse, search index, or backup artifact.**

Weak Week 4 architecture usually has:
- storage accounts used without lifecycle design
- private endpoints deployed without data-access model clarity
- databases chosen by familiarity rather than access pattern
- Cosmos DB partition keys chosen too late
- event backbones mixed up with queue semantics
- ADF pipelines built without integration-runtime strategy
- governance scanned after the fact rather than designed in
- Key Vault treated as a generic secret bag with weak recovery posture
- backups “enabled” but restore design untested

Strong Week 4 architecture can answer:
- what kind of state is this?
- who reads and writes it?
- how does it scale?
- what is the consistency model?
- what is the restore model?
- how is data classified and discovered?
- what is the event-delivery guarantee?
- how does search or analytics consume it?

---

# 2. Learning Outcomes

By the end of Week 4, you should be able to:

- explain **Azure Storage** object, file, and hierarchical-namespace patterns, including lifecycle management, versioning, object replication, immutable storage, and Data Lake Storage Gen2 ACLs
- explain **managed disks**, **disk encryption options**, **Azure Files** identity-based authentication, and **Azure NetApp Files** backup and performance-oriented file storage fit
- explain **Azure SQL Database**, **Azure SQL Managed Instance**, **Azure Database for PostgreSQL flexible server**, and **Azure Database for MySQL flexible server**, including PITR, backups, failover groups, read replicas, and operational boundaries
- explain **Azure Cosmos DB** partitioning, consistency levels, change feed, autoscale posture, and workload-shaping implications
- explain **Azure Cache for Redis** persistence, clustering, and private networking fit
- explain **Event Hubs**, **Service Bus**, and **Event Grid**, including consumer groups, partitions, capture, sessions, duplicate detection, dead-lettering, filtering, and retry semantics
- explain **Azure Data Factory**, **Synapse**, **Databricks**, **Microsoft Fabric/OneLake**, **Purview**, and **Azure AI Search** as parts of a broader data-platform architecture
- explain **Key Vault** keys, secrets, certificates, RBAC, soft delete, purge protection, and Private Link
- explain **Azure Backup** Recovery Services vaults and Backup vaults, and the difference between backup enabled and recovery operationally proven
- troubleshoot storage access failures, replica lag, event backlog, DLQ growth, ADF runtime/connectivity issues, lineage gaps, Key Vault recovery issues, and restore failures

---

# 3. Week 4 Mental Model

Think in nine state domains:

1. **Object and lake state**  
   Blob Storage, lifecycle, versioning, immutability, ADLS Gen2

2. **Infrastructure-attached state**  
   managed disks, snapshots, file shares, ANF volumes

3. **Operational relational state**  
   Azure SQL, Managed Instance, PostgreSQL, MySQL

4. **Globally distributed document state**  
   Cosmos DB

5. **Hot transient acceleration state**  
   Redis

6. **Stream, queue, and event state**  
   Event Hubs, Service Bus, Event Grid

7. **Transformation and analytics state**  
   ADF, Synapse, Databricks, Fabric/OneLake

8. **Metadata and discovery state**  
   Purview, AI Search

9. **Recovery and secret state**  
   Key Vault, Azure Backup

The Week 4 principle is:

> **The primary architecture question is not “Where can I store this?” but “What state model matches the access, scale, governance, and recovery behavior this workload actually requires?”**

---

# 4. Storage Selection Before Service Selection

## 4.1 Blob Storage
Use for:
- object storage
- unstructured content
- data lakes
- archival and lifecycle-driven storage
- event/data landing zones

## 4.2 Data Lake Storage Gen2
Use for:
- analytics-oriented hierarchical namespace
- ACL-based data-lake access control
- big-data and lakehouse patterns

## 4.3 Managed disks
Use for:
- VM/VMSS attached block storage
- OS/data disk separation
- snapshot-based infrastructure protection patterns

## 4.4 Azure Files
Use for:
- SMB/NFS share semantics
- shared file access
- lift-and-shift shared-storage patterns
- identity-based SMB integration

## 4.5 Azure NetApp Files
Use for:
- high-performance enterprise file workloads
- latency-sensitive or throughput-sensitive file services
- advanced file-protocol needs

## 4.6 Azure SQL / MI / PostgreSQL / MySQL
Use when:
- relational model and transactional semantics matter
- backup/restore and HA behavior must be well-defined

## 4.7 Cosmos DB
Use when:
- distribution, low-latency reads/writes, partitioned scale, or flexible document data model matter

## 4.8 Event Hubs / Service Bus / Event Grid
Choose by semantics:
- **Event Hubs** = high-throughput stream ingest
- **Service Bus** = brokered messaging and workflow semantics
- **Event Grid** = event routing and push-based reactive integration

## 4.9 Data platform stack
- **ADF** = orchestration and data movement
- **Synapse** = integrated analytics workspace patterns
- **Databricks** = Spark-centric analytics/engineering
- **Fabric/OneLake** = unified analytics estate and lakehouse patterns
- **Purview** = governance, scans, lineage, glossary
- **AI Search** = retrieval and indexing over enterprise data

---

# 5. Azure Storage and ADLS Gen2

Microsoft documents blob lifecycle management, versioning, object replication, immutable storage, and Data Lake Storage Gen2 access-control behavior. [1][2][3][4][5][6]

## 5.1 Azure Blob Storage

### Technical purpose
Blob Storage is the object-storage foundation for:
- unstructured application data
- backups and archives
- landing zones
- logs
- lake-style ingestion
- replicated object patterns

### Core entities
- storage account
- container
- blob
- access tier
- version
- snapshot
- lifecycle rule
- replication policy
- immutability policy

---

## 5.2 Lifecycle management

Microsoft documents lifecycle management policies for moving blobs across hot, cool, cold, and archive tiers and for deletion behavior. [1][2]

### Best practices
- define lifecycle early, not after cost surprises
- align tier transitions to actual access patterns
- use version cleanup policies if versioning is enabled
- document archive retrieval expectations before archiving aggressively

### Common mistakes
- turning on lifecycle without understanding restore/retrieval latency implications
- enabling versioning but not cleaning older versions
- cold/archive tiers used on data still needed for low-latency reads

### Expert nuance
Lifecycle policy is not merely cost automation.  
It is also:
- access-latency policy
- retention policy
- recovery-behavior policy

---

## 5.3 Blob versioning and immutability

Microsoft documents blob versioning and immutable storage with time-based retention and legal hold options. [3][4]

### Technical purpose
- **versioning** protects against overwrite/delete scenarios with recoverable historical versions
- **immutability** prevents modification/deletion for compliance or defensive data protection

### Best practices
- combine versioning with lifecycle cleanup
- use immutability where regulatory or ransomware-resilience posture requires it
- understand legal hold vs time-based retention operationally
- test restore and retrieval flow, not only policy creation

### Common mistakes
- versioning turned on without cost model
- immutable policy applied without deletion/retention governance
- operators assume immutable equals instantly easy operational recovery

---

## 5.4 Object replication

Microsoft documents object replication as asynchronous replication between source and destination accounts for block blobs. [5][6]

### Technical purpose
Use object replication for:
- distributed-read optimization
- asynchronous cross-account copy
- regional data-distribution patterns

### Best practices
- understand asynchronous behavior
- document source/destination ownership and replication rules
- monitor replication lag where business expectations are tight
- validate metadata/version implications per workload pattern

### Common mistakes
- object replication treated like synchronous DR
- destination assumed fully current during incident
- replication configured, but app still points only to source region

---

## 5.5 ADLS Gen2 and hierarchical namespace

Microsoft documents ADLS Gen2 ACLs and access-control model. [7][8]

### Technical purpose
ADLS Gen2 adds hierarchical namespace and POSIX-like ACL semantics for analytics and data-lake workloads.

### Core entities
- filesystem/container
- directory
- file
- ACL entry
- owner/group/other model
- inherited ACL behavior

### Best practices
- design namespace and directory ownership model early
- align RBAC and ACL understanding; they are not interchangeable
- minimize use of Shared Key where strong data governance matters
- document recursive ACL operations carefully

### Common mistakes
- teams rely only on Azure RBAC and ignore filesystem ACL behavior
- Shared Key use bypasses intended fine-grained control because it effectively acts as super-user access in many storage scenarios [8]
- recursive ACL changes applied without blast-radius review

### Expert nuance
ADLS security usually fails not because ACLs are too weak, but because:
- teams mix RBAC, ACLs, SAS, and Shared Key without a coherent access model

---

## 5.6 Storage / ADLS troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Data exists but access denied | auth/ACL | RBAC, ACL inheritance, Shared Key/SAS path, filesystem path ownership |
| Storage cost unexpectedly rises | lifecycle/versioning | access tiers, old versions, replication copies, archive retrieval |
| DR copy exists but not current | replication | async lag, replication rule coverage, app failover assumptions |
| Analytics job can read one path not another | namespace security | directory ACLs, recursive changes, inherited permissions |
| Archived data seems “lost” | tier semantics | archive retrieval workflow, lifecycle policy, access-latency expectations |

---

# 6. Managed Disks, Azure Files, and Azure NetApp Files

Microsoft documents managed disk encryption options, Azure Files identity-based authentication, and Azure NetApp Files backup. [9][10][11][12][13][14]

## 6.1 Managed disks

### Technical purpose
Block storage for VMs and VMSS.

### Core concerns
- OS vs data disk
- encryption model
- snapshot/restore posture
- state survivability during reimage or VMSS update

### Disk encryption options
Microsoft documents server-side encryption, customer-managed keys, encryption at host, Azure Disk Encryption, and confidential disk encryption options. [9][10]

### Best practices
- choose encryption model intentionally
- document key dependency and recovery implications
- keep OS/data responsibilities separate
- align snapshot strategy to state model, not just backup checkbox

### Common mistakes
- data stored only on ephemeral or non-protected path
- CMK used without key lifecycle/recovery planning
- teams assume VM recovery equals application-state recovery

---

## 6.2 Azure Files

Microsoft documents identity-based SMB authentication for Azure Files with AD DS, Microsoft Entra Domain Services, and Microsoft Entra Kerberos. [11]

### Technical purpose
Managed file shares with SMB/NFS semantics.

### Best practices
- align authentication mode to identity estate
- validate DNS and domain reachability for SMB identity-based auth
- use private networking where exposure must be restricted
- document performance and access-pattern expectations

### Common mistakes
- treating Azure Files as object storage
- auth model selected without client join/domain assumptions
- lift-and-shift file-server semantics assumed without permission testing

---

## 6.3 Azure NetApp Files (ANF)

Microsoft documents ANF backup as fully managed long-term recovery separate from near-term snapshots/clones. [12][13]

### Technical purpose
Enterprise-grade, high-performance file storage for demanding workloads.

### Core entities
- NetApp account
- capacity pool
- volume
- protocol/auth model
- snapshot/backup layering

### Best practices
- separate near-term operational recovery from long-term backup strategy
- design capacity pools and performance expectations intentionally
- align protocol/auth choice to enterprise identity model
- validate backup and restore expectations against workload RPO/RTO

### Common mistakes
- ANF chosen without real throughput/latency need
- snapshots mistaken for full long-term backup strategy
- authentication dependencies not tested in hybrid identity cases

---

## 6.4 Attached and shared storage troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| VM restored but app data missing | state model | OS vs data disk, snapshot coverage, external storage assumptions |
| Azure Files share mounted but access denied | identity/auth | AD/Entra Kerberos config, RBAC/share permissions, DNS/domain path |
| ANF backup exists but restore plan unclear | recovery design | backup vs snapshot distinction, target restore workflow, workload dependency map |
| Disk encryption breaks operations | key dependency | disk encryption set/CMK config, vault key access, recovery permissions |

---

# 7. Azure SQL Database, SQL Managed Instance, PostgreSQL, and MySQL

Microsoft documents Azure SQL Database restore from backups, SQL failover groups, SQL Managed Instance backup and failover groups, PostgreSQL flexible server backup/restore and read replicas, and MySQL flexible server backup/restore and read replicas. [15][16][17][18][19][20][21][22][23][24]

## 7.1 Azure SQL Database

### Technical purpose
Managed relational database for modern Azure-native apps.

### Core strengths
- automated backups
- point-in-time restore
- failover-group support
- strong PaaS abstraction

### Best practices
- define restore expectations by business tier
- monitor workload shape and connection behavior
- use failover groups for multi-region continuity where justified
- document application retry and failover behavior

### Common mistakes
- PITR exists, but application reconnection path is untested
- failover group created, but DNS/app behavior during failover not rehearsed
- restore success assumed to equal application recovery

---

## 7.2 Failover groups

Microsoft documents failover groups for Azure SQL Database and SQL Managed Instance. [16][18]

### Technical purpose
Coordinate geo-replication and planned/unplanned failover behavior across grouped databases or instances.

### Best practices
- define failover conditions and operational ownership
- test planned failover and client redirection behavior
- align failover-group design to business-service grouping
- separate DR architecture from backup architecture

### Common mistakes
- failover group assumed to replace restore planning
- databases grouped operationally with no shared business-failover logic
- write/read patterns during failover not validated

---

## 7.3 SQL Managed Instance

### Technical purpose
Managed SQL Server–compatible environment for workloads needing broader SQL Server surface area than Azure SQL Database typically provides.

### Best practices
- use when instance-level compatibility matters
- plan backup retention and restore behavior deliberately
- test failover-group behavior if MI-based DR is required
- document network/access assumptions because MI operational shape differs from single database PaaS

### Common mistakes
- MI chosen “just in case” without real compatibility need
- restore and LTR posture never reviewed
- network isolation assumptions not operationally tested

---

## 7.4 PostgreSQL flexible server

Microsoft documents PITR and read replicas using asynchronous native physical replication. [19][20][21]

### Technical purpose
Managed PostgreSQL for relational workloads needing PostgreSQL ecosystem compatibility.

### Best practices
- distinguish HA, backup restore, and read-replica use cases
- monitor replica lag before using replica for latency-sensitive reads
- align private networking and application connection strings early
- test PITR timeline assumptions under real data volume

### Common mistakes
- read replica treated as DR with no lag consideration
- PITR timing expectations unrealistic for larger datasets
- primary/replica usage pattern undocumented in app layer

---

## 7.5 MySQL flexible server

Microsoft documents backup/restore and read-replica behavior for MySQL flexible server. [22][23][24]

### Best practices
- define why replicas exist: scale reads, reporting, or transition strategy
- monitor restore timing and backup retention
- validate failover and reconnect behavior where architecture depends on it
- document storage/networking constraints of chosen topology

### Common mistakes
- replica created but no read-routing strategy exists
- restore model assumed but never timed
- app hard-codes primary assumptions in ways that hurt maintenance or failover

---

## 7.6 Relational database troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Restore succeeded but app still broken | app integration | connection strings, credentials, DNS, app migration state |
| Replica available but reads stale | replication | lag, app read-routing assumptions, workload shape |
| Failover group exists but continuity fails | client behavior | connection retry, DNS resolution, failover rehearse gaps |
| PITR taking longer than expected | restore mechanics | data volume, target creation, retention model, regional factors |
| “HA” and “DR” discussions are confused | architecture model | local HA, geo failover, backup restore, read replicas treated separately |

---

# 8. Azure Cosmos DB

Microsoft documents Cosmos DB partitioning, consistency levels, change feed, and consistency management. [25][26][27][28]

## 8.1 Technical purpose

Use Cosmos DB for:
- globally distributed applications
- flexible document or multimodel workloads
- low-latency access patterns
- partitioned scale
- event-driven downstream processing via change feed

## 8.2 Core entities

### Account
Top-level service boundary.

### Database
Logical container grouping.

### Container
Primary data and throughput design unit.

### Logical partition
Data grouped by partition key value.

### Physical partition
Underlying scale unit managed by service.

### Consistency level
Read/write consistency tradeoff model.

### Change feed
Ordered persistent record of container changes.

---

## 8.3 Partitioning

Microsoft documents logical and physical partitioning and partition-key design importance. [25]

### Best practices
- choose partition key before traffic grows
- optimize for write distribution and dominant query patterns
- watch for hot partitions
- treat partition-key mistakes as architectural, not cosmetic

### Common mistakes
- low-cardinality partition key
- partition key chosen only for data organization, not workload behavior
- hot key discovered only after production scale

---

## 8.4 Consistency levels

Microsoft documents five consistency levels and their tradeoffs. [26][27]

### Best practices
- choose default consistency by workload need, not intuition
- understand session consistency behavior and token implications
- reevaluate app code when changing account-level consistency
- balance latency, availability, and read expectations consciously

### Common mistakes
- strongest consistency expected with eventual-style design assumptions
- consistency changed without app retesting
- session behavior misunderstood across multi-instance clients

---

## 8.5 Change feed

Microsoft documents change feed as persistent ordered record of changes to a container. [28]

### Technical purpose
Drive downstream processors, projections, and event-like integrations from data changes.

### Best practices
- design idempotent downstream processing
- keep lease/checkpoint ownership clear
- use change feed as integration pattern, not as substitute for transactional workflow guarantees
- monitor backlog and processor health

### Common mistakes
- downstream consumer treats change feed like one-time queue with no replay thinking
- duplicate-safe processing ignored
- partitioning and change feed scaling relationship overlooked

---

## 8.6 Cosmos DB troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Performance inconsistent by key | partition design | hot partition, skewed workload, logical partition distribution |
| Data reads seem stale | consistency/session | account consistency, session token handling, read-region expectations |
| Change feed processor falls behind | consumer scaling | lease/checkpoint handling, partition count, downstream bottleneck |
| Global app design unstable | architecture choice | partitioning, consistency, client-region strategy, retry behavior |

---

# 9. Azure Cache for Redis

Microsoft documents Redis persistence and Private Link support. [29][30][31]

## 9.1 Technical purpose

Use Redis for:
- low-latency cache
- transient state acceleration
- session/state optimization
- some message/broker-like patterns, but only where semantics truly fit

## 9.2 Core concerns
- cache invalidation strategy
- persistence expectations
- clustering/sharding needs
- private access
- application fallback behavior

## 9.3 Persistence and clustering

Microsoft documents persistence for Premium and Enterprise tiers and clustering support in Premium. [29][31]

### Best practices
- decide whether Redis is disposable cache, durable-ish acceleration layer, or both
- understand persistence tradeoffs
- align clustering to throughput and data-size behavior
- use private networking where exposure should be limited

### Common mistakes
- app treats cache like source of truth
- persistence enabled but restore semantics not understood
- cache outage reveals no fallback path

## 9.4 Redis troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| App fails hard when cache unavailable | app design | fallback logic, cache-aside pattern, timeout behavior |
| Cache hit rate poor | data model | key strategy, TTL discipline, invalidation logic |
| Private Redis unreachable | network path | Private Link, DNS, client VNet integration, NSGs |
| Memory pressure or performance issues | workload fit | clustering, key size, eviction behavior, hot keys |

---

# 10. Event Hubs, Service Bus, and Event Grid

These are often confused because all “move events,” but their semantics differ fundamentally.

Microsoft documents Event Hubs features and terminology, Capture, resilient Event Hubs design, Service Bus duplicate detection, dead-letter queues, message loss/duplicates tradeoffs, and Event Grid delivery/retry/dead-letter behavior. [32][33][34][35][36][37][38][39]

## 10.1 Event Hubs

### Technical purpose
High-throughput event ingestion and streaming.

### Core entities
- namespace
- event hub
- partition
- consumer group
- offset/checkpoint
- throughput/capacity model
- Capture destination

### Best practices
- size partitions with producer/consumer scaling in mind
- use consumer groups per distinct consuming application
- checkpoint responsibly
- use Capture when long-term raw stream preservation is required

### Common mistakes
- consumer groups created casually with no ownership
- one consumer design assumption imposed on all readers
- Event Hubs used where brokered command/workflow semantics were actually needed

### Expert nuance
Event Hubs is about **append-only high-throughput streaming**, not command completion guarantees.

---

## 10.2 Service Bus

### Technical purpose
Brokered enterprise messaging with stronger workflow semantics than Event Hubs.

### Core features
- queues
- topics/subscriptions
- sessions
- duplicate detection
- dead-lettering
- scheduled delivery
- receive mode tradeoffs

Microsoft documents duplicate detection, dead-letter queues, and advanced features overview. [35][36][37]

### Best practices
- use for commands, workflows, and business-message handling
- use sessions where ordered correlated processing matters
- define DLQ triage ownership
- treat duplicate detection and idempotency as complementary, not mutually exclusive

### Common mistakes
- DLQ grows with no operational owner
- exactly-once expectations oversimplified
- Event Hubs chosen for workflow orchestration because it “handles events”

---

## 10.3 Event Grid

### Technical purpose
Reactive event routing with push delivery.

### Core entities
- topic/system topic
- event subscription
- filtering
- retry policy
- dead-letter location
- handler endpoint

Microsoft documents at-least-once delivery, retry, and dead-letter configuration. [38][39]

### Best practices
- use for event routing, not durable work-queue semantics
- design handlers idempotently
- configure dead-letter location where event loss is unacceptable
- filter narrowly to avoid noisy subscriptions

### Common mistakes
- handler endpoint assumes exactly-once delivery
- retry/dead-letter policy left default with no ownership
- Event Grid used where backlog/consumer-controlled replay semantics are needed

---

## 10.4 Messaging selection guide

### Choose Event Hubs when
- ingest rate is high
- multiple independent readers need offsets
- replay by offset matters
- telemetry/stream workloads dominate

### Choose Service Bus when
- queue/topic workflow semantics matter
- correlation, sessions, scheduling, and DLQ are first-class needs

### Choose Event Grid when
- you need reactive push routing
- publisher/subscriber event wiring is primary
- event handlers are idempotent and decoupled

---

## 10.5 Messaging troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Stream consumers lag badly | Event Hubs consumer design | partitions, checkpointing, consumer-group ownership, downstream throughput |
| DLQ grows steadily | Service Bus ops model | message contract issues, handler failures, lock/receive behavior, triage process |
| Duplicate processing surprises app | semantics mismatch | Service Bus receive mode, duplicate detection window, Event Grid retry model, idempotency |
| Events “missing” from push subscriber | Event Grid delivery | retry policy, dead-letter config, endpoint acknowledgment, filtering |
| Streaming solution hard to reason about | wrong product fit | Event Hubs vs Service Bus vs Event Grid semantic mismatch |

---

# 11. Data Factory, Synapse, Databricks, Fabric/OneLake, Purview, and AI Search

Week 4 is not only storage and databases. It is also the data-platform control layer.

Microsoft documents ADF linked services, integration runtime, triggers/pipeline execution, OneLake and shortcuts, Purview scans/ingestion and glossary, and Azure AI Search indexers and skillsets. [40][41][42][43][44][45][46][47][48]

## 11.1 Azure Data Factory (ADF)

### Technical purpose
Orchestrate data movement and transformation workflows.

### Core entities
- pipeline
- activity
- trigger
- linked service
- dataset
- integration runtime

### Integration runtime
Microsoft documents IR as the compute infrastructure for data movement and data flows across public/private networks. [41]

### Best practices
- design IR choice early for network reachability
- separate orchestration logic from business data-contract logic
- standardize linked-service governance
- treat pipeline triggers as scheduled operations with ownership, not invisible automation

### Common mistakes
- linked services created ad hoc with inconsistent auth models
- self-hosted/private connectivity needs discovered late
- pipeline triggers enabled with no operational calendar or dependency awareness

---

## 11.2 Synapse and Databricks

### Synapse
Best fit for integrated analytics workspace patterns, combining pipeline/orchestration plus SQL/Spark-oriented analytics contexts.

### Databricks
Best fit for Spark-first engineering, advanced notebooks/jobs, and data engineering/science collaboration patterns.

### Best practices
- choose based on operating model, not branding
- document whether transformation logic lives in ADF pipelines, Synapse, Databricks jobs, or Fabric workflows
- avoid duplicating data movement orchestration across tools without reason

### Common mistakes
- multiple analytics services adopted with no control-plane boundary
- same pipeline logic partially duplicated across ADF and Databricks jobs
- tool choice driven by team preference alone

---

## 11.3 Microsoft Fabric and OneLake

Microsoft documents OneLake and shortcuts as a unified analytics storage plane and virtualized access pattern. [43][44][45]

### Technical purpose
Provide unified analytical storage and shortcuts/mirroring-based data access patterns.

### Best practices
- use shortcuts when virtual unification is preferable to copying
- document authoritative data ownership when multiple domains surface data in OneLake
- distinguish virtualization from replication

### Common mistakes
- shortcuts used but teams assume independent copies exist
- governance unclear when data appears in multiple analytical contexts
- OneLake treated as exempt from ordinary data ownership rules

---

## 11.4 Purview

Microsoft documents scans/ingestion and glossary. [46][47]

### Technical purpose
Metadata, scanning, classification, glossary, lineage, and governance visibility.

### Best practices
- register and scan high-value sources early
- tie glossary and classification effort to business use, not just tool adoption
- treat lineage as operational evidence, not decorative metadata
- align Purview ownership with platform governance and data-domain ownership

### Common mistakes
- scanning starts after platform sprawl
- glossary becomes disconnected from real engineering assets
- classifications exist but do not influence access or lifecycle decisions

---

## 11.5 Azure AI Search

Microsoft documents AI Search, indexers, and skillsets. [48][49][50]

### Technical purpose
Search and retrieval over enterprise content, including enrichment/indexer pipelines.

### Core entities
- data source
- index
- indexer
- skillset
- enrichment pipeline
- query layer

### Best practices
- define indexing cadence and source-of-truth boundaries
- use indexers where source sync pattern fits
- use skillsets when enrichment truly adds retrieval value
- distinguish operational search from analytical querying

### Common mistakes
- search index treated like primary source of truth
- enrichment pipeline cost and latency not considered
- indexer failures unnoticed until search freshness degrades

---

## 11.6 Data platform troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| ADF pipeline works in public path not private path | integration runtime | IR type, network reachability, linked service auth, private endpoint DNS |
| Data platform tooling overlaps confusingly | architecture boundary | ADF vs Synapse vs Databricks vs Fabric role clarity |
| Lineage or classification incomplete | governance | Purview scans, connector coverage, glossary ownership, ingestion timing |
| Search results stale or poor quality | indexing/enrichment | indexer schedule, skillset errors, source freshness, index design |
| OneLake view differs from expectation | virtualization model | shortcut source ownership, refresh assumptions, copied vs referenced data |

---

# 12. Key Vault and Azure Backup

Microsoft documents Key Vault objects, RBAC, soft delete, purge protection, Private Link, Recovery Services vaults, and Backup vaults. [51][52][53][54][55][56][57][58]

## 12.1 Azure Key Vault

### Technical purpose
Store and protect:
- keys
- secrets
- certificates

### Core entities
- vault
- key
- secret
- certificate
- RBAC/access model
- soft-delete state
- purge-protection state
- private endpoint

### Best practices
- prefer RBAC-based access model with clear least-privilege assignments
- enable and understand soft delete and purge protection
- separate application secret governance from platform key governance where appropriate
- use Private Link for stronger network isolation where required

### Common mistakes
- one vault used for unrelated environments or ownership domains
- no tested recovery path for deleted objects
- app outages occur because secret rotation lifecycle is poorly coordinated
- private endpoint exists but runtime DNS path still points public

---

## 12.2 Recovery features

Microsoft documents soft delete and purge protection as core recovery safeguards. [53][54]

### Best practices
- treat purge protection as anti-destructive control, not optional decoration
- document who can recover deleted vaults/objects
- rehearse recovery for critical secrets or certificates

### Common mistakes
- teams assume vault delete is always easily reversible
- purge behavior misunderstood in incident or teardown scripts

---

## 12.3 Azure Backup

Microsoft documents Recovery Services vaults and Backup vaults as distinct vault types for different workloads. [55][56][57][58]

### Technical purpose
Centralize backup and recovery points for supported workloads.

### Recovery Services vault
Common for traditional Azure workload backup patterns such as VMs and SQL in Azure VMs.

### Backup vault
Supports newer Azure Backup workload patterns such as Blob and newer services.

### Best practices
- choose correct vault type per workload
- map backup policy to business-tier recovery needs
- monitor backup job success and restore readiness
- test restore workflows regularly

### Common mistakes
- “backup enabled” mistaken for “recoverable in business context”
- wrong vault type expectations
- restore permissions/network/dependency chain not tested
- backup team and application team have different definitions of successful recovery

---

## 12.4 Secrets and backup troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| App secret lookup fails after network hardening | network/DNS | Private Link, DNS, app runtime outbound path, managed identity/RBAC |
| Deleted secret or vault not recoverable as expected | recovery feature | soft delete state, purge protection, retention, permissions |
| Backup jobs green but recovery still blocked | recovery workflow | restore target permissions, dependent services, app bootstrap after restore |
| Wrong vault used for workload assumptions | service fit | Recovery Services vault vs Backup vault capability and workload support |

---

# 13. End-to-End Data Architecture Example

An enterprise platform has:
- application media and documents
- analytics raw ingestion
- VM-based app data disks
- shared team files
- transactional relational workloads
- globally distributed product catalog
- cache for session/state acceleration
- event ingestion from apps and devices
- workflow queueing for downstream processing
- analytics pipelines and governance
- secrets and backup requirements

A strong Week 4 design might look like this:

### Object and lake layer
- Blob Storage with lifecycle, versioning, and immutability where needed
- ADLS Gen2 for curated/raw analytical zones with ACL-based access

### Infrastructure storage
- managed disks for VM-attached state
- Azure Files for shared SMB use cases
- ANF only for workloads with real enterprise file-performance requirements

### Relational
- Azure SQL Database for modern app DBs
- MI only where SQL Server compatibility needs justify it
- PostgreSQL/MySQL flexible server for engine-aligned apps with PITR and replica strategy documented

### NoSQL
- Cosmos DB with partition key chosen from workload shape, not just entity semantics

### Acceleration
- Redis for cache-aside pattern, not source of truth

### Messaging
- Event Hubs for streaming telemetry
- Service Bus for workflow and command messaging
- Event Grid for reactive event fan-out

### Analytics and governance
- ADF for movement/orchestration
- Databricks/Synapse/Fabric chosen with explicit role boundaries
- Purview for scans, classification, glossary, lineage
- AI Search for retrieval/indexing use cases, not transactional truth

### Secrets and recovery
- Key Vault with RBAC, soft delete, purge protection, and private endpoints
- Azure Backup vault selection by workload
- restore runbooks validated with application teams

This is the Week 4 principle:

> state model, data movement, governance, and recovery must be designed as one system.

---

# 14. Troubleshooting Flow

When Week 4 systems fail, inspect in this order:

1. **State truth**  
   What kind of state is this: object, file, block, relational, NoSQL, event, search index, or backup artifact?

2. **Authority truth**  
   Which system is source of truth, and which is derived or replicated?

3. **Scale truth**  
   Is the partitioning, throughput, or consumer model aligned to actual usage?

4. **Security truth**  
   Is the failure auth, ACL, RBAC, private endpoint, DNS, or key dependency?

5. **Recovery truth**  
   Is there a real restore/replay/rebuild path, or only a checkbox feature?

6. **Governance truth**  
   Are metadata, lineage, classification, and ownership documented enough to reason about impact?

7. **Operational truth**  
   Which logs, lag indicators, dead-letter paths, job histories, or backup/restore job records confirm what really happened?

---

# 15. Common Week 4 Mistakes

- storage account used without lifecycle and retention strategy
- Blob versioning enabled with no cleanup logic
- ADLS ACLs and RBAC mixed without access model clarity
- disks treated as backup strategy rather than state component
- Azure Files auth path not tested end to end
- ANF chosen without real performance need
- HA, backup, replicas, and DR confused in relational database design
- Cosmos partition key chosen too late
- cache treated as durable source of truth
- Event Hubs, Service Bus, and Event Grid selected by product familiarity instead of delivery semantics
- ADF linked services and runtimes created ad hoc
- data tooling overlaps with no control-plane boundary
- Purview added after sprawl, so governance remains shallow
- Key Vault delete/recovery behavior not rehearsed
- backup enabled but restore never validated with the application

---

# 16. Revision Checklist

You should be able to explain clearly:

- how Blob Storage lifecycle, versioning, immutability, and object replication work together
- how ADLS Gen2 ACLs differ from ordinary Azure RBAC thinking
- when to use managed disks, Azure Files, and ANF
- how Azure SQL Database, SQL MI, PostgreSQL, and MySQL differ operationally
- why read replicas, failover groups, backups, and PITR are not the same thing
- how Cosmos DB partitioning and consistency shape workload behavior
- when Redis is safe as acceleration and unsafe as truth
- how Event Hubs, Service Bus, and Event Grid differ semantically
- how ADF, Synapse, Databricks, Fabric, Purview, and AI Search fit into a broader data platform
- how Key Vault recovery features and Azure Backup vault types affect recoverability

---

# 17. Memory Anchors

- **Blob Storage** = object storage
- **lifecycle management** = automated tier/delete control
- **versioning** = historical object recovery
- **immutability** = retention-protected blob state
- **object replication** = async blob copy across accounts
- **ADLS Gen2** = hierarchical namespace + ACL-aware data lake
- **managed disk** = VM-attached block storage
- **Azure Files** = managed SMB/NFS share
- **ANF** = enterprise high-performance file service
- **Azure SQL DB** = managed modern relational DB
- **SQL MI** = higher SQL Server compatibility managed instance
- **PostgreSQL/MySQL flexible server** = managed open-source relational engines
- **PITR** = restore to earlier point in time
- **failover group** = coordinated geo failover path
- **Cosmos DB** = globally distributed partitioned NoSQL
- **partition key** = scale and access-shaping key
- **consistency level** = latency/consistency tradeoff
- **change feed** = ordered change stream from container
- **Redis** = low-latency cache/acceleration layer
- **Event Hubs** = streaming ingest backbone
- **Service Bus** = brokered enterprise messaging
- **Event Grid** = reactive push event routing
- **ADF** = data movement/orchestration
- **IR** = ADF/Synapse runtime for movement and transformation
- **OneLake** = unified analytical storage plane
- **Purview** = metadata, lineage, classification
- **AI Search** = search index and enrichment platform
- **Key Vault** = keys, secrets, certificates
- **soft delete + purge protection** = recovery safety controls
- **Recovery Services vault / Backup vault** = Azure Backup control boundaries

---

# 18. Official References

## Azure Storage and ADLS Gen2
- Azure Blob Storage lifecycle management overview  
  https://learn.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview

- Configure a lifecycle management policy  
  https://learn.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-policy-configure

- Blob versioning overview  
  https://learn.microsoft.com/en-us/azure/storage/blobs/versioning-overview

- Immutable storage overview  
  https://learn.microsoft.com/en-us/azure/storage/blobs/immutable-storage-overview

- Object replication overview  
  https://learn.microsoft.com/en-us/azure/storage/blobs/object-replication-overview

- Configure object replication  
  https://learn.microsoft.com/en-us/azure/storage/blobs/object-replication-configure

- ADLS Gen2 ACLs  
  https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control

- ADLS Gen2 access control model  
  https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control-model

## Attached and shared storage
- Managed disk encryption overview  
  https://learn.microsoft.com/en-us/azure/virtual-machines/disk-encryption-overview

- Server-side encryption of Azure managed disks  
  https://learn.microsoft.com/en-us/azure/virtual-machines/disk-encryption

- Azure Files identity-based authentication overview  
  https://learn.microsoft.com/en-us/azure/storage/files/storage-files-active-directory-overview

- Azure NetApp Files documentation  
  https://learn.microsoft.com/en-us/azure/azure-netapp-files/

- Azure NetApp Files backup introduction  
  https://learn.microsoft.com/en-us/azure/azure-netapp-files/backup-introduction

## Relational databases
- Restore a database from a backup in Azure SQL Database  
  https://learn.microsoft.com/en-us/azure/azure-sql/database/recovery-using-backups?view=azuresql

- Failover groups for Azure SQL Database  
  https://learn.microsoft.com/en-us/azure/azure-sql/database/failover-group-sql-db?view=azuresql

- Azure SQL Managed Instance documentation  
  https://learn.microsoft.com/en-us/azure/azure-sql/managed-instance/?view=azuresql

- Automated backups in SQL Managed Instance  
  https://learn.microsoft.com/en-us/azure/azure-sql/managed-instance/automated-backups-overview?view=azuresql

- Point-in-time restore for SQL Managed Instance  
  https://learn.microsoft.com/en-us/azure/azure-sql/managed-instance/point-in-time-restore?view=azuresql

- Long-term backup retention for SQL Managed Instance  
  https://learn.microsoft.com/en-us/azure/azure-sql/managed-instance/long-term-backup-retention-configure?view=azuresql

- Failover groups for SQL Managed Instance  
  https://learn.microsoft.com/en-us/azure/azure-sql/managed-instance/failover-group-sql-mi?view=azuresql

- PostgreSQL flexible server backup and restore  
  https://learn.microsoft.com/en-us/azure/postgresql/backup-restore/concepts-backup-restore

- PostgreSQL flexible server read replicas  
  https://learn.microsoft.com/en-us/azure/postgresql/read-replica/concepts-read-replicas

- Create PostgreSQL read replica  
  https://learn.microsoft.com/en-us/azure/postgresql/read-replica/how-to-create-read-replica

- MySQL flexible server backup and restore  
  https://learn.microsoft.com/en-us/azure/mysql/flexible-server/concepts-backup-restore

- MySQL flexible server restore from backup  
  https://learn.microsoft.com/en-us/azure/mysql/flexible-server/how-to-restore-server-portal

- MySQL flexible server read replicas  
  https://learn.microsoft.com/en-us/azure/mysql/flexible-server/concepts-read-replicas

## Cosmos DB and Redis
- Cosmos DB partitioning and horizontal scaling  
  https://learn.microsoft.com/en-us/azure/cosmos-db/partitioning

- Cosmos DB consistency levels  
  https://learn.microsoft.com/en-us/azure/cosmos-db/consistency-levels

- Manage consistency in Cosmos DB  
  https://learn.microsoft.com/en-us/azure/cosmos-db/how-to-manage-consistency

- Cosmos DB change feed  
  https://learn.microsoft.com/en-us/azure/cosmos-db/change-feed

- Azure Cache for Redis documentation  
  https://learn.microsoft.com/en-us/azure/azure-cache-for-redis/

- Redis persistence  
  https://learn.microsoft.com/en-us/azure/azure-cache-for-redis/cache-how-to-premium-persistence

- Redis with Private Link  
  https://learn.microsoft.com/en-us/azure/azure-cache-for-redis/cache-private-link

## Messaging and events
- Azure Event Hubs documentation  
  https://learn.microsoft.com/en-us/azure/event-hubs/

- Event Hubs features and terminology  
  https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-features

- Event Hubs Capture overview  
  https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-capture-overview

- Resilient Event Hubs and Azure Functions design  
  https://learn.microsoft.com/en-us/azure/architecture/serverless/event-hubs-functions/resilient-design

- Service Bus duplicate detection  
  https://learn.microsoft.com/en-us/azure/service-bus-messaging/duplicate-detection

- Enable duplicate detection  
  https://learn.microsoft.com/en-us/azure/service-bus-messaging/enable-duplicate-detection

- Service Bus dead-letter queues  
  https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dead-letter-queues

- Service Bus advanced features overview  
  https://learn.microsoft.com/en-us/azure/service-bus-messaging/advanced-features-overview

- Prevent message loss and duplicate processing in Service Bus  
  https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-message-loss-and-duplicates

- Azure Event Grid documentation  
  https://learn.microsoft.com/en-us/azure/event-grid/

- Event Grid delivery and retry  
  https://learn.microsoft.com/en-us/azure/event-grid/delivery-and-retry

- Configure dead-letter location and retry policy  
  https://learn.microsoft.com/en-us/azure/event-grid/manage-event-delivery

## Data platform and governance
- Azure Data Factory documentation  
  https://learn.microsoft.com/en-us/azure/data-factory/

- Linked services in ADF and Synapse  
  https://learn.microsoft.com/en-us/azure/data-factory/concepts-linked-services

- Integration runtime in ADF and Synapse  
  https://learn.microsoft.com/en-us/azure/data-factory/concepts-integration-runtime

- Pipeline execution and triggers  
  https://learn.microsoft.com/en-us/azure/data-factory/concepts-pipeline-execution-triggers

- OneLake documentation  
  https://learn.microsoft.com/en-us/fabric/onelake/

- OneLake shortcuts  
  https://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts

- Unify data with OneLake shortcuts and mirroring  
  https://learn.microsoft.com/en-us/fabric/onelake/unify-data

- Purview glossary  
  https://learn.microsoft.com/en-us/purview/data-governance-glossary

- Purview scans and ingestion in Data Map  
  https://learn.microsoft.com/en-us/purview/data-map-scan-ingestion

- Azure AI Search documentation  
  https://learn.microsoft.com/en-us/azure/search/

- AI Search indexer overview  
  https://learn.microsoft.com/en-us/azure/search/search-indexer-overview

- AI Search skillset concepts  
  https://learn.microsoft.com/en-us/azure/search/cognitive-search-working-with-skillsets

## Secrets and backup
- Azure Key Vault overview  
  https://learn.microsoft.com/en-us/azure/key-vault/

- Keys, secrets, and certificates overview  
  https://learn.microsoft.com/en-us/azure/key-vault/general/about-keys-secrets-certificates

- Key Vault RBAC guide  
  https://learn.microsoft.com/en-us/azure/key-vault/general/rbac-guide

- Key Vault recovery overview  
  https://learn.microsoft.com/en-us/azure/key-vault/general/key-vault-recovery

- Key Vault soft-delete overview  
  https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview

- Key Vault with Private Link  
  https://learn.microsoft.com/en-us/azure/key-vault/general/private-link-service

- Azure Backup documentation  
  https://learn.microsoft.com/en-us/azure/backup/

- Recovery Services vault overview  
  https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview

- Create and configure Recovery Services vaults  
  https://learn.microsoft.com/en-us/azure/backup/backup-create-recovery-services-vault

- Backup vault overview  
  https://learn.microsoft.com/en-us/azure/backup/backup-vault-overview

---

# 19. Final Summary

Week 4 is about **choosing the right Azure state model and operating it with discipline**.

The strongest mental model is:

- **Blob + ADLS Gen2** = object and lake state
- **disks + files + ANF** = attached and shared infrastructure state
- **Azure SQL + MI + PostgreSQL + MySQL** = relational state
- **Cosmos DB** = distributed partitioned NoSQL state
- **Redis** = low-latency acceleration state
- **Event Hubs + Service Bus + Event Grid** = stream, queue, and reactive event state
- **ADF + Synapse + Databricks + Fabric + Purview + AI Search** = data movement, analytics, governance, and retrieval
- **Key Vault + Azure Backup** = secret protection and recovery state

When you can explain the **state model, consistency model, movement model, governance model, and recovery model** together, Azure Week 4 is understood architecturally.
