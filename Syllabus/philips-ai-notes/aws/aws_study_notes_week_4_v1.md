# AWS Study Notes — Week 4
## Data, Storage, Backup, and Stateful Reliability

> This is the **deep technical Week 4 guide**.  
> It is written as a **service-by-service study README** with:
> - technical explanations
> - key entities
> - architecture usage
> - controls
> - examples
> - best practices
> - common mistakes
> - troubleshooting cues
> - official AWS references
>
> **Noise control:** concise wording, high information density, minimal filler.

---

# 1. Why Week 4 Matters

Week 4 is about **state**.

If Week 3 was about where workloads run, Week 4 is about:
- where data lives
- how it is protected
- how it is queried
- how it is shared
- how it is restored
- how it survives failures and mistakes

The core Week 4 idea is:

> **Stateless compute is replaceable. Stateful systems require explicit design for durability, consistency, performance, retention, security, and recovery.**

A strong Week 4 understanding lets you answer:
- When should data be on S3, EBS, EFS, FSx, RDS, Aurora, DynamoDB, Redis, or Redshift?
- How do backups differ from replication?
- How do you apply encryption, lifecycle, retention, and deletion controls?
- How do data lakes, ETL, catalogs, and lake governance work together?
- How do queues, streams, events, and workflow orchestration differ?
- How do secrets and encryption keys fit into stateful reliability?

---

# 2. Week 4 Learning Outcomes

By the end of this week, you should be able to:

- explain **S3** buckets, objects, prefixes, access points, MRAP, Object Lock, lifecycle, replication, and storage classes
- explain **EBS**, **EFS**, and **FSx** entities and reliability controls
- explain **RDS** and **Aurora** instances, clusters, replicas, backups, endpoints, and failover controls
- explain **DynamoDB** tables, keys, indexes, streams, global tables, PITR, and hot-partition behavior
- explain specialized data stores: **DocumentDB**, **Neptune**, **Timestream**, **ElastiCache**, and **MemoryDB**
- explain **OpenSearch**, **Redshift**, and **Athena** in analytics/search patterns
- explain **Glue**, **DataBrew**, and **Lake Formation** in data lake architectures
- explain **Kinesis**, **Firehose**, **MSK**, **SQS**, **SNS**, **EventBridge**, and **Step Functions**
- explain **Secrets Manager**, **KMS**, **AWS Backup**, and **DMS**
- reason about backup, restore, failover, replication, retention, and operational recovery

---

# 3. Architectural Mental Model for Week 4

Think in layers:

1. **Storage layer**  
   S3, EBS, EFS, FSx

2. **Transactional database layer**  
   RDS, Aurora, DynamoDB, DocumentDB, Neptune, Timestream

3. **Cache and in-memory layer**  
   ElastiCache, MemoryDB

4. **Analytics and query layer**  
   OpenSearch, Redshift, Athena

5. **Data lake and ETL layer**  
   Glue, DataBrew, Lake Formation

6. **Streaming, messaging, and orchestration layer**  
   Kinesis, Firehose, MSK, SQS, SNS, EventBridge, Step Functions

7. **Protection and migration layer**  
   Secrets Manager, KMS, AWS Backup, DMS

If a stateful workload fails, find the broken layer first.

---

# 4. Amazon S3

Amazon S3 is the foundational object storage service in AWS.

## 4.1 Technical purpose
Store and retrieve objects at massive scale with high durability and flexible access patterns.

## 4.2 Core entities

### Bucket
Top-level container for objects, policies, encryption defaults, lifecycle, replication, and access controls.

### Object
Stored data item consisting of key, content, metadata, and optional version information.

### Prefix
Logical key namespace pattern used for organization and policy design.

### Access point
Named access path with its own policy for a bucket or shared data set.

### Multi-Region Access Point (MRAP)
Global access layer across S3 buckets in multiple Regions for multi-Region access routing.

### Bucket policy
Resource-based policy controlling access to the bucket and objects.

### Block Public Access
Top-level guardrail preventing public exposure patterns.

### Object Lock
WORM-style retention control for compliant write-once retention.

### Lifecycle rule
Automates transition and expiration actions on objects.

### Replication rule
Controls replication to another bucket/Region/account.

### Storage class
Defines durability/access tradeoff and cost model.

## 4.3 Key controls
- bucket policy
- default encryption
- versioning
- Object Ownership
- Block Public Access
- Object Lock retention modes
- lifecycle rules
- replication settings
- access logging and eventing where used

## 4.4 Example
A log archive bucket uses:
- versioning enabled
- bucket owner enforced object ownership
- default SSE-KMS encryption
- lifecycle transition to colder storage class
- cross-account replication to security archive account
- Object Lock for regulated logs

