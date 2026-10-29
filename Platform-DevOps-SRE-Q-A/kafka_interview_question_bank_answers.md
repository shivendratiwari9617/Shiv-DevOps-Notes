# Senior Kafka Platform, DevOps, and SRE Interview Answers

## Architecture and guarantees

### 1. Explain how brokers, partitions, leaders, replicas, and the controller cooperate to serve reads and writes. What happens when partition leadership changes?

Brokers store partition logs; one broker leads each partition, followers replicate, and producers and consumers use metadata to locate the leader. The cluster controller manages broker metadata and leader elections. When a leader fails, an eligible replica takes over; clients refresh metadata and retry, potentially seeing a short interruption. Verify ISR, leader distribution, and client errors after failover. A successful election does not prove acknowledged events remain available if durability settings or replica state were unsafe.

### 2. How do replication factor, in-sync replicas, `min.insync.replicas`, and producer acknowledgments affect durability and write availability?

Replication factor sets the replica count; ISR contains replicas sufficiently caught up for leader acknowledgment. With `acks=all`, the leader requires acknowledgment under its ISR and `min.insync.replicas` policy, so writes may fail when ISR falls below the minimum. `acks=1` can keep accepting writes but risks losing acknowledged records on leader loss. Set durability from the product's data-loss tolerance, monitor ISR shrinkage, and test broker failure before changing settings under incident pressure.

### 3. What failure modes arise from unclean leader election, and when might availability and durability requirements conflict?

Unclean election can promote an out-of-sync replica when no in-sync leader is available, restoring reads and writes at the cost of truncating records acknowledged on the old leader. For payment and order events, that loss is usually worse than temporary unavailability. Confirm replica state, application recovery options, and business RPO before considering the trade-off; do not toggle unclean election to make an alert disappear. Test the chosen policy with a partition failure exercise and customer-level reconciliation.

### 4. How would you select partition count and record keys for a topic with uneven tenant traffic, ordering requirements, and future growth?

Key records by the entity requiring order, such as customer or order ID, so that entity's events map to one partition. Size partition count using required throughput, consumer parallelism, broker overhead, and hot-tenant skew, with headroom for growth. Adding partitions changes key-to-partition mapping for subsequent events, which may break historical per-key order during transition. Measure per-partition traffic and design explicit migration or routing if hot tenants need isolation. Do not equate adding consumers with fixing a single hot partition.

### 5. What ordering guarantees does Kafka provide, and how can producer retries or changes to partitioning affect application assumptions?

Kafka orders records within one partition, not across a topic or all services. Keying related events consistently usually keeps them together, but adding partitions or changing partitioners can send future events for the same key elsewhere. Producer retries without appropriate idempotence and in-flight settings can add duplicates or reorder attempts. Test a customer event sequence through failover, retry, and partition changes. Consumers should enforce domain sequencing when a cross-partition or external ordering contract is required.

### 6. What do idempotent producers and Kafka transactions protect against, and what additional work is required for end-to-end processing guarantees?

An idempotent producer prevents duplicate writes caused by retries in its producer session. Transactions can atomically write to Kafka topics and commit consumed offsets, with `read_committed` consumers hiding aborted records. Neither protects an external payment, database, or email side effect by itself. Use a domain idempotency key, durable inbox/outbox, or transactional integration and reconcile ambiguous results. Test crash-after-external-effect-before-offset-commit to prove business correctness.

## Producers, consumers, and topic design

### 7. A producer times out after a deployment while broker CPU appears normal. How would you investigate acknowledgments, metadata, network, quotas, broker health, and retries?

Correlate request timeouts with deployment config, broker leader/ISR state, client metadata refresh, network RTT/TLS, quotas, and produce-request latency. Check `acks`, delivery timeout, retry volume, and whether a single hot partition or unreachable advertised listener is involved. Broker-wide CPU can look normal while one partition is unavailable. Revert a bad client config or route only after preserving duplicate-handling semantics. Verify accepted records and downstream customer actions, not just producer success metrics.

