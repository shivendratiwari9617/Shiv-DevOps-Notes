# Grafana and Prometheus Technical Notes

## 1. Core Mental Model

Prometheus and Grafana work best when treated as a **production monitoring and alerting system for operational decisions**, not just as a time-series database plus dashboards.

A practical working model is:

- **Prometheus** = metrics scraping, storage, querying, rule evaluation, and alert generation
- **Alertmanager** = deduplication, grouping, silencing, inhibition, and routing of alerts
- **Grafana** = operator-facing dashboards, alert visualization, annotations, drilldowns, and cross-team monitoring views
- **target** = an endpoint Prometheus scrapes for metrics
- **metric** = a measured value over time
- **label** = key-value metadata used to slice, filter, group, and aggregate time series
- **service** = an owned runtime system such as an API, worker, frontend, database, or broker
- **component** = a production building block with clear operational ownership
- **sub-service** = a critical slice inside a broader service, usually represented by route, operation, consumer group, queue, job type, or dependency path
- **alert** = an actionable condition, not just an unusual number
- **dashboard** = an operator view built to answer a specific operational question quickly

Prometheus official docs define alerting rules, recording rules, naming guidance, and the Alertmanager role in routing, grouping, inhibition, and silencing. Grafana docs define dashboards as panel-based at-a-glance views and document variables, annotations, and dashboard best practices. citeturn0search4turn0search7turn0search1turn0search5turn0search8turn0search2

A production-ready setup should answer:

- what is unhealthy
- which component is affected
- whether the issue is user-facing
- which sub-service or operation is degraded
- what threshold or trend triggered the alert
- who should respond
- what dashboard or runbook should be opened next

---

## 2. Component, Service, and Sub-Service Monitoring Model

## 2.1 Component

A component is the production building block a team owns.

Examples:

- web frontend
- API gateway
- auth service
- order service
- payment service
- inventory worker
- PostgreSQL database
- Kafka consumer
- Redis cache
- ingress controller

A good component definition supports:

- ownership
- health indicators
- alert routing
- dashboard grouping
- runbook mapping

## 2.2 Service

A service is the runtime system being monitored.

Examples:

- `order-service`
- `payment-service`
- `api-gateway`
- `notification-worker`
- `orders-postgres`

In Prometheus, service identity is usually expressed via labels such as:

- `job`
- `service`
- `app`
- `namespace`
- `cluster`
- `team`
- `env`

## 2.3 Sub-Service

A sub-service is a narrower operational slice inside a larger service.

Examples inside `payment-service`:

- authorization
- capture
- refund
- fraud check

Examples inside `order-service`:

- create order
- validate order
- publish event
- reserve inventory

Examples inside `notification-worker`:

- email send
- SMS send
- retry queue processor

In Prometheus, sub-service visibility is commonly expressed through bounded labels such as:

- `route`
- `handler`
- `operation`
- `method`
- `consumer_group`
- `queue`
- `job_type`
- `dependency`

### design rule

Create sub-service monitoring only where it improves actionability. Every extra label dimension has cost.

---

## 3. Prometheus Production Architecture

## 3.1 Scrape and Target Model

Prometheus scrapes targets over HTTP pull by reading exposed metrics endpoints.

Operationally, production teams should define scrape jobs that mirror stable operational boundaries such as:

- cluster
- namespace
- platform service
- application domain
- shared infrastructure tier

### design rule

Scrape configuration should map to discoverability and ownership, not arbitrary app names.

## 3.2 Service Discovery and Label Hygiene

Prometheus works best when service discovery and relabeling produce stable, meaningful labels.

Preferred identity labels:

- `env`
- `cluster`
- `namespace`
- `service`
- `job`
- `team`
- `instance`

Prometheus docs emphasize metric and label naming conventions as best practices, even though they are not strictly required. citeturn0search3turn0search9

### rule

