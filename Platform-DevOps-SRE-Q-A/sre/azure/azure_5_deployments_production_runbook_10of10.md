# Azure Cloud Architect Runbook
## 5 Enterprise-Grade Azure Deployment Patterns with Production Response, Monitoring, Failover, Scaling, Security, and Recovery

This document is an implementation-grade Azure runbook written from the perspective of an Azure Cloud Architect and Engineer. It is designed for real production use, not just interview preparation. It combines:

- deployment guidance
- monitoring baselines
- incident response steps
- failover patterns
- scaling procedures
- security controls
- backup and recovery procedures
- CI/CD rollout and rollback guidance
- production readiness checks

---

# 1. Platform Foundation

## 1.1 Management Group and Subscription Strategy

Use management groups and subscriptions to separate control, billing, policy, and blast radius.

```text
Tenant Root
|
+-- Platform
|   +-- mg-connectivity
|   +-- mg-security
|   +-- mg-shared-services
|
+-- NonProd
|   +-- sub-dev
|   +-- sub-test
|   +-- sub-stage
|
+-- Prod
    +-- sub-prod-primary
    +-- sub-prod-dr
```

## 1.2 Recommended Resource Group Model

```text
rg-<org>-<env>-network
rg-<org>-<env>-security
rg-<org>-<env>-identity
rg-<org>-<env>-shared
rg-<org>-<env>-app
rg-<org>-<env>-data
rg-<org>-<env>-monitoring
rg-<org>-<env>-backup
```

## 1.3 Naming Convention

```text
<resource>-<org>-<env>-<region>-<nn>
```

Examples:

```text
vnet-acme-prod-eus-01
agw-acme-prod-eus-01
aks-acme-prod-eus-01
kv-acme-prod-eus-01
law-acme-prod-eus-01
```

For storage accounts, use Azure naming constraints:

```text
stacmeprodeus01
```

## 1.4 Mandatory Tags

Apply these tags to all supported resources:

- `Environment`
- `Application`
- `Owner`
- `CostCenter`
- `BusinessService`
- `Criticality`
- `DataClassification`
- `ManagedBy`
- `BackupPolicy`
- `DRTier`

## 1.5 Identity and Access Model

Use:
- Microsoft Entra ID groups for role assignment
- Privileged Identity Management for elevation
- managed identities for workloads
- workload identity for AKS where applicable
- Key Vault for secrets and certificates

Separate responsibilities:
- platform admin
- network admin
- security admin
- CI/CD deployer
- application operator
- incident responder

Avoid:
- persistent Owner assignments
- embedded secrets in pipelines
- shared human admin accounts

---

# 2. Regional Design, RTO/RPO, and DR Topology

## 2.1 Primary and Secondary Region Pattern

Example:

- Primary: `East US`
- Secondary: `Central US`

Choose actual paired regions based on compliance, latency, and service support.

## 2.2 DR Tiers

### Tier 1: Mission-critical
- target RTO: 15–30 minutes
- target RPO: near-zero to 15 minutes
- active-active or hot standby
- regional traffic failover tested quarterly

### Tier 2: Business-critical
- target RTO: 1–4 hours
- target RPO: 1 hour
- warm standby
- restore automation partially scripted

### Tier 3: Standard
- target RTO: 24 hours
- target RPO: 24 hours
- backup and restore
- manual cutover acceptable

## 2.3 Failover Decision Criteria

Fail over only when one or more conditions are true:
- regional platform issue confirmed
- app unavailable beyond error budget
- database primary cannot recover inside RTO
- ingress failure persists after local remediation
- security incident requires isolation of primary environment

## 2.4 Failback Criteria

Fail back only after:
- root cause understood
- replication healthy
- smoke tests passed in primary
- DNS/Front Door/App Gateway routing plan approved
- business stakeholders informed

---

# 3. Networking Baseline

## 3.1 Hub-Spoke Reference

```text
Hub VNet
|
+-- Azure Firewall
+-- Bastion
+-- Shared DNS / Private DNS resolvers
+-- VPN / ExpressRoute
|
+-- Spoke-App
|   +-- ingress subnet
|   +-- app subnet
|   +-- aks subnet
|
+-- Spoke-Data
    +-- private endpoint subnet
    +-- database subnet
```