## 4.5 Best practices
- enable Block Public Access unless there is a deliberate public use case
- use versioning on important buckets
- use least-privilege bucket policies
- separate raw, processed, and curated data prefixes or buckets
- use lifecycle policies intentionally, not generically
- use replication for resilience or compliance, not as a backup substitute

## 4.6 Common mistakes
- public bucket exposure through weak policies
- assuming replication replaces backup/restore design
- using prefixes without governance or naming standards
- storing critical data without versioning or retention controls

## 4.7 Troubleshooting cues
- access denied often means bucket policy, IAM, KMS, or Object Ownership mismatch
- unexpected cost often comes from storage class misuse, replication, or request patterns
- data missing may be lifecycle expiration, delete marker, version confusion, or replication lag

---

# 5. Amazon EBS

Amazon EBS provides block storage volumes for EC2.

## 5.1 Technical purpose
Persistent block storage for EC2-based workloads requiring disk semantics.

## 5.2 Core entities

### Volume
Network-attached block device presented to EC2.

### Snapshot
Point-in-time incremental backup of an EBS volume stored in S3-managed snapshot infrastructure.

### Snapshot archive
Lower-cost archive tier for rarely accessed snapshots.

### Volume type
Performance/cost model such as general purpose SSD, provisioned IOPS SSD, or HDD classes.

### Provisioned IOPS
Performance setting for high-IOPS workloads.

### Throughput setting
Relevant volume throughput configuration for supported SSD types.

### Fast Snapshot Restore (FSR)
Improves restore performance from snapshots in selected AZs.

## 5.3 Key controls
- encryption and KMS key choice
- delete-on-termination behavior
- snapshot sharing
- volume type sizing
- IOPS/throughput settings
- backup policy

## 5.4 Example
A transactional EC2 database uses:
- io2 or high-performance SSD volume
- encrypted snapshots
- scheduled backup policy
- separate data and log volumes
- tested restore procedure

## 5.5 Best practices
- pick volume type based on workload, not habit
- encrypt by default
- snapshot regularly and test restore
- set delete-on-termination carefully for root vs data volumes
- monitor burst/performance behavior

## 5.6 Common mistakes
- gp volume selection without checking IOPS/throughput needs
- no restore testing from snapshots
- accidental data loss from wrong delete-on-termination settings
- sharing snapshots too broadly

## 5.7 Troubleshooting cues
- slow volume may be undersized, wrong type, or instance bottleneck
- restore delays may require FSR planning
- mount issues are often OS-level rather than EBS service failures

---

# 6. Amazon EFS

Amazon EFS is managed elastic NFS file storage for Linux workloads.

## 6.1 Technical purpose
Provide shared, multi-AZ file storage with POSIX semantics.

## 6.2 Core entities

### File system
Top-level EFS storage resource.

### Mount target
AZ-specific network endpoint inside subnets for clients to mount the file system.

### Access point
Application-specific entry point with path and POSIX identity controls.

### Lifecycle policy
Moves inactive files to lower-cost storage classes where supported.

### Throughput mode
Bursting, provisioned, or elastic behavior depending on service options.

### Performance mode
General purpose or max I/O style performance behavior depending on workload requirements.

### Replication configuration
Cross-Region or supported replication relationship.

## 6.3 Key controls
- file system policy
- security groups
- access point POSIX identities
- lifecycle configuration
- backup and replication settings
- encryption at rest and in transit

## 6.4 Example
A container platform mounts EFS through access points so each application gets:
- fixed application path
- enforced UID/GID
- isolated mount behavior

## 6.5 Best practices
- use access points for app isolation and path consistency
- deploy mount targets in used AZs
- enforce SG rules tightly
- monitor throughput and burst behavior for shared workloads
- validate restore and replication strategies

## 6.6 Common mistakes
- assuming EFS is a drop-in block storage replacement
- poor NFS security group design
- mounting across AZs without understanding latency and cost implications
- using one file system for too many unrelated applications

## 6.7 Troubleshooting cues
- mount failures are often SG, DNS, subnet, or mount helper issues
- performance issues often come from throughput mode mismatch or application I/O pattern

---

# 7. Amazon FSx

Amazon FSx provides managed file systems for specific enterprise or high-performance file system types.

## 7.1 Technical purpose
Deliver managed file systems with protocol and performance characteristics tailored to workloads such as Windows, Lustre, NetApp ONTAP, or OpenZFS.

## 7.2 Core entities
- file system
- backup
- storage virtual machine (for ONTAP where applicable)
- volume
- share
- endpoint
- deployment type
- throughput capacity

## 7.3 Typical variants
- FSx for Windows File Server
- FSx for Lustre
- FSx for NetApp ONTAP
- FSx for OpenZFS

## 7.4 Example
A Windows application requiring SMB, AD integration, and shared storage uses FSx for Windows instead of EFS.

## 7.5 Best practices
- choose FSx variant by protocol and workload fit
- understand deployment type and HA model
- align backup and restore policies to business need
- integrate authentication/authorization correctly for enterprise file access