### 8. How do batching, compression, linger, and in-flight requests influence throughput, latency, and ordering? How would you validate a tuning change?

Larger batches and linger improve compression and throughput but add queuing latency; compression saves network and disk at CPU cost. More in-flight requests can raise throughput but require correct idempotence and retry settings to preserve intended ordering. Benchmark p99 publish-to-consume latency, throughput, broker pressure, and error/retry behavior with representative event sizes and partition skew. Canary the change under production-like load. Validate customer order timeliness rather than optimizing bytes per second alone.

### 9. How do offset commit timing and consumer processing behavior affect duplicates, data loss, and recovery after a crash?

Commit before processing risks skipping an event if the consumer crashes; commit after processing can replay the event when a crash occurs between side effect and offset commit. Kafka's offset represents progress, not proof the business action happened exactly once. Make the side effect idempotent using an event ID and durable result, or atomically couple processing and offset where both are in Kafka. Test crashes at each boundary and reconcile external effects on restart.

### 10. A consumer group repeatedly rebalances after an application release. How would you distinguish processing delays, client failures, coordinator problems, and configuration changes?

Correlate rebalance count and duration with client logs, member restarts, poll intervals, session timeouts, coordinator errors, and rollout timing. Long processing can breach `max.poll.interval.ms`; a crash or network fault can cause missed heartbeats. Compare assignment strategy, static membership, and client version with the previous release. Fix the true cause before increasing timeouts, then test throughput and lag under a rolling deployment. Rebalances are a symptom, not automatically a broker failure.

### 11. Lag increases for only a few partitions while the rest remain healthy. How would you investigate hot keys, consumer assignment, broker leadership, and downstream behavior?

Compare incoming bytes, record counts, processing time, and per-key distribution for affected partitions against healthy ones. Check whether consumers are assigned and polling, whether the leader or replica is under pressure, and whether a downstream dependency is slow for certain tenants. More consumers cannot parallelize one partition within a group. Preserve per-key ordering when isolating hot customers, and measure event age and customer-visible delays after mitigation.

### 12. How would you handle poison messages and replay after a consumer bug without silently losing records or blocking unrelated work?

Capture the failing event ID, schema, error and offset; stop unsafe retries that repeatedly trigger a non-idempotent side effect. Route to a governed dead-letter path only with owner, reason, retention, and replay plan; do not treat skipping as successful processing. Patch and test the consumer on representative events, replay with idempotent operations, and reconcile counts against original source. Maintain ordering for entities where later events depend on the poison record. Expose backlog age and business impact.

### 13. How do retention, log compaction, and tombstones affect replay and storage? What assumptions should application teams validate?

Time or size retention can delete older records, limiting replay regardless of current consumer lag. Compaction retains the latest record per key on a best-effort schedule rather than a full event history; tombstones represent deletion and are eventually eligible for removal. Validate bootstrap, audit, and recovery needs against actual retention and compaction settings. Monitor disk use and replay tests under a broker failure. Do not promise indefinite replay from a compacted topic.

### 14. A topic needs higher throughput, but its consumers require per-customer ordering. What risks arise from increasing partitions, and how would you plan a safe change?

New partition count changes many key-to-partition assignments, so older and newer records for a customer can be consumed concurrently and out of order. Quantify whether the bottleneck is a hot customer, broker, or consumers before altering partitions. Consider a versioned new topic and controlled producer cutover with a drain or sequence gate, or isolate high-volume keys deliberately. Test per-customer order through the migration. Keep a rollback plan that does not mix conflicting partition schemes.

## Operations, reliability, and security

### 15. How would you size brokers and storage for throughput, retention, replication, failure headroom, and recovery rather than only steady-state averages?

Model peak ingress and egress by partition, replica traffic, retention bytes, compaction overhead, and disk I/O. Reserve capacity so remaining brokers absorb a failed node and replication catch-up without exhausting network or disks. Include consumer replay spikes, upgrades, and growth projections. Validate with load and broker-loss tests while tracking produce latency and partition age. Cost optimization is bounded by the product's acceptable data loss and recovery time.

