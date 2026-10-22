# GCP Study Notes — Week 4
## Storage, Databases, Streaming, Analytics, Governance, and Data Security

> This is the **high-depth Week 4 guide**, written to sit much closer to the **9.5+ bar**.  
> It is structured as **expert-level study material** with:
> - technical purpose
> - core entities
> - architecture fit
> - runtime and operational mechanics
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** tight structure, high signal, minimal filler.

---

# 1. Why Week 4 Matters

Week 4 is about **state, data movement, and data control**.

If:
- Week 1 = governance
- Week 2 = networking
- Week 3 = execution and delivery

then Week 4 = **where data lives, how it is protected, how it moves, how it is queried, and how it is governed**.

This week is where platform design becomes expensive if done badly.

The core idea is:

> **Data-platform design is a sequence of tradeoffs across consistency, latency, query shape, scale pattern, retention, recoverability, governance, and operational burden.**

Week 4 is not just “learn the database services.”  
It is learning to answer:

- file, object, block, cache, document, key-value, relational, globally consistent relational, or warehouse?
- transactional, analytical, or streaming?
- low-latency row lookup, SQL query, pub/sub fan-out, or batch ETL?
- backup, PITR, retention lock, soft delete, or immutable copy?
- row-level, column-level, bucket-level, managed-folder-level, or key-level access?
- schedule it, stream it, orchestrate it, catalog it, scan it, or tokenize it?

---

# 2. Learning Outcomes

By the end of Week 4, you should be able to:

- explain **Persistent Disk**, **Hyperdisk**, **regional persistent disk**, **snapshots**, and **machine-image-adjacent block-storage patterns**
- explain **Cloud Storage** buckets, storage classes, lifecycle rules, soft delete, versioning, retention policies, Bucket Lock, managed folders, hierarchical namespace, and replication-related design choices
- explain **Filestore** tiers, NFS semantics, backups, snapshots, replication, and throughput design
- explain **Cloud SQL** and **AlloyDB** architecture, HA, backups, PITR, replicas/read pools, private IP, and operational differences
- explain **Spanner** instances, databases, splits, leader region, backups, backup schedules, and change streams
- explain **Bigtable** instances, clusters, replication, app profiles, row-key design, hot-spot risk, and backups
- explain **Firestore** databases, documents, collections, indexes, TTL, and backup/restore concepts
- explain **Memorystore for Redis** and **Memorystore for Memcached**
- explain **Pub/Sub** topics, subscriptions, ordering keys, dead-letter topics, retry policies, seek, snapshots, and filtering
- explain **Dataflow** jobs, templates, Flex Templates, Streaming Engine, worker service accounts, and pipeline failure modes
- explain **Dataproc**, **Cloud Composer**, **BigQuery**, **Dataplex / Knowledge Catalog**, **Sensitive Data Protection**, and **Cloud Data Fusion**
- explain **Secret Manager**, **Cloud KMS**, **Cloud HSM**, and **Cloud EKM**
- explain **Document AI**, **Vision AI**, **Speech-to-Text**, and **Cloud Translation** as data-ingestion/enrichment services

---

# 3. Architectural Mental Model

Think in planes:

1. **State plane**  
   block, object, file, cache, transactional DB, wide-column DB, document DB

2. **Movement plane**  
   streaming, messaging, ETL, orchestration

3. **Analytics plane**  
   warehouse, SQL security, metadata, lineage, discovery

4. **Control plane for data**  
   secret management, key management, DLP, governance catalogs, retention controls

A mature Week 4 design does not choose services by brand familiarity.  
It chooses them by:

- access pattern
- scale pattern
- consistency requirement
- recovery target
- governance requirement
- operational burden

---

# 4. State-Service Selection Grid

## 4.1 Persistent Disk / Hyperdisk
Use for VM-attached block storage.

## 4.2 Cloud Storage
Use for object storage, data lake zones, artifacts, backups, static content, and unstructured durable storage.

## 4.3 Filestore
Use for managed NFS file workloads.

## 4.4 Cloud SQL
Use for managed relational databases with familiar engines and moderate scale.

## 4.5 AlloyDB
Use for PostgreSQL-compatible high-performance managed relational workloads with stronger scale/perf posture.

## 4.6 Spanner
Use for globally scalable strongly consistent relational workloads.

## 4.7 Bigtable
Use for very large-scale low-latency key/row access patterns.

## 4.8 Firestore
Use for app-centric document data models with developer-friendly access patterns.

## 4.9 Memorystore
Use for cache/session/in-memory acceleration.

## 4.10 BigQuery
Use for analytics, warehousing, large scans, BI, and governed data sharing.

---

# 5. Persistent Disk, Hyperdisk, Regional Persistent Disk, and Snapshots

These are the core block-storage primitives for VM-centric workloads.

## 5.1 Technical purpose

Use block storage when:
- the workload expects block devices
- the OS/filesystem owns data layout
- latency and throughput are tied to attached-disk configuration
- the runtime is Compute Engine or VM-based software on GCE

## 5.2 Core entities

### Persistent Disk
Managed durable block storage.

### Hyperdisk
Higher-performance/tunable block storage family for advanced workload profiles.

### Regional Persistent Disk
Synchronous regional replication pattern for higher availability.

### Snapshot
Point-in-time disk backup artifact.

### Boot disk
Primary OS disk.

### Data disk
Attached application data disk.

## 5.3 Architecture fit

### Use Persistent Disk when
- you need general VM block storage
- the workload is moderate and stable
- the VM fleet is not hyper performance sensitive

### Use Hyperdisk when
- you need higher performance tiers or more granular tuning
- the workload is latency/throughput sensitive
- the storage profile justifies the premium and design complexity

### Use regional persistent disk when
- zonal failure tolerance for block-backed state matters
- application design can use the regional availability pattern

## 5.4 Snapshots

### Technical role
Disk backup primitive, not full application recovery by itself.

### Best practices
- coordinate snapshots with app-consistent backup patterns where required
- define retention and restore-test discipline
- separate OS recovery from application/data recovery thinking

### Common mistakes
- assuming crash-consistent snapshot always equals app-consistent backup
- no restore drill
- snapshot sprawl with no retention policy

