Senior AWS Cloud Architect / Platform / DevOps / SRE Interview Question Set
1. Must-know senior-level screening questions
1. How would you describe the difference between building for availability and building for resilience on AWS?
2. How would you design a production-ready AWS landing zone for a product-based company with multiple engineering teams?
3. How do you decide when to use multi-account, multi-VPC, or shared services patterns on AWS?
4. What trade-offs would you consider between EKS, ECS, and Lambda for a new platform?
5. How would you define a secure and scalable IAM model for a large AWS organization?
6. What are the first things you would check when an AWS workload is slow, failing, or intermittently unavailable?
7. How would you approach production incident response in AWS when the root cause is not immediately obvious?
8. What signals would you use to determine whether a system is operationally mature?
9. How would you evaluate whether a service should be internet-facing, private, or accessible only via internal connectivity?
10. What are the most common AWS platform anti-patterns you have seen in production, and how would you prevent them?
11. How would you balance velocity, governance, security, and cost in a cloud platform team?
12. What would you consider before approving a service to go live on AWS?
13. How do you decide between centralized and decentralized platform ownership?
14. What is your approach to service quotas, account limits, and capacity planning on AWS?
15. How do you ensure platform changes are safe when multiple teams depend on shared infrastructure?

2. Architecture and platform design scenarios on AWS
1. How would you design a secure, scalable AWS platform for a product company that supports dozens of autonomous development teams?
2. How would you design a shared platform that supports both internet-facing customer workloads and private internal services?
3. How would you design a high-throughput, low-latency architecture on AWS with clear blast-radius boundaries?
4. How would you design for progressive delivery across multiple AWS accounts and environments?
5. What trade-offs would you consider when deciding between centralized and per-team AWS accounts?
6. How would you design service-to-service communication in a large AWS environment without exposing everything through public endpoints?
7. How would you design a platform that supports ephemeral environments for every pull request?
8. How would you design a standard reference architecture for a new product team onboarding onto AWS?
9. How would you design for platform consistency while still allowing teams enough flexibility to move fast?
10. How would you design a multi-tenant SaaS architecture on AWS with strong tenant isolation?
11. How would you design a global application using Route 53, CloudFront, regional failover, and regional data stores?
12. How would you design a private ingress and egress strategy for internal applications on AWS?
13. How would you design an AWS platform to support both Kubernetes-native teams and non-Kubernetes teams?
14. How would you design a standard deployment model that supports blue/green, canary, and rolling releases?
15. How would you design an enterprise platform where security controls are enforced by default, not manually requested?
16. How would you design a platform that can absorb sudden traffic spikes without overprovisioning all the time?
17. How would you design a platform for regulated workloads that need auditability, encryption, and access control?
18. How would you design for fault isolation across AWS Availability Zones and Regions?
19. How would you design a centralized observability platform for logs, metrics, traces, and alerts across many AWS accounts?
20. How would you design a reusable golden-path platform that accelerates delivery but still meets governance requirements?

3. AWS networking deep dive and troubleshooting
1. How would you design a VPC strategy for a large AWS organization with many teams and many services?
2. How would you decide on subnet design, route table boundaries, and security group strategy in AWS?
3. How would you design ingress and egress for workloads that must stay private but still reach required AWS and SaaS services?
4. What trade-offs would you consider between Transit Gateway, VPC peering, and shared services VPC designs?
5. How would you use PrivateLink, VPC endpoints, and interface endpoints in a secure platform design?
6. How would you design Route 53 routing for active-active, active-passive, and failover use cases?
7. How would you troubleshoot a service that cannot reach an internal API even though the security group rules look correct?
8. What would you check first if an ALB target group is unhealthy across all targets?
9. How would you debug intermittent timeouts between services running in different VPCs or accounts?
10. What would you inspect if DNS resolution works from one subnet but not another?
11. How would you investigate asymmetric routing or unexpected return-path issues in AWS networking?
12. How would you troubleshoot intermittent latency introduced by NAT gateways, firewalls, or proxy layers?
13. How would you design outbound internet access for production workloads while minimizing exposure and controlling egress?
14. How would you design a network architecture that supports both Kubernetes node traffic and application traffic cleanly?
15. What trade-offs would you consider between ALB and NLB for different traffic patterns?
16. How would you use CloudFront, WAF, Shield, and regional load balancers together for internet-facing applications?
17. How would you troubleshoot cross-account connectivity issues over Transit Gateway?
18. How would you identify whether a networking issue is caused by DNS, routing, security groups, NACLs, or application behavior?
19. How would you validate that network segmentation is actually working as intended in production?
20. How would you design and troubleshoot hybrid connectivity between on-premises and AWS for production workloads?

