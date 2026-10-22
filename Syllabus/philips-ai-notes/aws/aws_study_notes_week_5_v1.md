# AWS Study Notes — Week 5
## Observability, FinOps, and Operational Excellence

> This is the **deep technical Week 5 guide**.  
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

---

# 1. Why Week 5 Matters

Week 5 is about **running AWS well after deployment**.

If earlier weeks covered:
- governance
- networking
- compute
- data

then Week 5 covers the runtime questions:

- Is the system healthy?
- What changed?
- Where is latency coming from?
- Which dependency is failing?
- What does the incident timeline show?
- Are we spending efficiently?
- Which risks are operational, cost, resiliency, or security related?
- How do we turn raw telemetry into action?

The core Week 5 idea is:

> **Operational maturity comes from turning telemetry, findings, cost signals, and runbooks into repeatable decisions.**

This week spans four domains:

1. **Observability**  
2. **Operational automation**  
3. **FinOps and optimization**  
4. **Security and resiliency operations**

---

# 2. Week 5 Learning Outcomes

By the end of this week, you should be able to:

- explain **CloudWatch**, **X-Ray**, **Application Signals**, **Container Insights**, **Contributor Insights**, **Logs Insights**, and **Synthetics**
- explain **ADOT**, **Amazon Managed Service for Prometheus**, and **Amazon Managed Grafana**
- explain **Systems Manager** managed nodes, documents, Run Command, Session Manager, State Manager, Patch Manager, Automation, Parameter Store, and OpsItems
- explain **Well-Architected Tool** and **Resilience Hub**
- explain **Cost Explorer**, **Budgets**, **Cost Categories**, **CUR**, **Cost Anomaly Detection**, **Compute Optimizer**, and **Trusted Advisor**
- explain **GuardDuty**, **Inspector**, **Macie**, **Detective**, and **Security Lake**
- explain key **SageMaker** and **Bedrock** observability and governance entities for AI operations
- connect alarms, traces, dashboards, runbooks, findings, and cost controls into one operating model

---

# 3. Architectural Mental Model for Week 5

Think in layers:

1. **Telemetry collection**  
   Metrics, logs, traces, canaries, profiling signals

2. **Telemetry routing and storage**  
   CloudWatch, X-Ray, ADOT, Prometheus, Grafana

3. **Operational action**  
   Systems Manager, Automation, Patching, OpsItems

4. **Resilience review**  
   Well-Architected Tool, Resilience Hub

5. **Cost and efficiency**  
   Cost Explorer, Budgets, CUR, anomalies, rightsizing

6. **Security operations**  
   GuardDuty, Inspector, Macie, Detective, Security Lake

7. **AI operations**  
   SageMaker monitoring, lineage, bias/explainability, Bedrock logging and guardrails

A mature AWS operating model links all seven.

---

# 4. Amazon CloudWatch

CloudWatch is the central AWS observability service for metrics, logs, alarms, dashboards, synthetic monitoring, and service/application-level visibility.

## 4.1 Technical purpose

CloudWatch helps answer:
- what is happening now
- what changed
- what crossed a threshold
- what trend is emerging
- what service or workload is degraded

## 4.2 Core entities

### Metric
A time-series measurement.

### Namespace
Logical container grouping metrics, often by service or application domain.

### Dimension
A name/value attribute that identifies a metric stream variant.

### Alarm
A stateful evaluator over one or more metrics.

### Composite alarm
Alarm built from other alarms.

### Dashboard
Visual composition of metrics, logs, alarms, and widgets.

### Log group
Top-level container for log streams.

### Log stream
Sequence of log events from one source.

### Metric filter
Pattern-based extraction of metrics from logs.

### Contributor Insights rule
Rule that analyzes high-cardinality log or metric patterns to find top contributors.

### Logs Insights query
Interactive query over CloudWatch Logs.

### Synthetic canary
Scripted probe that continuously tests endpoints or user journeys.

### Application Signals service
Service-level entity used for application health, latency, dependency, and SLO-style visibility.

### Container Insights cluster/service metrics
Container-level telemetry aggregation for ECS, EKS, and other supported environments.

## 4.3 Metrics, namespaces, and dimensions

### Technical role
Metrics are the most common operational signal.

### Example
A custom namespace:
- `MyCompany/Payments`

Dimensions:
- `Environment=prod`
- `Service=payments-api`
- `Region=us-east-1`

### Best practices
- keep metric names stable
- use dimensions consistently
- control cardinality
- separate business KPIs from low-level host metrics

### Common mistakes
- exploding dimension cardinality with user IDs, request IDs, or session IDs
- naming metrics inconsistently across teams
- publishing too many near-duplicate metrics

---

## 4.4 Alarms and composite alarms

### Technical role
Alarms turn telemetry into action.

### Alarm states
- OK
- ALARM
- INSUFFICIENT_DATA

### Alarm actions
Common targets:
- SNS notifications
- Auto Scaling actions
- Incident workflows
- Automation triggers

### Composite alarms
Useful when you want fewer noisy alerts.

### Example
Trigger a critical incident only if:
- API 5xx alarm = ALARM
- latency alarm = ALARM
- deployment window suppression alarm = OK

### Best practices
- alarm on user impact and service health, not just infrastructure symptoms
- combine noisy low-level alarms using composite alarms
- align thresholds to actual SLO/SLA intent
- document alarm ownership and response actions

### Common mistakes
- too many threshold alarms with no triage value
- no runbook tied to alarms
- low-signal alarms paging humans unnecessarily

---

## 4.5 Dashboards

### Technical role
Dashboards provide shared operational context.

