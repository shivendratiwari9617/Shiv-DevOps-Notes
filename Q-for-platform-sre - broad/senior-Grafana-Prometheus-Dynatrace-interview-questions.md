Senior Observability Interview Question Set
Grafana, Prometheus, and Dynatrace
Target Level: 8–12 Years of Experience
---
1. Core senior observability screening questions
1. How do you define observability in a production product company, and how is it different from traditional monitoring?
2. What is your approach to building an observability strategy for a platform that serves multiple engineering teams?
3. How do you distinguish white-box monitoring from black-box monitoring, and when would you use each?
4. How do you decide whether a signal belongs in metrics, logs, traces, or synthetic checks?
5. What are the most common observability failures you have seen in mature production systems?
6. How do you evaluate whether an observability stack is actually reducing incident time and risk?
7. What are the first signals you trust most in the first 10 minutes of a production incident, and why?
8. How do you determine whether the issue is with the workload, the telemetry pipeline, or the observability tools themselves?
9. What makes observability useful for senior engineers versus only useful for operations teams?
10. How do you ensure observability is treated as a product and not just as a set of tools?

---
2. Observability architecture and telemetry strategy
1. How would you design an end-to-end observability architecture for a large product-based company with multiple services and teams?
2. How do you decide which telemetry should be generated at the application layer versus infrastructure layer versus platform layer?
3. What is your approach to standardizing telemetry across teams without blocking innovation?
4. How do you balance centralized observability governance with team-level ownership and autonomy?
5. What are the key design choices in a telemetry pipeline from collection to storage to visualization to alerting?
6. How do you evaluate whether your observability platform should be opinionated or flexible?
7. How would you define an observability taxonomy for metrics, logs, traces, events, and synthetics?
8. How do you build a telemetry strategy that works across Kubernetes, VMs, cloud services, and hybrid environments?
9. What trade-offs do you consider when deciding between open-source observability building blocks and a managed platform?
10. How would you design observability for a multi-region, multi-cluster, and multi-tenant production estate?
11. How do you ensure telemetry remains actionable and does not become “data exhaust”?
12. How do you separate business-critical observability needs from nice-to-have visibility?

---
3. Prometheus fundamentals, data model, and PromQL depth
1. Explain Prometheus’s data model and how time series are identified.
2. How do metric names, labels, and label values work together in Prometheus?
3. What is the difference between counters, gauges, histograms, and summaries, and when would you use each?
4. How do you think about metric naming conventions in a large organization?
5. What is PromQL’s model for selecting, aggregating, and transforming time series?
6. How would you debug a PromQL query that returns unexpected results during an incident?
7. How do you use rate, irate, increase, sum by, avg by, max by, and histogram_quantile correctly in production?
8. What are the common mistakes engineers make when querying counters in PromQL?
9. How do you use label matching, regex matching, and vector matching safely at scale?
10. How do joins and binary operations in PromQL create risk in production dashboards and alerts?
11. When would you use recording rules, and when would you avoid them?
12. How do you design PromQL queries so they remain maintainable across service and cluster growth?
13. How do you query for error rate, latency, and saturation using PromQL for a service with multiple replicas?
14. What are the implications of counter resets, missing samples, and stale series in PromQL analysis?
15. How do you use PromQL to distinguish traffic drops from application failures?
16. How do you handle histogram bucket design and query performance trade-offs?
17. What are the practical trade-offs between histograms and summaries for latency measurement?
18. How do exemplars change how you investigate performance spikes?
19. How do you build PromQL queries that are safe for dashboard panels but also useful for alerting?
20. How do you interpret absent data versus zero values in Prometheus?

