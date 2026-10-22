# Azure Week-Wise Cloud Coverage

Use this as an Azure-focused study guide for Azure Cloud Platform, Azure Solutions Architect, SRE, DevOps, Cloud, Security, disaster recovery (DR), and migration interviews.

## Azure Week 1: Landing Zones, Subscriptions, And Governance

### Service And Entity Coverage

- [Priority: Very High] Azure management groups, Azure subscriptions, Azure resource groups, and Azure landing zones: tenant root group, management group hierarchy, subscriptions, resource groups, landing zone archetypes, platform subscriptions, application subscriptions, and Azure landing zone accelerators.
- [Priority: Very High] Microsoft Entra ID and Microsoft Entra Privileged Identity Management: tenants, users, groups, enterprise applications, app registrations, service principals, managed identities, Conditional Access policies, access reviews, eligible assignments, active assignments, and role activation settings.
- [Priority: Very High] Azure role-based access control (Azure RBAC): scopes, role definitions, role assignments, built-in roles, custom roles, deny assignments, managed identities, and privileged roles.
- [Priority: Very High] Azure Policy: policy definitions, initiatives, assignments, exemptions, parameters, effects, compliance states, remediation tasks, and policy inheritance.
- [Priority: High] Azure Resource Graph, Azure Deployment Stacks, management locks, tags, and naming standards: resource queries, subscriptions, management groups, deployment stacks, stack resources, deny settings, CanNotDelete locks, ReadOnly locks, tag names, tag values, and naming rules.
- [Priority: Medium] Azure Lighthouse: managing tenants, customer tenants, delegated resource management, offers, assignments, authorizations, and eligible authorizations.
- [Priority: High] Microsoft Cost Management: billing accounts, billing profiles, invoice sections, subscriptions, budgets, alerts, cost allocation rules, exports, and reservations.

### Controls

- Management group hierarchy, subscription vending, resource group ownership, tag/naming policy, and landing-zone baseline inheritance.
- Microsoft Entra Conditional Access, Privileged Identity Management activation, access reviews, break-glass accounts, managed identities, and service principal governance.
- Azure RBAC scope design, built-in/custom roles, least privilege, privileged role assignment, and deny assignment awareness.
- Azure Policy initiative assignment, effect selection, exemptions, remediation tasks, and compliance reporting.
- Deployment Stack deny settings, management locks, Resource Graph inventory queries, and platform baseline validation.
- Cost Management budgets, alerts, exports, reservations, and subscription chargeback ownership.

### Operations

- Design a management group, subscription, and resource group model for platform, shared services, and workloads.
- Create subscription vending standards with naming, tags, budgets, policy assignments, RBAC, and diagnostic settings.
- Review Microsoft Entra privileged roles, Conditional Access, managed identities, and break-glass readiness.
- Validate Azure Policy compliance, exemptions, and remediation tasks before onboarding workloads.
- Query Azure Resource Graph for inventory, ownership, and governance evidence.
- Validate landing-zone readiness before migration by checking identity, policy, networking, billing, quotas, and operational access.

## Azure Week 2: Networking, Private Connectivity, And Edge Traffic

### Service And Entity Coverage

- [Priority: Very High] Azure Virtual Network: virtual networks, subnets, route tables, user-defined routes, network security groups (NSGs), application security groups, service endpoints, private endpoints, Azure Private Link, and virtual network peering.
- [Priority: High] Azure Virtual Network Manager and Azure Virtual WAN: network groups, connectivity configurations, security admin configurations, virtual hubs, hub route tables, connections, secured virtual hubs, and routing intent.
- [Priority: Very High] Azure Firewall, Azure Firewall Manager, Azure Bastion, and Azure NAT Gateway: firewalls, firewall policies, rule collection groups, application rules, network rules, DNAT rules, public IP addresses, bastion hosts, NAT gateways, and outbound IPs.
- [Priority: Very High] Azure VPN Gateway and Azure ExpressRoute: virtual network gateways, local network gateways, VPN connections, tunnels, ExpressRoute circuits, peerings, route filters, authorization keys, and ExpressRoute gateways.
- [Priority: Very High] Azure DNS, Azure Private DNS, and Azure DNS Private Resolver: DNS zones, record sets, private DNS zones, virtual network links, inbound endpoints, outbound endpoints, rulesets, and forwarding rules.
- [Priority: Very High] Azure Load Balancer, Azure Application Gateway, Azure Front Door, Azure Traffic Manager, and Azure CDN: frontend IP configurations, backend pools, probes, load balancing rules, listeners, routing rules, WAF policies, Front Door profiles, endpoints, origins, origin groups, Traffic Manager profiles, and CDN endpoints.
- [Priority: High] Azure DDoS Protection, Azure Web Application Firewall, and Azure Network Watcher: DDoS protection plans, WAF policies, packet capture, connection monitor, flow logs, effective routes, topology, and IP flow verify.
- [Priority: High] Azure API Management: API Management instances, APIs, products, subscriptions, policies, backends, gateways, revisions, versions, and developer portal.

