# Top 100 Senior SRE Interview Questions for Product-Based Companies

This version is intentionally focused on realistic senior SRE interviews at product-based companies. The questions are scenario-driven, aligned with on-call problem solving, and centered on production judgment rather than generic platform trivia.

Each question includes:
- a realistic senior-level interview prompt
- a concise answer aligned with product-company SRE expectations
- a short explanation of why it matters in real on-call and production environments

## Service Reliability and SLOs

### 1. Your checkout API had 99.95% availability for months, but this week it dropped to 99.5% after two bad deploys. What do you do next?
**Concise answer:** Freeze risky releases, quantify error-budget burn, stabilize the service, and review what release controls failed before allowing normal rollout speed again.
**Why it matters in practice:** In a product company, the senior SRE is expected to use reliability policy to influence shipping decisions. This question checks whether you can turn SLOs into action during real delivery pressure.

### 2. The product team wants a 99.99% SLO for an internal analytics service used once a day. How would you respond?
**Concise answer:** Push back and align the target with actual user criticality, support expectations, and cost. Recommend an SLO that reflects the business impact of failure rather than an aspirational number.
**Why it matters in practice:** Senior SREs in product companies must protect engineering time from unrealistic targets. The interviewer is looking for judgment, not blind compliance.

### 3. A service meets its uptime SLO, but users still complain that it feels unreliable. What might be wrong?
**Concise answer:** The SLI is probably measuring the wrong thing. I would check latency, correctness, freshness, and key user-journey success rather than relying only on coarse uptime.
**Why it matters in practice:** This is realistic in consumer and SaaS products where users care about end-to-end experience, not whether a process merely responded.

### 4. How would you define an SLI for a payments service?
**Concise answer:** Measure user-visible outcomes at the service boundary, such as successful payment completion rate and latency for authorization or capture operations.
**Why it matters in practice:** Product companies care about completed business transactions. This question checks whether you choose indicators tied to customer value rather than infrastructure health.

### 5. A product manager says reliability work is slowing roadmap delivery. How do you handle that conversation?
**Concise answer:** Frame the discussion using user impact, incident cost, and error-budget consumption, then show which reliability fixes directly reduce production risk and release interruptions.
**Why it matters in practice:** Senior SREs must influence without sounding theoretical. In real product teams, reliability work competes with feature work every quarter.

### 6. Your API is horizontally scalable, but incidents still happen during peak traffic. What reliability gap do you investigate first?
**Concise answer:** I would check dependencies, quotas, connection pools, caches, and load-shedding behavior because scalable stateless tiers still fail when bottlenecks sit downstream.
**Why it matters in practice:** This is a practical senior-level question because many systems appear scalable until a hidden shared dependency collapses.

### 7. What is a realistic way to use error budgets in a fast-moving product company?
**Concise answer:** Use them to adjust release speed, require stronger rollout controls, and prioritize reliability work when burn rate is unhealthy, rather than treating them as a compliance artifact.
**Why it matters in practice:** This checks whether you can make SRE policy useful in an environment that ships often and resists process overhead.

### 8. A service has strong redundancy, but one dependency still causes recurring customer incidents. What does that tell you?
**Concise answer:** The real single point of failure is likely outside the replicated tier, so I would map dependencies and redesign around the actual weak link or add protective isolation.
**Why it matters in practice:** Senior SREs should think in end-to-end critical paths. Product outages often come from an overlooked dependency, not the front-end service fleet.

### 9. How would you evaluate whether a proposed reliability investment is worth it?
**Concise answer:** I would compare incident frequency, blast radius, recovery pain, user impact, and engineering cost, then prioritize work that removes recurring or high-severity failure patterns.
**Why it matters in practice:** In product companies, SRE leaders are expected to make tradeoffs that are defensible to both engineering and business stakeholders.

