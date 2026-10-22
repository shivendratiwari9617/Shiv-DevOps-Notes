# AWS Week-Wise Cloud Coverage

Use this as an AWS-focused supplement for AWS Cloud Platform, AWS Solutions Architect, SRE, DevOps, Cloud, Security, DR, and migration.

## AWS Week 1: Landing Zone, Account Strategy, And Governance

### AWS Organizations

- Entities: organization, root, management account, member accounts, organizational units (OUs), and delegated administrators.
- Policies: service control policies (SCPs), resource control policies (RCPs), tag policies, backup policies, AI services opt-out policies, and policy inheritance.
- Operations: account creation, account invitations, account closure, OU movement, trusted access, delegated administrator registration, and consolidated billing.

### AWS Account Management

- Entities: AWS accounts, account names, root user email addresses, primary contacts, alternate contacts, AWS Regions, and Region opt-in status.
- Controls: primary-contact ownership, alternate-contact ownership, Region opt-in/opt-out, and account metadata governance.
- Operations: account-contact updates, alternate-contact review, Region access validation, and account closure readiness.

### AWS Control Tower

- Entities: landing zone, Account Factory, governed accounts, and registered organizational units (OUs).
- Controls: preventive controls, detective controls, proactive controls, drift detection, and control remediation.
- Operations: landing-zone version updates, account enrollment, OU registration, baseline customization, and drift remediation.

### AWS Identity and Access Management (IAM)

- Entities: IAM users, IAM groups, IAM roles, IAM policies, instance profiles, access keys, and MFA devices.
- Policy types: identity-based policies, resource-based policies, permissions boundaries, and role trust policies.
- Access controls: role assumption, multi-factor authentication (MFA), attribute-based access control (ABAC), least privilege, and AWS Security Token Service (AWS STS) temporary security credentials.

### AWS IAM Identity Center

- Entities: identity sources, users, groups, permission sets, account assignments, and application assignments.
- Access controls: workforce identity federation, permission-set provisioning, session duration, and account assignment review.

### AWS Identity and Access Management Access Analyzer (IAM Access Analyzer)

- Entities: analyzers, findings, archive rules, access previews, and policy validation checks.
- Controls: external access analysis, internal access analysis, unused access analysis, custom policy checks, analyzer scope, and finding review.

### AWS Resource Access Manager

- Entities: resource shares (AWS RAM shares), principals, resources, managed permissions, customer managed permissions, and invitations.
- Access controls: resource-share permissions, AWS Organizations sharing, external-account sharing, invitation acceptance, and access revocation.

### AWS Service Catalog

- Entities: portfolios, products, provisioning artifacts, provisioned products, constraints, launch paths, and portfolio access.
- Controls: launch constraints, template constraints, tag update constraints, portfolio sharing, and launch permissions.
- Operations: product launch, provisioned product updates, provisioning artifact versioning, and access review.

### AWS CloudFormation StackSets

- Entities: stack sets (StackSets), stack instances, stack operations, administrator accounts, execution roles, target accounts, and target organizational units (OUs).
- Controls: service-managed permissions, self-managed permissions, automatic deployments, account filters, operation preferences, region targeting, failure tolerance, and concurrency.
- Operations: multi-account deployment, StackSet updates, drift detection, stack instance remediation, and rollback.

### AWS CloudTrail

- Entities: trails, organization trails, event data stores, management events, data events, and Insights events.
- Controls: multi-Region trails, organization-wide logging, log file integrity validation, encryption, retention, and centralized audit storage.
- Operations: API activity investigation, change tracking, incident timelines, log delivery verification, and CloudTrail Lake queries.

### AWS Config

- Entities: configuration recorders, delivery channels, configuration items, rules, conformance packs, configuration aggregators, and remediation actions.
- Controls: resource recording, compliance evaluation, managed rules, custom rules, organization aggregation, and automated remediation.
- Operations: configuration history review, drift detection, audit reporting, resource relationship analysis, and compliance dashboards.

### AWS Security Hub

- Entities: security standards, controls, findings, insights, automation rules, finding aggregators, administrator accounts, and member accounts.
- Controls: AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark, delegated administration, cross-Region aggregation, and control enablement.
- Operations: centralized security posture review, cross-account finding triage, control status review, finding workflow status updates, severity prioritization, and remediation tracking.

### AWS Audit Manager

- Entities: assessments, frameworks, control sets, controls, evidence folders, evidence, Evidence Finder, and assessment reports.
- Controls: framework mapping, control owners, automated evidence collection, manual evidence uploads, and assessment scoping.
- Operations: audit preparation, evidence review, control-status tracking, report generation, and compliance handoff.

### AWS Artifact

- Entities: reports, agreements, and notifications.
- Controls: report access, agreement acceptance, notification subscriptions, and document retention.
- Operations: compliance report download, auditor document requests, agreement review, and shared-responsibility validation.

### AWS Health

- Entities: AWS Health events, affected entities, event types, and event status codes.
- Controls: organizational view, delegated administrator access, event filtering, Amazon EventBridge routing, and operational ownership.
- Operations: service-impact assessment, maintenance tracking, incident communication, affected-resource review, and escalation workflows.

### AWS Service Quotas

- Entities: service quotas, applied quota values, quota increase requests, quota request templates, and usage metrics.
- Controls: quota baselines, utilization monitoring, and quota-risk review.
- Operations: pre-migration quota validation, capacity planning, quota request submission, regional quota checks, and growth forecasting.

## AWS Week 2: Networking, Private Connectivity, And Edge Traffic

### Amazon Virtual Private Cloud (Amazon VPC)

- Entities: VPCs, subnets, route tables, internet gateways, egress-only internet gateways, NAT gateways, elastic IP addresses, elastic network interfaces, prefix lists, DHCP option sets, security groups, network ACLs, and VPC Flow Logs.
- Controls: CIDR allocation, subnet routing, route table associations, security group rules, network ACL rules, and flow log delivery.
- Operations: packet path validation, Amazon VPC Reachability Analyzer checks, Amazon VPC Network Access Analyzer review, route drift review, NAT gateway saturation review, and connectivity troubleshooting.

### Amazon VPC IP Address Manager (IPAM)

- Entities: IPAMs, scopes, pools, allocations, resource discoveries, and IPAM operating Regions.
- Controls: pool allocation rules, CIDR compliance monitoring, cross-account pool sharing, and overlapping CIDR detection.
- Operations: address planning, CIDR allocation, utilization review, migration overlap analysis, and multi-account IP inventory.

