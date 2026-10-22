# GCP Study Notes — Week 5 v2
## Observability, FinOps, Security Operations, Reliability Engineering, and ML Operational Governance

> This is the **9.5+ version** of Week 5.  
> It is written as a **deep expert-level technical guide** with:
> - technical purpose
> - core entities
> - operational mechanics
> - architecture patterns
> - examples
> - best practices
> - common mistakes
> - troubleshooting matrices
> - official references  
>
> **Noise control:** tight structure, high information density, minimal filler.

---

# 1. Why Week 5 Matters

Week 5 is about **how Google Cloud systems are observed, optimized, defended, and operated under stress**.

If:
- Week 1 = governance
- Week 2 = networking
- Week 3 = execution and delivery
- Week 4 = storage and data services

then Week 5 = **operational truth**.

This is where platform maturity becomes visible:
- not when services are deployed
- but when they degrade
- when they get expensive
- when alerts fire at 2:00 AM
- when a security finding arrives with incomplete context
- when a model silently drifts
- when the provider itself has an incident

The core idea is:

> **Good cloud operations unify telemetry, reliability targets, cost intelligence, security posture, provider-health awareness, and response workflows into one disciplined operating model.**

A weak Week 5 posture has:
- dashboards without owners
- metrics without clear labels or retention value
- logs with no routing design
- alerts that page for noise
- security findings with no triage queue
- billing exports with no decisions tied to them
- model monitoring with no rollback or retrain path

A strong Week 5 posture has:
- signal quality
- ownership
- runbooks
- review cadence
- severity models
- reduction of noise
- evidence-preserving incident flow

---

# 2. Learning Outcomes

By the end of Week 5, you should be able to:

- explain **Cloud Monitoring** metrics, monitored resources, aggregation and alignment concepts, dashboards, alert policies, notification channels, uptime checks, SLI/SLO thinking, metrics scopes, and billable metrics governance
- explain **Cloud Logging** buckets, views, retention, sinks, routing, exclusions, aggregated exports, structured logging strategy, and log-based metrics
- explain **Error Reporting**, **Cloud Trace**, and **Cloud Profiler** with more operator-level precision
- explain **Managed Service for Prometheus** and **Managed Service for Grafana** as production observability layers, including cardinality control, PromQL use, managed collection, and dashboard governance
- explain **Network Intelligence Center** and **Personalized Service Health** as operational accelerators
- explain **Cloud Billing**, **billing export to BigQuery**, **budgets**, **showback/chargeback inputs**, **FinOps hub**, **Recommender**, and **Active Assist**
- explain **Security Command Center**, **Event Threat Detection**, **Policy Intelligence**, and **Google Security Operations**, including finding lifecycle and case-oriented response
- explain **Vertex AI Model Monitoring**, **Vertex AI Experiments**, **ML Metadata**, and **model evaluation** as ML governance and operational controls
- design telemetry, alerting, incident-routing, cost-optimization, and security-investigation workflows that reduce mean time to detect and mean time to understand

---

# 3. Architectural Mental Model

Think in layers:

1. **Signal generation**  
   metrics, logs, traces, profiles, audit events, service health events

2. **Signal shaping**  
   alignment, aggregation, filtering, routing, label design, retention, views

3. **Decisioning**  
   dashboards, alerts, SLOs, budget thresholds, recommendations, detections

4. **Response**  
   on-call routing, case creation, investigation, rollback, escalation, postmortem

5. **Optimization**  
   cost review, recommender actions, noise reduction, metric hygiene, model retraining triggers

The Week 5 principle is:

> **Raw telemetry is not observability. Observability begins only when signals are shaped, trusted, routed, and acted on.**

---

# 4. Operations Design Principles

## 4.1 A signal hierarchy matters
Good operators distinguish:
- **synthetic/user-experience signals**  
- **application health signals**
- **infrastructure signals**
- **provider health signals**

This order matters.  
CPU alone does not tell you whether users are failing.

## 4.2 Alerting must encode intent
Every alert should answer:
- what symptom is observed?
- what scope is affected?
- what severity does this imply?
- who should respond?
- where should the responder look first?

## 4.3 Telemetry must be queryable
If metric names, log fields, labels, traces, or dashboards are inconsistent, operators lose time translating the system before investigating it.

## 4.4 Cost is a reliability-adjacent signal
A runaway scale event, high-cardinality metric explosion, duplicate logging pipeline, or unexpected model-serving burst is both a cost problem and an operational problem.

## 4.5 Security findings are part of operations
Cloud security findings are not separate from platform operations.  
They are part of:
- change review
- blast-radius assessment
- access review
- incident handling

## 4.6 Model operations are operations
ML systems need:
- evaluation evidence
- deployment lineage
- drift alerts
- rollback/retrain paths
just like ordinary services need health checks and rollbacks.

