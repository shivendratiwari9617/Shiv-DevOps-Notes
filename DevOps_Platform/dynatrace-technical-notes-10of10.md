# Dynatrace Technical Notes

## 1. Core Mental Model

Dynatrace should be treated as a **production observability control plane** rather than just a dashboarding tool.

A practical operating model is:

- **entity** = a monitored object such as a host, process group, process group instance, service, application, database, Kubernetes workload, synthetic monitor, or external dependency
- **component** = an operational building block with a real owner, such as a frontend, API, worker, database, queue, broker, or vendor dependency
- **service** = a runtime-traced service Dynatrace detects from traffic and application behavior
- **sub-service** = a critical functional slice inside a broader service, usually represented by key requests, request attributes, service methods, custom services, or filtered transaction views
- **management zone** = a visibility and access boundary used to partition the environment
- **tag** = metadata used for ownership, filtering, automation, and routing
- **Davis event** = an individual detected signal, such as a threshold breach, anomaly, crash, deployment marker, or state change
- **problem** = correlated impact view built from related Davis events
- **SLO** = a service-level objective used to measure health over time

A production-ready Dynatrace layout should answer these questions quickly:

- what is unhealthy
- which entity or service is impacted
- whether users are affected
- what dependency is involved
- what signal triggered detection
- who owns the response
- which runbook or workflow should execute next

---

## 2. Dynatrace Object Model in Production

## 2.1 Entities That Matter Most

The most operationally useful Dynatrace entities usually include:

- hosts
- host groups
- process groups
- process group instances
- services
- applications
- databases
- Kubernetes clusters, namespaces, workloads, and pods
- synthetic monitors and synthetic locations
- external services and dependencies

### practical rule

Do not organize production monitoring around infrastructure alone. Dynatrace becomes much more valuable when the entity model is aligned with:

- business capabilities
- team ownership
- runtime dependency chains
- user-impact paths

## 2.2 Component vs Service vs Sub-Service

### component

A component is the owned production building block.

Examples:

- web storefront
- mobile backend
- auth service
- order service
- payment service
- notification worker
- PostgreSQL database
- Kafka consumer
- Redis cache
- external payment provider

### service

A service is the runtime-traced service Dynatrace detects from request flow and code-level instrumentation.

Examples:

- `prod-order-service`
- `prod-payment-service`
- `prod-auth-service`
- outbound payment-provider call
- database request service
- background message-processing service

### sub-service

A sub-service is a narrower operational slice inside a broader service.

In Dynatrace practice, this is usually represented through:

- key requests
- request attributes
- service methods
- custom services
- filtered transaction views
- SLOs scoped to specific operations
- dashboard or notebook segmentation

Examples inside `prod-payment-service`:

- authorization
- capture
- refund
- fraud-check adapter

Examples inside `prod-order-service`:

- create order
- validate order
- reserve inventory
- publish fulfillment event

### design rule

Create sub-service visibility only where it helps incident response, ownership, SLO tracking, or business transaction monitoring. Avoid exploding every endpoint into its own operational object.

---

## 3. Current Platform Concepts vs Classic Concepts

Dynatrace documentation still contains both newer platform guidance and some areas labeled **Classic**. A strict production reference should distinguish between them.

## 3.1 Concepts still commonly encountered in Classic-labelled docs

These often appear in Classic documentation and may still matter in real environments:

- management zones
- alerting profiles
- key requests
- some request-attribute pages
- SLO pages labeled Classic

## 3.2 Practical interpretation

For production design, the right approach is:

- use the concepts because they remain operationally useful
- be aware that documentation location and UI wording may vary
- avoid writing internal standards that depend on ambiguous naming alone
- define your team’s target operating model in plain language: ownership, service scope, SLOs, event rules, workflows, and routing

## 3.3 Governance rule

When defining internal monitoring standards, document:

- the desired object
- the purpose it serves
- where it appears in your tenant
- whether your environment uses classic-era terminology or current-platform workflows

That removes ambiguity during onboarding and audits.

---

## 4. Service Detection, Key Requests, and Sub-Service Visibility

## 4.1 Auto-Detected Services

Dynatrace automatically detects many services from traffic and tracing behavior.

### production reality

Auto-detection gives fast topology coverage, but the default service boundary is not always the best operational boundary.

Strong teams refine service naming, request visibility, and transaction segmentation so that the monitored topology reflects how incidents are actually triaged.

## 4.2 Key Requests

Key requests are one of the best ways to create meaningful sub-service visibility for critical flows such as:

- login
- checkout
- create order
- payment authorization
- token issuance
- password reset
- account creation