---
4. Prometheus scaling, federation, remote write, HA, and multi-cluster design
1. How would you scale Prometheus for a large Kubernetes-based estate with thousands of targets?
2. What problems does federation solve, and what problems does it not solve?
3. When would you choose remote write over federation, and why?
4. How do you design a Prometheus HA setup, and what failure modes must you account for?
5. How do duplicate samples, replica disagreement, and alert duplication happen in Prometheus HA designs?
6. How do you prevent inconsistent alert firing when two Prometheus replicas evaluate the same rules?
7. What would you check first if remote write starts lagging during an incident?
8. How do you diagnose backpressure in a Prometheus remote write pipeline?
9. How do you think about long-term storage for Prometheus data in a cost-efficient way?
10. How do you design a multi-cluster Prometheus architecture without creating blind spots?
11. How do you decide which metrics stay local and which are aggregated centrally?
12. How do you handle retention policies differently for raw metrics, aggregated metrics, and alerting data?
13. What are the main trade-offs between single central Prometheus, per-cluster Prometheus, and a federated hierarchy?
14. How do you plan for target churn in Kubernetes when pods and endpoints are constantly changing?
15. How do you handle scrape target instability caused by service discovery in dynamic environments?
16. What are the operational risks of over-centralizing Prometheus?
17. How do you design for disaster recovery in a Prometheus-based observability stack?
18. How do you validate observability continuity during cluster upgrades or failovers?
19. How do you prevent storage blowups caused by high-cardinality labels in a scaled Prometheus environment?
20. How would you compare Thanos-like, Cortex-like, Mimir-like, and other distributed Prometheus patterns at a conceptual level?

---
5. Exporters, instrumentation, metric design, and label/cardinality control
1. How do you decide what should be instrumented directly in the application versus collected through exporters?
2. What makes a metric “good” from a production operations perspective?
3. How do you design metrics so they are useful for both troubleshooting and trend analysis?
4. How do you prevent label explosion while still preserving enough context for debugging?
5. What are the most common causes of cardinality explosions in Prometheus environments?
6. How would you investigate a sudden increase in Prometheus memory or storage usage?
7. What would you check first if a new release causes a sharp increase in metric cardinality?
8. How do you decide whether a label belongs in a metric or should be moved to logs or traces?
9. What are the trade-offs of per-user, per-session, per-request, or per-pod labels?
10. How do you instrument latency for APIs with variable paths, methods, and error conditions without creating high-cardinality metrics?
11. How do you design histogram buckets for real service latency distributions?
12. How do you use exemplars to connect metrics to traces in practice?
13. What is your approach to metric versioning and deprecating obsolete metrics?
14. How do you ensure exporters are reliable and not introducing blind spots?
15. How do you handle instrumentation in legacy services where code changes are risky?
16. What is the role of service discovery in exporter-based metrics collection?
17. How do you validate that instrumentation is accurate and not misleading under failure conditions?
18. How do you distinguish useful dimensioning from dangerous dimensioning in metric design?
19. What would you do if a team insists on adding high-cardinality labels for convenience?
20. How do you define organization-wide standards for metric naming, label hygiene, and instrumentation quality?

---
6. Alerting strategy, SLOs/SLIs, noise reduction, and incident usability
1. How do you design alerting so it detects real user impact rather than internal noise?
2. What is your approach to defining SLIs and SLOs for customer-facing services?
3. How do you ensure an SLO reflects user pain and not just infrastructure health?
4. What would you do if the dashboard is green but users are still impacted?
5. How do you decide between symptom-based alerts and cause-based alerts?
6. How do you reduce page volume without missing critical incidents?
7. How do you use burn-rate alerting effectively?
8. How do you define alert routing for multi-team, multi-service ownership models?
9. How do deduplication, inhibition, and silencing work in a mature alerting design?
10. What is your strategy for maintenance windows, planned outages, and alert suppression?
11. How do you prevent duplicate alerts from multiple replicas or multiple observability layers?
12. What would you check first if an alert starts flapping during a traffic spike?
13. How do you distinguish genuine regression from transient noise in alert evaluation?
14. How do you design alerts that remain useful during partial telemetry loss?
15. What are the risks of over-alerting on every metric threshold?
16. How do you make alert messages actionable for the responder in the first five minutes?
17. How do you test alerts before production rollout?
18. How do you handle alerts that are accurate but not operationally useful?
19. What is your strategy for paging, ticketing, dashboards, and chat notifications across severity levels?
20. How do you measure and improve alert quality over time?