---

# 5. Cloud Monitoring

Cloud Monitoring is the primary metrics, alerting, and service-observability platform.

Google documents Cloud Monitoring, metrics scopes, billable metrics visibility through Metrics Management, and uptime check configuration. [1][2][3][4]

## 5.1 Technical purpose

Use Cloud Monitoring for:
- platform and app metrics
- dashboards
- alert policies
- uptime checks
- SLI/SLO operations
- fleet and cross-project visibility

## 5.2 Core entities

### Metric
A named time-series signal.

### Metric type
The canonical identity of a metric.

### Monitored resource
The resource model attached to the metric.

### Label
A metric dimension used for slicing, grouping, and filtering.

### Time series
A stream of values for one metric-resource-label combination.

### Dashboard
A visualization and investigation surface.

### Alerting policy
Condition evaluation with channel routing.

### Notification channel
Destination for alert delivery.

### Metrics scope
Defines what projects’ metrics are visible together.

### Uptime check
Synthetic reachability/availability probe.

### SLO / error budget
Reliability objective and tolerated failure budget.

### Metrics Management
Controls and visibility for metric usage and billable metric footprint.

---

## 5.3 Alignment, aggregation, and alert semantics

This is one of the most important operator topics.

### Alignment
Turns raw points into regularized intervals over a chosen window.

### Aggregation
Combines multiple time series into grouped or reduced results.

### Why this matters
Many “bad alerts” are actually:
- wrong window
- wrong grouping
- wrong reduction
- wrong comparison target

### Example
A service has many replicas.  
If you alert on per-instance CPU spikes with no grouping strategy, one noisy replica pages everyone.  
If you aggregate too aggressively, you can hide one hot shard that is actually hurting users.

### Best practices
- choose windows that match failure speed
- group by the label that preserves operational meaning
- reduce only after deciding whether outliers matter
- test alert behavior against real incidents

### Common mistakes
- average across everything
- no grouping by service or region
- too-short evaluation windows
- threshold copied from tutorials with no workload evidence

---

## 5.4 Synthetic, app, and infra signals

### Synthetic signal
“What do users see?”

### Application signal
“What is the service doing?”

### Infrastructure signal
“What are hosts, runtimes, or dependencies doing?”

### Best practice
Page first on synthetic or strong application-failure symptoms when possible, not only host metrics.

### Common mistake
CPU or memory alerts become primary incident triggers even when user traffic is fine.

---

## 5.5 Metrics scopes

Google documents metrics scopes as the mechanism for multi-project observability. [1]

### Best practices
- central monitoring projects per org or major domain
- document scope boundaries
- separate visibility from edit rights where needed
- ensure critical platform services are visible in one operational plane

### Common mistakes
- fragmented project-by-project monitoring
- “shared dashboard” exists but half the metrics are invisible due to scope gaps

---

## 5.6 SLI/SLO mechanics

### SLI
A measurable reliability indicator.

### SLO
A target over time for that indicator.

### Error budget
How much failure is tolerated before action is required.

### Best practices
- define SLOs for user-facing critical paths
- choose SLIs that reflect user success, latency, or freshness
- use SLOs in change policy and incident review
- burn-rate-based alerting is often more meaningful than static thresholds

### Common mistakes
- SLO defined on host metrics
- no connection between SLO misses and release/change decisions
- only aggregate success rate tracked while slow failures are ignored

---

## 5.7 Uptime checks

Google documents uptime checks and how to manage them. [4]

### Best practices
- use for public and critical internal endpoints
- include important protocol assumptions
- create business-flow synthetic checks for high-value paths
- place checks in a wider service-health model, not as sole truth

### Common mistakes
- checking only LB reachability
- passing check while auth path, DB path, or downstream dependency is broken
- no region-aware or path-aware check design

---

## 5.8 Metrics Management and billable metrics

Google documents Metrics Management for visibility into billable metric use. [3]

### Best practices
- review metric usage monthly
- remove unused custom metrics
- cap label cardinality
- treat metrics as productized assets with ownership

### Common mistakes
- metrics emitted for debugging forever
- high-cardinality labels such as request IDs or user IDs
- no one knows which custom metrics drive costs

---

## 5.9 Cloud Monitoring troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Dashboard empty | scope/query | metrics scope, project selection, monitored resource type, filters |
| Alert never fires | alert semantics | alignment window, aggregation, threshold, missing labels |
| Alert too noisy | signal design | grouping, reduction, window length, rollout behavior |
| SLO seems healthy while users complain | SLI mismatch | wrong SLI definition, poor latency threshold, sampling gap |
| Billable metric cost spike | telemetry hygiene | custom metrics volume, label cardinality, unused dashboards/alerts |