### 16. A broker is lost and under-replicated partitions continue growing. How would you prioritize recovery, identify bottlenecks, and avoid making availability worse?

Identify offline leaders, ISR shrinkage, `acks=all` errors, and customer-critical partitions first. Check surviving broker disk, network, replication fetch rates, leader skew, and whether the lost broker can safely rejoin. Limit reassignment or restart concurrency to avoid saturating healthy nodes. Maintain durability policy unless product owners accept explicit data-loss risk. Verify ISR restoration, accepted write consistency, and consumer event age before ending incident response.

### 17. How would you plan a Kafka upgrade across brokers, controllers, and clients while checking compatibility, rollback constraints, and service health?

Check supported version hops, inter-broker protocol and metadata version, client compatibility, and KRaft controller quorum before starting. Stage upgrades, back up configurations, roll brokers and controllers only in supported order, and monitor ISR, election rate, error rate and lag after each step. Delay irreversible metadata changes until rollback is no longer needed. Rehearse a node failure during the change. Verify customer event publication and consumption, not just process health.

### 18. What signals would distinguish controller or metadata-plane degradation from an isolated broker, client, or network failure?

Metadata request errors, election churn, controller quorum health, and widespread leader changes suggest control-plane trouble. A few affected partitions on one broker suggest local storage or broker issues; one client version or network zone points to application or connectivity. Correlate timestamps, affected topics and clients, and independent broker health. Avoid mass restarts when controller availability is unclear. Validate leadership stability and customer produce/consume operations after recovery.

### 19. How would you design rack or availability-zone placement, leadership distribution, and maintenance procedures to limit correlated failure?

Spread replicas across independent zones using correct broker rack metadata and verify actual placement and leader balance after changes. Keep enough live in-sync replicas for `acks=all` under a planned outage and reserve recovery bandwidth. Maintain one broker or zone at a time, checking ISR and client errors between steps; a zone failure must not coincide with unsafe maintenance. Test a zone-loss scenario and compare resulting data loss, write availability, and customer delay against SLOs.

### 20. How would you secure a multi-team Kafka platform with authentication, authorization, encryption, quotas, and auditable access changes?

Use strong client identity, TLS, topic/group ACLs scoped to service ownership, and separate admin rights from producers and consumers. Apply per-team throughput and connection quotas so one product cannot exhaust shared brokers. Automate grant review and log principal, topic, and change actor without exposing event payloads. Validate unprivileged clients cannot read other teams' topics or bypass schema and retention policy. Rotate credentials and test failure behavior before a production cutover.

### 21. A disk is filling rapidly on one broker. How would you determine whether retention, partition skew, replication, or an unexpected producer is responsible?

Compare per-log-dir and per-partition growth, ingress bytes, retention settings, compaction backlog, leader/follower placement, and replication catch-up since the last baseline. Check deleted-but-open files and disk errors separately. Avoid deleting topic data or moving many replicas while ISR is degraded. Contain an unexpected producer or rebalance only with headroom on destinations. Verify disk trend, topic retention guarantees, and customer event integrity afterward.

### 22. What evidence would you require before concluding that consumer lag represents customer impact rather than normal processing or a misleading metric?

Measure age of the oldest unprocessed customer-critical event and end-to-end business completion, not just offset distance. Compare incoming rates, paused consumers, assignment changes, retention, and whether reported lag comes from committed versus processed offsets. A large lag may be a planned backfill; a small lag can still represent hours on a quiet partition. Alert using a customer-specific freshness SLI and reconcile completed events. Confirm metric and timestamp accuracy before paging.

## Ecosystem and incidents

### 23. A Kafka Connect pipeline begins producing duplicates after a restart. How would you investigate connector configuration, source offsets, sink behavior, and idempotency?

