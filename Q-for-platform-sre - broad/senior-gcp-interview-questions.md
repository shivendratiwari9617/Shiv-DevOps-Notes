GCP Senior Interview Question Bank
1. GCP Fundamentals and Architecture
1. How do you explain the Google Cloud resource hierarchy, and when would you use organization, folder, and project boundaries in a product-based company?
2. What are the core design principles you follow when building a cloud-native platform on GCP for multiple product teams?
3. How do you decide whether a workload should be designed as a managed service, custom service, or hybrid architecture on GCP?
4. What is your approach to designing for least privilege, isolation, and blast-radius reduction across GCP environments?
5. How do you model environments such as dev, test, stage, and prod in GCP for long-term scalability?
6. What are the key differences between regional and multi-regional service design on GCP, and how do you choose between them?
7. How do you evaluate managed GCP services versus self-managed infrastructure for platform reliability and operational overhead?
8. What are the most important GCP-native capabilities you rely on when designing a modern platform for product engineering teams?
9. How do you think about tenancy models on GCP for a product company with multiple applications and internal teams?
10. What architecture review questions do you ask before approving a new GCP workload?

2. Organization, Folders, Projects, IAM, and Governance
1. How do you design an enterprise GCP landing zone from scratch?
2. How do you structure folders and projects to balance security, autonomy, and operational simplicity?
3. What is your approach to IAM strategy across organization, folder, project, and resource levels?
4. How do you prevent privilege creep in a fast-moving engineering organization?
5. When would you use predefined roles versus custom roles, and how do you govern custom role sprawl?
6. How do you handle service accounts for applications, automation, and human access differently?
7. What is your strategy for workload identity and service account impersonation in GCP?
8. How do you manage cross-project and cross-folder access while maintaining auditability?
9. What organization policies have you seen as essential in a mature GCP environment?
10. How do you enforce guardrails without blocking developer velocity?
11. How do you design IAM for shared platform services used by multiple teams?
12. How do you approach break-glass access for production support in GCP?
13. What is your process for access reviews, role recertification, and periodic permission cleanup?
14. How do you control project creation, billing association, and lifecycle governance?
15. How do you audit and detect risky IAM bindings across an organization?
16. Scenario: A developer requests broad Editor access to unblock a production issue. How do you respond and design a safer alternative?
17. Scenario: Two teams need access to the same data platform but should not see each other’s resources. How would you structure access and governance?
18. What governance controls would you put in place before enabling self-service project provisioning?

3. VPC, Shared VPC, Connectivity, DNS, Load Balancing, CDN, Cloud Armor
1. How do you design a hub-and-spoke network architecture on GCP?
2. When would you choose Shared VPC, and what problems does it solve in a platform organization?
3. What are the trade-offs between Shared VPC and per-project isolated networking?
4. How do you plan IP address space for a large GCP organization to avoid future overlap issues?
5. How do you handle peering, private access, and service connectivity between multiple projects and environments?
6. What is your approach to hybrid connectivity on GCP using Cloud VPN versus Cloud Interconnect?
7. How do you decide between HA VPN and Interconnect for enterprise connectivity?
8. How do you design for resilient DNS in GCP across public and private zones?
9. How do you troubleshoot service discovery issues in private GCP environments?
10. What types of load balancers do you use on GCP, and how do you choose among them?
11. How do you design global traffic management for a user-facing service on GCP?
12. How do you use Cloud CDN effectively, and what content or traffic patterns benefit most?
13. How do you use Cloud Armor to protect public applications, APIs, and ingress points?
14. What is your strategy for rate limiting, WAF policies, and bot mitigation on GCP?
15. How do you design network segmentation for secure access to internal services?
16. How do you approach firewall rules, hierarchical firewall policies, and network tags in a managed way?
17. What are the common pitfalls when teams build GCP network architectures too quickly?
18. Scenario: An internal service is reachable from one GKE cluster but not another. How would you diagnose the network path?
19. Scenario: Public latency spikes in one geography. How would you reason about load balancing, CDN, DNS, and backend health?
20. Scenario: A security team wants all outbound traffic inspected. How would you redesign egress on GCP?

