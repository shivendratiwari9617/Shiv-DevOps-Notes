# Grafana and Prometheus Technical Notes — Monitoring, Alerting, Dashboards, Configuration, and Practical Examples

This file is a **deep technical-notes document** for **Grafana and Prometheus**.

It is designed to explain:

- what Grafana and Prometheus are
- how they fit together in a monitoring stack
- Prometheus monitoring and scraping concepts
- Prometheus alerting and rules
- Grafana dashboards, variables, and alerting
- practical configuration examples using official concepts
- monitoring and observability best practices
- troubleshooting and interview-ready explanations

This file is written as a **technical understanding document**, not as a distribution-specific installation guide.

---

# 1) What Grafana and Prometheus are

## 1.1 Prometheus

Prometheus is an **open-source systems monitoring and alerting toolkit**. It is widely used to collect metrics, store time-series data, evaluate rules, and send alerts. Prometheus is configured around concepts such as scrape targets, jobs, labels, PromQL, rule files, and Alertmanager integration. citeturn0search3turn0search7turn0search15

---

## 1.2 Grafana

Grafana is a visualization and observability platform used to connect to data sources, query them, and present their data through dashboards, panels, and alerting workflows. Grafana works with many data sources, including Prometheus, and can also manage alerting resources and dashboard variables. citeturn0search6turn0search10turn0search14

---

## 1.3 How they work together

A common architecture is:

1. applications and infrastructure expose metrics
2. Prometheus scrapes and stores those metrics
3. Prometheus evaluates recording and alerting rules
4. Alertmanager handles notification routing for Prometheus alerts
5. Grafana queries Prometheus and visualizes the data in dashboards
6. Grafana can also create its own alerts across supported data sources

This is a practical modern monitoring stack. citeturn0search3turn0search10turn0search15

---

# 2) Prometheus architecture and monitoring model

## 2.1 Pull-based scraping

Prometheus primarily works by **scraping** HTTP endpoints that expose metrics. A `scrape_config` defines a set of targets and parameters describing how metrics are collected for a job. In the general case, one scrape configuration specifies one job. citeturn0search3

---

## 2.2 Time series and labels

Prometheus stores metrics as time series identified by:
- metric name
- labels
- timestamp
- value

Labels are fundamental because they allow slicing, filtering, and aggregation across targets, instances, environments, services, and other dimensions.

---

## 2.3 Jobs and targets

In Prometheus, a **job** is a logical label for a group of targets. A target is typically one scrape endpoint, such as an application metrics endpoint or exporter endpoint. Scrape jobs are central to Prometheus configuration. citeturn0search3

---

## 2.4 PromQL

PromQL is Prometheus Query Language. It is used to:
- retrieve time series
- calculate rates
- aggregate metrics
- filter label sets
- power dashboards
- define recording rules
- define alerting rules

PromQL is one of the most important Prometheus topics.

---

# 3) Prometheus configuration

## 3.1 Core `prometheus.yml` structure

Prometheus configuration includes global settings and scrape configuration blocks. The official configuration reference documents items such as `global`, `scrape_configs`, `rule_files`, and `alerting`. citeturn0search3turn0search5turn0search13

Example:

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - "rules/*.yml"

alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - "alertmanager:9093"

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node-exporter"
    static_configs:
      - targets: ["node-exporter:9100"]

  - job_name: "app"
    metrics_path: /metrics
    static_configs:
      - targets: ["app1:8080", "app2:8080"]
```

This example shows:
- global scrape and rule evaluation intervals
- rule file loading
- Alertmanager configuration
- scrape targets for Prometheus itself, node exporter, and an application

The `scrape_timeout` cannot be greater than the scrape interval. citeturn0search3

---

## 3.2 Important configuration concepts

### `global`
Defines defaults such as:
- `scrape_interval`
- `scrape_timeout`
- `evaluation_interval`

### `scrape_configs`
Defines the monitored jobs and targets.

### `rule_files`
Loads recording and alerting rules from YAML rule files.

### `alerting`
Defines Alertmanager targets.

These are the core operational sections of Prometheus configuration. citeturn0search3turn0search13

---

## 3.3 Practical scrape example

A service exposing metrics at `/metrics` on port `8080` can be scraped with:

```yaml
scrape_configs:
  - job_name: "payments-service"
    metrics_path: /metrics
    static_configs:
      - targets: ["payments-1:8080", "payments-2:8080"]