### when to make something a key request

A request should be elevated when it is:

- business-critical
- high-volume or high-value
- latency-sensitive
- a common failure point
- likely to require its own SLO or alert

## 4.3 Request Attributes

Request attributes let you capture useful request dimensions for filtering and analysis.

Examples:

- tenant ID
- partner ID
- API version
- region
- sales channel
- payment method
- customer segment
- order type

### production value

Request attributes are especially important when one broad service supports:

- multiple products
- multiple teams
- multiple customer segments
- multiple partner integrations

They make it possible to answer questions like:

- is only one region affected
- is one payment method failing
- is one tenant generating the issue
- is only one API version regressing

## 4.4 Service Methods and Custom Services

Where broad service-level metrics are too coarse, use service methods or custom service detection to expose the operationally important boundaries.

### good use cases

- a single endpoint has very different latency than the rest
- a payment adapter behaves differently from the rest of the service
- a background worker has meaningful transaction classes
- one shared service needs split visibility by capability

---

## 5. Naming, Tagging, and Rule Design

## 5.1 Naming Standards

Use names that reflect ownership and stable business meaning.

Good examples:

- `prod-order-service`
- `prod-payment-service`
- `prod-auth-service`
- `prod-checkout-journey`
- `prod-orders-postgres`

Avoid:

- `service-1`
- `backend-new`
- `api-main-2`
- `worker-test`
- raw pod-generated names as primary identifiers

## 5.2 Mandatory Tag Schema

Every tier 1 and tier 2 entity should have at least:

- `env:prod`
- `team:<name>`
- `owner:<group>`
- `service_tier:tier1|tier2|tier3`
- `business_service:<capability>`
- `criticality:high|medium|low`
- `runbook:<name>`
- `support:24x7|business-hours`
- `escalation:<policy>`

## 5.3 Auto-Tagging Rule Design Pattern

Auto-tagging rules should be based on durable attributes rather than temporary values.

Prefer matching on:

- Kubernetes namespace
- workload name
- service name pattern
- host group
- process group metadata
- cloud tag
- environment marker

Avoid matching on:

- random pod IDs
- one-off deployment suffixes
- temporary hostnames
- unstable container instance names

## 5.4 Entity Rule Implementation Pattern

For each rule, define:

1. **scope**: what entity types it applies to
2. **condition**: the stable selector or match logic
3. **ownership outcome**: which tags or zone membership it drives
4. **validation**: how you verify the rule matches only intended entities
5. **review cadence**: who reviews drift after topology changes

### example pattern

For `prod-order-service` in Kubernetes:

- scope: services, process groups, workloads, pods
- match: namespace = `orders-prod`
- add tags: `env:prod`, `team:orders`, `service_tier:tier1`, `business_service:checkout`
- zone membership: `prod-orders`
- validate: compare matched entities against deployment inventory
- review: after every namespace or service naming change

---

## 6. Management Zones and Ownership Boundaries

Management zones should be used as operational and access boundaries, not just UI filters.

## 6.1 Recommended Zone Structure

A practical production layout usually includes:

- `prod-customer-experience`
- `prod-orders`
- `prod-payments`
- `prod-platform`
- `prod-data`
- `prod-external-dependencies`

## 6.2 Zone Design Rules

A management zone should map to one or more of:

- a team boundary
- an environment boundary
- a regulated or sensitive data boundary
- a business capability
- a shared dependency domain

## 6.3 Zone Anti-Pattern

Do not create dozens of overlapping zones without clear ownership purpose. Too many zones create confusion in permissions, dashboards, and alert routing.

## 6.4 Ownership Matrix

| Zone | Primary Owner | Typical Entities | Purpose |
|---|---|---|---|
| prod-customer-experience | digital experience | applications, synthetic monitors, frontend services | user journey health |
| prod-orders | orders team | order APIs, workers, DB dependencies | order domain ownership |
| prod-payments | payments team | payment APIs, gateway dependencies | payment visibility and alerting |
| prod-platform | platform/SRE | ingress, cluster services, service mesh, shared APIs | shared platform health |
| prod-data | data/platform | PostgreSQL, Kafka, Redis, pipelines | shared data and messaging |
| prod-external-dependencies | platform + domain teams | vendor APIs, federation, email providers | third-party monitoring |

---

## 7. Topology and Dependency Layout

Every tier 1 service should have a clearly understandable dependency path.

Examples:

- web storefront → API gateway → auth service → identity database
- checkout journey → order service → payment service → payment provider
- order service → Kafka → fulfillment worker → inventory database