### Best practices
- organize by service, business flow, or incident function
- include golden signals: latency, traffic, errors, saturation
- show alarms and key dependencies together
- create separate executive vs engineer dashboards

### Common mistakes
- dashboards that look good but are not used during incidents
- mixing too many unrelated workloads into one board
- no consistent layout between services

---

## 4.6 CloudWatch Logs

### Technical role
CloudWatch Logs stores, queries, and analyzes application and service logs.

### Important controls
- retention policy
- encryption
- data protection policies
- subscription filters
- metric filters

### Example
A service writes structured JSON logs into one log group per environment, with retention based on compliance and troubleshooting needs.

### Best practices
- set retention explicitly
- use structured logs
- standardize fields such as timestamp, level, service, request ID, trace ID
- avoid storing secrets or sensitive content in logs

### Common mistakes
- infinite retention by accident
- unstructured logs that break queryability
- no field normalization across services

---

## 4.7 Logs Insights

### Technical role
Logs Insights enables fast querying across logs.

### Example query use case
Find top 20 request paths by error count in the last 30 minutes.

### Best practices
- standardize JSON logs for better parsing
- save important operational queries
- use queries during post-incident review and trend analysis

### Common mistakes
- trying to do analytics-grade data lake work inside log queries
- no query conventions for teams

---

## 4.8 Metric filters

### Technical role
Metric filters convert log patterns into metrics.

### Example
Count `ERROR` events from a payment processor log stream and alarm when error count spikes.

### Common mistakes
- overusing metric filters instead of instrumenting proper app metrics
- creating brittle string-match filters on inconsistent logs

---

## 4.9 Contributor Insights

### Technical role
Find top contributors to a pattern such as:
- top failing API routes
- top throttled users
- top source IPs
- top hot keys

### Best practices
- use for high-cardinality problem spaces
- focus rules on actionable top contributors

### Common mistakes
- enabling rules with weak source data
- confusing contributor analysis with full trace-level root cause

---

## 4.10 CloudWatch Synthetics

### Technical role
Run scripted probes to validate endpoint or workflow health from a user-style perspective.

### Entities
- canary
- schedule
- runtime
- artifact storage
- canary metrics

### Example
A canary logs in to an application, loads a dashboard, and verifies a key transaction path every 5 minutes.

### Best practices
- test critical user journeys, not just `/health`
- keep canaries stable and versioned
- store canary artifacts securely
- use multiple locations where needed

### Common mistakes
- overly brittle scripts
- using canaries only after incidents start
- no artifact/log retention plan

---

## 4.11 Application Signals

CloudWatch Application Signals provides application-centric observability across services and dependencies. X-Ray integrates with CloudWatch Application Signals, CloudWatch RUM, and Synthetics to correlate metrics, logs, and traces in one operational view. [1]

### Technical role
- service inventory
- dependency visibility
- latency/error rate focus
- application map and topology
- faster root-cause navigation

### Best practices
- enable for critical user-facing applications
- standardize service naming and environment tagging
- connect traces, logs, and metrics for one-click drilldown

### Common mistakes
- enabling service maps without instrumenting enough dependencies
- poor naming conventions that fragment one logical service into many identities

---

## 4.12 Container Insights

Container Insights collects and summarizes metrics and logs for supported container environments such as ECS and EKS. [2]

### Technical role
- cluster/node/pod/task visibility
- CPU/memory/network/disk health
- workload troubleshooting
- container fleet baselines

### Best practices
- use for fleet-level operations
- separate infrastructure metrics from app-level business metrics
- combine with Prometheus and traces for deeper insight

### Common mistakes
- expecting Container Insights alone to replace full application instrumentation
- not controlling log volume in busy clusters

---

# 5. AWS X-Ray

AWS X-Ray provides distributed tracing for requests moving across services.

## 5.1 Technical purpose

X-Ray helps answer:
- where latency is occurring
- which downstream dependency failed
- how one request traversed the system
- whether the issue is internal, downstream, or edge-related

## 5.2 Core entities

### Trace
End-to-end record of a request path.

### Segment
Trace record for a service or component.

### Subsegment
Nested operation inside a segment, often a downstream call or internal block.

### Service map
Graph of services and their relationships.

### Group
Filtered set of traces or services.

### Sampling rule
Rule controlling which requests are traced.

### Annotation
Indexed key-value metadata for filtering traces.

### Metadata
Additional non-indexed trace context.

## 5.3 Example
A checkout request:
- enters ALB
- reaches payments API
- calls Aurora
- calls external fraud service
- waits on downstream timeout  
X-Ray reveals the fraud dependency as the latency spike source.

## 5.4 Best practices
- sample enough for operational value without uncontrolled cost/noise
- instrument critical dependencies
- add annotations for environment, service, tenant, or operation where useful
- correlate traces with logs and metrics

## 5.5 Common mistakes
- tracing only one service in a multi-hop architecture
- over-sampling everything in high-volume systems
- no trace-to-log correlation fields

## 5.6 Troubleshooting cues
- missing downstream data often means incomplete instrumentation or permissions
- sparse traces often mean bad sampling rules
- incomplete maps often reflect uninstrumented edges

---

# 6. ADOT, Amazon Managed Service for Prometheus, and Amazon Managed Grafana

This section covers modern AWS observability for cloud-native and OpenTelemetry-oriented systems.

---

## 6.1 AWS Distro for OpenTelemetry (ADOT)

ADOT is an AWS-supported OpenTelemetry distribution with SDKs, auto-instrumentation, and collectors. [3]

