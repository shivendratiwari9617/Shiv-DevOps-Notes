# Azure Top 100+ Production Troubleshooting Commands
**10/10 senior-level runbook pack for Azure Platform Engineers, DevOps Engineers, and SREs in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic Azure CLI cheat sheet.

Each command includes:

- the command itself
- a concise technical explanation
- a **risk level**
- a **use when** line so responders know where it fits in incident flow

## Command Risk Levels

- **Low risk** — read-only or generally safe diagnostic action
- **Medium risk** — controlled local-context change, scoped access action, or limited-impact operational step
- **High risk** — can mutate guest access, VM state, storage state, credentials, or production configuration
- **Emergency-only** — use only in tightly controlled recovery situations with explicit confirmation

## Fast “First 5 Commands” by Incident Type

### Wrong tenant, subscription, or CLI context suspicion
1. `az account show`
2. `az account list -o table`
3. `az configure --list-defaults`
4. `az cloud show`
5. `az ad signed-in-user show`

### VM unreachable, SSH/RDP failure, or bad guest state
1. `az vm show --resource-group <rg> --name <vm> -d`
2. `az vm get-instance-view --resource-group <rg> --name <vm>`
3. `az vm boot-diagnostics get-boot-log --resource-group <rg> --name <vm>`
4. `az network nic show --resource-group <rg> --name <nic>`
5. `az network nic list-effective-nsg --resource-group <rg> --name <nic>`

### NSG, route, or connectivity issue
1. `az network nic show --resource-group <rg> --name <nic>`
2. `az network nic list-effective-nsg --resource-group <rg> --name <nic>`
3. `az network nic show-effective-route-table --resource-group <rg> --name <nic>`
4. `az network watcher test-connectivity --source-resource <source-id> --dest-address <host> --dest-port <port>`
5. `az network route-table route list --resource-group <rg> --route-table-name <rt>`

### Application Gateway 502/503 or backend unhealthy
1. `az network application-gateway show --resource-group <rg> --name <agw>`
2. `az network application-gateway show-backend-health --resource-group <rg> --name <agw>`
3. `az network lb show --resource-group <rg> --name <lb>`
4. `az network lb probe list --resource-group <rg> --lb-name <lb>`
5. `az monitor activity-log list --resource-id <resource-id> --offset 1d`

### Private endpoint or private DNS failure
1. `az network private-endpoint list -o table`
2. `az network private-endpoint show --resource-group <rg> --name <pe>`
3. `az network private-dns zone list -o table`
4. `az network private-dns record-set a list --resource-group <rg> --zone-name <zone> -o table`
5. `az network private-dns link vnet list --resource-group <rg> --zone-name <zone>`

### ARM/Bicep deployment failure
1. `az deployment group list --resource-group <rg>`
2. `az deployment group show --resource-group <rg> --name <deployment>`
3. `az deployment group operation list --resource-group <rg> --name <deployment>`
4. `az policy assignment list --scope <scope>`
5. `az lock list --resource-group <rg>`

### AKS outage or pod crash loop
1. `az aks get-credentials --resource-group <rg> --name <aks>`
2. `kubectl config current-context`
3. `kubectl get pods -A -o wide`
4. `kubectl get events -A --sort-by=.lastTimestamp`
5. `kubectl describe pod <pod> -n <namespace>`

### App Service or Function App outage
1. `az webapp show --resource-group <rg> --name <app>`
2. `az functionapp show --resource-group <rg> --name <func>`
3. `az functionapp config appsettings list --resource-group <rg> --name <func>`
4. `az webapp log tail --resource-group <rg> --name <func>`
5. `az functionapp identity show --resource-group <rg> --name <func>`

### Key Vault or managed identity access denied
1. `az keyvault show --resource-group <rg> --name <vault>`
2. `az vm identity show --resource-group <rg> --name <vm>`
3. `az functionapp identity show --resource-group <rg> --name <func>`
4. `az role assignment list --scope $(az keyvault show --resource-group <rg> --name <vault> --query id -o tsv)`
5. `az keyvault secret list --vault-name <vault> -o table`

### Azure SQL or flexible database connectivity failure
1. `az sql db show --resource-group <rg> --server <server> --name <db>`
2. `az sql server firewall-rule list --resource-group <rg> --server <server> -o table`
3. `az postgres flexible-server show --resource-group <rg> --name <server>`
4. `az mysql flexible-server show --resource-group <rg> --name <server>`
5. `az network watcher test-connectivity --source-resource <source-id> --dest-address <host> --dest-port <port>`

### Storage or queue-backed workflow issue
1. `az storage account show --resource-group <rg> --name <account>`
2. `az storage container list --account-name <account> --auth-mode login -o table`
3. `az storage blob list --account-name <account> --container-name <container> --auth-mode login -o table`
4. `az storage queue list --account-name <account> --auth-mode login -o table`
5. `az storage message peek --queue-name <queue> --account-name <account> --auth-mode login`

### Policy-denied or governance-blocked remediation
1. `az policy assignment list --scope <scope>`
2. `az lock list --resource-group <rg>`
3. `az deployment sub list --location <region>`
4. `az monitor activity-log list --resource-group <rg> --offset 1d`
5. `az resource show --ids <resource-id>`

## Recommended Troubleshooting Flow

1. confirm tenant, subscription, resource group, and region before every major action
2. separate RBAC, policy, deployment, network, runtime, and application failure planes
3. prefer read-only inspection before mutation
4. collect boot logs, Activity Log evidence, monitor data, and service diagnostics before restart or reset actions
5. use resource IDs, effective-rule commands, and scoped queries aggressively to reduce noise
6. verify recovery from both the Azure control-plane perspective and the workload perspective

---

# Azure CLI, Auth, Tenant, and Subscription Context


## 1. `az version`


```bash

az version

```

**Risk:** Low risk

**Technical explanation:** Prints Azure CLI and extension versions so you can rule out client-version drift and broken extension behavior on operator workstations.

**Use when:** Use when CLI behavior differs across laptops, bastions, or CI runners.


## 2. `az account show`


```bash

az account show

```

**Risk:** Low risk

**Technical explanation:** Returns the active subscription, tenant, and user or service principal context, which is the first check against wrong-subscription execution.

**Use when:** Use before any potentially high-impact production command.


## 3. `az account list -o table`


```bash

az account list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists visible subscriptions so you can confirm whether the target subscription is accessible to the current principal.

**Use when:** Use when resources appear missing or access looks inconsistent across subscriptions.


## 4. `az account set --subscription <subscription-id-or-name>`


```bash

az account set --subscription <subscription-id-or-name>

```

**Risk:** Medium risk

**Technical explanation:** Switches the active subscription context, which is essential when troubleshooting multi-subscription production estates but changes the default target for subsequent commands.

**Use when:** Use only when you have confirmed the current subscription context is wrong.


## 5. `az configure --list-defaults`


```bash

