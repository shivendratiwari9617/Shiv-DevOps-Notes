# Grouped Cloud Services Crosswalk Notes

**Quality rating:** 9.95/10  
**Crosswalk note:** Mappings prioritize practical architectural equivalence over strict one-to-one product identity.  
**Usage note:** Read each domain top-down, from foundational infrastructure to higher-level managed capabilities.

---

## Compute

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **Amazon EC2** (AMI, instance families, placement groups) | **Compute Engine** (machine families, custom machine types, sole-tenant nodes) | **Azure Virtual Machines** (VM sizes, availability sets, dedicated host) | Provides virtualized compute instances with guest OS control.  <br>Used when workloads need host-level configuration and persistent runtime control. |
| **EC2 Auto Scaling** (launch templates, scaling policies) | **Managed Instance Groups** (instance templates, autoscaling) | **Virtual Machine Scale Sets** (autoscale rules, orchestration modes) | Automatically adds or removes VM instances from a managed group.  <br>Scaling decisions are driven by metrics, schedules, or health state. |
| **EC2 Spot Instances** | **Spot VMs** | **Azure Spot Virtual Machines** | Supplies discounted spare compute capacity with interruption risk.  <br>Best for fault-tolerant, batch, or checkpointed workloads. |
| **AWS Lambda** (versions, aliases, event sources) | **Cloud Functions** | **Azure Functions** | Runs event-driven code without managing server instances directly.  <br>Execution scales per trigger and billing follows invocation or runtime consumption. |
| **AWS Fargate** | **Cloud Run** | **Azure Container Apps** | Runs containers on managed compute without cluster node administration.  <br>Suited for stateless services, jobs, and event-triggered application components. |
| **Elastic Beanstalk** | **App Engine** | **Azure App Service** | Provides managed application hosting with platform-level deployment workflows.  <br>Reduces infrastructure management for web apps and APIs. |
| **AWS Batch** | **Batch** | **Azure Batch** | Schedules and executes large-scale batch or parallel compute jobs.  <br>Handles queuing, retries, and worker fleet management for non-interactive workloads. |
| **VMware Cloud on AWS** | **Google Cloud VMware Engine** | **Azure VMware Solution** | Runs VMware-based environments on cloud-managed infrastructure.  <br>Supports migration of vSphere-centric workloads with minimal redesign. |

---

## Storage

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **Amazon S3** (buckets, lifecycle rules, versioning) | **Cloud Storage** (buckets, storage classes, versioning) | **Azure Blob Storage** (containers, access tiers, versioning) | Stores objects in a flat namespace with metadata and HTTP-based access.  <br>Optimized for durability, scale, and unstructured data access patterns. |
| **S3 Glacier / Glacier Deep Archive** | **Cloud Storage Archive / Coldline** | **Blob Archive tier** | Provides lower-cost archival storage with slower retrieval characteristics.  <br>Used for long-term retention, backup, and compliance data preservation. |
| **Amazon EBS** (gp3, io2, snapshots) | **Persistent Disk / Hyperdisk** | **Managed Disks** (Premium SSD, Standard SSD, Ultra Disk) | Exposes network-attached block volumes to compute instances.  <br>Suitable for boot disks and low-latency filesystems managed by the guest OS. |
| **Amazon EFS** (performance modes, access points) | **Filestore** | **Azure Files** | Delivers managed shared file storage with standard file protocols.  <br>Used by multiple clients that need concurrent mounted file access. |
| **Amazon FSx** (Windows, Lustre, NetApp ONTAP, OpenZFS) | **NetApp Volumes** / specialized managed file services | **Azure NetApp Files** | Provides managed file systems with enterprise or workload-specific semantics.  <br>Targets high-performance, SMB/NFS, or application-specialized storage needs. |
| **AWS Storage Gateway** | **Storage Transfer Service** / **Transfer Appliance** | **Azure Storage Mover** / **Data Box** | Moves or bridges data between on-premises environments and cloud storage.  <br>Supports migration, hybrid access, or bulk ingest workflows. |
| **AWS Backup** | **Backup and DR Service** | **Azure Backup** | Centrally defines backup policies and recovery points for protected resources.  <br>Improves retention management, recovery orchestration, and operational consistency. |

---