### Technical purpose
Collect, process, and export metrics, logs, and traces with open standards.

### Core entities

### Collector
Telemetry pipeline service/process.

### Receiver
Input endpoint such as OTLP or Prometheus scraping.

### Processor
Pipeline stage for batching, filtering, attribute mutation, sampling, or transformation.

### Exporter
Output target such as CloudWatch, X-Ray, or AMP.

### Pipeline
Connected flow of receivers → processors → exporters.

## 6.1.1 Example
An EKS cluster runs ADOT collectors that:
- scrape Prometheus metrics
- receive OTLP traces
- batch and enrich telemetry
- export metrics to AMP
- export traces to X-Ray

## 6.1.2 Best practices
- keep collector pipelines simple and explicit
- control cardinality early
- separate metrics and trace routing where scaling needs differ
- version collector configs in source control

## 6.1.3 Common mistakes
- collector configs with too many transformations and no ownership
- exporting too much high-cardinality data
- not validating receiver/exporter compatibility before rollout

---

## 6.2 Amazon Managed Service for Prometheus (AMP)

AMP is a managed Prometheus-compatible metrics backend that scales ingestion, storage, and querying for PromQL-based monitoring. [4]

### Core entities

### Workspace
Top-level managed Prometheus metrics space.

### Scraper
Managed or integrated scraping configuration in supported modes.

### Rule groups namespace
Container for recording and alerting rules.

### Metrics and labels
Prometheus data model elements.

## 6.2.1 Technical role
Use AMP when you need:
- PromQL
- Kubernetes/container metrics at scale
- open-source Prometheus compatibility without self-managing Prometheus servers

## 6.2.2 Example
A platform team sends Kubernetes and application metrics via ADOT to an AMP workspace, then visualizes them in Grafana.

## 6.2.3 Best practices
- keep label design disciplined
- manage recording/alerting rules centrally
- separate workspaces by environment or trust boundary where justified
- watch ingestion growth and series cardinality

## 6.2.4 Common mistakes
- Prometheus labels with explosive cardinality
- no rule management strategy
- assuming “managed” removes the need for metric governance

---

## 6.3 Amazon Managed Grafana (AMG)

AMG is a managed Grafana service for querying, correlating, and visualizing metrics, logs, and traces from multiple data sources. [5]

### Core entities

### Workspace
Top-level Grafana environment.

### Data source
Backend such as CloudWatch, AMP, X-Ray, OpenSearch, or others.

### Dashboard
Visualization layout.

### Folder
Organizational grouping for dashboards.

### User
Human workspace identity.

### Team
Access grouping for users.

### Service account
Automation identity used for APIs and provisioning.

## 6.3.1 Example
An operations workspace includes:
- CloudWatch metrics
- CloudWatch Logs
- AMP metrics
- X-Ray traces  
with folders for each platform domain.

## 6.3.2 Best practices
- separate viewer/editor/admin roles
- organize dashboards by service domain
- use service accounts for automation
- control plugin and data source permissions

## 6.3.3 Common mistakes
- one shared admin-heavy workspace for all teams
- dashboard sprawl with no folder governance
- no access model between platform and app teams

---

# 7. AWS Systems Manager

Systems Manager is AWS’s operational control plane for fleet management, patching, automation, sessions, parameters, and incident-style operations.

## 7.1 Managed nodes

### Technical purpose
Resources enrolled for Systems Manager operations.

### Typical targets
- EC2 instances
- hybrid/on-prem servers
- edge devices
- supported compute environments

### Best practices
- standardize instance profiles/permissions
- validate node health centrally
- use Systems Manager as the preferred remote management layer

### Common mistakes
- SSH-first culture even when SSM is available
- no inventory or node enrollment standards

---

## 7.2 Systems Manager documents

SSM documents define actions Systems Manager performs on managed instances and resources. Documents are JSON or YAML and can be used by capabilities such as Run Command and Automation. [6]

### Core concepts
- document type
- parameters
- steps
- permissions

### Best practices
- version custom documents
- control document permissions
- treat documents like code

### Common mistakes
- editing documents ad hoc in production
- broad document sharing with no review

---

## 7.3 Run Command

Run Command remotely executes commands on managed nodes. [7]

### Technical role
- fleet actions
- diagnostics
- one-time remediation
- package install
- script execution

### Example
Run a shell command on all web servers to collect a suspect process list during an incident.

### Best practices
- target by tags or resource groups
- log outputs centrally
- use approval and blast-radius controls for large targets

### Common mistakes
- broad unreviewed commands on production fleets
- no output retention for audit or debugging

---

## 7.4 Session Manager

### Technical purpose
Provide secure interactive shell/remote access without opening inbound SSH/RDP ports.

### Core entities
- session
- session preferences
- session logs
- target managed node

### Best practices
- enable session logging to CloudWatch Logs or S3
- restrict who can start sessions
- prefer Session Manager over internet-exposed SSH

### Common mistakes
- enabling sessions without audit logging
- mixing break-glass and routine admin access

---

## 7.5 State Manager

### Technical purpose
Apply desired configuration over time using associations.

### Core entities
- association
- target
- schedule
- compliance status

### Example
Ensure CloudWatch agent config remains present on all production EC2 instances.

### Best practices
- use for configuration drift control
- separate baseline associations from app-specific associations
- review compliance regularly

### Common mistakes
- too many overlapping associations
- no ownership for baseline drift

---

## 7.6 Patch Manager

### Technical purpose
Patch managed nodes using defined baselines and schedules.

### Core entities
- patch baseline
- patch group
- maintenance window
- compliance result