Inspect connector/task logs and source offsets before and after restart, snapshot mode, retry settings, and whether sink acknowledgments happened before offsets were persisted. Distinguish repeated source reads from duplicated sink writes using stable event IDs. Contain the sink if duplicate business operations are harmful, reconcile existing records, and add idempotent upsert or deduplication. Reproduce crash-after-sink-before-offset to prove safe replay. A running connector does not guarantee exactly-once external delivery.

### 24. How would you govern schema compatibility and rollout when producers and consumers deploy independently?

Define the compatibility mode per topic and versioned schema contract, then test new events against all still-supported consumers. For a required field, first deploy consumers that tolerate missing and new data, then producers that populate it, then tighten constraints after old consumers retire. Use registry validation plus consumer contract tests, not registry checks alone. Keep a rollback path that handles events already emitted in the new shape. Monitor deserialization failures and quarantined events during rollout.

### 25. How would you assess cross-cluster replication for disaster recovery, including lag, offset translation, failover, data loss, and failback?

First inventory replicated topics, schemas, ACLs, producer configuration and consumer-group checkpoints; a mirror of topic data alone is not a usable recovery site. Seed identifiable committed records just before a drill, compare their presence on the target with source commit positions and timestamps, and test the replication system's offset translation for each consumer group. Stop or fence old producers before directing writes to the target, then start consumers at verified translated positions and reconcile external effects using event IDs: translating offsets cannot guarantee exactly-once payments. Measure event age and customer transaction success under representative load. For failback, treat the target as the new source of truth, validate reverse replication, and test duplicate and per-key order behavior before shifting writers back. Report RTO to a working customer path and RPO from the latest proven durable event, not dashboard lag alone.

### 26. When would Kafka be a poor fit compared with a queue or another streaming system? How do delivery, retention, ordering, and operations influence the choice?

If the workload needs simple task dispatch, per-task acknowledgments and retries, or few consumers with no replay requirement, a queue may be easier to operate. Kafka is valuable for durable ordered partition logs, fan-out and replay at scale but demands partition-key design, retention management, and consumer offset discipline. Compare latency, failure semantics, operational staffing, and the cost of keeping a cluster healthy. A product does not gain business exactly-once semantics by choosing Kafka alone.

### 27. Producer timeouts and consumer lag rise together during a network incident. What would you check first, what mitigation would you avoid until evidence is available, and how would you verify recovery?

Map affected zones and client-to-broker paths, leader and ISR changes, broker request latency, and client metadata/retry errors. Preserve durability settings and avoid disabling acknowledgments or unclean elections merely to restore traffic. Mitigate the broken network path or isolate an affected zone with a documented single-writer and data-loss decision. Verify acknowledged records, end-to-end event age, and customer actions after recovery; lag may remain temporarily while backlogs drain.

### 28. Tell me about a time application teams and platform teams disagreed on event ownership or delivery guarantees. How did you reach and validate an operational contract?

Use a real disagreement and state the desired business outcome, producer and consumer responsibilities, retention, replay, idempotency, and incident ownership. Explain how you tested a failure boundary such as duplicate payment or lost acknowledgment, which trade-off was accepted, and what metric proved it. Name your personal contribution and what changed after feedback. Do not claim a fictional incident as personal experience. A useful contract specifies recovery behavior, not only `acks=all`.

## Evidence-based exercises

### 29. A topic has replication factor 3 and `min.insync.replicas=2`; a producer uses `acks=all`. If a partition's ISR shrinks to one replica, what write behavior would you expect, what assumptions matter, and what would you verify before changing the configuration?

With one in-sync replica, the leader cannot satisfy minimum ISR of two and should reject `acks=all` writes with insufficient-replica errors rather than acknowledge them. Check topic-level overrides, actual ISR, leader health, producer retry behavior, and whether writes truly target that partition. Lowering the minimum can restore availability but weakens durability if that replica fails. Restore replica health first where possible and get an explicit product data-loss decision before changing policy.