---

# 6. Cloud Logging

Cloud Logging is the log ingestion, storage, routing, and export plane.

Google documents log buckets, routing, sinks, and log-based metrics. [5][6][7]

## 6.1 Technical purpose

Use Cloud Logging for:
- centralized app and platform logs
- audit visibility
- incident investigation
- compliance retention
- export to analytics or downstream SIEM
- metric derivation from event patterns

## 6.2 Core entities

### Log entry
Structured or unstructured log record.

### Log bucket
Storage boundary with retention and access configuration.

### Log router
System that routes entries to destinations.

### Sink
Export rule for selected log entries.

### Exclusion
Rule that suppresses matching logs from a path.

### Log view
Scoped visibility into a bucket.

### Log-based metric
Metric created from matched log entries.

---

## 6.3 Centralized logging architecture

A mature logging design often uses:
- local project ingestion
- centralized security or audit buckets
- analytics export to BigQuery for selected domains
- SIEM export for security feeds
- restricted views for least-privilege access

### Best practices
- separate operational, audit, and long-term retention needs
- document centralized sink topology
- review destination permissions for sink writer identities
- use structured logging across application fleets

### Common mistakes
- every team exports logs differently
- central bucket exists but no views/least-privilege design
- duplicate exports to many destinations with no reason

---

## 6.4 Structured logging discipline

### Technical role
Structured fields make logs queryable and correlate better with metrics/traces.

### Good fields
- severity
- service name
- environment
- request or correlation ID
- error class
- dependency identifier
- user-safe context

### Bad practice
- huge text blobs only
- inconsistent field names between services
- sensitive data leaked into free text

### Best practices
- standardize logging schema
- document required fields
- correlate with trace IDs where possible
- separate app diagnostics from audit/security context

### Common mistakes
- every team logs differently
- unstructured stack traces only
- PII or secrets accidentally logged

---

## 6.5 Buckets, views, retention, and access

### Best practices
- short retention for noisy low-value logs
- longer retention for audit, incident, or compliance-relevant logs
- use log views for scoped access
- protect audit and admin activity logs carefully

### Common mistakes
- one retention policy for all logs
- security teams cannot access what they need
- developers can read sensitive logs they do not need

---

## 6.6 Routing, sinks, and writer identities

Google documents sinks and routing destinations including Pub/Sub, BigQuery, Cloud Storage, and other buckets. [6]

### Important operational point
Many sink failures are permission failures, not filter failures.

### Best practices
- test sink with real sample logs
- verify writer identity permissions on destination
- document aggregated sinks and scope clearly
- filter narrowly for expensive destinations like BigQuery/SIEM

### Common mistakes
- sink created but export silently fails due to destination IAM
- aggregated sink accidentally exports too much
- security logs routed without data-handling review

---

## 6.7 Log-based metrics

Google documents project-scoped and bucket-scoped log-based metrics. [7]

### Best practices
- use for discrete event counting or error-rate derivation
- prefer structured fields for filter reliability
- keep log-based metrics low-cardinality and meaningful

### Common mistakes
- metric derived from noisy free text
- attempting to model complex analytics in log-based metrics instead of BigQuery

---

## 6.8 Cloud Logging troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Central export missing data | routing/IAM | sink filter, aggregated scope, writer identity permissions |
| Query works in one project not another | storage/view | bucket location, view permissions, routing path |
| BigQuery log export cost high | routing strategy | exported volume, duplicate sinks, verbose app logs |
| Log-based metric flat | metric scope | bucket vs project scope, filter fields, structured log schema |
| Sensitive data found in logs | app logging design | field sanitization, schema discipline, logger configuration |

---

# 7. Error Reporting, Cloud Trace, and Cloud Profiler

These tools answer different investigation questions.

---

## 7.1 Error Reporting

### Technical purpose
Group recurring application errors into actionable issue sets.

### Operational use
- identify new vs recurring failures
- estimate volume and trend
- prioritize fixes by service and impact

### Best practices
- standardize exception logging
- preserve stack traces
- map error groups to service ownership
- route new high-severity error groups into engineering review

### Common mistakes
- generic error messages collapse unrelated faults
- stack traces lost in formatting
- no team owns error groups

---

## 7.2 Cloud Trace

Google documents Cloud Trace, tracing with OpenTelemetry, span data model, and trace exploration. [8][9][10][11]

### Technical purpose
Follow latency and failure across service boundaries.

### Core entities
- trace
- span
- parent-child relationship
- attribute
- sampling decision

### Sampling tradeoff
Too little sampling hides incidents.  
Too much sampling raises volume and noise.

### Best practices
- instrument critical request paths end to end
- capture dependency spans: DB, RPC, cache, queue, external APIs
- standardize key attributes
- correlate trace IDs with logs
- adjust sampling strategically for critical services

