# Three End-to-End Deployment Notes on Google Cloud
## 10/10 Senior GCP Cloud Architect and Engineer Notes

These notes are written as an implementation-grade blueprint for engineering teams building real-time platforms on Google Cloud. They cover three production-ready deployment patterns:

1. **GKE-centric real-time digital commerce platform**
2. **Cloud Run-centric real-time API and event processing platform**
3. **Hybrid GKE + Cloud Run platform for mixed workload estates**

The goal is not to use every Google Cloud service blindly. The goal is to place each workload on the right runtime, pair it with the right data services, secure it properly, operate it reliably, and control cost.

---

# 1. Executive Decision Framework

## 1.1 Runtime Selection

### Choose GKE when
- you need sidecars, daemonsets, service mesh, or custom controllers
- services are long-lived and high-throughput
- you need pod-level or node-level tuning
- there are complex east-west traffic patterns
- you need special compute classes such as GPU, local SSD, taints/tolerations, or workload isolation
- you want portability across Kubernetes-based environments

### Choose Cloud Run when
- workloads are stateless
- traffic is bursty or unpredictable
- you want fast developer velocity
- you want minimal platform operations
- services are request/response or event-driven
- scale-to-zero is useful
- the team does not need Kubernetes-level control

### Choose Hybrid when
- core low-latency or stateful services need GKE
- public APIs, webhooks, scheduled jobs, and burst workers fit Cloud Run better
- workload economics differ sharply across components
- the platform team is mature enough to govern two runtimes coherently

## 1.2 GKE Standard vs GKE Autopilot

| Decision Point | GKE Standard | GKE Autopilot |
|---|---|---|
| Node pool control | Full | Limited |
| Specialized compute | Best choice | Constrained |
| Deep networking/service mesh control | Best | Good |
| Platform ops burden | Higher | Lower |
| Pod-centric cost model | Moderate | Strong |
| Best fit | complex real-time core | simpler internal K8s workloads |

### Senior Architect Rule
- **GKE Standard** for business-critical real-time platforms
- **GKE Autopilot** for selected internal services where Kubernetes is still needed but cluster operations should be minimized
- **Cloud Run** whenever Kubernetes complexity does not provide business value

---

# 2. Enterprise Naming and Tagging Standards

Consistency reduces operational entropy.

## 2.1 Resource Naming Convention

Use:

```text
<org>-<env>-<domain>-<service>-<region>-<suffix>
```

Examples:

```text
acme-prod-commerce-gke-uscentral1-cluster01
acme-prod-commerce-checkout-sa
acme-prod-net-sharedvpc
acme-stage-api-webhook-run
acme-prod-data-alloydb-primary
```

## 2.2 Project Naming

```text
acme-prd-net-host
acme-prd-commerce-app
acme-prd-commerce-data
acme-prd-security
acme-nonprod-dev-commerce
acme-nonprod-stage-commerce
acme-shared-cicd
acme-shared-observability
```

## 2.3 Subnet Naming

```text
sn-prod-gke-app-uscentral1
sn-prod-data-uscentral1
sn-prod-mgmt-uscentral1
sn-stage-serverless-uscentral1
```

## 2.4 Service Account Naming

```text
sa-checkout-runtime
sa-orders-runtime
sa-cloudbuild-deployer
sa-terraform-platform
sa-breakglass-admin
```

## 2.5 Kubernetes Namespace Naming

```text
ns-platform-system
ns-commerce-edge
ns-commerce-orders
ns-commerce-payments
ns-observability
```

## 2.6 Required Labels / Tags

Apply to every supported resource:

- `env`: dev | test | stage | prod
- `owner`: team name
- `cost_center`: finance code
- `service`: application/service name
- `criticality`: tier0 | tier1 | tier2
- `data_classification`: public | internal | confidential | restricted
- `managed_by`: terraform | platform | manual-approved
- `region`: deployment region

---

# 3. Enterprise Landing Zone Reference

## 3.1 Organization Model

```text
Organization
|
+-- Platform
|   +-- Networking
|   +-- Security
|   +-- Shared Services
|   +-- Observability
|
+-- NonProd
|   +-- Dev
|   +-- Test
|   +-- Stage
|
+-- Prod
|   +-- App Domains
|   +-- Data Domains
|   +-- Integration Domains
|
+-- Sandbox
```