```

This is a basic but realistic configuration pattern.

---


## 3.4 Service discovery

Prometheus supports many service discovery mechanisms. In addition to static targets, modern Prometheus environments often use dynamic discovery so that target lists do not have to be hardcoded.

Operationally this matters because cloud, Kubernetes, and autoscaled systems change frequently.

Common patterns include:
- static target discovery for simple setups
- file-based service discovery
- HTTP service discovery
- cloud or platform-specific discovery integrations

Prometheus documents both file-based and HTTP-based service discovery approaches as well as many built-in service discovery integrations. citeturn0search8turn0search10

---

## 3.5 Relabeling

Relabeling is one of the most important Prometheus configuration topics in real-world environments.

It is used to:
- rewrite labels
- drop unwanted targets
- keep only targets that match rules
- normalize discovered metadata
- control which target address or scheme is used

Prometheus documentation highlights relabeling as part of the normal configuration and service discovery workflow. It is essential in Kubernetes and dynamic infrastructure monitoring. citeturn0search8turn0search6

Example conceptual pattern:

```yaml
scrape_configs:
  - job_name: "kubernetes-pods"
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
        action: replace
        target_label: __metrics_path__
      - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
        action: replace
        regex: ([^:]+)(?::\d+)?;(\d+)
        replacement: $1:$2
        target_label: __address__
```

This is a representative relabeling workflow for dynamically discovered targets.

---

## 3.6 Exporters

Prometheus often collects system and platform metrics through exporters.

Common examples:
- `node_exporter` for Linux host metrics
- application-native `/metrics` endpoints
- exporter patterns for databases, queues, or cloud systems

Exporter design matters because Prometheus depends on scrapeable metrics endpoints rather than agent-only black-box telemetry.

Node Exporter is one of the most common operational building blocks in Prometheus environments. Prometheus provides guidance for monitoring Linux host metrics with Node Exporter. citeturn0search0

---

## 3.7 Federation and remote write

As Prometheus usage grows, teams often encounter scaling and long-term storage needs.

Important concepts:
- **federation** for selected metric aggregation across Prometheus servers
- **remote_write** for sending samples to external long-term or downstream systems

Prometheus documents remote write tuning and emphasizes configuration choices for queueing and throughput behavior. citeturn0search4turn0search8

Example conceptual `remote_write` block:

```yaml
remote_write:
  - url: "https://remote-storage.example/api/v1/write"
```

This is intentionally simple; production setups often add authentication and tuning parameters.

---

## 3.8 Histogram and summary guidance

Prometheus documentation includes dedicated guidance for **histograms and summaries** because they are critical for latency measurement and percentile-style analysis. This matters for queries such as p95 or p99 latency and for choosing the right instrumentation type. citeturn0search0

Practical guidance:
- histograms are commonly preferred when you need aggregation across instances
- summaries calculate quantiles differently and are less suited for cross-instance aggregation
- latency dashboards often use histogram buckets with `histogram_quantile(...)`

Example:

```promql
histogram_quantile(
  0.95,
  sum(rate(http_request_duration_seconds_bucket[5m])) by (le)
)
```

This is a standard service-latency dashboard pattern.

---
# 4) Prometheus monitoring and querying examples

## 4.1 Basic metric query examples

Examples:

```promql
up
```

Shows whether targets are reachable and scraped successfully.

```promql
up{job="payments-service"}
```

Filters by job.

```promql
process_cpu_seconds_total
```

Shows CPU time metrics if exposed by the target.

---

## 4.2 Rate and aggregation examples

Calculate request rate:

```promql
rate(http_requests_total[5m])
```

Sum request rate across all instances:

```promql
sum(rate(http_requests_total[5m]))
```

Per-job request rate:

```promql
sum by (job) (rate(http_requests_total[5m]))
```

These are common PromQL patterns.

---

## 4.3 Error-rate style example

Example:

```promql
sum(rate(http_requests_total{status=~"5.."}[5m]))
/
sum(rate(http_requests_total[5m]))
```

This is useful for dashboarding or alerting on server error ratios.

---

# 5) Prometheus rules and alerting

## 5.1 Recording rules

Prometheus supports **recording rules** and **alerting rules**. Rule files use YAML and are loaded through the `rule_files` field in Prometheus configuration. Recording rules let you precompute expensive or commonly used expressions into new persisted time series. Rule files can be reloaded at runtime by sending `SIGHUP` to the Prometheus process. citeturn0search1turn0search9

Example:

```yaml
groups:
  - name: app-recording-rules
    rules:
      - record: job:http_requests:rate5m
        expr: sum by (job) (rate(http_requests_total[5m]))