## 7.1 Dependency Classes

Classify dependencies explicitly:

- internal tier 1 dependency
- internal platform dependency
- shared data dependency
- external vendor dependency
- optional or degraded-mode dependency

## 7.2 Operational Use

This classification helps determine:

- alert severity
- escalation urgency
- whether a local team can fix the issue directly
- whether the event should be correlated into a larger shared incident

---

## 8. Health Indicators

Health indicators should be chosen by entity type and criticality.

## 8.1 Core Service Indicators

For most services, monitor:

- availability / success rate
- latency p95 and p99
- failure rate
- throughput
- dependency error rate
- restart or crash frequency

## 8.2 Application and Journey Indicators

For user-facing applications:

- synthetic availability
- page or action performance
- JavaScript error rate
- key journey success
- backend dependency failure propagation

## 8.3 Platform and Data Indicators

For platform and data services:

- availability
- saturation
- queue or lag growth
- disk pressure
- slow query behavior
- connection exhaustion
- restart storms

## 8.4 Sub-Service Indicators

For critical sub-services:

- operation success rate
- p95 latency
- timeout share
- fallback usage
- validation failure trend
- dependency-specific failure share

---

## 9. SLO Design

SLOs should exist for customer-critical services and customer-critical transactions.

## 9.1 What Must Have an SLO

At minimum, define SLOs for:

- login
- checkout
- create order
- payment authorization
- API gateway availability
- synthetic user journey success

## 9.2 Recommended SLO Pattern

For each tier 1 capability, define:

- one availability SLO
- one latency SLO if user experience is sensitive
- one transaction-specific SLO when broad service health is too coarse

## 9.3 Example SLO Mapping

| Capability | SLI | Example Target |
|---|---|---:|
| login | successful logins / total login attempts | 99.9% |
| checkout | successful checkout requests / total checkout requests | 99.9% |
| create order | successful create-order requests / total requests | 99.9% |
| payment authorization | successful authorizations / total authorization attempts | 99.95% |
| checkout synthetic | successful synthetic runs / total runs | 99.9% |

## 9.4 SLO Design Rule

A service can look healthy while the one business transaction that matters is failing. That is why transaction-scoped SLOs are essential.

---

## 10. Alerts: Problems, Metric Events, and Routing

A production Dynatrace setup should clearly separate correlated incident detection from threshold-style signal detection.

## 10.1 Use Problems For

- outages
- root-cause-correlated service degradation
- topology-aware incidents
- multi-entity impact
- dependency-linked failures

## 10.2 Use Metric Events For

- pod restart bursts
- consumer lag thresholds
- queue backlog
- certificate-expiry windows
- disk usage
- JVM heap pressure
- custom business metric thresholds

## 10.3 Use Workflow Routing For

- paging the right on-call team
- creating tickets
- enriching notifications
- attaching ownership and runbook metadata
- launching diagnostics
- initiating remediation steps

## 10.4 Alert Severity Model

### critical

Immediate customer, revenue, or authentication impact.

Examples:

- checkout unavailable
- login outage
- payment authorization failure spike
- API gateway outage
- database outage for tier 1 service

### high

Severe degradation likely to become customer-impacting.

Examples:

- p95 latency breach on tier 1 service
- dependency timeout increase
- rising queue lag threatening SLO
- instability on critical pods

### medium

Sustained degradation or capacity concern.

Examples:

- retry growth
- moderate backlog increase
- elevated latency on tier 2 service
- repeated restarts on noncritical workloads

### low

Informational or planned-review signal.

Examples:

- deployment markers
- best-effort dependency anomaly
- low-risk resource trend warnings

## 10.5 Alert Design Rules

Every alert should define:

- target entity or entity class
- severity
- owner
- support window
- escalation policy
- suppression or maintenance behavior
- runbook link
- response expectation

## 10.6 Static Threshold vs Baseline

Use static thresholds for:

- certificate expiry
- lag upper bounds
- restart counts
- queue backlog caps
- disk limits

Use anomaly or baseline-style alerting for:

- latency regressions
- request rate drops
- error-rate deviation
- throughput shifts with seasonality

## 10.7 Example Alert Set

### tier 1 availability

Trigger critical when:

- request success rate drops below 99% for 5 minutes
- or complete outage is detected for 2 minutes

### tier 1 latency

Trigger high when:

- p95 latency exceeds 1.5x baseline for 10 minutes
- or exceeds hard threshold for 5 minutes

### payment dependency

Trigger critical when:

- payment provider failure rate exceeds 10% for 5 minutes
- or timeout rate is above threshold with meaningful volume