## 3.2 Shared Services Baseline

- centralized DNS
- Certificate Manager
- Artifact Registry
- Cloud Build and deployment integration
- centralized logging sinks
- Security Command Center aggregation
- billing export to BigQuery
- common Terraform module repositories
- common IAM groups
- org policy management

## 3.3 Shared VPC Pattern

Use a **host project** for:
- VPCs
- subnets
- routes
- firewall policies
- Cloud NAT
- Cloud Router
- Private Service Connect
- private service access

Use **service projects** for:
- GKE clusters
- Cloud Run services
- Pub/Sub
- BigQuery
- databases
- application-specific services

## 3.4 Environment Separation

Use separate projects for:
- dev
- test
- stage
- prod

Do not rely only on namespaces for production isolation when compliance, IAM blast radius, and billing clarity matter.

## 3.5 Logging and Security Aggregation

- route audit logs centrally
- aggregate SCC findings centrally
- export billing data centrally to BigQuery
- maintain separate retained logs for:
  - security
  - platform operations
  - application diagnostics

---

# 4. Reference Terraform Structure

## 4.1 Recommended Repo Layout

```text
platform-live/
  environments/
    prod/
      global/
      us-central1/
      us-east1/
    nonprod/
      dev/
      test/
      stage/
  stacks/
    networking/
    security/
    observability/
    gke/
    cloudrun/
    data/
    messaging/
    cicd/

platform-modules/
  project_factory/
  shared_vpc/
  org_policies/
  iam_baseline/
  gke_cluster/
  gke_nodepool/
  cloud_run_service/
  pubsub_topic_subscription/
  bigquery_dataset/
  alloydb/
  cloudsql/
  firestore/
  redis/
  logging_sinks/
  monitoring_baseline/
  kms_keyring/
  secret_manager/
  cloud_armor/
```

## 4.2 Module Layering

### Layer 0: Organization and guardrails
- folders
- billing
- org policies
- logging aggregation
- SCC baseline

### Layer 1: Foundation networking
- host projects
- Shared VPC
- subnets
- NAT
- routers
- DNS
- PSC
- hybrid connectivity

### Layer 2: Security and observability
- IAM groups
- KMS
- secret access model
- log sinks
- monitoring workspace standards
- alert channels

### Layer 3: Runtime
- GKE clusters
- node pools
- Cloud Run service definitions
- Artifact Registry repos

### Layer 4: Data and messaging
- AlloyDB / Cloud SQL / Firestore / Bigtable / Redis
- Pub/Sub
- BigQuery
- Storage buckets

### Layer 5: Application delivery
- namespaces
- service identities
- deployment policies
- release automation

## 4.3 Sample Terraform Snippets

### Project Creation
```hcl
module "commerce_app_project" {
  source          = "../modules/project_factory"
  project_id      = "acme-prd-commerce-app"
  project_name    = "acme-prd-commerce-app"
  billing_account = var.billing_account
  folder_id       = var.prod_folder_id
  labels = {
    env        = "prod"
    owner      = "commerce-platform"
    managed_by = "terraform"
  }
}
```

### Shared VPC Attachment
```hcl
resource "google_compute_shared_vpc_service_project" "commerce_attach" {
  host_project    = var.host_project_id
  service_project = module.commerce_app_project.project_id
}
```

### GKE Cluster Baseline
```hcl
module "gke_core" {
  source                 = "../modules/gke_cluster"
  project_id             = module.commerce_app_project.project_id
  name                   = "acme-prod-commerce-core-uscentral1"
  region                 = "us-central1"
  network                = "acme-prod-sharedvpc"
  subnetwork             = "sn-prod-gke-app-uscentral1"
  private_cluster        = true
  enable_workload_identity = true
  release_channel        = "REGULAR"
  master_authorized_networks = var.admin_cidrs
  logging_components     = ["SYSTEM_COMPONENTS", "WORKLOADS"]
  monitoring_components  = ["SYSTEM_COMPONENTS", "APISERVER", "SCHEDULER"]
}
```

