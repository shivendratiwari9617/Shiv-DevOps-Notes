# Google Cloud Week-Wise Cloud Coverage

Use this as a Google Cloud-focused study guide for Google Cloud Platform, Google Cloud Solutions Architect, SRE, DevOps, Cloud, Security, disaster recovery (DR), and migration interviews.

## Google Cloud Week 1: Resource Hierarchy, Landing Zone, And Governance

### Service And Entity Coverage

- [Priority: Very High] Resource Manager and Cloud Identity: organizations, folders, projects, project IDs, project numbers, labels, tags, Cloud Identity users, groups, and domains.
- [Priority: Very High] Identity and Access Management (IAM): principals, roles, permissions, allow policies, deny policies, IAM Conditions, service accounts, service account keys, workload identity pools, workload identity providers, and policy bindings.
- [Priority: High] Organization Policy Service: organization policies, constraints, policy rules, inheritance, dry-run policies, and custom constraints.
- [Priority: High] Cloud Asset Inventory and Policy Intelligence: assets, asset feeds, asset history, IAM policy analysis, Policy Analyzer, Policy Troubleshooter, and IAM Recommender recommendations.
- [Priority: Medium] Config Connector and Config Controller: Kubernetes custom resources, Google Cloud resource manifests, Config Controller instances, namespaces, and GitOps-managed resources.
- [Priority: High] Shared VPC and project factory patterns: host projects, service projects, Shared VPC networks, subnet attachments, project vending, project ownership metadata, and quota baselines.
- [Priority: High] Access Context Manager and VPC Service Controls: access policies, access levels, service perimeters, perimeter bridges, protected services, ingress rules, and egress rules.
- [Priority: Medium] Assured Workloads: workload folders, compliance regimes, resource restrictions, control packages, and monitoring status.
- [Priority: Medium] Cloud Billing and quotas: billing accounts, billing subaccounts, budgets, alerts, project billing links, quotas, quota limits, and quota usage.

### Controls

- Organization/folder/project hierarchy, folder inheritance, project ownership, label/tag standards, and project vending guardrails.
- IAM least privilege, conditional access, service account key restrictions, workload identity federation, break-glass access, and policy troubleshooting.
- Organization Policy constraints, dry-run validation, custom constraints, and policy inheritance controls.
- Cloud Asset Inventory feeds, IAM policy analysis, Recommender review, and auditability for project/resource inventory.
- Shared VPC host/service project separation, subnet sharing, network ownership, and service project onboarding.
- VPC Service Controls perimeters, Access Context Manager access levels, ingress/egress rules, and protected service boundaries.
- Billing account ownership, quota baselines, budget alerts, and account/project metadata governance.

### Operations

- Design an organization, folder, and project model for environments, business units, and shared services.
- Create a project factory flow with billing, labels, quota baselines, IAM, audit logging, and network attachment.
- Review IAM policies, service accounts, workload identity federation, and service account key risk.
- Validate Organization Policy constraints with dry-run before enforcing them.
- Investigate access issues with Policy Troubleshooter and IAM Recommender.
- Query Cloud Asset Inventory for project/resource ownership and landing-zone evidence.
- Validate VPC Service Controls perimeter design before data-sensitive workloads are migrated.
- Confirm landing-zone readiness by checking hierarchy, identity, billing, quota, audit, and shared-network prerequisites.

## Google Cloud Week 2: Networking, Private Connectivity, And Edge Traffic

### Service And Entity Coverage

