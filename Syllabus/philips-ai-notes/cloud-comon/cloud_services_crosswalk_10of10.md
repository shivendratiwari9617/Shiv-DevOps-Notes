# Cloud Services Crosswalk Reference

**Quality target:** 10/10 reference edition  
**Crosswalk principle:** Main tables include direct equivalents or the closest practical capability-level equivalents.  
**Important note:** Some mappings are not identical products. Where clouds differ in service boundaries or platform design, the table uses the **closest practical equivalent** rather than forcing false one-to-one parity.

---

## Compute

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **Amazon EC2** | **Compute Engine** | **Azure Virtual Machines** | Virtualized compute instances with guest OS control, custom runtime configuration, and persistent workload management. |
| **EC2 Auto Scaling** | **Managed Instance Groups** | **Virtual Machine Scale Sets** | Managed VM fleet scaling based on metrics, schedules, or health state. |
| **EC2 Spot Instances** | **Spot VMs** | **Azure Spot Virtual Machines** | Discounted spare compute capacity with interruption risk for fault-tolerant workloads. |
| **AWS Lambda** | **Cloud Functions** | **Azure Functions** | Event-driven serverless code execution without direct server administration. |
| **AWS Fargate** | **Cloud Run** | **Azure Container Apps** | Managed container execution without cluster node management for stateless services and jobs. |
| **Elastic Beanstalk** | **App Engine** | **Azure App Service** | Managed application hosting for web apps and APIs with deployment platform features. |
| **AWS Batch** | **Batch** | **Azure Batch** | Batch job scheduling and parallel compute execution for non-interactive workloads. |
| **VMware Cloud on AWS** | **Google Cloud VMware Engine** | **Azure VMware Solution** | Managed VMware environments for migration of vSphere-based workloads with minimal redesign. |

---

## Storage

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **Amazon S3** | **Cloud Storage** | **Azure Blob Storage** | Managed object storage for durable, scalable storage of unstructured data. |
| **S3 Glacier / Glacier Deep Archive** | **Cloud Storage Archive / Coldline** | **Blob Archive tier** | Lower-cost archival storage optimized for long-term retention with slower retrieval. |
| **Amazon EBS** | **Persistent Disk / Hyperdisk** | **Managed Disks** | Network-attached block storage for VM boot volumes and low-latency filesystems. |
| **Amazon EFS** | **Filestore** | **Azure Files** | Managed shared file storage exposed through standard file protocols. |
| **Amazon FSx** | **NetApp Volumes** | **Azure NetApp Files** | Managed high-performance or enterprise file storage with workload-specific filesystem semantics. |
| **AWS Storage Gateway** | **Storage Transfer Service / Transfer Appliance** | **Azure Storage Mover / Data Box** | Data movement and hybrid transfer services between on-premises systems and cloud storage. |
| **AWS Backup** | **Backup and DR Service** | **Azure Backup** | Centralized backup policy, retention, and recovery management for protected resources. |

---

## Networking & Content Delivery

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **Amazon VPC** | **Virtual Private Cloud** | **Azure Virtual Network** | Isolated virtual networking boundary for addressing, segmentation, routing, and private connectivity. |
| **Elastic Load Balancing** | **Cloud Load Balancing** | **Azure Load Balancer / Application Gateway / Front Door** | Managed Layer 4 or Layer 7 traffic distribution with health checks and routing policies. |
| **Amazon Route 53** | **Cloud DNS** | **Azure DNS** | Managed authoritative DNS hosting for domain-to-endpoint resolution. |
| **Amazon CloudFront** | **Cloud CDN** | **Azure CDN / Front Door** | Edge caching for lower latency and reduced origin load. |
| **AWS Transit Gateway** | **Network Connectivity Center** | **Azure Virtual WAN** | Centralized inter-network routing for hub-and-spoke and multi-network connectivity. |
| **AWS Site-to-Site VPN / Client VPN** | **Cloud VPN** | **Azure VPN Gateway** | Encrypted connectivity between cloud networks, remote users, and on-premises environments. |
| **AWS Direct Connect** | **Cloud Interconnect** | **Azure ExpressRoute** | Private dedicated connectivity that avoids the public internet. |
| **AWS PrivateLink** | **Private Service Connect** | **Azure Private Link** | Private access to services over provider backbone networking without public exposure. |
| **AWS Global Accelerator** | **Premium Tier global load distribution** | **Azure Front Door** | Global traffic entry optimization over provider backbone infrastructure. |

---

