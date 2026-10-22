Do the deep analysis of each point of gcp-week-wise-cloud-coverage.md file, must consider each coma separated value as a point, if `and` is used between two services or entity then consider as separate point and give technical definition independently give only technical definition of each point, wherever possible, give one concrete production example. it must be technical study focused, Overall score of technical notes must be closer to 10/10. output must be in downloadable md file.



# AWS Week-Wise Cloud Coverage

Use this as an AWS-focused study guide for AWS Cloud Platform, AWS Solutions Architect, SRE, DevOps, Cloud, Security, disaster recovery (DR), and migration interviews.

## AWS Week 1: Landing Zone, Account Strategy, And Governance

### Service And Entity Coverage

- [Priority: Very High] AWS Organizations and AWS Account Management: organization, root, management account, member accounts, organizational units (OUs), delegated administrators, service control policies (SCPs), resource control policies (RCPs), tag policies, backup policies, AI services opt-out policies, AWS accounts, primary contacts, alternate contacts, AWS Regions, and Region opt-in status.
- [Priority: High] AWS Control Tower: landing zone, Account Factory, governed accounts, registered organizational units (OUs), preventive controls, detective controls, proactive controls, and drift status.
- [Priority: Very High] AWS Identity and Access Management (IAM): IAM users, IAM groups, IAM roles, IAM policies, instance profiles, access keys, MFA devices, permissions boundaries, role trust policies, analyzers, findings, archive rules, access previews, and policy validation checks.
- [Priority: High] AWS IAM Identity Center: identity sources, users, groups, permission sets, account assignments, and application assignments.
- [Priority: Medium] AWS Service Catalog and AWS Resource Access Manager: portfolios, products, provisioning artifacts, provisioned products, constraints, launch paths, portfolio access, resource shares (AWS RAM shares), principals, resources, managed permissions, customer managed permissions, and invitations.
- [Priority: High] AWS CloudFormation StackSets: stack sets (StackSets), stack instances, stack operations, administrator accounts, execution roles, target accounts, target organizational units (OUs), and operation preferences.
- [Priority: Very High] AWS CloudTrail and AWS Config: trails, organization trails, event data stores, management events, data events, Insights events, configuration recorders, delivery channels, configuration items, rules, conformance packs, configuration aggregators, and remediation actions.
- [Priority: High] AWS Security Hub and AWS Audit Manager: security standards, controls, findings, insights, automation rules, finding aggregators, administrator accounts, member accounts, assessments, frameworks, control sets, evidence folders, evidence, Evidence Finder, and assessment reports.
- [Priority: Medium] AWS Artifact, AWS Health, and AWS Service Quotas: reports, agreements, notifications, AWS Health events, affected entities, event status codes, service quotas, applied quota values, quota increase requests, quota request templates, and usage metrics.

### Controls

- AWS Organizations policy inheritance, trusted access, delegated administration, consolidated billing, account contact ownership, Region opt-in/opt-out, and account metadata governance.
- AWS Control Tower account baselines, control enablement, drift detection, landing-zone versioning, and control remediation.
- IAM identity-based policies, resource-based policies, permissions boundaries, role assumption, multi-factor authentication (MFA), attribute-based access control (ABAC), AWS Security Token Service (AWS STS), and IAM Access Analyzer findings.
- AWS IAM Identity Center workforce identity federation, permission-set provisioning, session duration, and account assignment review.
- AWS Service Catalog launch constraints, template constraints, tag update constraints, portfolio sharing, launch permissions, and AWS Resource Access Manager resource-share permissions.
- AWS CloudFormation StackSets service-managed permissions, self-managed permissions, automatic deployments, account filters, Region targeting, failure tolerance, and concurrency.
- AWS CloudTrail multi-Region trails, organization-wide logging, log file integrity validation, encryption, retention, and AWS Config organization aggregation.
- AWS Security Hub delegated administration, cross-Region aggregation, AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark, and AWS Audit Manager assessment scoping.
- AWS Artifact report access, agreement acceptance, AWS Health organizational view, Service Quotas baselines, utilization monitoring, and quota-risk review.

### Operations