4. IAM, Organizations, SCPs, multi-account governance, and landing zone strategy
1. How would you design AWS Organizations for a product-based company with engineering, security, shared services, and sandbox accounts?
2. How would you structure a landing zone so it scales without becoming a bottleneck for delivery teams?
3. How would you decide what belongs in the management account and what should be isolated elsewhere?
4. How would you design SCPs to enforce guardrails without blocking legitimate engineering workflows?
5. How would you separate identity, permission boundaries, and trust policies in a large AWS environment?
6. How would you design IAM roles for human access, workload access, and cross-account automation?
7. What trade-offs would you consider between IAM users, roles, federated access, and break-glass accounts?
8. How would you implement least privilege without making day-to-day operations impractical?
9. How would you troubleshoot a deployment failure caused by IAM, SCPs, or permission boundaries?
10. How would you design access to production for developers, SREs, and security teams?
11. How would you use Control Tower or an equivalent landing zone pattern to standardize account provisioning?
12. How would you enforce mandatory controls such as logging, encryption, and config baselines across accounts?
13. How would you manage cross-account access for CI/CD pipelines and platform automation?
14. How would you design an AWS account vending process with strong governance and minimal friction?
15. How would you evaluate whether a new account should be created or whether an existing account should be reused?
16. How would you design identity federation for a large organization with multiple teams and roles?
17. How would you detect and prevent privilege escalation paths in AWS?
18. How would you handle emergency access and break-glass procedures safely?
19. How would you govern exceptions to standard controls without losing auditability?
20. How would you prevent drift between your intended governance model and what is actually deployed?

5. EKS production scenarios and troubleshooting
1. How would you design a production EKS platform for multiple teams with different runtime and security requirements?
2. How would you decide on cluster-per-team, namespace-per-team, or shared-cluster patterns on EKS?
3. How would you design EKS networking, ingress, egress, and pod-to-pod communication at scale?
4. How would you secure EKS control plane access, worker nodes, and workload identity?
5. How would you implement multi-account or multi-cluster Kubernetes platform governance on AWS?
6. What would you check first when pods are running but cannot reach external APIs?
7. How would you debug intermittent pod startup failures in EKS?
8. What would you inspect when a deployment succeeds but the service remains unreachable?
9. How would you troubleshoot image pull failures, node provisioning issues, or unschedulable pods?
10. How would you design cluster autoscaling and node lifecycle management for predictable production behavior?
11. How would you choose between managed node groups, self-managed nodes, and Fargate?
12. How would you design observability for EKS at the cluster, node, namespace, and workload levels?
13. How would you handle secrets management for Kubernetes workloads on AWS?
14. How would you implement workload isolation, network policies, and pod security controls in EKS?
15. How would you design safe upgrades for Kubernetes clusters and add-ons in production?
16. How would you troubleshoot DNS issues inside EKS pods?
17. How would you reduce blast radius when a single namespace, deployment, or node group misbehaves?
18. How would you design ingress on EKS using ALB, NLB, or service mesh patterns?
19. How would you investigate request latency caused by cluster, node, CNI, or application-level issues?
20. How would you prevent recurring platform issues caused by misconfigured manifests or Helm charts?

6. Compute and serverless platform scenarios (EC2, ASG, ECS, Fargate, Lambda)
1. How would you choose between EC2, Auto Scaling Groups, ECS, Fargate, and Lambda for a new workload?
2. How would you design a scalable compute platform that supports both batch and long-running services?
3. How would you design an EC2-based platform with minimal operational burden and strong patching hygiene?
4. How would you design Auto Scaling policies for unpredictable traffic and avoid thrashing?
5. What would you check first if an ECS service is failing to place tasks or repeatedly crashing?
6. How would you design ECS/Fargate for secure private networking and predictable rollout behavior?
7. How would you decide when Lambda is the right fit and when it becomes a liability?
8. How would you design Lambda for high throughput, concurrency control, and downstream protection?
9. How would you troubleshoot Lambda timeouts, cold starts, and throttling under load?
10. How would you handle long-running workflows that do not fit neatly into Lambda execution limits?
11. How would you design compute platform observability across EC2, ECS, Fargate, and Lambda?
12. How would you use queues, retries, and circuit-breaking patterns to protect compute workloads?
13. How would you design AMI or image pipelines for secure, repeatable compute deployments?
14. How would you decide between containerizing a workload and keeping it on traditional EC2?
15. How would you prevent noisy-neighbor effects in shared compute platforms?
16. How would you design graceful shutdown, draining, and rollback behavior for compute services?
17. How would you troubleshoot a sudden surge in compute cost due to scaling or invocation patterns?
18. How would you design platform standards for health checks, logging, and metrics across compute types?
19. How would you handle blue/green deployments differently for ECS, EKS, EC2, and Lambda?
20. How would you prevent recurring issues caused by insufficient resource sizing or poor capacity assumptions?