### Common mistakes
- only ingress span exists
- no downstream visibility
- trace context lost across async boundaries
- trace volume too low to investigate real incidents

---

## 7.3 Cloud Profiler

### Technical purpose
Find sustained CPU and memory hotspots.

### Best fit
- slow code-path analysis
- production tuning
- identifying allocation or hot-loop issues

### Best practices
- use for services that are important and persistent enough to profile meaningfully
- pair profiler findings with trace latency evidence
- treat performance work as continuous operational improvement

### Common mistakes
- using profiler during every short-lived incident without enough sample history
- optimizing hot code paths that are not user-impactful

---

## 7.4 Trace and profiler troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Users slow, traces unhelpful | instrumentation | missing spans, lost context, low sampling |
| One service blamed but root cause elsewhere | trace topology | child spans, downstream dependency timings |
| CPU high with no obvious request bottleneck | profiler/runtime | hot functions, memory churn, background tasks |
| Error volume rising but no grouped view | error pipeline | structured exception logging, service context |

---

# 8. Managed Service for Prometheus and Managed Service for Grafana

Google documents Managed Service for Prometheus with managed collection, PromQL, metrics scopes, and Grafana integration. [12][13][14][15]

## 8.1 Managed Service for Prometheus

### Technical purpose
Support Prometheus-based monitoring at cloud-managed scale.

### Core entities
- scrape target
- managed collection
- PromQL query
- recording or alert rule
- metrics scope
- dashboard data source

### Best practices
- curate exporters and scrape configs
- eliminate high-cardinality labels early
- convert critical repeated queries into recording patterns where appropriate
- define ownership for rules and exporters

### Common mistakes
- importing every exporter blindly
- label explosion from pod/container/request dimensions
- rule files copied from community examples with no tuning

### Operational nuance
Prometheus compatibility is useful, but it does not remove the need for:
- scope design
- naming standards
- retention/cost review
- alert ownership

---

## 8.2 Managed Service for Grafana

### Technical purpose
Provide shared dashboards and visualization over Prometheus and other observability sources.

### Best practices
- treat dashboards as operational products
- use folder/team governance
- distinguish exploratory dashboards from production dashboards
- source-control critical dashboards and alert definitions where possible

### Common mistakes
- dashboard sprawl
- one-off panels with no owner
- copied dashboards that look good but do not reflect local architecture

---

## 8.3 Prometheus/Grafana troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| No PromQL data in shared dashboard | scope/auth | metrics scope, data source permissions, project visibility |
| Exporter up but metrics missing | collection | scrape config, target discovery, labels, workload permissions |
| Alert rule flaps | metric stability | cardinality, evaluation interval, stale targets, bad expression |
| Grafana team can’t see dashboard data | access model | data source auth, workspace permissions, scope mismatch |

---

# 9. Network Intelligence Center and Personalized Service Health

These tools are critical for distinguishing:
- self-caused network issues
- architecture issues
- provider-impact issues

Google documents Network Intelligence Center and Personalized Service Health for these use cases. [16][17][18][19]

## 9.1 Network Intelligence Center

### Technical purpose
Analyze connectivity and network posture with path-aware tooling.

### Core entities
- Connectivity Tests
- Network Analyzer
- Firewall Insights
- Performance Dashboard or performance view

### Best practices
- predefine tests for critical traffic paths
- review analyzer insights continuously
- use performance views in cross-region planning and incident triage
- include NIC tools in network runbooks

### Common mistakes
- debugging only from VM shells
- not preserving test baselines for critical services
- ignoring topology insights until outage

---

## 9.2 Personalized Service Health

### Technical purpose
Provide relevant provider incident visibility for your actual resources.

### Best practices
- route PSH into incident workflow
- distinguish provider-impact events from internal regressions early
- grant viewer access to incident managers and platform responders
- correlate PSH with Cloud Logging and Monitoring timelines

### Common mistakes
- service health only checked manually in console
- no alert path for relevant provider incidents
- provider incident confused with app deployment issue

---

## 9.3 NIC and PSH troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Traffic path broken only for one service | path analysis | Connectivity Test source/destination, recent route/firewall changes |
| Latency higher across regions | provider or topology | performance view, PSH events, recent regional deploys |
| Teams debate “Google issue or our issue?” | incident triage | PSH relevance, metrics timeline, trace/log evidence, recent config changes |

---

# 10. Cloud Billing, Billing Export to BigQuery, Budgets, FinOps Hub, Recommender, and Active Assist

Cost control is part of operational discipline.

Google documents billing export to BigQuery, FinOps hub, and Active Assist. [20][21][22][23]

## 10.1 Billing export to BigQuery