- Create a landing-zone account model with management account, shared services accounts, workload accounts, and environment-specific OUs.
- Design an SCP model for preventive guardrails without blocking required platform operations.
- Enroll accounts and OUs into AWS Control Tower and review drift remediation paths.
- Review IAM role trust policies, access keys, permissions boundaries, and unused access findings.
- Provision AWS IAM Identity Center permission sets and validate account assignments.
- Use AWS CloudFormation StackSets for multi-account baseline deployment and rollback.
- Investigate API activity with AWS CloudTrail and validate drift/compliance with AWS Config.
- Triage AWS Security Hub findings and prepare AWS Audit Manager evidence for compliance review.
- Validate landing-zone readiness before migration by checking identity, audit, account ownership, quotas, and health events.

## AWS Week 2: Networking, Private Connectivity, And Edge Traffic

### Service And Entity Coverage

- [Priority: Very High] Amazon Virtual Private Cloud (Amazon VPC): VPCs, subnets, route tables, internet gateways, egress-only internet gateways, NAT gateways, elastic IP addresses, elastic network interfaces, prefix lists, DHCP option sets, security groups, network ACLs, VPC Flow Logs, Amazon VPC Reachability Analyzer, Amazon VPC Network Access Analyzer, and Amazon VPC IP Address Manager (IPAM).
- [Priority: High] AWS Transit Gateway and AWS Cloud WAN: transit gateways, transit gateway attachments, transit gateway route tables, route table associations, route table propagations, peering attachments, global networks, core networks, core network policies, segments, attachments, and network function groups.
- [Priority: High] Amazon VPC Peering, Amazon VPC Endpoints, and AWS PrivateLink: VPC peering connections, requester VPCs, accepter VPCs, interface endpoints, gateway endpoints, Gateway Load Balancer endpoints, endpoint services, endpoint network interfaces, endpoint policies, and private DNS names.
- [Priority: Medium] Amazon VPC Lattice and AWS Verified Access: service networks, services, target groups, listeners, rules, auth policies, resource configurations, Verified Access instances, Verified Access groups, Verified Access endpoints, trust providers, access policies, and application domains.
- [Priority: High] AWS Network Firewall and AWS Firewall Manager: firewalls, firewall policies, rule groups, stateless rules, stateful rules, TLS inspection configurations, alert logs, flow logs, Firewall Manager policies, policy scopes, resource sets, and compliance status.
- [Priority: Very High] Elastic Load Balancing and AWS Certificate Manager: Application Load Balancers, Network Load Balancers, Gateway Load Balancers, target groups, listeners, listener rules, health checks, load balancer attributes, public certificates, private certificates, imported certificates, domain names, subject alternative names (SANs), and validation options.
- [Priority: High] AWS Site-to-Site VPN, AWS Client VPN, and AWS Direct Connect: customer gateways, virtual private gateways, VPN connections, VPN tunnels, tunnel options, customer gateway devices, Client VPN endpoints, target network associations, authorization rules, Direct Connect connections, link aggregation groups (LAGs), private virtual interfaces, public virtual interfaces, transit virtual interfaces, Direct Connect gateways, and hosted connections.
- [Priority: Very High] Amazon Route 53 and Amazon Route 53 Resolver: hosted zones, records, routing policies, health checks, reusable delegation sets, domains, key-signing keys (KSKs), inbound Resolver endpoints, outbound Resolver endpoints, Resolver rules, rule associations, query logging configurations, DNS Firewall rule groups, and domain lists.
- [Priority: High] Amazon CloudFront, AWS WAF, AWS Shield, and AWS Global Accelerator: distributions, origins, cache behaviors, cache policies, origin request policies, response headers policies, invalidations, key groups, origin access controls, web ACLs, rules, rule groups, managed rule groups, IP sets, labels, protections, protection groups, accelerators, listeners, endpoint groups, endpoints, static anycast IP addresses, endpoint weights, and traffic dials.
- [Priority: High] Amazon API Gateway: REST APIs, HTTP APIs, WebSocket APIs, resources, routes, methods, integrations, stages, deployments, authorizers, usage plans, API keys, custom domain names, and VPC links.

