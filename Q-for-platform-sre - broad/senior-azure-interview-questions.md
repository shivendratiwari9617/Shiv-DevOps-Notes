Senior Azure Cloud Architect / Platform / DevOps / SRE Interview Question Set
Target profile: 8–12 years of experience in a product-based company
---
1) Core must-master senior screening questions
1. Walk me through the Azure platform you would design for a product company that needs fast delivery, strong security, and multi-region resilience.
2. How do you decide whether a workload belongs on AKS, App Service, Container Apps, Functions, VM/VMSS, or a managed PaaS service?
3. What is your approach to designing Azure landing zones for an enterprise with multiple product teams and shared platform services?
4. How do you structure management groups, subscriptions, and resource groups for long-term scale?
5. How would you implement zero-trust principles on Azure across identity, network, data, and operations?
6. How do you make infrastructure changes safe in production on Azure?
7. What telemetry would you require before calling a system production-ready on Azure?
8. How do you handle a major Azure outage or partial service degradation when your platform depends on managed services?
9. How do you balance speed, cost, resilience, and operability when making platform decisions?
10. What are the top Azure anti-patterns you have seen in real production environments?

---
2) Architecture and platform design scenarios on Azure
1. Design a multi-tenant Azure platform for several product teams that need shared security, independent delivery, and controlled network access.
2. How would you design an Azure platform for a SaaS product with bursty traffic, regulated data, and global users?
3. A new product team wants to launch quickly, but security requires strict governance and private connectivity. How would you design the onboarding pattern?
4. How would you choose between one shared hub-spoke network versus multiple regional hubs?
5. How would you design platform services for developer self-service without letting teams bypass guardrails?
6. How would you design a secure shared services model for Key Vault, image registries, container supply chain, logging, and DNS?
7. How would you design for region failure if your application uses Azure SQL, Cosmos DB, Storage, and AKS?
8. How would you architect for regulated workloads that must use private endpoints and minimal public exposure?
9. How would you design platform standards to support both greenfield and legacy workloads migrating into Azure?
10. What would your reference architecture look like for an enterprise Azure platform that supports batch, microservices, event-driven systems, and data workloads?

---
3) Azure networking deep dive and troubleshooting
1. A workload in AKS cannot reach a private endpoint in another subnet. What would you check first, and why?
2. Users report intermittent timeouts after a hub-spoke routing change. How would you isolate whether the issue is UDR, firewall, peering, or DNS?
3. How do you design a hub-spoke network for shared services, AKS, private PaaS access, and on-prem connectivity?
4. What are the production trade-offs between Azure CNI, Azure CNI Overlay, and Kubenet for AKS networking?
5. A subnet is running out of IPs and new pods are failing to schedule in AKS. How would you diagnose and fix it?
6. Your application is behind Front Door, Application Gateway, and an internal Load Balancer. Requests break under load after a config change. How do you troubleshoot the chain?
7. How would you decide between Front Door, Application Gateway, Load Balancer, and API Management for different traffic patterns?
8. A private endpoint works from one network but not another. What DNS and network components would you verify?
9. How do you design secure egress for Azure workloads: NAT Gateway, Azure Firewall, user-defined routing, or something else?
10. What would you inspect when ExpressRoute connectivity is up, but application traffic still fails intermittently?
11. How would you troubleshoot VPN connectivity where tunnels are up but traffic does not pass?
12. What are the common failure modes in VNet peering and how do you detect them quickly?
13. How do you handle private DNS at scale across many subscriptions and multiple regions?
14. How would you verify whether a problem is caused by Azure Firewall, NSG, ASG, or a route table?
15. How do you design for overlapping IP space during acquisition or migration scenarios?

