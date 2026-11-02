# Azure Production Issues and Troubleshooting Runbook

This runbook covers 10 real production issues in Azure with symptoms, root causes, troubleshooting steps, useful checks, solutions, and recovery guidance.

---

## 1. App Service returns 500 after deployment

### Symptoms
- Users get 500 Internal Server Error
- Deployment succeeded, but app is unhealthy
- App works in staging but not in production

### Common causes
- Wrong app settings
- Bad container image or startup command
- Broken Key Vault reference
- Database connection string issue
- Slot swap moved bad config to production

### Troubleshooting steps
1. Check whether the issue started immediately after deployment.
2. Review application logs and container logs.
3. Compare production settings with the last working release.
4. Validate startup command, port, and health endpoint.
5. Check Key Vault secret resolution and managed identity access.

### Useful checks
```bash
az webapp show -g <rg> -n <app-name> -o table
az webapp log tail -g <rg> -n <app-name>
az webapp config appsettings list -g <rg> -n <app-name> -o table
az webapp deployment slot list -g <rg> -n <app-name> -o table
```

### Solution
- Restore correct app settings
- Redeploy the previous image or package
- Swap back to the previous slot
- Fix Key Vault permissions if secret resolution failed

### Fast recovery
```bash
az webapp deployment slot swap   -g <rg>   -n <app-name>   --slot staging   --target-slot production
```

---

## 2. Azure Front Door or Application Gateway returns 502/503

### Symptoms
- Frontend is reachable, but backend calls fail
- Users see 502 Bad Gateway or 503 Service Unavailable
- Health probes show backend unhealthy

### Common causes
- Backend app is down
- Wrong health probe path
- TLS or hostname mismatch
- NSG or backend pool issue
- WAF rule blocking valid traffic

### Troubleshooting steps
1. Check backend health status.
2. Confirm the backend responds directly.
3. Validate health probe path and expected response code.
4. Review listener, routing, and backend pool configuration.
5. Check WAF logs for false positives.

### Useful checks
```bash
az network application-gateway show -g <rg> -n <agw-name> -o table
az afd endpoint show -g <rg> --profile-name <profile> --endpoint-name <endpoint>
curl -I https://<backend-health-endpoint>
```

### Solution
- Fix backend health endpoint
- Correct backend host header or TLS settings
- Re-add healthy backend targets
- Tune or exclude false-positive WAF rules carefully

---

## 3. AKS pods stuck in Pending

### Symptoms
- New deployment never becomes ready
- Pods remain in Pending
- Traffic may partially fail during release

### Common causes
- Insufficient node capacity
- Wrong resource requests
- Node pool exhausted
- Taints and tolerations mismatch
- Volume or network attachment issues

### Troubleshooting steps
1. Describe the pod.
2. Check scheduler events.
3. Inspect node CPU and memory pressure.
4. Validate node selectors, taints, and tolerations.
5. Check autoscaler activity.

### Useful checks
```bash
kubectl get pods -A
kubectl describe pod <pod> -n <namespace>
kubectl get nodes
kubectl top nodes
kubectl get events -A --sort-by=.lastTimestamp
az aks show -g <rg> -n <aks-name> -o table
```

### Solution
- Scale node pool
- Reduce excessive CPU or memory requests
- Fix scheduling constraints
- Repair storage or network dependency

### Scale example
```bash
az aks nodepool scale   -g <rg>   --cluster-name <aks-name>   -n nodepool1   --node-count 5
```

---

## 4. AKS pods in CrashLoopBackOff

### Symptoms
- Pods start and immediately restart
- Service intermittently fails
- New version does not stabilize

### Common causes
- Bad environment variable
- Secret or configmap missing
- Wrong command or image
- Liveness or readiness probe failure
- Dependency like Redis or database unavailable

### Troubleshooting steps
1. Check previous container logs.
2. Review pod description.
3. Validate mounted secrets and configmaps.
4. Test dependency connectivity.
5. Confirm probes are not too aggressive.

### Useful checks
```bash
kubectl logs <pod> -n <namespace> --previous
kubectl describe pod <pod> -n <namespace>
kubectl get secret -n <namespace>
kubectl get configmap -n <namespace>
```

### Solution
- Fix startup config
- Restore missing secret or config
- Correct probes
- Roll back deployment

### Rollback
```bash
kubectl rollout undo deployment/<deployment> -n <namespace>
kubectl rollout status deployment/<deployment> -n <namespace>
```

---

## 5. Azure Function is timing out

### Symptoms
- API requests fail or hang
- Queue backlog grows
- Application Insights shows duration spike

### Common causes
- Slow downstream dependency
- VNet or NAT issue
- Cold start
- Function memory too low
- External API latency

### Troubleshooting steps
1. Check execution duration and failures in Application Insights.
2. Identify whether the function depends on SQL, Cosmos DB, Service Bus, or external API.
3. Review retry behavior and queue backlog.
4. Validate VNet integration and outbound connectivity.

### Useful checks
```bash
az functionapp show -g <rg> -n <function-name> -o table
az monitor metrics list --resource <function-resource-id> --metric "FunctionExecutionCount"
az monitor metrics list --resource <function-resource-id> --metric "FunctionExecutionUnits"
```

### Solution
- Increase plan capacity or move to Premium if needed
- Fix downstream latency
- Add queue-based decoupling
- Increase timeout only after confirming workload design

---

## 6. Cosmos DB throttling with 429 errors

### Symptoms
- Requests fail intermittently
- Application latency spikes
- App logs show 429 Too Many Requests