### Controls

- VPC CIDR allocation, subnet routing, route table associations, security group rules, network ACL rules, flow log delivery, IPAM pool rules, and overlapping CIDR detection.
- Transit Gateway route domains, attachment acceptance, route propagation, appliance mode, AWS Resource Access Manager sharing, Cloud WAN segment isolation, attachment policies, and route sharing.
- VPC peering non-overlapping CIDRs, endpoint policies, private DNS, allowed principals, acceptance settings, and private service exposure boundaries.
- VPC Lattice service network associations, routing rules, TLS settings, target group health checks, Verified Access identity trust, device trust, policy-based access, and access log delivery.
- AWS Network Firewall centralized inspection, Suricata-compatible rules, domain list rules, default actions, AWS Firewall Manager policies, and security group policies.
- Elastic Load Balancing TLS security policies, certificates, target group attributes, cross-zone load balancing, deletion protection, access logs, and AWS Certificate Manager managed renewal.
- VPN IKE/IPsec parameters, BGP routing, tunnel inside CIDRs, pre-shared keys, Client VPN authorization rules, Direct Connect BGP authentication, allowed prefixes, MACsec, and resiliency model.
- Route 53 TTLs, routing policies, health-check routing, DNSSEC signing, delegation, Route 53 Resolver conditional forwarding, DNS Firewall rules, and query logging.
- CloudFront viewer protocol policies, signed URLs, signed cookies, origin access control, AWS WAF rate-based rules, Bot Control, AWS Shield Advanced, Global Accelerator traffic dials, endpoint weights, and health checks.
- API Gateway authorizers, resource policies, throttling, mutual TLS, stage variables, request validation, and AWS WAF associations.

### Operations

- Validate VPC packet paths with route tables, security groups, network ACLs, Reachability Analyzer, and Network Access Analyzer.
- Review NAT gateway saturation, IPAM allocation, route drift, and overlapping CIDRs before migration.
- Onboard Transit Gateway attachments, validate route propagation, and test segmentation between network domains.
- Publish or consume private services through VPC endpoints and AWS PrivateLink without internet exposure.
- Investigate dropped flows in AWS Network Firewall and tune rule groups safely.
- Review Elastic Load Balancing target health, listener rules, TLS certificates, and traffic shifting.
- Test Site-to-Site VPN tunnel failover and Direct Connect backup routing.
- Execute DNS cutover with Route 53 records, health checks, Resolver rules, and rollback TTL planning.
- Tune CloudFront cache behavior, origin failover, AWS WAF false positives, and Global Accelerator failover.
- Deploy and troubleshoot API Gateway stages, private integrations, throttling, access logs, and canary releases.

## AWS Week 3: Compute, Kubernetes, CI/CD, And Platform Delivery

### Service And Entity Coverage