az configure --list-defaults

```

**Risk:** Low risk

**Technical explanation:** Shows current CLI defaults such as group and location so you can detect hidden context affecting commands.

**Use when:** Use when commands unexpectedly resolve to the wrong resource group or location.


## 6. `az cloud show`


```bash

az cloud show

```

**Risk:** Low risk

**Technical explanation:** Displays the active Azure cloud environment, which matters in public, government, or sovereign cloud operations.

**Use when:** Use when cloud-environment mismatch may explain missing resources or auth failures.


## 7. `az login`


```bash

az login

```

**Risk:** Medium risk

**Technical explanation:** Starts interactive authentication when local user credentials are stale or absent and you need fresh access for incident work.

**Use when:** Use when local user auth is broken and you need a fresh operator session.


## 8. `az login --identity`


```bash

az login --identity

```

**Risk:** Medium risk

**Technical explanation:** Uses a managed identity for authentication when operating from an Azure-hosted machine with assigned identity.

**Use when:** Use when troubleshooting from an Azure VM or host with managed identity access.


## 9. `az account get-access-token`


```bash

az account get-access-token

```

**Risk:** Low risk

**Technical explanation:** Mints an access token for the current principal so you can validate token issuance or test raw ARM/API behavior.

**Use when:** Use when ARM or downstream API calls fail and token issuance must be proved.


## 10. `az group list -o table`


```bash

az group list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists resource groups in the active subscription so you can quickly scope where the affected resources live.

**Use when:** Use when initial incident scoping requires fast subscription inventory.


# Tenant, Resource Inventory, and RBAC/IAM


## 11. `az ad signed-in-user show`


```bash

az ad signed-in-user show

```

**Risk:** Low risk

**Technical explanation:** Returns the current Entra ID user object when you need to confirm operator identity at the directory level.

**Use when:** Use when Azure CLI identity and directory identity may differ in troubleshooting context.


## 12. `az ad sp list --display-name <name>`


```bash

az ad sp list --display-name <name>

```

**Risk:** Low risk

**Technical explanation:** Lists service principals matching a display name so you can validate whether a workload identity exists.

**Use when:** Use when managed app or automation identity lookup is needed.


## 13. `az role assignment list --assignee <principal-id>`


```bash

az role assignment list --assignee <principal-id>

```

**Risk:** Low risk

**Technical explanation:** Lists RBAC assignments for a user, group, or service principal during permission troubleshooting.

**Use when:** Use when access denied may stem from missing role assignments.


## 14. `az role definition list --name <role-name>`


```bash

az role definition list --name <role-name>

```

**Risk:** Low risk

**Technical explanation:** Returns role definition details so you can inspect the permissions included in a built-in or custom role.

**Use when:** Use when you need to know whether a role actually grants the required action.


## 15. `az role assignment list --scope <resource-id>`


```bash

az role assignment list --scope <resource-id>

```

**Risk:** Low risk

**Technical explanation:** Shows who has access at a given scope when resource-level RBAC may be causing control-plane issues.

**Use when:** Use when one resource behaves differently from the rest of the environment.


## 16. `az resource list -o table`


```bash

az resource list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists resources in the current subscription so you can inventory impacted services quickly.

**Use when:** Use when broad subscription-level asset discovery is needed.


## 17. `az resource show --ids <resource-id>`


```bash

az resource show --ids <resource-id>

```

**Risk:** Low risk

**Technical explanation:** Returns full ARM metadata for one resource, which helps validate type, location, tags, and provisioning state.

**Use when:** Use when one resource’s exact ARM state matters.


## 18. `az tag list`


```bash

az tag list

```

**Risk:** Low risk

**Technical explanation:** Lists subscription-level tags so you can understand governance and discover tagging schemas used for environment scoping.

**Use when:** Use when blast-radius scoping depends on resource tags or conventions.


## 19. `az lock list --resource-group <rg>`


```bash

az lock list --resource-group <rg>

```

**Risk:** Low risk

**Technical explanation:** Lists management locks in a resource group when delete or update operations fail unexpectedly.

**Use when:** Use when a planned remediation is blocked without a clear error at first glance.


## 20. `az policy assignment list --scope <scope>`


```bash

az policy assignment list --scope <scope>

```

**Risk:** Low risk

**Technical explanation:** Lists policy assignments applied at a scope, which is essential when deployments or changes are denied by governance.

**Use when:** Use when Azure Policy may silently block deployment or remediation steps.


# Resource Groups, Deployments, and ARM/Bicep Troubleshooting


## 21. `az group show --name <rg>`


```bash

az group show --name <rg>

```

**Risk:** Low risk

**Technical explanation:** Returns resource-group metadata including location and provisioning state for scoped troubleshooting.

**Use when:** Use when verifying you are working in the correct resource group and region.


## 22. `az deployment group list --resource-group <rg>`


```bash

az deployment group list --resource-group <rg>

```

**Risk:** Low risk

**Technical explanation:** Lists group-scoped deployments so you can identify recent changes or failed releases.

**Use when:** Use when an incident may correlate with a recent ARM or Bicep rollout.


## 23. `az deployment group show --resource-group <rg> --name <deployment>`


```bash

az deployment group show --resource-group <rg> --name <deployment>

```

**Risk:** Low risk

**Technical explanation:** Shows one deployment’s state, template references, and outputs during release debugging.

**Use when:** Use when one deployment is the likely source of the incident.


## 24. `az deployment group operation list --resource-group <rg> --name <deployment>`


```bash

az deployment group operation list --resource-group <rg> --name <deployment>

```

**Risk:** Low risk

**Technical explanation:** Returns per-resource deployment operations so you can see exactly which resource failed and why.

**Use when:** Use when a deployment failed and you need the exact failed operation.


## 25. `az deployment sub list --location <region>`


```bash

az deployment sub list --location <region>

```

**Risk:** Low risk

**Technical explanation:** Lists subscription-scoped deployments when platform-level rollouts or policy assignments are involved.

**Use when:** Use when governance or shared-platform changes may have caused the issue.


## 26. `az deployment sub show --name <deployment> --location <region>`


```bash

az deployment sub show --name <deployment> --location <region>

```

**Risk:** Low risk

**Technical explanation:** Shows one subscription-scoped deployment during governance or shared-platform troubleshooting.

**Use when:** Use when one subscription-scope deployment may have impacted many services.


## 27. `az deployment group validate --resource-group <rg> --template-file main.bicep`


```bash

az deployment group validate --resource-group <rg> --template-file main.bicep

```

**Risk:** Low risk

**Technical explanation:** Validates a template or Bicep deployment before applying changes, which helps separate syntax/config issues from runtime failures.

**Use when:** Use before reattempting a failed template deployment.


## 28. `az resource list --resource-group <rg> -o table`


```bash