---
4) Identity, RBAC, Azure AD / Entra ID, management groups, subscriptions, policy, and landing zone strategy
1. How do you design Entra ID and RBAC boundaries for platform teams, app teams, and security teams?
2. A team claims they need Owner on a subscription to move fast. How would you challenge or redesign that model?
3. How would you structure management groups and subscriptions for a company with many products, environments, and compliance domains?
4. How would you implement Azure Policy to prevent noncompliant resources without blocking delivery unnecessarily?
5. A deployment starts failing because a new policy denies it. How do you investigate and resolve this without weakening governance?
6. How would you design a landing zone for private-by-default workloads with controlled public exposure?
7. How do managed identities change your security and deployment model compared with service principals?
8. How would you troubleshoot a workload that cannot read from Key Vault even though RBAC seems correct?
9. What is your strategy for privileged access, break-glass accounts, and emergency operations in Azure?
10. How would you manage tenant-wide standards when different products have different compliance and risk profiles?
11. How do you decide when to use subscription boundaries versus resource group boundaries?
12. How would you design cross-subscription access for shared services without creating long-term security debt?
13. How do you secure and govern service-to-service authentication across Azure workloads?
14. How would you approach Entra ID conditional access and workload access without breaking automation?
15. What would you standardize in an enterprise Azure landing zone blueprint?

---
5) AKS production scenarios and troubleshooting
1. An AKS cluster is healthy, but pods cannot reach external services after a node image upgrade. What would you inspect first?
2. A cluster is failing to scale during a traffic spike even though the cluster autoscaler is enabled. How would you troubleshoot it?
3. What are the trade-offs between a single large AKS cluster and multiple smaller clusters for production?
4. How would you diagnose image pull failures in AKS when some namespaces work and others fail?
5. How do you handle AKS upgrades safely in a production environment with strict uptime requirements?
6. A private AKS cluster cannot be accessed by engineers after a network change. How do you restore access safely?
7. How do you design ingress for AKS using NGINX, Application Gateway Ingress Controller, Front Door, or a service mesh?
8. What would you check when pods are running but services are timing out intermittently inside the cluster?
9. How do you handle workload identity in AKS, and what failures do you expect during rollout?
10. How do you troubleshoot node pool provisioning failures in AKS?
11. What are the practical differences between Azure CNI Overlay and classic Azure CNI for high-scale clusters?
12. How would you detect and mitigate noisy-neighbor issues in a shared AKS cluster?
13. How do you design for secure AKS secrets management without overexposing Key Vault?
14. How would you troubleshoot an AKS cluster where autoscaling is working, but application latency is still rising?
15. What are the key observability signals you would baseline for an AKS platform before go-live?

---
6) Compute and serverless platform scenarios (VMs, VMSS, App Service, Functions, Container Apps, ACI)
1. When would you choose VM/VMSS over PaaS for a production Azure workload, and what operational cost do you accept?
2. A VMSS deployment fails halfway through a scale-out. What would you check first?
3. How would you troubleshoot VM extension failures during provisioning or reimage events?
4. A VM boots but the application never becomes healthy after a deployment. What telemetry would you correlate?
5. How do you handle managed identity and IMDS failures on Azure VMs or VMSS?
6. How would you diagnose disk latency or IOPS throttling on Azure VMs?
7. What are the trade-offs between App Service and Container Apps for production microservices?
8. An App Service app is healthy in staging but intermittently fails in production after a slot swap. How do you troubleshoot?
9. How would you investigate a sudden spike in App Service cold starts or request latency?
10. What would you check when Azure Functions scale unpredictably or begin timing out?
11. How do you decide whether a workload should run on Container Apps instead of AKS or App Service?
12. What failure modes do you expect with Azure Container Instances in production-like usage?
13. How would you design zero-downtime deployments for App Service, Functions, and VM-based workloads?
14. How would you detect whether a platform issue is caused by compute SKU capacity, quota, or Azure service health?
15. How do you decide which compute workloads should be containerized versus left on VMs during modernization?