### Cloud Run Baseline
```hcl
module "notification_run" {
  source       = "../modules/cloud_run_service"
  project_id   = module.commerce_app_project.project_id
  name         = "notify-api"
  region       = "us-central1"
  image        = "us-central1-docker.pkg.dev/acme-shared-cicd/commerce/notify:v12"
  ingress      = "INGRESS_TRAFFIC_INTERNAL_LOAD_BALANCER"
  max_scale    = 200
  min_scale    = 2
  concurrency  = 40
  service_account = "sa-notify-runtime@acme-prd-commerce-app.iam.gserviceaccount.com"
}
```

---

# 5. IAM Reference Model

## 5.1 Role Separation by Persona

| Persona | Responsibility | Access Pattern |
|---|---|---|
| Platform Admin | GKE, networking, platform ops | elevated, tightly limited |
| Security Admin | SCC, KMS, policies, audit review | security tools only |
| Network Admin | VPC, firewall, NAT, DNS, interconnect | network-scoped |
| Developer | app code, non-prod deploy, logs | least privilege |
| Release Manager | approvals, rollout governance | deploy pipeline controls |
| CI/CD Service Account | build and deploy automation | no interactive login |
| Runtime Service Accounts | service-to-service and service-to-GCP auth | one per service |
| Auditor | read-only evidence and logs | no mutation rights |
| Break-Glass Admin | emergency actions | approval-based and monitored |

## 5.2 Senior IAM Rules

- no shared broad editor roles
- no long-lived service account keys unless explicitly approved and rotated
- use workload identities everywhere possible
- runtime SA must not be reused across unrelated services
- production admin access should be group-based and time-bound
- CI/CD identities should deploy only what they own

## 5.3 Example Workload Identity Mapping

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: checkout-sa
  namespace: ns-commerce-orders
  annotations:
    iam.gke.io/gcp-service-account: sa-checkout-runtime@acme-prd-commerce-app.iam.gserviceaccount.com
```

---

# 6. Security Baseline

## 6.1 Core Controls

- **Cloud Armor** for WAF, IP reputation, geo controls, bot/rate limits
- **reCAPTCHA Enterprise** for fraud-prone entry points
- **Secret Manager** for application secrets
- **Cloud KMS** for CMEK where required by classification/compliance
- **Artifact Registry** for containers
- vulnerability scanning on build artifacts
- **Binary Authorization** for trusted deploys
- **Security Command Center** for central security posture
- **Cloud Audit Logs** aggregated centrally
- **Organization Policies** to prevent risky resource creation
- **NetworkPolicy** in GKE
- pod security standards and admission controls
- optional **Cloud IDS** or partner NDR/SIEM integration in higher maturity environments

## 6.2 Supply Chain Security Flow

```text
Developer Commit
      |
      v
Source Control
      |
      v
Cloud Build
  - unit tests
  - SAST
  - dependency scan
  - container build
      |
      v
Artifact Registry
  - vulnerability scan
      |
      v
Image Signing
      |
      v
Binary Authorization / Policy Gate
      |
      v
Cloud Deploy / Approved Rollout
      |
      v