- [Priority: Very High] Amazon Elastic Compute Cloud (Amazon EC2) and Amazon EC2 Auto Scaling: instances, Amazon Machine Images (AMIs), instance types, key pairs, elastic network interfaces, instance profiles, user data, Elastic IP addresses, EC2 Image Builder image pipelines, Auto Scaling groups, launch templates, scaling policies, scheduled actions, lifecycle hooks, warm pools, instance refreshes, and health checks.
- [Priority: High] Amazon Elastic Container Registry (Amazon ECR): private repositories, public repositories, images, tags, manifests, lifecycle policies, pull through cache rules, and replication configurations.
- [Priority: Very High] Amazon Elastic Container Service (Amazon ECS) and AWS Fargate: clusters, services, task definitions, tasks, capacity providers, container instances, service deployments, task sets, Amazon ECS tasks on AWS Fargate, and Amazon ECS services on AWS Fargate.
- [Priority: Very High] Amazon Elastic Kubernetes Service (Amazon EKS) and Karpenter: clusters, control planes, managed node groups, self-managed nodes, Fargate profiles, Amazon EKS pods on AWS Fargate, add-ons, access entries, Kubernetes service accounts, pod identity associations, Karpenter NodePool resources, EC2NodeClass resources, and NodeClaim resources.
- [Priority: Very High] AWS Lambda: functions, versions, aliases, layers, event source mappings, function URLs, execution roles, provisioned concurrency, and reserved concurrency.
- [Priority: Medium] AWS App Runner, AWS Elastic Beanstalk, and Amazon Lightsail: App Runner services, source configurations, VPC connectors, custom domains, Elastic Beanstalk applications, application versions, environments, platforms, deployment policies, Lightsail instances, blueprints, bundles, static IPs, snapshots, and container services.
- [Priority: Low] AWS Batch: jobs, job queues, job definitions, compute environments, scheduling policies, array jobs, multi-node parallel jobs, and fair share identifiers.
- [Priority: High] AWS CodeBuild, AWS CodePipeline, AWS CodeDeploy, and AWS CodeArtifact: build projects, buildspec files, reports, caches, pipelines, stages, actions, source actions, AWS CodeConnections connections (CodeStar connections), applications, deployment groups, deployment configurations, AppSpec files, domains, repositories, packages, package versions, and external connections.
- [Priority: High] AWS CloudFormation, AWS Cloud Development Kit (AWS CDK), and AWS Proton: stacks, stack sets, templates, change sets, nested stacks, AWS SAM templates, AWS::Serverless resources (serverless resources), CDK apps, constructs, assets, synthesized CloudFormation templates, Proton environment templates, service templates, environments, services, service instances, and components.
- [Priority: Medium] Amazon SageMaker AI (Amazon SageMaker): domains (Studio domains), user profiles, spaces, training jobs, processing jobs, feature groups, data flows, models, endpoint configurations, endpoints, pipelines, model package groups, model packages, monitoring schedules, baselines, constraints, experiments, trials, trial components, lineage artifacts, lineage actions, and lineage contexts.
- [Priority: Medium] Amazon Bedrock: foundation models, inference profiles, provisioned throughput, agents, knowledge bases, guardrails, model customization jobs, model evaluation jobs, and invocation logs.

### Controls

- EC2 instance metadata options, IAM instance profiles, security group rules, termination protection, Auto Scaling desired/min/max capacity, health check type, termination policies, mixed instances policy, and instance refresh preferences.
- Amazon ECR repository policies, image scanning, tag immutability, lifecycle rules, encryption, and cross-Region replication.
- Amazon ECS task IAM roles, execution roles, deployment configuration, service discovery, load balancer integration, capacity provider strategy, and Fargate task sizing.
- Amazon EKS endpoint access, cluster access management, managed add-ons, IAM roles for service accounts (IRSA), EKS Pod Identity, security groups for pods, upgrade policies, Karpenter disruption controls, and Fargate profile placement.
- AWS Lambda execution roles, resource-based policies, VPC configuration, environment variables, concurrency limits, code signing, and dead-letter queues.
- App Runner deployment settings, Elastic Beanstalk platform versions, Lightsail firewall rules, health checks, VPC egress, access roles, instance profiles, and snapshots.
- AWS Batch compute environment type, allocation strategy, job priority, retry strategy, timeout settings, and IAM roles.
- Developer tool service roles, environment images, VPC configuration, artifact encryption, approval actions, stage transitions, cross-account actions, automatic rollback, alarms, repository policies, and package origin controls.
- CloudFormation stack policies, rollback triggers, termination protection, capabilities, hooks, service roles, AWS SAM deployment preferences, CDK bootstrap stacks, synthesizers, permissions boundaries, Proton template versions, and deployment workflows.
- SageMaker execution roles, network isolation, VPC configuration, model approval status, endpoint deployment configuration, data capture, bias metrics, lineage metadata, artifact encryption, and access permissions.
- Bedrock model access, guardrails, IAM permissions, VPC endpoints, encryption, and invocation logging.

### Operations