## 3.2 Subnet Intent

- `AzureFirewallSubnet`
- `AzureBastionSubnet`
- `snet-ingress`
- `snet-app`
- `snet-aks`
- `snet-data`
- `snet-private-endpoints`

## 3.3 Network Rules

- expose only ingress tier publicly
- data services private by default
- private endpoints for Key Vault, Storage, SQL, PostgreSQL, Cosmos DB where sensitivity requires
- use NSGs and route tables with explicit ownership
- use Private DNS Zones and validate link propagation

## 3.4 NSG Baseline Example

### App subnet
Allow:
- ingress from Application Gateway subnet on app ports
- outbound to data subnet/private endpoints on required ports
- outbound to monitoring endpoints if needed

Deny:
- direct internet inbound
- unrestricted east-west traffic

### Data subnet
Allow:
- inbound only from app or approved admin paths
- backup and monitoring traffic as required

Deny:
- any public ingress

---

# 4. Observability and Alerting Baseline

## 4.1 Core Services

Use:
- Azure Monitor
- Log Analytics
- Application Insights
- Activity Log
- Resource Health
- Service Health
- Network Watcher
- Defender for Cloud

## 4.2 Standard Alert Thresholds

Use these as starting points, then tune per workload.

### Front Door / App Gateway
- backend health below 90% for 5 minutes
- 5xx error rate above 2% for 5 minutes
- latency p95 above baseline by 2x for 10 minutes

### App Service
- HTTP 5xx count above 20 in 5 minutes
- CPU above 80% for 10 minutes
- memory above 85% for 10 minutes
- restart count abnormal for 15 minutes

### AKS
- node not ready for 5 minutes
- pod restart count spike above normal threshold
- pending pods > 5 for 10 minutes
- ingress 5xx above 2% for 5 minutes
- cluster autoscaler unable to scale

### Functions
- failure count above normal baseline for 5 minutes
- duration p95 above 2x baseline
- throttles > 0 for critical functions
- queue trigger backlog beyond SLO

### Databases
- PostgreSQL CPU above 80% for 15 minutes
- connections above 85% of max
- storage above 80%
- replica lag beyond RPO target
- Cosmos DB 429s above threshold

### Data Platform
- pipeline failures > 0 for critical jobs
- late-arriving data beyond agreed freshness window
- Synapse failed queries spike
- integration runtime unavailable

## 4.3 Action Group Design

Configure action groups for:
- NOC/on-call paging
- Teams/email notification
- ITSM ticket creation
- automation webhook for rollback or scaling
- security escalation

## 4.4 Sample KQL Queries

### App failures
```kusto
requests
| where timestamp > ago(30m)
| where success == false
| summarize failures=count(), p95=percentile(duration, 95) by cloud_RoleName, resultCode
| order by failures desc
```

### Dependency latency
```kusto
dependencies
| where timestamp > ago(30m)
| summarize p95=percentile(duration,95), failures=countif(success == false) by target, type
| order by p95 desc
```

### AKS pod restarts
```kusto
KubePodInventory
| where TimeGenerated > ago(1h)
| summarize restarts=sum(ContainerRestartCount) by Namespace, Name
| order by restarts desc
```

### Azure Activity changes
```kusto
AzureActivity
| where TimeGenerated > ago(2h)
| where ActivityStatusValue in ("Success", "Succeeded")
| project TimeGenerated, OperationNameValue, ResourceGroup, Caller
| order by TimeGenerated desc
```

---

# 5. CI/CD Governance and Release Safety

## 5.1 Release Rules

All production deployments should include:
- immutable artifacts
- environment approval gate
- infrastructure drift check
- smoke tests
- rollback command or script
- release note with change scope
- deployment owner and incident owner assignment

## 5.2 Release Strategies

Use:
- slot swap for App Service
- canary or rolling update for AKS
- APIM revisions for APIs
- staged region rollout for Front Door-based apps
- blue/green where downtime or regression risk is unacceptable

## 5.3 Sample GitHub Actions Workflow