## 7.6 Common mistakes
- choosing EFS when Windows/SMB semantics are required
- underestimating throughput capacity sizing
- using the wrong FSx family for the access pattern

## 7.7 Troubleshooting cues
- access issues usually map to network, identity integration, share permissions, or client protocol settings
- throughput constraints often appear as application latency before they are recognized as storage configuration issues

---

# 8. Amazon RDS and Amazon Aurora

RDS and Aurora provide managed relational databases, but with different architectures and operational tradeoffs.

---

## 8.1 Amazon RDS

### Technical purpose
Managed relational database service for common database engines with reduced operational burden versus self-managed EC2 databases.

### Core entities
- DB instance
- DB snapshot
- automated backups
- parameter group
- option group
- subnet group
- read replica
- reserved DB instance
- maintenance window

### Key controls
- Multi-AZ deployment
- backup retention
- deletion protection
- storage autoscaling
- encryption
- IAM database authentication where supported
- maintenance windows
- parameter alignment

### Example
A business application runs PostgreSQL on RDS with:
- Multi-AZ enabled
- 7-day backup retention
- deletion protection
- read replica for read-heavy reporting
- custom parameter group

### Best practices
- enable Multi-AZ for production transactional workloads
- use deletion protection for critical databases
- tune backup retention deliberately
- separate parameter groups by engine/workload class
- use read replicas for scale-out reads, not as a substitute for HA architecture confusion

### Common mistakes
- confusing Multi-AZ with read scaling
- production databases with no deletion protection
- treating snapshots as the only recovery plan
- unreviewed parameter changes in shared parameter groups

### Troubleshooting cues
- failover impact may come from DNS/client timeout handling
- performance issues may be query, storage, parameter, or instance class related
- replica lag often indicates write pressure or workload mismatch

---

## 8.2 Amazon Aurora

### Technical purpose
Managed relational database engine with cloud-native storage architecture and cluster-based design.

### Core entities
- DB cluster
- writer instance
- Aurora Replica
- cluster endpoint
- reader endpoint
- custom endpoint
- DB snapshot
- global database
- backtrack window

### Architectural distinction
Aurora separates compute instances from a distributed storage layer and supports cluster-oriented endpoints.

### Example
An Aurora cluster uses:
- one writer
- two Aurora Replicas
- cluster endpoint for writes
- reader endpoint for read-balanced traffic
- custom endpoint for analytics readers
- global database for cross-Region DR

### Best practices
- understand endpoint roles clearly
- use reader endpoint for generic read scaling
- validate failover behavior in application connection handling
- use global database only with explicit DR/runbook planning
- use backtrack only where it fits engine/workload and operational policy

### Common mistakes
- sending writes to reader endpoint
- assuming all replicas behave identically for every workload
- enabling features without operational testing
- confusing backup/replication/failover responsibilities

### Troubleshooting cues
- failover issues often surface as client retry or DNS caching problems
- reader lag, query plans, or endpoint targeting can create uneven performance
- global database switchover must be practiced, not only documented

---

# 9. Amazon DynamoDB

DynamoDB is a fully managed NoSQL key-value and document database optimized for low-latency scale.

## 9.1 Technical purpose
Provide horizontally scalable, highly available, low-latency storage without server management.

## 9.2 Core entities

### Table
Top-level storage resource.

### Item
Single record in the table.

### Attribute
Field inside an item.

### Partition key
Primary sharding key for data distribution.

### Sort key
Second primary key component for ordered item grouping.

### Local secondary index (LSI)
Alternative sort key on same partition key.

### Global secondary index (GSI)
Alternate access pattern with different partition/sort keys.

### Stream
Change feed for item-level modifications.

### Global table
Multi-Region active-active table replication.

### Backup
On-demand backup or point-in-time recovery support.

### TTL
Automatic expiration marker for items.

## 9.3 Key controls
- capacity mode
- auto scaling
- table class
- encryption
- PITR
- deletion protection
- resource-based policies
- stream settings

## 9.4 Example
An orders table uses:
- partition key: `customer_id`
- sort key: `order_timestamp`
- GSI for `order_status`
- PITR enabled
- Streams enabled for event-driven downstream processing

## 9.5 Best practices
- design access patterns first
- choose partition keys to avoid hot partitions
- enable PITR on important tables
- use TTL for ephemeral data cleanup
- use GSIs intentionally and cost-aware
- test global tables behavior before critical adoption

## 9.6 Common mistakes
- designing like a relational table and expecting ad hoc query flexibility
- hot partition keys such as one constant tenant or one current-day key
- too many GSIs without cost/performance justification
- enabling streams without consumer design

## 9.7 Troubleshooting cues
- throttling may be hot partitions, insufficient capacity, or skewed access patterns
- inconsistent read behavior may be eventual consistency misunderstanding
- replication lag on global tables needs architectural awareness, not wishful assumptions