- Build and roll back golden AMIs, review EC2 status checks, replace unhealthy instances, and validate Auto Scaling instance refreshes.
- Push and promote container images in Amazon ECR while reviewing image scan findings and lifecycle cleanup.
- Deploy ECS services with rolling or blue/green patterns and troubleshoot failed tasks, capacity providers, and target registration.
- Upgrade EKS clusters, managed node groups, add-ons, and review pod scheduling, IRSA, EKS Pod Identity, and Karpenter scale-out behavior.
- Deploy Lambda versions and aliases, investigate cold starts, throttling, retries, and event source mapping failures.
- Use CodeBuild, CodePipeline, CodeDeploy, and CodeArtifact for build, approval, deployment, rollback, and artifact promotion workflows.
- Review CloudFormation change sets, drift, stack rollback, CDK diffs, SAM serverless deployment, and Proton template rollout.
- Run SageMaker training, processing, pipelines, model registration, endpoint deployment, monitoring review, and inference troubleshooting.
- Invoke Bedrock models, test prompts, validate guardrails, synchronize knowledge bases, deploy agents, and review model evaluations.

## AWS Week 4: Data, Storage, Backup, And Stateful Reliability

### Service And Entity Coverage

- [Priority: Very High] Amazon Simple Storage Service (Amazon S3): buckets, objects, prefixes, access points, Multi-Region Access Points, bucket policies, S3 Block Public Access settings, S3 Object Lock configurations, lifecycle configurations, replication rules, and storage classes.
- [Priority: High] Amazon Elastic Block Store (Amazon EBS) and Amazon Elastic File System (Amazon EFS): EBS volumes, snapshots, snapshot archives, volume types, provisioned IOPS, throughput settings, Fast Snapshot Restore, EFS file systems, mount targets, access points, lifecycle policies, throughput modes, performance modes, backups, and replication configurations.
- [Priority: Medium] Amazon FSx: file systems, backups, storage virtual machines, volumes, shares, endpoints, deployment types, and throughput capacity.
- [Priority: Very High] Amazon Relational Database Service (Amazon RDS) and Amazon Aurora: DB instances, DB clusters, DB snapshots, automated backups, parameter groups, option groups, subnet groups, read replicas, reserved DB instances, Aurora Replicas, cluster endpoints, reader endpoints, custom endpoints, global databases, and backtrack windows.
- [Priority: Very High] Amazon DynamoDB: tables, items, attributes, partition keys, sort keys, local secondary indexes, global secondary indexes, streams, global tables, backups, and time to live (TTL).
- [Priority: Medium] Amazon DocumentDB, Amazon Neptune, Amazon Timestream for LiveAnalytics, Amazon ElastiCache, and Amazon MemoryDB: DocumentDB clusters, Neptune DB clusters, Timestream databases and tables, ElastiCache serverless caches, replication groups, shards, users, user groups, MemoryDB clusters, access control lists (ACLs), snapshots, and endpoints.
- [Priority: High] Amazon OpenSearch Service, Amazon Redshift, and Amazon Athena: OpenSearch domains, indexes, data nodes, cluster manager nodes, OpenSearch Dashboards, Redshift provisioned clusters, serverless namespaces, serverless workgroups, databases, schemas, tables, materialized views, Athena workgroups, queries, data catalogs, partitions, and query result locations.
- [Priority: High] AWS Glue and AWS Lake Formation: Data Catalog databases, Data Catalog tables, crawlers, jobs, triggers, workflows, connections, schemas, classifiers, data quality rulesets, AWS Glue DataBrew projects, datasets, recipes, profile jobs, Lake Formation data lake administrators, registered locations, LF-Tags, permissions, data filters, resource links, and blueprints.
- [Priority: High] Amazon Kinesis Data Streams, Amazon Data Firehose, Amazon Managed Streaming for Apache Kafka (Amazon MSK), Amazon Simple Queue Service (Amazon SQS), and Amazon Simple Notification Service (Amazon SNS): Kinesis data streams, shards, consumers, enhanced fan-out consumers, Firehose streams, sources, destinations, MSK clusters, brokers, topics, partitions, consumer groups, connectors, SQS standard queues, FIFO queues, dead-letter queues, SNS topics, subscriptions, and filter policies.
- [Priority: High] Amazon EventBridge and AWS Step Functions: event buses, rules, targets, event patterns, archives, replays, schedules, connections, API destinations, state machines, states, executions, activities, tasks, versions, aliases, distributed maps, and execution history.
- [Priority: High] AWS Secrets Manager and AWS Key Management Service (AWS KMS): secrets, secret versions, version stages, rotation schedules, rotation Lambda functions, resource policies, replication settings, KMS keys, aliases, key policies, grants, key material, multi-Region keys, imported key material, and external key stores.
- [Priority: High] AWS Backup and AWS Database Migration Service (AWS DMS): backup plans, backup rules, backup selections, recovery points, backup vaults, backup vault lock configurations, copy jobs, restore jobs, replication instances, replication tasks, endpoints, replication subnet groups, event subscriptions, data providers, and migration projects.