```yaml
name: azure-prod-deploy

on:
  workflow_dispatch:
  push:
    branches: [ main ]

jobs:
  validate-build-deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Azure login
        uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}

      - name: Lint IaC
        run: echo "validate bicep or terraform"

      - name: Build artifact
        run: echo "build app image/package"

      - name: Deploy infrastructure
        run: echo "deploy infra changes"

      - name: Deploy application
        run: echo "deploy app"

      - name: Smoke test
        run: echo "health checks and synthetic tests"

      - name: Rollback
        if: failure()
        run: echo "run rollback script"
```

## 5.4 Example Azure DevOps Stage Flow

```text
Build -> Security Scan -> IaC Validate -> Deploy Stage -> Verify -> Approval -> Deploy Prod -> Smoke Test -> Rollback if Verify Fails
```

---

# 6. Deployment 1: Static Web Application on Azure Storage + Front Door + Azure DNS + Key Vault

## 6.1 Architecture Summary

```text
User
 |
 v
Azure DNS
 |
 v
Azure Front Door + WAF
 |
 v
Storage Account Static Website
 |
 +--> optional secondary origin in DR region
 |
 +--> Key Vault for certificate governance
```

## 6.2 Production Use Cases
- SPA frontend
- product site
- docs portal
- globally distributed read-heavy site

## 6.3 Prerequisites
- DNS zone created
- certificate strategy approved
- WAF policy baseline defined
- origin health probes configured
- build artifacts versioned
- storage account versioning and soft delete considered

## 6.4 Deployment Steps

```bash
az group create -n rg-acme-prod-app -l eastus
az storage account create -n stacmeprodeus01 -g rg-acme-prod-app -l eastus --sku Standard_LRS --kind StorageV2
az storage blob service-properties update --account-name stacmeprodeus01 --static-website --index-document index.html --404-document 404.html
az storage blob upload-batch --account-name stacmeprodeus01 -d '$web' -s ./dist
az afd profile create --profile-name afd-acme-prod --resource-group rg-acme-prod-app --sku Premium_AzureFrontDoor
az afd endpoint create --resource-group rg-acme-prod-app --profile-name afd-acme-prod --endpoint-name afd-acme-prod-eus
```

## 6.5 Monitoring
Watch:
- endpoint availability
- backend health percentage
- WAF blocks
- 4xx/5xx
- TLS validity
- cache hit ratio

## 6.6 Production Issue Response
Common issues:
- stale content
- TLS/domain failure
- origin unhealthy
- WAF false positive block
- 404 due to missing SPA routing rules

Troubleshooting:
1. Check Front Door endpoint and origin health.
2. Confirm files exist in `$web`.
3. Check DNS resolution.
4. Check WAF logs for false positives.
5. Validate custom domain and certificate binding.
6. Compare current release artifact to previous release.

Validation:
```bash
az afd endpoint show -g rg-acme-prod-app --profile-name afd-acme-prod --endpoint-name afd-acme-prod-eus
az storage blob list --account-name stacmeprodeus01 -c '$web' -o table
nslookup www.example.com
curl -I https://www.example.com
```

## 6.7 Failover
Preferred:
- Front Door origin group with priority/latency routing
- secondary static site in paired region

Failover sequence:
1. confirm primary origin unavailable
2. enable or prioritize secondary origin
3. validate content and health
4. monitor error rate and latency

## 6.8 Scaling
- Front Door auto-scales
- Storage static website scales well for read-heavy workloads
- use cache-control and hashed asset names

## 6.9 Security
- WAF policy with managed rules
- RBAC on storage
- HTTPS only
- Key Vault certificate governance
- Defender alerts enabled
- storage public access reviewed against architecture intent

## 6.10 Recovery
- upload previous stable artifact
- purge Front Door content
- restore deleted blobs if versioning enabled

```bash
az storage blob upload-batch --account-name stacmeprodeus01 -d '$web' -s ./rollback-build
az afd endpoint purge --resource-group rg-acme-prod-app --profile-name afd-acme-prod --endpoint-name afd-acme-prod-eus --content-paths '/*'
```

## 6.11 Failback
- restore primary origin priority
- confirm asset parity
- re-enable standard WAF actions if temporarily relaxed