```

This creates a reusable recorded time series.

---

## 5.2 Recording rule naming guidance

The Prometheus practices guide recommends consistent naming for recording rules, generally of the form:

```text
level:metric:operations
```

This makes rule meaning easier to interpret and helps expose bad calculations more clearly. citeturn0search11

---

## 5.3 Alerting rules

Alerting rules define conditions based on PromQL expressions. Whenever the expression returns one or more vector elements at a point in time, the alert becomes active for those label sets. Prometheus sends alerts to Alertmanager, which then handles aggregation, silencing, inhibition, and notification delivery. citeturn0search7turn0search15

Example:

```yaml
groups:
  - name: app-alerts
    rules:
      - alert: HighErrorRate
        expr: (
          sum(rate(http_requests_total{status=~"5.."}[5m]))
          /
          sum(rate(http_requests_total[5m]))
        ) > 0.05
        for: 10m
        labels:
          severity: warning
          team: payments
        annotations:
          summary: "High error rate detected"
          description: "5xx ratio is above 5% for 10 minutes"
```

This rule:
- computes a 5xx ratio
- waits for 10 minutes before firing
- attaches severity and ownership labels
- adds summary and description annotations

---

## 5.4 Alertmanager relationship

Prometheus alerting is separated into two parts:
- Prometheus evaluates alerting rules and sends alerts
- Alertmanager manages those alerts using grouping, silencing, inhibition, and notification routing

This separation is central to the Prometheus alerting model. citeturn0search15

---

## 5.5 Prometheus config for alerting

Example from a practical tutorial pattern:

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - "alerts.yml"

alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - "alertmanager:9093"
```

The official tutorial explains that `evaluation_interval` controls how often rules are evaluated, `rule_files` defines the files with rules, and `alerting` configures Alertmanager. citeturn0search13

---


## 5.6 Alertmanager routing, grouping, silencing, and inhibition

Alertmanager is not just a notification sender. It also handles:
- grouping related alerts into fewer notifications
- silencing alerts during maintenance or incidents
- inhibition rules so one alert can suppress less useful downstream alerts
- routing alerts to the right receivers based on labels

This is one of the major reasons the Prometheus alerting model scales better than sending raw alerts directly. Prometheus Alertmanager documentation explicitly describes grouping, silencing, inhibition, and notification integration behavior. citeturn0search14turn0search2

---

## 5.7 Example Alertmanager routing configuration

Example:

```yaml
route:
  receiver: default-receiver
  group_by: ["alertname", "job", "team"]
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  routes:
    - matchers:
        - severity="critical"
      receiver: critical-receiver
    - matchers:
        - team="payments"
      receiver: payments-receiver

receivers:
  - name: default-receiver
  - name: critical-receiver
  - name: payments-receiver
```

This example shows a practical routing model:
- group related alerts together
- send critical alerts to a special receiver
- route team-owned alerts by label

---

## 5.8 Alertmanager HA and configuration reloads

Prometheus documentation notes that traffic between Prometheus and Alertmanager should not be load-balanced as a single endpoint; instead, Prometheus should point to a list of all Alertmanagers. Alertmanager also supports configuration reloads via its management API or `SIGHUP`. citeturn0search2turn0search16

This matters operationally because:
- HA requires correct topology awareness
- reloading config safely is part of alert operations
- routing changes should be version controlled and validated

---

## 5.9 Example Prometheus alerting block with multiple Alertmanagers

```yaml
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - "alertmanager-1:9093"
            - "alertmanager-2:9093"
```

This reflects the documented pattern of providing Prometheus with the full Alertmanager set rather than hiding them behind a naive load balancer. citeturn0search2

---
# 6) Grafana dashboards and data sources

## 6.1 Data sources

A Grafana data source is a connection to a storage backend that holds data, such as Prometheus, Loki, SQL, or a cloud monitoring system. Grafana queries these data sources and visualizes their results in dashboards and Explore. Grafana includes built-in support for many data sources. citeturn0search6

---

## 6.2 Prometheus as a Grafana data source