### Controls

- Virtual network CIDR planning, subnet delegation, route table association, NSG rules, application security groups, private endpoints, and Private Link approval.
- Virtual Network Manager connectivity/security configurations, Virtual WAN hub routing, secured hub policies, and routing intent.
- Azure Firewall policy hierarchy, rule collection groups, egress filtering, DNAT, threat intelligence mode, Bastion access, and NAT Gateway outbound design.
- VPN Gateway tunnel settings, BGP, active-active mode, ExpressRoute peering, route filters, private peering, and gateway resiliency.
- Private DNS zone linking, DNS Private Resolver forwarding rules, split-horizon DNS, and DNS cutover controls.
- Load balancer probes, Application Gateway listeners, WAF policy, Front Door origin groups, Traffic Manager routing methods, and TLS certificates.
- Network Watcher flow logs, packet capture, connection monitor, effective routes, and DDoS/WAF protection policies.
- API Management authentication, authorization policies, rate limits, backends, revisions, and versioning.

### Operations

- Validate packet paths with effective routes, NSG rules, IP flow verify, Connection Monitor, and flow logs.
- Troubleshoot private endpoint DNS, Private Link approval, and hybrid name resolution.
- Review Azure Firewall rule hits, NAT Gateway port exhaustion, egress paths, and blocked traffic.
- Test VPN Gateway failover, ExpressRoute routing, and hybrid BGP route propagation.
- Execute DNS cutover with Azure DNS, Private DNS, DNS Private Resolver, and rollback TTL planning.
- Review Application Gateway/Front Door health probes, WAF false positives, origin failover, and traffic routing.
- Deploy API Management APIs, policies, backends, revisions, and troubleshoot gateway access logs.

## Azure Week 3: Compute, Azure Kubernetes Service, DevOps, And Platform Delivery

### Service And Entity Coverage

- [Priority: Very High] Azure Virtual Machines and Azure Virtual Machine Scale Sets: virtual machines, VM sizes, images, disks, network interfaces, availability sets, availability zones, scale sets, upgrade policies, health probes, extensions, custom script extensions, and managed identities.
- [Priority: Very High] Azure Kubernetes Service (AKS): clusters, node pools, nodes, Kubernetes versions, system node pools, user node pools, managed identities, workload identity, ingress controllers, network policies, Azure Policy add-on, and cluster audit logs.
- [Priority: Very High] Azure App Service, Azure Functions, Azure Container Apps, and Azure Container Instances: app services, app service plans, deployment slots, function apps, triggers, bindings, container apps, revisions, jobs, environments, replicas, and container groups.
- [Priority: High] Azure Container Registry and Microsoft Defender for Cloud container signals: registries, repositories, images, tags, tasks, webhooks, private endpoints, content trust/signing patterns, and vulnerability findings.
- [Priority: High] Azure DevOps and GitHub Actions on Azure: projects, repositories, pipelines, environments, service connections, variable groups, agents, workflow runs, secrets, environments, and deployment approvals.
- [Priority: High] Azure Resource Manager templates, Azure Bicep, and Azure Deployment Environments: ARM templates, Bicep files, modules, parameters, outputs, deployments, environment definitions, catalogs, projects, and environments.
- [Priority: Medium] Azure Batch: pools, compute nodes, jobs, tasks, application packages, start tasks, and autoscale formulas.
- [Priority: Medium] Azure Machine Learning and Azure AI Foundry: workspaces, compute instances, compute clusters, environments, data assets, datastores, jobs, pipelines, model registries, managed online endpoints, batch endpoints, prompt flows, hubs, projects, and connections.
- [Priority: Medium] Azure OpenAI Service: Azure OpenAI resources, deployments, models, content filters, quotas, private endpoints, and diagnostic settings.