### Example
Linux production instances patch during a Sunday window with controlled approval delays.

### Best practices
- define separate baselines by OS and environment
- use maintenance windows
- monitor patch compliance
- test patching in lower environments first

### Common mistakes
- one patch baseline for everything
- patching without reboot/recovery planning
- no compliance review after patch cycles

---

## 7.7 Automation

### Technical purpose
Run multi-step operational runbooks.

### Core entities
- automation runbook
- step
- input/output
- service role
- concurrency/error thresholds

### Example
A runbook:
1. snapshot EBS volume
2. stop instance
3. replace root volume
4. start instance
5. validate health

### Best practices
- use Automation for repeatable remediation
- version runbooks
- define timeouts and failure handling
- integrate with incidents and alarms

### Common mistakes
- runbooks without rollback paths
- automation that assumes one account/region layout only

---

## 7.8 Parameter Store

### Technical purpose
Store configuration parameters and secure strings.

### Core entities
- parameter
- hierarchy/path
- parameter version
- parameter policy
- secure string

### Best practices
- use hierarchy conventions
- separate config from secrets
- apply parameter policies for lifecycle where needed
- control access tightly

### Common mistakes
- storing secrets loosely when Secrets Manager is the better fit
- no naming convention across teams

---

## 7.9 OpsItems

### Technical purpose
Track and manage operational issues in a structured way.

### Example
A recurring disk-space alarm opens OpsItems with remediation guidance and owner assignment.

### Best practices
- use consistent severity and category conventions
- connect alarms and automations to OpsCenter/OpsItems where appropriate

### Common mistakes
- creating OpsItems with no assignment or closure workflow

---

# 8. AWS Well-Architected Tool and AWS Resilience Hub

---

## 8.1 AWS Well-Architected Tool

The Well-Architected Tool supports structured reviews of workloads against AWS best-practice lenses and pillars. [8]

### Core entities
- workload
- lens
- milestone
- review template
- improvement plan
- workload share

### Technical role
Translate architecture review into tracked risks and improvement actions.

### Example
A payment workload is reviewed with reliability, security, cost, and operational excellence focus, and milestones are captured after each major platform change.

### Best practices
- review critical workloads regularly
- create milestones before and after major change waves
- share workloads with the right review stakeholders
- turn identified risks into tracked actions

### Common mistakes
- running one review once and never revisiting it
- treating the review as compliance theater instead of engineering input

---

## 8.2 AWS Resilience Hub

AWS Resilience Hub helps assess application resilience against defined recovery objectives and provides resiliency recommendations. AWS states it helps validate whether applications can meet recovery time objective (RTO) and recovery point objective (RPO) targets and integrates into the software delivery lifecycle. [9]

### Core entities
- application
- app component
- resilience policy
- assessment
- resiliency score
- recommendation
- SOP
- resource mapping/import source

### Technical role
Evaluate whether an application architecture aligns with resilience goals.

### Example
A critical order-processing system has:
- RTO target: 15 minutes
- RPO target: 5 minutes  
Resilience Hub assessment reveals single-AZ dependency and missing recovery runbooks.

### Best practices
- define realistic resilience policies
- scope applications correctly
- reassess after major architecture changes
- align recommendations with disaster recovery runbooks and testing

### Common mistakes
- wrong application boundaries
- no mapping between business criticality and RTO/RPO policy
- treating score as an end rather than a signal

---

# 9. Cost Management and FinOps Services

Week 5 includes the main AWS cost-management toolkit.

---

## 9.1 AWS Cost Explorer

Cost Explorer lets you analyze cost and usage data, apply filters/groupings, and view forecasts. AWS documents that it supports up to 13 months of historical data and forecasts for the next three months. [10]

### Core entities
- cost and usage data
- dimensions
- filters
- groupings
- forecasts
- saved reports

### Example
Filter spend by:
- linked account
- service
- environment tag
- cost category  
then compare MoM growth.

### Best practices
- use cost categories and tags for meaningful views
- separate blended accounting views from engineering usage views
- review forecasts regularly

### Common mistakes
- trying to do finance-grade raw allocation solely in Cost Explorer
- no tagging/cost category model, making reports low value

---

## 9.2 AWS Budgets

AWS Budgets supports cost, usage, and related budget tracking with alerts and actions. AWS documents cost budgets, usage budgets, RI utilization budgets, and other types. [11]

### Core entities
- budget
- threshold
- alert
- budget action
- forecasted vs actual tracking

### Example
If forecasted monthly compute spend exceeds threshold, send alert and optionally trigger budget action for sandbox restrictions.

### Best practices
- use forecasted and actual thresholds
- define budget actions carefully
- separate team budgets from executive budgets
- tune thresholds to avoid alert fatigue

### Common mistakes
- one global budget for the whole org only
- no actionability in alerts
- too many budgets no one reviews

---

## 9.3 AWS Cost Categories

Cost Categories create business-aligned billing dimensions that appear in Cost Explorer, Budgets, CUR, and Cost Anomaly Detection. [12]

### Core entities
- cost category
- rule
- inherited category
- split-charge allocation

### Example
Map linked accounts and tags into:
- Team
- BusinessUnit
- ProductLine

### Best practices
- align categories to ownership model
- document rule logic
- use hierarchical categories where needed

### Common mistakes
- category rules that overlap ambiguously
- no ownership for chargeback taxonomy

---

## 9.4 AWS Cost and Usage Report (CUR)

### Technical purpose
Provide the detailed billing dataset for advanced analysis.

### Core entities
- report definition
- delivery location
- time granularity
- additional artifacts
- integration destinations