- [Priority: Very High] Virtual Private Cloud (VPC): VPC networks, subnets, routes, firewall rules, hierarchical firewall policies, network tags, service accounts as firewall targets, VPC Flow Logs, Firewall Insights, and Connectivity Tests.
- [Priority: High] Shared VPC, VPC Network Peering, Private Service Access, and Private Service Connect: host projects, service projects, peering connections, allocated IP ranges, private connections, service attachments, endpoints, forwarding rules, and published services.
- [Priority: High] Cloud Router, Cloud NAT, Network Connectivity Center, Cloud VPN, and Cloud Interconnect: routers, BGP sessions, NAT gateways, spokes, hubs, VPN gateways, tunnels, VLAN attachments, Partner Interconnect, Dedicated Interconnect, and Cloud Interconnect locations.
- [Priority: Very High] Cloud DNS: managed zones, private zones, public zones, forwarding zones, peering zones, DNS records, policies, response policies, and DNSSEC settings.
- [Priority: Very High] Cloud Load Balancing: global external Application Load Balancers, regional external Application Load Balancers, internal Application Load Balancers, proxy Network Load Balancers, passthrough Network Load Balancers, backend services, URL maps, target proxies, forwarding rules, health checks, SSL certificates, and Cloud CDN-backed backends.
- [Priority: High] Cloud Armor, Cloud CDN, Cloud IDS, and Packet Mirroring: security policies, rules, preconfigured WAF rules, edge caches, cache keys, invalidations, IDS endpoints, mirrored sources, collectors, and packet mirroring policies.
- [Priority: High] API Gateway and Apigee: APIs, API configs, gateways, managed services, Apigee organizations, environments, environment groups, proxies, deployments, products, developers, apps, and Apigee hybrid runtimes.

### Controls

- VPC CIDR planning, subnet regionality, route priority, least-open firewall rules, hierarchical firewall policy inheritance, VPC Flow Logs, and Connectivity Tests.
- Shared VPC IAM boundaries, service project attachment, VPC peering route exchange, Private Service Access allocated ranges, Private Service Connect endpoints, and service attachment acceptance.
- Cloud Router BGP advertisements, Cloud NAT port allocation, Network Connectivity Center hub/spoke design, Cloud VPN tunnel settings, Interconnect VLAN attachment redundancy, and hybrid route governance.
- Cloud DNS private zone visibility, forwarding policies, peering zones, DNSSEC, response policies, and hybrid DNS boundaries.
- Load balancer health checks, backend service policies, SSL certificates, Cloud CDN caching, Cloud Armor policies, and WAF rule tuning.
- API Gateway authentication, service config rollout, Apigee proxy policy enforcement, environment promotion, and traffic management.

### Operations

- Validate packet paths with routes, firewall rules, VPC Flow Logs, Firewall Insights, and Connectivity Tests.
- Troubleshoot private access through VPC peering, Private Service Access, and Private Service Connect.
- Test Cloud VPN failover, Cloud Interconnect redundancy, Cloud Router BGP route exchange, and Cloud NAT exhaustion scenarios.
- Execute DNS cutover using Cloud DNS records, forwarding zones, private zones, and rollback TTL planning.
- Review load balancer health checks, backend capacity, URL maps, SSL certificates, Cloud CDN behavior, and Cloud Armor false positives.
- Deploy and troubleshoot API Gateway or Apigee proxies, authentication, routing, environment promotion, and access logs.

## Google Cloud Week 3: Compute, Google Kubernetes Engine, DevOps, And Platform Delivery

### Service And Entity Coverage

- [Priority: Very High] Compute Engine and managed instance groups: VM instances, machine types, disks, images, machine images, instance templates, managed instance groups (MIGs), autoscalers, health checks, metadata, startup scripts, service accounts, sole-tenant nodes, and reservations.
- [Priority: Very High] Google Kubernetes Engine (GKE): Standard clusters, Autopilot clusters, node pools, nodes, releases, control planes, workloads, services, Gateway API resources, Ingress, Config Sync, Workload Identity, Kubernetes service accounts, and GKE audit logs.
- [Priority: Very High] Cloud Run and Cloud Run functions: services, revisions, jobs, containers, triggers, functions, environment variables, traffic splits, domains, service accounts, concurrency, and min/max instances.
- [Priority: Medium] App Engine and Batch: applications, services, versions, instances, queues, jobs, tasks, allocation policies, instance policies, and logs.
- [Priority: High] Artifact Registry and Binary Authorization: repositories, packages, images, tags, versions, attachments, cleanup policies, vulnerability scanning, attestations, attestors, policies, and platform policies.
- [Priority: High] Cloud Build and Cloud Deploy: build triggers, build configs, steps, artifacts, worker pools, connections, repositories, delivery pipelines, targets, releases, rollouts, and Skaffold configs.
- [Priority: High] Infrastructure Manager and Terraform on Google Cloud: deployments, revisions, Terraform configurations, state, service accounts, input variables, outputs, and execution plans.
- [Priority: Medium] Cloud Scheduler, Eventarc, and Workflows: jobs, schedules, targets, triggers, channels, providers, workflows, executions, steps, connectors, and callbacks.
- [Priority: Medium] Vertex AI and Gemini API: Workbench instances, training jobs, custom jobs, models, endpoints, batch predictions, pipelines, Model Registry, Model Garden, feature stores, datasets, metadata, Gemini models, prompts, safety settings, and evaluation jobs.
- [Priority: Low] Cloud TPU and BigQuery ML: TPU nodes, queued resources, accelerator types, BigQuery ML models, training data, evaluation metrics, and predictions.

