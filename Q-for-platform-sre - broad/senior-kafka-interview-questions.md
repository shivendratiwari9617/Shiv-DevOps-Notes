Senior Kafka Interview Question Set
1. Core senior Kafka screening questions
1. How would you explain Kafka’s role in a modern event-driven platform to a product-based company that runs multiple mission-critical services?
2. What are the main architectural responsibilities of Kafka as a streaming platform, and where do its guarantees stop?
3. How do you distinguish between a strong senior Kafka engineer and someone who has only used Kafka at the application level?
4. What are the most common reasons Kafka projects fail in production despite the platform being technically healthy?
5. When would you choose Kafka over a traditional queue, and when is Kafka the wrong choice?
6. How do you think about Kafka as both a messaging system and a distributed log?
7. What are the most important Kafka design decisions that must be made early to avoid long-term operational pain?
8. How would you evaluate whether a Kafka platform is ready for enterprise-scale multi-team adoption?
9. What are the risks of treating Kafka as “just infrastructure” rather than a product platform?
10. What are the first questions you would ask when inheriting an existing Kafka estate with unclear ownership?

2. Kafka architecture, brokers, controllers, metadata flow, partitions, replicas, leaders, ISR, and cluster behavior
1. Can you walk through what happens at a high level when a topic is created, assigned, and made available to clients?
2. How do brokers, partitions, leaders, followers, and replicas cooperate to serve reads and writes?
3. What is ISR, and how does it affect write availability and durability?
4. How does leader election work in Kafka, and what operational risks arise during leader movement?
5. What is the controller’s role in Kafka, and how does metadata propagation affect the rest of the cluster?
6. How would you explain controller quorum and metadata concepts at a high level to an engineering team?
7. What kinds of metadata-path failures can cause cluster-wide symptoms even when brokers are up?
8. How do partition leadership and replica assignment influence throughput, availability, and fault tolerance?
9. What causes a broker to fall out of ISR, and how would you distinguish transient lag from a serious replication problem?
10. What is the impact of leader imbalance on performance and operational stability?
11. How do partition count and replica factor shape Kafka’s internal behavior during failure and recovery?
12. What happens when a broker rejoins after downtime, and what conditions determine whether it can recover cleanly?
13. How does Kafka behave when the cluster has insufficient replicas to satisfy the configured write guarantees?
14. What is the operational significance of min.insync.replicas in real production systems?
15. How does unclean leader election change the durability and availability trade-off?
16. What are the risks of allowing unclean leader election in a platform with critical data?
17. How do metadata changes propagate to producers and consumers, and what failures can arise if metadata becomes stale?
18. What broker responsibilities are most often underestimated in production planning?
19. How do segment files, indexes, and log cleanup interact with partition behavior?
20. What cluster-state symptoms would make you suspect controller stress versus broker stress?

3. Producers, consumers, consumer groups, offsets, rebalancing, delivery semantics, and ordering guarantees
1. How do producer configuration choices influence delivery guarantees, latency, and throughput?
2. What does idempotent production solve, and what does it not solve?
3. How do retries interact with duplicates, ordering, and failure recovery?
4. What is the practical impact of max.in.flight requests on ordering and retries?
5. How do linger, batch size, and compression change producer behavior under load?
6. When would you tune producer batching aggressively, and when would that be risky?
7. What does acks=all actually guarantee, and what must be true on the broker side for that guarantee to hold?
8. How do you think about delivery semantics in a real system: at-most-once, at-least-once, and exactly-once?
9. What are the operational trade-offs of trying to achieve exactly-once semantics end to end?
10. How do consumer group coordination and rebalancing work at a high level?
11. What causes repeated rebalances, and how would you identify whether the issue is client-side or cluster-side?
12. How do offset commit strategies influence duplicates, data loss, and recovery behavior?
13. What is the relationship between commit timing, processing latency, and lag growth?
14. How do you design consumers to handle poison messages without blocking the entire partition?
15. What is your replay strategy when a consumer bug or downstream outage requires reprocessing historical data?
16. How do you preserve ordering guarantees in a distributed system while still scaling consumer throughput?
17. What are the limits of ordering in Kafka, and how would you explain them to application teams?
18. How do you reason about partition affinity, consumer parallelism, and rebalancing cost?
19. How would you diagnose consumer lag if the lag increase started immediately after a deployment?
20. What evidence would you gather before blaming the broker for a consumer lag incident?
21. How do autoscaling events commonly affect consumer groups, and why do they sometimes increase lag instead of reducing it?
22. How do long-running batch jobs or slow downstream dependencies affect offset progress and rebalance behavior?