---
7) Storage and data platform architecture and failure scenarios
1. How do you choose between Blob Storage, Managed Disks, Azure Files, and Azure NetApp Files for different workload patterns?
2. A Blob Storage-based application suddenly sees access denied errors after a network change. What would you check first?
3. How would you design Blob Storage for high durability, secure access, lifecycle management, and event-driven processing?
4. What are the main failure scenarios in Azure SQL Database during production traffic spikes?
5. How would you decide between Azure SQL Database and SQL Managed Instance for a migration?
6. A SQL Managed Instance workload begins failing after failover. What would you investigate?
7. How would you design for geo-replication and failover for Azure SQL workloads?
8. What would you check when Cosmos DB starts returning 429 throttling errors?
9. How do you approach partition key design in Cosmos DB for production scale?
10. How do you think about consistency levels in Cosmos DB from an application and operations perspective?
11. When would you choose PostgreSQL Flexible Server or MySQL Flexible Server over Azure SQL?
12. A PostgreSQL Flexible Server is experiencing connection exhaustion and slow queries. What would you inspect?
13. How do you troubleshoot Azure Cache for Redis latency spikes or memory pressure?
14. How would you choose Service Bus versus Event Hubs versus Event Grid for a platform eventing design?
15. A Service Bus consumer is seeing duplicate messages and dead-letter buildup. What do you check?
16. How would you troubleshoot Event Hubs lag during a burst event?
17. How do you design Event Grid for resilient eventing when downstream handlers are unreliable?
18. How would you architect and operate Synapse from a platform perspective rather than an analytics-user perspective?
19. How do you manage encryption, key rotation, and private access across Azure storage and data services?
20. How do you design backup, restore, and retention for data services with different RPO/RTO expectations?

---
8) CI/CD, platform engineering, developer experience, and release engineering on Azure
1. How do you design a CI/CD model that supports multiple teams, multiple environments, and controlled production releases?
2. How do you create a paved-road developer experience without slowing teams down?
3. A deployment works in dev but fails in prod because of policy or permission differences. How do you prevent this class of issue?
4. How would you design release strategies for Azure workloads: blue/green, canary, ring-based, or progressive delivery?
5. How do you handle secrets, certificates, and identity in pipelines without creating long-lived credentials?
6. What does a production-grade artifact promotion model look like on Azure?
7. How would you secure and standardize build agents or hosted runners for Azure deployments?
8. How do you design pipelines to reduce blast radius when a platform change affects many services?
9. How do you validate infrastructure changes before they reach production?
10. How would you support developer self-service for common Azure patterns like creating an app, database, secrets, and alerts?
11. How do you handle versioning and deprecation of shared platform modules across many teams?
12. What would you instrument in the delivery pipeline itself to improve reliability?

---
9) Terraform / Bicep / ARM / IaC / GitOps / platform automation deep dive
1. When would you choose Terraform, Bicep, or ARM for enterprise Azure platform automation?
2. How do you prevent drift between deployed Azure resources and source control?
3. A Terraform apply failed halfway through and left resources in a partially created state. How do you recover safely?
4. How do you structure Terraform state for large Azure enterprises with multiple subscriptions and teams?
5. How do you manage remote state security and access across teams?
6. How would you implement reusable Bicep modules for landing zones and platform services?
7. How do you safely roll out policy, networking, or identity changes through IaC?
8. How do you detect and handle policy-denied deployments in an automated pipeline?
9. How would you set up GitOps for AKS while maintaining governance and controlled promotion?
10. How do you version and deprecate infrastructure modules without breaking consumers?
11. What is your approach to handling environment-specific values without scattering configuration logic everywhere?
12. How would you automate landing zone provisioning from a new tenant or subscription request to a ready-to-use environment?
13. How do you validate IaC changes for networking, identity, and security before applying them?
14. How do you keep automation idempotent and safe in the face of partial failures and retries?