GKE / Cloud Run
```

## 6.3 Binary Authorization / Policy Gate Example

```text
Policy gates before prod:
1. image digest pinned
2. image signed by approved attestor
3. critical vulnerabilities below approved threshold
4. IaC policy checks passed
5. deployment approved by release manager
6. prod change window respected
```

## 6.4 Secret Rotation Standard

- database passwords rotated on a schedule
- API keys versioned in Secret Manager
- services consume latest stable secret version through controlled rollout
- no secrets hardcoded in images or manifests
- certificate renewal tracked and tested before expiry

---

# 7. Observability and SRE Model

## 7.1 Required Log Fields

All application logs should include:

- `timestamp`
- `severity`
- `service`
- `env`
- `region`
- `version`
- `trace_id`
- `request_id`
- `event_id`
- `user_id` when permitted
- `tenant_id` when applicable
- `business_operation`
- `outcome`
- `latency_ms`

## 7.2 Tracing and Correlation

Use one correlation strategy across all runtimes:
- propagate trace headers across HTTP/gRPC
- copy correlation IDs into Pub/Sub message attributes
- log the same IDs in downstream consumers
- preserve event IDs through retries

## 7.3 SLI / SLO Templates

### API SLI Examples
- availability
- p95 latency
- p99 latency
- 5xx rate
- auth failure rate

### Event Pipeline SLI Examples
- publish-to-process latency
- backlog age
- dead-letter growth
- duplicate processing rate

### Sample SLOs
- checkout API availability: 99.95%
- checkout p95 latency: < 250 ms
- order event completion: 99% within 60 seconds
- webhook intake acknowledgment: 99.9% within 2 seconds

## 7.4 Alert Severity Model

| Severity | Meaning | Response Expectation |
|---|---|---|
| Sev1 | customer-critical outage | immediate paging |
| Sev2 | major degradation or data risk | rapid engineer response |
| Sev3 | partial issue / rising error budget burn | business-hours or on-call review |
| Sev4 | warning / hygiene | backlog or maintenance |

## 7.5 Dashboard Hierarchy

1. executive/customer journey dashboards
2. business capability dashboards
3. platform health dashboards
4. dependency dashboards
5. deep service diagnostics

## 7.6 Runbook Minimum Standard

Each runbook should contain:
- alert definition
- impact description
- detection metric
- immediate triage steps
- rollback or mitigation steps
- escalation path
- validation steps
- post-incident follow-up checklist

---

# 8. Pub/Sub and Eventing Reference Design

## 8.1 Naming Standard

```text
topic.<domain>.<entity>.<event>
sub.<consumer>.<event>
dlq.<consumer>.<event>
sch.<event-schema-name>
```

Examples:

```text
topic.commerce.order.created
sub.inventory.order-created
dlq.inventory.order-created
```

## 8.2 Event Design Rules

- events must be immutable
- consumers must be idempotent
- use explicit schema versioning
- avoid overloaded “misc” events
- include event ID, source, type, timestamp, trace ID, schema version
- partition topics by domain, not by every microservice

## 8.3 Retry and Dead-Letter Guidance

- retry transient failures automatically
- send poison messages to DLQ after max delivery attempts
- expose DLQ growth as an alert
- design replay procedure and validate it quarterly

## 8.4 Ordering Guidance

Use message ordering only when required. It reduces consumer flexibility and can become a throughput bottleneck. Most business workflows should rely on idempotency and version checks instead of global ordering assumptions.

## 8.5 Consumer Pseudocode

```python
def handle_message(msg):
    event = parse(msg)
    if already_processed(event.event_id):
        ack(msg)
        return

    validate_schema(event)
    execute_business_logic(event)
    record_processed(event.event_id)
    ack(msg)
```

---

# 9. Architecture 1: GKE-Centric Real-Time Digital Commerce Platform

## 9.1 Business Scenario

A global commerce company requires:
- storefront browsing
- personalized recommendations
- cart and checkout
- pricing and promotions
- order management
- payment orchestration
- fraud checks
- inventory sync
- notifications
- near-real-time analytics

## 9.2 Why GKE is Primary

This platform needs:
- multiple microservices with uneven scaling patterns
- pod-level tuning
- service mesh optionality
- advanced deployment control
- strong east-west traffic management
- deep integration with eventing and data systems

## 9.3 End-to-End Flow

1. users hit the global load balancer
2. Cloud Armor protects and filters traffic
3. static assets come from Cloud Storage via CDN
4. dynamic requests enter GKE through Gateway/Ingress
5. services call Redis, AlloyDB/Cloud SQL, Firestore as needed
6. order lifecycle events publish to Pub/Sub
7. Cloud Run handles notifications or burst side workflows
8. Dataflow moves streams to BigQuery
9. Vertex AI serves recommendations or fraud scoring if justified

## 9.4 Detailed Deployment Topology

### Regions
- primary region: `us-central1`
- secondary DR region: `us-east1`

### Cluster Strategy
- one regional production cluster per region
- separate non-prod clusters
- dedicated node pools:
  - edge services
  - core business services
  - async consumers
  - platform/system
  - optional ML/compute heavy pool

### Example Topology
```text
prod-region-uscentral1
  gke-commerce-core
    np-edge
    np-core
    np-async
    np-system

prod-region-useast1
  gke-commerce-dr
    warm-standby or active-active subset