### Controls

- Compute Engine service accounts, OS Login, Shielded VM, metadata access, image baselines, instance template versioning, MIG autoscaling, and health checks.
- GKE release channels, authorized networks, private clusters, Workload Identity, Binary Authorization, network policy, Pod Security Admission, Config Sync, and maintenance windows.
- Cloud Run ingress, authentication, service accounts, concurrency, CPU allocation, min/max instances, VPC egress, and traffic splitting.
- Artifact Registry IAM, cleanup policies, vulnerability scanning, Binary Authorization attestations, build provenance, and signed image promotion.
- Cloud Build private pools, repository connections, substitutions, service accounts, secret usage, approval gates, and Cloud Deploy target promotion.
- Infrastructure Manager deployment service accounts, Terraform state handling, plan review, policy validation, and rollback approach.
- Vertex AI execution roles, VPC Service Controls, private service access, model approval, endpoint deployment controls, and data access boundaries.

### Operations

- Build golden VM images, roll out managed instance group updates, test autoscaling, and roll back instance templates.
- Upgrade GKE control planes/node pools, review workload rollout health, troubleshoot image pulls, and validate Workload Identity.
- Deploy Cloud Run services/functions, shift traffic between revisions, tune concurrency, and troubleshoot cold starts or failed revisions.
- Promote container images through Artifact Registry with vulnerability scan review and Binary Authorization enforcement.
- Run Cloud Build pipelines, promote releases with Cloud Deploy, validate deployment targets, and roll back failed rollouts.
- Review Terraform plans with Infrastructure Manager and reconcile drift from infrastructure as code.
- Run Vertex AI training, model registration, endpoint deployment, pipeline execution, and inference troubleshooting.

## Google Cloud Week 4: Data, Storage, Backup, And Stateful Reliability

### Service And Entity Coverage

- [Priority: Very High] Cloud Storage: buckets, objects, generations, storage classes, object versioning, retention policies, lifecycle rules, soft delete, bucket lock, hierarchical namespace, and managed folders.
- [Priority: High] Persistent Disk, Hyperdisk, and Filestore: persistent disks, snapshots, images, regional disks, Hyperdisk volumes, Filestore instances, shares, backups, and replication.
- [Priority: Very High] Cloud SQL and AlloyDB for PostgreSQL: instances, databases, users, flags, backups, point-in-time recovery (PITR), read replicas, high availability, AlloyDB clusters, primary instances, read pool instances, and automated backups.
- [Priority: Very High] Spanner, Bigtable, and Firestore: Spanner instances, databases, tables, splits, backups, change streams, Bigtable instances, clusters, tables, column families, app profiles, Firestore databases, collections, documents, indexes, and TTL policies.
- [Priority: High] Memorystore: Memorystore for Redis instances, Memorystore for Memcached instances, clusters, nodes, replicas, endpoints, persistence settings, and maintenance windows.
- [Priority: Very High] Pub/Sub: topics, subscriptions, messages, schemas, snapshots, dead-letter topics, retry policies, filters, ordering keys, and seek operations.
- [Priority: Very High] BigQuery: datasets, tables, views, materialized views, partitions, clusters, jobs, reservations, slots, routines, connections, and BigQuery ML models.
- [Priority: High] Dataflow, Dataproc, and Cloud Composer: Dataflow jobs, templates, workers, pipelines, Dataproc clusters, jobs, workflows, Cloud Composer environments, DAGs, and schedulers.
- [Priority: High] Dataplex, Data Catalog, Cloud Data Fusion, and Sensitive Data Protection: lakes, zones, assets, entries, tags, lineage, pipelines, Wrangler transformations, inspection templates, de-identification templates, and findings.
- [Priority: Medium] Document AI, Vision AI, Speech-to-Text, and Cloud Translation: processors, processor versions, datasets, evaluations, models, glossaries, adaptive MT datasets, and long-running operations.
- [Priority: High] Secret Manager and Cloud Key Management Service (Cloud KMS): secrets, secret versions, replication policies, rotation schedules, key rings, keys, key versions, protection levels, import jobs, and external key managers.