A dashboard or alert should not depend on unstable labels like random pod IDs unless the purpose is per-instance debugging.

## 3.3 High Availability and Rule Separation

For production use, alerting and recording rules should be treated as first-class operational assets.

Recommended separation:

- scrape configs by environment / discovery domain
- recording rules for reusable service metrics
- alerting rules for actionable incidents
- Alertmanager routes by ownership and severity

### value

This keeps dashboards fast, alert logic readable, and team ownership clear.

## 3.4 Recording Rules

Prometheus recording rules precompute frequently used expressions and store the result as new series. Prometheus docs explicitly document recording rules and note per-group limits for produced series and alerts. citeturn0search7

Use recording rules for:

- service success rate
- service error rate
- request rate
- p95 latency
- queue backlog totals
- consumer lag by group
- dependency failure rate

### operational rule

If the same expensive PromQL expression appears in multiple dashboards and alerts, promote it to a recording rule.

## 3.5 Missing Metrics and Absent Signals

A production monitoring design must account for missing signals, not just bad values.

Examples:

- target disappeared
- exporter stopped exposing data
- service stopped emitting traffic
- queue metric vanished after deploy

### practical pattern

Use:

- `up`
- `absent()`
- watchdog-style heartbeat metrics
- synthetic checks for critical user paths

This prevents silent failures where a broken exporter makes a dashboard look “quiet” instead of unhealthy.

---

## 4. Metric Design and Label Safety

## 4.1 Metric Types in Practice

Prometheus metric types usually include:

- **counter** = cumulative value that only increases
- **gauge** = current value
- **histogram** = bucketed distribution with count and sum
- **summary** = client-side quantile and count representation

Prometheus documentation explains histograms and summaries and their tradeoffs. Histograms are generally preferable when you need server-side aggregation across instances. citeturn0search6

## 4.2 Histogram Guidance

For service latency, histograms are usually the best choice.

### practical rules

- use histograms for request duration
- keep bucket layout aligned to real SLO thresholds
- avoid dozens of unnecessary buckets
- keep bucket strategy consistent across similar services
- prefer native histograms where your stack supports them and where that fits your platform standards, as Prometheus docs increasingly emphasize them as advantageous over classic histograms. citeturn0search6turn0search12

## 4.3 Label Cardinality Control

Labels are powerful, but they can become dangerous when overused.

Good labels:

- environment
- cluster
- namespace
- service
- route
- operation
- status_code
- team

Dangerous labels:

- raw user ID
- order ID
- session ID
- request ID
- unbounded full path
- stack trace fragments

### approved sub-service label dimensions

Use only bounded, reviewable dimensions for sub-service monitoring, such as:

- approved route templates
- named operations
- queue names
- consumer groups
- dependency name
- job type

### do not use sub-service labels for

- unique IDs
- free-form customer inputs
- arbitrary URLs
- every downstream hostname if the set is unbounded

---

## 5. Service Health Indicators

Health indicators should be chosen by service type and business importance.

## 5.1 Core Signals

For most services, monitor:

- availability or success rate
- latency
- error rate
- throughput
- saturation

## 5.2 Typical Metrics by Signal

### availability / success

- successful requests
- failed requests
- total requests
- probe success
- synthetic success rate

### latency

- request duration histogram
- dependency call duration histogram
- queue processing duration histogram

### error rate

- HTTP 5xx rate
- gRPC failure rate
- application exception rate
- dependency timeout rate

### throughput

- requests per second
- messages processed per second
- jobs completed per minute

### saturation

- CPU
- memory pressure
- thread pool use
- connection pool use
- queue depth
- consumer lag

## 5.3 Indicators by Component Type

### frontend

- synthetic availability
- backend dependency errors
- page or journey latency
- browser-side errors
- key user-journey success

### API service

- request success rate
- p95/p99 latency
- 5xx rate
- dependency timeout rate
- restart count

### worker / async processor