### Best practices
- centralize CUR delivery
- define clear downstream consumers
- keep schema evolution in mind for analytics pipelines
- build Athena/warehouse models over CUR for serious FinOps

### Common mistakes
- enabling CUR with no analytics plan
- poor partitioning and query cost management downstream

---

## 9.5 AWS Cost Anomaly Detection

AWS Cost Anomaly Detection uses ML models to detect unusual spend patterns and can send alert subscriptions. [13]

### Core entities
- monitor
- anomaly
- alert subscription
- threshold

### Example
A sudden increase in NAT Gateway or Bedrock usage creates an anomaly alert before month-end surprises finance.

### Best practices
- create monitors by account/team/business dimension
- tune alert thresholds to avoid noise
- route alerts to both platform and finance owners

### Common mistakes
- one giant monitor only
- thresholds too low, creating alert fatigue
- no triage owner for anomalies

---

## 9.6 AWS Compute Optimizer

Compute Optimizer recommends more optimal resource configurations using historical utilization metrics. [14]

### Typical recommendation targets
- EC2
- EBS
- Lambda memory sizing
- other supported resources over time

### Core entities
- recommendation
- finding
- projected utilization
- preference setting

### Example
An overprovisioned EC2 fleet is flagged for downsizing; EBS volumes are flagged for over- or under-provisioning.

### Best practices
- review recommendations with workload context
- combine with performance and scaling knowledge
- validate before broad rightsizing

### Common mistakes
- blindly applying recommendations to bursty or seasonally variable workloads
- ignoring that recommendation quality depends on observed history

---

## 9.7 AWS Trusted Advisor

### Technical purpose
Provide checks and recommendations across cost, performance, service limits, security, and fault tolerance domains.

### Core entities
- check
- recommendation
- status
- refresh schedule

### Example
Trusted Advisor surfaces idle load balancers, exposed security groups, or service limit concerns for a central platform team.

### Best practices
- review checks on a scheduled cadence
- map checks to ownership teams
- combine with Compute Optimizer and cost data, not in isolation

### Common mistakes
- no ownership for surfaced issues
- confusing generic checks with workload-aware optimization decisions

---

# 10. GuardDuty, Inspector, Macie, Detective, and Security Lake

These services form a major part of continuous security operations.

---

## 10.1 Amazon GuardDuty

GuardDuty is a continuous security monitoring service that detects potentially malicious or unauthorized activity in AWS environments. [15]

### Core entities
- detector
- finding
- filter
- trusted threat intel and IP sets
- malware protection plan
- coverage view
- delegated admin integration features

### Detection domains
- account and API activity
- S3 protection
- EKS protection
- EKS runtime/cluster-related protections where enabled
- ECR scanning integrations where applicable
- Lambda-related protections where supported

### Example
GuardDuty flags anomalous credential usage from an unusual geolocation and suspicious S3 access behavior.

### Best practices
- enable organization-wide with delegated admin
- review finding severity and suppression logic carefully
- integrate findings with Security Hub, Detective, ticketing, and response workflows
- tune filters, not by hiding real issues but by suppressing known benign patterns responsibly

### Common mistakes
- enabling GuardDuty but not assigning investigation ownership
- filtering too aggressively
- not enabling relevant protection features for used services

---

## 10.2 Amazon Inspector

Inspector is a vulnerability assessment service that continuously assesses supported resources and produces prioritized findings. [16]

### Core entities
- coverage
- finding
- filter
- SBOM export and vulnerability views in supported workflows

### Typical scope
- EC2
- container images in ECR
- Lambda functions
- code/package exposure patterns in supported services

### Example
Inspector identifies critical CVEs in a production image repository and a vulnerable package in a Lambda function.

### Best practices
- integrate with image promotion pipelines
- track coverage, not just findings
- prioritize internet-exposed and production workloads first
- connect remediation to patching/build workflows

### Common mistakes
- focusing only on findings while missing uncovered resources
- no SLA for remediation
- no bridge between Inspector and CI/CD gates

---

## 10.3 Amazon Macie

Macie is a managed data security and privacy service that discovers, monitors, and helps protect sensitive data in Amazon S3. [17]

### Core entities
- classification job
- sensitive data discovery result
- managed data identifier
- custom data identifier
- findings
- bucket-level posture views

### Example
Macie identifies PII in an analytics bucket that was assumed to contain only anonymized data.

### Best practices
- prioritize high-risk buckets first
- combine automated discovery with governance classification
- validate custom identifiers for organization-specific patterns
- integrate Macie findings with data governance owners

### Common mistakes
- assuming bucket tags or names are enough to know data sensitivity
- running one-time jobs only and not revisiting discovery
- no response process for discovered sensitive data

---

## 10.4 Amazon Detective

Detective helps analyze and investigate security events across accounts using behavior graphs and entity relationships. GuardDuty findings can pivot into Detective for investigation. [18]

### Core entities
- behavior graph
- entity profile
- investigation
- finding pivot

### Technical role
Detective is for investigation depth, not primary detection.

### Example
A GuardDuty finding for suspicious EC2 behavior is opened in Detective to examine related IPs, instances, API activity, and account interactions.

### Best practices
- use Detective as part of investigation workflow
- correlate with GuardDuty and Security Hub findings
- train responders on entity pivoting

### Common mistakes
- expecting Detective to replace detection services
- no analyst playbook for pivoting from findings into graph investigation

---

## 10.5 Amazon Security Lake

Security Lake centralizes security-related logs and events, converts supported sources to OCSF schema and Apache Parquet, and stores them in S3 in your account. [19]