### Controls

- VM managed identity, disk encryption, VM extensions, patch settings, scale set upgrade policy, health probes, and zone placement.
- AKS private clusters, authorized IP ranges, managed identities, workload identity, network policy, Azure Policy add-on, upgrade channels, and node pool isolation.
- App Service access restrictions, deployment slots, managed identities, VNet integration, Functions identity, Container Apps ingress, revision traffic splitting, and secrets.
- Azure Container Registry private endpoints, repository access, image scanning, signing patterns, retention, and quarantine workflow.
- Azure DevOps service connections, approvals, protected environments, variable groups, secret management, and agent pool isolation.
- ARM/Bicep parameter controls, module reuse, what-if review, deployment scopes, deployment stacks, and environment catalog governance.
- Azure Machine Learning workspace private networking, managed identities, compute isolation, model approval, endpoint access, and data access controls.
- Azure OpenAI content filters, private networking, managed identity, diagnostic settings, and quota governance.

### Operations

- Roll out VM image updates, scale set upgrades, health probe changes, and extension remediation.
- Upgrade AKS clusters/node pools, troubleshoot pod scheduling, workload identity, ingress, image pulls, and rollout health.
- Deploy App Service slots, Functions, Container Apps revisions, traffic splits, and rollback failed revisions.
- Promote images through Azure Container Registry with vulnerability review and signing controls.
- Run Azure DevOps/GitHub Actions pipelines with approvals, deployment environments, rollback, and artifact promotion.
- Review ARM/Bicep what-if output, deploy infrastructure, troubleshoot drift, and rollback failed deployments.
- Run Azure Machine Learning training, pipeline execution, model registration, endpoint deployment, and endpoint troubleshooting.

## Azure Week 4: Data, Storage, Backup, And Stateful Reliability

### Service And Entity Coverage

- [Priority: Very High] Azure Storage and Azure Data Lake Storage Gen2: storage accounts, containers, blobs, access tiers, lifecycle management policies, object replication, immutable storage, versioning, snapshots, file shares, queues, tables, hierarchical namespace, directories, and access control lists.
- [Priority: High] Azure managed disks, Azure Files, and Azure NetApp Files: managed disks, snapshots, disk encryption sets, shared disks, file shares, private endpoints, NetApp accounts, capacity pools, volumes, snapshots, and backup policies.
- [Priority: Very High] Azure SQL Database, Azure SQL Managed Instance, Azure Database for PostgreSQL, Azure Database for MySQL, and AlloyDB-equivalent migration considerations: logical servers, databases, elastic pools, managed instances, failover groups, backups, private endpoints, firewall rules, read replicas, and point-in-time restore.
- [Priority: Very High] Azure Cosmos DB: accounts, databases, containers, partition keys, items, consistency levels, regions, multi-region writes, change feed, indexing policies, throughput, autoscale, and backups.
- [Priority: High] Azure Cache for Redis: caches, clustering, replicas, private endpoints, persistence, access keys, firewall rules, and maintenance windows.
- [Priority: Very High] Azure Event Hubs, Azure Service Bus, and Azure Event Grid: namespaces, event hubs, consumer groups, partitions, capture, queues, topics, subscriptions, dead-letter queues, events, event subscriptions, domains, and system topics.
- [Priority: High] Azure Data Factory, Azure Synapse Analytics, Azure Databricks, Azure Stream Analytics, Microsoft Fabric, and Microsoft OneLake: factories, pipelines, datasets, linked services, triggers, integration runtimes, Synapse workspaces, SQL pools, Spark pools, Databricks workspaces, clusters, jobs, streams, lakehouses, warehouses, and shortcuts.
- [Priority: High] Azure AI Search, Microsoft Purview, and data classification: search services, indexes, indexers, data sources, skillsets, Purview accounts, collections, scans, classifications, glossary terms, lineage, and data map assets.
- [Priority: High] Azure Key Vault and Azure Backup: vaults, keys, secrets, certificates, key versions, access policies, RBAC assignments, backup vaults, Recovery Services vaults, backup policies, recovery points, snapshots, and restore jobs.

### Controls