### Technical purpose
Turn cloud spend into operationally analyzable data.

### Best practices
- enable export early
- standardize labels/tags for service, owner, environment, cost center
- publish shared cost views for engineering and finance
- retain pricing and usage context together where useful

### Showback vs chargeback
- **showback** = visibility without direct cost transfer
- **chargeback** = formal allocation of cost to owners

Billing export supports both, but only if labels/tags and project structure are disciplined.

### Common mistakes
- no tagging strategy
- export exists but engineering cannot query it
- cost spikes are seen after the fact with no operational dashboard

---

## 10.2 Budgets

### Technical purpose
Threshold-based notification for actual and forecast spend.

### Best practices
- create budgets at useful scopes
- send alerts to both budget owners and operators
- combine with FinOps review cadence
- use budgets as trigger for investigation, not as sole cost program

### Common mistakes
- global budget only
- email-only alert with no operational response
- thresholds too late to influence spend

---

## 10.3 FinOps hub, Recommender, and Active Assist

Google documents FinOps hub as the unified cost optimization dashboard and Active Assist as the broader recommendations portfolio. [21][22][23]

### Best practices
- review recommendations regularly with workload owners
- track accepted, rejected, and deferred recommendations
- validate impact before acting on performance-sensitive workloads
- treat recommendations as decision support, not automation truth

### Commitment-aware optimization
Some recommendations interact with:
- committed use posture
- recent scale events
- seasonal workloads
- reliability objectives

### Common mistakes
- treating every saving suggestion as immediately safe
- rejecting recommendations with no rationale
- savings estimated but not measured post-change

---

## 10.4 FinOps troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Cost spike unexplained | billing data model | labels/tags, detailed export freshness, top service/project deltas |
| Savings recommendation seems unsafe | workload context | SLO sensitivity, seasonal load, commitments, performance margins |
| Budget alert not actionable | governance | recipient list, budget scope, threshold timing, runbook |
| Teams argue about “who owns cost” | allocation model | project mapping, labels/tags, showback/chargeback design |

---

# 11. Security Command Center, Event Threat Detection, Policy Intelligence, and Google Security Operations

This is the security-operations spine of Week 5.

Google documents SCC, ETD, Policy Intelligence, and Google SecOps. [24][25][26][27][28][29][30][31]

## 11.1 Security Command Center (SCC)

### Technical purpose
Centralize cloud security findings, posture issues, and prioritized risk context.

### Core entities
- finding
- source
- asset
- severity
- category/type
- mute state
- exposure or attack-path context

### Finding lifecycle
A mature SCC operating model needs:
1. ingest
2. classify
3. enrich with asset criticality
4. assign owner
5. investigate
6. mute or resolve
7. review recurring themes

### Best practices
- define triage SLAs by severity
- enrich critical assets with ownership context externally if needed
- distinguish posture findings from active threats
- use mute state intentionally with documented reasons

### Common mistakes
- no lifecycle model
- “resolved” means ignored
- repeated benign findings never muted systematically
- active threat findings mixed with minor hygiene findings in one queue

---

## 11.2 Event Threat Detection (ETD)

Google documents ETD as near-real-time threat detection writing findings to SCC. [25]

### Technical purpose
Detect suspicious behavior patterns quickly and feed investigation.

### Best practices
- map high-confidence ETD findings to immediate triage
- correlate ETD with logs and identity events
- define playbooks by finding family
- separate informational detections from pager-worthy detections

### Common mistakes
- every ETD finding treated equally
- no enrichment with resource criticality
- no tuning or mute workflow for repeated known-benign events

---

## 11.3 Policy Intelligence

Google documents Policy Intelligence and Policy Analyzer for IAM and organization-policy analysis. [26][27][28]

### Technical purpose
Answer:
- who can do what?
- why can they do it?
- where is the policy set?
- what changed effective access?

### Best practices
- use during access reviews and incident response
- include impersonation and inheritance thinking
- export large analyses when scale demands it
- pair access questions with logs and SCC context

### Common mistakes
- checking one IAM page and concluding access cause
- ignoring organization policy inheritance
- troubleshooting access manually without analyzer tools

---

## 11.4 Google Security Operations (Google SecOps)

Google documents Google SecOps around ingestion, parser normalization to UDM, detection content, analytics, alerts, and cases. [29][30][31]

### Technical purpose
Provide SIEM/SOC-scale telemetry analysis, detection, investigation, and response workflow.

### Core entities
- feed
- parser
- UDM event
- detection
- alert
- case
- rule/playbook content

### Parser quality matters
If parsing is poor:
- detections misfire
- enrichment weakens
- cases lack context
- analysts lose trust

### Best practices
- onboard data sources by priority
- validate parser normalization with sample events
- tune detections before wide paging
- use case management to preserve evidence and workflow