7. Storage and data platform architecture and failure scenarios
1. How would you choose between S3, EBS, EFS, FSx, RDS, Aurora, DynamoDB, ElastiCache, Kinesis, SQS, SNS, MSK, and Redshift for different platform needs?
2. How would you design storage and data services for durability, performance, and operational simplicity?
3. How would you design a data platform on AWS that supports both transactional and analytical workloads?
4. How would you decide when to use managed databases versus self-managed data services on EC2 or containers?
5. How would you design backup, restore, and retention strategies for critical AWS data services?
6. What would you check first if an S3-backed application starts returning errors or stale data?
7. How would you troubleshoot EBS performance degradation or filesystem-related application failures?
8. How would you investigate EFS latency, mount failures, or throughput constraints?
9. How would you design RDS or Aurora for failover, read scaling, and safe maintenance windows?
10. What would you check first if a database application shows high latency but CPU looks normal?
11. How would you troubleshoot DynamoDB throttling, partition hotspots, or unexpected latency?
12. How would you design caching with ElastiCache without causing data consistency or failover problems?
13. How would you design streaming ingestion with Kinesis or MSK while protecting downstream consumers?
14. How would you handle message duplication, poison messages, and retry storms in SQS and SNS-based systems?
15. How would you design Redshift consumption patterns for platform teams without causing cost or concurrency issues?
16. How would you design data encryption, key management, and access control across storage services?
17. How would you design retention, archival, and lifecycle controls for large-scale object storage?
18. How would you troubleshoot a consumer lag problem in a streaming or queue-based system?
19. How would you prevent data loss during failover, deployment, or infrastructure replacement?
20. How would you validate that your backup and restore process actually works for production data services?

8. CI/CD, platform engineering, developer experience, and release engineering on AWS
1. How would you design a CI/CD platform on AWS that supports many teams and many deployment targets?
2. How would you standardize build, test, scan, package, and deploy workflows across teams?
3. How would you design release engineering for safe, repeatable, low-friction production deployments?
4. How would you implement progressive delivery, feature flags, and rollback strategies in AWS?
5. How would you build a self-service developer portal or internal platform to improve developer experience?
6. How would you reduce pipeline time while preserving quality gates and security checks?
7. How would you design artifact management, image promotion, and versioning across environments?
8. How would you handle secrets, credentials, and temporary access in CI/CD pipelines?
9. How would you design cross-account deployment pipelines for multiple AWS environments?
10. How would you troubleshoot a pipeline that passes locally but fails in CI or deployment?
11. How would you prevent CI/CD from becoming a single point of failure for the organization?
12. How would you design release approvals and change control for regulated production environments?
13. How would you build golden pipelines that are opinionated but extensible?
14. How would you design build isolation and supply-chain safeguards for production deployments?
15. How would you manage pipeline drift across multiple repositories and teams?
16. How would you handle database migrations as part of automated application releases?
17. How would you design deployment automation so platform changes and app changes do not interfere with each other?
18. How would you measure developer productivity and platform effectiveness without optimizing the wrong metrics?
19. How would you roll out a breaking platform change to many teams safely?
20. How would you prevent recurring deployment failures caused by inconsistent environment assumptions?