```

## 9.5 Network Design

- Shared VPC host project
- private GKE clusters
- master authorized networks for admin access
- private database IP only
- Internal Load Balancers for internal front doors
- External HTTP(S) Load Balancer for internet
- Cloud NAT for egress
- Private Service Connect where managed service private reachability is desired

## 9.6 Core GKE Manifest Patterns

### Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: checkout-service
  namespace: ns-commerce-orders
spec:
  replicas: 4
  selector:
    matchLabels:
      app: checkout-service
  template:
    metadata:
      labels:
        app: checkout-service
        version: v1
    spec:
      serviceAccountName: checkout-sa
      containers:
      - name: checkout
        image: us-central1-docker.pkg.dev/acme-shared-cicd/commerce/checkout@sha256:abcd1234
        ports:
        - containerPort: 8080
        envFrom:
        - configMapRef:
            name: checkout-config
        resources:
          requests:
            cpu: "500m"
            memory: "512Mi"
          limits:
            cpu: "1"
            memory: "1Gi"
```

### Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: checkout-service
  namespace: ns-commerce-orders
spec:
  selector:
    app: checkout-service
  ports:
  - port: 80
    targetPort: 8080
```

### HPA
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: checkout-hpa
  namespace: ns-commerce-orders
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: checkout-service
  minReplicas: 4
  maxReplicas: 30
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 65
```

### PodDisruptionBudget
```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: checkout-pdb
  namespace: ns-commerce-orders
spec:
  minAvailable: 3
  selector:
    matchLabels:
      app: checkout-service
```

### NetworkPolicy
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-checkout-to-orders-db-proxy
  namespace: ns-commerce-orders
spec:
  podSelector:
    matchLabels:
      app: checkout-service
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          name: ns-commerce-orders
```

## 9.7 CI/CD and Release Model

- Cloud Build for build/test/package
- Artifact Registry for image storage
- signed immutable digests
- Cloud Deploy for environment promotion
- canary release for checkout/pricing
- rollback on latency or error-budget burn

### Branching Standard
- feature branches
- protected `main`
- release tags for production promotions
- no direct prod manifest edits

## 9.8 Data Placement

| Workload | Recommended Service | Reason |
|---|---|---|
| Orders / checkout | AlloyDB | fast relational OLTP |
| simpler relational admin workloads | Cloud SQL | lower operational overhead |
| sessions / carts | Memorystore | ultra-fast transient state |
| user preferences / flexible metadata | Firestore | document model |
| extreme telemetry scale | Bigtable | high-throughput key access |
| analytics | BigQuery | analytical scale |
| media / exports | Cloud Storage | durable object store |

## 9.9 Multi-Region Strategy

### Option A: Active/Passive
- all writes in primary region
- warm standby in secondary
- lower complexity
- best for most enterprises

### Option B: Selective Active/Active
- browse/catalog active in both regions
- checkout primary with controlled failover
- events replicated or replay-ready
- higher complexity

### Recovery Sequence
1. confirm primary impairment
2. stop unsafe writes if partial data inconsistency risk exists
3. promote secondary DB or redirect to replica
4. shift traffic by LB / DNS policy
5. validate queue consumers and downstream integrations
6. re-enable writes and monitor error budget

## 9.10 HA / DR Targets

| Tier | Service | RTO | RPO |
|---|---|---:|---:|
| Tier0 | checkout, order commit | 15 min | 0–5 min |
| Tier1 | cart, inventory, pricing | 30 min | 5–15 min |
| Tier2 | notifications, reporting | 4 hr | 1 hr |

## 9.11 Cost Drivers

- steady GKE node pools
- AlloyDB HA and storage
- Redis memory footprint
- Dataflow streaming
- egress from CDN / LB / downstream APIs
- BigQuery query consumption

## 9.12 Common Failure Modes

| Symptom | Likely Cause | Detection | Mitigation |
|---|---|---|---|
| checkout latency spikes | DB connection exhaustion | DB connection metrics, p95 latency | pool tuning, read/write split, HPA |
| cart failures during sale | Redis saturation | Redis CPU/memory | scale tier, TTL review, hot key mitigation |
| queue lag grows | consumer under-scaling | Pub/Sub backlog age | custom autoscaling by lag |
| 3rd-party payment slowness | external dependency issue | external call latency | circuit breaker, async retry, fail-open/closed policy |
| cluster autoscaling lag | sudden spike | pending pods | pre-scale, larger buffer pools |

## 9.13 When to Choose This Pattern

Choose this when:
- microservice density is high
- platform engineering maturity exists
- critical paths need fine control
- workload is sustained enough to justify cluster economics

---

# 10. Architecture 2: Cloud Run-Centric Real-Time API and Event Platform

## 10.1 Business Scenario

A fintech or mobile backend platform needs:
- public APIs
- secure partner webhooks
- real-time event intake
- async workflows
- regional elasticity
- low idle spend
- fast deployment cycles

## 10.2 Cloud Run Configuration Matrix

| Service Type | Concurrency | Min Instances | Max Instances | CPU Allocation | Ingress | Auth |
|---|---:|---:|---:|---|---|---|
| public API | 20–80 | 1–3 if latency sensitive | 100–500 | on-request or always-on selectively | external via LB/gateway | JWT/OIDC |
| webhook intake | 10–40 | 1–2 | 200+ | on-request | controlled public | signed webhook validation |
| internal worker | 1–10 | 0 | high | job-specific | internal | IAM |
| notification sender | 10–50 | 0–1 | 100 | on-request | internal | IAM |

## 10.3 End-to-End Flow

1. client enters through API Gateway or Apigee
2. Cloud Armor applies edge security
3. request lands on Cloud Run
4. synchronous writes go to Firestore, Cloud SQL, or AlloyDB
5. async events go to Pub/Sub
6. background processors run in Cloud Run
7. delayed work uses Cloud Tasks
8. orchestrated interactions use Workflows
9. analytics lands in BigQuery

## 10.4 ASCII Architecture Diagram

```text
Clients / Partners / Mobile Apps
               |
               v
      API Gateway / Apigee
               |
          Cloud Armor
               |
               v
           Cloud Run
   +--------------------------+
   | auth-api                 |
   | transaction-api          |
   | webhook-intake           |
   | notification-api         |
   +--------------------------+
      |          |         |
      |          |         +------> Cloud Tasks
      |          |
      |          +---------------> Pub/Sub ---> Cloud Run Workers
      |
      +------> Firestore / Cloud SQL / AlloyDB
                           |
                           +------> Dataflow / BigQuery