### Controls

- S3 bucket policies, Object Ownership, versioning, default encryption, S3 Object Lock retention modes, lifecycle rules, replication, and access logging.
- EBS volume encryption, snapshot sharing, delete-on-termination settings, KMS key selection, EFS file system policies, security groups, access point POSIX identities, lifecycle management, and backup policies.
- RDS and Aurora Multi-AZ deployment, backup retention, deletion protection, storage autoscaling, encryption, IAM database authentication, maintenance windows, failover priority, and parameter alignment.
- DynamoDB capacity modes, auto scaling, table classes, encryption, point-in-time recovery (PITR), resource-based policies, and deletion protection.
- Specialized database VPC placement, TLS, encryption, backup retention, audit logging, retention configuration, Redis-compatible access control lists, Multi-AZ deployment, and maintenance windows.
- OpenSearch fine-grained access control, VPC access, encryption, index state management, Redshift workload management (WLM), data sharing, Athena workgroup settings, bytes scanned limits, and result location enforcement.
- Glue job bookmarks, worker type, security configurations, IAM roles, Data Catalog permissions, Lake Formation permissions, LF-Tag-based access control, row-level filters, column-level filters, and cross-account sharing.
- Streaming and messaging retention, encryption, shard count, Firehose buffering, broker storage, Apache Kafka ACLs, queue policies, visibility timeout, redrive policies, topic policies, subscription filter policies, and delivery retry policies.
- EventBridge event bus policies, input transformers, retry policies, dead-letter queues, schema discovery, Step Functions retry/catch handlers, timeout settings, IAM roles, logging, tracing, and encryption.
- Secrets Manager automatic rotation, version staging labels, resource policies, KMS key policies, grants, rotation, deletion waiting periods, and multi-Region replication.
- AWS Backup schedules, lifecycle rules, vault policies, AWS Backup Vault Lock, cross-Region copy, cross-account copy, legal holds, DMS full-load settings, change data capture (CDC), table mappings, validation settings, encryption, and endpoint permissions.

### Operations

- Restore S3 objects, validate lifecycle policies, review replication, check public access, and optimize storage classes.
- Attach and resize EBS volumes, restore snapshots, validate EFS mounts, review throughput, and test file-system recovery.
- Validate RDS/Aurora failover, point-in-time recovery (PITR), replica promotion, global database switchover, engine upgrades, and replica lag.
- Troubleshoot DynamoDB hot partitions, throttling, secondary indexes, backup restore, streams, and global table replication.
- Review cache hit ratios, Redis-compatible access, snapshot restore, eviction behavior, and specialized database migration fit.
- Tune OpenSearch shards, Redshift queries, Athena partitions, and data lake query failures.
- Run Glue crawlers and ETL jobs, validate schemas, review data quality, grant Lake Formation permissions, and troubleshoot governed data sharing.
- Monitor stream ingestion, shard scaling, consumer lag, MSK connectors, SQS dead-letter queue redrive, and SNS fanout.
- Validate EventBridge routing and replay, then troubleshoot failed Step Functions executions and orchestration rollback.
- Rotate secrets, roll back secret versions, review KMS grants, validate cross-account key access, test backups, restore recovery points, and execute DMS full-load plus CDC cutover.

## AWS Week 5: Observability, FinOps, And Operational Excellence

### Service And Entity Coverage