---

# 10. Specialized Databases and In-Memory Services

---

## 10.1 Amazon DocumentDB

### Technical purpose
Managed document database service for JSON/document-oriented workloads.

### Core entities
- cluster
- instance
- endpoint
- snapshot
- parameter group

### Best fit
Document-centric application models with managed service operations.

### Common mistakes
- assuming feature parity with every MongoDB deployment pattern without checking service compatibility
- using it for graph, relational, or time-series problems that need other engines

---

## 10.2 Amazon Neptune

### Technical purpose
Managed graph database for relationship-heavy graph workloads.

### Core entities
- DB cluster
- instance
- endpoint
- snapshot
- parameter group

### Best fit
Fraud graphs, knowledge graphs, recommendations, network relationship traversal.

### Common mistakes
- forcing graph problems into relational joins or document stores
- choosing Neptune without a real graph traversal use case

---

## 10.3 Amazon Timestream for LiveAnalytics

### Technical purpose
Managed time-series database for timestamped metrics/events.

### Core entities
- database
- table

### Best fit
IoT metrics, telemetry, operational time-series, application event measurements.

### Common mistakes
- using relational schemas for very high-volume time-series when Timestream is the better fit
- poor retention/query design

---

## 10.4 Amazon ElastiCache

### Technical purpose
Managed in-memory caching layer, commonly Redis-compatible or Memcached-based depending on workload.

### Core entities
- serverless cache in supported model
- replication group
- shard
- user
- user group
- snapshot
- endpoint

### Typical use cases
- cache-aside
- session store
- leaderboards
- rate limiting
- hot key/value acceleration

### Best practices
- define eviction policy intentionally
- monitor hit ratio and memory pressure
- secure access with VPC placement, auth, and encryption where supported
- understand cluster mode/sharding design

### Common mistakes
- treating cache as primary durable store
- no eviction planning
- poor key design causing memory or hot-key problems

---

## 10.5 Amazon MemoryDB

### Technical purpose
Redis-compatible durable in-memory database, positioned differently from pure cache patterns.

### Core entities
- cluster
- ACL
- user
- snapshot
- endpoint

### Best fit
Low-latency data workloads needing durability plus Redis-compatible access patterns.

### Common mistakes
- assuming it is the same decision as ElastiCache
- choosing it when cache-only behavior was sufficient

---

# 11. OpenSearch, Redshift, and Athena

---

## 11.1 Amazon OpenSearch Service

### Technical purpose
Managed search and analytics engine for indexed search, log analytics, and text/query workloads.

### Core entities
- domain
- index
- data node
- cluster manager node
- OpenSearch Dashboards

### Key controls
- VPC access
- encryption
- fine-grained access control
- index lifecycle/state management

### Example
Centralized application logs are indexed into OpenSearch with separate index patterns by environment.

### Best practices
- design shard counts carefully
- secure with VPC access and auth controls
- monitor cluster state, storage, and indexing pressure
- use index lifecycle/state policies

### Common mistakes
- oversharding
- treating OpenSearch as a generic database
- weak access control on analytics clusters

---

## 11.2 Amazon Redshift

### Technical purpose
Managed data warehouse for large-scale analytical SQL workloads.

### Core entities
- provisioned cluster
- serverless namespace
- serverless workgroup
- database
- schema
- table
- materialized view

### Key controls
- workload management (WLM)
- data sharing
- encryption
- snapshot/backup posture
- network and query controls

### Example
Business analytics team uses Redshift to query curated warehouse tables loaded from S3 and ETL pipelines.

### Best practices
- separate ETL, BI, and ad hoc workloads where appropriate
- optimize distribution/sort/query design
- use materialized views for repeated workloads
- tune workgroup/WLM behavior intentionally

### Common mistakes
- treating it like OLTP RDS
- poor table design causing heavy skew and slow queries
- no workload separation

---

## 11.3 Amazon Athena

### Technical purpose
Serverless SQL query service over S3-backed data and catalogs.

### Core entities
- workgroup
- query
- data catalog
- partition
- query result location

### Key controls
- workgroup settings
- bytes scanned limits
- result location enforcement
- catalog and access control

### Example
Finance team queries partitioned Parquet data in S3 through Athena using a governed workgroup with cost limits.

### Best practices
- use columnar formats and partitioning
- enforce result location and workgroup controls
- optimize schema and partition projection where appropriate
- control ad hoc spend with workgroup settings

### Common mistakes
- querying raw CSV at large scale
- ignoring partitioning
- no governance on result locations or scanned bytes

---

# 12. Glue, DataBrew, and Lake Formation

---

## 12.1 AWS Glue

### Technical purpose
Managed ETL, metadata catalog, crawler, and data integration platform.