---

# 7. Deployment 2: Containerized Web App on App Service + ACR + Azure Database for PostgreSQL

## 7.1 Architecture Summary

```text
User
 |
 v
Front Door or App Gateway
 |
 v
App Service for Containers
 |
 +--> ACR
 +--> Key Vault via managed identity
 +--> PostgreSQL Flexible Server via private access
```

## 7.2 Prerequisites
- ACR ready
- PostgreSQL Flexible Server deployed
- private DNS set if private DB access is used
- staging slot created
- managed identity enabled
- Key Vault references configured

## 7.3 Deployment Steps

```bash
az acr create -n acracmeprod01 -g rg-acme-prod-app --sku Premium
az acr build -r acracmeprod01 -t webapp:v1 .
az appservice plan create -g rg-acme-prod-app -n asp-acme-prod --is-linux --sku P1v3
az webapp create -g rg-acme-prod-app -p asp-acme-prod -n app-acme-prod-web -i acracmeprod01.azurecr.io/webapp:v1
az webapp identity assign -g rg-acme-prod-app -n app-acme-prod-web
az postgres flexible-server create -g rg-acme-prod-data -n pg-acme-prod-01 -l eastus --sku-name Standard_D4s_v3 --tier GeneralPurpose
az webapp deployment slot create -g rg-acme-prod-app -n app-acme-prod-web --slot staging
```

## 7.4 Monitoring
Watch:
- HTTP 5xx
- p95 latency
- CPU/memory
- restart count
- deployment slot health
- DB connections
- slow queries

## 7.5 Production Issue Response
Common issues:
- image pull/auth failure
- container crash
- slot swap introduces regression
- DB connectivity failure
- Key Vault secret resolution failure

Troubleshooting:
1. tail app logs
2. validate app settings and identity
3. check staging slot before swap
4. confirm database reachability and firewall/private DNS
5. inspect recent platform changes in Activity Log

Validation:
```bash
az webapp log tail -g rg-acme-prod-app -n app-acme-prod-web
az webapp config appsettings list -g rg-acme-prod-app -n app-acme-prod-web -o table
az webapp deployment slot list -g rg-acme-prod-app -n app-acme-prod-web -o table
az postgres flexible-server show -g rg-acme-prod-data -n pg-acme-prod-01 -o table
```

## 7.6 Failover
- Front Door to secondary region App Service
- PostgreSQL read replica or cross-region DR plan
- deployment slots for local rollback without regional failover

## 7.7 Scaling

```bash
az appservice plan update -g rg-acme-prod-app -n asp-acme-prod --number-of-workers 3
az appservice plan update -g rg-acme-prod-app -n asp-acme-prod --sku P2v3
```

Use autoscale where supported in surrounding design, or combine with Front Door and multiple instances.

## 7.8 Security
- managed identity to Key Vault
- disable unnecessary FTP/basic auth if applicable
- private database access
- Defender for Cloud review
- TLS enforcement
- SCM access restrictions

## 7.9 Recovery
- swap back to previous slot
- redeploy previous image tag
- restore DB using PITR if bad migration happened

```bash
az webapp deployment slot swap -g rg-acme-prod-app -n app-acme-prod-web --slot staging --target-slot production
az webapp config container set -g rg-acme-prod-app -n app-acme-prod-web --container-image-name acracmeprod01.azurecr.io/webapp:v0
```

## 7.10 PITR Guidance
For destructive migration:
1. identify last known-good timestamp
2. restore PostgreSQL to new server from PITR
3. validate schema/data
4. redirect app config to restored server
5. reconcile missing writes if needed

---

# 8. Deployment 3: AKS Microservices Platform with Application Gateway Ingress

## 8.1 Architecture Summary

```text
User
 |
 v
Azure Front Door
 |
 v
Application Gateway + WAF
 |
 v
AKS
 |--- services
 |--- ingress controller
 |--- HPA
 |
 +--> ACR
 +--> Redis
 +--> PostgreSQL or Azure SQL
 +--> Key Vault CSI / workload identity
```