### Core entities
- data lake
- source
- subscriber
- OCSF event class
- custom source
- S3-backed regional data store

### Technical role
Normalize security telemetry for downstream analysis and subscriptions.

### Example
Security Lake ingests AWS security service logs, converts them to OCSF, and allows downstream analytics or SIEM subscribers to consume normalized events.

### Best practices
- define subscriber model early
- understand source onboarding and regional storage behavior
- align retention, access, and classification controls with security governance
- validate OCSF mapping for custom sources

### Common mistakes
- enabling a lake with no consumers or analytics plan
- not designing S3 access and governance around lake data
- misunderstanding that custom sources must already conform to source requirements

---

# 11. SageMaker AI and Bedrock Observability and Governance

Week 5 includes the operational side of AI platforms.

---

## 11.1 Amazon SageMaker operational entities

### Key entities
- monitoring schedule
- baseline
- constraints
- processing job
- bias report
- explainability report
- experiment
- trial
- lineage artifact
- lineage action

### Technical role
SageMaker observability is about:
- data drift
- model quality drift
- bias
- explainability
- experiment tracking
- lineage and auditability

### Example
A fraud model has:
- baseline from pre-production data
- scheduled monitoring for feature drift
- bias reports for fairness review
- lineage linking training data, model package, and deployment

### Best practices
- create baselines before production deployment
- monitor both data and model quality
- record lineage for audit and rollback confidence
- separate training observability from inference observability

### Common mistakes
- production endpoints with no baselines
- no lineage chain from data to model to endpoint
- treating model deployment like ordinary stateless app deploy without drift considerations

---

## 11.2 Amazon Bedrock operational entities

Bedrock supports model invocation logging to CloudWatch Logs and Amazon S3 for model invocations in an account in a Region. [20] Bedrock also exposes observability options around the runtime endpoint and publishes guardrail metrics in CloudWatch. [21]

### Key entities
- model evaluation job
- evaluation dataset
- guardrail
- guardrail version
- foundation model
- agent
- invocation logs

### Technical role
Operational concerns include:
- who used which model
- what prompts/responses need to be logged
- whether guardrails are effective
- whether evaluated model quality remains acceptable
- whether model access is governed

### Example
A customer support assistant uses:
- Bedrock guardrails for restricted content
- invocation logging to S3 and CloudWatch Logs
- evaluation datasets to compare quality between model choices

### Best practices
- enable logging where policy allows and requires it
- version guardrails
- validate models with representative evaluation datasets
- control model access by role and environment
- monitor guardrail metrics and runtime behavior

### Common mistakes
- no invocation logging
- no guardrail testing before production
- mixing experimental and production model access
- no review process for prompt/output retention risk

---

# 12. Operational Excellence Playbooks

Week 5 is not just service knowledge. It is operating model knowledge.

## 12.1 Incident triage sequence
1. Alarm fires
2. Check dashboard and recent deploys
3. Review relevant logs and traces
4. Confirm user impact
5. Run investigation query/playbook
6. Open OpsItem or incident artifact
7. Execute safe automation/runbook
8. Record timeline and findings
9. Review cost/resilience/security side effects if relevant

## 12.2 Cost spike playbook
1. Check Cost Anomaly Detection
2. Validate Cost Explorer grouping by service/account/category
3. Query CUR for detail
4. Confirm if usage is expected or waste
5. Check deployment and scaling changes
6. Review Compute Optimizer / Trusted Advisor suggestions
7. Apply temporary budget guardrails if needed

## 12.3 Security finding playbook
1. Inspect GuardDuty/Inspector/Macie finding
2. Pivot into Detective if needed
3. Correlate with CloudTrail, logs, or endpoint telemetry
4. Confirm scope and impacted entities
5. Contain with automation or manual controls
6. Preserve evidence
7. Track in case management/OpsItems

## 12.4 Resilience review playbook
1. Review Well-Architected milestone
2. Run Resilience Hub assessment
3. Compare against target RTO/RPO
4. Identify single points of failure
5. Update SOPs/runbooks
6. Schedule resilience validation

---

# 13. Service Selection Guide

## 13.1 CloudWatch vs X-Ray vs AMP/Grafana
- **CloudWatch**: default AWS observability backbone
- **X-Ray**: distributed tracing and dependency latency
- **AMP/Grafana**: PromQL-heavy cloud-native metrics and flexible visualization

## 13.2 Run Command vs Session Manager vs Automation
- **Run Command**: one-time remote command execution
- **Session Manager**: interactive access
- **Automation**: structured multi-step runbooks

## 13.3 Cost Explorer vs CUR
- **Cost Explorer**: interactive analysis
- **CUR**: detailed raw billing dataset for deeper FinOps analytics

## 13.4 GuardDuty vs Inspector vs Macie vs Detective
- **GuardDuty**: threat detection
- **Inspector**: vulnerability and exposure assessment
- **Macie**: sensitive data discovery in S3
- **Detective**: investigation and relationship analysis

## 13.5 Security Lake vs individual findings
- **Individual services**: detection and finding generation
- **Security Lake**: normalized central data lake for security telemetry

---

# 14. End-to-End Week 5 Example

A mature platform team could operate like this:

### Observability
- CloudWatch dashboards for service health
- Application Signals for service topology
- X-Ray traces for latency analysis
- Logs Insights saved queries for incidents
- Synthetics canaries for user journey checks
- AMP + Grafana for Kubernetes/platform metrics

### Fleet operations
- Systems Manager managed nodes
- Session Manager for access
- Patch Manager for scheduled patching
- Automation for remediation
- OpsItems for operational tracking