### Core entities
- Data Catalog database
- Data Catalog table
- crawler
- job
- trigger
- workflow
- connection
- schema
- classifier
- data quality ruleset

### Key controls
- job bookmarks
- worker type
- security configuration
- IAM roles
- catalog permissions

### Example
A crawler discovers raw S3 data, ETL job converts it to Parquet, and catalog tables are updated for Athena and Redshift consumption.

### Best practices
- separate raw, transformed, and curated data contracts
- use bookmarks for incremental jobs when appropriate
- control IAM and data access tightly
- validate schemas instead of blindly trusting crawler output
- use data quality rules where data trust matters

### Common mistakes
- overreliance on crawlers without schema governance
- ETL jobs with no partition strategy
- one giant monolithic Glue job for unrelated pipelines

---

## 12.2 AWS Glue DataBrew

### Technical purpose
Visual/no-code data preparation service.

### Core entities
- project
- dataset
- recipe
- profile job

### Best fit
Data preparation and profiling for analysts or low-code transformation workflows.

### Common mistakes
- using DataBrew for complex engineering-grade ETL platforms where Glue jobs are the right layer

---

## 12.3 AWS Lake Formation

### Technical purpose
Govern data lake permissions and sharing over data stored in S3 and cataloged data assets.

### Core entities
- data lake administrator
- registered location
- LF-Tag
- permissions
- data filter
- resource link
- blueprint

### Key controls
- LF-tag-based access control
- row-level filters
- column-level filters
- cross-account sharing

### Example
A central lake team grants analysts access to only selected columns from a curated customer table using Lake Formation permissions and filters.

### Best practices
- centralize lake governance
- use LF-tags for scalable access policy
- distinguish storage access from data governance access
- test cross-account sharing carefully

### Common mistakes
- mixing S3-only permissions with governed lake permissions without clarity
- no metadata ownership model
- weak column/row filtering design

---

# 13. Streaming, Messaging, and Event Services

---

## 13.1 Amazon Kinesis Data Streams

### Technical purpose
Real-time ordered event ingestion and stream processing backbone.

### Core entities
- stream
- shard
- consumer
- enhanced fan-out consumer

### Key controls
- retention
- shard count
- encryption
- consumer design

### Example
Application telemetry events are pushed to a stream and consumed by multiple independent analytics services.

### Best practices
- size shard count to throughput and consumer needs
- monitor consumer lag
- use enhanced fan-out where dedicated throughput matters
- plan retention deliberately

### Common mistakes
- undersharding
- no consumer checkpoint strategy
- confusing Kinesis with queue semantics

---

## 13.2 Amazon Data Firehose

### Technical purpose
Managed delivery stream service for moving streaming data to destinations like S3, OpenSearch, and others.

### Core entities
- delivery stream
- source
- destination

### Key controls
- buffering
- transformation
- destination configuration
- encryption

### Best fit
When the main goal is delivery, not custom stream-processing consumers.

### Common mistakes
- using Firehose when custom multi-consumer streaming is required

---

## 13.3 Amazon MSK

### Technical purpose
Managed Apache Kafka service.

### Core entities
- cluster
- broker
- topic
- partition
- consumer group
- connector

### Key controls
- broker storage
- encryption
- ACLs
- monitoring
- topic and retention design

### Example
An enterprise event platform standardizes on Kafka-compatible patterns and uses MSK for managed broker infrastructure.

### Best practices
- separate cluster operations from topic governance
- monitor lag and broker health
- design partition counts for throughput and ordering tradeoffs
- secure with network and auth controls

### Common mistakes
- choosing Kafka when simpler SNS/SQS/EventBridge patterns are enough
- no partition strategy
- poor consumer group scaling assumptions

---

## 13.4 Amazon SQS

### Technical purpose
Managed queue service for decoupling and buffering asynchronous work.

### Core entities
- standard queue
- FIFO queue
- dead-letter queue
- redrive policy
- visibility timeout

### Example
An order-processing workflow pushes asynchronous work to SQS so downstream workers can scale independently.

### Best practices
- choose standard vs FIFO intentionally
- size visibility timeout to processing behavior
- use DLQs and redrive policies
- design idempotent consumers

### Common mistakes
- treating SQS as publish-subscribe
- wrong visibility timeout causing duplicate work or stuck retries
- no DLQ for failure investigation

---

## 13.5 Amazon SNS

### Technical purpose
Managed pub/sub notification and fanout service.

### Core entities
- topic
- subscription
- filter policy

### Example
A single order event fans out to billing, notifications, and analytics subscribers.

### Best practices
- use filter policies to reduce unnecessary downstream processing
- secure topic policies
- design retry and delivery behavior per endpoint type

### Common mistakes
- using SNS when durable consumer-managed queueing semantics are required but not added
- no subscription filtering, creating noisy fanout

---