A common setup is to configure Prometheus as a Grafana data source and then build dashboards using PromQL queries.

Example provisioning snippet:

```yaml
apiVersion: 1

datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus:9090
    isDefault: true
```

Grafana provisioning supports data source configuration through files and also supports environment variables in provisioning configuration. Grafana documents that environment variables can be used in provisioning configuration, but not inside dashboard definition files themselves. citeturn0search0turn0search6

---

## 6.3 Dashboards

Grafana dashboards are collections of panels that query and transform raw data into visualizations. Queries reduce large datasets into a more manageable dataset for visualization. Grafana dashboard panels depend on the connected data source and its query model. citeturn0search14

---

## 6.4 Good dashboard sections for Prometheus-backed services


## 6.5 Panel transformations and query shaping

In real Grafana use, a dashboard is not just raw query output. Teams often rely on:
- panel-level transformations
- field overrides
- panel titles driven by variables
- better legends and units
- clearer thresholds and display logic

Operationally, this is what turns a technically correct dashboard into a useful one. The exact transformation workflow depends on Grafana’s visualization and panel configuration model.

---

## 6.6 Dashboard folders and organization

Grafana documentation emphasizes folders as the primary way to organize resources. Folder permissions apply to resources inside the folder, including dashboards, alert rules, SLOs, and more. This makes folders central to multi-team dashboard governance. citeturn0search1turn0search7

A practical folder strategy might be:
- Platform
- Shared Services
- Payments
- Customer APIs
- Production Operations
- Staging

---

## 6.7 Dashboard permissions and access control

Grafana supports dashboard and folder permissions. Documentation explains that permissions can allow a viewer to edit and save dashboard changes, or restrict an editor’s ability to modify a dashboard. Grafana also documents server, organization, and folder-oriented permission models. citeturn0search3turn0search9turn0search19

This matters because dashboards are often operational assets, not just personal charts.

---


A useful service dashboard often includes:
- availability (`up`)
- request rate
- latency
- error rate
- CPU and memory
- top instances
- alert state or active incidents
- deployment markers if available

This is a practical operations-oriented dashboard design.

---

# 7) Grafana variables and reusable dashboards

## 7.1 What variables are

Grafana variables are placeholders for values used in dashboard queries, titles, links, and other elements. When a variable changes, Grafana updates every dashboard element using it. Variables are typically displayed as drop-down selections. citeturn0search2

---

## 7.2 Why variables matter

Variables make dashboards:
- reusable across services or environments
- easier to filter
- easier to share
- less repetitive

This is one of the most useful Grafana features.

---

## 7.3 Example variable use

A dashboard variable might hold a `job` value, allowing the same panels to render for different Prometheus jobs.

Example query:

```promql
sum(rate(http_requests_total{job="$job"}[5m]))
```

This is a common dashboard reuse pattern.

---

## 7.4 Add variable types

Grafana documents multiple variable types, including query-based variables and other forms. Variables can be used to filter data, change data sources, set time intervals, and reuse values across queries, titles, and links. citeturn0search8

---

# 8) Grafana alerting

## 8.1 What Grafana Alerting does

Grafana Alerting lets you create queries and expressions from multiple data sources and alert on metrics and logs across those sources. This gives teams flexibility to alert beyond a single backend. citeturn0search10

---

## 8.2 Grafana-managed vs data source-managed alerts

Grafana can display alerting rules from Prometheus data sources in the Alerting UI when the Prometheus data source option to manage alerts via the UI is enabled. These rules are marked as **data source-managed**. For Prometheus data sources, this view is read-only in Grafana; to modify those rules, update the Prometheus rule files directly. citeturn0search16

This distinction is important:

- **Grafana-managed rules** → created and managed in Grafana
- **Prometheus data source-managed rules** → defined in Prometheus rule files and only viewed in Grafana

---

## 8.3 Provisioning alerting resources

Grafana supports file provisioning for alerting resources so that they can be version controlled. At startup, Grafana provisions the resources defined in those configuration files and can create, update, or delete resources in the Grafana instance. Grafana documents that file provisioning for alerting resources is not available in Grafana Cloud, and provisioned resources from files cannot be edited in Grafana. citeturn0search4

Example conceptual alerting provisioning file:

```yaml
apiVersion: 1

groups:
  - orgId: 1
    name: service-alerts
    folder: production
    interval: 1m
    rules:
      - uid: high_error_rate
        title: High Error Rate
        condition: A
        data:
          - refId: A
            datasourceUid: prometheus_uid
            model:
              expr: |
                (
                  sum(rate(http_requests_total{status=~"5.."}[5m]))
                  /
                  sum(rate(http_requests_total[5m]))
                ) > 0.05
        for: 10m
```

This example is illustrative of the structure used in file-based provisioning.

---

# 9) Grafana provisioning

## 9.1 Why provisioning matters

Grafana provisioning is useful because it allows teams to define:
- data sources
- dashboards
- alerting resources

in version-controlled files rather than manual UI steps. This supports reproducibility and consistent environments. citeturn0search0turn0search4

---

## 9.2 Example data source provisioning with environment variables

Grafana documents environment-variable substitution in provisioning configuration files.

Example style:

```yaml
apiVersion: 1

datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus:$PORT
    basicAuth: true
    basicAuthUser: $USER
    secureJsonData:
      basicAuthPassword: $PASSWORD
```

Grafana warns to use environment variables in provisioning configuration, not in dashboard definition files, and notes that `$` is treated as part of its variable replacement behavior. citeturn0search0

---


## 9.3 Provisioning directory structure

Grafana documentation describes classic provisioning using configuration files and indicates specific provisioning directories such as `provisioning/datasources`. This is important because teams usually Git-manage these files and mount them into the Grafana container or host filesystem. citeturn0search5

Typical practical layout:

```text
grafana/
  provisioning/
    datasources/
      prometheus.yml
    dashboards/
      dashboards.yml
  dashboards/
    platform-overview.json
    payments-service.json
```

This is a common GitOps-style pattern.

---

## 9.4 Newer file provisioning model for dashboards

Grafana also documents a newer local file provisioning setup for dashboards in v12+ on on-prem open source and Enterprise deployments. Grafana notes that this newer file provisioning is only for dashboards and does not replace classic provisioning. It is also not available in Grafana Cloud. citeturn0search17

This distinction matters when reading mixed tutorials or migrating older Grafana setups.

---

## 9.5 RBAC and folder-based governance

Grafana documentation includes roles and permissions, folder access control, and RBAC provisioning. Folders are the main way to organize and control access, and Grafana also documents provisioning RBAC assignments through configuration in supported setups. citeturn0search1turn0search9turn0search11

A practical governance model might separate:
- platform administrators
- observability maintainers
- team dashboard editors
- viewers/on-call users

---
# 10) Monitoring best practices

## 10.1 Start with critical signals

For most services, start with:
- availability
- request rate
- latency
- errors
- saturation indicators

This gives a strong first monitoring layer.

---

## 10.2 Keep labels meaningful

Prometheus labels are powerful but can become dangerous if poorly designed.

Good label design should:
- reflect useful operational dimensions
- avoid unnecessary cardinality
- support filtering by service, environment, team, and instance

High-cardinality mistakes can hurt storage, query cost, and dashboard usability.

---

## 10.3 Use recording rules for expensive repeated queries

If a dashboard or alert repeatedly computes a heavy aggregate expression, a recording rule can make it cheaper and easier to reuse. This is an official Prometheus pattern. citeturn0search1turn0search9turn0search11

---

## 10.4 Separate alerting from dashboard curiosity

Dashboards are for exploration and operational visibility. Alerts are for actionable conditions.

A metric that is interesting is not always alert-worthy.

---

## 10.5 Build reusable Grafana dashboards

Use variables, clear naming, and consistent panel design so the same dashboard structure can serve multiple jobs or environments. Grafana variables are a core official feature for this. citeturn0search2turn0search8

---

## 10.6 Keep alerts actionable

Good alerts should:
- point to impact
- avoid flapping
- include ownership labels
- include useful annotations
- use `for` durations to reduce noise where appropriate

This is central to Prometheus alert quality. citeturn0search7turn0search15

---

# 11) Practical dashboard and alert examples

## 11.1 Example service dashboard panels

For a Prometheus-backed API service, common panels are:

- `up{job="$job"}`
- `sum(rate(http_requests_total{job="$job"}[5m]))`
- `histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket{job="$job"}[5m])) by (le))`
- `sum(rate(http_requests_total{job="$job",status=~"5.."}[5m])) / sum(rate(http_requests_total{job="$job"}[5m]))`