### FinOps
- Cost Categories aligned to products
- Cost Explorer for routine analysis
- Budgets with thresholds and selected actions
- CUR feeding Athena/warehouse models
- anomaly detection alerts
- Compute Optimizer and Trusted Advisor reviews

### Security operations
- GuardDuty, Inspector, and Macie findings
- Detective for investigations
- Security Lake for normalized security telemetry

### AI operations
- SageMaker monitoring and lineage
- Bedrock invocation logging and guardrail monitoring

This is the Week 5 principle:
> telemetry has value only when it drives decision, automation, ownership, and review.

---

# 15. Troubleshooting Framework for Week 5

## 15.1 Missing visibility
- is telemetry emitted?
- is ADOT/agent config correct?
- is the log group or metric namespace correct?
- is sampling too low?
- is data source permission missing?

## 15.2 Noisy operations
- too many alarms?
- weak thresholds?
- no composite alarms?
- no filter policies or suppression logic?
- too much metric cardinality?

## 15.3 Slow incident resolution
- missing dashboards?
- poor trace coverage?
- no saved log queries?
- no documented runbook?
- no ownership mapping?

## 15.4 Cost confusion
- no cost categories?
- bad tags?
- no CUR analytics?
- no anomaly owner?
- using generic rightsizing recommendations without workload context?

## 15.5 Security operations gaps
- detectors enabled but no triage path?
- findings not integrated with downstream tools?
- Macie/Inspector coverage incomplete?
- Security Lake has no subscribers?
- Bedrock/SageMaker logs not retained or reviewed?

---

# 16. Common Week 5 Mistakes

- metrics everywhere, but no signal design
- alarms without runbooks
- logs with no retention policy
- traces with poor sampling strategy
- dashboard sprawl without ownership
- SSM enabled without session logging
- patching without compliance review
- Well-Architected reviews done once and forgotten
- budgets created but not operationalized
- anomaly alerts routed to no one
- rightsizing without workload context
- GuardDuty/Inspector enabled but not triaged
- Security Lake built with no consumer strategy
- AI models deployed with no monitoring, lineage, or logging policy

---

# 17. Week 5 Revision Checklist

You should be able to explain clearly:

- how CloudWatch metrics, dimensions, alarms, log groups, metric filters, dashboards, and Logs Insights relate
- when to use Contributor Insights, Synthetics, Application Signals, and Container Insights
- how X-Ray traces, segments, subsegments, service maps, groups, and sampling rules work
- how ADOT receivers, processors, exporters, and pipelines route telemetry
- what AMP workspaces and rule group namespaces do
- how Managed Grafana workspaces, data sources, folders, teams, and service accounts are used
- how Systems Manager managed nodes, documents, Run Command, Session Manager, State Manager, Patch Manager, Automation, Parameter Store, and OpsItems differ
- how Well-Architected workloads, lenses, milestones, and improvement plans work
- how Resilience Hub applications, resilience policies, assessments, and scores work
- how Cost Explorer, Budgets, Cost Categories, CUR, anomaly detection, Compute Optimizer, and Trusted Advisor fit together
- how GuardDuty, Inspector, Macie, Detective, and Security Lake differ and connect
- how SageMaker monitoring, lineage, bias/explainability, and Bedrock evaluation, guardrails, and invocation logging support AI operations

---

# 18. Memory Anchors

- **CloudWatch** = core metrics/logs/alarms/dashboards
- **Logs Insights** = query logs fast
- **Contributor Insights** = top contributors in noisy data
- **Synthetics** = scripted user-path checks
- **Application Signals** = service-centric app observability
- **Container Insights** = fleet/container telemetry
- **X-Ray** = distributed traces and dependency latency
- **ADOT** = OpenTelemetry collection and routing
- **AMP** = managed Prometheus backend
- **Managed Grafana** = visualization across observability data sources
- **Systems Manager** = fleet operations control plane
- **Run Command** = remote commands
- **Session Manager** = secure shell/session access
- **State Manager** = desired config associations
- **Patch Manager** = patch compliance
- **Automation** = runbooks
- **Parameter Store** = config parameters
- **OpsItems** = tracked operational issues
- **Well-Architected Tool** = architecture review tracking
- **Resilience Hub** = resilience assessment vs RTO/RPO
- **Cost Explorer** = interactive cost analysis
- **Budgets** = threshold alerts/actions
- **Cost Categories** = business billing dimensions
- **CUR** = raw detailed billing data
- **Cost Anomaly Detection** = unusual spend alerts
- **Compute Optimizer** = rightsizing recommendations
- **Trusted Advisor** = broad optimization checks
- **GuardDuty** = threat detection
- **Inspector** = vulnerability findings
- **Macie** = S3 sensitive data discovery
- **Detective** = investigation graphs
- **Security Lake** = normalized security telemetry lake
- **SageMaker monitoring** = drift, bias, explainability, lineage
- **Bedrock logging/guardrails** = GenAI governance and runtime observability

---

# 19. Official AWS Documentation References

## Observability
- Amazon CloudWatch Documentation  
  https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/

- AWS X-Ray Documentation  
  https://docs.aws.amazon.com/xray/

- AWS Distro for OpenTelemetry and CloudWatch  
  https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-OTLP-UsingADOT.html

- Amazon Managed Service for Prometheus Documentation  
  https://docs.aws.amazon.com/prometheus/

- Amazon Managed Grafana Documentation  
  https://docs.aws.amazon.com/grafana/

## Fleet operations
- AWS Systems Manager Documentation  
  https://docs.aws.amazon.com/systems-manager/