- backlog
- processing latency
- retry rate
- failure rate
- dead-letter growth

### database

- availability
- query latency
- connection saturation
- replication lag
- disk pressure

### messaging / queue consumer

- consumer lag
- processing throughput
- commit or ack failures
- rebalance frequency
- queue backlog

---

## 6. Practical PromQL Patterns

## 6.1 Success Rate

```promql
sum(rate(http_requests_total{service="order-service",status_code!~"5.."}[5m]))
/
sum(rate(http_requests_total{service="order-service"}[5m]))
```

## 6.2 Error Rate

```promql
sum(rate(http_requests_total{service="payment-service",status_code=~"5.."}[5m]))
/
sum(rate(http_requests_total{service="payment-service"}[5m]))
```

## 6.3 Request Volume

```promql
sum(rate(http_requests_total{service="api-gateway"}[5m]))
```

## 6.4 p95 Latency from Histogram

```promql
histogram_quantile(
  0.95,
  sum by (le) (
    rate(http_request_duration_seconds_bucket{service="auth-service"}[5m])
  )
)
```

## 6.5 Sub-Service Latency

```promql
histogram_quantile(
  0.95,
  sum by (le, route) (
    rate(http_request_duration_seconds_bucket{service="order-service"}[5m])
  )
)
```

## 6.6 Queue Backlog

```promql
sum(queue_messages_ready{queue="email-send"})
```

## 6.7 Consumer Lag

```promql
sum(kafka_consumergroup_lag{consumergroup="orders-processor"})
```

## 6.8 Missing Target

```promql
up{job="payment-service"} == 0
```

## 6.9 Missing Metric

```promql
absent(http_requests_total{service="checkout-service"})
```

### practical rule

Always test queries against real labels before standardizing alerts or dashboards. Most PromQL failures come from inconsistent labeling, not syntax.

---

## 7. Alerting Model

Prometheus alerting rules define the conditions under which alerts fire, and Alertmanager handles deduplication, grouping, routing, silencing, and inhibition. citeturn0search4turn0search1turn0search10turn0search13

## 7.1 Use Prometheus Alerting Rules For

- threshold or state-based conditions
- service-level error and latency breaches
- queue backlog thresholds
- missing target or exporter signals
- dependency-specific failure conditions

## 7.2 Use Alertmanager For

- grouping related alerts
- routing by team or severity
- suppressing lower-level alerts
- controlling repeat behavior
- maintenance silencing

## 7.3 Core Routing Fields

Every actionable alert should include:

- alert name
- service or component
- environment
- severity
- owner team
- runbook reference
- summary
- escalation policy
- dashboard link

Common labels:

- `severity`
- `team`
- `service`
- `env`
- `tier`
- `runbook`
- `pager`
- `namespace`

## 7.4 Routing Tree Design

A practical route tree usually starts with:

1. environment
2. severity
3. owning team
4. special-case services or compliance needs

### example logic

- `env=prod` and `severity=critical` → paging path
- `env=prod` and `severity=high` → on-call or primary Slack + ticket
- `env=stage` → non-paging channel
- `team=payments` → payments receiver
- `team=platform` → platform receiver

## 7.5 Grouping, Repeat, and Noise Control

Alertmanager groups related alerts so responders receive one practical notification instead of hundreds. Prometheus docs explicitly call out grouping as a core function. citeturn0search1turn0search16

Recommended defaults by intent:

- **group by**: `alertname`, `team`, `service`, `env`
- **short group wait** for urgent prod incidents
- **moderate group interval** for updates
- **longer repeat interval** to avoid excessive paging on stable incidents

### rule

Group by response boundary, not by every possible label.

## 7.6 Inhibition Design

Use inhibition to prevent secondary alerts from overwhelming responders.

Examples:

- suppress pod restart alerts when service-down alert is active
- suppress per-instance alerts when service-wide outage alert is firing
- suppress downstream timeout alerts when a shared dependency outage is confirmed