### AWS Transit Gateway

- Entities: transit gateways, transit gateway attachments, transit gateway route tables, route table associations, route table propagations, peering attachments, and multicast domains.
- Controls: route domains, attachment acceptance, route propagation, appliance mode, and cross-account sharing through AWS Resource Access Manager.
- Operations: attachment onboarding, route validation, segmentation review, failover testing, and hybrid connectivity troubleshooting.

### AWS Cloud WAN

- Entities: global networks, core networks, core network policies, core network edges, segments, attachments, and network function groups.
- Controls: segment isolation, attachment policies, route sharing, policy versions, and centralized network governance.
- Operations: branch or VPC onboarding, global route review, segment troubleshooting, and policy deployment validation.

### Amazon VPC Peering

- Entities: VPC peering connections, requester VPCs, accepter VPCs, route table entries, and DNS resolution settings.
- Controls: non-overlapping CIDRs, route table updates, security group references, and DNS resolution options.
- Operations: peering acceptance, connectivity validation, route troubleshooting, and point-to-point migration cleanup.

### VPC Endpoints and AWS PrivateLink

- Entities: interface endpoints, gateway endpoints, Gateway Load Balancer endpoints, endpoint services, endpoint network interfaces, endpoint policies, and private DNS names.
- Controls: endpoint policies, security groups, private DNS, allowed principals, acceptance settings, and service exposure boundaries.
- Operations: private AWS service access, private service publishing, DNS troubleshooting, endpoint health validation, and internet-egress reduction.

### Amazon VPC Lattice

- Entities: service networks, services, target groups, listeners, rules, auth policies, resource configurations, and access logs.
- Controls: auth policies, service network associations, routing rules, TLS settings, and target group health checks.
- Operations: service-to-service connectivity, cross-account service exposure, listener updates, access log review, and traffic routing validation.

### AWS Verified Access

- Entities: Verified Access instances, Verified Access groups, Verified Access endpoints, trust providers, access policies, and application domains.
- Controls: identity trust, device trust, policy-based access, TLS certificates, and access log delivery.
- Operations: private application onboarding, policy testing, access troubleshooting, trust provider updates, and session review.

### AWS Network Firewall

- Entities: firewalls, firewall policies, rule groups, stateless rules, stateful rules, TLS inspection configurations, alert logs, and flow logs.
- Controls: centralized inspection, Suricata-compatible rules, domain list rules, default actions, and logging configuration.
- Operations: egress filtering, east-west inspection, dropped-flow investigation, rule deployment, and firewall policy review.

### Elastic Load Balancing

- Entities: Application Load Balancers, Network Load Balancers, Gateway Load Balancers, target groups, listeners, listener rules, health checks, and load balancer attributes.
- Controls: TLS security policies, certificates, target group attributes, cross-zone load balancing, deletion protection, and access logs.
- Operations: target health review, deregistration delay review, listener updates, traffic shifting, and load balancer troubleshooting.

### AWS Site-to-Site VPN

- Entities: customer gateways, virtual private gateways, transit gateways, VPN connections, VPN tunnels, tunnel options, and customer gateway devices.
- Controls: IKE/IPsec parameters, BGP routing, static routing, tunnel inside CIDRs, pre-shared keys, and tunnel endpoint lifecycle control.
- Operations: tunnel failover testing, route validation, customer gateway updates, hybrid cutover, and VPN backup for AWS Direct Connect.

### AWS Client VPN

- Entities: Client VPN endpoints, target network associations, authorization rules, routes, client CIDR ranges, client certificates, and connection logs.
- Controls: client authentication, authorization rules, security groups, split-tunnel routing, self-service portal access, and connection logging.
- Operations: user VPN onboarding, route validation, certificate rotation, connection troubleshooting, and access review.

### AWS Direct Connect

- Entities: connections, link aggregation groups (LAGs), private virtual interfaces, public virtual interfaces, transit virtual interfaces, Direct Connect gateways, and hosted connections.
- Controls: BGP authentication, allowed prefixes, MACsec, virtual interface ownership, gateway associations, and resiliency model.
- Operations: circuit turn-up, route validation, failover testing, bandwidth review, and hybrid connectivity troubleshooting.

### Amazon Route 53

- Entities: hosted zones, records, routing policies, health checks, reusable delegation sets, domains, and key-signing keys (KSKs).
- Controls: TTLs, routing policies, health-check routing, DNSSEC signing, delegation, and domain transfer lock.
- Operations: DNS cutover, failover validation, record change review, delegation troubleshooting, and domain lifecycle management.

### Amazon Route 53 Resolver

- Entities: inbound Resolver endpoints, outbound Resolver endpoints, Resolver rules, rule associations, query logging configurations, DNS Firewall rule groups, and domain lists.
- Controls: conditional forwarding, DNS Firewall rules, query logging, VPC rule associations, and hybrid DNS boundaries.
- Operations: hybrid DNS troubleshooting, query log review, forwarding validation, resolver endpoint scaling, and domain block-list maintenance.

### AWS Certificate Manager

- Entities: public certificates, private certificates, imported certificates, domain names, subject alternative names (SANs), and validation options.
- Controls: DNS validation, email validation, certificate transparency logging preference, key algorithms, and managed renewal.
- Operations: certificate issuance, domain validation, renewal tracking, load balancer association, CloudFront association, and API Gateway custom domain association.

### Amazon CloudFront

- Entities: distributions, origins, cache behaviors, cache policies, origin request policies, response headers policies, invalidations, key groups, and origin access controls.
- Controls: TLS certificates, viewer protocol policies, signed URLs, signed cookies, origin access control, geo restrictions, and AWS WAF associations.
- Operations: cache invalidation, origin failover validation, latency review, header/cookie forwarding review, and edge delivery troubleshooting.

### AWS WAF

- Entities: web ACLs, rules, rule groups, managed rule groups, IP sets, regex pattern sets, labels, and CAPTCHA configurations.
- Controls: rate-based rules, managed rule groups, custom rules, Bot Control, rule priority, and logging configuration.
- Operations: false-positive tuning, sampled request review, rule rollout, block-list updates, and web ACL association review.

### AWS Shield

- Entities: protections, protected resources, protection groups, attacks, and AWS Shield Advanced subscriptions.
- Controls: AWS Shield Standard, AWS Shield Advanced, proactive engagement, automatic application layer DDoS mitigation, and cost protection.
- Operations: DDoS event review, protected-resource onboarding, AWS Shield Response Team (SRT) escalation readiness, and post-event analysis.