### 10. What does graceful degradation look like in a consumer product?
**Concise answer:** Keep the core user workflow alive while disabling secondary features such as recommendations, analytics, avatars, or export functions.
**Why it matters in practice:** This is very realistic for product companies, where preserving the main funnel often matters more than keeping every feature online.

### 11. How do you identify whether your current SLOs are too lenient?
**Concise answer:** If the service meets SLOs while users still report pain, incidents feel frequent, or important workflows degrade without policy impact, the objective is likely too weak or poorly scoped.
**Why it matters in practice:** This tests whether you treat SLOs as living product reliability tools instead of fixed paperwork.

### 12. A team wants different SLOs for enterprise customers and free-tier users. Is that reasonable?
**Concise answer:** Yes, if the product genuinely offers differentiated service expectations and the measurement boundaries are clear, observable, and operationally supportable.
**Why it matters in practice:** Many product businesses have tiered customer commitments. Senior SREs need to reason about fairness, complexity, and supportability.

### 13. What is the biggest mistake teams make with SLO dashboards?
**Concise answer:** They show a target status but not the burn rate, user impact, or the specific slices where reliability is actually failing.
**Why it matters in practice:** In real operations, a green dashboard can hide fast-moving problems until the budget is already gone.

## On-Call, Incident Response, and Escalation

### 14. You get paged at 2:00 AM: checkout success rate drops from 98% to 70% in five minutes. What do you do first?
**Concise answer:** Confirm user impact, stop ongoing risky changes, establish an incident lead, and narrow the failure to recent deploys, dependencies, or regional scope before chasing deeper root cause.
**Why it matters in practice:** This is classic product-company on-call work. The interviewer wants fast, structured triage rather than random debugging.

### 15. During an incident, ten engineers jump into the call and all start proposing different fixes. How do you handle it?
**Concise answer:** Assign incident command, separate mitigation from investigation, give owners to each workstream, and keep one person responsible for coordination and updates.
**Why it matters in practice:** Senior SREs are expected to reduce chaos. This is a realistic test of leadership under pressure.

### 16. A bad deploy is suspected, but the developer says rollback might make things worse because of a schema change. What do you do?
**Concise answer:** Pause further rollout, assess the schema compatibility risk, choose the lowest-risk mitigation, and prefer the action that restores service fastest with the highest confidence.
**Why it matters in practice:** Product incidents often involve partial compatibility and time pressure. This checks senior judgment, not just textbook rollback advice.

### 17. How do you know when to declare a major incident instead of continuing quiet debugging?
**Concise answer:** Declare early when user impact is meaningful, uncertainty is high, or multiple teams will likely be needed to recover quickly.
**Why it matters in practice:** Late incident declaration is a common failure in product companies that try to avoid noise and lose coordination time.

### 18. A dependency owned by another team is failing, but they are slow to respond. What should a senior SRE do?
**Concise answer:** Escalate using the incident process, quantify user impact clearly, define the dependency as the likely blocker, and build temporary mitigation if possible rather than waiting passively.
**Why it matters in practice:** Product-company SREs often operate in cross-team dependency graphs. This question tests how you move blocked incidents forward.

### 19. What belongs in a good incident status update to leadership?
**Concise answer:** Customer impact, current scope, mitigation progress, known facts, risks, and the next update time.
**Why it matters in practice:** Executives and support teams need clarity without technical noise. Strong responders communicate with discipline.

### 20. How do you avoid alert storms during an outage?
**Concise answer:** Use symptom-based paging, deduplication, dependency-aware suppression, and route follow-on alarms into the active incident instead of paging every team independently.
**Why it matters in practice:** This is realistic in large products where one backend issue can flood many downstream alerts.

### 21. You mitigated the incident with a feature flag. Is the incident over?
**Concise answer:** The user impact may be mitigated, but the incident is not fully closed until the underlying cause, residual risk, and safe re-enable plan are understood.
**Why it matters in practice:** Senior SREs distinguish service restoration from durable recovery. Product teams often stop too early after a workaround.