## Security & Identity

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **AWS IAM** | **Cloud IAM** | **Azure RBAC** | Resource authorization through identities, roles, and policy assignment. |
| **IAM Identity Center** | **Cloud Identity** | **Microsoft Entra ID** | Workforce identity, federation, and directory-backed access administration. |
| **AWS KMS** | **Cloud KMS** | **Azure Key Vault Keys / Managed HSM** | Managed cryptographic key control for encryption and signing operations. |
| **AWS Secrets Manager** | **Secret Manager** | **Azure Key Vault Secrets** | Centralized secret storage with controlled retrieval and rotation workflows. |
| **AWS Certificate Manager** | **Certificate Manager** | **Azure Key Vault Certificates** | Certificate lifecycle management for TLS-enabled services. |
| **AWS WAF** | **Cloud Armor** | **Azure Web Application Firewall** | Layer 7 request inspection and rule enforcement for web application protection. |
| **AWS Shield** | **Cloud Armor DDoS protections** | **Azure DDoS Protection** | DDoS mitigation for internet-facing services and endpoints. |
| **Amazon GuardDuty** | **Security Command Center threat detection** | **Microsoft Defender for Cloud** | Threat detection based on cloud telemetry, activity, and configuration signals. |
| **AWS Security Hub** | **Security Command Center** | **Microsoft Defender for Cloud** | Centralized security posture and finding aggregation across cloud resources. |

---

## Databases

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **Amazon RDS** | **Cloud SQL** | **Azure SQL Database / Azure Database services** | Managed relational databases with patching, backup, and high-availability support. |
| **Amazon Aurora** | **AlloyDB** | **Azure SQL Managed Instance** | Cloud-optimized managed relational database platforms for transactional workloads. |
| **Amazon DynamoDB** | **Firestore / Bigtable** | **Azure Cosmos DB** | Managed non-relational databases for scalable low-latency access, with model differences by platform. |
| **Amazon ElastiCache** | **Memorystore** | **Azure Cache for Redis** | Managed in-memory caching for low-latency reads, sessions, and transient state. |
| **Amazon Redshift** | **BigQuery** | **Azure Synapse Analytics** | Managed analytical SQL engines for warehousing and large-scale query execution. |
| **Amazon Neptune** | **Graph database solutions** | **Cosmos DB Gremlin** | Graph-oriented data platforms for relationship-heavy workloads such as dependency and recommendation analysis. |
| **Amazon DocumentDB** | **Document database solutions** | **Cosmos DB for MongoDB** | Managed document-oriented storage for JSON-like flexible schema applications. |

---

## Analytics & Data Integration

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **AWS Glue** | **Dataflow / Data Fusion** | **Azure Data Factory** | Managed data integration, transformation, and pipeline orchestration for ETL and ELT workflows. |
| **Amazon EMR** | **Dataproc** | **Azure HDInsight / Synapse Spark** | Managed big-data processing platforms for Spark- and Hadoop-based workloads. |
| **Amazon Kinesis** | **Pub/Sub + Dataflow** | **Event Hubs + Stream Analytics** | Event streaming and near-real-time ingestion and processing for telemetry and log pipelines. |
| **Amazon Athena** | **BigQuery external querying patterns** | **Synapse serverless SQL** | Serverless SQL querying of data stored directly in cloud object storage. |
| **AWS Lake Formation** | **Dataplex / BigLake** | **Microsoft Purview + lake governance patterns** | Governance, catalog, and access control overlays for lake-centric data environments. |
| **AWS DMS** | **Database Migration Service** | **Azure Database Migration Service** | Managed database migration with replication and cutover support. |

---

## Application Integration

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **Amazon SQS** | **Pub/Sub** | **Service Bus queues / Queue Storage** | Asynchronous message queueing between decoupled application components. |
| **Amazon SNS** | **Pub/Sub** | **Service Bus topics / Event Grid** | Publish-subscribe event distribution to multiple downstream consumers. |
| **Amazon EventBridge** | **Eventarc** | **Event Grid** | Managed event routing between producers and consumers with filtering and target bindings. |
| **AWS Step Functions** | **Workflows** | **Logic Apps / Durable Functions** | Stateful orchestration for multi-step, retry-aware, and branching workflows. |
| **Amazon MQ** | **Managed messaging partner solutions** | **Azure Service Bus** | Brokered messaging for enterprise integration patterns requiring durable delivery semantics. |
| **Amazon API Gateway** | **API Gateway / Apigee** | **Azure API Management** | API publishing, protection, transformation, and lifecycle management. |

---

## Containers & DevOps

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **Amazon EKS** | **Google Kubernetes Engine** | **Azure Kubernetes Service** | Managed Kubernetes platforms for orchestrating containerized workloads. |
| **Amazon ECS** | **Cloud Run** | **Azure Container Apps** | Managed container orchestration without requiring full Kubernetes administration for common service workloads. |
| **Amazon ECR** | **Artifact Registry** | **Azure Container Registry** | Container image and OCI artifact storage integrated with deployment pipelines. |
| **AWS CodeBuild / CodeDeploy / CodePipeline** | **Cloud Build / Cloud Deploy** | **Azure Pipelines** | CI/CD services for build, release, and deployment automation. |
| **AWS CloudFormation** | **Infrastructure Manager** | **ARM templates / Bicep** | Infrastructure-as-code definitions for repeatable resource provisioning. |
| **AWS Systems Manager Parameter Store** | **Secret Manager / config patterns** | **App Configuration** | Externalized application configuration and runtime parameter management. |

---