---
7. Grafana architecture, dashboards, data sources, templating, RBAC, and governance
1. How do you design Grafana as a shared platform for many teams without causing dashboard sprawl?
2. What are the architectural considerations when deploying Grafana for enterprise-scale usage?
3. How do you decide which data sources should be exposed in Grafana and to whom?
4. How do you manage dashboard templating across teams and environments?
5. What are common failure patterns in Grafana variables and templated dashboards?
6. How do you prevent inconsistent dashboard design across product teams?
7. How do you define good dashboard design for production operations?
8. What are the most common dashboard anti-patterns you have seen in practice?
9. How do you design dashboards for service ownership, incident response, and executive visibility differently?
10. How do you keep dashboards aligned with SLOs, not just metrics availability?
11. What is your approach to RBAC, folder structure, and access control in Grafana?
12. How do you govern dashboard creation, ownership, review, and retirement?
13. How do you handle broken dashboards caused by renamed metrics, removed labels, or changed data sources?
14. What would you check first if a Grafana dashboard loads but panels are blank during an incident?
15. How do you manage dashboard-as-code workflows in Grafana?
16. How do you prevent dashboard duplication across teams and services?
17. How do you design annotations, drill-downs, and links between dashboards, logs, and traces?
18. How do you use Grafana to support both day-to-day health views and deep troubleshooting views?
19. How do you balance customization versus standardization in Grafana dashboards?
20. How do you define governance for shared panels, library panels, and common dashboard patterns?

---
8. Dynatrace architecture, OneAgent/ActiveGate deployment, tagging, management zones, and Davis AI usage
1. How would you explain Dynatrace’s architecture at a high level in a large enterprise environment?
2. What is the role of OneAgent, and what operational issues can arise during rollout?
3. What is the role of ActiveGate, and when is it needed?
4. How do you plan a safe OneAgent deployment across diverse infrastructure estates?
5. What would you check first if OneAgent rollout causes performance concerns on critical hosts?
6. How do you evaluate overhead concerns related to agent-based observability?
7. How do you use tagging effectively in Dynatrace for ownership, service segmentation, and searchability?
8. What are management zones, and how do they support access control and operational separation?
9. How do you prevent management zone design from becoming unmaintainable?
10. How do you think about auto-tagging strategy versus manual tagging strategy?
11. How do you use Dynatrace topology mapping in production troubleshooting?
12. What is the role of Davis AI, and how should a senior engineer validate or challenge its findings?
13. How do you distinguish signal from noise when using automated anomaly detection?
14. How do you use synthetic monitoring and real user monitoring together in Dynatrace?
15. What would you check first if synthetic checks pass but users report degraded experience?
16. How do you use Dynatrace for root cause analysis across infrastructure, services, and dependencies?
17. How do you manage data access, segmentation, and governance across different business units in Dynatrace?
18. How do you handle environment separation across dev, test, staging, and production in Dynatrace?
19. How do you identify blind spots in an agent-heavy observability strategy?
20. How do you compare Dynatrace’s operational model with a Prometheus/Grafana-centric model?