This gives:
- availability
- throughput
- p95 latency
- error ratio

---

## 11.2 Example Prometheus alert rules file

```yaml
groups:
  - name: payments-service
    rules:
      - alert: PaymentsServiceDown
        expr: up{job="payments-service"} == 0
        for: 5m
        labels:
          severity: critical
          team: payments
        annotations:
          summary: "Payments service target is down"
          description: "One or more payments-service targets have been unreachable for 5 minutes"

      - alert: PaymentsHighErrorRate
        expr: (
          sum(rate(http_requests_total{job="payments-service",status=~"5.."}[5m]))
          /
          sum(rate(http_requests_total{job="payments-service"}[5m]))
        ) > 0.05
        for: 10m
        labels:
          severity: warning
          team: payments
        annotations:
          summary: "Payments service error rate is high"
          description: "5xx ratio is above 5% for 10 minutes"
```

This is a practical, version-controlled rule file approach.

---

## 11.3 Example Grafana Prometheus dashboard query set


## 11.4 End-to-end practical stack example

A realistic small production pattern looks like this:

1. application exposes `/metrics`
2. `node_exporter` exposes host metrics
3. Prometheus scrapes the app and exporter jobs
4. recording rules precompute expensive rates and aggregates
5. alerting rules detect target down, high error rate, or latency problems
6. Alertmanager groups and routes notifications by severity and team
7. Grafana uses Prometheus as a data source for team and platform dashboards
8. dashboard folders and permissions align with team ownership

This is the practical operating shape of many Grafana + Prometheus environments.

---

## 11.5 Example host and service monitoring combination

Prometheus scrape config concept:

```yaml
scrape_configs:
  - job_name: "node"
    static_configs:
      - targets: ["host1:9100", "host2:9100"]

  - job_name: "payments-service"
    static_configs:
      - targets: ["payments-1:8080", "payments-2:8080"]
```

Grafana dashboard ideas:
- host CPU and memory from `node_exporter`
- service latency and error rate from app metrics
- combined dashboard section correlating host saturation with app latency

This is a practical way to bridge infrastructure and service monitoring.

---

## 11.6 Recording rule strategy

Recording rules are most useful when they support:
- heavy dashboard queries used repeatedly
- shared alerting expressions
- team-wide standard metrics such as request rate or error ratio
- consistent naming and interpretation

Example:

```yaml
groups:
  - name: service-derived-metrics
    rules:
      - record: job:http_requests:rate5m
        expr: sum by (job) (rate(http_requests_total[5m]))
      - record: job:http_requests_5xx:rate5m
        expr: sum by (job) (rate(http_requests_total{status=~"5.."}[5m]))
```

Then dashboards and alerts can use the recorded metrics instead of recomputing everything from scratch.

---

## 11.7 High-cardinality warning signs

One of the biggest Prometheus operational risks is uncontrolled label cardinality.

Common bad patterns:
- per-user IDs in labels
- request IDs in labels
- raw URLs with many unique values
- ephemeral high-volume identifiers

Symptoms:
- memory growth
- slow queries
- hard-to-use dashboards
- expensive storage and remote write pressure

This is one of the most important practical Prometheus design lessons.

---


Useful panel queries:

```promql
up{job="$job"}
```

```promql
sum(rate(http_requests_total{job="$job"}[5m]))
```

```promql
sum(rate(http_requests_total{job="$job",status=~"5.."}[5m]))
/
sum(rate(http_requests_total{job="$job"}[5m]))
```

```promql
histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket{job="$job"}[5m])) by (le))
```

These are practical examples for service health dashboards.

---

# 12) Common operational commands

## 12.1 Prometheus

Validate configuration:

```bash
promtool check config prometheus.yml
```

Validate rule files:

```bash
promtool check rules alerts.yml
```

Reload rules and configuration conceptually by signaling the process:

```bash
kill -HUP <prometheus-pid>
```

The official Prometheus documentation states that rule files can be reloaded by sending `SIGHUP` to the Prometheus process. citeturn0search1

---

## 12.2 Grafana

Grafana is commonly managed through:
- system service commands
- Docker or Kubernetes lifecycle commands
- provisioning file changes
- UI configuration
- APIs depending on the deployment model

Examples on Linux:

```bash
systemctl status grafana-server
sudo systemctl restart grafana-server
journalctl -u grafana-server -f
```