## 5.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| VM I/O latency high | storage sizing/perf | disk type, IOPS/throughput profile, instance shape, queue depth |
| Restore works but app corrupted | app consistency | app-aware backup process, filesystem flush, DB state at snapshot time |
| Failover design still loses data | architecture | regional disk semantics, app write behavior, cross-zone design assumptions |

---

# 6. Cloud Storage

Cloud Storage is the foundational object store.

Google documents hierarchical namespace, managed folders, Object Lifecycle Management, soft delete, and Bucket Lock/retention policy behavior, including that locking a retention policy places a project lien that prevents project deletion. [1][2][3][4][5]

## 6.1 Technical purpose

Use Cloud Storage for:
- durable object storage
- data lake layers
- archive and backup targets
- static assets
- logs and exported analytics
- staging zones for pipelines

## 6.2 Core entities

### Bucket
Top-level storage container.

### Object
Stored item with metadata.

### Storage class
Cost/performance/access tradeoff class.

### Lifecycle rule
Automated object transition/deletion logic.

### Versioning
Multiple object generations retained.

### Soft delete
Recover deleted objects within retention window.

### Retention policy
Prevents deletion or modification before retention period.

### Bucket Lock
Irreversible lock on retention policy.

### Managed folder
IAM-grantable folder resource in supported bucket models.

### Hierarchical namespace (HNS)
Folder-aware namespace model for higher-performance and governance-sensitive folder operations.

## 6.3 Storage classes and lifecycle

### Technical role
Optimize cost based on access frequency and durability assumptions.

### Best practices
- classify by access pattern, not guesswork
- use lifecycle rules to move or delete stale data
- separate hot operational buckets from archive/retention buckets

### Common mistakes
- using one bucket for every purpose
- lifecycle rules added without downstream impact review
- archive-like data kept in hot classes indefinitely

---

## 6.4 Soft delete, versioning, and retention controls

Google documents soft delete with a default seven-day retention for newly created buckets unless configured otherwise, and documents restoration of soft-deleted objects. [6][7]

### Soft delete
Good for accidental deletion recovery.

### Versioning
Good for overwrite protection and data recovery scenarios.

### Retention policy / Bucket Lock
Good for immutability/compliance.

### Critical distinction
- soft delete = recover deleted objects during a retention window
- versioning = keep historical object generations
- retention policy = prevent deletion/overwrite before time expiry
- Bucket Lock = make retention irreversible

### Best practices
- choose the control that matches the risk
- use retention lock only with strong governance review
- document the cost implications of versioning and soft delete

### Common mistakes
- enabling versioning without cleanup governance
- Bucket Lock applied casually
- assuming soft delete replaces backup strategy

---

## 6.5 Managed folders and hierarchical namespace

Google documents managed folders as IAM-bearing resources and HNS as a folder-aware namespace with operational benefits. [2][8]

### Technical purpose
Improve folder-level governance and large-scale namespace management.

### Best practices
- use managed folders where finer-grained IAM is needed
- use HNS intentionally for folder-heavy or Hadoop-like data patterns
- standardize folder ownership and naming

### Common mistakes
- confusing simulated prefixes with actual folder governance constructs
- inconsistent folder conventions across teams

---

## 6.6 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Deleted data expected but not recoverable | retention control mismatch | soft delete policy, versioning, retention settings, object generation |
| Team cannot access one path in bucket | IAM/folder model | bucket IAM vs managed-folder IAM, uniform access assumptions |
| Lifecycle deleted needed data | lifecycle governance | rule conditions, prefixes, age filters, missing exception bucket |
| Project cannot be deleted after retention lock | compliance control | Bucket Lock lien behavior, retention lock state |

---

# 7. Filestore

Filestore is the managed NFS service.

Google documents Filestore service tiers, backups, enhanced backups, backup chains, and best practices for creating backups during off-peak periods. [9][10][11][12]

## 7.1 Technical purpose

Use Filestore when:
- applications require NFS semantics
- shared file access is needed
- VM/GKE workloads expect file-system rather than object-storage semantics

## 7.2 Core entities

### Filestore instance
Managed NFS server.

### Service tier
Performance/availability feature tier.

### File share
Mounted NFS export.

### Backup
Point-in-time backup artifact.

### Enhanced backup
Backup and DR-integrated model with vault/plan support.

### Replication
Regional/zone resilience feature in supported tiers.

## 7.3 Architecture fit

### Good fit
- content repos
- ML/shared file datasets
- legacy NFS-dependent apps
- shared app state that truly needs POSIX/NFS-like behavior

### Bad fit
- object-like workloads better served by Cloud Storage
- high-scale structured DB-style workloads

## 7.4 Backups and replication

### Best practices
- schedule backups off-peak
- understand tier-specific backup/replication support
- test restore and mount behavior
- treat NFS recovery as app recovery, not only file restore

### Common mistakes
- no performance review under peak concurrency
- backup schedule created but no restore validation
- assuming file-share replication replaces broader app DR design

## 7.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Mount works but performance is poor | sizing/tier | service tier, throughput needs, client concurrency, NFS pattern |
| Backup exists but restore timing unacceptable | recovery design | backup type, dataset size, restore workflow, cutover assumptions |
| App hangs on shared files | semantics/app design | NFS locking expectations, client retry behavior, concurrency pattern |

---

# 8. Cloud SQL

Cloud SQL is the managed relational service for MySQL, PostgreSQL, and SQL Server.

Google documents PITR, IAM authentication for PostgreSQL, and read replica behavior, including some edition- and connectivity-specific constraints. [13][14][15][16]

## 8.1 Technical purpose

Use Cloud SQL when you need:
- managed relational database operations
- common engine compatibility
- HA, backups, and moderate scale without self-managing engine VMs
- application teams that need familiar SQL engines more than globally distributed semantics

## 8.2 Core entities

### Instance
Managed database instance.

### Primary instance
Main writable instance.

### Read replica
Read-only replica for read scale or DR patterns.

### High availability configuration
Regional availability architecture.

### Backup
Managed backup artifact.

### PITR
Restore to a specific point in time.

### Database flag / setting
Engine tuning surface.

### Private IP
Private network connectivity path.

### IAM authentication
Identity-based DB login in supported engines/features.

## 8.3 Architecture fit

