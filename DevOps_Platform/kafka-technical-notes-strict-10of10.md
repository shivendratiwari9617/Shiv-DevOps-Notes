# Kafka Technical Notes

## 1. Core Mental Model

Apache Kafka is a distributed event streaming platform built around **durable, ordered, replayable logs**.

A strong working model is:

- **broker** = Kafka server that stores partitions, serves reads/writes, and participates in replication
- **topic** = named stream of records
- **partition** = ordered append-only shard of a topic
- **producer** = client that writes records
- **consumer** = client that reads records
- **consumer group** = cooperating consumers sharing work for a topic
- **offset** = record position within a partition
- **replication** = copies of partitions across brokers for durability and failover

Kafka is not best understood as a traditional queue. It is a **distributed commit log** with retention, replay, partitioned parallelism, and explicit client-controlled progress.

That framing matters because most production behavior comes from five ideas:

1. **writes go to partition leaders**
2. **ordering exists only within a partition**
3. **durability depends on replication plus client settings**
4. **consumers control read progress through offsets**
5. **consumer groups scale by partition ownership, not by topic-wide fan-out**

---

## 2. Brokers: What They Actually Do

A Kafka cluster consists of brokers. Each broker is responsible for some combination of:

- storing partition data on disk
- serving producer writes
- serving consumer reads
- leading some partitions
- following other partitions
- replicating data
- participating in metadata and cluster coordination

A broker is not just a passive storage node. In production, broker behavior affects:

- latency
- durability
- failover speed
- read/write throughput
- partition leadership distribution
- cluster stability

### practical implication

When people say “Kafka is slow” or “Kafka lost messages,” the real causes are often broker-level issues such as:

- overloaded disks
- insufficient replication
- poor partition distribution
- weak acknowledgment settings
- leader failover turbulence
- client retry behavior
- unhealthy follower replicas

---

## 3. Topics: Logical Streams, Not Execution Units

A topic is a named stream of records such as:

- `orders.created.v1`
- `payments.captured.v1`
- `inventory.updated.v1`
- `security.audit.v1`

Topics are logical containers. The actual scaling, ordering, and storage behavior happen at the **partition** level.

### production design note

A topic should represent a stable event stream with clear ownership, retention expectations, schema discipline, and access control boundaries.

### anti-pattern

Using one giant generic topic for many unrelated event types often leads to:

- unclear ownership
- weak ACL design
- schema chaos
- hard-to-manage consumer logic
- accidental data exposure

---

## 4. Partitions: Where Ordering and Parallelism Really Come From

A topic is split into one or more partitions. Each partition is an **ordered append-only log**.

### critical rule

Kafka guarantees ordering **within a single partition**, not across all partitions of a topic.

That means:

- event A before event B is guaranteed only if both land in the same partition
- a multi-partition topic improves parallelism, but weakens any assumption of global order

### practical consequence

If order matters for a business entity such as a customer, account, or order, records for that entity should usually use a stable key so they route to the same partition.

```python
event = {
    "key": "customer-123",
    "value": {"order_id": "o-1001", "status": "created"}
}
```

### partition count tradeoff

More partitions can provide:

- more parallel consumer processing
- more write throughput
- more scheduling flexibility

But more partitions also increase:

- broker metadata overhead
- leader/follower management cost
- rebalance time and complexity
- open file and disk activity
- operational tuning burden

### hot partition risk

Bad key distribution can create one overloaded partition while others sit mostly idle. This causes:

- uneven throughput
- lag concentration
- poor scaling
- misleading topic-level health signals

---

## 5. Replication: Durability and Failover Mechanics

Replication is what allows Kafka to survive broker failures without immediately losing data.

For each partition, Kafka maintains:

- **leader replica** = handles client reads and writes
- **follower replicas** = copy data from the leader
- **ISR (in-sync replicas)** = follower replicas caught up enough to be considered safe for durable replication decisions

### replication factor

The replication factor is the number of replicas for a partition.

Example:

- replication factor `1` = one copy only
- replication factor `3` = one leader and two followers

### production guidance

Critical topics should not use replication factor `1` unless the environment is explicitly disposable.

### why ISR matters