4. Topic design, partition strategy, keying, throughput planning, retention, compaction, and storage trade-offs
1. How do you decide the right number of partitions for a topic in a new platform?
2. What are the trade-offs between too few partitions and too many partitions?
3. How do you choose a message key, and what failure modes appear when key selection is poor?
4. What causes hot partitions, and how would you detect them when cluster-wide metrics still look acceptable?
5. How do you plan topic design for both current throughput and future growth?
6. When would you use log compaction instead of time-based retention?
7. What are the operational implications of choosing retention-based topics versus compacted topics?
8. How do compaction backlog and segment accumulation affect disk usage and recovery time?
9. What design mistakes in topic naming, partitioning, or retention become expensive to fix later?
10. How do you balance cost against durability when defining retention windows?
11. What are the risks of over-partitioning a topic?
12. How does partition count affect consumer scalability, controller overhead, and operational complexity?
13. How would you design topics for high-cardinality tenants while avoiding noisy-neighbor problems?
14. What storage trade-offs do you consider when choosing segment size, retention, and cleanup policy?
15. How do topic design choices affect reprocessing, archival, and compliance requirements?
16. What would you do if a topic’s key distribution is causing sustained skew and localized saturation?
17. How do you decide whether to change the key, increase partitions, or redesign the event model?
18. What are the consequences of changing partition count after clients are already in production?
19. How do you plan for throughput if one tenant can suddenly dominate traffic?
20. How do you distinguish a topic design problem from a broker capacity problem?

5. Cluster operations, broker sizing, scaling, balancing, rack awareness, upgrades, and capacity planning
1. How do you size brokers for CPU, disk, network, page cache, and JVM overhead in a production Kafka cluster?
2. What broker resources are most likely to become bottlenecks first in a busy cluster?
3. How do you plan capacity for both steady-state load and failure headroom?
4. What is your approach to scaling a cluster without causing avoidable partition movement or client disruption?
5. How would you evaluate whether a cluster needs more brokers, more partitions, or better topic design?
6. What is rack awareness, and why does it matter for fault tolerance?
7. How do you validate that replica placement actually provides the resilience you think it does?
8. What are the operational risks of leader imbalance, and how would you correct it safely?
9. How do rolling upgrades affect brokers, clients, and cluster behavior?
10. What compatibility concerns matter most during Kafka version upgrades?
11. How do you reduce blast radius during upgrades or major config changes?
12. What evidence would you collect before moving partitions or rebalancing leadership in production?
13. How do you handle configuration drift across brokers, and why is it dangerous?
14. What operational checks would you run before and after a broker restart?
15. How do you approach broker decommissioning in a production cluster?
16. What are the common mistakes in capacity planning for storage growth and log retention?
17. How do you account for burst traffic, replay storms, and recovery traffic in sizing exercises?
18. What metrics tell you a cluster is approaching its safe operating limit?
19. How do you think about balancing automation and manual control in Kafka operations?
20. What changes would you make to a platform that requires frequent partition movement to stay healthy?

6. Reliability, availability, durability, replication behavior, failure handling, and recovery scenarios
1. How do Kafka’s replication mechanisms translate into durability in a real incident?
2. What happens to availability and data safety when brokers fail during peak traffic?
3. How do you decide whether to prefer availability or strict durability in a production design?
4. What does replica lag tell you, and what does it not tell you?
5. How would you interpret under-replicated partitions during maintenance versus during an incident?
6. What is the relationship among replication factor, ISR, min.insync.replicas, and acks?
7. How do you reason about write availability when a follower is slow or unavailable?
8. What is the risk of unclean leader election during a regional or multi-broker failure?
9. How would you handle recovery after broker loss without creating a second incident?
10. How do you distinguish data loss risk from temporary unavailability?
11. What happens when a broker rejoins but has stale or incomplete data?
12. How do you think about durability in the presence of disk corruption, network loss, or partial cluster failure?
13. How would you handle a scenario where producers keep timing out but consumers are still reading?
14. What are the recovery risks of aggressively restarting brokers during an outage?
15. How do you validate that replication has actually converged after an incident?
16. What operational signals indicate that the cluster is healing versus silently degrading?
17. How do you design for graceful degradation when the cluster cannot meet normal performance levels?
18. What is the role of replica placement in avoiding correlated failures?
19. How do you respond when a cluster appears healthy overall but specific partitions are unavailable?
20. What is your recovery strategy if the outage spans brokers, clients, and downstream dependencies at the same time?