9. Terraform / CloudFormation / IaC / GitOps / platform automation deep dive
1. How would you decide between Terraform, CloudFormation, and a GitOps-driven automation model for AWS platform work?
2. How would you structure infrastructure code for reuse without creating unmanageable abstraction layers?
3. How would you organize state, modules, environments, and promotion flows in Terraform?
4. How would you prevent accidental drift between IaC and actual AWS resources?
5. How would you design safe change workflows for shared infrastructure managed through code?
6. How would you troubleshoot an IaC deployment that partially applied and left the environment inconsistent?
7. How would you handle cross-account and cross-region IaC deployments safely?
8. How would you design automated validation, policy checks, and drift detection for platform code?
9. How would you manage secrets and sensitive values in infrastructure code workflows?
10. How would you design module standards so teams can adopt them without unnecessary friction?
11. How would you prevent module sprawl, version chaos, and hidden coupling?
12. How would you implement GitOps for AWS platform resources and application delivery?
13. How would you automate account baseline provisioning and compliance controls?
14. How would you handle blue/green infrastructure changes when the underlying resource replacement is disruptive?
15. How would you structure reviews and approvals for infrastructure changes that affect production?
16. How would you deal with mutable resources that are hard to express cleanly in IaC?
17. How would you detect and recover from configuration drift caused by manual console changes?
18. How would you design automation to support ephemeral environments and automated teardown?
19. How would you manage IaC dependency ordering across shared platform components?
20. How would you ensure automation is idempotent, auditable, and safe to rerun?

10. Security engineering and compliance on AWS
1. How would you design a secure-by-default AWS environment for production workloads?
2. How would you use KMS, Secrets Manager, and IAM together in a secure platform design?
3. How would you design encryption at rest and in transit across AWS services?
4. How would you implement centralized logging and evidence collection for security and compliance?
5. How would you use GuardDuty, Security Hub, CloudTrail, AWS Config, and SSM together in an operations model?
6. How would you design a control framework that scales across many AWS accounts?
7. How would you troubleshoot a security control that is blocking a legitimate production action?
8. How would you manage secret rotation and access lifecycle for applications and operators?
9. How would you design vulnerability management for EC2, containers, and serverless workloads on AWS?
10. How would you prevent public exposure of data stores, buckets, and internal services?
11. How would you design WAF rules, rate limits, and edge protections for internet-facing applications?
12. How would you approach threat modeling for a new AWS service or platform capability?
13. How would you investigate suspicious IAM activity or unusual API calls in AWS?
14. How would you design secure break-glass access and emergency response procedures?
15. How would you enforce policy-as-code for security baselines?
16. How would you manage compliance evidence collection without slowing down engineering teams?
17. How would you secure CI/CD pipelines against credential theft, tampering, and supply-chain attacks?
18. How would you design tenant isolation and data segregation for regulated workloads?
19. How would you apply the principle of least privilege without making systems unmaintainable?
20. How would you prevent recurring security findings from reappearing in new accounts and workloads?

11. Observability, SRE, reliability engineering, incidents, and postmortems
1. How would you define SLIs, SLOs, and error budgets for an AWS-hosted product platform?
2. How would you design observability that supports both engineering debugging and operational management?
3. How would you decide what to alert on versus what to dashboard only?
4. How would you design logs, metrics, traces, and events so incidents can be diagnosed quickly?
5. How would you handle noisy alerts, missing alerts, and alert fatigue in a large platform?
6. What would you check first when a service is degraded but no single metric looks alarming?
7. How would you debug a production incident when symptoms span application, infrastructure, and AWS-managed services?
8. How would you run a war room for a high-severity AWS incident?
9. How would you use postmortems to drive lasting reliability improvements?
10. How would you prevent repeat incidents caused by the same class of failures?
11. How would you design alerting for latency, error rate, saturation, availability, and dependency health?
12. How would you differentiate between user-facing impact and internal technical symptoms?
13. How would you create operational readiness criteria for launching a new service?
14. How would you design synthetic monitoring and health checks for critical AWS paths?
15. How would you correlate events across CloudWatch, ELB, EKS, Lambda, RDS, and application logs?
16. How would you diagnose a sudden increase in 5xx responses in a distributed AWS architecture?
17. How would you handle partial outages where only a subset of customers or requests are affected?
18. How would you design runbooks that are actually useful during an incident?
19. How would you ensure ownership, escalation, and communication are clear during incidents?
20. How would you measure whether the platform is becoming more reliable over time?