- Storage account public access prevention, network rules, private endpoints, shared access signatures, lifecycle policies, immutable storage, versioning, replication, and customer-managed keys.
- Disk encryption sets, snapshot policies, Azure Files identity-based access, Azure NetApp Files backup policies, private endpoints, and throughput/capacity controls.
- Azure SQL firewall rules, private endpoints, Microsoft Entra authentication, transparent data encryption, backup retention, failover groups, maintenance windows, and point-in-time restore.
- Cosmos DB partition-key design, consistency level, indexing policy, multi-region writes, backup mode, throughput/autoscale, and private endpoints.
- Event Hubs capture, partition count, consumer groups, Service Bus lock duration, sessions, duplicate detection, dead-letter queues, Event Grid filtering, retry policies, and delivery endpoints.
- Data Factory integration runtimes, linked service credentials, managed identities, Synapse workspace firewall, Databricks secret scopes, and Fabric/OneLake access controls.
- Microsoft Purview scans, classifications, glossary ownership, lineage capture, and Azure AI Search access controls.
- Key Vault soft delete, purge protection, RBAC, private endpoints, key rotation, backup policies, recovery point retention, and restore permissions.

### Operations

- Restore blobs/files/snapshots, validate retention policies, review replication, and troubleshoot storage access.
- Resize disks, restore snapshots, validate file share access, and test Azure NetApp Files recovery.
- Test Azure SQL failover groups, point-in-time restore, read replicas, connection limits, and migration cutover.
- Troubleshoot Cosmos DB hot partitions, RU throttling, indexing, consistency, change feed, and multi-region replication.
- Monitor Event Hubs lag, Service Bus dead-letter queues, retry behavior, and Event Grid delivery failures.
- Run Data Factory/Synapse/Databricks pipelines, validate integration runtimes, and troubleshoot streaming jobs.
- Review Purview scans, data classification, lineage, and data access evidence.
- Rotate Key Vault secrets/keys, validate private endpoint access, test backups, and restore recovery points.

## Azure Week 5: Observability, FinOps, And Operational Excellence

### Service And Entity Coverage

- [Priority: Very High] Azure Monitor, Log Analytics workspaces, Application Insights, Azure Monitor Agent, and Data Collection Rules: metrics, logs, workspaces, tables, queries, alert rules, dashboards, workbooks, agents, data collection endpoints, data collection rules, action groups, and application maps.
- [Priority: High] Azure Monitor managed service for Prometheus and Azure Managed Grafana: Prometheus workspaces, scrape configs, rule groups, Grafana workspaces, dashboards, data sources, users, teams, and service accounts.
- [Priority: High] Azure Activity Log, Azure Service Health, Azure Resource Health, Azure Monitor Change Analysis, and Azure Network Watcher: activity events, service health alerts, resource health states, change records, packet captures, connection monitors, NSG flow logs, and topology.
- [Priority: High] Azure Automation and Azure Update Manager: automation accounts, runbooks, schedules, hybrid workers, update configurations, machines, update assessments, and maintenance configurations.
- [Priority: High] Microsoft Defender for Cloud, Microsoft Sentinel, and Microsoft Purview: secure score, recommendations, regulatory compliance controls, Defender plans, Sentinel workspaces, data connectors, analytics rules, incidents, watchlists, playbooks, and Purview classifications.
- [Priority: High] Microsoft Cost Management, Azure Advisor, Azure Reservations, and Azure savings plan for compute: cost analysis views, budgets, exports, alerts, recommendations, reservations, savings plans, scopes, and amortized cost views.
- [Priority: Medium] Azure Machine Learning model monitoring, responsible AI dashboard, Azure OpenAI Service, and Azure AI Content Safety: monitoring signals, data drift, model explanations, content filters, safety categories, evaluations, and diagnostic logs.

### Controls

- Azure Monitor workspace topology, diagnostic settings, data collection rules, log retention, table access, alert rules, action groups, workbook access, and Application Insights sampling.
- Managed Prometheus scrape config, rule groups, Grafana data source permissions, dashboard access, and identity configuration.
- Activity Log export, Service Health alerts, Resource Health tracking, Network Watcher flow logs, packet capture permissions, and change analysis visibility.
- Automation account identity, runbook permissions, hybrid worker access, Update Manager maintenance configuration, and patch compliance settings.
- Defender for Cloud plan enablement, regulatory compliance, Sentinel data connector access, analytics rule tuning, incident automation, Purview scan/classification access, and retention.
- Cost Management budgets, exports, cost allocation tags, Advisor recommendation review, reservation ownership, savings plan commitment tracking, and scope permissions.
- Azure Machine Learning monitoring thresholds, responsible AI artifact access, Azure OpenAI diagnostic settings, content safety filters, and private endpoint access.