### Good fit
- OLTP web apps
- internal systems
- app teams needing PostgreSQL/MySQL familiarity
- moderate scale with strong managed-ops value

### Weak fit
- globally distributed write path
- near-limitless scale expectations
- workloads needing Spanner-like semantics

## 8.4 HA, replicas, backups, PITR

### Best practices
- enable HA for important production systems
- enable PITR where recovery objectives justify it
- use private IP for production network posture
- test restore and failover assumptions
- tune flags deliberately, not by cargo cult

### Common mistakes
- replicas assumed to be HA equivalent
- no restore drill
- public IP by convenience
- application connection handling not prepared for failover or maintenance

## 8.5 IAM database authentication

Google documents IAM authentication and IAM group authentication for Cloud SQL for PostgreSQL. [14]

### Best practices
- use IAM-backed access where it simplifies governance
- still manage DB-level privileges intentionally
- separate app identities from admin identities

### Common mistakes
- assuming IAM auth removes need for in-database privilege design
- identity granted but app role/search_path/schema grants not aligned

## 8.6 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| App sees connection timeouts during failover | app/db HA handling | client retry logic, connection pool config, HA event behavior |
| PITR enabled but restore target unavailable | backup policy | log-retention window, PITR config, backup-plan assumptions |
| Read replica creation blocked | configuration/edition | engine version, connectivity mode, primary config constraints |
| IAM auth login fails | identity/db grants | IAM role, DB user mapping, engine support, connection method |

---

# 9. AlloyDB for PostgreSQL

AlloyDB is PostgreSQL-compatible but architecturally distinct from Cloud SQL.

Google documents continuous backup and recovery, PITR, primary and read pool instances, and cluster/instance settings. [17][18][19][20]

## 9.1 Technical purpose

Use AlloyDB when you need:
- PostgreSQL compatibility
- stronger performance and scale posture than standard managed PostgreSQL
- managed HA and read scaling with a more database-platform-oriented architecture

## 9.2 Core entities

### Cluster
Top-level AlloyDB data/control boundary.

### Primary instance
Writable database instance.

### Read pool instance
Read scale-out instance class.

### Backup
Cluster backup artifact.

### Continuous backup
Always-on recovery posture.

### PITR
Point-in-time recovery.

### Secondary cluster
Disaster recovery / replication design component in supported architectures.

## 9.3 Read pools and transparent query forwarding

Google documents read pool instances and optional transparent query forwarding. [18]

### Best practices
- route read traffic intentionally
- understand which workloads benefit from read pools
- test failover and promotion assumptions

### Common mistakes
- assuming read pools solve write bottlenecks
- app drivers not separating read/write behavior

## 9.4 Recovery model

Google documents continuous backup enabled by default and out-of-place restore behavior. [17][21]

### Important concept
Restore is to a **new cluster**, not in-place overwrite.

### Best practices
- budget for restore environment creation
- practice cutover to restored cluster
- document which backups support which RTO path

### Common mistakes
- assuming in-place restore
- no post-restore app validation checklist

## 9.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Read scaling poor despite read pools | query/app routing | read/write routing, query shape, connection endpoint choice |
| PITR expected after cluster deletion | recovery-model misunderstanding | cluster existence requirements, backup type, recovery method |
| Restore completed but app still broken | cutover/app config | new cluster endpoint, credentials, network, app routing |

---

# 10. Spanner

Spanner is the globally scalable strongly consistent relational database.

Google documents backups, backup schedules, incremental backups, and change streams. [22][23][24]

## 10.1 Technical purpose

Use Spanner when you need:
- strong consistency at scale
- relational model with horizontal scale
- multi-region architecture
- low operational burden relative to self-managed globally distributed databases

## 10.2 Core entities

### Instance
Compute/storage capacity container.

### Database
Logical relational database.

### Configuration
Regional or multi-region instance placement config.

### Split
Internal storage/serving partition.

### Leader region
Primary write-serving leadership region in a database config.

### Backup
Full backup artifact.

### Backup schedule
Managed recurring backup config.

### Incremental backup
Backup containing changed data relative to prior backup in supported schedule patterns.

### Change stream
Database change capture stream.

## 10.3 Architecture fit

### Good fit
- globally distributed transactional systems
- multi-region critical systems
- high scale relational platforms

### Weak fit
- small simple app DB where Cloud SQL is enough
- teams unwilling to understand Spanner data-model and access-pattern implications

## 10.4 Data-model and scaling notes

### Important concept
Spanner scalability depends heavily on key design, write distribution, and hotspot avoidance.

### Best practices
- design primary keys to avoid concentrated write hotspots
- understand leader placement and latency tradeoffs
- separate OLTP design from warehouse-style expectations

### Common mistakes
- monotonic keys that concentrate writes
- multi-region config chosen without latency understanding
- assuming “managed” means schema design no longer matters

## 10.5 Backups and change streams

### Best practices
- use scheduled backups for operational discipline
- understand expiration and restore path
- use change streams for CDC/integration where justified

### Common mistakes
- no backup expiration governance
- no restore testing
- change streams added without downstream consumer design

## 10.6 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Write latency higher than expected | topology/schema | leader region, client locality, key distribution |
| Backup exists but restore plan unclear | ops process | backup expiration, target instance capacity, cutover playbook |
| CDC consumer lagging | downstream integration | change-stream config, consumer throughput, Dataflow/Data stream design |

---

# 11. Bigtable

Bigtable is the wide-column, very large-scale, low-latency database.

Google documents replication, app profiles, and backups. [25][26][27][28]

## 11.1 Technical purpose

Use Bigtable when you need:
- massive scale
- low-latency key/row lookups
- time-series or sparse wide-table patterns
- high write/read throughput outside relational semantics

## 11.2 Core entities

### Instance
Top-level Bigtable deployment.

### Cluster
Serving cluster within the instance.

### Table
Wide-column table.

### Row key
Primary access path. This is the most important design decision.

### Column family
Grouping for storage and access semantics.

### App profile
Routing and request-handling policy for applications.

### Replication
Multi-cluster replication model.

### Backup
Backup artifact in supported edition/features.

## 11.3 Row-key design

### Critical concept
Poor row-key design causes hot tablets/hotspots and uneven performance.