---
9. Logs, traces, metrics correlation and end-to-end troubleshooting design
1. How do you design telemetry so metrics, logs, traces, and topology can be correlated during incidents?
2. What context must be propagated through a request path to enable effective correlation?
3. How do you ensure trace IDs and correlation IDs are present across services, queues, and async workflows?
4. How do you investigate a production issue when logs exist but traces do not correlate?
5. What would you check first if you cannot connect a spike in latency to a specific trace pattern?
6. How do you correlate user impact with backend service telemetry?
7. How do you use topology maps to narrow the blast radius of an incident?
8. How do you troubleshoot when one telemetry type is healthy but another is missing or delayed?
9. How do you design for observability across synchronous, asynchronous, and event-driven systems?
10. How do you correlate gateway metrics, service metrics, database metrics, and application traces?
11. How do you handle missing trace context in distributed systems?
12. How do you decide whether to troubleshoot from the top down or bottom up?
13. How do you integrate logs into a production troubleshooting workflow without overwhelming responders?
14. What are the trade-offs between metrics-first and traces-first troubleshooting approaches?
15. How do you identify whether a symptom is caused by dependency latency, queue backpressure, thread starvation, or resource exhaustion?
16. How do you detect telemetry gaps that could create false confidence during incidents?
17. How do you use exemplars, annotations, and request context to accelerate debugging?
18. How would you design observability for an incident where the user-facing API is healthy but downstream jobs are failing?
19. How do you ensure telemetry supports both fast triage and deeper forensic analysis?
20. What would you change in the telemetry design to reduce future blind spots?

---
10. Kubernetes and cloud-native observability with Grafana, Prometheus, and Dynatrace
1. How do you monitor Kubernetes at node, pod, workload, namespace, and cluster levels without overwhelming the platform?
2. What are the hardest observability problems in Kubernetes environments?
3. How do you handle target churn caused by pod restarts, rolling deployments, and autoscaling?
4. What would you check first if Kubernetes service discovery stops discovering expected targets?
5. How do you monitor control plane components, nodes, and workloads differently?
6. How do you build observability for ingress, service mesh, and internal east-west traffic?
7. How do you troubleshoot a situation where Kubernetes metrics look healthy but applications are still degraded?
8. How do you design observability for jobs, cronjobs, and ephemeral workloads?
9. How do you handle Prometheus scrape configuration in a dynamic cluster environment?
10. How do relabeling mistakes show up in production, and how do you debug them?
11. How do you compare operator-managed Prometheus deployments with external managed observability agents in Kubernetes?
12. What role do DaemonSets, sidecars, and exporters play in cloud-native observability?
13. How do you observe autoscaling behavior and its impact on user experience?
14. How do you troubleshoot noisy or missing data during cluster upgrades?
15. How do you monitor multi-cluster services that share dependencies across regions?
16. How do you capture observability for managed cloud services where host-level access is limited?
17. How do you integrate cloud-provider signals with Prometheus, Grafana, and Dynatrace?
18. How do you handle observability when workloads span private cloud and public cloud?
19. What are the observability blind spots created by container abstraction?
20. How do you use cloud-native observability to detect saturation before users feel it?

---
11. CI/CD, GitOps, dashboard-as-code, alert-as-code, and observability platform operations
1. How do you manage dashboards, alerts, and rules as code in a mature engineering organization?
2. What should be version-controlled in an observability platform, and what should remain runtime-configurable?
3. How do you design CI/CD checks for dashboards and alert definitions?
4. How do you prevent broken PromQL, invalid dashboard variables, and misconfigured alerts from reaching production?
5. How do you handle approvals, reviews, and ownership for observability changes?
6. How do you integrate observability changes into GitOps workflows?
7. How do you roll out alert changes safely across multiple services and environments?
8. What is your approach to testing observability changes without generating pager noise?
9. How do you operationalize standard dashboards across many teams while still allowing customization?
10. How do you detect and manage configuration drift in observability systems?
11. How do you handle upgrades, migrations, and deprecations in Grafana, Prometheus, and Dynatrace?
12. What would you check first if an observability configuration change causes missing data or broken alerts?
13. How do you manage secrets, tokens, and credentials used by observability pipelines?
14. How do you document platform conventions so they are adopted consistently?
15. How do you measure observability platform reliability as an internal service?
16. How do you define SLAs or support expectations for an internal observability platform?
17. How do you prioritize platform work between new features, incident fixes, and debt reduction?
18. How do you manage observability tool upgrades with minimal service impact?
19. How do you prevent observability automation from becoming too rigid or too brittle?
20. How do you treat observability platform operations as part of product engineering excellence?