Not every replica is always fully caught up. Kafka uses the in-sync replica set to determine which replicas are sufficiently current for durability decisions and future leadership.

### durability is not just “replication exists”

Durability depends on the combination of:

- replication factor
- ISR health
- producer `acks`
- `min.insync.replicas`
- broker failure timing
- client retry behavior

---

## 6. `acks`, `min.insync.replicas`, and Real Durability

These settings are among the most misunderstood parts of Kafka.

## 6.1 Producer `acks`

Common settings:

- `acks=0` = producer does not wait for broker acknowledgment
- `acks=1` = leader acknowledgment only
- `acks=all` = wait for all required in-sync replicas

### practical meaning

- `acks=0` maximizes throughput but offers weak delivery confidence
- `acks=1` is common but can still lose acknowledged writes during certain leader failure windows
- `acks=all` is stronger, especially when paired with proper broker-side replica requirements

## 6.2 `min.insync.replicas`

This broker/topic setting defines the minimum number of in-sync replicas required for a write to succeed when stronger acknowledgment behavior is used.

### important relationship

A high replication factor does **not** guarantee strong durability unless `acks` and `min.insync.replicas` are configured coherently.

### example

If:

- replication factor = 3
- `min.insync.replicas = 2`
- producer uses `acks=all`

then Kafka can reject writes when fewer than two replicas are in sync, protecting durability at the cost of temporary write availability.

### production tradeoff

This is the classic tradeoff:

- stricter durability settings reduce risk of acknowledged-message loss
- looser settings increase availability and throughput but weaken guarantees

---

## 7. Offsets: Consumer Progress, Replay, and Recovery

Each record in a partition has a monotonically increasing offset.

Offsets matter because they enable:

- sequential reads
- progress tracking
- replay
- lag measurement
- recovery after restart

### important clarification

Offsets are per partition, not globally unique across the entire topic.

### committed offset meaning

A committed offset usually represents the **next record to read** after the consumer’s acknowledged processing checkpoint.

### practical consequence

Commit timing determines failure behavior:

- commit too early → application-level data loss risk
- commit too late → duplicate processing risk on restart

---

## 8. Producers: Production Behavior Beyond “send()”

A producer does more than simply push one event at a time.

The producer typically:

1. fetches metadata
2. chooses a partition
3. batches records
4. sends them to the leader broker
5. retries when needed
6. receives acknowledgment according to configured durability settings

## 8.1 Producer Batching and Linger

Producers usually batch multiple records for efficiency.

### tradeoff

Larger batches and a nonzero linger interval can improve throughput, but they may also increase end-to-end latency slightly.

## 8.2 Retries

Retries help survive transient failures, but they interact with ordering and duplication risk.

### production note

Retries should be paired with idempotence for important event streams.

## 8.3 Idempotent Producers

Idempotent production helps Kafka avoid duplicate writes caused by retries from the same producer session.

### value

This is one of the most important safety features for production producers.

## 8.4 `max.in.flight.requests.per.connection`

This setting affects how many unacknowledged requests can be active at once on a connection.

### ordering caveat

Under retry conditions, aggressive in-flight settings can interact with ordering and duplicate safety unless producer idempotence and related settings are designed carefully.

## 8.5 Production-Aware Producer Example

```python
from kafka import KafkaProducer
import json

producer = KafkaProducer(
    bootstrap_servers=["kafka1:9092", "kafka2:9092", "kafka3:9092"],
    acks="all",
    retries=10,
    linger_ms=20,
    batch_size=65536,
    max_in_flight_requests_per_connection=5,
    value_serializer=lambda v: json.dumps(v).encode("utf-8"),
    key_serializer=lambda k: k.encode("utf-8")
)

producer.send(
    "orders.created.v1",
    key="customer-123",
    value={"order_id": "o-1001", "amount": 250.75}
)

producer.flush()
producer.close()
```

### good producer practices

- key records intentionally
- use `acks=all` for important streams
- use retries deliberately
- use idempotence where supported and appropriate
- monitor delivery errors, retries, and latency
- tune batch and linger based on throughput vs latency goals

---

## 9. Consumers: Poll Loop, Processing, and Commit Safety