---
10) Security engineering and compliance on Azure
1. How do you design defense in depth for Azure workloads across identity, network, compute, data, and operations?
2. How would you operationalize Defender for Cloud across multiple subscriptions and teams?
3. How do you use Microsoft Sentinel in a platform environment without overwhelming the security team with noise?
4. How do you secure secrets, certificates, and managed identities in a large Azure estate?
5. A workload suddenly loses access to Key Vault in production. What would you check first?
6. How do you design a secure ingress/egress model for internet-facing Azure applications?
7. How would you implement Azure Policy to satisfy compliance while enabling developer velocity?
8. How do you approach RBAC review and privilege creep in Azure at scale?
9. How do you secure AKS beyond default settings?
10. How do you design for secure-by-default private networking with necessary operational access?
11. How do you protect against accidental or malicious data exposure in Azure Storage and databases?
12. How would you investigate a suspected credential leak or unauthorized access in Azure?
13. How do you design auditability and evidence collection for regulated Azure environments?

---
11) Observability, SRE, reliability engineering, incidents, and postmortems
1. What does a production observability strategy look like on Azure for platform and application layers?
2. How do you define SLOs and error budgets for platform services on Azure?
3. An alert fires repeatedly but operators ignore it. How do you fix the alerting system?
4. How do you troubleshoot missing telemetry in Azure Monitor or Log Analytics?
5. How would you correlate an incident across AKS, App Gateway, Azure Firewall, and the application layer?
6. What are your first steps in a Sev-1 incident on Azure?
7. How do you distinguish symptom from root cause during a complex cloud incident?
8. How do you design postmortems that lead to real prevention rather than just documentation?
9. How do you build actionable dashboards for platform health rather than vanity metrics?
10. How would you detect SLO burn-rate issues before customers complain?
11. How do you instrument platform services to make future incidents easier to diagnose?
12. How do you define operational readiness for a new Azure platform or service?

---
12) HA/DR, resilience, backup, restore, and multi-region strategy
1. How do you design a multi-region Azure architecture for active-active, active-passive, or pilot-light patterns?
2. What is your approach to choosing paired regions and regional topology on Azure?
3. How do you test disaster recovery without creating unnecessary risk?
4. How would you design backup and restore for VMs, databases, storage, and AKS-related state?
5. A region outage occurs, but the app depends on several cross-region services. How do you sequence failover?
6. How do you decide whether to replicate data synchronously or asynchronously?
7. What are the failure modes in Azure Traffic Manager, Front Door, and regional backends during failover?
8. How would you handle DNS during regional failover and failback?
9. How do you design for partial dependency failure, not just total region loss?
10. How do you validate that backups are actually restorable and consistent?
11. How do you manage stateful services in a multi-region design without overengineering?
12. How do you ensure platform components like Key Vault, DNS, monitoring, and identity are resilient enough for DR?

---
13) Cost optimization / FinOps scenario questions
1. A product’s Azure bill suddenly doubled. How would you investigate the cost drivers first?
2. How would you control cost for AKS without harming reliability?
3. A NAT Gateway bill is unexpectedly high. What would you check?
4. How do you control Log Analytics ingestion costs while preserving useful observability?
5. Cosmos DB spend is climbing quickly. How would you identify the cause?
6. A Functions app has become expensive under traffic spikes. How would you investigate whether the cost is legitimate or due to misconfiguration?
7. How do you decide when to reserve capacity, use savings options, or accept on-demand pricing?
8. How would you set chargeback or showback for shared platform services?
9. What cost controls would you put in place for development and non-production environments?
10. How do you prevent cost from becoming a surprise during a high-traffic event or incident?

---
14) Migration, modernization, and legacy transformation to Azure
1. How would you assess whether to rehost, replatform, refactor, or retire a legacy workload moving to Azure?
2. How would you migrate a legacy VM-based application with minimal downtime?
3. What would you do differently when migrating a monolith versus a microservice estate to Azure?
4. How do you modernize a legacy app that uses shared file systems, hardcoded IPs, and batch jobs?
5. How would you handle database migration when the source platform has features not directly available in Azure?
6. How do you migrate an app into a private Azure network without breaking integrations?
7. How do you design coexistence between on-prem and Azure during a long migration program?
8. What is your approach to data migration with strict downtime constraints and rollback needs?
9. How would you modernize a system that currently runs on a single VM into a more resilient Azure-native architecture?
10. How do you avoid creating a cloud-hosted legacy after migration?