### Common mistakes
- ingest all feeds first, tune later
- detections enabled with no content ownership
- case queues fill with low-context alerts

---

## 11.5 Security-operations troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| SCC backlog grows endlessly | triage lifecycle | severity rules, mute policy, asset criticality, ownership |
| ETD alert lacks usable context | enrichment | Cloud Logging correlation, asset mapping, identity context |
| Access issue still unexplained | policy analysis | inherited IAM, org policies, impersonation, analyzer scope |
| SecOps alert volume explodes | detection/parsing | parser normalization, noisy rules, suppression logic, feed quality |
| Analyst trust in SIEM falls | content quality | parser validation, detection precision, case enrichment, feedback loop |

---

# 12. Vertex AI Model Monitoring, Experiments, ML Metadata, and Model Evaluation

This is the ML operations and governance layer.

Google documents model-monitoring schema handling, ML Metadata data model, experiments comparison journeys, and model evaluation resources. [32][33][34][35]

## 12.1 Model Monitoring

### Technical purpose
Detect when production inputs or outcomes diverge from expected behavior.

### Core concepts
- baseline
- skew
- drift
- threshold
- monitoring schedule/job
- alert route

### Drift vs skew
- **skew** = difference between training-time and serving-time input distributions
- **drift** = change in production data distribution over time

### Best practices
- define stable baselines deliberately
- monitor features that matter operationally
- route alerts to model owners and platform teams where appropriate
- connect model monitoring to retrain or rollback decisions

### Common mistakes
- random baseline with no documentation
- alert thresholds never reviewed
- drift detected but no response path exists

---

## 12.2 Vertex AI Experiments

### Technical purpose
Track training runs, parameters, metrics, and comparisons.

### Best practices
- standardize experiment naming
- log parameters and outcomes consistently
- link experiment outputs to approval and deployment stages
- avoid notebook-only experiment history

### Common mistakes
- one-off runs not captured
- deployed model cannot be tied back to experiment evidence

---

## 12.3 ML Metadata

Google documents artifacts, executions, contexts, and metadata schemas. [33]

### Technical purpose
Preserve lineage across data, training, evaluation, and deployment.

### Best practices
- capture lineage for every production model
- use metadata to answer “what changed?”
- preserve dataset, feature, model, and deployment relationships

### Common mistakes
- model deployed with no traceable artifact lineage
- no context linking dataset version to evaluation and deployment

---

## 12.4 Model evaluation

### Technical purpose
Provide quantitative evidence before promotion.

### Best practices
- use slice-based evaluation, not only global aggregates
- keep evaluation datasets versioned
- align metrics to business outcomes
- compare candidate model versus incumbent before rollout

### Common mistakes
- single score used as approval gate
- evaluation disconnected from production outcomes
- no rollback threshold after deployment

---

## 12.5 ML operations troubleshooting matrix

| Symptom | Likely layer | First things to inspect |
|---|---|---|
| Production quality worsens, offline metrics still good | monitoring/eval mismatch | slice coverage, drift/skew, baseline, label delay |
| Nobody knows why model was approved | governance | experiment logs, metadata lineage, evaluation artifacts |
| Drift alert fires constantly | threshold/baseline | unstable baseline, overly sensitive threshold, seasonal data |
| Model rollback impossible | release process | incumbent model tracking, registry/version linkage, deployment history |

---

# 13. Reliability Engineering Patterns

Week 5 should also be studied as reliability engineering.

## 13.1 Golden-signal dashboard pattern
For each critical service:
- latency
- error rate
- traffic
- saturation
- dependency health
- rollout markers
- cost anomalies where relevant

## 13.2 Alert hierarchy pattern
- page on user-impact or fast-burn SLO conditions
- notify on concerning but nonurgent degradation
- dashboard-only for informational telemetry

## 13.3 Logging pattern
- structured logs
- trace correlation
- retention by value
- exports by business need
- log-based metrics only where useful

## 13.4 Security triage pattern
- SCC intake
- ETD escalation
- Policy Intelligence for access questions
- SecOps case for coordinated investigation

## 13.5 FinOps review pattern
- billing export dashboards
- monthly optimization review
- recommendation tracking
- architecture review for large spend deltas

## 13.6 ML governance pattern
- experiment evidence
- evaluation gate
- deployment lineage
- monitoring thresholds
- retrain/rollback path

---

# 14. End-to-End Incident Example

A customer-facing API suddenly shows rising latency and intermittent failures.

A strong Week 5 operator path is:

1. **Cloud Monitoring**
   - synthetic check latency up
   - error budget burn increasing
   - service dashboard shows dependency call error increase

2. **Cloud Logging**
   - structured logs show timeout and retry storm patterns