## 7.7 Silence and Maintenance Strategy

Use silence windows for:

- scheduled maintenance
- controlled migrations
- known noisy tests
- incident command decisions during active remediation

### rule

A silence should have:

- owner
- reason
- scope
- expiration time

---

## 8. Practical Alert Rules

## 8.1 Service Availability Alert

```yaml
- alert: ServiceAvailabilityLow
  expr: |
    (
      sum(rate(http_requests_total{service="order-service",status_code!~"5.."}[5m]))
      /
      sum(rate(http_requests_total{service="order-service"}[5m]))
    ) < 0.99
  for: 5m
  labels:
    severity: critical
    team: orders
    service: order-service
    env: prod
    tier: tier1
    runbook: order-service-availability
  annotations:
    summary: "order-service availability below 99%"
    description: "The order-service success rate has dropped below 99% for 5 minutes."
```

## 8.2 High Latency Alert

```yaml
- alert: PaymentServiceLatencyHigh
  expr: |
    histogram_quantile(
      0.95,
      sum by (le) (
        rate(http_request_duration_seconds_bucket{service="payment-service"}[5m])
      )
    ) > 0.75
  for: 10m
  labels:
    severity: high
    team: payments
    service: payment-service
    env: prod
    tier: tier1
    runbook: payment-service-latency
  annotations:
    summary: "payment-service p95 latency is high"
    description: "payment-service p95 latency has exceeded 750ms for 10 minutes."
```

## 8.3 Error Burst Alert

```yaml
- alert: AuthServiceErrorRateHigh
  expr: |
    (
      sum(rate(http_requests_total{service="auth-service",status_code=~"5.."}[5m]))
      /
      sum(rate(http_requests_total{service="auth-service"}[5m]))
    ) > 0.05
  for: 5m
  labels:
    severity: critical
    team: identity
    service: auth-service
    env: prod
    tier: tier1
    runbook: auth-service-errors
  annotations:
    summary: "auth-service 5xx error rate above 5%"
    description: "auth-service has sustained 5xx errors above 5% for 5 minutes."
```

## 8.4 Queue Backlog Alert

```yaml
- alert: NotificationQueueBacklogGrowing
  expr: queue_messages_ready{queue="email-send"} > 5000
  for: 15m
  labels:
    severity: high
    team: messaging
    service: notification-worker
    env: prod
    tier: tier2
    runbook: notification-queue-backlog
  annotations:
    summary: "email-send queue backlog is high"
    description: "Queue depth has remained above 5000 for 15 minutes."
```

## 8.5 Consumer Lag Alert

```yaml
- alert: OrdersConsumerLagHigh
  expr: sum(kafka_consumergroup_lag{consumergroup="orders-processor"}) > 10000
  for: 15m
  labels:
    severity: high
    team: data-platform
    service: kafka-orders
    env: prod
    tier: tier2
    runbook: consumer-lag
  annotations:
    summary: "orders-processor consumer lag is high"
    description: "Consumer lag has exceeded 10000 for 15 minutes."
```

## 8.6 Target Missing Alert

```yaml
- alert: PaymentServiceTargetMissing
  expr: up{job="payment-service"} == 0
  for: 5m
  labels:
    severity: critical
    team: payments
    service: payment-service
    env: prod
    tier: tier1
    runbook: payment-service-target-missing
  annotations:
    summary: "payment-service scrape target is down"
    description: "Prometheus cannot scrape one or more payment-service targets."
```

---

## 9. Alertmanager Policy Design

## 9.1 Recommended Route Policy Layers

Build routing from coarse to fine:

- route 1: environment
- route 2: severity
- route 3: team
- route 4: special service exceptions
- route 5: fallback receiver

## 9.2 Recommended Paging Policy by Tier