4. Compute Engine, MIGs, Autoscaling, Instance Templates, Sole-Tenant, Reservations
1. When would you choose Compute Engine over GKE or Cloud Run for a workload?
2. How do you design immutable infrastructure on Compute Engine?
3. What is the role of instance templates in a mature Compute Engine platform?
4. How do managed instance groups support reliability and deployment workflows?
5. How do you design autoscaling policies for CPU-bound, memory-bound, and queue-driven workloads?
6. What are the key considerations when using regional MIGs versus zonal MIGs?
7. How do you handle stateful workloads on Compute Engine when scaling and replacing instances?
8. When are sole-tenant nodes appropriate, and what operational constraints do they introduce?
9. How do reservations and committed use considerations influence capacity planning?
10. How do you plan for maintenance events, live migration, and reboot behavior for Compute Engine workloads?
11. How do you manage boot disks, attached disks, and image lifecycle for fleets of VMs?
12. What logging, patching, and security hardening do you expect for production Compute Engine instances?
13. How do you standardize startup scripts, metadata, and configuration injection across teams?
14. How do you design blue-green or rolling deployments for VM-based services?
15. How do you troubleshoot poor performance on Compute Engine instances at the platform layer?
16. Scenario: A MIG keeps recreating unhealthy instances. What do you inspect first?
17. Scenario: A workload needs guaranteed compute capacity in a single zone for licensing reasons. How would you design that?
18. Scenario: You need to migrate legacy VM workloads with minimal code changes. What is your migration approach?

5. GKE Architecture, Networking, Scaling, Upgrades, Security, Operations
1. What are the major architectural choices you make when designing GKE for a product company?
2. How do you decide between Autopilot and Standard GKE?
3. What are the trade-offs in using shared clusters versus dedicated clusters per team or application?
4. How do you design cluster and namespace boundaries for multi-tenancy?
5. What is your approach to node pool design, taints, tolerations, and workload isolation?
6. How do you design GKE networking for high-scale, low-latency services?
7. How do you think about pod IPs, service IPs, ingress, and east-west traffic on GKE?
8. What is your strategy for cluster autoscaler, HPA, VPA, and custom autoscaling signals?
9. How do you prepare for GKE upgrades without disrupting production workloads?
10. What practices do you use to minimize downtime during node pool upgrades and control plane changes?
11. How do you handle container security, image policy, and runtime hardening in GKE?
12. What is your strategy for secrets, config, and identity on GKE?
13. How do Workload Identity and service account mapping improve security posture?
14. What is your approach to ingress on GKE using internal and external load balancers?
15. How do you manage multi-cluster architectures and when do you need them?
16. How do you design for observability and troubleshooting in large GKE fleets?
17. How do you handle persistent storage and stateful workloads on GKE?
18. What is your process for capacity planning and cost control in GKE?
19. How do you validate cluster policy compliance before workloads reach production?
20. Scenario: A production deployment causes node pressure and pod evictions. How do you analyze and prevent this pattern?
21. Scenario: One namespace is consuming disproportionate resources in a shared cluster. How would you protect the platform?
22. Scenario: A GKE upgrade is blocked by incompatible workloads. How do you identify and remediate the blockers?
23. Scenario: A service is healthy in one cluster but failing in another with the same manifests. What differences would you check?

6. Cloud Run, App Engine, Functions, Serverless Design on GCP
1. When would you choose Cloud Run over GKE or Compute Engine?
2. What are the design constraints that matter most for Cloud Run-based services?
3. How do you handle cold starts, concurrency, and autoscaling in Cloud Run?
4. How do you design request handling and background processing for serverless workloads on GCP?
5. What is your approach to deploying APIs and internal services with Cloud Run?
6. How do you secure Cloud Run services for internal versus external access?
7. How do you use Cloud Run jobs, and when are they more appropriate than services?
8. What workloads still fit App Engine well in a modern GCP platform?
9. How do you evaluate Cloud Functions for event-driven architecture, and what limitations do you account for?
10. How do you design idempotency and retry safety in serverless systems on GCP?
11. How do you manage networking for serverless workloads that need access to private resources?
12. How do you implement progressive delivery for Cloud Run services?
13. How do you monitor and troubleshoot latency or scaling issues in serverless services?
14. What patterns do you use for secrets, configuration, and environment separation in serverless deployments?
15. Scenario: A Cloud Run service performs well in staging but times out in production under burst traffic. What do you investigate?
16. Scenario: An event-driven function processes duplicate messages. How would you design for safe retries?
17. Scenario: You need a low-ops internal service with predictable traffic and strict security. Would you pick Cloud Run, GKE, or Compute Engine, and why?