### Common causes
- RU/s too low
- Hot partition
- Bad partition key design
- Sudden traffic burst
- Inefficient queries

### Troubleshooting steps
1. Check request rate and RU consumption.
2. Identify hot partition behavior.
3. Review query patterns and indexing.
4. Confirm autoscale and max throughput settings.

### Useful checks
```bash
az cosmosdb show -g <rg> -n <account-name>
az cosmosdb sql container throughput show   -a <account-name> -g <rg> -d <db> -n <container>
```

### Solution
- Increase RU/s or autoscale limit
- Redesign partition key if skew is severe
- Optimize queries and indexing
- Add retry with exponential backoff in app code

---

## 7. PostgreSQL or Azure SQL connectivity failure

### Symptoms
- App throws connection timeout or authentication error
- Only some app instances fail
- Database-related 5xx errors spike

### Common causes
- Wrong credentials
- Firewall or private endpoint DNS issue
- Database max connections reached
- NSG or routing problem
- Recent failover or maintenance event

### Troubleshooting steps
1. Check database server health.
2. Validate firewall or private endpoint path.
3. Confirm DNS resolution from app subnet.
4. Review connection pool exhaustion.
5. Check recent maintenance or failover history.

### Useful checks
```bash
az postgres flexible-server show -g <rg> -n <server-name> -o table
nslookup <db-hostname>
nc -zv <db-hostname> 5432
```

### Solution
- Fix credentials
- Repair private DNS or firewall rules
- Add connection pooling
- Scale database or app pool settings
- Restore from point-in-time restore if corruption or destructive change occurred

---

## 8. Key Vault secrets are not accessible

### Symptoms
- App starts failing after secret rotation or deployment
- Key Vault references show unresolved values
- Managed identity authentication fails

### Common causes
- Missing RBAC role assignment
- Wrong managed identity
- Private endpoint or DNS issue
- Secret version disabled or deleted
- Key Vault firewall restriction

### Troubleshooting steps
1. Confirm which identity the workload uses.
2. Check role assignment or access policy.
3. Validate Key Vault networking.
4. Confirm the secret exists and is enabled.
5. Test from the app environment if possible.

### Useful checks
```bash
az keyvault show -g <rg> -n <kv-name> -o table
az role assignment list --scope <keyvault-resource-id> -o table
az keyvault secret show --vault-name <kv-name> --name <secret-name>
```

### Solution
- Grant correct secret read permissions
- Fix managed identity assignment
- Repair private endpoint or DNS
- Restore or re-enable the secret version

---

## 9. Service Bus backlog or dead-letter queue growth

### Symptoms
- Messages are piling up
- Downstream processing is delayed
- Dead-letter queue grows rapidly

### Common causes
- Consumer app is down
- Message lock timeout too short
- Poison messages
- Concurrency too low
- Dependency failure in consumers

### Troubleshooting steps
1. Check queue depth and dead-letter count.
2. Confirm consumers are running.
3. Inspect failed message patterns.
4. Review retries and lock duration.
5. Check downstream database or API dependencies.

### Useful checks
```bash
az servicebus queue show   --resource-group <rg>   --namespace-name <namespace>   --name <queue>
```

### Solution
- Restart or scale consumers
- Move poison messages aside for analysis
- Increase concurrency carefully
- Fix dependency bottleneck
- Replay dead-letter messages only after root cause is fixed

---

## 10. DNS or private endpoint resolution issue

### Symptoms
- App cannot reach Storage, Key Vault, SQL, or Cosmos DB
- Connectivity works publicly but not privately
- After network changes, services fail unexpectedly

### Common causes
- Private DNS zone not linked
- Wrong VNet link
- Split-brain DNS
- Incorrect hostnames in config
- Route or firewall changes

### Troubleshooting steps
1. Check private endpoint status.
2. Verify private DNS zone records.
3. Test name resolution from app host or pod.
4. Confirm VNet links and peering.
5. Review recent network policy or route changes.

### Useful checks
```bash
az network private-endpoint list -g <rg> -o table
az network private-dns zone list -g <rg> -o table
nslookup <service-private-fqdn>
```

### Solution
- Link the correct VNet to the private DNS zone
- Recreate missing DNS records if needed
- Fix hostname configuration
- Validate route and firewall path end to end

---

## Standard Azure Production Troubleshooting Workflow

1. Confirm blast radius
2. Check recent changes
3. Check ingress:
   - Azure DNS
   - Front Door
   - Application Gateway
   - API Management
4. Check compute:
   - App Service
   - AKS
   - Functions
   - VM or VMSS if applicable
5. Check dependencies:
   - Key Vault
   - SQL or PostgreSQL
   - Cosmos DB
   - Service Bus
   - Storage
   - Redis
   - external APIs
6. Mitigate first:
   - rollback
   - swap slot back
   - scale out
   - shift traffic
   - disable broken consumer
   - route to DR region
7. Verify recovery:
   - health endpoint
   - error rate
   - queue drain rate
   - database latency
   - business transaction success

---

## High-Value Azure Commands

```bash
az account show -o table
az monitor activity-log list --offset 2h
az webapp log tail -g <rg> -n <app-name>
az aks show -g <rg> -n <aks-name> -o table
kubectl get pods -A
az functionapp show -g <rg> -n <function-name> -o table
az postgres flexible-server show -g <rg> -n <server-name> -o table
az cosmosdb show -g <rg> -n <account-name>
az servicebus queue show --resource-group <rg> --namespace-name <ns> --name <queue>
az keyvault show -g <rg> -n <kv-name>
```