3. **Cloud Trace**
   - latency accumulates in downstream database spans

4. **Cloud Profiler**
   - no major CPU regression in application code

5. **Cloud Billing / FinOps**
   - no abnormal traffic-cost jump that would suggest unexpected load surge

6. **NIC**
   - no route/firewall break or provider-network issue visible

7. **PSH**
   - no relevant Google provider event

8. **SCC / ETD**
   - no threat event suggesting abuse or compromise path

Result:
- most likely downstream data-service regression or application retry amplification

This is the Week 5 goal:
**rapid narrowing of uncertainty using trusted signals**.

---

# 15. Troubleshooting Flow

When incidents happen, inspect in this order:

1. **Validate symptom**
   - real user issue, synthetic issue, or noisy alert?

2. **Define blast radius**
   - one service, one region, many projects, or provider-wide?

3. **Correlate signals**
   - metrics, logs, traces, PSH, SCC if security-relevant

4. **Separate layers**
   - app, platform, network, provider, security, or cost anomaly?

5. **Take safe action**
   - rollback, failover, mute, scale, isolate, or escalate

6. **Preserve evidence**
   - logs, traces, findings, cases, billing/export snapshots, timeline notes

7. **Feed back**
   - update alert, dashboard, runbook, mute rule, or evaluation threshold after incident

---

# 16. Common Week 5 Mistakes

- treating metrics as free and infinite
- no alignment/aggregation discipline in alerts
- logs routed everywhere with no retention strategy
- sink IAM never tested
- tracing without downstream spans
- Grafana dashboard sprawl
- provider incident monitoring left manual
- billing export enabled but not operationalized
- recommendations accepted without workload context
- SCC triage with no mute policy
- ETD findings with no playbook
- parser normalization ignored in SecOps
- model monitoring thresholds never revisited
- evaluation not tied to approval
- postmortems do not improve telemetry

---

# 17. Revision Checklist

You should be able to explain clearly:

- how alignment, aggregation, grouping, and windows affect Monitoring alerts
- how synthetic, app, infra, and provider signals should be prioritized
- how Logging buckets, views, sinks, writer identities, and structured logging fit together
- how Error Reporting, Trace, and Profiler answer different operator questions
- how Managed Service for Prometheus and Grafana should be governed in production
- how to use NIC and PSH to separate self-caused outages from provider-impact events
- how billing export, budgets, showback/chargeback, FinOps hub, and Active Assist fit together
- how SCC, ETD, Policy Intelligence, and SecOps form a security-operations pipeline
- how model monitoring, experiments, metadata, and evaluation form ML governance
- how to design alerts and runbooks that reduce mean time to understand

---

# 18. Memory Anchors

- **Monitoring** = metrics, alerts, dashboards, SLOs
- **alignment** = regularize points over time window
- **aggregation** = combine/group time series
- **metrics scope** = cross-project visibility
- **Logging** = ingest, store, route, export logs
- **log bucket** = retention boundary
- **sink** = export rule
- **log view** = scoped access view
- **structured logging** = queryable logs
- **Error Reporting** = grouped app failures
- **Trace** = request path timing
- **Profiler** = code hotspot sampling
- **Managed Prometheus** = Prometheus-compatible managed metrics
- **Grafana** = shared visualization/query layer
- **NIC** = network diagnostics
- **PSH** = relevant provider health feed
- **Billing export** = cost data in BigQuery
- **FinOps hub** = optimization dashboard
- **Active Assist** = recommendations portfolio
- **SCC** = cloud security findings
- **ETD** = near-real-time threat findings
- **Policy Intelligence** = effective access analysis
- **SecOps** = SIEM/SOC telemetry and cases
- **model drift** = production distribution changes over time
- **model skew** = training vs serving mismatch
- **ML Metadata** = lineage
- **model evaluation** = promotion evidence

---

# 19. Official References

## Monitoring and Logging
- Cloud Monitoring overview  
  https://docs.cloud.google.com/monitoring/docs/monitoring-overview

- Collect metrics overview  
  https://docs.cloud.google.com/monitoring/docs/collect-metrics-overview

- Metrics Management  
  https://docs.cloud.google.com/monitoring/docs/metrics-management

- Configure log buckets  
  https://docs.cloud.google.com/logging/docs/buckets

- Route log entries  
  https://docs.cloud.google.com/logging/docs/routing/overview

- Log-based metrics overview  
  https://docs.cloud.google.com/logging/docs/logs-based-metrics

## Trace and profiling
- Cloud Trace overview  
  https://docs.cloud.google.com/trace/docs/overview

- Instrument for Cloud Trace  
  https://docs.cloud.google.com/trace/docs/setup

- Traces and spans  
  https://docs.cloud.google.com/trace/docs/traces-and-spans