```

## 10.5 Network Topology

- Shared VPC service project attachment
- Serverless VPC Access connector for private resources
- private IP databases
- optional external load balancer in front of Cloud Run for unified ingress policy
- NAT-controlled egress for private workloads

## 10.6 Sample Cloud Run Deployment

```bash
gcloud run deploy transaction-api \
  --image us-central1-docker.pkg.dev/acme-shared-cicd/serverless/transaction-api@sha256:1234abcd \
  --region us-central1 \
  --service-account sa-transaction-runtime@acme-prd-api-app.iam.gserviceaccount.com \
  --ingress internal-and-cloud-load-balancing \
  --min-instances 2 \
  --max-instances 300 \
  --concurrency 40 \
  --vpc-connector prod-serverless-connector \
  --set-secrets DB_PASSWORD=db-password:latest \
  --no-allow-unauthenticated
```

## 10.7 Data Decisions

| Requirement | Service |
|---|---|
| mobile JSON-heavy profile state | Firestore |
| moderate relational transactions | Cloud SQL |
| high-performance PostgreSQL OLTP | AlloyDB |
| archives and payload storage | Cloud Storage |
| analytics and compliance reporting | BigQuery |

## 10.8 Multi-Region Strategy

### Active/Active APIs
- deploy stateless APIs in two regions
- route through global edge
- use regional backends
- replicate or partition backing data appropriately

### Active/Passive Data
- many teams keep DB primary in one region with replica/failover strategy
- this reduces complexity while preserving API resiliency

### Recovery Sequence
1. edge stops sending traffic to unhealthy region
2. workers drain/retry from Pub/Sub
3. DB failover if needed
4. replay missed events if required
5. validate webhook idempotency and duplicate suppression

## 10.9 CI/CD Controls

- merge triggers build and tests
- container scan
- artifact sign
- stage deploy
- synthetic API tests
- prod approval
- canary rollout
- rollback if p95, 5xx, or business KPI violates thresholds

## 10.10 Failure Modes

| Symptom | Likely Cause | Detection | Mitigation |
|---|---|---|---|
| cold-start latency | no min instances | p95 spike after idle | keep warm only for critical endpoints |
| DB saturation | too many parallel requests | DB CPU/connections | reduce concurrency, pool carefully |
| webhook duplicates | sender retries | duplicate event count | idempotency keys, dedupe store |
| runaway retries | downstream outage | task retry age, DLQ growth | rate limiting, backoff, circuit breaker |

## 10.11 Cost Drivers

- request volume
- min instances
- VPC connector and egress
- database cost
- BigQuery usage
- premium API management if Apigee is used

## 10.12 When to Choose This Pattern

Choose this when:
- the estate is mostly stateless
- business values speed and low ops burden
- traffic is bursty
- workloads are API- and event-heavy

---

# 11. Architecture 3: Hybrid GKE + Cloud Run Real-Time Platform

## 11.1 Business Scenario

A ride-sharing, gaming, collaboration, or media platform needs:
- low-latency matching/session state
- bursty public APIs
- partner integrations
- heavy event streams
- analytics
- mixed economics across workloads

## 11.2 Workload Placement Standard

### Place on GKE
- matching and dispatch
- websocket coordination
- high-throughput stream consumers
- stateful low-latency services
- sidecar/mesh-reliant services

### Place on Cloud Run
- public APIs
- admin APIs
- webhooks
- notifications
- scheduled jobs
- burst workers
- document/report generation

## 11.3 End-to-End Flow

1. users and partners enter via global load balancer
2. edge APIs terminate on Cloud Run
3. critical core routes privately to GKE
4. events publish to Pub/Sub
5. GKE and Cloud Run both consume according to workload fit
6. Dataflow processes streams
7. BigQuery stores analytical data
8. Workflows and Tasks orchestrate external systems

## 11.4 ASCII Architecture Diagram

```text
Users / Devices / Partners
             |
             v
 Global HTTP(S) Load Balancer + Cloud Armor
             |
      +------+------+
      |             |
      v             v
   Cloud Run     GKE Core
  Edge APIs     Match / Session / Stream
      |             |
      +------+------+
             |
           Pub/Sub
     +--------+---------+
     |                  |
     v                  v
 Cloud Run Workers   GKE Consumers
     |                  |
     v                  v
 Tasks/Workflows    Redis / Bigtable
             \        /
              \      /
               v    v
        AlloyDB / Cloud SQL
               |
               v
            BigQuery