az resource list --resource-group <rg> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists resources inside a resource group so you can narrow incident scope quickly.

**Use when:** Use when scoping impact within one application or platform resource group.


# Azure Virtual Machines, Availability, and Host State


## 29. `az vm list -d -o table`


```bash

az vm list -d -o table

```

**Risk:** Low risk

**Technical explanation:** Lists VMs with power state, private IP, and public IP so you can baseline compute status rapidly.

**Use when:** Use when fleet-level VM state needs quick review.


## 30. `az vm show --resource-group <rg> --name <vm> -d`


```bash

az vm show --resource-group <rg> --name <vm> -d

```

**Risk:** Low risk

**Technical explanation:** Returns detailed VM metadata including size, IPs, OS profile, and power state for one instance.

**Use when:** Use when one VM’s identity, networking, or power state may explain the problem.


## 31. `az vm get-instance-view --resource-group <rg> --name <vm>`


```bash

az vm get-instance-view --resource-group <rg> --name <vm>

```

**Risk:** Low risk

**Technical explanation:** Shows runtime instance view, including power state and extension statuses, which is one of the best first checks for VM issues.

**Use when:** Use when a VM is unhealthy, unreachable, or unexpectedly stopped.


## 32. `az vm boot-diagnostics get-boot-log --resource-group <rg> --name <vm>`


```bash

az vm boot-diagnostics get-boot-log --resource-group <rg> --name <vm>

```

**Risk:** Low risk

**Technical explanation:** Retrieves boot diagnostics serial log output when SSH or RDP is failing or the guest is stuck during startup.

**Use when:** Use when guest boot or kernel startup may be failing before network access is available.


## 33. `az vm boot-diagnostics get-boot-log-uris --resource-group <rg> --name <vm>`


```bash

az vm boot-diagnostics get-boot-log-uris --resource-group <rg> --name <vm>

```

**Risk:** Low risk

**Technical explanation:** Returns URIs for boot diagnostic logs so you can follow evidence collection paths during deep VM recovery.

**Use when:** Use when you need to collect or share boot evidence outside the CLI response.


## 34. `az vm restart --resource-group <rg> --name <vm>`


```bash

az vm restart --resource-group <rg> --name <vm>

```

**Risk:** High risk

**Technical explanation:** Performs a guest reboot when a VM may recover through restart, but should follow evidence collection in production because it disrupts workloads.

**Use when:** Use only after boot logs, instance view, and connectivity evidence have been reviewed.


## 35. `az vm stop --resource-group <rg> --name <vm>`


```bash

az vm stop --resource-group <rg> --name <vm>

```

**Risk:** High risk

**Technical explanation:** Stops a VM while preserving allocation state, which is useful for controlled service interruption or diagnostics but causes immediate workload impact.

**Use when:** Use only when downtime is acceptable or already in progress.


## 36. `az vm deallocate --resource-group <rg> --name <vm>`


```bash

az vm deallocate --resource-group <rg> --name <vm>

```

**Risk:** High risk

**Technical explanation:** Deallocates a VM fully so compute charges stop and certain host issues can be cleared, but workload impact is immediate and capacity placement may change on restart.

**Use when:** Use only when full stop/deallocation is part of a deliberate recovery plan.


## 37. `az vm start --resource-group <rg> --name <vm>`


```bash

az vm start --resource-group <rg> --name <vm>

```

**Risk:** Medium risk

**Technical explanation:** Starts a stopped or deallocated VM during controlled recovery.

**Use when:** Use when planned recovery requires bringing the VM back online.


## 38. `az vm list-ip-addresses --resource-group <rg> --name <vm>`


```bash

az vm list-ip-addresses --resource-group <rg> --name <vm>

```

**Risk:** Low risk

**Technical explanation:** Returns NIC and IP information so you can verify addressing during connectivity incidents.

**Use when:** Use when public/private IP drift or NIC mapping may explain access failures.


# VM Extensions, Run Command, and Guest Access


## 39. `az vm extension list --resource-group <rg> --vm-name <vm>`


```bash

az vm extension list --resource-group <rg> --vm-name <vm>

```

**Risk:** Low risk

**Technical explanation:** Lists installed VM extensions so you can inspect whether monitoring, dependency, or custom-script agents are failing.

**Use when:** Use when VM functionality depends on extensions and one may be unhealthy.


## 40. `az vm extension show --resource-group <rg> --vm-name <vm> --name <extension>`


```bash

az vm extension show --resource-group <rg> --vm-name <vm> --name <extension>

```

**Risk:** Low risk

**Technical explanation:** Shows one VM extension’s provisioning state and settings references.

**Use when:** Use when one extension is the likely root cause.


## 41. `az vm run-command invoke --resource-group <rg> --name <vm> --command-id RunShellScript --scripts 'uptime'`


```bash

az vm run-command invoke --resource-group <rg> --name <vm> --command-id RunShellScript --scripts 'uptime'

```

**Risk:** High risk

**Technical explanation:** Executes a command in the guest through the Azure control plane when direct SSH access is unavailable, but it runs code on the target host.

**Use when:** Use only when remote guest execution is justified and command blast radius is understood.


## 42. `az vm user update --resource-group <rg> --name <vm> --username <user> --password <password>`


```bash

az vm user update --resource-group <rg> --name <vm> --username <user> --password <password>

```

**Risk:** High risk

**Technical explanation:** Resets or updates a VM local user credential when guest-access remediation is necessary, but it changes access state on the host.

**Use when:** Use only when credential recovery is required and identity impact is understood.


## 43. `az network nic list --resource-group <rg> -o table`


```bash

az network nic list --resource-group <rg> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists NICs in a resource group so you can confirm VM-to-interface mapping during network incidents.

**Use when:** Use when one VM’s NIC or multiple NIC attachments may be involved.


## 44. `az network nic show --resource-group <rg> --name <nic>`


```bash

az network nic show --resource-group <rg> --name <nic>

```

**Risk:** Low risk

**Technical explanation:** Returns NIC configuration including IP configs, NSGs, and subnet attachments for focused network troubleshooting.

**Use when:** Use when one NIC is the best scope for network debugging.


## 45. `az vm identity show --resource-group <rg> --name <vm>`


```bash

az vm identity show --resource-group <rg> --name <vm>

```

**Risk:** Low risk

**Technical explanation:** Shows the managed identity configuration of a VM when workload access to Azure services is failing.

**Use when:** Use when the guest should access Key Vault, Storage, or other services via managed identity.


## 46. `az vm extension image list --location <region> -o table`


```bash

az vm extension image list --location <region> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists available extension images in a region when an extension deployment or upgrade is failing.

**Use when:** Use when extension availability or versioning may differ by region.


# Managed Disks, Snapshots, and Storage Attachments