## 8.2 Prerequisites
- VNet/subnets prepared
- ACR integrated
- cluster autoscaler design approved
- workload identity enabled where applicable
- Key Vault CSI or secret sync approach defined
- Redis/DB connectivity planned
- DR cluster strategy documented

## 8.3 Deployment Steps

```bash
az aks create -g rg-acme-prod-app -n aks-acme-prod-eus-01 -l eastus --node-count 3 --enable-managed-identity --network-plugin azure --attach-acr acracmeprod01
az aks get-credentials -g rg-acme-prod-app -n aks-acme-prod-eus-01 --overwrite-existing
kubectl apply -f k8s/namespaces.yaml
kubectl apply -f k8s/config/
kubectl apply -f k8s/apps/
kubectl apply -f k8s/ingress/
```

## 8.4 Monitoring
Watch:
- node readiness
- pod restart count
- pending pods
- ingress 5xx
- HPA saturation
- memory pressure
- Redis hit rate and latency
- DB connection saturation

## 8.5 Production Issue Response
Common issues:
- pods pending
- CrashLoopBackOff
- ingress 502
- Key Vault mount failure
- DNS failure to private endpoints
- autoscaler cannot provision capacity

Troubleshooting:
1. check events and pod description
2. check node usage and autoscaler
3. inspect ingress, service, endpoints
4. validate workload identity and Key Vault CSI
5. test network path to Redis/DB/private endpoints
6. inspect image pull and ACR auth

Validation:
```bash
kubectl get pods -A
kubectl describe pod <pod> -n <namespace>
kubectl logs <pod> -n <namespace> --previous
kubectl get ingress -A
kubectl get svc -A
kubectl get events -A --sort-by=.lastTimestamp
az aks show -g rg-acme-prod-app -n aks-acme-prod-eus-01 -o table
```

## 8.6 Failover
Tier 1 approach:
- multi-zone AKS in primary
- warm or hot DR cluster in paired region
- Front Door regional routing
- replicated data layer per service RPO/RTO

Failover steps:
1. confirm primary degradation exceeds RTO
2. ensure DR cluster healthy
3. confirm secrets, config, and image access in DR
4. shift traffic using Front Door
5. validate user journey and dependency health

## 8.7 Scaling

```bash
az aks nodepool update -g rg-acme-prod-app --cluster-name aks-acme-prod-eus-01 -n nodepool1 --enable-cluster-autoscaler --min-count 3 --max-count 10
kubectl autoscale deployment microservice-a -n prod --cpu-percent=70 --min=3 --max=10
```

## 8.8 Security
- WAF on ingress path
- workload identity
- network policies
- image scanning
- private cluster for sensitive workloads
- restricted egress
- RBAC mapped to Entra groups

## 8.9 Recovery
- rollout undo
- recreate broken deployment
- cordon/drain faulty nodes
- restore dependent DB/cache
- fail back after primary stabilization

```bash
kubectl rollout undo deployment/microservice-a -n prod
kubectl rollout status deployment/microservice-a -n prod
kubectl cordon <node>
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data
```

## 8.10 AKS-Specific Deep Checks
- `kubectl get endpoints`
- CoreDNS health
- CSI driver logs
- ingress controller logs
- node pool VMSS scaling events

---

# 9. Deployment 4: Serverless API on Azure Functions + API Management + Service Bus + Cosmos DB

## 9.1 Architecture Summary

```text
Client
 |
 v
API Management
 |
 v
Azure Functions
 |
 +--> Cosmos DB
 +--> Service Bus
 +--> Key Vault
 +--> Application Insights
```

## 9.2 Prerequisites
- APIM tier chosen
- Function plan chosen
- Service Bus queue/topic design done
- Cosmos partition key selected
- DLQ and replay policy defined
- managed identity and secret access configured

## 9.3 Deployment Steps

```bash
az storage account create -n stacmefuncprod01 -g rg-acme-prod-app -l eastus --sku Standard_LRS
az functionapp create -g rg-acme-prod-app -n func-acme-prod-api --storage-account stacmefuncprod01 --consumption-plan-location eastus --runtime python --functions-version 4
az servicebus namespace create -g rg-acme-prod-app -n sb-acme-prod-01 -l eastus --sku Premium
az cosmosdb create -g rg-acme-prod-data -n cosmos-acme-prod-01
```