### queue backlog

Trigger high when:

- consumer lag grows continuously for 15 minutes
- and processing throughput is below expected baseline

### pod instability

Trigger medium when:

- restart count exceeds threshold in 15 minutes
- or crash-loop behavior appears on tier 1 pods

### synthetic journey

Trigger critical when:

- checkout synthetic monitor fails from multiple locations for 3 consecutive runs

---

## 11. Dashboards and Monitoring Views

## 11.1 Executive / NOC Overview

Show only:

- tier 1 service status
- active critical problems
- SLO compliance
- synthetic critical journeys
- major shared dependency health

## 11.2 Team Service Overview

Per team, show:

- owned services
- open problems
- top metric events
- latency and error trends
- recent deployments
- dependency failures
- saturation impacting owned services

## 11.3 Service Detail View

For each critical service, include:

- golden signals
- key requests
- request-attribute segmentation
- topology dependencies
- logs and traces pivots
- deployment markers
- runbook reference
- owner metadata

## 11.4 Dependency Dashboard

Maintain one dashboard for shared dependencies:

- ingress
- auth
- PostgreSQL
- Kafka
- Redis
- DNS
- external providers

This helps teams quickly tell whether a local incident is actually a shared platform or vendor issue.

---

## 12. Production Layout Example

## 12.1 Service and Ownership Map

| Component | Dynatrace Representation | Sub-Service Representation | Tier | Owner | Key Signals | Alerting |
|---|---|---|---:|---|---|---|
| web-storefront | application + synthetic | checkout journey, login journey | 1 | digital experience | synthetic success, JS errors, load time | problem + synthetic alerts |
| api-gateway | service | auth route, checkout route | 1 | platform API | availability, p95, 5xx | problem + metric events |
| auth-service | service | login, token issue | 1 | identity team | login success, auth errors, latency | problem + SLO alerts |
| order-service | service | create-order key request | 1 | orders team | success rate, DB latency, queue publish success | problem + transaction alerts |
| payment-service | service | authorize, capture, refund | 1 | payments team | gateway success, timeout rate, p95 | problem + dependency alerts |
| orders-postgres | database | n/a | 1 | DB platform | availability, connections, slow queries | metric events + problems |
| kafka-orders | messaging metrics | lag by consumer group | 2 | data platform | lag, retry rate, broker connectivity | metric events |
| notification-worker | service / worker | email-send operation | 3 | messaging team | backlog, retry volume, failures | medium / low metric events |

## 12.2 Alert Routing Metadata Contract

| Field | Example |
|---|---|
| owner team | `team:payments` |
| support model | `support:24x7` |
| tier | `service_tier:tier1` |
| escalation policy | `escalation:payments-primary` |
| runbook | `runbook:payment-auth` |
| business capability | `business_service:checkout` |
| environment | `env:prod` |

---

## 13. Rollout Plan

A strong rollout sequence is:

1. define naming standards
2. define mandatory tag schema
3. define management zones by ownership and environment
4. validate auto-tagging and zone rules
5. identify tier 1 services and dependencies
6. create key requests for business-critical flows
7. add request attributes for meaningful segmentation
8. define SLOs for top transactions
9. separate problem-based alerting from metric-event alerting
10. route notifications through ownership metadata and runbooks
11. validate against real incidents
12. review quarterly for topology drift, ownership drift, and alert noise

---

## 14. Common Anti-Patterns

### 14.1 Monitoring only hosts and CPU

Healthy infrastructure does not prove healthy business transactions.

### 14.2 No key requests for critical flows

This hides important failures inside broad service averages.

### 14.3 No ownership metadata

This slows routing and weakens accountability.

### 14.4 One giant dashboard

This becomes operationally useless.

### 14.5 No distinction between problem alerts and metric events

This creates noisy overlap and unclear response paths.

### 14.6 Mixing prod and non-prod alert channels

This weakens trust in alerts.

### 14.7 Treating every endpoint as its own service

This causes governance and cardinality sprawl.

### 14.8 No transaction-level SLOs

A broad service can be green while the revenue path is red.

---

## 15. Final Framing

Dynatrace becomes 10/10 production-ready when it is organized around:

- real owned components
- clean service boundaries
- sub-service visibility for critical transactions
- clear tagging and management-zone rules
- transaction-aware SLOs
- separate problem and metric-event alert layers
- explicit routing metadata and runbooks

A strong Dynatrace implementation should let an operator determine **what is broken, how Dynatrace represents it, how severe it is, who owns it, and what to do next** within minutes.