## 47. `az disk list -o table`


```bash

az disk list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists managed disks so you can locate attached, unattached, or unexpectedly large disks in the subscription.

**Use when:** Use when storage topology or orphaned disks may matter.


## 48. `az disk show --resource-group <rg> --name <disk>`


```bash

az disk show --resource-group <rg> --name <disk>

```

**Risk:** Low risk

**Technical explanation:** Returns one managed disk’s properties including SKU, size, and attachment state.

**Use when:** Use when one disk’s performance tier or attachment state may explain the issue.


## 49. `az snapshot list -o table`


```bash

az snapshot list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists snapshots so you can confirm whether restore points exist before high-risk actions.

**Use when:** Use before destructive remediation or rollback planning.


## 50. `az snapshot show --resource-group <rg> --name <snapshot>`


```bash

az snapshot show --resource-group <rg> --name <snapshot>

```

**Risk:** Low risk

**Technical explanation:** Shows metadata for one snapshot, which helps validate source disk and region.

**Use when:** Use when one snapshot is being evaluated for recovery.


## 51. `az snapshot create --resource-group <rg> --name <snapshot> --source <disk-id>`


```bash

az snapshot create --resource-group <rg> --name <snapshot> --source <disk-id>

```

**Risk:** High risk

**Technical explanation:** Creates a disk snapshot before risky VM or storage changes when you need a rollback point, but it mutates storage state and may increase cost.

**Use when:** Use when a pre-change recovery point is required for a critical disk.


## 52. `az vm show --resource-group <rg> --name <vm> --query storageProfile`


```bash

az vm show --resource-group <rg> --name <vm> --query storageProfile

```

**Risk:** Low risk

**Technical explanation:** Displays a VM’s storage profile so you can confirm OS/data disk mappings and image references.

**Use when:** Use when the guest disk layout may not match expectations.


## 53. `az disk list --query "[?managedBy==null]" -o table`


```bash

az disk list --query "[?managedBy==null]" -o table

```

**Risk:** Low risk

**Technical explanation:** Finds unattached disks so you can identify orphaned storage after incidents or failed automation.

**Use when:** Use when cleanup, recovery, or failed detach workflows may have left orphaned disks.


## 54. `az vm disk list --resource-group <rg> --vm-name <vm>`


```bash

az vm disk list --resource-group <rg> --vm-name <vm>

```

**Risk:** Low risk

**Technical explanation:** Lists disks attached to a VM so you can verify expected data volumes are present.

**Use when:** Use when a VM appears to be missing data disks or attachments.


# Virtual Network, Subnets, NSGs, UDRs, and Connectivity


## 55. `az network vnet list -o table`


```bash

az network vnet list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists VNets so you can confirm which virtual network the workload should be using.

**Use when:** Use when workloads may be deployed in the wrong virtual network.


## 56. `az network vnet show --resource-group <rg> --name <vnet>`


```bash

az network vnet show --resource-group <rg> --name <vnet>

```

**Risk:** Low risk

**Technical explanation:** Returns VNet details including address space, peerings, and DNS settings.

**Use when:** Use when routing, peering, or DNS inheritance may explain behavior.


## 57. `az network vnet subnet list --resource-group <rg> --vnet-name <vnet> -o table`


```bash

az network vnet subnet list --resource-group <rg> --vnet-name <vnet> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists subnets in a VNet so you can inspect CIDRs and placement.

**Use when:** Use when subnet sizing, placement, or delegation may matter.


## 58. `az network nsg list -o table`


```bash

az network nsg list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists network security groups so you can inspect subnet- and NIC-level security controls.

**Use when:** Use when traffic may be blocked by security policy.


## 59. `az network nsg show --resource-group <rg> --name <nsg>`


```bash

az network nsg show --resource-group <rg> --name <nsg>

```

**Risk:** Low risk

**Technical explanation:** Shows one NSG’s rule set in detail for targeted policy debugging.

**Use when:** Use when one NSG is the primary suspect.


## 60. `az network nsg rule list --resource-group <rg> --nsg-name <nsg> -o table`


```bash

az network nsg rule list --resource-group <rg> --nsg-name <nsg> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists NSG rules in evaluation order so you can identify allow/deny conflicts quickly.

**Use when:** Use when rule priority or conflicting allows and denies may matter.


## 61. `az network route-table list -o table`


```bash

az network route-table list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists user-defined route tables so you can validate whether custom routing is in play.

**Use when:** Use when packet pathing may be affected by UDRs.


## 62. `az network route-table route list --resource-group <rg> --route-table-name <rt>`


```bash

az network route-table route list --resource-group <rg> --route-table-name <rt>

```

**Risk:** Low risk

**Technical explanation:** Lists routes inside one UDR table so you can inspect next hops and prefixes.

**Use when:** Use when one route table may be causing blackholing or unexpected egress paths.


## 63. `az network nic show-effective-route-table --resource-group <rg> --name <nic>`


```bash

az network nic show-effective-route-table --resource-group <rg> --name <nic>

```

**Risk:** Low risk

**Technical explanation:** Calculates the effective route table for a NIC, which is one of the most useful path-debugging commands in Azure.

**Use when:** Use when configured UDRs are not enough and you need the final effective route view.


## 64. `az network nic list-effective-nsg --resource-group <rg> --name <nic>`


```bash

az network nic list-effective-nsg --resource-group <rg> --name <nic>

```

**Risk:** Low risk

**Technical explanation:** Shows effective NSG rules applied to a NIC so you can see the final policy after inheritance.

**Use when:** Use when subnet and NIC policies may combine in unexpected ways.


# Azure Load Balancer, Application Gateway, and Front Door


## 65. `az network lb list -o table`


```bash

az network lb list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Azure Load Balancers so you can identify the frontend objects serving traffic.

**Use when:** Use when the correct layer-4 entry point must be identified first.


## 66. `az network lb show --resource-group <rg> --name <lb>`


```bash

az network lb show --resource-group <rg> --name <lb>

```

**Risk:** Low risk

**Technical explanation:** Returns full LB configuration including frontends, backends, rules, and probes.

**Use when:** Use when one load balancer’s exact rule chain must be validated.


## 67. `az network lb address-pool list --resource-group <rg> --lb-name <lb>`


```bash

az network lb address-pool list --resource-group <rg> --lb-name <lb>

```

**Risk:** Low risk

**Technical explanation:** Lists backend pools so you can verify which NICs or IPs should be serving traffic.

**Use when:** Use when traffic reaches the LB but not the intended backends.


## 68. `az network lb probe list --resource-group <rg> --lb-name <lb>`


```bash

az network lb probe list --resource-group <rg> --lb-name <lb>

```

**Risk:** Low risk

**Technical explanation:** Lists health probes configured on a Load Balancer during backend health troubleshooting.