### Best practices
- design keys for read/write distribution
- model access pattern first, then schema
- separate app profiles by workload when useful
- use replication to isolate workloads or improve availability where justified

### Common mistakes
- sequential keys
- trying to model relational joins
- too many column families with poor access planning

## 11.4 App profiles

Google documents app profiles as settings that control request handling, and notes they can isolate workloads and route requests differently. [26][28]

### Best practices
- create app profiles by workload type
- separate latency-sensitive and batch traffic
- document which clients use which profile

### Common mistakes
- all clients use default behavior with no routing/traffic strategy

## 11.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Cluster overloaded unevenly | key design/routing | row-key distribution, hotspotting, app profile routing |
| Reads slow only for one app | client profile | app profile choice, request path, cluster routing |
| Backups unavailable or missing features | product tier/edition | edition support, backup enablement, region/cluster assumptions |

---

# 12. Firestore

Firestore is the managed document database for application-centric development.

Google documents backup resources and backup schedules through the Firestore Admin API. [29][30]

## 12.1 Technical purpose

Use Firestore when you need:
- document-centric data model
- developer-friendly app integration
- automatic indexing and scalable app data access
- flexible schema at document level

## 12.2 Core entities

### Database
Top-level Firestore database.

### Collection
Group of documents.

### Document
JSON-like record with fields.

### Index
Query-supporting structure.

### TTL
Automatic expiration for selected documents/fields in supported patterns.

### Backup
Database backup artifact.

### Backup schedule
Scheduled backup resource.

## 12.3 Architecture fit

### Good fit
- mobile/web app backends
- user profile/state
- event or session-like document models
- flexible hierarchical app data

### Weak fit
- heavy analytical SQL
- relational join-heavy workloads
- very strict relational transaction/reporting patterns better served elsewhere

## 12.4 Indexing and query behavior

### Best practices
- design queries and indexes together
- watch index growth and query limits
- keep document shape stable enough for operational clarity

### Common mistakes
- treating document DB like arbitrary JSON dump
- index requirements discovered only at runtime
- large hot documents or pathologies in document design

## 12.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Query fails unexpectedly | indexing/query model | required index, unsupported query combination, field path assumptions |
| Restore plan unclear | admin/ops | backup schedule, backup retention, restore target workflow |
| App latency spikes | data model | document hot paths, index fan-out, client retry behavior |

---

# 13. Memorystore for Redis and Memorystore for Memcached

These are in-memory acceleration services, not durable primary system-of-record stores.

## 13.1 Memorystore for Redis

Google documents standard-tier failover/manual failover and Redis maintenance concepts. [31][32]

### Technical purpose
Use for:
- caching
- session state
- rate limiting
- queue-like ephemeral coordination
- low-latency key-value access where Redis semantics fit

### Core entities
- instance
- tier
- primary/replica in supported tier
- maintenance window
- persistence/backup behavior depending on product mode and feature set

### Best practices
- treat Redis as cache first unless intentionally designed otherwise
- design for failover and cache refill
- set memory and eviction expectations clearly

### Common mistakes
- using cache as undisputed source of truth
- no failover testing
- app fails hard when cache is empty

## 13.2 Memorystore for Memcached

Google documents Memcached overview and maintenance-window concepts. [33][34]

### Technical purpose
Use for distributed volatile cache with Memcached semantics.

### Best practices
- use where simple distributed cache semantics fit
- design application to survive cache loss
- understand maintenance behavior and instance topology assumptions

### Common mistakes
- expecting Redis-like features from Memcached
- no warm-up/cold-cache strategy

## 13.3 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| App outage after cache failover | app resiliency | fallback path, retry strategy, cache refill, connection handling |
| Cache hit ratio low | data model/app use | key TTLs, cache invalidation, client behavior |
| Memcached maintenance caused surprise impact | ops assumptions | maintenance window, client reconnection behavior, cold-cache handling |

---

# 14. Pub/Sub

Pub/Sub is the messaging backbone for decoupled asynchronous systems.

Google documents ordering keys, dead-letter topics, retry behavior, seek, snapshots, and subscription properties. [35][36][37][38][39]

## 14.1 Technical purpose

Use Pub/Sub for:
- asynchronous decoupling
- fan-out delivery
- event ingestion
- stream buffering before downstream processing

## 14.2 Core entities

### Topic
Publish destination.

### Subscription
Delivery contract for consumers.

### Push subscription
Pub/Sub pushes to endpoint.

### Pull subscription
Consumer pulls messages.

### Ordering key
Preserves message order per key.

### Dead-letter topic
Receives undeliverable messages after retry thresholds.

### Retry policy
Controls retry timing behavior.

### Snapshot
Point-in-time ack-state record of a subscription.

### Seek
Replay or reposition subscription ack state.

### Filter
Subscription-side message selection.

## 14.3 Ordering keys

Google documents ordering-key behavior and retry nuances. [35][40]

### Best practices
- use ordering only when the business requirement is real
- partition with many ordering keys where possible
- understand throughput implications of serial ordering

### Common mistakes
- one global ordering key
- assuming ordering across unrelated keys
- retry behavior not understood in ordered streams

## 14.4 Dead-letter, retry, snapshots, seek

### Best practices
- configure dead-letter topics for failure isolation
- define ack/retry policy consciously
- use snapshots and seek for replay operations carefully
- document replay impact on downstream idempotency

### Common mistakes
- replaying without idempotent consumers
- dead-letter topic created but never monitored
- treating Pub/Sub as guaranteed exactly-once business processing without full design support

## 14.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Consumer sees duplicates | delivery semantics | ack timing, retries, idempotency, client handling |
| Ordered subscription stalls | ordering design | blocked key, consumer failure on one key, retry backlog |
| DLQ grows quickly | consumer/app logic | malformed messages, timeout, auth issue, downstream outage |
| Replay causes data corruption | downstream process | idempotency, seek scope, snapshot timing, side-effect control |

---

# 15. Dataflow

Dataflow is the managed Apache Beam execution engine for batch and streaming pipelines.

Google documents templates, Flex Templates, worker service accounts, and Streaming Engine. [41][42][43][44]

## 15.1 Technical purpose