### 22. What makes a runbook useful during a real on-call page?
**Concise answer:** It should help someone under pressure quickly validate symptoms, perform safe mitigations, know escalation paths, and avoid guesswork.
**Why it matters in practice:** A long document full of theory is not helpful at 3:00 AM. This question checks whether you write for responders, not for audits.

### 23. You keep seeing incidents where responders spend 20 minutes figuring out who owns the system. What do you fix?
**Concise answer:** I would fix ownership metadata, paging routes, service catalog accuracy, and incident entry points so responders immediately know who and what is involved.
**Why it matters in practice:** In product organizations, unclear ownership is a surprisingly common cause of longer outages.

### 24. How do you know whether your on-call rotation is unhealthy?
**Concise answer:** Look for high page volume, repeat pages, poor page quality, burnout signals, low action-item closure, and incidents caused by known unresolved toil.
**Why it matters in practice:** Senior SREs are expected to improve the operating model, not just staff more people into an unhealthy rotation.

### 25. What is the difference between a responder and an incident commander?
**Concise answer:** A responder works a technical stream, while the incident commander coordinates people, decisions, priorities, and communication.
**Why it matters in practice:** This sounds basic, but confusion here is a common reason incidents become disorganized.

### 26. How do you handle a recurring incident pattern that never reaches sev1 but keeps waking up on-call?
**Concise answer:** Treat it as a reliability problem worth prioritizing, quantify the page burden and user pain, and drive a structural fix instead of letting it normalize.
**Why it matters in practice:** Senior SREs should not let repeated medium pain become culturally accepted just because it is not catastrophic.

## Observability, Alerting, and Triage

### 27. Your service is up, CPU is normal, but users say the product is slow. Where do you start?
**Concise answer:** I start with user-facing latency, recent deploys, downstream dependency timing, queueing, and trace data before trusting host-level health signals.
**Why it matters in practice:** This is realistic in product companies where infrastructure can look healthy while request paths are degraded.

### 28. What makes an alert page-worthy for a senior SRE team?
**Concise answer:** It must signal urgent user impact or a fast-moving risk that requires human action now.
**Why it matters in practice:** This tests whether you understand that on-call quality depends more on alert actionability than alert coverage volume.

### 29. Why do product teams often drown in alerts even with modern tooling?
**Concise answer:** Because they alert on component symptoms instead of customer impact, ignore dependency relationships, and rarely review whether a page changed responder behavior.
**Why it matters in practice:** This is a practical senior-level view of why observability programs fail operationally.

### 30. How do you design a dashboard for the primary on-caller during an incident?
**Concise answer:** Make it answer what broke, where it broke, when it changed, what dependencies are involved, and whether mitigation is helping.
**Why it matters in practice:** This is different from business dashboards. The interviewer wants incident-ready thinking.

### 31. A service has metrics and logs but incidents still take too long to debug. What observability gap do you suspect?
**Concise answer:** Likely missing request correlation, poor log structure, weak ownership context, missing deploy markers, or lack of end-to-end tracing.
**Why it matters in practice:** This reflects real-world observability failures in microservice products.

### 32. When are traces more useful than logs?
**Concise answer:** When I need to understand cross-service request flow, latency contribution, or which downstream hop is actually failing under one user transaction.
**Why it matters in practice:** This is common in product-company architectures with many backend services and asynchronous edges.

### 33. How do you detect a silent failure that does not trigger technical alerts?
**Concise answer:** Use synthetic user journeys, business KPIs, and monitoring on critical workflows such as signup, checkout, login, or content delivery.
**Why it matters in practice:** Product companies often discover outages from support tickets because their telemetry misses user-centric failure modes.

### 34. What is the biggest mistake in latency alerting?
**Concise answer:** Alerting on brief spikes or averages instead of sustained tail-latency degradation tied to user pain.
**Why it matters in practice:** This question checks whether you understand why many latency alerts are either noisy or useless.