**Use when:** Use when backend health may be failing due to probe configuration.


## 69. `az network application-gateway list -o table`


```bash

az network application-gateway list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Application Gateways so you can confirm the L7 entry point exists and is in the expected subscription.

**Use when:** Use when HTTP(S) entry may be owned by Application Gateway rather than Front Door or LB.


## 70. `az network application-gateway show --resource-group <rg> --name <agw>`


```bash

az network application-gateway show --resource-group <rg> --name <agw>

```

**Risk:** Low risk

**Technical explanation:** Returns Application Gateway listeners, probes, HTTP settings, and backend pool configuration.

**Use when:** Use when a layer-7 routing issue may be caused by gateway configuration.


## 71. `az network application-gateway show-backend-health --resource-group <rg> --name <agw>`


```bash

az network application-gateway show-backend-health --resource-group <rg> --name <agw>

```

**Risk:** Low risk

**Technical explanation:** Shows backend health from the Application Gateway perspective, which is one of the fastest checks during 502/503 incidents.

**Use when:** Use when the gateway is reachable but users still see backend failures.


## 72. `az afd profile list -o table`


```bash

az afd profile list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Azure Front Door profiles so you can confirm global edge routing objects exist.

**Use when:** Use when public traffic may be entering through Front Door rather than regional services.


# DNS, Private DNS, and Name Resolution


## 73. `az network dns zone list -o table`


```bash

az network dns zone list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists public DNS zones so you can validate authoritative DNS ownership.

**Use when:** Use when the authoritative public zone is unclear.


## 74. `az network dns zone show --resource-group <rg> --name <zone>`


```bash

az network dns zone show --resource-group <rg> --name <zone>

```

**Risk:** Low risk

**Technical explanation:** Returns public zone metadata for one DNS namespace.

**Use when:** Use when one zone’s settings or ownership matter.


## 75. `az network dns record-set list --resource-group <rg> --zone-name <zone> -o table`


```bash

az network dns record-set list --resource-group <rg> --zone-name <zone> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists DNS record sets in a public zone so you can inspect current authoritative values.

**Use when:** Use when validating public DNS records broadly.


## 76. `az network private-dns zone list -o table`


```bash

az network private-dns zone list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Private DNS zones when internal service resolution may be failing.

**Use when:** Use when internal name resolution behaves differently from public DNS.


## 77. `az network private-dns zone show --resource-group <rg> --name <zone>`


```bash

az network private-dns zone show --resource-group <rg> --name <zone>

```

**Risk:** Low risk

**Technical explanation:** Shows one Private DNS zone’s properties and links.

**Use when:** Use when private-zone linkage or scope may explain failures.


## 78. `az network private-dns record-set a list --resource-group <rg> --zone-name <zone> -o table`


```bash

az network private-dns record-set a list --resource-group <rg> --zone-name <zone> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists A records in a Private DNS zone to validate internal endpoint mapping.

**Use when:** Use when private endpoints or internal services resolve incorrectly.


## 79. `nslookup <fqdn>`


```bash

nslookup <fqdn>

```

**Risk:** Low risk

**Technical explanation:** Performs a quick DNS lookup from the shell to compare live resolver output with Azure DNS configuration.

**Use when:** Use when a basic live resolution check is enough.


## 80. `dig <fqdn>`


```bash

dig <fqdn>

```

**Risk:** Low risk

**Technical explanation:** Queries DNS in more detail, which helps distinguish authoritative state from resolver behavior.

**Use when:** Use when TTL, answer chain, or resolver detail matters.


# Azure Monitor, Metrics, Activity Log, and Resource Health


## 81. `az monitor metrics list --resource <resource-id> --metric Percentage CPU`


```bash

az monitor metrics list --resource <resource-id> --metric Percentage CPU

```

**Risk:** Low risk

**Technical explanation:** Pulls time-series metrics for a resource so you can validate whether the platform observed the same performance symptom.

**Use when:** Use when you need hard metric evidence around an incident window.


## 82. `az monitor metrics list-definitions --resource <resource-id>`


```bash

az monitor metrics list-definitions --resource <resource-id>

```

**Risk:** Low risk

**Technical explanation:** Lists available metric definitions so you can discover which signals exist for a resource type.

**Use when:** Use when you are unsure which metric names are supported.


## 83. `az monitor activity-log list --resource-group <rg> --offset 1d`


```bash

az monitor activity-log list --resource-group <rg> --offset 1d

```

**Risk:** Low risk

**Technical explanation:** Reads Activity Log events for the last day so you can identify recent control-plane changes and failures.

**Use when:** Use when the incident may correlate with recent Azure-side changes.


## 84. `az monitor activity-log list --resource-id <resource-id> --offset 1d`


```bash

az monitor activity-log list --resource-id <resource-id> --offset 1d

```

**Risk:** Low risk

**Technical explanation:** Scopes Activity Log to one resource so you can reconstruct change history without subscription-wide noise.

**Use when:** Use when one resource is the likely source of the failure.


## 85. `az monitor diagnostic-settings list --resource <resource-id>`


```bash

az monitor diagnostic-settings list --resource <resource-id>

```

**Risk:** Low risk

**Technical explanation:** Lists diagnostic settings so you can verify whether logs and metrics are being exported as expected.

**Use when:** Use when observability gaps themselves may be part of the incident.


## 86. `az monitor log-analytics workspace list -o table`


```bash

az monitor log-analytics workspace list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Log Analytics workspaces so you can locate where operational data is stored.

**Use when:** Use when responders do not know which workspace contains the evidence.


## 87. `az monitor log-analytics query --workspace <workspace-id> --analytics-query "AzureActivity | take 10"`


```bash

az monitor log-analytics query --workspace <workspace-id> --analytics-query "AzureActivity | take 10"

```

**Risk:** Low risk

**Technical explanation:** Executes a KQL query against a Log Analytics workspace for deep operational investigation.

**Use when:** Use when ARM or service logs need deeper investigation than Activity Log alone.


## 88. `az resource show --ids <resource-id> --query properties.resourceHealth`


```bash

az resource show --ids <resource-id> --query properties.resourceHealth

```

**Risk:** Low risk

**Technical explanation:** Checks ARM properties for health-related fields when resource health status may be relevant.

**Use when:** Use when Azure platform health may explain degraded resource behavior.


# Log Analytics and Kusto-Oriented Operational Queries


## 89. `az monitor log-analytics query --workspace <workspace-id> --analytics-query "Heartbeat | summarize count() by Computer"`


```bash

az monitor log-analytics query --workspace <workspace-id> --analytics-query "Heartbeat | summarize count() by Computer"

```

**Risk:** Low risk

**Technical explanation:** Queries agent heartbeat data to identify machines that stopped reporting.

**Use when:** Use when VMs or agents may have silently stopped sending telemetry.


## 90. `az monitor log-analytics query --workspace <workspace-id> --analytics-query "Syslog | where SeverityLevel <= 3 | take 20"`


```bash