Consumers do not just “receive messages.” They participate in a coordinated progress model.

## 9.1 Consumer Group Behavior

Consumers in the same group divide partitions among themselves.

### hard rule

Within one consumer group, a partition is actively consumed by only one group member at a time.

### scaling implication

If a topic has 6 partitions, one consumer group can actively consume at most 6 partitions in parallel. Adding a seventh consumer does not increase parallelism for that topic.

## 9.2 Different Groups, Same Topic

Multiple consumer groups can read the same topic independently.

That is how Kafka supports fan-out across applications such as:

- billing
- analytics
- fraud detection
- search indexing
- audit pipelines

Each group tracks its own offsets independently.

## 9.3 Poll Loop Reality

A Kafka consumer repeatedly polls for records, processes them, and decides when offsets are safe to commit.

### operational caveat

A consumer that polls too slowly, processes too long without coordination, or fails heartbeat expectations may trigger rebalances and partition reassignment.

## 9.4 Auto Commit vs Manual Commit

Auto commit is simple but can be unsafe for important workflows.

Manual commit gives tighter control.

### practical rule

Use manual commit when processing must complete successfully before progress is recorded.

## 9.5 Production-Aware Consumer Example

```python
from kafka import KafkaConsumer
import json

consumer = KafkaConsumer(
    "orders.created.v1",
    bootstrap_servers=["kafka1:9092", "kafka2:9092", "kafka3:9092"],
    group_id="billing-service",
    auto_offset_reset="earliest",
    enable_auto_commit=False,
    max_poll_records=100,
    value_deserializer=lambda v: json.loads(v.decode("utf-8")),
    key_deserializer=lambda k: k.decode("utf-8") if k else None
)

for message in consumer:
    order = message.value
    customer = message.key

    # application processing happens here
    print(f"Processing customer={customer} order={order['order_id']} partition={message.partition} offset={message.offset}")

    # commit only after successful processing
    consumer.commit()
```

### consumer safety practices

- make processing idempotent where possible
- commit only after safe completion
- monitor lag and rebalance frequency
- avoid blocking the poll loop carelessly
- size partition counts to realistic concurrency needs

---

## 10. Rebalancing: Necessary but Operationally Expensive

Rebalancing happens when consumer-group membership or partition ownership changes, such as when:

- a consumer joins
- a consumer leaves
- a consumer crashes
- partition counts change
- session/heartbeat expectations are missed

### what rebalance does

Kafka pauses or adjusts ownership so partitions can be reassigned safely across group members.

### production impact

Rebalances can cause:

- temporary consumption pauses
- duplicate processing near commit boundaries
- lag spikes
- operational noise during deployments or autoscaling

### design implication

Frequent restarts, unstable autoscaling, slow consumers, and poor timeout settings can make a consumer group look unhealthy even when the topic itself is fine.

---

## 11. Partition Assignment and Consumer Scaling

Consumer-group scaling is constrained by partition ownership.

### assignment reality

If a topic has:

- 3 partitions and 1 consumer → 1 consumer handles all 3 partitions
- 3 partitions and 3 consumers → ideal one partition each
- 3 partitions and 5 consumers → 2 consumers remain idle

### production takeaway

Partition count is not just a throughput knob. It is also a concurrency contract for consumer groups.

### sizing implication

Choose partition count based on:

- expected throughput
- parallel processing needs
- likely future scaling
- rebalance cost tolerance
- broker/storage limits

---

## 12. Retention and Compaction

Kafka keeps data according to configured retention policies even after consumers have read it.

## 12.1 Retention

Retention may be based on:

- time
- size
- both

### value

Retention supports:

- replay
- delayed consumers
- backfills
- incident investigation
- audit use cases

### caveat

Retention is not the same thing as backup or long-term archival strategy.

## 12.2 Log Compaction

Compaction keeps the latest record per key over time rather than preserving every historical update forever.

### good fit

Compaction is useful for state-like topics such as:

- latest account profile
- current inventory state
- current feature flags
- service routing metadata

### caveat

Compaction is not the same as strict snapshot semantics. Applications must still understand topic semantics carefully.

---

## 13. Lag, Backpressure, and Throughput Reality