### 35. How would you instrument a newly launched product feature before turning it on for all users?
**Concise answer:** Add success/failure metrics, latency, rate, structured logs, traces, deploy markers, and feature-flag dimensions for segmented analysis.
**Why it matters in practice:** This is highly realistic in product companies where features are rolled out gradually and must be observed safely.

### 36. Why is high-cardinality telemetry dangerous?
**Concise answer:** It can make the observability stack expensive, slow, or unstable and can hurt debugging if the system becomes hard to query during incidents.
**Why it matters in practice:** Senior SREs need to care about observability reliability too, not just application reliability.

### 37. How do you know an alert should be deleted rather than tuned?
**Concise answer:** If it does not drive action, is always downstream of a better signal, or repeatedly pages without changing incident outcomes, it should be removed.
**Why it matters in practice:** This tests maturity. Good SRE teams prune alerting aggressively.

### 38. What makes structured logging valuable in incident response?
**Concise answer:** It allows fast filtering by request, tenant, region, feature flag, or dependency outcome instead of forcing responders to parse raw text under pressure.
**Why it matters in practice:** In product systems with multi-tenant and feature-driven behavior, structure dramatically reduces time to isolate impact.

### 39. A dashboard shows errors are rising, but support tickets mention data is stale rather than failing outright. What does that suggest?
**Concise answer:** The service may be meeting a coarse success signal while violating freshness or correctness, so the main SLI is likely incomplete.
**Why it matters in practice:** This is realistic for feeds, search, analytics, and content products where stale output is a real user-facing failure.

## Safe Change Management and Release Reliability

### 40. A release went through CI successfully but caused a production incident. What does that tell you?
**Concise answer:** It means test success did not cover the real production risk, so I would examine rollout safety, environment differences, dependency behavior, and missing canary signals.
**Why it matters in practice:** This is extremely common in product companies. Senior SREs are expected to close the gap between CI confidence and production truth.

### 41. How do you make product releases safer without blocking delivery velocity?
**Concise answer:** Use staged rollouts, strong rollback paths, feature flags, production telemetry gates, and smaller change batches rather than adding heavy manual process everywhere.
**Why it matters in practice:** This reflects the reality of product companies that ship continuously and resist slow approval workflows.

### 42. What makes a canary rollout effective?
**Concise answer:** A narrow blast radius, meaningful comparison metrics, fast rollback criteria, and enough traffic to reveal real behavior differences.
**Why it matters in practice:** This is senior-level because many teams say they do canaries but still lack the signals needed to trust them.

### 43. How would you decide whether a change is too risky for a Friday evening release?
**Concise answer:** I would consider blast radius, rollback complexity, on-call staffing, dependency sensitivity, customer usage patterns, and whether failure would be quickly observable.
**Why it matters in practice:** This tests practical operational judgment rather than rigid calendar superstition.

### 44. A developer says, 'We can always roll back.' Why might that be false?
**Concise answer:** Schema changes, cache poisoning, irreversible side effects, background jobs, and partial writes can make rollback incomplete or unsafe.
**Why it matters in practice:** Senior SREs should understand why rollback confidence must be designed, not assumed.

### 45. How do feature flags reduce operational risk?
**Concise answer:** They let teams separate deploy from release, limit exposure, and disable risky behavior quickly without redeploying the whole service.
**Why it matters in practice:** This is deeply relevant in product companies with frequent experiments and progressive delivery.

### 46. How do you prevent config changes from becoming hidden production risk?
**Concise answer:** Treat config like code, review it, version it, validate it, and observe it after rollout with the same discipline as application changes.
**Why it matters in practice:** Many incidents come from 'just config' changes that bypass normal delivery safeguards.

### 47. What is change failure rate and why do you care?
**Concise answer:** It is the percentage of production changes that cause user-impacting incidents, rollbacks, or degraded service.
**Why it matters in practice:** This is a practical release-health metric for teams that want both speed and operational quality.