az monitor log-analytics query --workspace <workspace-id> --analytics-query "Syslog | where SeverityLevel <= 3 | take 20"

```

**Risk:** Low risk

**Technical explanation:** Searches Linux syslog data for high-severity issues collected into Log Analytics.

**Use when:** Use when Linux guest errors need central log review.


## 91. `az monitor log-analytics query --workspace <workspace-id> --analytics-query "Event | where Level == 1 | take 20"`


```bash

az monitor log-analytics query --workspace <workspace-id> --analytics-query "Event | where Level == 1 | take 20"

```

**Risk:** Low risk

**Technical explanation:** Queries Windows event data for error-level operational evidence.

**Use when:** Use when Windows guest issues are suspected.


## 92. `az monitor log-analytics query --workspace <workspace-id> --analytics-query "AzureDiagnostics | where Level == 'Error' | take 20"`


```bash

az monitor log-analytics query --workspace <workspace-id> --analytics-query "AzureDiagnostics | where Level == 'Error' | take 20"

```

**Risk:** Low risk

**Technical explanation:** Searches AzureDiagnostics for service-side errors across integrated resources.

**Use when:** Use when Azure platform-integrated diagnostics may hold the root-cause evidence.


## 93. `az monitor log-analytics query --workspace <workspace-id> --analytics-query "Perf | where CounterName == '% Processor Time' | take 20"`


```bash

az monitor log-analytics query --workspace <workspace-id> --analytics-query "Perf | where CounterName == '% Processor Time' | take 20"

```

**Risk:** Low risk

**Technical explanation:** Retrieves performance counter data when investigating host-level saturation.

**Use when:** Use when CPU saturation may have driven the incident.


## 94. `az monitor log-analytics query --workspace <workspace-id> --analytics-query "KubePodInventory | take 20"`


```bash

az monitor log-analytics query --workspace <workspace-id> --analytics-query "KubePodInventory | take 20"

```

**Risk:** Low risk

**Technical explanation:** Pulls Kubernetes inventory data when AKS state needs to be validated through Log Analytics.

**Use when:** Use when kubectl is limited or a central AKS view is preferred.


# AKS, Kubernetes, and Cluster Access


## 95. `az aks list -o table`


```bash

az aks list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists AKS clusters so you can confirm which clusters exist and where they run.

**Use when:** Use when cluster location or naming may be the problem.


## 96. `az aks show --resource-group <rg> --name <aks>`


```bash

az aks show --resource-group <rg> --name <aks>

```

**Risk:** Low risk

**Technical explanation:** Returns cluster configuration including Kubernetes version, node pools, network settings, and identity.

**Use when:** Use when cluster-wide configuration may explain workload issues.


## 97. `az aks get-credentials --resource-group <rg> --name <aks>`


```bash

az aks get-credentials --resource-group <rg> --name <aks>

```

**Risk:** Medium risk

**Technical explanation:** Writes kubeconfig for the target AKS cluster so kubectl can reach the intended control plane, but it changes local kubectl context state.

**Use when:** Use when deliberate AKS access is required and the target cluster is confirmed.


## 98. `kubectl config current-context`


```bash

kubectl config current-context

```

**Risk:** Low risk

**Technical explanation:** Shows the active kubectl context so you can confirm you are troubleshooting the correct AKS cluster.

**Use when:** Use before any mutating kubectl command.


## 99. `kubectl get nodes -o wide`


```bash

kubectl get nodes -o wide

```

**Risk:** Low risk

**Technical explanation:** Lists AKS nodes and readiness states so you can baseline worker health quickly.

**Use when:** Use when cluster-wide health may be degraded.


## 100. `kubectl get pods -A -o wide`


```bash

kubectl get pods -A -o wide

```

**Risk:** Low risk

**Technical explanation:** Lists pods across namespaces to expose broad workload health, restarts, and placement issues.

**Use when:** Use when impact appears broader than one namespace.


## 101. `kubectl get events -A --sort-by=.lastTimestamp`


```bash

kubectl get events -A --sort-by=.lastTimestamp

```

**Risk:** Low risk

**Technical explanation:** Shows recent Kubernetes events, which often surface scheduling, image pull, and readiness failures rapidly.

**Use when:** Use when pod states alone do not explain the failure.


## 102. `kubectl describe pod <pod> -n <namespace>`


```bash

kubectl describe pod <pod> -n <namespace>

```

**Risk:** Low risk

**Technical explanation:** Provides detailed pod conditions, mounts, probes, and event history for one failing workload.

**Use when:** Use when a specific pod is unhealthy or crash-looping.


## 103. `kubectl logs <pod> -n <namespace> --previous`


```bash

kubectl logs <pod> -n <namespace> --previous

```

**Risk:** Low risk

**Technical explanation:** Fetches logs from the previous container instance when a pod is crash-looping.

**Use when:** Use when the current container already restarted and the prior failure logs are needed.


## 104. `az aks nodepool list --resource-group <rg> --cluster-name <aks> -o table`


```bash

az aks nodepool list --resource-group <rg> --cluster-name <aks> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists AKS node pools so you can inspect pool separation, scaling, and version skew.

**Use when:** Use when only some pools or node classes appear affected.


# Azure Container Apps, ACR, and App Service


## 105. `az containerapp list -o table`


```bash

az containerapp list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Azure Container Apps so you can confirm which apps are deployed in the subscription.

**Use when:** Use when service placement or environment membership is unclear.


## 106. `az containerapp show --resource-group <rg> --name <app>`


```bash

az containerapp show --resource-group <rg> --name <app>

```

**Risk:** Low risk

**Technical explanation:** Returns Container App configuration including ingress, revisions, and environment bindings.

**Use when:** Use when one app’s revision, ingress, or env config may explain the issue.


## 107. `az containerapp revision list --resource-group <rg> --name <app> -o table`


```bash

az containerapp revision list --resource-group <rg> --name <app> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists revisions so you can inspect rollout history and traffic targets.

**Use when:** Use when a recent revision rollout may have caused the incident.


## 108. `az acr list -o table`


```bash

az acr list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Azure Container Registries so you can identify the expected image registry.

**Use when:** Use when image pulls or release pipelines may target the wrong registry.


## 109. `az acr repository list --name <acr> -o table`


```bash

az acr repository list --name <acr> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists repositories in an ACR registry so you can confirm expected image paths exist.

**Use when:** Use when a deployment references an image repository that may not exist.


## 110. `az acr repository show-tags --name <acr> --repository <repo> -o table`


```bash

az acr repository show-tags --name <acr> --repository <repo> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists image tags in a repository so you can validate release identity and rollout expectations.