---
12. Security, compliance, access control, data residency, and sensitive telemetry handling
1. How do you determine what telemetry can be collected, stored, and shared under security and compliance constraints?
2. What are the risks of sensitive data leaking into metrics, logs, or traces?
3. How do you design access control for observability tools across teams and roles?
4. How do you implement least privilege in Grafana, Prometheus, and Dynatrace environments?
5. How do you handle data residency restrictions in a centralized observability architecture?
6. What would you do if a compliance policy prevents certain telemetry from being sent to a centralized platform?
7. How do you sanitize or redact sensitive fields without making telemetry unusable?
8. How do you manage secrets, tokens, and credentials used by observability collectors and integrations?
9. How do you think about auditability for dashboard access, alert changes, and query usage?
10. How do you prevent observability tooling from becoming a path for lateral access?
11. How do you design RBAC and segmentation for multi-tenant observability?
12. What are the implications of storing PII in labels, logs, or traces?
13. How do you handle observability in regulated environments with strict retention or export controls?
14. How do you validate that auto-tagging or metadata enrichment does not create compliance exposure?
15. How do you separate operational visibility from business-sensitive telemetry?
16. How do you respond when a security team requests telemetry limitation that may reduce troubleshooting capability?
17. What controls would you put in place for shared dashboards and broad query access?
18. How do you balance forensic visibility with privacy requirements?
19. How do you ensure observability exports do not violate cross-border data transfer rules?
20. How do you build a governance model for sensitive telemetry across multiple engineering teams?

---
13. Reliability, scale, cost control, and retention strategy
1. How do you decide the right retention period for metrics, logs, and traces?
2. What is your strategy for balancing long retention with storage and query cost?
3. How do you identify the telemetry sources that contribute most to cost without providing proportional value?
4. What would you check first if observability costs suddenly increase?
5. How do you prevent high-cardinality metrics from causing retention and storage blowups?
6. How do you control trace ingestion volume without losing debugging capability?
7. How do you manage log verbosity in production while preserving enough context for incidents?
8. What is your approach to downsampling, aggregation, or rollups for long-term analysis?
9. How do you think about hot, warm, and cold observability data tiers?
10. How do you decide whether to retain raw samples or only pre-aggregated metrics?
11. How do you ensure observability tooling itself remains reliable under load?
12. What would you do if query performance degrades as retention grows?
13. How do you measure and optimize the efficiency of dashboards, alerts, and recording rules?
14. How do you use quotas, budgets, or guardrails to control telemetry growth?
15. How do you handle cost allocation across teams consuming a shared observability platform?
16. How do you decide which signals should be sampled versus collected fully?
17. What are the reliability risks of over-optimizing for cost?
18. How do you keep observability effective during periods of extreme traffic growth?
19. How do you plan for observability capacity ahead of major product launches or seasonal peaks?
20. How do you prove that observability spend is justified by incident reduction or productivity gains?

---
14. Incident response, war-room troubleshooting, and postmortem-oriented questions
1. Walk through your first 10 minutes in a major production incident with incomplete telemetry.
2. What signals would you trust most first, and why?
3. How do you distinguish telemetry failure from real workload failure during an outage?
4. What would you check first if dashboards are green but customer complaints are increasing?
5. How do you structure a war room so that signal gathering, hypothesis testing, and communication stay organized?
6. How do you avoid tunnel vision when early signals are misleading?
7. How do you correlate dashboards, metrics, traces, logs, topology, and user impact under time pressure?
8. What would you do if you suspect a telemetry gap is hiding the true root cause?
9. How do you troubleshoot missing or delayed metrics during an incident?
10. How do you investigate scrape failures, stale series, and service discovery gaps in the middle of an outage?
11. How do you respond when alerts are noisy and responders stop trusting them?
12. How do you handle incidents where multiple teams own different parts of the observability path?
13. How do you decide when to rely on synthetic monitoring versus real-user signals during an incident?
14. How do you document observability lessons learned in a postmortem?
15. What changes would you prioritize after an incident to reduce future blind spots?
16. How do you ensure postmortems lead to measurable observability improvements?
17. How do you validate that the same incident would be easier to detect next time?
18. How do you use incident analysis to improve alert quality and dashboard design?
19. How do you avoid adding more noise in response to one noisy incident?
20. What is your process for converting incident findings into platform standards?