These are operationally common even though Grafana’s official docs for dashboards and provisioning focus more on configuration behavior than on Linux service administration.

---

# 13) Troubleshooting patterns

## 13.1 Prometheus target is down

Check:
- `up` metric
- scrape target URL
- network reachability
- endpoint availability
- metrics path
- exporter process state

---

## 13.2 Alert does not fire

Check:
- PromQL expression correctness
- labels and filters
- whether the condition is actually true
- `for` duration
- rule file loading
- Alertmanager configuration

---

## 13.3 Dashboard panel is empty

Check:
- Grafana data source configuration
- time range
- variable values
- PromQL query
- label filters
- whether Prometheus actually has the series

---

## 13.4 Too many alerts

Check:
- noisy expressions
- missing `for` durations
- lack of aggregation
- duplicated rules
- poor ownership scoping

---

## 13.5 Provisioned settings do not match UI expectations


## 13.6 Dynamic targets are discovered incorrectly

Check:
- service discovery mechanism
- relabel rules
- target metadata labels
- whether expected discovered targets were dropped or rewritten
- job naming consistency

In dynamic environments, bad relabeling is one of the most common reasons monitoring looks incomplete.

---

## 13.7 Alerts fire but routing is wrong

Check:
- alert labels
- Alertmanager route matchers
- receiver names
- grouping behavior
- inhibition or silencing rules
- whether team labels exist on the firing alerts

This is often not an alert-expression problem but a routing-design problem.

---

## 13.8 Grafana dashboard exists but team cannot use it effectively

Check:
- folder permissions
- dashboard permissions
- variable defaults
- panel units and legends
- whether too many raw series are shown without aggregation
- whether the dashboard answers an operational question clearly

A dashboard can be technically correct and still operationally poor.

---

## 13.9 Remote write or long-term storage issues

Check:
- remote storage endpoint reachability
- authentication
- queue or throughput tuning
- high-cardinality metrics causing load
- whether remote write is sending more data than intended

Prometheus documents remote write tuning specifically because production requirements differ and default behavior may need adjustment. citeturn0search4

---


Check:
- provisioning file path
- startup reload behavior
- environment-variable substitution
- whether the resource is provisioned and therefore not UI-editable
- Grafana Cloud vs OSS/Enterprise file-provisioning limitations for alerting

Grafana documents that provisioned alerting resources from files are not editable in Grafana and that file provisioning for alerting resources is unavailable in Grafana Cloud. citeturn0search4

---

# 14) Interview-ready technical explanations

## What is the difference between Prometheus and Grafana?

A strong answer:

> Prometheus is primarily a monitoring and alerting backend that scrapes, stores, and evaluates metric time series, while Grafana is primarily a visualization and alerting platform that queries data sources like Prometheus and presents the results in dashboards and operational views.

---

## What is a scrape job in Prometheus?

A strong answer:

> A scrape job is a logical configuration block that defines how Prometheus collects metrics from one or more targets. It specifies the targets, metrics path, and scrape behavior for a particular monitored service or exporter group.

---

## What is the difference between recording rules and alerting rules?

A strong answer:

> Recording rules precompute PromQL expressions into new stored time series for reuse and efficiency, while alerting rules evaluate PromQL expressions to determine whether alerts should become active and be sent to Alertmanager.

---

## Why are Grafana variables important?

A strong answer:

> Grafana variables make dashboards reusable and interactive. They let teams filter by job, environment, or instance and reuse the same dashboard structure across many services without duplicating panels.

---

## Why should alerts include labels and annotations?


## Why is relabeling important in Prometheus?

A strong answer:

> Relabeling is important because it lets you control which discovered targets are kept, how labels are normalized, and how scrape parameters such as addresses or paths are rewritten. In dynamic environments, relabeling is often the difference between noisy raw discovery and usable monitoring.

---

## Why are folders important in Grafana?

A strong answer:

> Folders are important because they organize dashboards and also control access. Grafana documentation treats folders as the primary way to manage resources and permissions, so a good folder strategy supports both discoverability and governance.

---

## Why is Alertmanager separate from Prometheus rule evaluation?

A strong answer:

> Prometheus evaluates alert expressions, but Alertmanager handles grouping, silencing, inhibition, and notification routing. Keeping those responsibilities separate makes alert delivery more scalable and more operationally manageable.

---

## What is a common Prometheus scaling path?