## Observability & Operations

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **Amazon CloudWatch** | **Cloud Monitoring** | **Azure Monitor** | Metrics collection, alerting, dashboards, and service health visibility. |
| **CloudWatch Logs** | **Cloud Logging** | **Log Analytics / Azure Monitor Logs** | Centralized log ingestion, storage, and query for troubleshooting and audit use. |
| **AWS X-Ray** | **Cloud Trace** | **Application Insights** | Distributed tracing for request flow analysis and latency diagnosis. |
| **AWS CloudTrail** | **Cloud Audit Logs** | **Azure Activity Log** | Control-plane and administrative activity auditing. |
| **AWS Config** | **Cloud Asset Inventory / policy evaluation patterns** | **Azure Policy / Resource Graph** | Resource configuration visibility, policy evaluation, and governance support. |
| **AWS Trusted Advisor** | **Recommender** | **Azure Advisor** | Best-practice recommendations across reliability, cost, security, and operations. |

---

## AI / ML

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **Amazon SageMaker** | **Vertex AI** | **Azure Machine Learning** | Managed machine learning lifecycle platforms for model build, train, deploy, and operate workflows. |
| **Amazon Bedrock** | **Vertex AI Generative AI** | **Azure AI Foundry / Azure OpenAI Service** | Managed foundation-model access for generative AI application development. |
| **Textract / Rekognition / Comprehend** | **Document AI / Vision AI / Natural Language AI** | **Azure AI Document Intelligence / Vision / Language** | Prebuilt AI APIs for OCR, perception, extraction, and language analysis. |

---

## Governance & Cost

| AWS | GCP | Azure | Technical Definition |
|---|---|---|---|
| **AWS Organizations** | **Resource hierarchy and organization model** | **Management Groups** | Hierarchical governance boundaries for large multi-account or multi-subscription estates. |
| **AWS Control Tower** | **Landing zone governance patterns** | **Azure Landing Zones** | Standardized governed environment baselines for enterprise cloud adoption. |
| **AWS Budgets / Cost Explorer** | **Cloud Billing budgets / reports** | **Azure Cost Management + Billing** | Spend tracking, budgeting, forecasting, and financial governance. |
| **AWS Service Quotas** | **Quotas and System Limits** | **Azure service and subscription limits** | Enforced service capacity and API limits affecting deployment scale. |
| **AWS License Manager** | **License governance partner patterns** | **Azure Hybrid Benefit / license governance patterns** | Software licensing visibility and optimization across cloud resources. |

---

## Non-Equivalent / Platform-Specific Services

These constructs are important, but they should remain outside the main crosswalk because they do **not** have a clean service-to-service equivalent across all three clouds.

### Tenancy & Billing Boundaries

| Cloud | Service / Construct | Why it is separate |
|---|---|---|
| AWS | **AWS Account** | Primary tenancy, billing, and isolation boundary rather than a workload service. |
| GCP | **Project** | Core resource, quota, and billing boundary rather than a workload service. |
| Azure | **Subscription** | Primary billing and administrative boundary rather than a workload service. |

### Governance & Hierarchy

| Cloud | Service / Construct | Why it is separate |
|---|---|---|
| AWS | **AWS Organizations** | Multi-account governance hierarchy, not a direct runtime service peer. |
| GCP | **Folders / Organization** | Hierarchical governance and policy structure rather than a runtime service. |
| Azure | **Management Groups** | Governance hierarchy above subscriptions rather than a workload service. |

### Identity & Directory Boundaries

| Cloud | Service / Construct | Why it is separate |
|---|---|---|
| AWS | **IAM Identity Center** | Workforce access and federation layer, not a direct peer to resource IAM services. |
| GCP | **Cloud Identity** | Organization-wide identity layer that extends beyond simple resource authorization. |
| Azure | **Microsoft Entra tenant** | Directory and identity boundary spanning many control-plane and SaaS integrations. |

### Storage Namespace / Resource Containers

| Cloud | Service / Construct | Why it is separate |
|---|---|---|
| AWS | **S3 bucket namespace model** | Object namespace concept rather than a separate top-level multi-service storage container. |
| GCP | **Project-scoped storage ownership model** | Administrative boundary for storage resources, not a standalone storage wrapper service. |
| Azure | **Storage Account** | Top-level storage namespace and configuration boundary containing multiple storage services. |
| Azure | **Resource Group** | Lifecycle and deployment grouping abstraction with no stable one-to-one cross-cloud equivalent. |

### Architecture Frameworks

| Cloud | Service / Construct | Why it is separate |
|---|---|---|
| AWS | **Control Tower** | Opinionated landing-zone framework rather than a single runtime service. |
| GCP | **Enterprise foundation / landing zone patterns** | Architecture framework patterns rather than a single directly comparable service. |
| Azure | **Landing Zones** | Prescriptive architecture framework rather than a single runtime service. |

---

## Editorial Standards Applied

- Main tables keep **direct equivalents** or **closest practical capability-level equivalents**.
- Platform-specific constructs are isolated in a dedicated **non-equivalent** section.
- Service names are normalized to commonly used current naming.
- Rows avoid forcing weak one-to-one parity when product boundaries differ.
- The document is optimized for study, architecture review, and cloud comparison reference.