### AWS Firewall Manager

- Entities: policies, policy scopes, administrator accounts, member accounts, resource sets, and compliance status.
- Controls: centralized AWS WAF policies, AWS Shield Advanced policies, AWS Network Firewall policies, security group policies, and Amazon Route 53 Resolver DNS Firewall policies.
- Operations: multi-account policy rollout, noncompliant resource review, policy remediation, and delegated administrator management.

### AWS Global Accelerator

- Entities: accelerators, listeners, endpoint groups, endpoints, static anycast IP addresses, endpoint weights, and traffic dials.
- Controls: traffic dials, endpoint weights, health checks, client affinity, and accelerator attributes.
- Operations: global traffic shifting, regional failover validation, endpoint health review, blue/green cutover, and latency improvement analysis.

### Amazon API Gateway

- Entities: REST APIs, HTTP APIs, WebSocket APIs, resources, routes, methods, integrations, stages, deployments, authorizers, usage plans, API keys, custom domain names, and VPC links.
- Controls: authorizers, resource policies, throttling, mutual TLS, stage variables, request validation, and AWS WAF associations.
- Operations: stage deployment, access log review, private integration troubleshooting, canary release, throttling review, and API version cutover.

## AWS Week 3: Compute, Kubernetes, CI/CD, And Platform Delivery

### Amazon Elastic Compute Cloud (Amazon EC2)

- Entities: instances, Amazon Machine Images (AMIs), instance types, key pairs, security groups, elastic network interfaces, instance profiles, user data, and Elastic IP addresses.
- Controls: instance metadata options, IAM instance profiles, security group rules, termination protection, hibernation settings, and EBS-optimized configuration.
- Operations: instance launch, stop/start, AMI-based rollback, console output review, system status checks, instance recovery, and right-sizing review.

### Amazon EC2 Auto Scaling

- Entities: Auto Scaling groups, launch templates, launch configurations, scaling policies, scheduled actions, lifecycle hooks, warm pools, instance refreshes, and health checks.
- Controls: desired/min/max capacity, health check type, termination policies, mixed instances policy, scaling cooldowns, and instance refresh preferences.
- Operations: rolling replacement, capacity rebalancing, scale-out validation, unhealthy instance replacement, deployment rollback, and quota review.

### EC2 Image Builder

- Entities: image pipelines, recipes, components, infrastructure configurations, distribution settings, image workflows, and output AMIs.
- Controls: component versioning, test workflows, vulnerability scanning, distribution accounts, and KMS encryption.
- Operations: golden AMI creation, patch baseline refresh, image promotion, compliance validation, and rollback to previous AMIs.

### Amazon Elastic Container Registry (Amazon ECR)

- Entities: private repositories, public repositories, images, tags, manifests, lifecycle policies, pull through cache rules, and replication configurations.
- Controls: repository policies, image scanning, tag immutability, lifecycle rules, encryption, and cross-Region replication.
- Operations: image push/pull, vulnerability review, image promotion, cleanup of stale tags, and pull-through cache troubleshooting.

### Amazon Elastic Container Service (Amazon ECS)

- Entities: clusters, services, task definitions, tasks, capacity providers, container instances, service deployments, and task sets.
- Controls: task IAM roles, execution roles, deployment configuration, service discovery, load balancer integration, and capacity provider strategy.
- Operations: rolling deployments, blue/green deployments through AWS CodeDeploy, task placement troubleshooting, service scaling, and failed-task analysis.

### Amazon Elastic Kubernetes Service (Amazon EKS)

- Entities: clusters, control planes, managed node groups, self-managed nodes, Fargate profiles, add-ons, access entries, Kubernetes service accounts, and pod identity associations.
- Controls: endpoint access, cluster access management, Amazon EKS add-ons, IAM roles for service accounts (IRSA), EKS Pod Identity, security groups for pods, and upgrade policies.
- Operations: cluster upgrades, node group upgrades, add-on updates, workload rollout review, pod scheduling troubleshooting, and audit log review.

### Karpenter on Amazon EKS

- Entities: NodePool resources, EC2NodeClass resources, NodeClaim resources, provisioned nodes, instance requirements, and disruption budgets.
- Controls: instance type requirements, capacity type selection, consolidation policy, node expiration, taints, labels, and disruption controls.
- Operations: node provisioning review, scale-out troubleshooting, consolidation review, interruption handling, and cost-aware capacity tuning.

### AWS Fargate

- Entities: Amazon ECS tasks on AWS Fargate, Amazon ECS services on AWS Fargate, Amazon EKS pods on AWS Fargate, Fargate profiles, task sizes, ephemeral storage, and platform versions.
- Controls: task CPU/memory, platform version, subnet placement, security groups, task roles, and execution roles.
- Operations: serverless container deployment, task startup troubleshooting, platform version review, capacity validation, and cost review.

### AWS Lambda

- Entities: functions, versions, aliases, layers, event source mappings, function URLs, execution roles, provisioned concurrency, and reserved concurrency.
- Controls: IAM execution roles, resource-based policies, VPC configuration, environment variables, concurrency limits, code signing, and dead-letter queues.
- Operations: deployment through versions and aliases, cold-start review, throttling analysis, retry behavior review, and event source troubleshooting.

### AWS App Runner

- Entities: services, source configurations, image repositories, build configurations, VPC connectors, observability configurations, and custom domains.
- Controls: auto scaling configuration, health checks, deployment settings, VPC egress, access roles, instance roles, and encryption.
- Operations: containerized web app deployment, automatic redeployment, domain association, service rollback, and health troubleshooting.

### AWS Elastic Beanstalk

- Entities: applications, application versions, environments, environment tiers, platforms, saved configurations, and deployment policies.
- Controls: platform versions, environment configuration, rolling updates, immutable deployments, load balancer settings, and instance profiles.
- Operations: application deployment, environment cloning, blue/green deployment through CNAME swap, platform update, and environment health review.

### Amazon Lightsail

- Entities: instances, blueprints, bundles, static IPs, snapshots, disks, load balancers, container services, databases, and distributions.
- Controls: firewall rules, snapshots, static IP assignment, key pairs, and load balancer health checks.
- Operations: small workload launch, snapshot restore, instance migration planning, static IP cutover, and simple application hosting.

### AWS Batch