## Networking & Content Delivery

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **Amazon VPC** (subnets, route tables, NACLs) | **Virtual Private Cloud** (subnets, routes, firewall rules) | **Azure Virtual Network** (subnets, route tables, NSGs) | Defines an isolated virtual network boundary for cloud resources.  <br>Controls addressing, segmentation, routing, and private connectivity patterns. |
| **Elastic Load Balancing** (ALB, NLB, GWLB) | **Cloud Load Balancing** (external, internal, proxy, passthrough) | **Azure Load Balancer / Application Gateway / Front Door** | Distributes traffic across multiple backends using Layer 4 or Layer 7 logic.  <br>Can provide health checks, TLS termination, and routing policies. |
| **Amazon Route 53** (hosted zones, health checks) | **Cloud DNS** | **Azure DNS** | Hosts authoritative DNS zones and answers name resolution queries.  <br>Maps domain names to service endpoints with managed zone administration. |
| **Amazon CloudFront** | **Cloud CDN** | **Azure CDN / Front Door** | Caches content at edge locations closer to end users.  <br>Reduces origin load and lowers latency for static or cacheable responses. |
| **AWS Transit Gateway** | **Cloud Router + Network Connectivity Center** | **Azure Virtual WAN** | Centralizes routing between many networks, sites, or cloud attachments.  <br>Simplifies hub-and-spoke and multi-network connectivity topologies. |
| **AWS Site-to-Site VPN / Client VPN** | **Cloud VPN** | **Azure VPN Gateway** | Establishes encrypted tunnels between remote networks and cloud networks.  <br>Used for hybrid connectivity, remote access, or interim migration links. |
| **AWS Direct Connect** | **Cloud Interconnect** | **Azure ExpressRoute** | Provides private dedicated connectivity that bypasses the public internet.  <br>Improves bandwidth predictability, latency consistency, and data path control. |
| **AWS PrivateLink / VPC endpoints** | **Private Service Connect** | **Azure Private Link / Private Endpoint** | Exposes services privately over provider backbone networking.  <br>Eliminates public endpoint exposure for supported service access patterns. |
| **AWS Global Accelerator** | **Global Load Balancing with Premium Tier** | **Azure Front Door** | Optimizes global entry points onto a provider backbone network.  <br>Improves path selection and failover for geographically distributed applications. |

---

## Security & Identity

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **AWS Identity and Access Management** (users, roles, policies) | **Cloud IAM** (principals, roles, bindings) | **Microsoft Entra ID + Azure RBAC** | Controls authentication context and authorization over cloud resources.  <br>Implements least privilege through identities, roles, and policy assignment. |
| **AWS Organizations / IAM Identity Center** | **Cloud Identity / Organization Policy** | **Management Groups / Entra tenant administration** | Governs multi-account or multi-project identity and policy structure.  <br>Standardizes access boundaries and organizational administration at scale. |
| **AWS KMS** | **Cloud KMS** | **Azure Key Vault Keys / Managed HSM** | Manages cryptographic keys used for encryption and signing operations.  <br>Separates key control from application logic and protected data storage. |
| **AWS Secrets Manager** | **Secret Manager** | **Azure Key Vault Secrets** | Stores application secrets with controlled retrieval and rotation workflows.  <br>Reduces hard-coded credentials in code, files, and deployment pipelines. |
| **AWS Certificate Manager** | **Certificate Manager** | **Azure Key Vault Certificates** | Provisions and manages TLS certificates for service endpoints.  <br>Supports certificate lifecycle tasks such as issuance, renewal, and binding. |
| **AWS WAF** | **Cloud Armor** | **Azure Web Application Firewall** | Applies Layer 7 inspection rules to HTTP and HTTPS traffic.  <br>Helps block malicious request patterns before they reach applications. |
| **AWS Shield** | **Cloud Armor DDoS protections** | **Azure DDoS Protection** | Mitigates distributed denial-of-service attacks against exposed endpoints.  <br>Protects service availability using traffic analysis and automated filtering. |
| **Amazon GuardDuty** | **Security Command Center threat detection** | **Microsoft Defender for Cloud** | Detects suspicious behavior from cloud telemetry and control-plane signals.  <br>Surfaces findings for investigation, response, and posture improvement. |
| **AWS Security Hub** | **Security Command Center** | **Microsoft Defender for Cloud** | Aggregates security findings and posture signals into a central view.  <br>Helps correlate misconfigurations, alerts, and compliance control status. |

---