7. Storage Options: Cloud Storage, Filestore, Persistent Disk, Hyperdisk, Storage Design
1. How do you select between Cloud Storage, Filestore, Persistent Disk, and Hyperdisk for different workloads?
2. What storage patterns are best suited for application binaries, logs, backups, and user uploads on GCP?
3. How do you design for durability and availability when storing critical business data?
4. What are the performance considerations when using block storage for production databases or stateful services?
5. How do you think about zonal versus regional storage options for reliability?
6. How do you manage snapshotting, backup retention, and restore testing for disk-based workloads?
7. When would Filestore be appropriate, and what constraints does it introduce?
8. How do you secure sensitive data stored in Cloud Storage and other GCP storage services?
9. How do you handle lifecycle policies, object versioning, and archival storage tiers?
10. What is your approach to data replication, retention, and deletion policies?
11. How do you design storage for microservices that need shared access versus isolated ownership?
12. How do you detect and prevent accidental data exposure in storage systems?
13. How do you optimize storage cost without compromising recovery objectives?
14. Scenario: A stateful service running on VMs requires fast failover and high IOPS. What storage design would you propose?
15. Scenario: A team wants to mount shared storage across multiple pods in GKE. How would you evaluate the design?
16. Scenario: Backups exist, but restores have never been tested. How do you improve the storage recovery posture?

8. Databases and Data Services: Cloud SQL, AlloyDB, Spanner, Bigtable, Firestore, Memorystore, Pub/Sub, Dataflow, BigQuery from a Platform Perspective
1. How do you choose between Cloud SQL, AlloyDB, and Spanner for a product workload?
2. What are the architectural trade-offs between relational, distributed relational, NoSQL, and analytical storage on GCP?
3. When would you use Bigtable, and what workload characteristics make it the right choice?
4. How do you determine whether Firestore is a fit for a given application?
5. What operational responsibilities remain with the platform team even when using managed databases on GCP?
6. How do you approach high availability and failover for Cloud SQL-based services?
7. What are the scaling and schema design considerations for AlloyDB?
8. How do you design for Spanner’s strengths and limitations in a globally distributed system?
9. How do you think about consistency, transaction boundaries, and latency in distributed data stores?
10. What is your strategy for connection pooling and database access patterns in cloud-native systems?
11. How do you manage backups, replicas, maintenance windows, and upgrades for managed databases?
12. How do you choose between Memorystore options and where does in-memory caching fit in architecture?
13. How do you design event-driven systems with Pub/Sub for reliability and replayability?
14. How do you handle ordering, duplicates, retries, and dead-lettering in Pub/Sub-based pipelines?
15. How do you position Dataflow in a platform architecture, and what operational concerns matter most?
16. How do you govern BigQuery usage across teams from a platform, security, and cost perspective?
17. How do you manage data access controls, partitioning, clustering, and dataset ownership in BigQuery?
18. What are the platform implications of using multiple data services in the same product ecosystem?
19. How do you monitor database saturation, replication lag, and service degradation across managed data systems?
20. Scenario: A spike in traffic causes application timeouts, but the database itself is healthy. What platform-layer causes would you investigate?
21. Scenario: A Pub/Sub consumer is falling behind after a deployment. How would you analyze the issue?
22. Scenario: A product team wants a globally available transactional datastore. How would you evaluate Spanner versus alternatives?
23. Scenario: Analysts want direct access to production data. How would you design safer data exposure patterns?

9. CI/CD and Developer Platform Engineering on GCP: Cloud Build, Artifact Registry, Deployment Strategies, GitOps, Policy Checks
1. How do you design a secure, scalable CI/CD platform on GCP for many product teams?
2. How do you structure build, test, scan, and deploy stages in Cloud Build or equivalent GCP-based pipelines?
3. What is your strategy for artifact versioning, promotion, and provenance in Artifact Registry?
4. How do you ensure pipeline credentials and service account usage are secure and auditable?
5. How do you implement environment promotion without rebuilding artifacts?
6. How do you design deployment strategies such as rolling, blue-green, canary, and progressive delivery on GCP?
7. What guardrails do you put in place before a workload can reach production?
8. How do you integrate policy checks, security scans, and compliance checks into delivery pipelines?
9. What is your approach to GitOps on GKE or other GCP workloads?
10. How do you handle secrets and configuration across CI/CD pipelines and runtime environments?
11. How do you make CI/CD self-service without losing governance?
12. How do you support multiple language stacks and deployment targets in a shared platform?
13. How do you design pipeline approvals for regulated or high-risk changes?
14. What build optimization techniques matter at scale in a product organization?
15. How do you prevent drift between source code, deployed state, and infrastructure state?
16. Scenario: A deployment succeeded, but the service is unhealthy after rollout. How do you decide whether to rollback or roll forward?
17. Scenario: One team’s pipeline is significantly slower than others. How do you investigate platform bottlenecks?
18. Scenario: Security requires that only signed images run in production. How would you enforce that on GCP?