- Entities: jobs, job queues, job definitions, compute environments, scheduling policies, array jobs, multi-node parallel jobs, and fair share identifiers.
- Controls: compute environment type, allocation strategy, job priority, retry strategy, timeout settings, and IAM roles.
- Operations: batch workload submission, queue backlog review, compute environment scaling, failed-job analysis, and Spot interruption handling.

### AWS CodeBuild

- Entities: build projects, buildspec files, build environments, reports, report groups, artifacts, caches, and webhooks.
- Controls: service roles, environment images, VPC configuration, privileged mode, environment variables, artifact encryption, and timeout settings.
- Operations: build execution, test reporting, dependency caching, image build pipelines, failed-build analysis, and artifact publishing.

### AWS CodePipeline

- Entities: pipelines, stages, actions, source actions, AWS CodeConnections connections (CodeStar connections), transitions, artifacts, variables, triggers, and executions.
- Controls: approval actions, stage transitions, artifact stores, service roles, cross-account actions, and retry behavior.
- Operations: release orchestration, execution review, manual approval, rollback coordination, failed-action troubleshooting, and deployment traceability.

### AWS CodeDeploy

- Entities: applications, deployment groups, deployments, deployment configurations, AppSpec files, lifecycle event hooks, and revisions.
- Controls: in-place deployments, blue/green deployments, traffic shifting, automatic rollback, alarms, and deployment health constraints.
- Operations: deployment monitoring, rollback execution, lifecycle hook troubleshooting, canary validation, and failed deployment review.

### AWS CodeArtifact

- Entities: domains, repositories, packages, package versions, assets, upstream repositories, external connections, and authorization tokens.
- Controls: domain policies, repository policies, upstream control, package origin controls, encryption, and token duration.
- Operations: package publishing, dependency resolution, artifact promotion, package version cleanup, and upstream repository troubleshooting.

### AWS CloudFormation

- Entities: stacks, stack sets, templates, parameters, outputs, mappings, conditions, change sets, drift detection results, and nested stacks.
- Controls: stack policies, change sets, rollback triggers, termination protection, capabilities, hooks, and service roles.
- Operations: infrastructure deployment, change review, drift detection, stack rollback, nested stack updates, and multi-account deployment through StackSets.

### AWS Cloud Development Kit (AWS CDK)

- Entities: apps, stacks, constructs, aspects, stages, environments, context values, assets, and synthesized CloudFormation templates.
- Controls: bootstrap stacks, synthesizers, permissions boundaries, context management, stack dependencies, and asset publishing.
- Operations: construct library reuse, synthesis review, diff review, deployment, rollback through CloudFormation, and pipeline integration.

### AWS Serverless Application Model (AWS SAM)

- Entities: AWS SAM templates, AWS::Serverless::Function resources (serverless functions), AWS::Serverless::Api resources (REST APIs), AWS::Serverless::HttpApi resources (HTTP APIs), AWS::Serverless::SimpleTable resources (DynamoDB tables), AWS::Serverless::StateMachine resources (state machines), events, globals, and deployment configurations.
- Controls: template transforms, deployment preferences, permissions policies, local configuration, and generated CloudFormation resources.
- Operations: local testing, package and deploy workflows, guided deployments, Lambda rollout validation, and rollback through CloudFormation.

### AWS Proton

- Entities: environment templates, service templates, environments, services, service instances, components, template bundles, and deployments.
- Controls: platform-approved templates, template versions, environment account connections, deployment workflows, and infrastructure as code bundles.
- Operations: self-service service provisioning, template version rollout, environment update, service instance update, and platform standardization.

### Amazon SageMaker AI (Amazon SageMaker)

- Entities: Amazon SageMaker AI domains (Studio domains), user profiles, spaces, training jobs, processing jobs, models, endpoint configurations, endpoints, pipelines, model package groups, and model packages.
- Controls: execution roles, network isolation, VPC configuration, model approval status, endpoint deployment configuration, and artifact encryption.
- Operations: model training, processing, pipeline execution, model registration, endpoint deployment, endpoint rollback, and inference troubleshooting.

### Amazon Bedrock

- Entities: foundation models, inference profiles, provisioned throughput, agents, knowledge bases, guardrails, model customization jobs, and model evaluation jobs.
- Controls: model access, guardrails, IAM permissions, VPC endpoints, encryption, and invocation logging.
- Operations: model invocation, prompt testing, guardrail validation, knowledge base synchronization, agent deployment, and evaluation review.

## AWS Week 4: Data, Storage, Backup, And Stateful Reliability

### Amazon Simple Storage Service (Amazon S3)

- Entities: buckets, objects, prefixes, access points, Multi-Region Access Points, bucket policies, S3 Block Public Access settings, S3 Object Lock configurations, lifecycle configurations, replication rules, and storage classes.
- Controls: bucket policies, Object Ownership, versioning, default encryption, S3 Object Lock retention modes, lifecycle rules, replication, and access logging.
- Operations: object restore, lifecycle validation, replication monitoring, public-access review, storage class optimization, and data retention review.

### Amazon Elastic Block Store (Amazon EBS)

- Entities: volumes, snapshots, snapshot archives, volume types, provisioned IOPS, throughput settings, encryption settings, and Fast Snapshot Restore.
- Controls: volume encryption, snapshot sharing, delete-on-termination settings, volume type selection, IOPS/throughput limits, and KMS key selection.
- Operations: volume attachment, online volume modification, snapshot creation, snapshot restore, performance review, and rollback from snapshots.

### Amazon Elastic File System (Amazon EFS)

- Entities: file systems, mount targets, access points, lifecycle policies, throughput modes, performance modes, backups, and replication configurations.
- Controls: file system policies, security groups, encryption, access point POSIX identities, lifecycle management, and backup policies.
- Operations: mount troubleshooting, throughput review, client access validation, backup restore, and replication failover planning.

### Amazon FSx

- Entities: file systems, backups, storage virtual machines, volumes, shares, endpoints, deployment types, and throughput capacity.
- Controls: encryption, subnet placement, security groups, Active Directory integration, backup retention, and file system type selection.
- Operations: file system provisioning, backup restore, throughput tuning, failover validation, and workload-specific file service migration.

### Amazon Relational Database Service (Amazon RDS)