7. Performance tuning, latency, throughput, batching, compression, backpressure, and hotspot troubleshooting
1. How do you approach performance tuning differently for low latency versus high throughput workloads?
2. What are the main producer-side settings that influence throughput and latency most directly?
3. How do batching and compression interact with network utilization and end-to-end latency?
4. When can aggressive batching hurt rather than help?
5. How do you identify whether throughput limits are caused by CPU, disk, network, or request queue pressure?
6. What broker metrics would make you suspect page cache pressure or disk saturation?
7. How do request queues, network threads, and I/O threads influence observed latency?
8. What does backpressure look like in Kafka, and where can it originate?
9. How would you troubleshoot a sudden producer throughput drop when CPU is not maxed?
10. How do you identify hot partitions that create localized bottlenecks while the cluster looks fine at a macro level?
11. What role does message size play in throughput, latency, and broker health?
12. How do retries and request timeouts contribute to load amplification during an incident?
13. What causes excessive fetch wait times or slow consumer polls?
14. How do you separate broker-side latency from client-side serialization, processing, or GC issues?
15. What Kafka metrics are most useful when investigating p99 latency problems?
16. How do compression choice and compression level influence overall platform efficiency?
17. What changes would you make when broker network saturation is the dominant bottleneck?
18. How would you evaluate whether increasing partitions will improve performance or make the system worse?
19. What tuning mistakes commonly create false confidence during load tests?
20. How do you confirm that a performance improvement in one component does not create instability elsewhere?

8. Security, ACLs, authentication, authorization, encryption, secret handling, and multi-tenant platform governance
1. How do you design Kafka security for a multi-team platform without making the system unusable?
2. What authentication mechanisms would you consider for enterprise Kafka, and how do you choose among them?
3. How do ACLs shape access control, and what are the operational risks of misconfiguration?
4. What kinds of security failures show up as silent application failures rather than obvious auth errors?
5. How do you approach encryption in transit and at rest for Kafka deployments?
6. What are the consequences of poor secret handling in producer and consumer clients?
7. How would you prevent one tenant from overwhelming shared brokers or violating platform policies?
8. What quota strategies would you use for connections, produce rate, fetch rate, or disk consumption?
9. How do you balance centralized governance with tenant flexibility and self-service?
10. What naming, lifecycle, and ownership standards would you impose for topics in a shared platform?
11. How do you manage access reviews and auditability for Kafka topics and consumer groups?
12. What are the security implications of schema registries, Connect clusters, and replication tools?
13. How would you investigate a production issue that turned out to be an ACL regression?
14. What are the operational risks of giving teams broad access to create topics or alter configurations?
15. How do you handle tenant isolation when multiple high-risk workloads share the same Kafka estate?
16. What abuse patterns are most likely to destabilize a multi-tenant Kafka platform?
17. How do you design guardrails so platform users can move fast without creating security debt?
18. What evidence would you collect during a suspected credential compromise involving Kafka clients?
19. How do you govern topic retention, retention exceptions, and data deletion requests?
20. How do you ensure security controls do not become the hidden cause of repeated application outages?

9. Kafka Connect, MirrorMaker, schema management, ecosystem integration, and platform ownership questions
1. How do you evaluate Kafka Connect as a platform component rather than a convenience tool?
2. What operational risks arise when Connect becomes a critical ingestion or export layer?
3. How do you isolate connector failures from broker failures during an incident?
4. What is the difference between a connector problem, a task problem, and a worker problem?
5. How would you design for connector restarts, retries, and failure containment?
6. What is your approach to schema management in a large event-driven organization?
7. How do schema evolution mistakes create partial outages or compatibility regressions?
8. What compatibility strategy would you enforce for producers and consumers?
9. How do you diagnose issues when schema registry changes break downstream consumers?
10. What are the trade-offs between strict schema governance and application-team autonomy?
11. How would you own MirrorMaker or cross-cluster replication in a production platform?
12. What are the most common failure modes for replication pipelines during regional issues?
13. How do you decide whether to replicate all topics or only a curated subset?
14. What risks arise when replication lag grows faster than the recovery window?
15. How do you validate data consistency across source and target clusters?
16. What operational controls would you put around connector deployment, configuration changes, and secret rotation?
17. How do you integrate Kafka with databases, search systems, caches, or stream processors without creating tight coupling?
18. What ownership model works best for shared platform components like Connect and schema tooling?
19. How do you troubleshoot an application that appears healthy but stops consuming because of an ecosystem dependency issue?
20. How do you prevent platform support burden from growing uncontrollably as integrations multiply?