### 30. A consumer reads and processes a record, performs an external payment, and crashes before committing its offset. What can happen on restart, and what application-level evidence and design would you examine to prevent an incorrect repeated payment?

The record can be replayed because the committed offset is behind the completed payment. Check a stable event or payment idempotency key, payment provider receipt, and durable local result before retrying. Make the external charge idempotent and reconcile ambiguous timeouts; commit only after a durable business outcome is recorded. Test a crash immediately after charge and before offset commit. Kafka producer idempotence and transactions do not by themselves protect the external payment.

### 31. A topic has 12 partitions, but only one shows rapidly growing lag while its consumer remains assigned. How would you compare key distribution, processing time, downstream calls, and partition leadership before adding consumers?

Compare that partition's ingress bytes and event age with other partitions, then sample key distribution and per-record processing and downstream latency. Check its leader broker and client poll errors for storage or network skew. A consumer group can run only one assigned consumer per partition, so extra consumers cannot divide this backlog. Fix a hot customer or slow dependency without changing key routing midstream. Confirm event-order integrity and customer delay after mitigation.

### 32. A broker's disk utilization rises from 60% to 90% in one hour while total incoming bytes are unchanged. What retention, replication, partition-placement, and disk evidence would you collect before deleting data or moving replicas?

Inspect broker log-dir usage by partition, recent replica reassignments and catch-up, leader/follower imbalance, retention policy changes, and compaction or deletion backlog. Check disk health and deleted-but-open files; cluster-wide ingress can remain flat while new replicas concentrate data on one broker. Estimate time to full disk and destination headroom before moving replicas. Preserve the affected topics' retention and recovery obligations. Verify space trend and ISR after safe mitigation.

## Product-company practical exercises

### 33. Order events for a single customer must be processed in order, but their partition's lag grows while other partitions remain stable. How would you investigate and mitigate the delay without silently breaking the ordering contract?

Trace that customer's key, partition assignment, incoming rate, oldest-event age, processing duration, and dependency calls; check for a poison event blocking progress. Scale a downstream bottleneck or isolate work by controlled key migration only with a sequence gate. Adding consumer instances does not split a partition, and changing partition count can reorder future records. Reconcile completed orders and measure customer delay after replay. Communicate expected recovery time and preserve every unprocessed event.

### 34. A producer release adds a required field to order events, while two consumer versions remain deployed. What compatibility evidence would you request before rollout, and how would you stage and reverse the change if older consumers fail?

Check registry compatibility policy and test actual old consumer deserialization and business behavior against both old and proposed records. Roll out tolerant consumers first, then producers that populate the field; mark it required only when all readers can handle it and historical records have a policy. Canary producer traffic and monitor deserialization and dead-letter counts. If old consumers fail, halt new production and deploy compatibility handling for already-written records; simply reverting code cannot erase emitted events.

### 35. A product team wants indefinite retention for every topic, but the platform has a fixed storage budget. How would you choose retention and replay guarantees, model capacity under failure, and negotiate an exception?

Inventory consumer replay horizons, audit obligations, topic volume, replication factor, compaction suitability, and the age of oldest unprocessed records. Model peak ingress and a broker failure plus reassignment headroom, not only average retained bytes. Offer tiered retention or archived immutable data with tested restore where product needs exceed broker budget. Document an exception with owner, cost, expiry, and recovery test. Make it clear that compacted or expired logs cannot provide full event-history replay.

### 36. A consumer deployment causes a payment-event backlog, and retries may repeat an external ledger operation. How would you contain impact, establish which records were processed, preserve ordering, prevent duplicate business effects, and verify safe replay?

Pause the faulty consumer and disable blind retries to the external ledger; preserve topic data, offsets, error logs, and ledger receipts. Reconcile each event ID with durable ledger result, separating committed offsets from actually completed business operations. Fix the consumer and use ledger idempotency keys plus ordered replay per payment entity. Canary a bounded backlog and compare expected versus completed payments before scaling. Declare recovery when event age and customer balances agree, not merely when lag reaches zero.