- Entities: DB instances, DB clusters, DB snapshots, automated backups, parameter groups, option groups, subnet groups, read replicas, and reserved DB instances.
- Controls: Multi-AZ deployment, backup retention, deletion protection, storage autoscaling, encryption, IAM database authentication, and maintenance windows.
- Operations: failover validation, point-in-time recovery (PITR), replica promotion, engine upgrades, parameter changes, and connection troubleshooting.

### Amazon Aurora

- Entities: DB clusters, DB instances, cluster endpoints, reader endpoints, custom endpoints, Aurora Replicas, cluster parameter groups, global databases, and backtrack windows.
- Controls: Multi-AZ replication, backup retention, deletion protection, storage encryption, IAM database authentication, and failover priority.
- Operations: reader scaling, failover testing, global database switchover, point-in-time recovery (PITR), query performance review, and replica lag review.

### Amazon DynamoDB

- Entities: tables, items, attributes, partition keys, sort keys, local secondary indexes, global secondary indexes, streams, global tables, backups, and time to live (TTL).
- Controls: capacity modes, auto scaling, table classes, encryption, point-in-time recovery (PITR), resource-based policies, and deletion protection.
- Operations: hot partition review, throttling analysis, index tuning, backup restore, stream processing validation, and global table replication review.

### Amazon DocumentDB (with MongoDB compatibility)

- Entities: clusters, instances, cluster endpoints, reader endpoints, parameter groups, subnet groups, snapshots, automated backups, and change streams.
- Controls: VPC placement, TLS, encryption, backup retention, maintenance windows, and audit logging.
- Operations: cluster scaling, snapshot restore, failover validation, query analysis, connection troubleshooting, and migration from MongoDB-compatible workloads.

### Amazon Neptune

- Entities: DB clusters, DB instances, cluster endpoints, reader endpoints, parameter groups, subnet groups, snapshots, and Neptune Streams.
- Controls: IAM database authentication, encryption, backup retention, VPC placement, audit logs, and query timeout settings.
- Operations: graph query tuning, snapshot restore, failover validation, loader job review, and Gremlin/SPARQL/openCypher troubleshooting.

### Amazon Timestream for LiveAnalytics (Amazon Timestream)

- Entities: databases, tables, memory stores, magnetic stores, retention properties, scheduled queries, and query results.
- Controls: retention configuration, magnetic store writes, encryption, IAM permissions, and scheduled query configuration.
- Operations: time-series ingestion review, retention tuning, query optimization, scheduled query validation, and late-arriving data handling.

### Amazon ElastiCache

- Entities: serverless caches, replication groups, cache clusters, nodes, shards, parameter groups, subnet groups, users, user groups, snapshots, and endpoints.
- Controls: engine selection, encryption in transit, encryption at rest, user group access, automatic backups, maintenance windows, and cluster mode.
- Operations: cache hit ratio review, failover testing, shard scaling, snapshot restore, eviction analysis, and connection troubleshooting.

### Amazon MemoryDB

- Entities: clusters, shards, nodes, subnet groups, parameter groups, users, access control lists (ACLs), snapshots, and endpoints.
- Controls: Redis-compatible access control lists, encryption, Multi-AZ deployment, snapshot retention, maintenance windows, and parameter settings.
- Operations: failover validation, snapshot restore, memory usage review, latency analysis, and Redis-compatible workload migration.

### Amazon OpenSearch Service

- Entities: domains, indexes, data nodes, cluster manager nodes, UltraWarm nodes, cold storage, index templates, snapshots, and OpenSearch Dashboards.
- Controls: fine-grained access control, VPC access, encryption, node-to-node encryption, index state management, automated snapshots, and audit logs.
- Operations: shard allocation review, snapshot restore, index lifecycle review, query latency tuning, cluster health review, and log analytics troubleshooting.

### Amazon Redshift

- Entities: provisioned clusters, serverless namespaces, serverless workgroups, databases, schemas, tables, materialized views, snapshots, parameter groups, and workload management (WLM) queues.
- Controls: IAM roles, KMS encryption, audit logging, workload management, concurrency scaling, data sharing, and snapshot retention.
- Operations: query tuning, vacuum/analyze review, snapshot restore, workload queue review, resize planning, and data warehouse migration.

### Amazon Athena

- Entities: workgroups, queries, named queries, prepared statements, data catalogs, databases, tables, partitions, and query result locations.
- Controls: workgroup settings, query result encryption, bytes scanned limits, data catalog permissions, and result location enforcement.
- Operations: SQL query execution, partition repair, cost review, query failure troubleshooting, and data lake validation.

### AWS Glue

- Entities: Data Catalog databases, Data Catalog tables, crawlers, jobs, triggers, workflows, connections, schemas, classifiers, and data quality rulesets.
- Controls: job bookmarks, worker type, connections, security configurations, encryption, IAM roles, and Data Catalog permissions.
- Operations: crawler execution, extract-transform-load (ETL) job runs, schema discovery, job retry review, data quality validation, and pipeline troubleshooting.

### AWS Glue DataBrew

- Entities: projects, datasets, recipes, recipe jobs, profile jobs, rulesets, schedules, and outputs.
- Controls: recipe versioning, dataset connections, job output settings, encryption, and IAM roles.
- Operations: visual data preparation, data profiling, recipe execution, data quality review, and prepared dataset publishing.

### AWS Lake Formation

- Entities: data lake administrators, registered locations, Data Catalog resources, LF-Tags, permissions, data filters, resource links, and blueprints.
- Controls: Lake Formation permissions, LF-Tag-based access control, row-level filters, column-level filters, cross-account sharing, and trusted resource owners.
- Operations: data lake onboarding, permission grants, access troubleshooting, tag policy review, and governed data sharing.

### Amazon Kinesis Data Streams

- Entities: data streams, shards, producers, consumers, enhanced fan-out consumers, sequence numbers, retention periods, and stream modes.
- Controls: shard count, on-demand capacity mode, provisioned capacity mode, retention, encryption, resource policies, and consumer registration.
- Operations: ingestion monitoring, shard scaling, iterator age review, consumer lag troubleshooting, and replay from retention.

### Amazon Data Firehose (Amazon Kinesis Data Firehose)

- Entities: Firehose streams, sources, destinations, buffering hints, data format conversion settings, dynamic partitioning settings, and error output prefixes.
- Controls: buffering, compression, encryption, delivery retry settings, Lambda transformations, and destination backup settings.
- Operations: streaming delivery validation, failed record review, transformation troubleshooting, buffering tuning, and destination cutover.

### Amazon Managed Streaming for Apache Kafka (Amazon MSK)