**Use when:** Use when mutable tags may hide what actually shipped.


## 111. `az webapp list -o table`


```bash

az webapp list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists App Services so you can find the affected web app quickly.

**Use when:** Use when initial scoping is needed across many web apps.


## 112. `az webapp show --resource-group <rg> --name <app>`


```bash

az webapp show --resource-group <rg> --name <app>

```

**Risk:** Low risk

**Technical explanation:** Returns App Service configuration including runtime, hostnames, and state.

**Use when:** Use when one App Service’s config or state may explain downtime.


# Azure Functions and Serverless


## 113. `az functionapp list -o table`


```bash

az functionapp list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Function Apps so you can confirm which serverless apps exist in the subscription.

**Use when:** Use when identifying the impacted serverless app.


## 114. `az functionapp show --resource-group <rg> --name <func>`


```bash

az functionapp show --resource-group <rg> --name <func>

```

**Risk:** Low risk

**Technical explanation:** Returns Function App configuration including runtime, storage, and site settings.

**Use when:** Use when runtime or site config may explain the failure.


## 115. `az functionapp config appsettings list --resource-group <rg> --name <func>`


```bash

az functionapp config appsettings list --resource-group <rg> --name <func>

```

**Risk:** Low risk

**Technical explanation:** Lists application settings so you can inspect environment configuration during incident debugging.

**Use when:** Use when app setting drift or missing config may be the root cause.


## 116. `az webapp log tail --resource-group <rg> --name <func>`


```bash

az webapp log tail --resource-group <rg> --name <func>

```

**Risk:** Medium risk

**Technical explanation:** Streams Function App logs live during reproduction or rollback validation, which can expose sensitive runtime behavior to the operator session.

**Use when:** Use when you need live serverless logs while reproducing or validating a fix.


## 117. `az functionapp deployment list-publishing-profiles --resource-group <rg> --name <func>`


```bash

az functionapp deployment list-publishing-profiles --resource-group <rg> --name <func>

```

**Risk:** Medium risk

**Technical explanation:** Returns publishing profiles when deployment-path or authentication issues may be involved, but it exposes deployment credentials and should be handled carefully.

**Use when:** Use only when deployment-path debugging requires publishing-profile inspection.


## 118. `az functionapp identity show --resource-group <rg> --name <func>`


```bash

az functionapp identity show --resource-group <rg> --name <func>

```

**Risk:** Low risk

**Technical explanation:** Shows managed identity configuration for a Function App when downstream Azure access fails.

**Use when:** Use when the function should access Azure resources via managed identity.


# Azure SQL, PostgreSQL, MySQL, and Data Services


## 119. `az sql server list -o table`


```bash

az sql server list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Azure SQL logical servers so you can identify the correct data-plane endpoint.

**Use when:** Use when database endpoint ownership or location is unclear.


## 120. `az sql db list --resource-group <rg> --server <server> -o table`


```bash

az sql db list --resource-group <rg> --server <server> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists databases on a logical server so you can confirm the expected DB exists.

**Use when:** Use when application connection strings may target the wrong database.


## 121. `az sql db show --resource-group <rg> --server <server> --name <db>`


```bash

az sql db show --resource-group <rg> --server <server> --name <db>

```

**Risk:** Low risk

**Technical explanation:** Returns database SKU, status, and configuration for one Azure SQL database.

**Use when:** Use when one database’s state, tier, or status may explain performance or availability issues.


## 122. `az sql server firewall-rule list --resource-group <rg> --server <server> -o table`


```bash

az sql server firewall-rule list --resource-group <rg> --server <server> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists SQL firewall rules so you can validate whether client access should be allowed.

**Use when:** Use when connectivity may be blocked at the database firewall layer.


## 123. `az postgres flexible-server list -o table`


```bash

az postgres flexible-server list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Azure Database for PostgreSQL flexible servers across the subscription.

**Use when:** Use when locating the affected PostgreSQL service.


## 124. `az postgres flexible-server show --resource-group <rg> --name <server>`


```bash

az postgres flexible-server show --resource-group <rg> --name <server>

```

**Risk:** Low risk

**Technical explanation:** Returns PostgreSQL flexible server state, HA, network mode, and version.

**Use when:** Use when one PostgreSQL server’s topology or status may explain failure.


## 125. `az mysql flexible-server list -o table`


```bash

az mysql flexible-server list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists MySQL flexible servers so you can locate the affected database service.

**Use when:** Use when the affected MySQL server must be identified quickly.


## 126. `az mysql flexible-server show --resource-group <rg> --name <server>`


```bash

az mysql flexible-server show --resource-group <rg> --name <server>

```

**Risk:** Low risk

**Technical explanation:** Shows one MySQL flexible server’s status and configuration.

**Use when:** Use when one MySQL instance’s settings or state may be the issue.


# Storage Accounts, Blobs, Queues, and File Shares


## 127. `az storage account list -o table`


```bash

az storage account list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists storage accounts so you can identify the correct account during data-path incidents.

**Use when:** Use when storage ownership or account selection is unclear.


## 128. `az storage account show --resource-group <rg> --name <account>`


```bash

az storage account show --resource-group <rg> --name <account>

```

**Risk:** Low risk

**Technical explanation:** Returns storage account properties including kind, replication, and network rules.

**Use when:** Use when replication mode or network ACLs may explain behavior.


## 129. `az storage account show-connection-string --resource-group <rg> --name <account>`


```bash

az storage account show-connection-string --resource-group <rg> --name <account>

```

**Risk:** Medium risk

**Technical explanation:** Returns a connection string for testing or validating storage client configuration, but it exposes sensitive access material.

**Use when:** Use only when connection-string-based validation is required and credential handling is controlled.


## 130. `az storage container list --account-name <account> --auth-mode login -o table`


```bash

az storage container list --account-name <account> --auth-mode login -o table

```

**Risk:** Low risk

**Technical explanation:** Lists blob containers so you can confirm data namespaces exist.

**Use when:** Use when object storage paths may be wrong or missing.


## 131. `az storage blob list --account-name <account> --container-name <container> --auth-mode login -o table`


```bash

az storage blob list --account-name <account> --container-name <container> --auth-mode login -o table

```

**Risk:** Low risk

**Technical explanation:** Lists blobs in a container so you can validate presence, naming, and recent deliveries.

**Use when:** Use when applications claim blobs are missing or delayed.


## 132. `az storage queue list --account-name <account> --auth-mode login -o table`


```bash

az storage queue list --account-name <account> --auth-mode login -o table

```

**Risk:** Low risk

**Technical explanation:** Lists storage queues so you can inspect messaging endpoints in the account.

**Use when:** Use when queue-backed workflows may be affected.


## 133. `az storage message peek --queue-name <queue> --account-name <account> --auth-mode login`


```bash

az storage message peek --queue-name <queue> --account-name <account> --auth-mode login

```