```

## 11.5 Topology

### Regions
- `us-central1` primary
- `us-east1` secondary
- optional active-active edge APIs in both
- selective active-active core only where operationally justified

### Projects
- `acme-prd-edge-app`
- `acme-prd-core-realtime`
- `acme-prd-data`
- `acme-prd-observability`
- `acme-prd-security`
- `acme-prd-net-host`

## 11.6 Data Mapping Example

| Workload | Service | Why |
|---|---|---|
| active trip/session state | Memorystore | sub-millisecond cache/state |
| telemetry/time-series | Bigtable | high throughput |
| transactional trips/payments | AlloyDB | strong relational OLTP |
| flexible user metadata | Firestore | schema flexibility |
| receipts/media | Cloud Storage | durable object storage |
| analytics | BigQuery | analytical scale |
| globally consistent transactions | Spanner | only if truly required |

## 11.7 Network and Trust Boundaries

### Trust Zones
1. internet/public edge
2. edge application zone
3. core realtime zone
4. data zone
5. operations/security zone

### Network Rules
- GKE core not directly public
- Cloud Run public exposure only for approved edge services
- internal service-to-service auth via IAM / identity-aware design
- private database access only
- serverless VPC connector for private reachability
- east-west controls with network policy and service segmentation

## 11.8 CI/CD Model

- shared source standards
- runtime-specific pipelines
- common artifact signing and policy gates
- progressive delivery on Cloud Run
- controlled canary or wave rollout on GKE
- release freeze tied to error budget burn for tier0 systems

## 11.9 Multi-Region Strategy

### Recommended Pattern
- edge APIs: active-active across regions
- core realtime matching: active-passive or partitioned active-active by geography
- analytics: regional ingest with centralized dataset strategy
- databases: replica/failover pattern based on service criticality

### Recovery Sequence
1. route edge traffic away from impaired region
2. validate Pub/Sub subscriptions and worker health
3. fail over transactional store if needed
4. bring GKE core online in secondary
5. warm caches and verify downstream integrations
6. reopen public traffic gradually

## 11.10 Failure Modes

| Symptom | Likely Cause | Detection | Mitigation |
|---|---|---|---|
| dispatch latency spike | cache churn or hot key | p95 dispatch latency, Redis metrics | shard state, tune TTL, pre-warm |
| cross-runtime delay | poorly placed dependency chain | distributed traces | re-place workload, reduce chatty hops |
| queue surge after outage | replay backlog | backlog age | consumer burst scaling, staged replay |
| auth inconsistency | different runtime patterns | 401/403 anomalies | standardize identity model |

## 11.11 Cost Strategy

Use GKE for:
- sustained high-throughput low-latency core

Use Cloud Run for:
- bursty, intermittent, or edge-facing workloads

Review carefully:
- inter-service egress
- cross-region traffic
- over-fragmented APIs
- unnecessary warm capacity on Cloud Run
- oversized GKE node buffers

## 11.12 When to Choose This Pattern

Choose this when:
- workload classes are materially different
- there is a true split between control-heavy core and bursty edge
- platform governance is mature enough to support two runtimes without chaos

---

# 12. Cloud Run and GKE Operations Standards

## 12.1 Patching and Upgrades

### GKE
- subscribe to release channel
- quarterly minor version planning
- monthly node image review
- test upgrades in stage before prod
- maintain maintenance windows and exclusions for peak business periods

### Cloud Run
- revision-based rollouts
- image base refresh policy
- dependency refresh cadence
- deploy latest secure runtime image after validation

## 12.2 Certificate Lifecycle

- central ownership in Certificate Manager
- expiry alerts
- renewal validation in lower environments
- no manual cert handling in application teams

## 12.3 Backup and Restore Verification

- database backup success is not enough
- test restore at least quarterly for tier0/tier1
- validate application compatibility with restored state
- document step-by-step recovery evidence

## 12.4 DR Test Cadence

| Criticality | Test Cadence |
|---|---|
| Tier0 | quarterly tabletop + semiannual execution |
| Tier1 | semiannual |
| Tier2 | annual |

---

# 13. Delivery Checklist

## 13.1 Before Production Go-Live

- landing zone approved
- project separation complete
- Shared VPC attached
- IAM groups and SAs reviewed
- secrets externalized
- artifact signing enabled
- WAF rules validated
- audit logs routed centrally
- SLOs defined
- dashboards and alerts tested
- runbooks completed
- backup and restore tested
- DR scenario tested
- cost labels applied
- load test completed
- rollback procedure rehearsed

---

# 14. Common Anti-Patterns

- using GKE for every service by default
- using Cloud Run for latency-critical ultra-chatty internal workflows without testing
- sharing runtime service accounts across many services
- exposing databases or internal services publicly
- mixing prod and non-prod too closely
- no DLQ or replay design
- no idempotency strategy
- broad IAM roles for speed
- unbounded retries
- no cost ownership labels
- no RTO/RPO classification
- assuming HA equals DR
- treating logs as observability without SLOs and traces

---

# 15. Final Comparison Matrix

| Dimension | GKE-Centric Commerce | Cloud Run-Centric API Platform | Hybrid GKE + Cloud Run |
|---|---|---|---|
| Latency Control | Excellent | Good | Excellent on critical paths |
| Steady-State Cost | Strong at scale | Moderate | Strong if workload placed well |
| Burst Cost Efficiency | Moderate | Excellent | Excellent |
| Operations Burden | High | Low to medium | Medium to high |
| Compliance Readiness | Excellent | Strong | Excellent |
| Runtime Flexibility | Excellent | Moderate | Excellent |
| Portability | High | Medium | High for core |
| Team Skill Requirement | High | Medium | High |
| Best-Fit Workloads | dense microservices and control-heavy core | stateless APIs and event services | mixed runtime enterprises |

---

# 16. Senior Architect Recommendations

1. **Choose runtime by workload behavior, not platform preference.**
2. **Make identity, network segmentation, and observability part of the architecture from day one.**
3. **Design critical paths synchronously and supporting workflows asynchronously.**
4. **Use managed services aggressively, but only when they fit the access pattern.**
5. **Standardize naming, labels, IAM, CI/CD, and runbook formats early.**
6. **Engineer for replay, rollback, restore, and failover, not just for happy-path scale.**
7. **Treat cost as a design dimension equal to security and reliability.**
8. **A 10/10 architecture is not the one with the most services. It is the one with the clearest trade-offs, strongest guardrails, and best operational behavior.**