- AWS Systems Manager Run Command  
  https://docs.aws.amazon.com/systems-manager/latest/userguide/run-command.html

- AWS Systems Manager Documents  
  https://docs.aws.amazon.com/systems-manager/latest/userguide/documents.html

## Architecture and resilience
- AWS Well-Architected Tool Documentation  
  https://docs.aws.amazon.com/wellarchitected/

- AWS Resilience Hub Documentation  
  https://docs.aws.amazon.com/resilience-hub/

## FinOps
- AWS Cost Explorer Documentation  
  https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html

- AWS Budgets Documentation  
  https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html

- AWS Cost Categories Documentation  
  https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-cost-categories.html

- AWS Cost and Usage Reports Documentation  
  https://docs.aws.amazon.com/cur/

- AWS Cost Anomaly Detection Documentation  
  https://docs.aws.amazon.com/cost-management/latest/userguide/manage-ad.html

- AWS Compute Optimizer Documentation  
  https://docs.aws.amazon.com/compute-optimizer/

- AWS Trusted Advisor Documentation  
  https://docs.aws.amazon.com/awssupport/latest/user/trusted-advisor.html

## Security operations
- Amazon GuardDuty Documentation  
  https://docs.aws.amazon.com/guardduty/

- Amazon Inspector Documentation  
  https://docs.aws.amazon.com/inspector/

- Amazon Macie Documentation  
  https://docs.aws.amazon.com/macie/

- Amazon Detective Documentation  
  https://docs.aws.amazon.com/detective/

- Amazon Security Lake Documentation  
  https://docs.aws.amazon.com/security-lake/

## AI operations
- Amazon SageMaker Documentation  
  https://docs.aws.amazon.com/sagemaker/

- Amazon Bedrock Documentation  
  https://docs.aws.amazon.com/bedrock/

---

# 20. Final Summary

Week 5 is about **seeing, understanding, optimizing, and operating AWS systems well**.

The strongest mental model is:

- **CloudWatch, X-Ray, ADOT, Prometheus, and Grafana** provide telemetry and visibility
- **Systems Manager** provides fleet access, automation, and control
- **Well-Architected Tool and Resilience Hub** provide structured review of risk and resilience
- **Cost Explorer, Budgets, CUR, anomalies, Compute Optimizer, and Trusted Advisor** provide FinOps and efficiency control
- **GuardDuty, Inspector, Macie, Detective, and Security Lake** provide continuous security operations
- **SageMaker and Bedrock operational controls** extend observability and governance into AI systems

When you can explain how those signals become alarms, investigations, runbooks, cost decisions, and resilience improvements, Week 5 is understood architecturally.

---

## Reference Notes

[1] X-Ray integrates with CloudWatch Application Signals, CloudWatch RUM, and CloudWatch Synthetics for correlated application monitoring and troubleshooting. Source: AWS X-Ray and CloudWatch integration docs.  
[2] Container Insights is available for supported container environments including ECS and EKS. Source: CloudWatch Container Insights docs.  
[3] ADOT is AWS’s supported OpenTelemetry distribution and includes collectors, SDKs, and integrations with AWS observability services. Source: AWS X-Ray and CloudWatch ADOT docs.  
[4] Amazon Managed Service for Prometheus is a managed Prometheus-compatible monitoring service that scales ingestion, storage, and query. Source: AMP docs.  
[5] Amazon Managed Grafana is a managed visualization service for metrics, logs, and traces from multiple data sources. Source: AMG docs.  
[6] Systems Manager documents define actions in JSON/YAML and are used by multiple Systems Manager capabilities. Source: SSM documents docs.  
[7] Run Command remotely manages EC2 instances and hybrid nodes. Source: SSM Run Command docs.  
[8] The Well-Architected Tool supports workload reviews against AWS best practices and lenses. Source: AWS Well-Architected docs.  
[9] Resilience Hub assesses applications against RTO/RPO-oriented resiliency targets and recommendations. Source: AWS Resilience Hub docs.  
[10] AWS documents Cost Explorer as supporting up to 13 months of historical data and 3 months of forecast. Source: Cost Explorer docs.  
[11] AWS documents multiple budget types including cost and usage budgets. Source: AWS Budgets docs.  
[12] Cost Categories appear in Cost Explorer, Budgets, CUR, and Cost Anomaly Detection. Source: Cost Categories docs.  
[13] AWS Cost Anomaly Detection uses ML models to identify unusual spend patterns. Source: Cost Anomaly Detection docs.  
[14] Compute Optimizer recommends more optimal compute configurations using historical utilization metrics. Source: Compute Optimizer docs.  
[15] GuardDuty is a continuous security monitoring service for detecting suspicious or malicious activity in AWS environments. Source: GuardDuty docs.  
[16] Inspector continuously assesses supported resources for vulnerabilities and best-practice deviations. Source: Inspector docs.  
[17] Macie discovers and helps protect sensitive data in Amazon S3. Source: Macie docs.  
[18] Detective helps investigate security events using behavior graphs and can integrate with GuardDuty finding workflows. Source: Detective/GuardDuty integration docs.  
[19] Security Lake normalizes supported sources to OCSF and Parquet and stores them in S3 in your account. Source: Security Lake docs.  
[20] Bedrock supports model invocation logging to CloudWatch Logs and Amazon S3 for account-region model invocations. Source: Bedrock invocation logging docs.  
[21] Bedrock Guardrails publish runtime metrics to CloudWatch and Bedrock runtime endpoints can be monitored using CloudWatch metrics, CloudTrail, and invocation logging. Source: Bedrock monitoring docs.