Use Dataflow for:
- stream processing
- ETL/ELT
- Pub/Sub to BigQuery pipelines
- large-scale batch transforms
- Beam-based data engineering

## 15.2 Core entities

### Job
Running pipeline execution.

### Pipeline
Beam-defined data processing graph.

### Worker
Compute resource executing pipeline steps.

### Worker service account
Identity used by worker VMs.

### Template
Packaged pipeline launch format.

### Flex Template
Containerized, more flexible template packaging.

### Streaming Engine
Moves streaming execution state/processing into the Dataflow backend for improved efficiency in streaming jobs.

## 15.3 Flex Templates and worker identities

Google documents that Dataflow jobs use both a Dataflow service account and worker service accounts. [41][42]

### Best practices
- separate job-launch identity from worker identity concerns
- use Flex Templates for portable packaged deployment
- validate template parameters and image versions
- control worker service account permissions tightly

### Common mistakes
- worker service account too broad
- template parameters undocumented
- pipeline image drift across environments

## 15.4 Streaming Engine

Google documents Streaming Engine as moving execution out of worker VMs for streaming jobs. [43]

### Best practices
- use for suitable streaming jobs
- size and observe stream lag, watermark, and sink pressure
- protect BigQuery or downstream sinks from uncontrolled write patterns

### Common mistakes
- debugging only worker VMs when the issue is in stream semantics or downstream sink
- no backpressure/cost awareness in streaming architecture

## 15.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Job launches but workers fail | identity/runtime | worker service account, subnet/network, temp/staging paths |
| Streaming lag grows | pipeline/downstream | hot keys, sink pressure, watermark behavior, unbounded backlog |
| Template works in dev not prod | config/environment | parameters, service accounts, VPC path, org policy |
| BigQuery sink errors | downstream schema | schema mismatch, write disposition, row format, throttling |

---

# 16. Dataproc and Cloud Composer

These are orchestration/processing tools for different needs.

## 16.1 Dataproc

### Technical purpose
Managed Spark/Hadoop ecosystem runtime.

### Core entities
- cluster
- job
- workflow template
- serverless or cluster-based execution patterns depending on design

### Best fit
- Spark/Hadoop workloads
- migration of existing data-engineering patterns
- workloads where Beam/Dataflow is not the right abstraction

### Best practices
- use Dataproc when Spark ecosystem fit is real
- separate ephemeral and long-lived cluster patterns
- control image/version compatibility

### Common mistakes
- using Dataproc for everything because team knows Spark
- long-lived underutilized clusters
- poor dependency and image governance

## 16.2 Cloud Composer

Google documents Composer as managed Airflow, with environment, DAG scheduling, and explicit warnings that DAG/task isolation is weak and production/test environments should be separated. [45][46][47][48]

### Technical purpose
Managed Airflow orchestration.

### Core entities
- environment
- DAG
- scheduler
- worker
- task
- GCS-backed DAG deployment path

### Best practices
- separate test and prod Composer environments
- keep DAGs lightweight and deterministic
- use Composer for orchestration, not heavy data processing itself
- monitor scheduler health and DAG parse issues

### Common mistakes
- huge DAG files with side effects
- Composer used as compute engine
- DAG interference across environments
- no DAG deployment validation

## 16.3 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Spark job slow/unreliable | Dataproc design | cluster sizing, autoscaling, image version, data locality |
| DAG uploaded but not visible | Composer sync | DAG bucket sync logs, parse errors, environment health |
| Scheduler overloaded | orchestration design | DAG count, DAG complexity, parse time, task design |
| Workflow flaky only in prod | env separation | dependency versions, env config drift, private connectivity |

---

# 17. BigQuery

BigQuery is the analytical warehouse and governed query engine.

Google documents row-level security, column-level security with policy tags, and the BigQuery Security Center overview. [49][50][51]

## 17.1 Technical purpose

Use BigQuery for:
- analytics
- BI
- data warehousing
- large-scale SQL
- governed data sharing
- batch and streaming analytical consumption

## 17.2 Core entities

### Dataset
Container for tables, views, routines, models.

### Table
Stored analytical data object.

### Partitioning
Physical/logical data pruning pattern.

### Clustering
Sorting/organization optimization for query pruning.

### Slot / reservation model
Compute consumption and workload management in certain pricing/governance patterns.

### Row-level security policy
Restricts row visibility.

### Column-level security
Restricts column visibility via policy tags.

### Policy tag
Taxonomy-based governance signal applied to columns.

## 17.3 Partitioning and clustering

### Best practices
- partition by genuine filter pattern
- cluster by frequently filtered/joined columns when it helps
- design storage layout with query patterns, not afterthought

### Common mistakes
- no partitioning on massive fact tables
- overpartitioning with weak query benefit
- clustering chosen with no workload evidence

## 17.4 Row-level and column-level security

Google documents row-level policies and column-level security using policy tags. [49][50]

### Best practices
- use table design plus row/column controls together
- centralize policy-tag governance
- test authorized personas explicitly
- document interaction with views and downstream tools

### Common mistakes
- using only dataset IAM for all cases
- policy tags created but not governed
- security tested only with admin accounts

## 17.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Query scans too much data | storage layout | partition filter, clustering, query predicate, table design |
| User sees fewer rows than expected | row security | row-access policy, user identity, authorized view path |
| User cannot see one column | column security | policy tags, taxonomy permissions, downstream query behavior |
| BI tool works for admin not analyst | governance | dataset IAM, row policies, column tags, service-account identity |

---

# 18. Dataplex / Knowledge Catalog, Sensitive Data Protection, and Cloud Data Fusion

These are governance and integration accelerators.

## 18.1 Dataplex / Knowledge Catalog

Google documents Dataplex Universal Catalog as a unified metadata and governance solution, with continuity of lineage, data quality, glossary, and governance features during transition to Knowledge Catalog experience. [52][53][54]

### Technical purpose
Provide metadata, governance, lineage, glossary, and discovery context across distributed data assets.

### Best practices
- use as governance layer, not only search
- assign ownership and metadata responsibilities
- connect to lineage and policy workflows

### Common mistakes
- catalog exists but nobody curates it
- glossary/tags become stale
- governance project disconnected from actual producers

## 18.2 Sensitive Data Protection

### Technical purpose
Inspect, classify, de-identify, and tokenize sensitive data.