| Service Tier | Severity | Typical Action |
|---|---|---|
| tier1 | critical | immediate paging |
| tier1 | high | on-call page or urgent chat + ticket |
| tier2 | critical | page if customer-impacting |
| tier2 | high | urgent team channel + ticket |
| tier3 | medium/low | non-paging channel |

## 9.3 Inhibition Hierarchy Example

- `ServiceDown` inhibits `HighLatencyInstance`
- `SharedDatabaseUnavailable` inhibits `DependentServiceTimeout`
- `NamespaceOutage` inhibits `PodRestartHigh`

## 9.4 Maintenance Rule

Never rely only on “people remembering” planned work. Use silences with clear scopes and expiration.

---

## 10. Grafana Dashboard Design

Grafana docs describe dashboards as panel-based at-a-glance views and document best practices, variables, and annotations. Variables let one control queries and labels dynamically across dashboard elements, and annotations let teams overlay events like deploys on graphs. citeturn0search8turn0search5turn0search2turn0search11

## 10.1 Dashboard Design Principles

A dashboard should answer one main question, such as:

- is the service healthy
- which dependency is failing
- which sub-service is degraded
- what changed recently
- what should on-call inspect next

## 10.2 Standard Dashboard Layers

A strong production setup usually includes:

- executive / NOC overview
- team service overview
- service detail dashboard
- sub-service / transaction dashboard
- shared dependency dashboard
- incident investigation dashboard

## 10.3 Dashboard Usability Standards

Recommended standards:

- keep overview dashboards compact and scannable
- avoid overcrowding; use rows and drilldowns instead of one giant board
- place status and availability panels first
- use consistent units and naming
- use consistent time ranges per dashboard purpose
- include annotations for deploys, incidents, and maintenance
- add links to logs, traces, runbooks, and downstream dashboards where available

## 10.4 Standard Time Windows

Recommended defaults:

- executive / NOC: last 1h to 6h
- team overview: last 6h to 24h
- service detail: last 1h with zoom-out option to 24h
- capacity / trend dashboard: 7d to 30d

## 10.5 Variable Design

Use variables for:

- environment
- cluster
- namespace
- team
- service
- route
- instance

### warning

Do not chain so many variables that dashboards become slow or confusing.

---

## 11. Practical Dashboard Layouts

## 11.1 Executive / Operations Dashboard

Purpose: quickly assess production risk.

Show:

- tier 1 service health
- active alert counts by severity
- service availability scorecards
- top latency regressions
- top error spikes
- critical dependency health
- synthetic or uptime checks

## 11.2 Team Service Overview Dashboard

Purpose: support one team’s operational ownership.

Show:

- owned services
- availability
- latency
- error rate
- throughput
- recent deploy annotations
- saturation
- top alerting components
- dependency health

### standard row structure

- row 1: summary stats
- row 2: availability / error / latency
- row 3: throughput and saturation
- row 4: dependency health
- row 5: active alerts and recent annotations

## 11.3 Service Detail Dashboard

Purpose: investigate one service deeply.

Show:

- request volume
- success rate
- p95/p99 latency
- error breakdown by code
- restarts
- CPU and memory
- dependency latency
- request duration by route
- top failing operations

## 11.4 Sub-Service Dashboard

Purpose: isolate critical operations inside a broader service.

For `payment-service`, show:

- authorization success rate
- capture success rate
- refund error rate
- payment provider latency
- timeout trend by operation
- dependency-specific failures

For `order-service`, show:

- create-order latency
- validation failures
- publish-event failures
- inventory reservation time
- route-level request volume

## 11.5 Dependency Dashboard

Show shared dependency health for:

- ingress
- auth
- PostgreSQL
- Kafka
- Redis
- DNS
- external providers

This helps teams quickly determine whether a local issue is actually a shared platform incident.

---

## 12. Grafana Panel Standards

## 12.1 Best Panel Types

Use:

- **stat panels** for current health snapshots
- **time series** for trends
- **tables** for top failing routes, pods, or instances
- **heatmaps** for latency distribution
- **state timeline** when state changes matter
- **bar gauges** sparingly for ranked comparisons