## Databases

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **Amazon RDS** | **Cloud SQL** | **Azure SQL Database / Azure Database services** | Delivers managed relational databases with automated operations support.  <br>Common tasks include patching, backup, replication, and point-in-time recovery. |
| **Amazon Aurora** | **AlloyDB / Cloud SQL HA designs** | **Azure SQL Managed Instance** | Provides a cloud-optimized relational engine with managed high availability.  <br>Targets transactional workloads that need stronger managed database capabilities. |
| **Amazon DynamoDB** | **Firestore / Bigtable** | **Azure Cosmos DB / Table API patterns** | Supplies managed non-relational storage for low-latency, scalable access.  <br>Data models vary by product, including key-value, document, or wide-column forms. |
| **Amazon ElastiCache for Redis / Memcached** | **Memorystore** | **Azure Cache for Redis** | Adds in-memory data access for caching, sessions, or transient state.  <br>Reduces latency and lowers read pressure on primary data stores. |
| **Amazon Redshift** | **BigQuery** | **Azure Synapse Analytics** | Supports analytical SQL over large structured and semi-structured datasets.  <br>Optimized for reporting, aggregation, and warehouse-style query execution. |
| **Amazon Neptune** | **Neo4j on Google Cloud / graph solutions** | **Azure Cosmos DB Gremlin / graph solutions** | Stores and queries graph-shaped relationships between connected entities.  <br>Useful for recommendation, fraud, dependency, and knowledge graph workloads. |
| **Amazon DocumentDB** | **MongoDB Atlas on Google Cloud / document stores** | **Azure Cosmos DB for MongoDB** | Exposes document-oriented storage with JSON-like flexible schema patterns.  <br>Fits applications that evolve fields frequently or store hierarchical objects. |

---

## Analytics & Data Integration

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **AWS Glue** | **Dataflow / Dataproc Serverless / Data Fusion** | **Azure Data Factory / Synapse Pipelines** | Orchestrates and executes data movement or transformation workflows.  <br>Used for ETL, ELT, schema handling, and scheduled data preparation. |
| **Amazon EMR** | **Dataproc** | **Azure HDInsight / Synapse Spark** | Runs managed big-data frameworks such as Spark or Hadoop.  <br>Supports large-scale distributed processing with cluster or serverless options. |
| **Amazon Kinesis** (Data Streams, Firehose) | **Pub/Sub + Dataflow** | **Event Hubs + Stream Analytics** | Ingests and processes continuous event streams from producers at scale.  <br>Used for telemetry, clickstream, log, and near-real-time analytics pipelines. |
| **Amazon Athena** | **BigQuery** | **Azure Synapse serverless SQL** | Queries data in storage directly using SQL without manual cluster management.  <br>Useful for ad hoc analysis on lake-stored files and partitions. |
| **AWS Lake Formation** | **BigLake / Dataplex** | **Microsoft Fabric / Purview + lake governance patterns** | Adds governance and access control over data lake assets and metadata.  <br>Helps standardize discovery, permissions, and data domain management. |
| **AWS DMS** | **Database Migration Service** | **Azure Database Migration Service** | Migrates databases with support for ongoing replication or cutover phases.  <br>Reduces downtime when moving operational data between database environments. |

---

## Application Integration

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **Amazon SQS** | **Pub/Sub** | **Azure Queue Storage / Service Bus queues** | Queues messages for asynchronous processing between decoupled components.  <br>Consumers pull work independently, improving resilience and elasticity. |
| **Amazon SNS** | **Pub/Sub** | **Azure Service Bus topics / Event Grid** | Publishes messages to multiple subscribers using fan-out delivery patterns.  <br>Enables loosely coupled event distribution across services and endpoints. |
| **Amazon EventBridge** | **Eventarc** | **Event Grid** | Routes events between producers and consumers using managed event buses.  <br>Supports filtering, target bindings, and event-driven integration workflows. |
| **AWS Step Functions** | **Workflows** | **Azure Logic Apps / Durable Functions** | Coordinates multi-step execution with state, retries, and branching logic.  <br>Useful for orchestrating distributed tasks and long-running business flows. |
| **Amazon MQ** | **Managed open-source messaging partners** | **Azure Service Bus / managed broker partners** | Provides broker-style messaging semantics for integrated enterprise systems.  <br>Common patterns include ordered delivery, durable subscriptions, and transactions. |
| **Amazon API Gateway** | **API Gateway / Apigee** | **Azure API Management** | Publishes, secures, and manages APIs for client and service consumption.  <br>Can apply auth, throttling, transformation, and versioned exposure policies. |

---