10. IaC and Automation: Terraform for GCP, Modules, State, Drift, Promotion, Landing Zones
1. How do you design Terraform modules for reusable GCP platform building blocks?
2. What principles do you use to keep Terraform code maintainable across many teams?
3. How do you manage Terraform state for a large GCP organization?
4. What is your strategy for state isolation, locking, and workspace usage?
5. How do you handle drift detection and remediation in Terraform-managed GCP environments?
6. How do you promote infrastructure changes across dev, stage, and prod?
7. What is your approach to bootstrapping a landing zone with Terraform?
8. How do you design Terraform for network, IAM, security, and platform foundations separately?
9. How do you manage lifecycle dependencies and avoid brittle module coupling?
10. How do you handle secrets and sensitive outputs in Terraform for GCP?
11. What review and approval model do you apply to infrastructure changes?
12. How do you support both platform-managed and team-managed infrastructure safely?
13. How do you validate Terraform plans for policy compliance before apply?
14. What are the dangers of overusing Terraform for operational workflows, and how do you avoid them?
15. How do you manage provider upgrades, module versioning, and backward compatibility?
16. Scenario: A manual console change broke Terraform drift assumptions. How do you recover?
17. Scenario: A shared Terraform module is used by dozens of teams and needs a breaking change. How do you roll it out safely?
18. Scenario: A landing zone must be deployed across multiple environments with environment-specific constraints. How do you structure the codebase?

11. Security, Secrets, KMS, CMEK/CSEK, VPC Service Controls, Binary Authorization, Organization Policies
1. How do you build a layered security model for GCP across identity, network, compute, data, and runtime controls?
2. What is your approach to key management on GCP using Cloud KMS?
3. When would you use CMEK versus Google-managed encryption keys?
4. In what cases would CSEK be considered, and what trade-offs does it introduce?
5. How do you manage key rotation, access, separation of duties, and auditability?
6. How do you secure secrets in GCP, and how do you prevent secrets from leaking into CI/CD or logs?
7. What are the strengths and limitations of VPC Service Controls?
8. How do you decide where service perimeters are necessary in a product organization?
9. What is your strategy for container image trust and Binary Authorization?
10. How do organization policies help enforce security posture, and where can they create friction?
11. How do you implement security baselines for projects, folders, and workloads?
12. How do you handle security exceptions and temporary waivers in a controlled way?
13. What is your process for vulnerability management across images, workloads, and dependencies on GCP?
14. How do you design secure service-to-service communication on GCP?
15. What controls would you require for internet-facing workloads?
16. How do you detect and respond to suspicious IAM, network, or data access activity?
17. Scenario: A service account key has been exposed. What platform response steps would you expect?
18. Scenario: A team wants to access sensitive data from a notebook environment. How would you secure that workflow?
19. Scenario: A workload needs to read from a private API and a restricted storage bucket. How would you design the access model?
20. How do you communicate security trade-offs to product engineering teams without slowing delivery unnecessarily?

12. Observability and SRE: Cloud Monitoring, Logging, Error Reporting, Trace, Profiler, SLI/SLO/SLA, Alerting, Incident Response, Postmortems
1. How do you define service health for a product on GCP beyond basic infrastructure metrics?
2. What SLIs would you choose for an API-based service, and how do you avoid vanity metrics?
3. How do you translate business impact into SLOs and error budgets?
4. How do you design alerting so that it is actionable and not noisy?
5. How do you use Cloud Monitoring, Logging, Error Reporting, Trace, and Profiler together during incident response?
6. What is your approach to log structure, correlation IDs, and distributed tracing standards?
7. How do you design dashboards for developers, platform engineers, and leadership audiences?
8. How do you distinguish between symptoms and root causes in a cloud incident?
9. How do you set up on-call processes and escalation paths in a product company?
10. What should a strong incident command and communication process look like?
11. How do you write effective postmortems that lead to real platform changes?
12. How do you prevent repeat incidents after a production outage?
13. How do you monitor GCP-native managed services where you have limited underlying access?
14. How do you instrument GKE, Cloud Run, and VM-based workloads consistently?
15. What is your approach to synthetic monitoring and user-centric monitoring on GCP?
16. How do you validate that alerts reflect customer impact rather than internal noise?
17. Scenario: A service shows no error logs, but latency is climbing steadily. How do you investigate?
18. Scenario: A new release reduced error rates but increased tail latency. How would you analyze the trade-off?
19. Scenario: An incident affects multiple microservices. How do you determine ownership and coordinate response?
20. Scenario: Postmortems are being written but not driving change. How would you improve the process?