## 12.2 Recommended Panel Ordering

1. overall availability / success rate
2. latency
3. error rate
4. throughput
5. saturation
6. dependency health
7. sub-service detail
8. infrastructure detail

## 12.3 Visual Consistency Rules

Use:

- the same color semantics across dashboards
- the same units for similar metrics
- the same service names everywhere
- the same threshold colors for the same KPI classes

---

## 13. Component and Sub-Service Monitoring Standards

## 13.1 Component-Level Monitoring Contract

For each component, define:

- owner
- tier
- service list
- primary health indicators
- alert thresholds
- dashboard link
- runbook link

## 13.2 Sub-Service Monitoring Rules

Create sub-service dashboards or alerts only when the operation is:

- business-critical
- operationally distinct
- failure-prone
- high-volume or high-value
- backed by bounded labels

## 13.3 When Not to Alert at Sub-Service Level

Do not create sub-service alerts when:

- the same issue is already captured at service level
- volume is too low for reliable thresholds
- the label dimension is unstable
- responders cannot act differently at that granularity

### better alternative

Dashboard the sub-service, but keep paging at service or component level.

## 13.4 Example Mapping

| Service | Sub-Service | Key Metrics | Alert Strategy |
|---|---|---|---|
| order-service | create-order | success rate, p95 latency | alert if revenue path is impacted |
| order-service | reserve-inventory | timeout rate, dependency latency | usually correlate with dependency alerts |
| payment-service | authorize | success rate, dependency failures | page on sustained failure |
| payment-service | refund | error rate, processing time | dashboard first, alert if business-critical |
| notification-worker | email-send | queue depth, failure rate | high alert, usually not paging |

---

## 14. Recording Rules and Query Reuse

Use recording rules for expensive or repeated expressions.

Useful candidates:

- service success rate
- service error rate
- p95 latency
- queue backlog totals
- consumer lag by group
- dependency error rate
- service saturation summaries

### value

Recording rules improve:

- dashboard speed
- alert readability
- query consistency
- team reuse

### implementation rule

A dashboard query should be either:
- simple enough to read directly, or
- backed by a clearly named recording rule

---

## 15. Common Anti-Patterns

### 15.1 Alerting on raw infrastructure without service context

This creates noise.

### 15.2 Unbounded label cardinality

This damages Prometheus performance.

### 15.3 One giant dashboard for everything

This slows operators down.

### 15.4 No ownership labels

Alert routing becomes confused.

### 15.5 No absent-metric detection

Broken exporters can hide incidents.

### 15.6 No inhibition or grouping

Large incidents turn into alert floods.

### 15.7 No recording rules for repeated heavy queries

Dashboards become slow and inconsistent.

### 15.8 Over-alerting at sub-service level

Teams get paged for slices they cannot act on separately.

---

## 16. Practical Rollout Plan

A strong rollout sequence is:

1. define service and component naming standards
2. standardize labels and approved sub-service dimensions
3. map service tiers and ownership
4. define core health indicators by component type
5. implement recording rules for service KPIs
6. create alert rules with ownership labels and runbook metadata
7. configure Alertmanager routing, grouping, inhibition, and silences
8. build executive, team, service, and sub-service dashboards
9. add annotations for deploys and incidents
10. test alerts with controlled failures
11. tune thresholds, routes, and dashboards quarterly

---

## 17. Final Framing

Prometheus and Grafana become production-ready when they are organized around:

- clear component ownership
- meaningful service and sub-service visibility
- bounded label design
- strong service health indicators
- alert routing tied to action
- dashboard standards built for operator speed
- recording rules for reuse and scale
- explicit absent-signal detection

A good setup should let an operator understand **what is unhealthy, which service or sub-service is failing, who owns it, what signal triggered the alert, and what to inspect next** within minutes.