- Entities: clusters, brokers, topics, partitions, consumer groups, configurations, client authentication settings, and connectors.
- Controls: broker instance type, storage, encryption, client authentication, Apache Kafka ACLs, logging, and version upgrades.
- Operations: consumer lag review, broker scaling, partition reassignment, topic validation, connector troubleshooting, and rolling version upgrades.

### Amazon Simple Queue Service (Amazon SQS)

- Entities: standard queues, FIFO queues, messages, message groups, deduplication IDs, dead-letter queues, visibility timeout, and redrive policies.
- Controls: queue policies, encryption, delay seconds, message retention, visibility timeout, receive message wait time, and redrive allow policies.
- Operations: backlog review, dead-letter queue redrive, duplicate-message analysis, consumer scaling, and poison-message handling.

### Amazon Simple Notification Service (Amazon SNS)

- Entities: topics, subscriptions, messages, filter policies, delivery policies, message attributes, and platform applications.
- Controls: topic policies, encryption, subscription filter policies, delivery retry policies, and data protection policies.
- Operations: fanout validation, subscription troubleshooting, message filtering review, delivery failure review, and event notification routing.

### Amazon EventBridge

- Entities: event buses, rules, targets, event patterns, archives, replays, schedules, connections, and API destinations.
- Controls: event bus policies, rule patterns, input transformers, retry policies, dead-letter queues, and schema discovery.
- Operations: event routing validation, replay execution, failed target troubleshooting, archive review, and event pattern testing.

### AWS Step Functions

- Entities: state machines, states, executions, activities, tasks, versions, aliases, distributed maps, and execution history.
- Controls: state machine type, retry policies, catch handlers, timeout settings, IAM roles, logging, tracing, and encryption.
- Operations: workflow execution review, failed-state troubleshooting, retry tuning, version deployment, and orchestration rollback.

### Amazon SageMaker Feature Store

- Entities: feature groups, feature definitions, records, online stores, offline stores, event time, and record identifiers.
- Controls: online/offline store configuration, encryption, IAM roles, data catalog integration, and feature group retention.
- Operations: feature ingestion, feature lookup validation, offline store review, training-serving consistency checks, and feature reuse governance.

### Amazon SageMaker Data Wrangler

- Entities: data flows, data sources, transforms, analyses, processing jobs, and exported notebooks.
- Controls: data source access, processing job configuration, encryption, and IAM roles.
- Operations: data preparation, feature engineering, data quality review, flow export, and processing job troubleshooting.

### AWS Secrets Manager

- Entities: secrets, secret versions, version stages, rotation schedules, rotation Lambda functions, resource policies, and replication settings.
- Controls: KMS encryption, automatic rotation, resource policies, version staging labels, cross-Region replication, and access permissions.
- Operations: secret retrieval validation, rotation troubleshooting, version rollback, replication review, and credential lifecycle management.

### AWS Key Management Service (AWS KMS)

- Entities: AWS KMS keys, aliases, key policies, grants, key material, multi-Region keys, imported key material, and external key stores.
- Controls: key policies, grants, rotation, deletion waiting periods, imported key material expiration, and multi-Region replication.
- Operations: encryption troubleshooting, key rotation review, grant cleanup, key usage audit, and cross-account key access validation.

### AWS Backup

- Entities: backup plans, backup rules, backup selections, recovery points, backup vaults, backup vault lock configurations, copy jobs, restore jobs, and backup reports.
- Controls: backup schedules, lifecycle rules, vault policies, AWS Backup Vault Lock, cross-Region copy, cross-account copy, and legal holds.
- Operations: backup job review, restore testing, recovery point validation, backup copy validation, and ransomware recovery readiness.

### AWS Database Migration Service (AWS DMS)

- Entities: replication instances, replication tasks, endpoints, replication subnet groups, event subscriptions, data providers, and migration projects.
- Controls: full-load settings, change data capture (CDC), table mappings, task settings, validation settings, encryption, and endpoint permissions.
- Operations: schema migration with AWS Schema Conversion Tool (AWS SCT), full-load migration, CDC replication, data validation, cutover, and rollback planning.

## AWS Week 5: Observability, FinOps, And Operational Excellence

### Amazon CloudWatch

- Entities: metrics, namespaces, dimensions, alarms, dashboards, log groups, log streams, metric filters, Contributor Insights rules, synthetic canaries, Application Signals services, Container Insights clusters, and Logs Insights queries.
- Controls: alarm actions, metric math, composite alarms, log retention, data protection policies, cross-account observability, and dashboard permissions.
- Operations: dashboard review, alarm tuning, log investigation, Logs Insights analysis, synthetic check review, Application Signals latency analysis, and incident timeline correlation.

### AWS Distro for OpenTelemetry (ADOT)

- Entities: collectors, pipelines, receivers, processors, exporters, instrumentation libraries, and configuration files.
- Controls: telemetry routing, sampling, resource attributes, exporter configuration, and secure endpoint delivery.
- Operations: metric/trace/log collection, collector rollout, sampling tuning, pipeline troubleshooting, and telemetry backend migration.

### Amazon Managed Service for Prometheus

- Entities: workspaces, alert manager definitions, rule groups namespaces, scrapers, metrics, labels, and remote write endpoints.
- Controls: workspace policies, ingestion limits, retention, alerting rules, and IAM permissions.
- Operations: Prometheus metric ingestion, query validation, alert rule deployment, cardinality review, and workspace access troubleshooting.

### Amazon Managed Grafana

- Entities: workspaces, data sources, dashboards, folders, users, teams, service accounts, and workspace API keys.
- Controls: authentication providers, data source permissions, workspace roles, network access, and plugin management.
- Operations: dashboard publishing, data source validation, workspace access review, alert visualization, and cross-account observability views.

### AWS X-Ray

- Entities: traces, segments, subsegments, service maps, groups, sampling rules, annotations, and metadata.
- Controls: sampling rules, encryption, trace retention, daemon/SDK configuration, and IAM permissions.
- Operations: distributed tracing, dependency map review, latency root-cause analysis, error trace investigation, and sampling tuning.

### AWS Systems Manager

- Entities: managed nodes, documents, Run Command, Session Manager sessions, State Manager associations, Patch Manager baselines, Automation runbooks, Parameter Store parameters, and OpsItems.
- Controls: instance profiles, session logging, patch baselines, maintenance windows, document permissions, and parameter policies.
- Operations: fleet command execution, secure shell access through Session Manager, patch compliance review, automation execution, and operational issue tracking.