### 48. How do you investigate whether incidents correlate with release timing?
**Concise answer:** Compare incident timelines with deploy markers, feature rollouts, and config changes, then validate whether the correlation holds across similar events.
**Why it matters in practice:** Senior SREs use production evidence to improve release safety rather than relying on anecdotes.

### 49. When should a rollout be halted even if impact seems minor?
**Concise answer:** If the failure pattern is growing, confidence is low, rollback is cheap, or the issue affects a critical workflow or high-value customer segment, I would stop.
**Why it matters in practice:** This checks whether you can act early rather than waiting for a small issue to become a headline incident.

### 50. What should a pre-release readiness check include for a critical product service?
**Concise answer:** Observability, rollback path, dependency health, capacity posture, feature-flag plan, on-call awareness, and clear ownership during the release window.
**Why it matters in practice:** This is realistic senior SRE work in teams shipping high-impact changes.

### 51. How do smaller batch sizes improve reliability?
**Concise answer:** They reduce blast radius, simplify debugging, and make rollback or mitigation faster because fewer variables changed at once.
**Why it matters in practice:** This is a core practical principle in continuous delivery at product companies.

### 52. What is the operational downside of long-lived feature branches?
**Concise answer:** They increase integration risk, enlarge change size, and make production behavior less predictable when finally merged.
**Why it matters in practice:** Senior SREs care about development patterns when they directly affect incident risk.

## Performance, Capacity, and Overload Handling

### 53. Your p99 latency doubled after traffic grew 20%, but average latency barely moved. What do you investigate?
**Concise answer:** I would investigate queueing, dependency saturation, lock contention, noisy neighbors, and whether a subset of requests or tenants is hitting a bottleneck.
**Why it matters in practice:** This is a realistic senior-level performance question because tail latency usually exposes system stress before averages do.

### 54. How do you distinguish a CPU problem from a queueing problem during an incident?
**Concise answer:** I look at request concurrency, wait time, queue depth, service time, and resource saturation together rather than assuming high latency means CPU shortage.
**Why it matters in practice:** Product systems often fail from waiting, not pure compute exhaustion.

### 55. What is load shedding and when should you use it?
**Concise answer:** Load shedding rejects excess work to preserve the core service when the system cannot safely serve all demand.
**Why it matters in practice:** Senior SREs in product companies must protect the main user journey during spikes instead of letting everything fail slowly.

### 56. What is backpressure in practical terms?
**Concise answer:** It is a mechanism that forces producers to slow down when downstream systems cannot keep up.
**Why it matters in practice:** This is critical in event-driven and microservice product architectures where overload otherwise spreads quickly.

### 57. A cache miss spike causes database saturation. What do you do?
**Concise answer:** Reduce load on the database first with rate controls, emergency caching, or degraded behavior, then investigate why cache effectiveness dropped.
**Why it matters in practice:** This is a highly realistic incident pattern in product systems under load.

### 58. How do retries make incidents worse?
**Concise answer:** Unbounded or synchronized retries amplify traffic on failing dependencies and can turn a partial problem into a full outage.
**Why it matters in practice:** Senior SREs are expected to think about failure amplification, not just failure detection.

### 59. What metrics matter most during a capacity review for a critical user-facing service?
**Concise answer:** Traffic shape, headroom, saturation, queueing, dependency limits, cache efficiency, and behavior during failover or one-zone loss.
**Why it matters in practice:** This is practical because real capacity failures often happen in degraded modes, not normal steady state.

### 60. How do you know your autoscaling policy is not enough?
**Concise answer:** If scale-up happens too late, scaling does not relieve the actual bottleneck, or new capacity cannot register fast enough to help, the policy is insufficient.
**Why it matters in practice:** This is realistic in product companies where autoscaling exists but does not guarantee resilience.