12. HA/DR, resilience, backup, restore, and multi-region strategy
1. How would you design high availability for a critical production system on AWS?
2. How would you choose between multi-AZ, multi-region active-active, and active-passive architectures?
3. How would you decide what should be replicated, what should be rebuilt, and what should fail over?
4. How would you design disaster recovery for a platform with strict RTO and RPO targets?
5. How would you test backups and restores without disrupting production?
6. How would you design application failover when dependencies are not all globally consistent?
7. How would you handle regional service degradation in a way that minimizes customer impact?
8. How would you design multi-region DNS, routing, and health-check behavior?
9. How would you manage data consistency during cross-region failover?
10. How would you determine whether a service can truly be considered resilient?
11. How would you design backup retention, immutability, and recovery access controls?
12. How would you validate that infrastructure can be recreated from code in another region?
13. How would you design dependent service failover when not every service supports multi-region parity?
14. How would you run a DR game day for a large AWS platform?
15. How would you prevent hidden single points of failure in an AWS architecture?
16. How would you design resilience for IAM, DNS, networking, and control-plane dependencies?
17. How would you handle stateful workloads differently from stateless workloads in DR planning?
18. How would you decide where to place failover complexity versus operational simplicity?
19. How would you detect that a failover mechanism is healthy before a real disaster occurs?
20. How would you prevent recurring resilience gaps discovered only during incidents or audits?

13. Cost optimization / FinOps scenario questions
1. How would you design a platform that is cost-conscious by default without sacrificing reliability?
2. How would you identify the biggest cost drivers in an AWS environment quickly?
3. How would you optimize compute, storage, network, and managed service costs in a production setting?
4. How would you balance savings plans, reserved capacity, and on-demand usage?
5. How would you decide when to optimize cost versus preserve headroom for reliability?
6. How would you detect and remediate cost spikes caused by misconfiguration or runaway automation?
7. How would you design cost allocation and tagging that engineering teams will actually follow?
8. How would you enforce budget visibility across teams, accounts, and workloads?
9. How would you optimize EKS cost without harming availability or scaling behavior?
10. How would you optimize Lambda cost while maintaining performance and operational simplicity?
11. How would you control NAT gateway, data transfer, and cross-AZ networking costs?
12. How would you approach storage lifecycle and retention policies to reduce cost safely?
13. How would you identify idle, overprovisioned, or abandoned AWS resources?
14. How would you make cost a product metric rather than just a finance conversation?
15. How would you implement alerts and guardrails for unexpected spend growth?
16. How would you compare the cost implications of ECS, EKS, Fargate, and EC2 for a given service?
17. How would you drive cost accountability without slowing teams down?
18. How would you investigate whether a cost reduction will create hidden operational risk?
19. How would you manage platform cost optimization as an ongoing program rather than a one-time cleanup?
20. How would you prevent recurring waste from experimental, ephemeral, or misclassified environments?

14. Migration, modernization, and legacy transformation to AWS
1. How would you assess a legacy application before planning migration to AWS?
2. How would you decide between rehost, replatform, refactor, retire, or replace for a legacy system?
3. How would you design a migration path that minimizes downtime and business risk?
4. How would you handle dependency discovery for a monolith moving to AWS?
5. How would you modernize a VM-based application into containers or managed services on AWS?
6. How would you migrate databases while preserving correctness and minimizing cutover risk?
7. How would you design a hybrid period where some components remain on-premises and others run on AWS?
8. How would you manage identity, networking, and security during a staged migration?
9. How would you validate performance and behavior parity after migration?
10. How would you approach decommissioning legacy infrastructure safely after migration?
11. How would you prevent migration waves from creating instability in shared AWS platforms?
12. How would you migrate a workload with hardcoded assumptions about its environment?
13. How would you use feature flags, blue/green, or shadow traffic during modernization?
14. How would you modernize a batch or ETL platform into AWS-native services?
15. How would you decide when modernization is worth the complexity compared with maintaining the current system?
16. How would you handle organizational resistance to platform standardization during migration?
17. How would you build landing-zone and governance foundations before large-scale migrations?
18. How would you run a migration factory with repeatable patterns and measurable outcomes?
19. How would you manage rollback if a migration exposes unknown application behavior?
20. How would you ensure modernization actually improves reliability, security, and cost rather than just changing technology?