### Core entities
- inspection template
- de-identification template
- finding
- infoType
- risk analysis pattern

### Best practices
- scan before broad data sharing
- integrate findings with metadata/governance workflows
- use templates for repeatable policy

### Common mistakes
- one-time scans only
- findings generated but not operationalized
- tokenization/de-id decisions not matched to business use case

## 18.3 Cloud Data Fusion

### Technical purpose
Managed integration/ETL development platform for pipeline assembly.

### Best fit
- integration workflows
- GUI-first pipeline authoring
- teams needing faster data-integration assembly than code-first patterns

### Common mistakes
- using Data Fusion when Beam/Dataflow or native ingestion is simpler
- no promotion model for integration pipelines

## 18.4 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Analysts cannot find trusted dataset | governance process | catalog metadata, ownership, glossary completeness |
| DLP findings exist but no controls changed | operating model | integration with policy/catalog workflows |
| Integration pipeline hard to promote | delivery discipline | environment separation, parameterization, source/target auth |

---

# 19. Secret Manager, Cloud KMS, Cloud HSM, and Cloud EKM

These are the cryptographic and secret-control primitives for the platform.

Google documents Secret Manager secret/version model, replication choices, rotation schedules, and notes that secret payload data is stored in immutable secret versions. Google documents Cloud KMS key management, import jobs, key import, and Cloud EKM. [55][56][57][58][59][60][61]

## 19.1 Secret Manager

### Technical purpose
Store and manage secrets and credentials.

### Core entities
- secret
- secret version
- replication policy
- rotation schedule
- labels/metadata

### Important concept
A secret is the container.  
A secret version contains the immutable secret payload. [56][57]

### Replication
Google recommends automatic replication for most users in the documented guidance. [58]

### Rotation
Google documents rotation schedules and notes the `rotation_period` cannot be less than one hour. [59]

### Best practices
- version secrets intentionally
- automate rotation where possible
- use Secret Manager, not env vars or source repo
- map secret ownership to service ownership

### Common mistakes
- one secret reused by many apps with no lifecycle control
- old versions never destroyed or retired
- rotation configured but downstream system not updated

---

## 19.2 Cloud KMS / HSM / import jobs / EKM

### Technical purpose
Create, manage, import, or externally source encryption keys for Google Cloud services and applications.

### Core entities
- key ring
- crypto key
- key version
- primary key version
- import job
- protection level
- Cloud EKM external key link

### Key import
Google documents import jobs, wrapping keys, and imported key-version behavior, including that imported symmetric versions are not automatically made primary in every scenario. [60][61]

### Cloud EKM
Use when keys must remain under external control in compatible external key management systems. [62]

### Best practices
- separate key-admin and data-admin roles
- version and rotate keys intentionally
- test key import and failover processes
- use EKM only when requirement justifies additional dependency complexity

### Common mistakes
- unclear key ownership
- imported key not promoted as expected
- application dependency on external key service not tested under failure

## 19.3 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| App cannot read secret | IAM/runtime | secret accessor role, version state, project/location assumptions |
| Rotation fired but app still uses old secret | integration | downstream secret update logic, rollout order, old-version cleanup |
| Key import succeeded but app still encrypts with old key | key management | primary version selection, application key reference, service config |
| EKM-backed crypto operations fail intermittently | dependency path | external key manager availability, network path, timeout assumptions |

---

# 20. Document AI, Vision AI, Speech-to-Text, and Translation

These are ingestion and enrichment services that often feed data pipelines.

## 20.1 Document AI

Google documents processors and processor versions, including customized versions and processor-version management. [63][64][65]

### Technical purpose
Extract structured information from documents.

### Core entities
- processor
- processor version
- specialized processor type
- customized version

### Best practices
- choose processor by document type
- version processor behavior explicitly
- validate extraction quality before operationalizing downstream automations

### Common mistakes
- one generic OCR assumption for all documents
- processor-version changes rolled into production with no evaluation
- downstream schemas tightly coupled to unstable extraction outputs

## 20.2 Vision AI

### Technical purpose
Image analysis and OCR-related enrichment.

### Best fit
Image-centric classification, detection, and OCR-like tasks where full document understanding is not required.

## 20.3 Speech-to-Text

### Technical purpose
Audio transcription for voice and speech workloads.

### Best fit
Call processing, transcription, voice analytics.

## 20.4 Cloud Translation

### Technical purpose
Language translation and localization acceleration.

### Core concepts
- glossary support
- adaptive/quality controls depending on product mode and use case

## 20.5 Troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Extraction quality suddenly changed | model/version | processor version, input document quality, schema assumptions |
| OCR or AI output breaks pipeline | downstream contract | parser expectations, null handling, schema evolution |
| Translation quality unacceptable for domain terms | config/model fit | glossary/domain adaptation, language pair, post-edit expectations |

---

# 21. End-to-End Reference Architecture Example

A regulated enterprise needs:
- relational OLTP
- global transactions
- event ingestion
- ETL and streaming
- analytical warehouse
- metadata governance
- secret rotation
- key control
- file shares for legacy tools
- strong storage retention controls

A strong design could be:

### Operational state
- Cloud SQL for ordinary app databases
- AlloyDB for PostgreSQL-heavy high-performance systems
- Spanner for globally distributed transaction-critical systems
- Bigtable for time-series/high-throughput key access
- Firestore for app-centric document state
- Memorystore for cache/session
- Filestore for NFS-dependent workloads
- Cloud Storage for object data lake, exports, backups, and archives

### Movement
- Pub/Sub for events
- Dataflow for stream/batch transforms
- Dataproc where Spark ecosystem fit is real
- Composer for orchestration of cross-system workflows

### Analytics and governance
- BigQuery for warehouse and BI
- Dataplex / Knowledge Catalog for metadata and lineage
- Sensitive Data Protection for inspection and de-identification
- Data Fusion where managed integration assembly is useful

### Security
- Secret Manager for credentials
- Cloud KMS / HSM / EKM for keys and encryption control
- Bucket Lock and retention policies for immutable object retention

This is the Week 4 principle:

> choose the data service by access pattern and recovery model, then wrap it with movement, governance, and cryptographic controls.

---

# 22. Troubleshooting Flow