- [Priority: Very High] Amazon CloudWatch and AWS X-Ray: metrics, namespaces, dimensions, alarms, dashboards, log groups, log streams, metric filters, Contributor Insights rules, synthetic canaries, Application Signals services, Container Insights clusters, Logs Insights queries, traces, segments, subsegments, service maps, groups, sampling rules, annotations, and metadata.
- [Priority: High] AWS Distro for OpenTelemetry (ADOT), Amazon Managed Service for Prometheus, and Amazon Managed Grafana: ADOT collectors, pipelines, receivers, processors, exporters, Prometheus workspaces, rule groups namespaces, scrapers, metrics, labels, Grafana workspaces, data sources, dashboards, folders, users, teams, and service accounts.
- [Priority: High] AWS Systems Manager: managed nodes, documents, Run Command, Session Manager sessions, State Manager associations, Patch Manager baselines, Automation runbooks, Parameter Store parameters, and OpsItems.
- [Priority: Medium] AWS Well-Architected Tool and AWS Resilience Hub: workloads, lenses, milestones, improvement plans, review templates, profiles, workload shares, applications, app components, resilience policies, assessments, resiliency scores, recommendations, and standard operating procedures (SOPs).
- [Priority: High] AWS Cost Explorer, AWS Budgets, AWS Cost Categories, AWS Cost and Usage Reports (AWS CUR), AWS Cost Anomaly Detection, AWS Compute Optimizer, and AWS Trusted Advisor: cost and usage data, dimensions, filters, forecasts, budgets, budget actions, alerts, cost category rules, CUR report definitions, anomaly monitors, anomalies, recommendations, and checks.
- [Priority: High] Amazon GuardDuty, Amazon Inspector, Amazon Macie, Amazon Detective, and Amazon Security Lake: detectors, findings, threat intelligence sets, malware protection plans, coverage, software bills of materials (SBOMs), classification jobs, sensitive data discovery results, managed data identifiers, behavior graphs, investigations, data lakes, sources, subscribers, and Open Cybersecurity Schema Framework (OCSF) event classes.
- [Priority: Medium] Amazon SageMaker AI and Amazon Bedrock: SageMaker monitoring schedules, baselines, constraints, processing jobs, bias reports, explainability reports, experiments, trials, lineage artifacts, lineage actions, Bedrock model evaluation jobs, evaluation datasets, guardrails, guardrail versions, foundation models, agents, and invocation logs.

### Controls

- CloudWatch alarm actions, metric math, composite alarms, log retention, data protection policies, cross-account observability, dashboards, X-Ray sampling rules, encryption, trace retention, and SDK configuration.
- ADOT telemetry routing, sampling, exporter configuration, Prometheus workspace policies, ingestion limits, alerting rules, Grafana authentication providers, data source permissions, workspace roles, and plugin management.
- Systems Manager instance profiles, session logging, patch baselines, maintenance windows, document permissions, and parameter policies.
- Well-Architected lens selection, workload sharing, Resilience Hub recovery time objective/recovery point objective (RTO/RPO) targets, policy mapping, application import sources, and assessment scope.
- Cost filters, grouping dimensions, budget thresholds, budget actions, split charge allocation, CUR delivery options, anomaly alert thresholds, recommendation preferences, and organizational views.
- Security delegated administrator accounts, GuardDuty detector features, S3 protection, EKS protection, Amazon ECR scanning, AWS Lambda scanning, finding filters, automated sensitive data discovery, graph retention, source collection, subscriber access, and cross-Region rollup.
- SageMaker data capture, baseline constraints, bias metrics, explainability configuration, experiment metadata, lineage metadata, Bedrock model access, guardrail policies, content filters, sensitive information filters, VPC endpoints, and logging.

### Operations

- Review dashboards, alarms, Logs Insights queries, synthetic checks, distributed traces, dependency maps, latency root causes, and incident timelines.
- Roll out ADOT collectors, validate Prometheus queries, tune metric cardinality, publish Grafana dashboards, and troubleshoot data sources.
- Execute fleet commands, open Session Manager sessions, review patch compliance, run automations, and track OpsItems.
- Perform Well-Architected reviews, track risks, run Resilience Hub assessments, and review resiliency recommendations.
- Analyze spend, review forecasts, tune budgets, validate CUR delivery, triage cost anomalies, and track rightsizing recommendations.
- Triage GuardDuty findings, Inspector vulnerabilities, Macie sensitive data findings, Detective investigations, and Security Lake subscriber delivery.
- Review model drift, bias reports, explainability reports, model lineage, Bedrock evaluations, guardrail tests, and generative AI governance evidence.