10. Observability, metrics, lag analysis, logs, alerting, auditability, and signal quality during incidents
1. What Kafka metrics do you consider essential for day-to-day operations?
2. Which metrics matter most for broker health, and which matter most for user-visible service health?
3. How would you design alerting so it is useful during incidents rather than noisy and ignored?
4. What are the limitations of relying on consumer lag as the main health signal?
5. How do you analyze lag when multiple consumer groups and partitions are involved?
6. What would you check first when lag is rising but brokers look healthy?
7. How do you correlate broker metrics, client metrics, and downstream service metrics during an outage?
8. What log signals help distinguish controller issues, replica issues, and client misbehavior?
9. How do you build auditability into a Kafka platform for security and compliance requirements?
10. What does “signal quality” mean during a Kafka incident, and how do you preserve it?
11. How would you identify whether lag is caused by slow consumption, rebalance churn, or partition skew?
12. What are the most common false positives in Kafka alerting?
13. How do you investigate under-replicated partitions when dashboards are conflicting?
14. What metrics would you expect to change when a broker is disk-bound versus network-bound?
15. What observability gaps most often prevent teams from reaching root cause quickly?
16. How would you instrument a platform so tenant-level issues are visible without exposing sensitive data?
17. What logs and timestamps are most useful when reconstructing an incident timeline?
18. How do you tell whether the controller path or data path is the source of degradation?
19. What evidence would you capture before making a risky operational change?
20. How do you ensure post-incident analysis is based on facts rather than assumptions?

11. Troubleshooting failed producers, consumer lag, stuck partitions, under-replicated partitions, controller issues, and data-flow anomalies
1. A producer starts timing out after a deployment, but cluster CPU is normal. What would you check first, and why?
2. Consumer lag is rising across one group, but only some partitions are affected. How would you determine whether the problem is in partitioning, consumer behavior, or downstream dependency?
3. Under-replicated partitions appear during broker maintenance. What evidence would you gather before restarting brokers, moving partitions, or changing replication settings?
4. A single partition is stuck behind while the rest of the topic is healthy. How would you isolate whether the issue is a hot key, slow consumer, leader problem, or replication lag?
5. Producers are retrying heavily and throughput has dropped sharply. How would you determine whether the problem is ISR shrinkage, network issues, batching settings, or broker pressure?
6. A consumer group repeatedly rebalances. What would you inspect first, and how would you decide whether the root cause is client instability, long processing time, or coordinator stress?
7. The cluster looks healthy, but one tenant reports missing or delayed data. How would you investigate a tenant-specific degradation in a shared platform?
8. Messages are being produced but downstream systems are not seeing them consistently. What end-to-end checks would you perform?
9. You suspect a controller issue, but brokers are still accepting traffic. What signs would make you confirm or reject that suspicion?
10. A topic begins to accumulate disk faster than expected. How would you distinguish retention growth, compaction backlog, runaway topic creation, or producer misuse?
11. A consumer deployment causes duplicate processing after autoscaling. How would you reason through the failure chain?
12. A schema change partially breaks consumers, but no producer errors were raised. How do you identify the blast radius and root cause?
13. MirrorMaker or replication lag is increasing during a regional issue. What would you inspect before taking action?
14. Access suddenly fails for one application, but no obvious platform errors are present. How would you investigate ACL, auth, and secret-related causes?
15. Bad client behavior is overwhelming brokers with excessive connections or request storms. What would you throttle or block first, and how would you avoid collateral damage?
16. Partition leadership is badly imbalanced after maintenance. How would you confirm the problem and restore balance safely?
17. A broker is alive but causing anomalies on only some partitions. How would you distinguish disk, network, JVM, or metadata-path issues?
18. How do you decide whether to reduce blast radius by isolating a tenant, moving partitions, pausing traffic, or scaling the cluster?
19. What would make you restart a broker versus wait for self-healing?
20. What is your step-by-step approach when the data flow is degraded but the exact failing component is unclear?