## 9.4 Monitoring
Watch:
- APIM gateway 4xx/5xx
- function failures
- duration p95
- Service Bus active message count
- DLQ depth
- Cosmos RU usage
- Cosmos 429 throttles

## 9.5 Production Issue Response
Common issues:
- function timeout
- APIM backend failure
- queue backlog
- DLQ growth
- Cosmos RU exhaustion
- secret resolution failure

Troubleshooting:
1. inspect Application Insights traces and dependencies
2. check function host logs
3. check APIM revision and backend settings
4. inspect queue depth and consumer lag
5. inspect Cosmos throttling and partition hot spots
6. verify Key Vault access and private networking

Validation:
```bash
az functionapp show -g rg-acme-prod-app -n func-acme-prod-api -o table
az servicebus queue show --resource-group rg-acme-prod-app --namespace-name sb-acme-prod-01 --name orders
az cosmosdb show -g rg-acme-prod-data -n cosmos-acme-prod-01
```

## 9.6 Failover
- APIM multi-region for Tier 1 APIs
- secondary Function deployment in DR region
- Cosmos multi-region replication if RPO requires
- queue buffering during partial backend outage

## 9.7 Scaling
- Premium Functions for predictable scale and VNet support
- Service Bus Premium for isolation and throughput
- Cosmos autoscale with hot-partition review

```bash
az cosmosdb sql container throughput update -a cosmos-acme-prod-01 -g rg-acme-prod-data -d appdb -n orders --max-throughput 4000
```

## 9.8 Security
- APIM auth policies
- OAuth/JWT validation
- managed identity to Key Vault
- private endpoints where needed
- RBAC and diagnostic logging

## 9.9 Recovery
- route APIM to previous revision/backend
- redeploy previous Function artifact
- replay messages from DLQ after fix
- increase throughput temporarily to clear backlog

### Replay Checklist
1. fix root cause
2. validate idempotency
3. replay small sample
4. watch duplicate processing indicators
5. release controlled replay batch

---

# 10. Deployment 5: Data Platform on Data Factory + Storage + Synapse + Key Vault + Monitor

## 10.1 Architecture Summary

```text
Sources
 |
 v
Landing Storage
 |
 v
Azure Data Factory
 |
 v
Raw / Curated Storage
 |
 v
Synapse
 |
 v
Consumers
```

## 10.2 Prerequisites
- storage zones defined
- integration runtime model decided
- Synapse workspace ready
- managed identities and Key Vault integrated
- data retention and lineage requirements approved
- DR approach for metadata and pipeline artifacts documented

## 10.3 Deployment Steps

```bash
az storage account create -n stacmedataprod01 -g rg-acme-prod-data -l eastus --sku Standard_GRS
az datafactory factory create -g rg-acme-prod-data -n adf-acme-prod-01 -l eastus
az synapse workspace create --name syn-acme-prod-01 --resource-group rg-acme-prod-data --storage-account stacmedataprod01 --file-system synapse
```

## 10.4 Monitoring
Watch:
- pipeline failures
- data freshness
- trigger failures
- integration runtime health
- storage growth
- Synapse failed jobs
- secret access failures

## 10.5 Production Issue Response
Common issues:
- schema drift
- source auth failure
- sink write failure
- delayed data
- Synapse timeout
- private endpoint or DNS breakage

Troubleshooting:
1. inspect failed ADF activity
2. inspect linked service auth and Key Vault references
3. validate storage path and schema
4. inspect Synapse workload pressure
5. validate network and private endpoint DNS
6. compare arrival timestamps to freshness SLO

Validation:
```bash
az datafactory pipeline list -g rg-acme-prod-data --factory-name adf-acme-prod-01 -o table
az synapse workspace show -g rg-acme-prod-data -n syn-acme-prod-01 -o table
az storage container list --account-name stacmedataprod01 -o table
```

## 10.6 Failover
- GRS or GZRS storage where required
- replicated ADF definitions via IaC
- standby Synapse artifacts in DR region for critical use cases
- manual or semi-automated cutover plan