### 61. What does a healthy overload strategy look like?
**Concise answer:** Prioritize core requests, shed less valuable work, limit concurrency, preserve recovery capacity, and fail quickly rather than timing out everything.
**Why it matters in practice:** This is senior-level reliability design that directly affects user experience during peak events.

### 62. A single large tenant can degrade others in a multi-tenant product. What is the SRE concern?
**Concise answer:** The system lacks fair isolation, so I would look at quotas, rate limits, workload segregation, and tenancy-aware resource controls.
**Why it matters in practice:** This is common in B2B SaaS products and tests whether you think beyond aggregate metrics.

### 63. How do you investigate whether a performance issue is code, data, or dependency driven?
**Concise answer:** I compare recent changes, request mix, data size shifts, dependency timing, and traces for the hot path before making assumptions.
**Why it matters in practice:** This reflects real-world incident analysis where similar symptoms can come from very different causes.

### 64. What is the operational risk of relying on a cache for correctness instead of acceleration?
**Concise answer:** Cache inconsistency or invalidation failure can become a user-visible correctness incident, not just a performance regression.
**Why it matters in practice:** Senior SREs must understand that performance layers can turn into correctness risks in product systems.

### 65. How do you prepare for a major traffic event like a launch or sale?
**Concise answer:** Review capacity, dependency headroom, rate limits, cache readiness, alert quality, load-shedding controls, and an explicit rollback or mitigation plan.
**Why it matters in practice:** This is highly realistic in product companies with periodic traffic spikes.

## Distributed Systems, Dependencies, and Resilience

### 66. One downstream service is timing out, and now five upstream services are degraded. What is happening?
**Concise answer:** A dependency failure is cascading through callers, likely through retries, thread exhaustion, or queue buildup.
**Why it matters in practice:** This is one of the most common real-world failure patterns in product backends.

### 67. What is the purpose of a circuit breaker in a production service?
**Concise answer:** It stops sending traffic to a failing dependency for a period so the caller can fail fast, degrade gracefully, or recover capacity.
**Why it matters in practice:** Senior SREs should know how to contain dependency failure rather than letting it spread.

### 68. Why is idempotency important in product-company systems?
**Concise answer:** It makes retries, replays, and recovery operations safer by preventing duplicate side effects from repeated requests or events.
**Why it matters in practice:** This matters a lot in payment, messaging, billing, and workflow systems.

### 69. How do you think about dependency maps during incidents?
**Concise answer:** They help identify likely blast radius, critical paths, ownership, and which teams or systems need immediate attention.
**Why it matters in practice:** The interviewer wants to see end-to-end systems thinking, not component-local reasoning.

### 70. A queue is growing, but consumers are still running. What do you investigate?
**Concise answer:** I would check processing rate, dependency latency, poison messages, concurrency limits, downstream saturation, and whether work shape changed.
**Why it matters in practice:** This is realistic in event-driven products where backlog growth can hide a worsening incident.

### 71. What is the danger of a 'healthy' async backlog?
**Concise answer:** A backlog can look acceptable by count while age, retry churn, or dead-letter growth indicates users are already experiencing delayed outcomes.
**Why it matters in practice:** Senior SREs should monitor the user effect of async systems, not just internal buffer size.

### 72. How do you reduce blast radius in a distributed system?
**Concise answer:** Isolate failure domains, limit concurrency, use bulkheads, partition workloads, and protect dependencies with timeouts, retries, and circuit-breaking policies.
**Why it matters in practice:** This is core product-company SRE work because modern services rarely fail in isolation.

### 73. What does eventual consistency mean for on-call response?
**Concise answer:** It means some user-visible lag may be expected, so responders must distinguish normal propagation delay from real stuck or divergent state.
**Why it matters in practice:** This is realistic for feeds, search indexing, analytics, and replicated data paths.

### 74. How do you debug partial failure where some users succeed and others fail?
**Concise answer:** Slice by region, tenant, feature flag, dependency path, payload size, and recent rollout exposure to find the common failing dimension.
**Why it matters in practice:** Senior SREs should be able to reason about segmented impact, not just global averages.