- Find and explore traces  
  https://docs.cloud.google.com/trace/docs/finding-traces

## Prometheus and Grafana
- Managed Service for Prometheus  
  https://docs.cloud.google.com/stackdriver/docs/managed-prometheus

- Get started with managed collection  
  https://docs.cloud.google.com/stackdriver/docs/managed-prometheus/setup-managed

- Query using Grafana  
  https://docs.cloud.google.com/stackdriver/docs/managed-prometheus/query

- Query using the Prometheus API or UI  
  https://docs.cloud.google.com/stackdriver/docs/managed-prometheus/query-api-ui

- Import Grafana dashboards into Cloud Monitoring  
  https://docs.cloud.google.com/stackdriver/docs/managed-prometheus/import-grafana-dashboards

## Network operations and provider health
- Network Intelligence Center overview  
  https://docs.cloud.google.com/network-intelligence-center/docs/overview

- Connectivity Tests overview  
  https://docs.cloud.google.com/network-intelligence-center/docs/connectivity-tests/concepts/overview

- Personalized Service Health overview  
  https://docs.cloud.google.com/service-health/docs/overview

- Personalized Service Health concepts  
  https://docs.cloud.google.com/service-health/docs/concepts

- Integrate with Personalized Service Health  
  https://docs.cloud.google.com/service-health/docs/how-to-integrate

- Manage Personalized Service Health access  
  https://docs.cloud.google.com/service-health/docs/manage-access

## FinOps and recommendations
- Export Cloud Billing data to BigQuery  
  https://docs.cloud.google.com/billing/docs/how-to/export-data-bigquery

- Set up Billing export to BigQuery  
  https://docs.cloud.google.com/billing/docs/how-to/export-data-bigquery-setup

- FinOps hub  
  https://docs.cloud.google.com/billing/docs/how-to/finops-hub

- Recommendations dashboard in FinOps hub  
  https://docs.cloud.google.com/billing/docs/how-to/finops-recommendations-dashboard

- What is Active Assist  
  https://docs.cloud.google.com/recommender/docs/whatis-activeassist

- Active Assist dashboard overview  
  https://docs.cloud.google.com/recommender/docs/active-assist/dashboard-overview

## Security operations
- Security Command Center overview  
  https://docs.cloud.google.com/security-command-center/docs/security-command-center-overview

- Vulnerability findings  
  https://docs.cloud.google.com/security-command-center/docs/concepts-vulnerabilities-findings

- Event Threat Detection overview  
  https://docs.cloud.google.com/security-command-center/docs/concepts-event-threat-detection-overview

- Use Event Threat Detection  
  https://docs.cloud.google.com/security-command-center/docs/how-to-use-event-threat-detection

- Policy Intelligence overview  
  https://docs.cloud.google.com/policy-intelligence/docs/overview

- Policy Analyzer for allow policies  
  https://docs.cloud.google.com/policy-intelligence/docs/policy-analyzer-overview

- Analyze allow policies  
  https://docs.cloud.google.com/policy-intelligence/docs/analyze-iam-policies

- Analyze organization policies  
  https://docs.cloud.google.com/policy-intelligence/docs/analyze-organization-policies

- Google SecOps overview  
  https://docs.cloud.google.com/chronicle/docs/secops/secops-overview

- Parsers overview  
  https://docs.cloud.google.com/chronicle/docs/event-processing/parsing-overview

- Cases overview  
  https://docs.cloud.google.com/chronicle/docs/soar/investigate/working-with-cases/cases-overview

## Vertex AI operational governance
- Model monitoring schemas  
  https://docs.cloud.google.com/vertex-ai/docs/model-monitoring/schemas

- Vertex ML Metadata data model  
  https://docs.cloud.google.com/vertex-ai/docs/ml-metadata/data-model

- Compare trained and evaluated models  
  https://docs.cloud.google.com/vertex-ai/docs/experiments/user-journey/uj-compare-models

- Vertex AI model evaluation sample  
  https://docs.cloud.google.com/vertex-ai/docs/samples/aiplatform-get-model-evaluation-sample

---

# 20. Final Summary

Week 5 is about **making Google Cloud systems observable, governable, cost-aware, and defensible in production**.

The strongest mental model is:

- **Monitoring + Logging + Trace + Profiler** = operational evidence
- **Prometheus + Grafana** = managed ecosystem compatibility and shared views
- **NIC + PSH** = network and provider-health acceleration
- **Billing export + FinOps hub + Active Assist** = cost-aware operations
- **SCC + ETD + Policy Intelligence + SecOps** = security operations lifecycle
- **model monitoring + experiments + metadata + evaluation** = ML governance

When you can explain the **signal model, alert model, cost model, security model, and response model** together, Week 5 is understood architecturally.