15. Leadership, ownership, prioritization, and cross-functional influence
1. How would you prioritize platform work when security, reliability, developer experience, and cost all compete for attention?
2. How would you influence product and engineering leaders to invest in foundational AWS platform work?
3. How would you handle a situation where multiple teams want exceptions to platform standards?
4. How would you communicate architecture trade-offs to executives and non-experts?
5. How would you establish yourself as the owner of a critical platform without becoming a bottleneck?
6. How would you handle conflict between delivery velocity and platform guardrails?
7. How would you create alignment across security, infra, app, and data teams on AWS standards?
8. How would you build trust when introducing opinionated platform controls?
9. How would you coach engineers who are strong in development but weaker in production operations?
10. How would you define and communicate platform ownership boundaries?
11. How would you lead a difficult incident where accountability is unclear?
12. How would you decide what to standardize and what to leave flexible?
13. How would you measure platform success from a business and engineering perspective?
14. How would you handle roadmap pressure when foundational work is being deprioritized?
15. How would you influence teams to adopt better operational practices without mandate fatigue?
16. How would you handle a major platform failure that affects many teams?
17. How would you balance short-term delivery needs with long-term platform health?
18. How would you mentor junior engineers while still operating at senior architecture depth?
19. How would you make architecture decisions durable across changing team structures?
20. How would you ensure your platform strategy remains relevant as the organization scales?

16. Bar-raiser troubleshooting war-room scenarios
1. A production service on AWS is returning intermittent 5xx errors only during traffic spikes; what would you check first?
2. A deployment to EKS succeeded, but the application is unreachable from the ALB; how would you debug it?
3. A Lambda-backed API starts timing out after a seemingly harmless config change; how would you investigate?
4. An RDS-based application is slow, but CPU and memory appear normal; where would you look next?
5. A multi-account pipeline suddenly cannot assume a deployment role; how would you troubleshoot it?
6. Traffic to a private internal service fails only from one VPC and not another; what would you inspect?
7. An S3-based workload starts failing after a security hardening change; how would you isolate the cause?
8. Kubernetes pods are stuck in Pending state during a production rollout; what would you check?
9. A NAT gateway-related issue is causing timeouts for outbound dependencies; how would you confirm it?
10. A Route 53 failover configuration did not behave as expected during a regional issue; what would you examine?
11. A message queue is building backlog rapidly; how would you determine whether the problem is producer-side, consumer-side, or infrastructure-side?
12. A service on ECS is restarting repeatedly after deployment; how would you debug the failure chain?
13. A new SCP or permission boundary is blocking critical automation; how would you find the exact denied action?
14. A sudden cost spike is detected in a shared AWS account; how would you isolate the source fast?
15. A CI/CD release went out successfully but customer errors increased; how would you execute the incident response?
16. A cross-account PrivateLink-based integration stopped working after network changes; how would you troubleshoot it?
17. A DynamoDB-backed service is throttling despite apparent low utilization; what would you investigate?
18. An application appears healthy, but synthetic checks and real user behavior disagree; how would you reconcile the evidence?
19. An EKS cluster is healthy, but one namespace is experiencing widespread failures; how would you narrow it down?
20. You are in a war room with incomplete data and pressure to restore service quickly; how would you structure your next 15 minutes?

17. Mock panel round questions grouped by architect round, DevOps/platform round, SRE round, and hiring manager round
Architect round
1. How would you design an AWS platform for a product company that must support fast team onboarding, strong governance, and high availability?
2. How would you choose a reference architecture for a new global service on AWS?
3. How would you design multi-account governance while preserving developer autonomy?
4. How would you design networking, identity, and shared services to minimize coupling across teams?
5. How would you evaluate whether EKS, ECS, or Lambda should be the default for a platform?

DevOps/platform round
1. How would you design a self-service deployment platform on AWS for dozens of teams?
2. How would you standardize CI/CD pipelines while supporting different workloads and release patterns?
3. How would you implement infrastructure as code for repeatable account and environment provisioning?
4. How would you troubleshoot a failed pipeline that involves IAM, networking, and deployment automation?
5. How would you improve developer experience without weakening security or compliance?

SRE round
1. How would you define SLOs for a critical AWS service and connect them to operational actions?
2. How would you design observability and alerting so incidents are detected early and diagnosed quickly?
3. How would you handle an AWS incident that crosses compute, networking, and managed services?
4. How would you test DR readiness and validate backups, restores, and regional failover?
5. How would you prevent repeat incidents by turning postmortem findings into platform improvements?

Hiring manager round
1. Tell me about a time you had to lead a difficult AWS platform change with strong resistance from engineering teams.
2. How do you decide what problems to solve yourself versus delegate to platform or product teams?
3. How do you influence cross-functional stakeholders when architecture choices have business trade-offs?
4. How do you balance long-term platform investment against urgent delivery pressure?
5. How do you create trust and clarity when your team owns shared AWS infrastructure used by many others?