13. Reliability, DR, HA, Multi-Zone, Multi-Region, Backup/Restore, Chaos/Failure Scenarios
1. How do you define HA and DR requirements for different types of product workloads on GCP?
2. What factors drive your choice between multi-zone and multi-region architectures?
3. How do you set RTO and RPO targets, and how do they influence platform design?
4. What is your strategy for backup, restore, and validation testing across compute, storage, and databases?
5. How do you design failover for user-facing services on GCP?
6. How do you think about active-active versus active-passive topologies?
7. What are the failure modes you plan for at the zone, region, and dependency level?
8. How do you reduce blast radius when a shared platform component fails?
9. What is your approach to chaos testing or controlled failure injection on GCP?
10. How do you validate the resilience of managed services you depend on?
11. How do you manage dependencies on DNS, IAM, network connectivity, and third-party services during outages?
12. What operational runbooks should exist for disaster recovery?
13. How do you balance cost against resilience requirements?
14. How do you ensure backups are restorable and not just retained?
15. What is your strategy for regional evacuations or partial regional failover?
16. Scenario: A region becomes unavailable. What are the first platform actions you expect?
17. Scenario: A database failover works, but the application still cannot serve traffic. What else could be broken?
18. Scenario: A backup restore succeeds technically but the application data is unusable. How would you investigate?
19. Scenario: A shared dependency outage affects multiple teams. How do you coordinate recovery and communication?
20. How do you decide which failure scenarios must be tested regularly versus documented only?

14. Cost Optimization and FinOps on GCP
1. How do you build cost accountability into a shared GCP platform?
2. What tagging, labeling, and chargeback/showback practices do you recommend?
3. How do you identify the biggest cost drivers in a GCP environment?
4. What techniques do you use to right-size compute, storage, and managed services?
5. How do you evaluate committed use discounts, sustained use, and reservations from a platform perspective?
6. How do you control spend in GKE-heavy environments?
7. What cost controls do you apply to logging, monitoring, and observability data?
8. How do you reduce costs without degrading reliability or developer experience?
9. How do you design guardrails to prevent runaway ephemeral environments or unused resources?
10. How do you work with product teams to make cost visible and actionable?
11. What is your strategy for cost-aware architecture reviews?
12. How do you identify and eliminate orphaned resources and shadow IT on GCP?
13. How do you optimize Cloud Storage lifecycle and backup retention costs?
14. How do you manage costs for data services, especially BigQuery and Pub/Sub-heavy systems?
15. Scenario: A team’s monthly GCP bill doubled after a release. How would you investigate and contain it?
16. Scenario: Leadership wants a 20% infrastructure cost reduction without harming reliability. How would you approach that?
17. Scenario: A shared platform service becomes expensive because one tenant grows rapidly. What governance model would you recommend?

15. Migration and Modernization to GCP
1. How do you assess a legacy workload for migration to GCP?
2. How do you choose among rehost, replatform, refactor, retire, and replace strategies?
3. What is your approach to migration sequencing across applications, data, and dependencies?
4. How do you handle identity, networking, and security during a phased migration?
5. How do you plan parallel run, cutover, and rollback for critical systems?
6. How do you migrate databases to GCP with minimal downtime?
7. How do you modernize VM-based workloads into GKE or Cloud Run when appropriate?
8. What are the biggest risks in cloud migration programs, and how do you mitigate them?
9. How do you validate application performance after migration?
10. How do you handle data gravity and integration with on-prem systems during transition?
11. How do you decide whether to modernize during migration or after migration?
12. What is your approach to landing zone readiness before moving production workloads?
13. How do you communicate migration trade-offs to product and business stakeholders?
14. Scenario: A legacy application depends on static IPs, shared files, and cron jobs. How would you migrate it to GCP?
15. Scenario: A monolith is too risky to refactor immediately. What migration path would you recommend?
16. Scenario: A business-critical system must be migrated with near-zero downtime. How would you structure the plan?
17. Scenario: After migration, latency is higher than expected. What platform causes would you inspect?