12. Incident response, degraded clusters, broker loss, disk pressure, network partitions, bad client behavior, and recovery under pressure
1. How do you structure incident response for a Kafka degradation when multiple teams are involved?
2. What are the first five minutes of your response when a production cluster shows widespread lag and producer errors?
3. How do you reduce blast radius while keeping data flowing during a Kafka incident?
4. What would you do if a broker suffers disk pressure during peak traffic?
5. How do you respond to a network partition that splits cluster communication but does not fully take brokers down?
6. What is your plan when one or more brokers are lost unexpectedly?
7. How do you decide whether to prioritize customer-facing availability or data durability during an active incident?
8. What evidence would you collect before changing retention, increasing partitions, or altering replication under pressure?
9. How do you handle a bad client rollout that floods brokers with retries and connection churn?
10. How would you stabilize a cluster facing both producer timeouts and consumer lag at the same time?
11. What do you do when disk usage is rising faster than your incident team can explain?
12. How do you coordinate safe partition movement during an incident without creating more instability?
13. What is your approach to a degraded cluster that appears healthy in dashboards but is failing for a few critical tenants?
14. How do you decide whether to fail over to another cluster, region, or service path?
15. What recovery steps are safe to automate, and which ones need deliberate human approval?
16. How do you make incident decisions when telemetry is incomplete or contradictory?
17. What would you do if a recovery action causes a second-order effect like a rebalance storm or leader churn?
18. How do you validate that the incident is truly resolved before closing it?
19. What changes would you make after the incident to prevent recurrence?
20. How would you write a post-incident review for a Kafka outage that had multiple contributing factors?

13. Tooling and architecture trade-offs: Kafka vs queues vs stream-processing alternatives vs managed services
1. When is Kafka a better fit than a traditional queue, and when is it not?
2. What kinds of use cases are better served by durable queues, pub/sub brokers, or event streams rather than Kafka?
3. How do you compare Kafka with alternative stream-processing architectures for real-time data movement?
4. When would you choose a managed Kafka service over self-managed Kafka?
5. What operational capabilities do you gain or lose by using a managed service?
6. What hidden costs often appear in self-managed Kafka at scale?
7. What risks come from assuming managed Kafka removes the need for platform expertise?
8. How do you compare a single cluster, multi-cluster, and regional replication model?
9. When is multi-cluster architecture justified, and when does it add unnecessary complexity?
10. How do you evaluate trade-offs between higher durability settings and higher throughput or lower latency?
11. What are the trade-offs between consumer simplicity and exactly-once ambitions?
12. How do you think about synchronous downstream processing versus decoupled event-driven designs?
13. What are the operational consequences of choosing high partition counts to solve a scaling problem?
14. How do you decide whether to build platform governance in Kafka or in surrounding systems?
15. What are the trade-offs between retention and compaction from a cost, recovery, and replay perspective?
16. How do you judge whether a use case belongs in Kafka, a database, a search engine, or a workflow system?
17. What signs tell you the event architecture is becoming too complex for its business value?
18. How would you explain to a product team why Kafka is not a drop-in replacement for every integration pattern?
19. What decisions would you standardize centrally, and what would you leave to application teams?
20. What architecture would you recommend for a highly available event platform that serves multiple regions and teams?