### 75. Why are synchronous dependency chains risky?
**Concise answer:** They make end-to-end latency and availability the product of multiple systems, increasing failure propagation and making overload harder to contain.
**Why it matters in practice:** This reflects real architectural risk in feature-rich product platforms.

### 76. What is a bulkhead pattern in practical SRE terms?
**Concise answer:** It isolates one class of traffic, tenant, or dependency pool so failure or saturation there does not consume all shared capacity.
**Why it matters in practice:** This matters in multi-tenant product systems where one noisy path can impact all users.

### 77. How do you know a service is too dependent on retries for normal success?
**Concise answer:** If retries contribute a meaningful portion of baseline success, the underlying dependency is already unstable or the client behavior is masking a latent issue.
**Why it matters in practice:** This is a subtle but very realistic production smell.

## Linux, Networking, and Systems Troubleshooting for On-Call

### 78. A service is healthy locally on the host but unreachable through the product endpoint. How do you triage it?
**Concise answer:** I check listener bindings, local process health, service discovery, load balancer registration, DNS, firewall rules, and upstream routing in that order.
**Why it matters in practice:** This is a realistic on-call scenario that requires layered systems thinking.

### 79. How do you investigate a sudden spike in TCP connection failures?
**Concise answer:** I would check file descriptor exhaustion, SYN backlog issues, load balancer health, NAT or ephemeral port exhaustion, and recent network or release changes.
**Why it matters in practice:** Senior SREs need practical debugging instincts for connection-oriented product traffic.

### 80. Users report intermittent timeouts, but server CPU is low. What does that suggest?
**Concise answer:** The bottleneck may be network, storage, dependency latency, queueing, or thread starvation rather than CPU saturation.
**Why it matters in practice:** This is realistic because many production incidents are waiting problems, not compute problems.

### 81. What does high iowait tell you during an incident?
**Concise answer:** The system is spending time waiting on I/O completion, which usually points me toward disk, storage, or downstream blocking behavior.
**Why it matters in practice:** This is useful for separating application symptoms from infrastructure bottlenecks.

### 82. A pod or process keeps restarting. What do you check first?
**Concise answer:** I check exit reason, health probes, resource limits, dependency startup assumptions, and whether the restart loop is caused by the app or its supervisor.
**Why it matters in practice:** This is one of the most common real on-call scenarios in modern production systems.

### 83. Why do file descriptors matter for a busy API service?
**Concise answer:** Because every socket, log file, and open resource consumes one, and exhaustion can prevent new traffic from being accepted.
**Why it matters in practice:** This is a practical senior troubleshooting topic, especially for connection-heavy services.

### 84. How can DNS become a product incident even when application code is unchanged?
**Concise answer:** Record changes, resolver issues, TTL behavior, split-horizon mistakes, or dependency name resolution failures can break traffic flow instantly.
**Why it matters in practice:** Real outages often originate in operational layers far below the product logic.

### 85. What is a practical sign of NAT or ephemeral port exhaustion?
**Concise answer:** New outbound connections fail or become intermittent under load while existing traffic may still partially work.
**Why it matters in practice:** This is realistic in service-to-service architectures with high outbound fan-out.

### 86. How would you debug memory pressure without immediately assuming a leak?
**Concise answer:** I would separate cache growth, workload shape change, per-process usage, swap behavior, and OOM history before calling it a leak.
**Why it matters in practice:** Senior SREs should know that low free memory is not itself a diagnosis.

### 87. What does clock skew break in real systems?
**Concise answer:** It can break auth tokens, distort logs and traces, confuse ordering, and cause coordination issues in distributed workflows.
**Why it matters in practice:** This is a subtle but realistic source of hard-to-explain production failures.