When a data platform issue occurs, inspect in this order:

1. **Data model**
   - Is the service choice correct?
   - Is the schema/key/object layout correct?

2. **Access path**
   - Can the caller authenticate and reach the resource?

3. **Protection and retention**
   - Did lifecycle, retention, soft delete, policy tags, row policies, or key state affect behavior?

4. **Scale path**
   - Hot keys, hot partitions, cluster sizing, slot pressure, queue backlog?

5. **Recovery path**
   - Backup exists, but can it restore in time and in the expected form?

6. **Governance path**
   - Was data cataloged, tagged, scanned, or classified correctly?

---

# 23. Common Week 4 Mistakes

- choosing data services by familiarity instead of access pattern
- assuming backup equals restore readiness
- using object storage like file storage
- using Firestore or Bigtable like a relational DB
- weak row-key design in Bigtable
- replicas assumed to solve every relational availability issue
- no PITR testing
- retention controls enabled with no lifecycle governance
- pub/sub replay without idempotency
- streaming pipelines with no downstream pressure controls
- BigQuery security tested only with admin personas
- secrets treated as static config
- key ownership unclear
- DLP findings not connected to governance workflows
- ML/document enrichment outputs pushed downstream without schema/quality validation

---

# 24. Revision Checklist

You should be able to explain clearly:

- when to choose block, object, file, relational, globally distributed relational, wide-column, document, or cache
- how Cloud Storage soft delete, versioning, retention policies, Bucket Lock, managed folders, and HNS differ
- how Filestore backups and replication fit into file-share recovery design
- how Cloud SQL differs from AlloyDB architecturally and operationally
- how Spanner differs from Cloud SQL and when its consistency/scale model matters
- why Bigtable row-key design is the central design decision
- how Firestore indexing and backup constructs fit operationally
- how Redis and Memcached differ
- how Pub/Sub ordering, retries, DLQs, seek, and snapshots affect consumer design
- how Dataflow worker service accounts and templates affect deployment
- how Dataproc and Composer differ in role
- how BigQuery partitioning, clustering, row-level security, and column-level security fit together
- how Dataplex / Knowledge Catalog and Sensitive Data Protection fit into governance
- how Secret Manager versions and rotation differ from KMS keys and key versions
- how key import and Cloud EKM alter operational risk
- how Document AI processor versions affect downstream data contracts

---

# 25. Memory Anchors

- **Persistent Disk / Hyperdisk** = VM block storage
- **Cloud Storage** = object store
- **soft delete** = recover deleted object
- **versioning** = keep generations
- **retention policy** = time-based delete prevention
- **Bucket Lock** = irreversible retention lock
- **managed folder** = folder-level IAM resource
- **Filestore** = managed NFS
- **Cloud SQL** = familiar managed relational DB
- **AlloyDB** = higher-performance PostgreSQL platform
- **Spanner** = globally scalable strongly consistent relational DB
- **Bigtable** = low-latency wide-column at scale
- **Firestore** = document DB
- **Memorystore** = cache
- **Pub/Sub** = async messaging backbone
- **Dataflow** = Beam execution engine
- **Dataproc** = Spark/Hadoop runtime
- **Composer** = Airflow orchestration
- **BigQuery** = analytical warehouse
- **Dataplex / Knowledge Catalog** = metadata and governance
- **Sensitive Data Protection** = inspect/de-identify sensitive data
- **Secret Manager** = secrets container + versions
- **Cloud KMS** = keys and crypto control
- **Cloud EKM** = external key control
- **Document AI** = document extraction platform

---

# 26. Official References

## Storage
- Soft delete overview  
  https://docs.cloud.google.com/storage/docs/soft-delete

- Use and restore soft-deleted objects  
  https://docs.cloud.google.com/storage/docs/use-soft-deleted-objects

- Hierarchical namespace  
  https://docs.cloud.google.com/storage/docs/hns-overview

- Managed folders  
  https://docs.cloud.google.com/storage/docs/managed-folders

- Bucket Lock  
  https://docs.cloud.google.com/storage/docs/bucket-lock

- Use and lock retention policies  
  https://docs.cloud.google.com/storage/docs/using-bucket-lock

- Object Lifecycle Management  
  https://docs.cloud.google.com/storage/docs/lifecycle

- Manage object lifecycles  
  https://docs.cloud.google.com/storage/docs/managing-lifecycles

## Filestore
- Filestore overview  
  https://docs.cloud.google.com/filestore/docs/overview

- Get started with backups  
  https://docs.cloud.google.com/filestore/docs/backups-before-you-start

- About backups  
  https://docs.cloud.google.com/filestore/docs/backups

- Create enhanced backups  
  https://docs.cloud.google.com/filestore/docs/backups-enhanced

- Backup chains  
  https://docs.cloud.google.com/filestore/docs/backup-chains

- Backup best practices  
  https://docs.cloud.google.com/filestore/docs/backups-best-practices

## Cloud SQL and AlloyDB
- Cloud SQL PITR for MySQL  
  https://docs.cloud.google.com/sql/docs/mysql/backup-recovery/pitr

- Cloud SQL PITR for PostgreSQL  
  https://docs.cloud.google.com/sql/docs/postgres/backup-recovery/configure-pitr

- Cloud SQL IAM authentication for PostgreSQL  
  https://docs.cloud.google.com/sql/docs/postgres/iam-authentication

- Cloud SQL read replicas for PostgreSQL  
  https://docs.cloud.google.com/sql/docs/postgres/replication/create-replica

- AlloyDB backup and recovery overview  
  https://docs.cloud.google.com/alloydb/docs/backup/overview

- AlloyDB PITR  
  https://docs.cloud.google.com/alloydb/docs/backup/restore-pitr

- AlloyDB read pool instances  
  https://docs.cloud.google.com/alloydb/docs/instance-read-pool-create

- AlloyDB cluster settings  
  https://docs.cloud.google.com/alloydb/docs/reference/cluster-settings

- AlloyDB backup/restore troubleshooting  
  https://docs.cloud.google.com/alloydb/docs/troubleshoot/backup-restore

## Spanner
- Spanner backups overview  
  https://docs.cloud.google.com/spanner/docs/backup

- Create backups  
  https://docs.cloud.google.com/spanner/docs/backup/create-backups