14. Bar-raiser troubleshooting and war-room scenarios
1. A critical topic is lagging only on one side of a multi-region setup. How would you lead the war room?
2. A deployment caused duplicates, lag, and a spike in producer retries at the same time. How would you isolate the first domino?
3. A “healthy” cluster has no broad alarms, but one revenue-critical tenant is missing events. What would you investigate first, and why?
4. You suspect both broker pressure and a bad client rollout. How would you prove which one is primary?
5. What would you check first if a sudden increase in partitions was followed by instability across the cluster?
6. How would you handle a war-room situation where teams want to restart brokers immediately but evidence is incomplete?
7. A topic has severe skew and one partition is saturated. How would you stabilize the system quickly without masking the underlying problem?
8. How would you proceed if consumer lag rises after an autoscaling event and the team cannot tell whether it is normal warm-up or a real incident?
9. A schema update has partially broken downstream consumers in production. How do you limit damage while restoring service?
10. A replication pipeline is falling behind during a regional network issue. What decisions would you make under time pressure?
11. What evidence would you demand before changing retention or compaction settings in the middle of a crisis?
12. How do you decide whether to throttle clients, add capacity, or pause traffic during an overload event?
13. A broker is slow only during certain hours. How would you determine whether the cause is workload shape, maintenance, backup jobs, or tenant behavior?
14. What would you do if a cluster-wide dashboard looks acceptable, but several business-critical workflows are failing?
15. How do you keep a war room focused on data, hypotheses, and actions rather than assumptions and noise?
16. What are the hardest Kafka incidents to resolve quickly, and why?
17. How do you prevent a “fix” from creating a larger outage downstream?
18. What would you ask application teams to produce immediately during a Kafka incident?
19. How do you communicate risk when the safest action might be to accept some data delay?
20. What would a strong incident commander ask a Kafka platform owner in the first ten minutes?

15. Mock panel round questions grouped by architect round, platform/DevOps round, SRE/operations round, security/compliance round, and hiring manager round
Architect round
1. How would you design a Kafka platform for multiple product teams with different durability, throughput, and retention needs?
2. How would you decide between a single shared cluster and multiple isolated clusters?
3. How do you design for tenant isolation without fragmenting the platform too much?
4. How would you model data flow, replay, and disaster recovery for a cross-region Kafka architecture?
5. What topic, partition, and replication standards would you mandate for a company-scale platform?
6. How do you design an event model that supports evolution without constant consumer breakage?
7. What is your strategy for balancing platform governance and developer autonomy?
8. How would you prevent Kafka from becoming the bottleneck in a larger event-driven architecture?

Platform / DevOps round
1. How would you automate topic provisioning, ACL management, and lifecycle governance?
2. What configuration drift checks would you implement across brokers and clients?
3. How would you operationalize rolling upgrades with minimal client impact?
4. What would your standard broker capacity model include?
5. How would you handle broker decommissioning, partition movement, and leader rebalancing safely?
6. What platform guardrails would you build to prevent abuse by internal teams?
7. How would you support self-service onboarding while preserving compliance and reliability?
8. What would you monitor to know whether the platform is becoming operationally fragile?

SRE / operations round
1. How would you triage a cluster showing producer timeouts, rising lag, and under-replicated partitions simultaneously?
2. What is your playbook for disk pressure on a broker during business hours?
3. How would you investigate frequent consumer rebalances in a production consumer fleet?
4. How would you distinguish broker saturation from client-side misuse during an incident?
5. What would your incident thresholds be for pausing traffic, throttling clients, or failing over?
6. How do you validate that recovery actions have not introduced silent data loss or duplication?
7. What are the most important runbooks for a Kafka operations team?
8. How would you improve signal quality in the dashboards and alerts for a Kafka estate?

Security / compliance round
1. How would you design authentication, authorization, and audit logging for a multi-tenant Kafka platform?
2. What is your approach to secrets rotation across producers, consumers, Connect workers, and replication pipelines?
3. How would you enforce ACL standards without creating excessive operational friction?
4. How do you handle retention and deletion policies for regulated data in Kafka?
5. What are the security risks of broad topic-creation rights or unconstrained consumer-group access?
6. How would you investigate a suspected privilege regression or unauthorized access incident?
7. How do you ensure encryption and access controls remain correct across clusters and regions?
8. How would you explain Kafka security posture to auditors or risk stakeholders?

Hiring manager round
1. What does good Kafka ownership look like in a senior engineer or architect?
2. How do you balance short-term incident response with long-term platform improvements?
3. Tell me about a time you simplified a Kafka platform rather than adding more tooling.
4. Tell me about a time you had to push back on an architecture that would have increased operational risk.
5. How do you influence application teams to build reliable consumers and producers without becoming a bottleneck?
6. What is your approach to teaching teams to think in terms of failure modes, not just happy paths?
7. How do you decide which Kafka problems belong to the platform team and which belong to the application team?
8. What would success look like after your first 6 to 12 months owning a Kafka platform?