## AWS Week 6: DR, Multi-Region, And Migration Execution

### Service And Entity Coverage

- [Priority: Very High] Amazon Application Recovery Controller (ARC) (Route 53 ARC): clusters, routing controls, control panels, safety rules, readiness checks, recovery groups, cells, and resource sets.
- [Priority: Very High] AWS Elastic Disaster Recovery (AWS DRS) and AWS Application Migration Service (AWS MGN): DRS source servers, replication servers, staging area subnets, recovery instances, recovery snapshots, conversion servers, MGN source servers, replication agents, replication settings templates, launch settings, test instances, cutover instances, and waves.
- [Priority: Medium] AWS Migration Hub: applications, servers, migration waves, discovered resources, migration tools, home Region, and migration statuses.
- [Priority: Very High] AWS Database Migration Service (AWS DMS) and AWS Schema Conversion Tool (AWS SCT): replication instances, replication tasks, source endpoints, target endpoints, replication subnet groups, table mappings, validation reports, SCT migration projects, source schemas, target schemas, assessment reports, conversion rules, and converted schema objects.
- [Priority: High] AWS DataSync and AWS Transfer Family: DataSync agents, locations, tasks, task executions, manifests, filters, reports, transfer options, Transfer Family servers, users, workflows, connectors, identity providers, protocols, host keys, and access endpoints.
- [Priority: Very High] AWS Backup: backup plans, backup vaults, recovery points, backup vault lock configurations, copy jobs, restore jobs, legal holds, and restore testing plans.
- [Priority: Medium] AWS Fault Injection Service (AWS FIS): experiment templates, experiments, actions, targets, stop conditions, experiment roles, and experiment reports.

### DR Dependencies From Earlier Weeks

- Amazon Route 53 DNS failover and health checks are covered in Week 2.
- Amazon S3 replication, Amazon EFS replication, Amazon Aurora Global Database, Amazon RDS Read Replicas, and Amazon DynamoDB global tables are covered in Week 4.
- IAM emergency access, CloudTrail evidence, AWS Config drift data, AWS KMS keys, and AWS Backup vault controls are covered in earlier weeks.

### Controls

- ARC routing control state, safety rules, readiness check scope, cell design, and recovery group modeling.
- DRS/MGN replication settings, launch settings, staging area configuration, point-in-time recovery selection, failback settings, post-launch actions, and cutover settings.
- Migration Hub home Region selection, application grouping, migration status tracking, and migration tool integration.
- DMS full-load settings, change data capture (CDC), task settings, validation, transformation rules, endpoint encryption, SCT compatibility assessment, schema mapping, and conversion settings.
- DataSync task options, bandwidth limits, verification mode, include/exclude filters, encryption, schedule configuration, Transfer Family protocol selection, identity provider configuration, IAM roles, workflow triggers, and logging.
- AWS Backup schedules, lifecycle rules, AWS Backup Vault Lock, cross-Region copy, cross-account copy, vault access policies, and restore testing configuration.
- FIS stop conditions, IAM experiment roles, target selection, action parameters, and safety guardrails.

### Operations

- Execute ARC routing control tests, readiness checks, failover rehearsal, and game day evidence collection.
- Replicate servers with AWS DRS, run non-disruptive recovery drills, launch recovery instances, validate failback, and review replication lag.
- Use AWS MGN for migration waves, test launches, cutover launches, rollback planning, and post-launch validation.
- Track portfolio migration status in AWS Migration Hub and report wave readiness.
- Run DMS full-load plus CDC migration, validate data, troubleshoot replication lag, convert schemas with AWS SCT, and rehearse cutover/rollback.
- Use DataSync for file/object seeding, incremental sync, transfer validation, and cutover sync.
- Validate AWS Backup recovery points, restore testing, immutable backup controls, and ransomware recovery readiness.
- Run AWS FIS experiments with stop conditions to validate resilience assumptions and blast radius.