- Manage backups  
  https://docs.cloud.google.com/spanner/docs/backup/manage-backups

- Change streams  
  https://cloud.google.com/spanner/docs/change-streams/manage

## Bigtable and Firestore
- Bigtable replication overview  
  https://docs.cloud.google.com/bigtable/docs/replication-overview

- Bigtable app profiles overview  
  https://docs.cloud.google.com/bigtable/docs/app-profiles

- Create and configure app profiles  
  https://docs.cloud.google.com/bigtable/docs/configuring-app-profiles

- Bigtable backups overview  
  https://docs.cloud.google.com/bigtable/docs/backups

- Firestore backup resource  
  https://docs.cloud.google.com/firestore/docs/reference/rest/v1/projects.locations.backups

- Firestore backup schedules  
  https://docs.cloud.google.com/firestore/docs/reference/rest/v1/projects.databases.backupSchedules

## Caching and messaging
- Memorystore for Redis documentation  
  https://docs.cloud.google.com/memorystore/docs/redis

- Manual failover for Memorystore for Redis  
  https://cloud.google.com/memorystore/docs/redis/about-manual-failover

- Memorystore for Memcached overview  
  https://docs.cloud.google.com/memorystore/docs/memcached/memcached-overview

- Memorystore for Memcached maintenance  
  https://docs.cloud.google.com/memorystore/docs/memcached/about-maintenance

- Pub/Sub ordering  
  https://docs.cloud.google.com/pubsub/docs/ordering

- Dead-letter topics  
  https://docs.cloud.google.com/pubsub/docs/dead-letter-topics

- Retry requests  
  https://docs.cloud.google.com/pubsub/docs/retry-requests

- Replay and purge messages with seek  
  https://docs.cloud.google.com/pubsub/docs/replay-overview

- Subscription properties  
  https://docs.cloud.google.com/pubsub/docs/subscription-properties

## Data processing and orchestration
- Dataflow templates  
  https://docs.cloud.google.com/dataflow/docs/concepts/dataflow-templates

- Build and run Flex Templates  
  https://docs.cloud.google.com/dataflow/docs/guides/templates/using-flex-templates

- Configure Flex Templates  
  https://docs.cloud.google.com/dataflow/docs/guides/templates/configuring-flex-templates

- Run Flex Templates  
  https://docs.cloud.google.com/dataflow/docs/guides/templates/run-flex-templates

- Use Streaming Engine  
  https://docs.cloud.google.com/dataflow/docs/streaming-engine

- Cloud Composer overview  
  https://docs.cloud.google.com/composer/docs/composer-3/composer-overview

- Create Composer environments  
  https://docs.cloud.google.com/composer/docs/composer-3/create-environments

- Write DAGs  
  https://docs.cloud.google.com/composer/docs/composer-3/write-dags

- Schedule and trigger DAGs  
  https://cloud.google.com/composer/docs/composer-3/schedule-and-trigger-dags

- Add and update DAGs  
  https://docs.cloud.google.com/composer/docs/composer-3/manage-dags

## Analytics and governance
- BigQuery row-level security  
  https://docs.cloud.google.com/bigquery/docs/managing-row-level-security

- BigQuery row-level security intro  
  https://docs.cloud.google.com/bigquery/docs/row-level-security-intro

- BigQuery column-level access control  
  https://docs.cloud.google.com/bigquery/docs/column-level-security

- BigQuery column-level access control intro  
  https://docs.cloud.google.com/bigquery/docs/column-level-security-intro

- BigQuery Security Center overview  
  https://docs.cloud.google.com/bigquery/docs/security-center-overview

- Dataplex Universal Catalog overview  
  https://cloud.google.com/dataplex/docs/introduction

- Knowledge Catalog use cases  
  https://docs.cloud.google.com/dataplex/docs/use-cases

- Transition from Dataplex Universal Catalog  
  https://docs.cloud.google.com/dataplex/docs/transition-dataplex-universal-catalog

## Secrets and encryption
- Secret Manager overview  
  https://docs.cloud.google.com/secret-manager/docs/overview

- View secret details  
  https://docs.cloud.google.com/secret-manager/docs/view-secret-details

- Add a secret version  
  https://docs.cloud.google.com/secret-manager/docs/add-secret-version

- Create rotation schedules  
  https://docs.cloud.google.com/secret-manager/docs/secret-rotation

- Rotation recommendations  
  https://docs.cloud.google.com/secret-manager/docs/rotation-recommendations

- Cloud KMS overview  
  https://docs.cloud.google.com/kms/docs/key-management-service

- Key import overview  
  https://docs.cloud.google.com/kms/docs/key-import

- Import a key version  
  https://docs.cloud.google.com/kms/docs/importing-a-key

- Import jobs API reference  
  https://docs.cloud.google.com/kms/docs/reference/rest/v1/projects.locations.keyRings.importJobs

- Cloud EKM  
  https://docs.cloud.google.com/kms/docs/ekm

## Intelligent extraction services
- Document AI processors list  
  https://docs.cloud.google.com/document-ai/docs/processors-list

- Document AI reference  
  https://docs.cloud.google.com/document-ai/docs/reference

- Processor versions reference  
  https://docs.cloud.google.com/document-ai/docs/reference/rest/v1/projects.locations.processors.processorVersions

- Manage processor versions  
  https://docs.cloud.google.com/document-ai/docs/manage-processor-versions

---

# 27. Final Summary

Week 4 is about **choosing the right stateful service, moving data safely, governing it, and protecting it cryptographically and operationally**.

The strongest mental model is:

- **Persistent Disk / Hyperdisk / Filestore / Cloud Storage** = storage primitives by access model
- **Cloud SQL / AlloyDB / Spanner / Bigtable / Firestore / Memorystore** = database and state engines by consistency and access pattern
- **Pub/Sub / Dataflow / Dataproc / Composer** = movement and orchestration
- **BigQuery / Dataplex / Sensitive Data Protection** = analytics and governance
- **Secret Manager / KMS / EKM** = secret and key control
- **Document AI and adjacent AI extraction services** = ingestion and enrichment

When you can explain the **state model, access model, recovery model, movement model, and governance model** together, Week 4 is understood architecturally.