### Controls

- Cloud Storage uniform bucket-level access, public access prevention, object versioning, retention policies, lifecycle rules, bucket lock, soft delete, and customer-managed encryption keys (CMEK).
- Disk snapshot policies, regional disk placement, Filestore backup policies, encryption, private access, and restore controls.
- Cloud SQL/AlloyDB private IP, authorized networks, IAM database authentication, backup retention, point-in-time recovery, maintenance windows, flags, and high availability.
- Spanner leader/region configuration, backup schedules, change streams, Bigtable app profiles, Firestore security rules, index governance, TTL, and backups.
- Pub/Sub IAM, schema validation, dead-letter topics, retry policies, message retention, ordering keys, and subscription filters.
- BigQuery dataset/table IAM, row-level security, column-level security, policy tags, authorized views, reservations, partitioning, clustering, and query cost controls.
- Dataflow service accounts, worker networking, streaming engine, templates, Dataproc cluster IAM, Composer environment access, and DAG permissions.
- Dataplex governance, Data Catalog tags, lineage capture, Sensitive Data Protection inspection templates, de-identification templates, and VPC Service Controls.
- Secret Manager replication, rotation, IAM, Cloud KMS key rotation, key version state, imported keys, and CMEK usage.

### Operations

- Restore Cloud Storage object versions, validate lifecycle/retention, review public access, and test replication dependencies.
- Restore disks and Filestore backups, validate mount access, and review throughput/capacity.
- Test Cloud SQL/AlloyDB failover, point-in-time recovery, read replica promotion, connection limits, and maintenance changes.
- Troubleshoot Spanner latency, Bigtable hot tablets, Firestore indexes, and multi-region consistency behavior.
- Monitor Pub/Sub backlog, dead-letter topics, retry behavior, ordering keys, and subscriber lag.
- Tune BigQuery partitioning/clustering, reservations, query cost, materialized views, and BigQuery ML jobs.
- Operate Dataflow streaming pipelines, Dataproc jobs, Composer DAGs, and Cloud Data Fusion pipelines.
- Classify sensitive data with Sensitive Data Protection and validate governed data access through Dataplex/Data Catalog.
- Rotate secrets, review Cloud KMS key usage, validate CMEK permissions, and troubleshoot encryption access failures.

## Google Cloud Week 5: Observability, FinOps, And Operational Excellence

### Service And Entity Coverage

- [Priority: Very High] Cloud Monitoring, Cloud Logging, Error Reporting, Cloud Trace, and Cloud Profiler: metrics, monitored resources, alerting policies, uptime checks, notification channels, dashboards, log buckets, log sinks, log-based metrics, error groups, traces, spans, profiles, and service graphs.
- [Priority: High] Managed Service for Prometheus and Google Cloud Managed Service for Grafana: collectors, rules, metrics scopes, workspaces, dashboards, data sources, alerting, and service accounts.
- [Priority: High] Network Intelligence Center and Personalized Service Health: Connectivity Tests, Performance Dashboard, Firewall Insights, Network Analyzer insights, service health events, and impacted resources.
- [Priority: High] Cloud Billing, Billing export to BigQuery, budgets, Recommender, and Active Assist: billing accounts, budgets, alerts, exported billing tables, recommendations, insights, commitments, and utilization metrics.
- [Priority: High] Security Command Center, Event Threat Detection, Policy Intelligence, and Google Security Operations: findings, sources, assets, attack paths, detectors, rules, IAM analysis, cases, alerts, parsers, feeds, and detections.
- [Priority: Medium] Vertex AI Model Monitoring, Vertex AI Experiments, Vertex ML Metadata, and Vertex AI model evaluation: monitoring jobs, baselines, skew/drift metrics, experiments, runs, metadata artifacts, executions, contexts, evaluation datasets, and evaluation metrics.

### Controls