16. Real-World Troubleshooting and Scenario-Based Deep-Dive Questions
1. A production service on GKE intermittently returns 502 errors. What are the first layers you inspect?
2. A Cloud Run service works from one region but fails from another. How would you debug that?
3. A VM-based application cannot reach a private Google API endpoint. What could be misconfigured?
4. A pipeline succeeds but the deployed artifact is not the one you expected. How do you trace provenance?
5. A service account suddenly loses access to a critical resource. How do you investigate the cause?
6. A GKE pod is stuck in CrashLoopBackOff only in production. What would you check first?
7. A database connection pool appears exhausted during moderate traffic. How do you determine whether the issue is app, platform, or database-related?
8. A Pub/Sub subscription backlog grows even though consumers are healthy. What could explain this?
9. A Cloud Storage operation becomes slow or fails under load. What causes would you consider?
10. A GCP quota exhaustion incident disrupts production. How do you prevent recurrence?
11. A platform change to IAM or networking breaks one team but not others. How do you reduce hidden dependencies?
12. A deployment works in one cluster but not another even though the manifests are identical. What else might differ?
13. A service is seeing high costs but low traffic. How do you locate the waste?
14. A shared VPC environment has intermittent connectivity issues. What diagnostics do you run?
15. A major incident requires temporary exceptions to policy. How do you balance urgency and control?
16. A workload depends on a third-party API that is now rate limiting requests. How would you redesign the platform behavior?
17. Scenario: An internal platform service becomes the bottleneck for every team. How do you prioritize and remediate?
18. Scenario: A new security control blocks several production deployments. How do you roll out the control safely?
19. Scenario: An application is healthy, but end users report failures. What platform and edge layers do you investigate?
20. Scenario: You inherit a GCP environment with poor documentation, inconsistent IAM, and no clear ownership. What is your first 30/60/90-day plan?

17. Leadership, Architecture Reviews, Stakeholder Management, and Product-Company Ownership
1. How do you run an architecture review for a new GCP service without becoming a bottleneck?
2. What standards do you expect from a senior architect when reviewing a platform design?
3. How do you balance speed of delivery against platform consistency and governance?
4. How do you influence application teams to adopt platform standards they did not create?
5. How do you handle disagreements with product engineers who prefer a faster but riskier design?
6. How do you communicate platform trade-offs to non-technical stakeholders such as product managers and leadership?
7. How do you decide which platform capabilities should be centralized versus delegated to teams?
8. How do you measure whether the platform is helping or slowing the product organization?
9. What does good ownership look like for a senior cloud engineer in a product company?
10. How do you ensure documentation, runbooks, and operational readiness do not get ignored?
11. How do you mentor junior engineers while still owning high-severity operational work?
12. How do you handle incidents that expose gaps in architecture, process, or ownership?
13. How do you prioritize competing asks from security, product, operations, and finance?
14. How do you drive standardization across multiple teams without reducing innovation?
15. What does “you build it, you run it” mean in a GCP-based product organization, and where does the platform team fit?
16. How do you evaluate whether a candidate can act as a true platform owner rather than just an executor?
17. Scenario: Leadership wants a faster launch, but your review shows the design has major reliability gaps. How do you respond?
18. Scenario: Two product teams want conflicting platform requirements. How do you mediate and decide?
19. Scenario: The platform team is overwhelmed with support requests and cannot invest in roadmap work. How would you change the operating model?
20. What behaviors distinguish a strong senior GCP architect, platform engineer, DevOps engineer, or SRE in a product-based company?

18. Final Seniority-Caliber Closing Questions
1. If you were building a new GCP platform for a product company from zero, what would you implement in the first 90 days?
2. What are the three most common mistakes you see teams make on GCP at scale?
3. Which GCP services or patterns do you consider overused, and under what conditions do they become risky?
4. What platform decision have you changed your mind about after operating GCP systems at scale?
5. What would you standardize immediately, and what would you intentionally leave flexible for product teams?
6. What is the most difficult GCP production issue you have solved, and what did it teach you about platform design?
7. What architecture trade-off do you find hardest to explain to business stakeholders?
8. If you joined this company, what are the first questions you would ask about the existing GCP environment?
9. What would you look for to determine whether a GCP platform is truly production-ready?
10. How do you know when a cloud platform has matured enough to shift focus from building to optimizing?