### 88. How do you investigate packet loss complaints from only one region?
**Concise answer:** I compare regional health, edge routing, dependency paths, ISP or provider differences, and whether the issue appears at transport or application layer.
**Why it matters in practice:** This is realistic for global or multi-region product companies.

### 89. What is the practical impact of MTU mismatch?
**Concise answer:** It can cause fragmentation or dropped packets, leading to retransmissions, slow connections, or broken requests under specific paths.
**Why it matters in practice:** This is a classic hard-to-diagnose networking issue in overlays, VPNs, and container networks.

### 90. Why do keepalive settings matter for product traffic?
**Concise answer:** Poor keepalive tuning can leave dead connections open too long or churn healthy ones too aggressively, affecting latency and connection reuse.
**Why it matters in practice:** This question checks whether you can connect protocol tuning to user-facing behavior.

## Senior Judgment, Ownership, and Real Product-Company Scenarios

### 91. A new feature increases revenue but causes a 3x rise in sev2 incidents. How do you respond as a senior SRE?
**Concise answer:** I would quantify the tradeoff, push for guardrails or redesign, and make the reliability cost visible so the business can choose with real operational data.
**Why it matters in practice:** Senior SREs in product companies are expected to influence product direction using evidence, not just say yes or no.

### 92. What does strong ownership look like for a senior SRE during a customer-facing outage?
**Concise answer:** Driving clarity, coordination, mitigation, follow-through, and prevention work until the operational risk is actually reduced.
**Why it matters in practice:** This checks for leadership beyond technical debugging.

### 93. How do you push back on a team that repeatedly ships without observability for new features?
**Concise answer:** I treat observability as a release requirement for user-facing risk, define the minimum production signals needed, and block or constrain rollout until they exist.
**Why it matters in practice:** This is realistic in product environments where feature pressure often outruns operational readiness.

### 94. A team says, 'This outage only affected 2% of users.' Why might that still be serious?
**Concise answer:** That 2% may map to a critical cohort, a high-value tenant, a revenue funnel, or a region where failure is concentrated.
**Why it matters in practice:** Senior SREs should think in business and customer slices, not only global percentages.

### 95. How do you decide what deserves a postmortem?
**Concise answer:** I include not only high-severity outages but also repeated pages, near misses, risky mitigations, and failures that exposed major control gaps.
**Why it matters in practice:** This shows maturity. Product reliability improves fastest when teams learn from more than just headline incidents.

### 96. What is a realistic sign that a team is normalizing operational pain?
**Concise answer:** Frequent manual mitigations, repeated night pages, known noisy alerts, and incident action items that never get prioritized are all red flags.
**Why it matters in practice:** Senior SREs should recognize unhealthy patterns before they become part of the culture.

### 97. How do you handle a high-performing developer who keeps causing operational incidents with rushed changes?
**Concise answer:** I focus on release controls, review quality, rollout safety, and systemic incentives rather than making it a personal argument.
**Why it matters in practice:** This is realistic senior-level work because many reliability problems are social-technical, not purely technical.

### 98. A service has no major outages, but on-call still feels miserable. What could be wrong?
**Concise answer:** Too many low-quality pages, manual toil, poor tools, noisy dependencies, weak runbooks, or a high rate of minor recurring incidents.
**Why it matters in practice:** This reflects real product-company operations where uptime alone does not define a healthy system.

### 99. How do you know whether a reliability problem is technical debt or product strategy debt?
**Concise answer:** If the issue comes from repeated shortcuts forced by roadmap pressure or unsupported complexity in the product model, it is also strategy debt, not just code debt.
**Why it matters in practice:** Senior SREs should understand when reliability pain originates from higher-level product decisions.

### 100. What makes a senior SRE answer feel senior in an interview?
**Concise answer:** It ties diagnosis and action to user impact, risk, tradeoffs, recovery speed, and the realities of on-call ownership in production.
**Why it matters in practice:** This is exactly what product-company interviewers look for: practical judgment shaped by real incidents.