### AWS Well-Architected Tool

- Entities: workloads, lenses, milestones, improvement plans, review templates, profiles, and workload shares.
- Controls: lens selection, review ownership, workload sharing, milestone tracking, and improvement plan priorities.
- Operations: architecture review, risk tracking, remediation planning, review refresh, and operational excellence reporting.

### AWS Resilience Hub

- Entities: applications, app components, resilience policies, assessments, resiliency scores, recommendations, and standard operating procedures (SOPs).
- Controls: recovery time objective/recovery point objective (RTO/RPO) targets, policy mapping, application import sources, and assessment scope.
- Operations: resilience assessment, recommendation tracking, alarm/SOP generation, drift review, and application resiliency reporting.

### AWS Cost Explorer

- Entities: cost and usage data, dimensions, filters, groups, forecasts, rightsizing recommendations, and Savings Plans recommendations.
- Controls: cost filters, grouping dimensions, amortized cost views, forecast windows, and linked account visibility.
- Operations: spend analysis, cost trend review, service cost breakdown, forecast review, and savings opportunity analysis.

### AWS Budgets

- Entities: budgets, budget actions, alerts, subscribers, thresholds, cost filters, and usage filters.
- Controls: budget thresholds, alert recipients, budget actions, cost scope, and usage scope.
- Operations: budget creation, threshold tuning, alert review, budget action validation, and monthly spend governance.

### AWS Cost Categories

- Entities: cost categories, rules, rule versions, split charge rules, inherited values, and effective dates.
- Controls: rule order, account/service/tag mapping, split charge allocation, and category versioning.
- Operations: chargeback mapping, showback reporting, cost allocation validation, and category rule maintenance.

### AWS Cost and Usage Reports (AWS CUR)

- Entities: report definitions, report versions, Amazon S3 report destinations, time granularity, resource IDs, and report schemas.
- Controls: report delivery options, compression, encryption, S3 bucket policies, report versioning, and integration with Athena.
- Operations: CUR delivery validation, Athena query setup, cost analytics, chargeback reporting, and billing anomaly investigation.

### AWS Cost Anomaly Detection

- Entities: anomaly monitors, alert subscriptions, anomalies, root causes, thresholds, and subscribers.
- Controls: monitor dimensions, alert thresholds, subscription frequency, and subscriber routing.
- Operations: anomaly triage, root-cause review, alert tuning, service/account spend investigation, and incident-style cost follow-up.

### AWS Compute Optimizer

- Entities: recommendations, findings, utilization metrics, projected metrics, recommendation preferences, and enrolled accounts.
- Controls: account enrollment, enhanced infrastructure metrics, recommendation preferences, and rightsizing risk settings.
- Operations: EC2 rightsizing, Auto Scaling group review, EBS volume optimization, Lambda memory review, and recommendation adoption tracking.

### AWS Trusted Advisor

- Entities: checks, recommendations, categories, resources, organizational views, priorities, and Trusted Advisor refreshes.
- Controls: AWS Organizations integration, notification preferences, refresh access, and support-plan visibility.
- Operations: operational check review, cost optimization review, security recommendation tracking, quota review, and organizational reporting.

### Amazon GuardDuty

- Entities: detectors, findings, finding types, threat intelligence sets, trusted IP lists, malware protection plans, and member accounts.
- Controls: delegated administrator, detector features, S3 protection, EKS protection, malware protection, and finding export configuration.
- Operations: threat finding triage, member account onboarding, suppression rule review, malware finding investigation, and incident escalation.

### Amazon Inspector

- Entities: findings, coverage, software bills of materials (SBOMs), resource scans, filter criteria, suppression rules, and delegated administrator accounts.
- Controls: EC2 scanning, Amazon ECR scanning, AWS Lambda scanning, finding filters, delegated administration, and export settings.
- Operations: vulnerability triage, package remediation tracking, image vulnerability review, Lambda finding review, and coverage validation.

### Amazon Macie

- Entities: S3 buckets, classification jobs, findings, sensitive data discovery results, managed data identifiers, custom data identifiers, and allow lists.
- Controls: automated sensitive data discovery, classification job scope, finding publishing, allow lists, and delegated administration.
- Operations: sensitive data review, public bucket risk triage, classification job execution, finding remediation, and compliance reporting.

### Amazon Detective

- Entities: behavior graphs, finding groups, investigations, profiles, member accounts, and data source packages.
- Controls: delegated administration, member account invitations, data source selection, and graph retention.
- Operations: security investigation, entity behavior review, finding group analysis, GuardDuty finding enrichment, and incident evidence review.

### Amazon Security Lake

- Entities: data lakes, sources, subscribers, rollup Regions, custom sources, Amazon Security Lake tables, and Open Cybersecurity Schema Framework (OCSF) event classes.
- Controls: source collection, subscriber access, encryption, lifecycle settings, cross-Region rollup, and delegated administration.
- Operations: security data normalization, subscriber delivery validation, threat hunting, retention review, and SIEM integration.

### Amazon SageMaker AI Model Monitor

- Entities: monitoring schedules, baselines, constraints, statistics, monitoring jobs, reports, and endpoint data capture configurations.
- Controls: data capture, monitoring frequency, baseline constraints, alert routing, and output encryption.
- Operations: model drift review, data quality monitoring, bias monitoring handoff, endpoint monitoring, and retraining signal review.

### Amazon SageMaker Clarify

- Entities: processing jobs, bias reports, explainability reports, analysis configurations, baselines, and SHAP values.
- Controls: bias metrics, explainability configuration, dataset selection, IAM roles, and artifact encryption.
- Operations: bias analysis, model explainability review, report publishing, compliance review support, and model governance evidence.

### Amazon SageMaker Experiments

- Entities: experiments, trials, trial components, runs, metrics, parameters, and artifacts.
- Controls: experiment tracking structure, run metadata, artifact storage, and access permissions.
- Operations: training run comparison, hyperparameter review, model lineage support, and experiment reproducibility tracking.

### Amazon SageMaker ML Lineage Tracking

- Entities: lineage groups, artifacts, actions, contexts, associations, models, datasets, and endpoint deployments.
- Controls: lineage metadata, artifact associations, model package linkage, and access permissions.
- Operations: model lineage review, audit evidence creation, deployment traceability, dataset-to-model tracking, and rollback investigation.