## 13.6 Amazon EventBridge

### Technical purpose
Event routing bus for application, AWS service, SaaS, and scheduled events.

### Core entities
- event bus
- rule
- target
- event pattern
- archive
- replay
- schedule
- connection
- API destination

### Key controls
- event bus policies
- retry policies
- DLQs
- schema handling
- input transformers

### Example
Security events are routed by EventBridge rules to remediation workflows, notifications, and audit pipelines.

### Best practices
- keep event patterns precise
- use archives/replays for operational recovery where needed
- separate custom buses by domain when useful
- secure bus policies carefully

### Common mistakes
- overly broad event patterns
- no DLQ or replay strategy for critical eventing
- mixing unrelated domains into one noisy bus

---

## 13.7 AWS Step Functions

### Technical purpose
Workflow orchestration service for stateful application flow, retries, branching, and long-running business processes.

### Core entities
- state machine
- state
- execution
- activity
- task
- version
- alias
- distributed map
- execution history

### Key controls
- retry/catch handlers
- timeout settings
- IAM roles
- logging
- tracing
- encryption

### Example
An order workflow:
1. validate payment
2. reserve inventory
3. send shipment request
4. compensate on failure using catch/rollback branches

### Best practices
- design retries intentionally per task type
- use timeouts everywhere
- model compensating actions explicitly
- version and alias important workflows

### Common mistakes
- putting all business logic inside one Lambda instead of orchestrating steps
- no catch/timeout handling
- no execution history review for failures

---

# 14. Secrets Manager and KMS

---

## 14.1 AWS Secrets Manager

### Technical purpose
Store, rotate, and control access to application secrets.

### Core entities
- secret
- secret version
- version stage
- rotation schedule
- rotation Lambda
- resource policy
- replication setting

### Key controls
- automatic rotation
- version staging labels
- deletion waiting period
- resource policy
- encryption via KMS
- multi-Region replication where required

### Example
A database credential secret uses rotation Lambda and version stages such as current/pending during rotation flow.

### Best practices
- rotate where supported and justified
- use app/runtime identity to fetch secrets dynamically
- avoid baking secrets into images or user data
- control cross-account access carefully

### Common mistakes
- storing static credentials in parameter files or environment text instead of managed secrets
- deleting secrets without understanding recovery window
- overbroad access to secret values

---

## 14.2 AWS KMS

### Technical purpose
Managed key management for encryption operations and cryptographic control.

### Core entities
- KMS key
- alias
- key policy
- grant
- key material
- multi-Region key
- imported key material
- external key store

### Key controls
- key policy
- grants
- rotation
- cross-account access design
- Region strategy
- imported key material or external key store where required by policy

### Example
A centralized security account owns CMKs used by S3, RDS, EBS, and Secrets Manager with controlled cross-account permissions.

### Best practices
- separate admin from usage permissions
- prefer aliases and clear naming
- review grants regularly
- understand service-integrated encryption behavior
- test cross-account access explicitly

### Common mistakes
- weak or overly permissive key policies
- assuming IAM alone can override bad key policy design
- accidental encryption lockout through mismanaged key permissions

---

# 15. AWS Backup

AWS Backup centralizes backup policy and recovery management across supported AWS services.

## 15.1 Core entities
- backup plan
- backup rule
- backup selection
- recovery point
- backup vault
- backup vault lock
- copy job
- restore job
- legal hold where supported

## 15.2 Technical purpose
Provide policy-based backup orchestration and centralized recovery tracking.

## 15.3 Key controls
- schedules
- lifecycle rules
- vault policies
- vault lock
- cross-Region copy
- cross-account copy
- retention
- legal hold

## 15.4 Example
An enterprise backup plan selects:
- production RDS
- EFS
- DynamoDB
with daily backups, 35-day retention, and cross-account copy to a recovery account.

## 15.5 Best practices
- centralize backup governance
- separate operational account from recovery/archive account where possible
- test restore jobs regularly
- use Vault Lock where immutability is required
- distinguish backup from replication and snapshot-only assumptions

## 15.6 Common mistakes
- backup plan exists but restore never tested
- no immutability for compliance workloads
- assuming service-native replication equals backup compliance

---

# 16. AWS Database Migration Service (DMS)

DMS supports database and selected data-source migration using full load and change data capture patterns.

## 16.1 Core entities
- replication instance
- replication task
- endpoint
- replication subnet group
- event subscription
- data provider
- migration project

## 16.2 Technical purpose
Migrate or replicate data between supported sources and targets with minimal downtime strategies.

## 16.3 Common migration modes
- full load
- CDC
- full load + CDC

## 16.4 Example
A legacy on-prem database is migrated to Aurora:
1. full load
2. CDC catches ongoing changes
3. validation checks run
4. cutover happens after lag approaches zero