---
15) Leadership, ownership, prioritization, and cross-functional influence
1. Tell me about a time you had to drive a platform decision that product teams initially resisted.
2. How do you prioritize platform work when product teams want features and the platform needs reliability work?
3. How do you communicate technical risk to executives without over-technicalizing the message?
4. Describe how you would lead an architectural review for a high-risk Azure change.
5. How do you create ownership when a platform spans multiple teams with overlapping responsibilities?
6. How do you handle situations where security, operations, and engineering disagree on an Azure design?
7. What does good judgment look like in a senior Azure architect during a production incident?
8. How do you mentor teams to improve their Azure operational maturity?
9. How do you decide when to standardize versus when to allow exceptions?
10. How do you measure whether a platform team is actually enabling the business?

---
16) Bar-raiser troubleshooting war-room scenarios
1. AKS incident: sudden pod crash loop after a node image upgrade, plus image pull failures in a subset of namespaces. What do you check first?
2. Networking incident: intermittent 502/504 errors only when traffic goes through Front Door -> Application Gateway -> AKS. How do you break down the path?
3. Identity incident: a large set of workloads lost access to Key Vault and Storage after a policy rollout. How do you contain and diagnose it?
4. Data incident: Azure SQL latency spikes, connection storms, and partial application timeouts after failover. What do you inspect in order?
5. Cosmos incident: RU throttling is rising and one partition is hot. What immediate and long-term actions do you take?
6. Functions incident: cost is exploding and logs show repeated retries and duplicate processing. How do you prove where the loop is?
7. Platform rollout incident: Terraform applied successfully, but some resources are inaccessible because of policy and DNS interactions. How do you untangle it?
8. Hybrid connectivity incident: ExpressRoute is up, but private endpoint traffic fails from on-prem only. What hypotheses do you test first?
9. Observability incident: your dashboards are green, but customers are reporting outages. What do you suspect and how do you validate it?
10. Cost incident: Log Analytics ingestion and egress bills spike after an incident. How do you separate necessary telemetry from waste?

---
17) Mock panel round questions grouped by round
Architect round
1. Design a secure, scalable Azure platform for a product company with multiple teams, private networking, and global users.
2. How would you decide the right balance between centralized platform services and team autonomy?
3. What is your reference Azure architecture for a regulated SaaS product?
4. How would you evolve a single-region platform into multi-region without rewriting everything?
5. How do you rationalize service choices across AKS, App Service, Functions, Container Apps, and VMs?

Platform / DevOps round
1. How would you implement a self-service Azure provisioning experience for product teams?
2. How do you design CI/CD for safe, repeatable, environment-aware releases on Azure?
3. How would you manage IaC modules, drift, and policy enforcement across many subscriptions?
4. How do you build platform guardrails that do not block development velocity?
5. How would you standardize observability and release processes across heterogeneous workloads?

SRE / operations round
1. How would you define SLOs and operational readiness for Azure platform services?
2. What would your incident response model look like for a multi-region Azure production system?
3. How do you troubleshoot a degradation that spans compute, network, and managed services?
4. How do you design alerting that is meaningful, actionable, and not noisy?
5. How do you lead a postmortem so it produces systemic reliability improvements?

Security / governance round
1. How do you enforce least privilege, policy compliance, and private access across Azure at scale?
2. How do you secure workload identity, secrets, and cross-service access in Azure?
3. How would you operationalize Defender for Cloud and Sentinel for a large enterprise platform?
4. How do you manage exceptions without weakening the overall security posture?
5. What are your top Azure security failure patterns and how do you prevent them?

Hiring manager round
1. Why do you want a senior platform role that blends architecture, DevOps, and SRE responsibilities?
2. Tell me about a time you owned a difficult platform problem end to end.
3. How do you influence teams when you do not have formal authority?
4. How do you balance technical excellence with delivery pressure?
5. What would your first 90 days look like in a product company running Azure at scale?