**Risk:** Low risk

**Technical explanation:** Peeks queue messages without removing them, which is useful when validating whether producers are sending data.

**Use when:** Use when diagnostic inspection must not consume messages.


## 134. `az storage share list --account-name <account> --auth-mode login -o table`


```bash

az storage share list --account-name <account> --auth-mode login -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Azure Files shares so you can verify expected SMB/NFS data shares exist.

**Use when:** Use when file-share-backed applications cannot find expected data.


# Service Bus, Event Grid, and Messaging


## 135. `az servicebus namespace list -o table`


```bash

az servicebus namespace list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Service Bus namespaces so you can find the messaging control-plane objects in use.

**Use when:** Use when initial messaging scoping is required.


## 136. `az servicebus queue list --resource-group <rg> --namespace-name <ns> -o table`


```bash

az servicebus queue list --resource-group <rg> --namespace-name <ns> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Service Bus queues so you can verify expected messaging endpoints.

**Use when:** Use when queue-backed processing may be failing.


## 137. `az servicebus topic list --resource-group <rg> --namespace-name <ns> -o table`


```bash

az servicebus topic list --resource-group <rg> --namespace-name <ns> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Service Bus topics so you can inspect pub/sub paths.

**Use when:** Use when fan-out or topic-based delivery may be affected.


## 138. `az servicebus topic subscription list --resource-group <rg> --namespace-name <ns> --topic-name <topic> -o table`


```bash

az servicebus topic subscription list --resource-group <rg> --namespace-name <ns> --topic-name <topic> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists topic subscriptions so you can validate downstream fan-out configuration.

**Use when:** Use when publishes succeed but downstream consumers may not be subscribed correctly.


## 139. `az eventgrid topic list -o table`


```bash

az eventgrid topic list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Event Grid topics so you can verify event-ingress endpoints.

**Use when:** Use when event-ingress objects may be missing or mis-targeted.


## 140. `az eventgrid event-subscription list --source-resource-id <resource-id>`


```bash

az eventgrid event-subscription list --source-resource-id <resource-id>

```

**Risk:** Low risk

**Technical explanation:** Lists Event Grid subscriptions attached to a source so you can inspect event-routing configuration.

**Use when:** Use when events are not reaching downstream handlers.


# Key Vault, Secrets, Certificates, and Identity-Backed Access


## 141. `az keyvault list -o table`


```bash

az keyvault list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Key Vaults so you can identify where secrets and certificates are stored.

**Use when:** Use when application config depends on one of several vaults.


## 142. `az keyvault show --resource-group <rg> --name <vault>`


```bash

az keyvault show --resource-group <rg> --name <vault>

```

**Risk:** Low risk

**Technical explanation:** Returns Key Vault properties including network ACLs and soft-delete settings.

**Use when:** Use when network restrictions or vault mode may explain access failures.


## 143. `az keyvault secret list --vault-name <vault> -o table`


```bash

az keyvault secret list --vault-name <vault> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists secrets in a vault so you can confirm expected configuration objects exist.

**Use when:** Use when one secret may be missing or misnamed.


## 144. `az keyvault secret show --vault-name <vault> --name <secret>`


```bash

az keyvault secret show --vault-name <vault> --name <secret>

```

**Risk:** Medium risk

**Technical explanation:** Returns one secret’s current version metadata during configuration troubleshooting and can expose sensitive values depending on output handling.

**Use when:** Use only when direct secret inspection is justified and output handling is controlled.


## 145. `az keyvault certificate list --vault-name <vault> -o table`


```bash

az keyvault certificate list --vault-name <vault> -o table

```

**Risk:** Low risk

**Technical explanation:** Lists certificates in a vault so you can inspect certificate presence and rotation state.

**Use when:** Use when TLS or mTLS issues may be tied to certificate lifecycle.


## 146. `az role assignment list --scope $(az keyvault show --resource-group <rg> --name <vault> --query id -o tsv)`


```bash

az role assignment list --scope $(az keyvault show --resource-group <rg> --name <vault> --query id -o tsv)

```

**Risk:** Low risk

**Technical explanation:** Lists RBAC assignments scoped to a Key Vault when secret access is failing under RBAC mode.

**Use when:** Use when vault access is denied and RBAC, not access policy mode, is in use.


# Azure Firewall, Bastion, Private Endpoints, and Edge Connectivity


## 147. `az network firewall list -o table`


```bash

az network firewall list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Azure Firewalls so you can confirm whether centralized egress or inspection is in the path.

**Use when:** Use when shared egress or inspection layers may be blocking traffic.


## 148. `az network firewall show --resource-group <rg> --name <fw>`


```bash

az network firewall show --resource-group <rg> --name <fw>

```

**Risk:** Low risk

**Technical explanation:** Returns firewall configuration including IPs, policy references, and provisioning state.

**Use when:** Use when one Azure Firewall may be the network choke point.


## 149. `az network bastion list -o table`


```bash

az network bastion list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists Bastion hosts so you can verify whether browser-based VM access is available.

**Use when:** Use when SSH/RDP access paths are intentionally restricted.


## 150. `az network bastion show --resource-group <rg> --name <bastion>`


```bash

az network bastion show --resource-group <rg> --name <bastion>

```

**Risk:** Low risk

**Technical explanation:** Shows one Bastion host’s configuration and scale units.

**Use when:** Use when Bastion availability or configuration may explain access issues.


## 151. `az network private-endpoint list -o table`


```bash

az network private-endpoint list -o table

```

**Risk:** Low risk

**Technical explanation:** Lists private endpoints so you can identify private-link backed service access paths.

**Use when:** Use when a service should be reachable only via private endpoint.


## 152. `az network private-endpoint show --resource-group <rg> --name <pe>`


```bash

az network private-endpoint show --resource-group <rg> --name <pe>

```

**Risk:** Low risk

**Technical explanation:** Returns one private endpoint’s NICs, subnet, and connection status.

**Use when:** Use when one private endpoint may be disconnected or mis-linked.


## 153. `az network private-dns link vnet list --resource-group <rg> --zone-name <zone>`


```bash

az network private-dns link vnet list --resource-group <rg> --zone-name <zone>

```

**Risk:** Low risk

**Technical explanation:** Lists VNet links on a Private DNS zone so you can validate internal DNS reachability paths.

**Use when:** Use when private DNS works from some VNets but not others.


## 154. `az network watcher test-connectivity --source-resource <source-id> --dest-address <host> --dest-port <port>`


```bash

az network watcher test-connectivity --source-resource <source-id> --dest-address <host> --dest-port <port>

```

**Risk:** Low risk

**Technical explanation:** Performs an Azure-side connectivity test to validate network reachability between source and destination.

**Use when:** Use when you need Azure’s perspective on source-to-destination connectivity.