### Amazon Bedrock

- Entities: model evaluation jobs, evaluation datasets, guardrails, guardrail versions, foundation models, agents, and invocation logs.
- Controls: model access, guardrail policies, content filters, denied topics, sensitive information filters, VPC endpoints, and logging.
- Operations: model evaluation review, guardrail testing, prompt safety validation, invocation troubleshooting, and generative AI governance reporting.

## AWS Week 6: DR, Multi-Region, And Migration Execution

### Amazon Route 53

- Entities: hosted zones, failover routing records, weighted routing records, latency routing records, health checks, calculated health checks, and record sets.
- Controls: routing policies, health check thresholds, TTLs, DNS failover, and delegation.
- Operations: DNS failover validation, cutover rehearsal, rollback record changes, health check review, and recovery routing verification.

### Amazon Application Recovery Controller (ARC) (Route 53 ARC)

- Entities: clusters, routing controls, control panels, safety rules, readiness checks, recovery groups, cells, and resource sets.
- Controls: routing control state, safety rules, readiness check scope, cell design, and recovery group modeling.
- Operations: failover execution, readiness validation, cell-based recovery review, routing control testing, and game day evidence collection.

### AWS Elastic Disaster Recovery (AWS DRS)

- Entities: source servers, replication servers, staging area subnets, launch templates, recovery instances, recovery snapshots, and conversion servers.
- Controls: replication settings, launch settings, staging area configuration, point-in-time recovery selection, and failback settings.
- Operations: server replication, non-disruptive recovery drill, cutover launch, failback, recovery validation, and replication lag review.

### AWS Application Migration Service (AWS MGN)

- Entities: source servers, replication agents, replication settings templates, launch settings, test instances, cutover instances, and waves.
- Controls: replication server configuration, staging area subnets, launch templates, post-launch actions, and cutover settings.
- Operations: lift-and-shift migration, test launch, cutover launch, wave execution, rollback planning, and post-launch validation.

### AWS Migration Hub

- Entities: applications, servers, migration waves, discovered resources, migration tools, home Region, and migration statuses.
- Controls: home Region selection, application grouping, migration status tracking, and tool integration.
- Operations: portfolio tracking, wave reporting, dependency review, migration status updates, and executive migration visibility.

### AWS Database Migration Service (AWS DMS)

- Entities: replication instances, replication tasks, source endpoints, target endpoints, replication subnet groups, table mappings, and validation reports.
- Controls: full-load settings, change data capture (CDC), task settings, validation, transformation rules, and endpoint encryption.
- Operations: database migration rehearsal, CDC lag review, data validation, cutover execution, rollback planning, and replication troubleshooting.

### AWS Schema Conversion Tool (AWS SCT)

- Entities: database migration projects, source schemas, target schemas, assessment reports, conversion rules, and converted schema objects.
- Controls: conversion settings, compatibility assessment, schema mapping, extension pack usage, and report review.
- Operations: schema assessment, schema conversion, incompatible object review, migration effort estimation, and target schema deployment.

### AWS DataSync

- Entities: agents, locations, tasks, task executions, manifests, filters, reports, and transfer options.
- Controls: task options, bandwidth limits, verification mode, include/exclude filters, encryption, and schedule configuration.
- Operations: file/object transfer, migration seeding, incremental sync, transfer validation, cutover sync, and error remediation.

### AWS Transfer Family

- Entities: servers, users, workflows, connectors, identity providers, protocols, host keys, and access endpoints.
- Controls: protocol selection, identity provider configuration, user home directories, IAM roles, workflow triggers, and logging.
- Operations: managed SFTP/FTPS/FTP transfer, partner onboarding, file transfer cutover, workflow execution, and transfer troubleshooting.

### AWS Backup

- Entities: backup plans, backup vaults, recovery points, backup vault lock configurations, copy jobs, restore jobs, legal holds, and restore testing plans.
- Controls: backup schedules, lifecycle rules, AWS Backup Vault Lock, cross-Region copy, cross-account copy, vault access policies, and restore testing configuration.
- Operations: restore rehearsal, recovery point validation, immutable backup review, ransomware recovery readiness, and restore evidence collection.

### Amazon Simple Storage Service Replication (Amazon S3 Replication)

- Entities: replication rules, destination buckets, replication metrics, replication time control, delete marker replication, replica modification sync, and batch replication jobs.
- Controls: versioning, replication rule scope, destination encryption, IAM replication roles, S3 Replication Time Control (S3 RTC), and Object Ownership.
- Operations: replication status review, batch replication, regional copy validation, failover data readiness, and object-level recovery testing.

### Amazon Elastic File System Replication (Amazon EFS Replication)

- Entities: replication configurations, source file systems, destination file systems, replication overwrite protection, and recovery point objectives.
- Controls: destination Region, encryption, replication overwrite protection, and failback planning.
- Operations: file system replication monitoring, failover rehearsal, destination promotion, failback preparation, and replication health review.

### Amazon Aurora Global Database

- Entities: global databases, primary DB clusters, secondary DB clusters, global cluster identifiers, writer endpoints, reader endpoints, and replica lag metrics.
- Controls: secondary Region placement, write forwarding, failover settings, backup retention, encryption, and parameter alignment.
- Operations: managed planned failover, unplanned failover, replica lag review, write endpoint cutover, and global database recovery testing.

### Amazon Relational Database Service Read Replicas (Amazon RDS Read Replicas)

- Entities: read replicas, source DB instances, replica endpoints, replica lag metrics, promotion settings, and automated backups.
- Controls: replica Region, instance class, storage, backup settings, encryption, and promotion planning.
- Operations: read scaling, cross-Region replica validation, replica promotion, failover rehearsal, and replication lag troubleshooting.

### Amazon DynamoDB Global Tables

- Entities: global tables, replica tables, replica Regions, streams, conflict resolution, and replication metrics.
- Controls: replica Region selection, capacity mode, encryption, deletion protection, point-in-time recovery (PITR), and stream replication.
- Operations: active-active data validation, replica addition/removal, replication latency review, regional failover validation, and conflict behavior review.

### AWS Fault Injection Service (AWS FIS)

- Entities: experiment templates, experiments, actions, targets, stop conditions, experiment roles, and experiment reports.
- Controls: stop conditions, IAM experiment roles, target selection, action parameters, and safety guardrails.
- Operations: controlled failure injection, game day execution, resilience hypothesis validation, blast-radius review, and experiment rollback.