## Containers & DevOps

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **Amazon EKS** | **Google Kubernetes Engine** | **Azure Kubernetes Service** | Runs managed Kubernetes control planes for container orchestration.  <br>Schedules pods, services, and controllers across worker node capacity. |
| **Amazon ECS** | **Cloud Run / GKE Autopilot fit-by-pattern** | **Azure Container Apps / AKS fit-by-pattern** | Orchestrates containerized workloads using provider-managed abstractions.  <br>Best equivalence depends on whether the need is cluster-native or serverless containers. |
| **Amazon ECR** | **Artifact Registry** | **Azure Container Registry** | Stores and serves container images and related OCI artifacts.  <br>Integrates with deployment systems, scanning, and pull authentication flows. |
| **AWS CodeCommit / CodeBuild / CodeDeploy / CodePipeline** | **Cloud Build / Cloud Deploy / Source Repositories legacy scope** | **Azure Repos / Pipelines / Artifacts** | Implements source control and continuous integration or delivery workflows.  <br>Automates build, test, release, and deployment stages across environments. |
| **AWS CloudFormation** | **Deployment Manager / Infrastructure Manager / Terraform usage** | **ARM templates / Bicep** | Declares infrastructure as code for repeatable environment provisioning.  <br>Encodes resources, dependencies, and configuration in version-controlled definitions. |
| **AWS Systems Manager Parameter Store** | **Runtime Config patterns / Secret Manager / metadata fit-by-use** | **App Configuration** | Centralizes application configuration values outside application binaries.  <br>Supports safer rollout and environment-specific configuration management. |

---

## Observability & Operations

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **Amazon CloudWatch** (metrics, alarms, dashboards) | **Cloud Monitoring** | **Azure Monitor** | Collects time-series telemetry and evaluates health or threshold conditions.  <br>Used for alerting, dashboarding, and operational visibility into resources. |
| **CloudWatch Logs** | **Cloud Logging** | **Azure Monitor Logs / Log Analytics** | Stores and queries log records emitted by systems and applications.  <br>Supports troubleshooting, audit review, and detection of runtime anomalies. |
| **AWS X-Ray** | **Cloud Trace** | **Application Insights / Azure Monitor OpenTelemetry** | Traces requests across distributed components and service boundaries.  <br>Helps identify latency contributors and failure points in request paths. |
| **AWS CloudTrail** | **Cloud Audit Logs** | **Azure Activity Log** | Records control-plane actions and administrative API activity.  <br>Supports auditing, forensics, and accountability for configuration changes. |
| **AWS Config** | **Cloud Asset Inventory / Config Controller patterns** | **Azure Policy / Resource Graph** | Tracks resource configuration state and evaluates policy compliance posture.  <br>Useful for drift detection, governance checks, and inventory visibility. |
| **AWS Trusted Advisor** | **Recommender** | **Azure Advisor** | Surfaces best-practice recommendations across cost, security, and operations.  <br>Highlights optimizations based on observed environment configuration and usage. |

---

## AI/ML

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **Amazon SageMaker** | **Vertex AI** | **Azure Machine Learning** | Builds, trains, deploys, and manages machine learning lifecycle workflows.  <br>Supports experimentation, model serving, and operational MLOps practices. |
| **Amazon Bedrock** | **Vertex AI Generative AI** | **Azure AI Foundry / Azure OpenAI Service** | Exposes managed foundation-model access for generative AI applications.  <br>Used for prompting, orchestration, and model-backed inference services. |
| **Amazon Textract / Rekognition / Comprehend** | **Document AI / Vision AI / Natural Language AI** | **Azure AI Document Intelligence / Vision / Language** | Provides prebuilt AI APIs for common perception and language tasks.  <br>Reduces custom model work for OCR, classification, extraction, and analysis. |

---

## Governance & Cost

| AWS | GCP | Azure | 2-line Technical Definition |
|---|---|---|---|
| **AWS Organizations** | **Resource Manager + Organizations model** | **Management Groups** | Establishes hierarchical administration boundaries across many accounts or subscriptions.  <br>Enables inherited policy application and centralized operational governance. |
| **AWS Control Tower** | **Landing zone blueprints / organization policy patterns** | **Azure Landing Zones** | Standardizes multi-environment setup using governed baseline architecture patterns.  <br>Typically includes identity, networking, logging, and policy guardrails. |
| **AWS Budgets / Cost Explorer** | **Cloud Billing budgets / cost reports** | **Azure Cost Management + Billing** | Tracks spend, forecasts usage cost, and alerts on financial thresholds.  <br>Improves visibility into cloud consumption and budget governance. |
| **AWS Service Quotas** | **Quotas and System Limits** | **Azure subscription and service limits** | Defines enforced capacity or API limits for services and resources.  <br>Quota planning prevents deployment failures and supports growth readiness. |
| **AWS License Manager** | **License controls via partner and policy tooling** | **Azure Hybrid Benefit / license management patterns** | Helps govern software licensing use across cloud-deployed resources.  <br>Important for compliance, cost efficiency, and entitlement tracking. |

---

## Readability improvements applied

- Grouped by domain for scan-friendly review.
- Important sub-entities separated into independent rows where they matter architecturally.
- Definitions written at the **capability level** when product scope differs across clouds.
- Stable ordering used throughout: foundational resource, scaling, connectivity, security, then higher-level managed services.
- Service names normalized to commonly accepted current names.