A strong answer:

> A common scaling path starts with one Prometheus server scraping applications and exporters, then adds better service discovery, recording rules, Alertmanager routing, and eventually federation or remote write for larger environments or long-term storage needs.

---


A strong answer:

> Labels help route and classify alerts, while annotations make alerts understandable by providing human-readable context such as summary and description. Together, they improve ownership, triage, and notification quality.

---

# 15) Official references

Useful sources for deeper learning:

1. Grafana provisioning  
   https://grafana.com/docs/grafana/latest/administration/provisioning/

2. Grafana variables  
   https://grafana.com/docs/grafana/latest/visualizations/dashboards/variables/

3. Grafana add variables  
   https://grafana.com/docs/grafana/latest/visualizations/dashboards/variables/add-template-variables/

4. Grafana data sources  
   https://grafana.com/docs/grafana/latest/datasources/

5. Grafana dashboards  
   https://grafana.com/docs/grafana/latest/visualizations/dashboards/

6. Grafana alerting  
   https://grafana.com/docs/grafana/latest/alerting/

7. Grafana alerting file provisioning  
   https://grafana.com/docs/grafana/latest/alerting/set-up/provision-alerting-resources/file-provisioning/

8. Grafana Prometheus alerting integration  
   https://grafana.com/docs/grafana/latest/datasources/prometheus/alerting/

9. Prometheus configuration  
   https://prometheus.io/docs/prometheus/latest/configuration/configuration/

10. Prometheus configuration overview  
   https://prometheus.io/docs/prometheus/latest/configuration/

11. Prometheus recording rules  
   https://prometheus.io/docs/prometheus/latest/configuration/recording_rules/

12. Prometheus alerting rules  
   https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/

13. Prometheus alerting overview  
   https://prometheus.io/docs/alerting/latest/overview/

14. Prometheus recording rules practices  
   https://prometheus.io/docs/practices/rules/

15. Prometheus getting started  
   https://prometheus.io/docs/prometheus/latest/getting_started/

16. Prometheus alerting tutorial  
   https://prometheus.io/docs/tutorials/alerting_based_on_metrics/

---

17. Prometheus Alertmanager  
   https://prometheus.io/docs/alerting/latest/alertmanager/

18. Prometheus Alertmanager configuration  
   https://prometheus.io/docs/alerting/latest/configuration/

19. Prometheus remote write tuning  
   https://prometheus.io/docs/practices/remote_write/

20. Prometheus service discovery and configuration reference  
   https://prometheus.io/docs/prometheus/latest/configuration/configuration/

21. Prometheus HTTP service discovery  
   https://prometheus.io/docs/prometheus/latest/http_sd/

22. Grafana folder access control  
   https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/folder-access-control/

23. Grafana manage dashboard permissions  
   https://grafana.com/docs/grafana/latest/administration/user-management/manage-dashboard-permissions/

24. Grafana roles and permissions  
   https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/

25. Grafana RBAC provisioning  
   https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/access-control/rbac-grafana-provisioning/

26. Grafana file path setup for local dashboard provisioning  
   https://grafana.com/docs/grafana/latest/as-code/observability-as-code/provision-resources/file-path-setup/

# 16) Final summary

The most important way to understand Grafana and Prometheus is this:

- Prometheus collects and stores time-series metrics
- Prometheus uses scrape jobs, service discovery, relabeling, labels, PromQL, and rule files
- recording rules improve reuse and efficiency
- alerting rules send alerts to Alertmanager, which handles routing, grouping, silencing, and inhibition
- exporters and instrumentation design shape what Prometheus can observe
- Grafana connects to data sources and turns queries into dashboards
- Grafana variables, folders, permissions, and provisioning make dashboards reusable and governable
- Grafana alerting can coexist with Prometheus-managed alert rules
- provisioning and Git-managed configuration make monitoring environments more reproducible
- federation and remote write become important as environments scale
- good monitoring depends on meaningful metrics, sane labels, useful dashboards, and actionable alerts

If you understand:
- scrape configs and service discovery
- relabeling
- PromQL
- recording rules
- alerting rules and Alertmanager routing
- exporters and metric design
- Grafana data sources
- dashboards, variables, and folders
- permissions and provisioning concepts
- practical alert and dashboard design
- scaling patterns such as federation and remote write

then you understand the practical technical core of Grafana and Prometheus at a strong advanced level.