## 13.1 Consumer Lag

Lag is the difference between the latest available offset and the committed offset for a consumer group.

### why lag matters

Lag is one of the most important operational signals because it can indicate:

- slow processing
- downstream outages
- partition imbalance
- traffic spikes
- broken consumer logic

## 13.2 Backpressure

Kafka can absorb bursty traffic better than many downstream systems, but that does not remove capacity limits.

### system truth

A fast producer plus a slow database still means the end-to-end pipeline is slow. Kafka often converts immediate failure into growing lag, not magical completion.

## 13.3 Throughput vs Latency

Tuning for maximum throughput often involves larger batches, more partitions, and more aggressive buffering. Tuning for low latency often means smaller batches and tighter timing. These goals pull in different directions.

---

## 14. Failure Modes, Duplicates, and Poison Messages

## 14.1 Broker Failure

If a partition leader fails, Kafka can promote another eligible replica to leader if replication health supports it.

### requirement

Failover quality depends on:

- healthy replicas
- ISR state
- election readiness
- client retry handling

## 14.2 Duplicate Processing

Duplicates can happen around:

- retries
- consumer restarts
- commit boundaries
- rebalance events
- downstream timeouts

### practical rule

Consumers should be idempotent where possible.

## 14.3 Poison Messages

A malformed or permanently unprocessable message can block a partition’s forward progress if the consumer repeatedly fails on the same record.

### safer pattern

Use a controlled failure strategy such as:

- validation
- bounded retries
- quarantine or dead-letter handling where appropriate
- alerting and operator visibility

## 14.4 Recovery Thinking

Recovery is often about reconciling three things:

- what Kafka successfully stored
- what the consumer actually processed
- what offsets were committed

That is why commit discipline matters so much.

---

## 15. Secure Streaming Workflows

Kafka security is not just “turn on SSL.” It includes identity, authorization, topic ownership, client isolation, secret handling, and controlled data movement.

## 15.1 Encryption in Transit

Use TLS to protect traffic between:

- clients and brokers
- brokers and brokers, where applicable

This reduces exposure of credentials and event data on the network.

## 15.2 Authentication

Every producer, consumer, connector, and processor should authenticate with a workload identity appropriate to its role.

### practical goal

Avoid shared human-style credentials across many services.

## 15.3 Authorization and ACL Scope

Authorization should control who can:

- read a topic
- write a topic
- create topics
- alter configs
- manage consumer groups
- administer the cluster

### least-privilege rule

A billing consumer should not also have broad admin or unrelated topic-write permissions.

## 15.4 Multi-Tenant Isolation

Shared Kafka environments need explicit separation by:

- topic naming
- topic ownership
- ACL boundaries
- environment boundaries
- service identities
- operational responsibility

### anti-pattern

One shared credential used by many applications destroys auditability and weakens isolation.

## 15.5 Secret Hygiene

Kafka credentials, certificates, and trust material should not be hardcoded in source code.

Stronger patterns include:

- secret managers
- mounted cert/key material with restricted permissions
- controlled environment injection
- rotation procedures
- workload-specific identities

## 15.6 Secure Producer Example

```python
from kafka import KafkaProducer
import json
import ssl

ssl_context = ssl.create_default_context(cafile="/etc/certs/ca.pem")
ssl_context.load_cert_chain(
    certfile="/etc/certs/client.pem",
    keyfile="/etc/certs/client.key"
)

producer = KafkaProducer(
    bootstrap_servers=["kafka1:9093", "kafka2:9093", "kafka3:9093"],
    security_protocol="SSL",
    ssl_context=ssl_context,
    acks="all",
    retries=5,
    linger_ms=10,
    value_serializer=lambda v: json.dumps(v).encode("utf-8")
)

producer.send("security.audit.v1", value={"event": "login_success", "user_id": "u-42"})
producer.flush()
producer.close()
```

## 15.7 Secure Consumer Example