### Operations

- Build Azure Monitor dashboards, Kusto Query Language (KQL) queries, alert rules, action groups, and incident timelines.
- Validate Prometheus scraping, Grafana dashboards, log ingestion, and metric cardinality.
- Use Activity Log, Service Health, Resource Health, Change Analysis, and Network Watcher during incidents.
- Run automation jobs, patch assessments, maintenance configurations, and update compliance reviews.
- Triage Defender for Cloud recommendations, Sentinel incidents, analytics rules, and Purview classification findings.
- Review budgets, cost exports, Advisor recommendations, reservations, savings plans, and right-sizing actions.
- Review model drift, responsible AI dashboards, Azure OpenAI content filtering, and AI safety evidence.

## Azure Week 6: DR, Resilience, And Migration Execution

### Service And Entity Coverage

- [Priority: Very High] Azure Site Recovery and Recovery Services vaults: vaults, replicated items, replication policies, mobility agents, recovery plans, test failovers, failovers, failbacks, recovery points, and protected machines.
- [Priority: Very High] Azure Migrate and Azure Database Migration Service: projects, appliances, discovered servers, assessments, groups, migration waves, replication jobs, test migrations, cutovers, source endpoints, target endpoints, migration projects, and migration activities.
- [Priority: High] Azure Resource Mover: move collections, resources, dependencies, prepare operations, initiate move operations, commit operations, and discard operations.
- [Priority: High] Azure Data Box and Azure Storage Mover: Data Box orders, devices, jobs, shares, transfer logs, Storage Mover agents, projects, endpoints, job definitions, and job runs.
- [Priority: Medium] Azure Arc: Azure Arc-enabled servers, connected machines, extensions, policies, guest configurations, and update management integrations.
- [Priority: Medium] Azure Load Testing and Azure Chaos Studio: load tests, test engines, test runs, metrics, chaos experiments, targets, capabilities, steps, branches, and faults.
- [Priority: Medium] Azure Well-Architected Review: workloads, pillars, assessments, recommendations, and action plans.

### DR Dependencies From Earlier Weeks

- Azure DNS, Azure Traffic Manager, Azure Front Door, Azure Load Balancer, Azure Application Gateway, VPN Gateway, and ExpressRoute are covered in Week 2.
- Azure Storage replication, Azure Files, Azure NetApp Files, Azure SQL failover groups, Azure Cosmos DB multi-region writes, Azure Event Hubs, Azure Service Bus, Azure Key Vault, and Azure Backup are covered in Week 4.
- Azure Monitor, Log Analytics, Service Health, Resource Health, Defender for Cloud, Sentinel, and Cost Management are covered in Week 5.

### Controls

- Azure Site Recovery replication policies, recovery point retention, test failover networks, recovery plan groups, failback settings, and vault access controls.
- Azure Migrate appliance access, discovery scope, assessment assumptions, dependency mapping, target sizing, migration wave grouping, and cutover controls.
- Database Migration Service source/target connectivity, migration mode, schema compatibility, validation, downtime window, and rollback planning.
- Resource Mover dependency validation, prepare/move/commit flow, target region readiness, and rollback checkpoints.
- Data Box chain-of-custody, encryption, job access, Storage Mover agent configuration, endpoint permissions, and transfer validation.
- Chaos Studio experiment permissions, target selection, blast-radius control, stop conditions, and monitoring integration.

### Operations

- Run Azure Site Recovery test failovers, planned failovers, unplanned failovers, failbacks, and recovery plan validation.
- Use Azure Migrate for discovery, dependency grouping, assessment, test migration, cutover, and rollback planning.
- Execute database migration rehearsals, validate data, monitor replication, and complete cutover with rollback criteria.
- Move regional resources with Azure Resource Mover and validate dependency readiness.
- Transfer offline or large datasets with Azure Data Box or Azure Storage Mover and validate checksums/cutover sync.
- Run Azure Chaos Studio and Azure Load Testing exercises with monitoring, stop conditions, and post-test evidence.