## 16.5 Best practices
- validate endpoint permissions and connectivity early
- test table mappings carefully
- measure CDC lag and cutover readiness
- separate migration dry-runs from production cutover execution
- use data validation where appropriate

## 16.6 Common mistakes
- treating DMS as zero-thinking “copy data” tooling
- ignoring datatype/LOB/schema conversion details
- no cutover rehearsal
- under-sizing replication infrastructure

---

# 17. Service Selection Guide

## 17.1 S3 vs EBS vs EFS vs FSx
- **S3**: object storage, massive scale, decoupled access
- **EBS**: block storage for EC2
- **EFS**: shared POSIX file storage
- **FSx**: managed specialized file systems

## 17.2 RDS/Aurora vs DynamoDB
- **RDS/Aurora**: relational consistency, joins, SQL OLTP
- **DynamoDB**: low-latency key-value/document, scale-first design

## 17.3 ElastiCache vs MemoryDB
- **ElastiCache**: caching and fast transient acceleration
- **MemoryDB**: durable Redis-compatible database use cases

## 17.4 OpenSearch vs Redshift vs Athena
- **OpenSearch**: indexed search/log analytics
- **Redshift**: warehouse analytics
- **Athena**: serverless SQL over data lake

## 17.5 SQS vs SNS vs EventBridge vs Kinesis vs MSK
- **SQS**: queue and buffering
- **SNS**: fanout pub/sub
- **EventBridge**: event routing bus
- **Kinesis**: ordered stream ingestion
- **MSK**: Kafka-compatible streaming platform

## 17.6 Backup vs replication
- **Backup**: restore from historical recovery point
- **Replication**: copy data/location for resilience or locality
You often need both.

---

# 18. End-to-End Data Platform Example

A modern enterprise data platform could use:

### Storage
- S3 for raw and curated lake zones
- EFS for shared application file needs
- EBS for EC2-hosted transactional systems

### Databases
- Aurora for core transactional apps
- DynamoDB for low-latency session and metadata workloads
- ElastiCache for cache acceleration

### Analytics
- Glue crawlers and ETL jobs
- Lake Formation for governed lake permissions
- Athena for ad hoc SQL
- Redshift for BI warehouse
- OpenSearch for indexed operational search

### Streaming and events
- Kinesis for telemetry stream ingestion
- Firehose for S3 delivery
- SQS for decoupled workers
- SNS for fanout
- EventBridge for app/domain events
- Step Functions for orchestration

### Protection
- Secrets Manager for credentials
- KMS for encryption control
- AWS Backup for centralized backup policy
- DMS for migration or replication cutover

This is the Week 4 principle:
> stateful systems are reliable only when storage, database, eventing, and recovery controls are designed together.

---

# 19. Troubleshooting Framework for Week 4

## 19.1 Access problems
- IAM?
- resource policy?
- KMS key policy?
- network path?
- engine auth issue?

## 19.2 Data durability/recovery problems
- backup exists?
- restore tested?
- replication configured correctly?
- retention window still valid?
- Object Lock or vault policy blocking intended action?

## 19.3 Performance problems
- wrong storage class/type?
- bad key design?
- hot partitions?
- query plan/index issue?
- shard count too low?
- cache miss ratio poor?

## 19.4 Governance problems
- catalog/table mismatch?
- Lake Formation permission issue?
- secret rotation breakage?
- key grants incorrect?
- workgroup or result location enforcement?

## 19.5 Workflow problems
- DLQ filling?
- Step Functions retries missing?
- EventBridge pattern wrong?
- consumer lag high?
- DMS CDC lag too high?

---

# 20. Common Week 4 Mistakes

- treating replication as backup
- no restore testing
- no deletion protection on critical databases
- poor DynamoDB key design leading to hot partitions
- overusing S3 without bucket governance
- using caches as durable stores
- querying unpartitioned raw files with Athena
- no data catalog ownership model
- weak Lake Formation governance
- event services chosen by habit instead of semantics
- broad KMS key permissions
- secret rotation enabled without application rotation readiness
- migration cutover attempted without rehearsal

---

# 21. Week 4 Revision Checklist

You should be able to explain clearly:

- how S3 versioning, lifecycle, replication, Object Lock, and storage classes differ
- when to use EBS vs EFS vs FSx
- how RDS Multi-AZ differs from read replicas
- how Aurora endpoints, replicas, global databases, and backtrack windows work
- how DynamoDB partition keys, GSIs, streams, PITR, and global tables affect design
- when DocumentDB, Neptune, Timestream, ElastiCache, or MemoryDB are a better fit
- how OpenSearch, Redshift, and Athena solve different analytics problems
- how Glue, DataBrew, and Lake Formation fit together
- how Kinesis, Firehose, MSK, SQS, SNS, EventBridge, and Step Functions differ
- how Secrets Manager and KMS work together
- how AWS Backup differs from replication and snapshots
- how DMS supports full-load and CDC cutover patterns