## 10.7 Scaling
- increase parallel copy carefully
- scale Synapse pools
- partition data
- stagger heavy pipelines

## 10.8 Security
- Key Vault-backed credentials
- private endpoints
- RBAC
- Purview/lineage integration if used
- encryption and classification controls

## 10.9 Recovery
- stop broken triggers
- fix auth or schema
- rerun only failed slices
- restore storage objects from soft delete/versioning if enabled
- rehydrate downstream tables if partial loads caused inconsistency

```bash
az datafactory trigger stop -g rg-acme-prod-data --factory-name adf-acme-prod-01 -n daily-ingest-trigger
```

## 10.10 Recovery Precision
For bad data loads:
1. identify bad ingestion window
2. quarantine affected files
3. restore clean input if needed
4. truncate or isolate corrupted curated partitions
5. rerun idempotent pipelines
6. validate row counts and business checks

---

# 11. Security Baseline Examples

## 11.1 Azure Policy Ideas
Apply policies for:
- allowed regions only
- required tags
- diagnostic settings required
- storage secure transfer required
- public network access disabled for sensitive PaaS
- approved SKUs only
- Key Vault soft delete and purge protection required
- container images from approved registries only

## 11.2 Defender for Cloud Focus Areas
Review:
- exposed management ports
- vulnerable container images
- missing endpoint protection
- weak network segmentation
- missing backup coverage
- SQL/PostgreSQL hardening recommendations

## 11.3 Key Vault Standards
- enable purge protection
- enable soft delete
- use RBAC-based access where governance prefers
- rotate secrets and certificates
- monitor secret access failures

---

# 12. Common Azure Troubleshooting Flow

## 12.1 Step-by-Step
1. confirm blast radius
2. check recent deployment, policy, DNS, or secret changes
3. inspect ingress path: DNS, Front Door, App Gateway, APIM
4. inspect compute path: App Service, AKS, Functions
5. inspect dependencies: DB, cache, storage, queues
6. mitigate quickly: rollback, scale, reroute, disable broken consumer
7. verify with health checks, logs, business flow
8. document RCA and preventive action

## 12.2 High-Value Azure Commands

```bash
az account show -o table
az group list -o table
az resource list -g <resource-group> -o table
az monitor activity-log list --offset 2h
az monitor metrics list --resource <resource-id> --metric "Requests"
az network private-endpoint list -g <resource-group> -o table
az network dns zone list -g <resource-group> -o table
az webapp log tail -g <resource-group> -n <app-name>
az aks show -g <resource-group> -n <cluster-name> -o table
kubectl get pods -A
az functionapp show -g <resource-group> -n <function-name> -o table
az servicebus queue show --resource-group <resource-group> --namespace-name <namespace> --name <queue>
az cosmosdb show -g <resource-group> -n <account-name>
az datafactory pipeline list -g <resource-group> --factory-name <factory-name> -o table
```

---

# 13. Production Readiness Checklist

- management groups and subscriptions defined
- RBAC mapped to Entra groups
- PIM model agreed
- managed identities implemented
- Key Vault integrated
- network zones and private endpoints documented
- Log Analytics and Application Insights configured
- alert thresholds tuned
- Action Groups configured
- backup and PITR tested
- DR tier assigned with RTO/RPO
- failover and failback procedures documented
- smoke tests and synthetic checks live
- CI/CD approvals enabled
- rollback validated
- security policies assigned
- cost guardrails reviewed
- service ownership and on-call documented

---

# 14. Anti-Patterns to Avoid

- single subscription for all environments
- public access left on for data services by default
- secrets stored directly in application settings
- no diagnostic settings
- no Action Groups
- mutable container tags in production
- no slot strategy for critical App Service workloads
- no canary or rollback path for APIs
- no DR classification
- no failback plan
- no testing of backup restore
- no IaC source of truth

---

# 15. Final Architect Notes

1. **A deployment is not production-ready until monitoring, rollback, and recovery are defined.**
2. **Identity and network design are as important as compute design.**
3. **Regional failover without rehearsed failback is incomplete.**
4. **The correct scaling model depends on workload shape, not service popularity.**
5. **Runbooks should reduce human decision time during incidents, not add to it.**