```python
from kafka import KafkaConsumer
import json
import ssl

ssl_context = ssl.create_default_context(cafile="/etc/certs/ca.pem")
ssl_context.load_cert_chain(
    certfile="/etc/certs/client.pem",
    keyfile="/etc/certs/client.key"
)

consumer = KafkaConsumer(
    "security.audit.v1",
    bootstrap_servers=["kafka1:9093", "kafka2:9093", "kafka3:9093"],
    security_protocol="SSL",
    ssl_context=ssl_context,
    group_id="security-analytics",
    enable_auto_commit=False,
    value_deserializer=lambda v: json.loads(v.decode("utf-8"))
)

for message in consumer:
    event = message.value
    # process securely
    consumer.commit()
```

---

## 16. Practical Producer-Consumer Flow

This example shows a production-style pattern for an orders topic.

## 16.1 Producer

```python
from kafka import KafkaProducer
import json

producer = KafkaProducer(
    bootstrap_servers=["localhost:9092"],
    acks="all",
    retries=5,
    linger_ms=10,
    value_serializer=lambda v: json.dumps(v).encode("utf-8"),
    key_serializer=lambda k: k.encode("utf-8")
)

orders = [
    ("customer-101", {"order_id": "o-1", "status": "created", "amount": 120}),
    ("customer-102", {"order_id": "o-2", "status": "created", "amount": 95}),
    ("customer-101", {"order_id": "o-3", "status": "created", "amount": 220})
]

for key, value in orders:
    producer.send("orders.created.v1", key=key, value=value)

producer.flush()
producer.close()
```

## 16.2 Consumer

```python
from kafka import KafkaConsumer
import json

consumer = KafkaConsumer(
    "orders.created.v1",
    bootstrap_servers=["localhost:9092"],
    group_id="orders-processor",
    auto_offset_reset="earliest",
    enable_auto_commit=False,
    value_deserializer=lambda v: json.loads(v.decode("utf-8")),
    key_deserializer=lambda k: k.decode("utf-8") if k else None
)

for message in consumer:
    order = message.value
    customer = message.key

    print(f"Processing customer={customer} order={order['order_id']} amount={order['amount']}")

    # business processing succeeds
    consumer.commit()
```

## 16.3 Why the Key Matters

Because customer ID is used as the key, all events for the same customer are routed consistently, which preserves per-customer ordering within a partition.

---

## 17. Production Best Practices

- use replication for durability, not single-copy topics
- align replication factor, `acks`, and `min.insync.replicas`
- design keys intentionally around ordering and load
- choose partition counts for both throughput and consumer-group scaling
- use manual offset commit when processing safety matters
- make consumers idempotent where possible
- monitor lag, retries, ISR health, and rebalance frequency
- keep topic ownership and ACL scope clear
- use TLS and workload-specific identities
- treat retention as replay support, not as full backup strategy
- avoid topic sprawl and shared credentials

---

## 18. Common Anti-Patterns

### 18.1 Assuming topic-wide ordering

Ordering is partition-scoped, not global across the topic.

### 18.2 Using random or poor keys when entity ordering matters

This breaks deterministic ordering and can create hot partitions.

### 18.3 Setting replication factor high but leaving weak durability settings

Replication without proper `acks` and `min.insync.replicas` can create false confidence.

### 18.4 Blind auto-commit use

This can record progress before the application has safely completed work.

### 18.5 Ignoring rebalances

Unstable consumer groups can create pauses, lag spikes, and duplicate handling.

### 18.6 Treating retention as backup

Retention supports replay, not full disaster recovery.

### 18.7 Using shared credentials across many applications

This weakens isolation, least privilege, and auditability.

### 18.8 Assuming exactly-once without end-to-end validation

Exactly-once claims require whole-workflow design, not just one Kafka setting.

---

## 19. Final Framing

Kafka is safest and most effective when treated as a **durable distributed event log** with explicit design choices around partitioning, replication, ordering, and consumer progress.

The most important practical skills are understanding how to:

- design topics and keys intentionally
- choose partition counts based on concurrency and throughput needs
- align replication with real durability goals
- configure producers for safe writes
- manage offset commit timing carefully
- reason about consumer-group scaling and rebalancing
- secure identities, traffic, and topic access
- monitor lag, replica health, and failure behavior

Strong Kafka practice comes from combining **clear event design**, **realistic durability settings**, **careful consumer semantics**, **least-privilege security**, and **operational observability**.