---

# 22. Memory Anchors

- **S3** = object storage
- **EBS** = EC2 block storage
- **EFS** = shared POSIX file storage
- **FSx** = specialized managed file systems
- **RDS** = managed relational DB instance service
- **Aurora** = cluster-based cloud-native relational DB
- **DynamoDB** = key-value/document NoSQL at scale
- **DocumentDB** = managed document DB
- **Neptune** = graph DB
- **Timestream** = time-series DB
- **ElastiCache** = cache layer
- **MemoryDB** = durable Redis-compatible DB
- **OpenSearch** = indexed search/analytics
- **Redshift** = data warehouse
- **Athena** = serverless SQL on S3
- **Glue** = catalog + ETL
- **DataBrew** = visual data prep
- **Lake Formation** = governed data lake permissions
- **Kinesis** = ordered event stream
- **Firehose** = managed stream delivery
- **MSK** = managed Kafka
- **SQS** = queue
- **SNS** = pub/sub fanout
- **EventBridge** = event routing bus
- **Step Functions** = workflow orchestration
- **Secrets Manager** = secret storage and rotation
- **KMS** = encryption key control
- **AWS Backup** = centralized backup governance
- **DMS** = migration and CDC

---

# 23. Official AWS Documentation References

## Storage
- Amazon S3 Documentation  
  https://docs.aws.amazon.com/s3/

- Amazon EBS Documentation  
  https://docs.aws.amazon.com/ebs/

- Amazon EFS Documentation  
  https://docs.aws.amazon.com/efs/

- Amazon FSx Documentation  
  https://docs.aws.amazon.com/fsx/

## Databases
- Amazon RDS Documentation  
  https://docs.aws.amazon.com/rds/

- Amazon Aurora Documentation  
  https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/

- Amazon DynamoDB Documentation  
  https://docs.aws.amazon.com/dynamodb/

- Amazon DocumentDB Documentation  
  https://docs.aws.amazon.com/documentdb/

- Amazon Neptune Documentation  
  https://docs.aws.amazon.com/neptune/

- Amazon Timestream Documentation  
  https://docs.aws.amazon.com/timestream/

- Amazon ElastiCache Documentation  
  https://docs.aws.amazon.com/elasticache/

- Amazon MemoryDB Documentation  
  https://docs.aws.amazon.com/memorydb/

## Analytics
- Amazon OpenSearch Service Documentation  
  https://docs.aws.amazon.com/opensearch-service/

- Amazon Redshift Documentation  
  https://docs.aws.amazon.com/redshift/

- Amazon Athena Documentation  
  https://docs.aws.amazon.com/athena/

## Data lake and ETL
- AWS Glue Documentation  
  https://docs.aws.amazon.com/glue/

- AWS Glue DataBrew Documentation  
  https://docs.aws.amazon.com/databrew/

- AWS Lake Formation Documentation  
  https://docs.aws.amazon.com/lake-formation/

## Streaming, messaging, and orchestration
- Amazon Kinesis Documentation  
  https://docs.aws.amazon.com/kinesis/

- Amazon Data Firehose Documentation  
  https://docs.aws.amazon.com/firehose/

- Amazon MSK Documentation  
  https://docs.aws.amazon.com/msk/

- Amazon SQS Documentation  
  https://docs.aws.amazon.com/sqs/

- Amazon SNS Documentation  
  https://docs.aws.amazon.com/sns/

- Amazon EventBridge Documentation  
  https://docs.aws.amazon.com/eventbridge/

- AWS Step Functions Documentation  
  https://docs.aws.amazon.com/step-functions/

## Protection and migration
- AWS Secrets Manager Documentation  
  https://docs.aws.amazon.com/secretsmanager/

- AWS KMS Documentation  
  https://docs.aws.amazon.com/kms/

- AWS Backup Documentation  
  https://docs.aws.amazon.com/aws-backup/

- AWS DMS Documentation  
  https://docs.aws.amazon.com/dms/

---

# 24. Final Summary

Week 4 is about **where data lives, how it is protected, and how it recovers**.

To master Week 4, understand both:
- each service individually
- how they combine into a reliable data architecture

The strongest mental model is:

- **S3, EBS, EFS, FSx** define storage choices
- **RDS, Aurora, DynamoDB, and specialized databases** define transactional and domain-specific state models
- **OpenSearch, Redshift, Athena** define query and analytics patterns
- **Glue, DataBrew, Lake Formation** define lake metadata, ETL, and governance
- **Kinesis, Firehose, MSK, SQS, SNS, EventBridge, Step Functions** define data movement and workflow
- **Secrets Manager, KMS, AWS Backup, DMS** define protection, encryption, recovery, and migration

When you can explain backup, replication, retention, encryption, failover, and restore for each stateful layer, Week 4 is understood architecturally.