- Cloud Monitoring alert policies, notification channels, SLOs, uptime checks, log retention, log sinks, log-based metrics, metrics scopes, and dashboard permissions.
- Managed Service for Prometheus rule evaluation, collection configuration, metric scope design, Grafana data source permissions, and dashboard sharing.
- Network Intelligence Center connectivity tests, firewall insights, analyzer recommendations, service health event routing, and incident notification paths.
- Billing export, budget thresholds, cost labels, Recommender permissions, commitment utilization, and anomaly review.
- Security Command Center organization activation, finding sources, Event Threat Detection rules, attack path visibility, Policy Intelligence access analysis, and Google Security Operations ingestion.
- Vertex AI model monitoring thresholds, experiment metadata, ML metadata lineage, evaluation metrics, and model governance evidence.

### Operations

- Build service dashboards, alert policies, log-based metrics, uptime checks, traces, profiles, and incident timelines.
- Tune Prometheus alerts, metric cardinality, Grafana dashboards, and cross-project observability.
- Use Connectivity Tests, Firewall Insights, Network Analyzer, and Personalized Service Health during network incidents.
- Review billing export, budget alerts, Recommender savings, commitment utilization, and right-sizing actions.
- Triage Security Command Center findings, Event Threat Detection alerts, IAM analysis results, and Google Security Operations cases.
- Review Vertex AI model drift, skew, experiments, metadata lineage, and model evaluation results.

## Google Cloud Week 6: DR, Resilience, And Migration Execution

### Service And Entity Coverage

- [Priority: Very High] Backup and DR Service: backup/recovery appliances, backup plans, backup vaults, protected resources, recovery points, backup/recovery policies, and restore jobs.
- [Priority: Very High] Migrate to Virtual Machines and Migrate to Containers: source VMs, target projects, replication cycles, test clones, cutover jobs, migration waves, migration plans, fit assessments, artifacts, and generated deployment files.
- [Priority: Very High] Database Migration Service: connection profiles, migration jobs, conversion workspaces, source databases, destination databases, change data capture (CDC), and migration phases.
- [Priority: High] Migration Center: discovery clients, assets, groups, preference sets, cost estimates, fit assessments, and migration plans.
- [Priority: High] Storage Transfer Service and Transfer Appliance: transfer jobs, sources, sinks, agents, schedules, manifests, appliances, jobs, and transfer logs.
- [Priority: Medium] Disaster Recovery Patterns: runbooks, recovery tiers, dependency maps, restore order, failover steps, failback steps, and game day evidence.

### DR Dependencies From Earlier Weeks

- Cloud DNS, Cloud Load Balancing, Cloud VPN, and Cloud Interconnect are covered in Week 2.
- Cloud Storage replication, Filestore backups, Cloud SQL high availability, Spanner multi-region configurations, Bigtable replication, and Pub/Sub retry/dead-letter patterns are covered in Week 4.
- Cloud Monitoring, Cloud Logging, Personalized Service Health, Security Command Center, and Cloud KMS are covered in earlier weeks.

### Controls

- Backup and DR backup plan scope, backup vault permissions, immutability posture, restore permissions, and recovery point retention.
- Migrate to Virtual Machines replication settings, test clone configuration, cutover settings, target project mapping, service account permissions, and rollback planning.
- Migrate to Containers fit assessment, generated artifacts, cluster target configuration, and deployment validation.
- Database Migration Service connection profiles, private connectivity, full-load settings, change data capture (CDC), validation, and cutover controls.
- Migration Center discovery scope, asset grouping, preference sets, dependency mapping, and wave planning.
- Storage Transfer Service schedule, source/sink permissions, agent configuration, transfer validation, and Transfer Appliance chain-of-custody controls.

### Operations

- Run backup restore tests, validate recovery points, and collect recovery evidence.
- Execute VM migration test clones, cutover jobs, rollback planning, and post-cutover validation.
- Assess container migration fit, review generated Kubernetes artifacts, and validate target clusters.
- Run database migration full load plus CDC, validate data, monitor replication lag, and rehearse cutover/rollback.
- Use Migration Center for inventory, grouping, wave planning, and migration reporting.
- Transfer file/object datasets with Storage Transfer Service or Transfer Appliance and validate checksums/cutover sync.
- Run DR game days with failover, failback, monitoring, security access, and dependency validation.