---
15. Tooling trade-offs: Prometheus/Grafana vs managed observability vs Dynatrace-style platform approaches
1. When would you prefer self-managed Prometheus and Grafana over a managed observability platform?
2. When would you prefer a managed observability platform over building your own stack?
3. What are the architectural and operational trade-offs between Prometheus/Grafana and Dynatrace-style platforms?
4. How do you evaluate open-source flexibility versus commercial AI-assisted observability capabilities?
5. What are the risks of vendor lock-in in observability, and how do you mitigate them?
6. How do you compare scraping-based observability with agent-based or push-based approaches?
7. What are the strengths and weaknesses of metrics-first versus traces-first troubleshooting strategies?
8. How do you weigh the benefits of centralized observability against federated team ownership?
9. When does standardization help more than it hurts in observability?
10. When is team-level customization worth the extra operational complexity?
11. How do you compare self-managed retention strategies with vendor-managed retention models?
12. What are the trade-offs in using many specialized tools versus one integrated observability platform?
13. How do you evaluate whether AI-driven anomaly detection genuinely reduces incident burden?
14. What are the hidden costs of operating an open-source observability stack at scale?
15. What are the hidden costs of adopting a commercial observability platform at scale?
16. How do you decide whether to keep observability data portable across tools?
17. How would you support an organization that has overlapping observability tools and unclear ownership?
18. How do you handle teams that prefer local autonomy while the platform team wants standardization?
19. What is your decision framework for introducing a new observability tool into an existing ecosystem?
20. How do you ensure tooling choices align with long-term reliability and operating model goals?

---
16. Leadership, platform ownership, developer enablement, and cross-team observability standards
1. How do you define ownership for observability across platform, SRE, DevOps, and application teams?
2. How do you turn observability into a platform capability that developers actually adopt?
3. How do you create standards without becoming a bottleneck for product teams?
4. How do you influence teams to instrument services correctly without heavy-handed enforcement?
5. How do you create a shared observability vocabulary across engineering organizations?
6. How do you decide which observability patterns should be mandatory and which should be recommended?
7. How do you support teams that are new to Prometheus, Grafana, or Dynatrace?
8. How do you measure adoption and maturity of observability practices across teams?
9. How do you resolve ownership confusion when multiple teams rely on the same telemetry pipeline?
10. How do you design an internal observability platform roadmap?
11. How do you prioritize developer enablement, governance, and platform reliability work?
12. How do you handle resistance from teams that see observability as overhead?
13. How do you evangelize good dashboarding and alerting habits across the organization?
14. How do you scale observability knowledge transfer across many services and teams?
15. How do you prevent observability from becoming fragmented by team-specific conventions?
16. How do you define success metrics for a centralized observability platform?
17. How do you build trust in platform-provided dashboards and alerts?
18. How do you support both deep technical users and less specialized consumers of observability data?
19. How do you guide teams toward better SLOs, better telemetry, and better incident behavior?
20. How do you lead observability change in a product organization with competing priorities?

---
17. Bar-raiser troubleshooting and war-room scenarios
1. A customer impact is reported, but Prometheus dashboards are green, Grafana panels are normal, and Dynatrace detects no anomaly. What would you check first, and why?
2. A Kubernetes rollout coincides with missing metrics, stale time series, and incomplete target discovery. How would you isolate whether this is a telemetry issue or an application issue?
3. Prometheus remote write starts backing up during a traffic spike, and downstream long-term storage shows data gaps. What would you investigate immediately?
4. Grafana dashboards show data for some clusters but not others after a permissions change. How would you troubleshoot this under incident pressure?
5. One Dynatrace OneAgent rollout causes host overhead complaints on critical nodes. How would you determine whether to pause, rollback, or tune the deployment?
6. A service’s latency SLO is healthy, but user complaints indicate severe performance problems. How would you reconcile the mismatch?
7. Duplicate alerts are paging multiple teams because Prometheus HA replicas disagree. What would you check first, and what would you change afterward?
8. A relabeling change causes an explosion of missing alerts and unstable target labels. How would you identify the breakage path?
9. A synthetic check continues to pass while real-user experience is degraded in one geography. How would you trace the issue?
10. Logs contain errors, traces are partial, and metrics are delayed. How would you proceed if only one signal seems trustworthy?
11. A dashboard built by another team is broken during a production issue, but the underlying service is healthy. What do you do next?
12. A new metric rollout causes high cardinality and rapidly increasing storage cost. How would you contain the blast radius?
13. A multi-cluster service has observability in two regions but is blind in the third. How would you find the gap quickly?
14. An alert is technically correct but produces pager fatigue every night. How would you redesign it?
15. A team claims the observability platform is the bottleneck, while the platform team believes the service is failing. How would you arbitrate?
16. The same incident appears differently in Grafana, Prometheus, and Dynatrace. How would you determine which source is most trustworthy?
17. A release removes a trace context header and breaks correlation across services. How would you detect and prevent this pattern?
18. A compliance review blocks telemetry centralization for one business unit. How would you preserve troubleshooting capability?
19. During a regional outage, metrics and traces are delayed, but logs are arriving. How would you adapt your triage approach?
20. A production incident is resolved, but the observability trail is too weak to explain root cause confidently. What would you improve first?

---
18. Mock panel round questions grouped by architect round, platform/DevOps round, SRE/operations round, security/compliance round, and hiring manager round
Architect round
1. How would you design an enterprise observability architecture for 200+ services across Kubernetes, VMs, and managed cloud services?
2. How do you decide the right balance between centralized telemetry architecture and team autonomy?
3. How would you design telemetry pipelines to support metrics, logs, traces, synthetics, and topology at scale?
4. What is your framework for choosing between Prometheus/Grafana, Dynatrace, or a hybrid model?
5. How would you standardize observability for a product company without reducing engineering velocity?

Platform / DevOps round
1. How would you operate Prometheus at scale across multiple clusters and regions?
2. How do you manage scrape configuration, relabeling, retention, and remote write safely?
3. How would you govern Grafana dashboards, data sources, and access at enterprise scale?
4. How do you roll out instrumentation and alerting changes through CI/CD or GitOps?
5. What would you do to prevent dashboard sprawl, broken templates, and alert duplication?

SRE / operations round
1. How do you detect and troubleshoot missing or delayed telemetry during an outage?
2. What signals would you trust first when dashboards conflict with user reports?
3. How do you design alerting that is useful during on-call and during incidents?
4. How would you investigate cardinality explosions, stale series, and scrape failures?
5. How do you run a war room when multiple observability tools show inconsistent pictures?

Security / compliance round
1. How do you protect sensitive telemetry while preserving debugging value?
2. How do you design observability access control for multi-tenant or multi-business-unit environments?
3. How do you handle data residency, retention, and export constraints in observability platforms?
4. How do you prevent observability tooling from exposing secrets or regulated data?
5. How do you reconcile security restrictions with production troubleshooting requirements?

Hiring manager round
1. How do you define what “great” looks like for an observability platform in a product company?
2. How do you influence engineering teams to adopt consistent observability practices?
3. How do you prioritize observability investments against product delivery pressure?
4. How do you build trust between platform teams and product teams around telemetry and incident response?
5. How would you lead the observability function if you had to improve reliability, reduce cost, and increase developer adoption at the same time?