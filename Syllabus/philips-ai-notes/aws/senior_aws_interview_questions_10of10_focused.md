# Senior AWS Interview Questions — 10/10 Focused Pack

## Senior Platform Engineer

1. How would you design a self-service AWS platform where product teams can provision environments without bypassing central guardrails?
2. How would you structure AWS Organizations, Organizational Units, SCPs, accounts, IAM Identity Center, and billing boundaries for a multi-team platform?
3. How would you design a golden path for deploying services on AWS with standardized networking, identity, observability, CI/CD, and security controls?
4. How would you enforce private-by-default access to RDS, S3, ElastiCache, OpenSearch, and internal services across AWS accounts?
5. How would you design environment vending for dev, qa, stage, and prod using account factories, IaC, and guardrail automation?
6. How would you prevent infrastructure drift when teams deploy Terraform or CloudFormation independently across many AWS accounts?
7. How would you design shared services for DNS, ingress, egress, secrets, CI/CD, logging, and monitoring in AWS?
8. How would you expose platform capabilities such as EKS, ECS, Lambda, ECR, Secrets Manager, and observability through reusable templates?
9. How would you separate platform-owned and team-owned responsibilities across VPCs, IAM roles, runtime operations, and deployment pipelines?
10. How would you standardize policy-as-code, compliance evidence, and exception handling across multiple AWS accounts?
11. How would you make logging, alerting, backup, security baselines, and cost controls mandatory for new workloads in AWS?
12. How would you version and promote shared Terraform or CloudFormation modules safely when many teams consume them?
13. How would you design isolation for environments, tenants, or business units using accounts, VPCs, IAM, and encryption boundaries?
14. How would you handle runtime diversity when teams want EKS, ECS Fargate, Lambda, and EC2?
15. How would you measure whether your internal AWS platform is improving delivery speed, reliability, security, and governance?

## Senior Cloud Architect

16. Design a multi-region AWS architecture for a customer-facing product requiring low latency, strong isolation, resilience, and private service access.
17. How would you choose between EKS, ECS, Lambda, Beanstalk, and EC2 for a large product platform?
18. How would you design secure ingress using CloudFront, WAF, ALB, NLB, API Gateway, and private backends?
19. How would you design AWS networking for a product with public APIs, internal services, hybrid dependencies, and private managed services?
20. How would you design a resilient data architecture using RDS, Aurora, DynamoDB, ElastiCache, S3, SQS, SNS, EventBridge, and Redshift?
21. How would you choose between SQS, SNS, EventBridge, Kinesis, MQ, and Step Functions in an architecture round?
22. How would you design a B2B API platform in AWS with external clients, partner integrations, throttling, versioning, and auditability?
23. How would you design disaster recovery for a stateful AWS product spanning compute, messaging, storage, identity, and external dependencies?
24. How would you design a secure enterprise data platform in AWS using S3, Glue, EMR, Redshift, Lake Formation, and private networking?
25. How would you architect a regulated AWS workload where only edge services are public and all data services remain private?
26. How would you decide between active-active and active-passive region design for an AWS-hosted product?
27. How would you model failure domains in AWS across AZs, regions, control-plane dependencies, Route 53, and managed services?
28. How would you design a global SaaS platform on AWS with tenant isolation, cost efficiency, and noisy-neighbor protection?
29. How would you design secrets, keys, certificates, and workload authentication in AWS without creating long-lived credential sprawl?
30. How would you redesign a legacy EC2-heavy AWS estate into a modern platform architecture with lower operational risk?

## Senior DevOps / SRE

31. How would you design an end-to-end AWS CI/CD pipeline for microservices using Terraform or CloudFormation, GitHub Actions or CodePipeline, ECR, EKS or ECS, Secrets Manager, and progressive delivery?
32. How would you implement OIDC or short-lived role assumption in CI/CD so pipelines never depend on static AWS keys?
33. How would you design release gates using IaC plan review, policy validation, container scanning, approvals, and smoke tests?
34. How would you implement rollback for both infrastructure and application changes in AWS?
35. How would you design release strategy for a multi-service platform where schema, infrastructure, and app changes must stay coordinated?
36. How would you define SLOs, SLIs, and error-budget operations for services running on AWS?
37. How would you design observability across EKS, ECS, Lambda, EC2, API Gateway, ALB, SQS, and RDS?
38. How would you reduce alert noise across CloudWatch, X-Ray, OpenSearch, Prometheus, Grafana, and third-party observability tools?
39. How would you implement canary, blue-green, rolling, and weighted release patterns across ECS, EKS, Lambda, and EC2 workloads?
40. How would you design incident response workflows using CloudWatch, CloudTrail, Config, Health Dashboard, and deployment history?
41. How would you troubleshoot intermittent 502, 503, and 504 issues across CloudFront, ALB, API Gateway, ingress, and backend services?
42. How would you determine whether a production incident is caused by application regression, networking drift, IAM failure, scaling failure, or AWS platform event?
43. How would you structure logging accounts, observability pipelines, retention, dashboards, and alerts for a large AWS estate?
44. How would you design patching, automation, and update governance for EC2, container nodes, and hybrid-attached servers?
45. How would you integrate load testing, chaos testing, and resilience validation into AWS release processes?

## Staff-level Cloud / Platform

46. How would you define the long-term AWS platform strategy for a company moving from project-based cloud adoption to product-centric cloud engineering?
47. How would you decide which platform capabilities stay centralized and which should be delegated to product teams?
48. How would you resolve tension between security, developer velocity, and platform standardization in AWS?
49. How would you drive adoption of private networking, short-lived identities, account guardrails, and standardized observability across existing teams?
50. How would you design an AWS platform roadmap that supports current workloads and future data/AI platform growth?
51. How would you evaluate whether the company should standardize on EKS, ECS, Lambda, or a mixed runtime model?
52. How would you define reliability targets and operating models across teams, regions, and business-critical services?
53. How would you define ownership boundaries for networking, IAM, secrets, CI/CD, observability, runtime operations, and incident response?
54. How would you justify platform engineering investment to leadership using reliability, delivery speed, compliance, and cost evidence?
55. How would you redesign a fragmented AWS estate after repeated outages caused by inconsistent patterns across teams?
56. How would you identify and reduce hidden coupling across accounts, IAM roles, VPCs, pipelines, shared services, and data dependencies?
57. How would you drive organization-wide migration from manually provisioned infrastructure to governed IaC and platform templates?
58. How would you prepare an AWS platform for acquisitions, new business units, or cross-account expansion at scale?
59. How would you define technical standards in AWS that are strict enough for safety but flexible enough for product innovation?
60. How would you decide when platform exceptions are justified and how should they be reviewed and expired?

## Strong Product-Company Architecture Rounds

61. Design an AWS architecture for a high-scale e-commerce platform with global traffic, payment isolation, async order processing, analytics, and DR.
62. Design an AWS architecture for a B2B SaaS product serving enterprise tenants with strict auditability and security isolation.
63. Design an AWS architecture for a real-time notifications platform using event-driven services, low latency, and regional resilience.
64. Design an AWS architecture for an internal developer platform that provisions secure application environments on demand.
65. Design a secure GenAI architecture on AWS using Bedrock, vector/search services, private networking, and enterprise content controls.
66. Design an AWS architecture for a healthcare or financial-services workload where all managed data services are private.
67. Design an AWS architecture for a platform that must survive regional failure while preserving transactional integrity.
68. Design an AWS architecture for a multi-channel API product with external clients, internal services, throttling, and version governance.
69. Design an AWS architecture for a hybrid-first enterprise migrating from on-premises systems into AWS.
70. Design an AWS architecture for a platform supporting both synchronous APIs and high-volume asynchronous workloads.

## EKS / Containers / Runtime Deep-Dive

71. How would you design an EKS platform with private clusters, IRSA or pod identity patterns, ingress, egress control, and policy enforcement?
72. How would you choose between EKS, ECS on EC2, ECS on Fargate, and Lambda for product-company workloads?
73. How would you design node groups for system workloads, user workloads, burst traffic, GPU jobs, and isolation-sensitive services?
74. How would you secure ingress to EKS using CloudFront, WAF, ALB, NLB, ingress controllers, and private services?
75. How would you design secret access for EKS workloads using IAM roles for service accounts and Secrets Manager or Parameter Store?
76. How would you troubleshoot an EKS cluster where pods start but cannot access private AWS managed services?
77. How would you debug a failed EKS upgrade that affects only one node group and causes partial outage?
78. How would you investigate pod restart storms when node health and cluster metrics appear normal?
79. How would you design multi-cluster strategy in AWS for regional resilience, environment separation, and team autonomy?
80. How would you choose between GitOps and pipeline-driven deployments for EKS in a regulated enterprise?
81. How would you troubleshoot IAM role assumption failures in EKS workloads?
82. How would you enforce policy in EKS across image trust, namespace restrictions, ingress rules, and privileged workloads?
83. How would you handle stateful workloads on EKS when teams ask to run databases or queues inside the cluster?
84. How would you design observability for EKS covering cluster health, service health, business metrics, and cost visibility?
85. How would you design safe release patterns for latency-sensitive workloads running on EKS or ECS?

## Platform Engineering / Internal Developer Platform

86. How would you build an internal developer platform on AWS that gives teams self-service without exposing raw infrastructure complexity?
87. How would you define reusable templates for APIs, workers, data pipelines, event-driven services, and AI applications on AWS?
88. How would you expose platform services through catalogs, golden paths, or templates while preserving governance?
89. How would you design environment vending with preapproved network, IAM, monitoring, backup, and compliance controls?
90. How would you manage shared platform modules used by many teams without creating release bottlenecks?
91. How would you create platform workflows for provisioning accounts, DNS, IAM roles, secrets access, and CI/CD integration?
92. How would you measure whether your internal AWS platform improves developer productivity, reliability, and security posture?
93. How would you design exception workflows when product teams need capabilities outside the standard platform path?
94. How would you model service ownership and support boundaries between platform teams and application teams in AWS?
95. How would you evolve a centralized AWS platform toward greater team autonomy without losing guardrails?

## Security-Focused Questions

96. How would you design a zero-trust style AWS architecture using IAM, SCPs, VPC controls, private endpoints, encryption, and runtime identity?
97. How would you structure IAM roles, permission boundaries, SCPs, and break-glass access for a multi-account AWS platform?
98. How would you secure secrets, certificates, KMS keys, and encryption boundaries across many AWS accounts and workloads?
99. How would you design detection and response using CloudTrail, Config, GuardDuty, Security Hub, Detective, and centralized SIEM patterns?
100. How would you secure CI/CD pipelines in AWS so build systems cannot become a privilege-escalation path?
101. How would you design private access to S3, RDS, DynamoDB, OpenSearch, and Secrets Manager without exposing workloads to the internet?
102. How would you govern cross-account access for platform teams, security teams, and application workloads?
103. How would you design container security for EKS and ECS across image trust, runtime access, network segmentation, and secret delivery?
104. How would you investigate a suspected IAM compromise affecting multiple AWS accounts?
105. How would you design security controls for an external-facing API platform using WAF, Shield, API Gateway, ALB, and rate limiting?
106. How would you secure a multi-tenant SaaS platform in AWS so one tenant cannot impact another tenant’s data or workload?
107. How would you design data protection, logging, and access controls for regulated workloads in AWS?
108. How would you handle key rotation, certificate rotation, and secret rotation without causing production outages?
109. How would you design security guardrails for teams that want rapid autonomy but have mixed cloud maturity?
110. How would you validate that your AWS security architecture is actually enforced rather than only documented?

## Disaster Recovery (DR) and Resilience

111. How would you design a DR strategy for an AWS product with strict RTO and RPO requirements across application, data, and identity layers?
112. How would you choose between backup/restore, pilot light, warm standby, and multi-site active-active in AWS?
113. How would you design regional failover for an application using Route 53, ALB, compute services, messaging, and databases?
114. How would you design DR for Aurora, RDS, DynamoDB, S3, ElastiCache, OpenSearch, and EKS-backed applications?
115. How would you test disaster recovery in AWS without creating unacceptable production risk?
116. How would you design failover for stateful workloads when application tier can fail over faster than data tier?
117. How would you identify hidden DR dependencies such as secrets, IAM, DNS, external integrations, and build pipelines?
118. How would you decide whether a service needs multi-AZ, multi-region, or only robust backup in AWS?
119. How would you design DR for event-driven systems using SQS, SNS, EventBridge, Kinesis, and Lambda?
120. How would you design backup, retention, immutability, and restore workflows for regulated AWS environments?
121. How would you handle DR for EKS or ECS platforms where workloads, secrets, ingress, and storage must all recover correctly?
122. How would you validate whether your theoretical AWS DR design actually works under pressure?
123. How would you redesign an AWS platform after a failed failover exercise exposed major assumptions?
124. How would you design resilience for third-party dependency failure in an otherwise healthy AWS environment?
125. How would you communicate DR tradeoffs to leadership when they want near-zero downtime without paying for full active-active?

## Migration-Focused Questions

126. How would you design a migration strategy from on-premises infrastructure to AWS for a business-critical product?
127. How would you choose between rehost, replatform, refactor, retire, and retain for different workload types in AWS migration programs?
128. How would you design landing zones, account structures, connectivity, and guardrails before migrating applications into AWS?
129. How would you migrate a monolithic VM-based application into AWS with minimal business disruption?
130. How would you migrate databases to AWS while reducing cutover risk and preserving rollback options?
131. How would you handle DNS, identity, secrets, certificates, and firewall dependencies during AWS migration?
132. How would you migrate from self-managed Kubernetes or OpenShift to EKS with low outage risk?
133. How would you migrate from legacy message brokers to SQS, SNS, EventBridge, or MSK without breaking consumers?
134. How would you design file and object storage migration into S3 while preserving metadata, permissions, and integrity?
135. How would you phase a migration where some services must remain on-premises for regulatory or technical reasons?
136. How would you prove migration readiness before go-live for a high-risk product workload?
137. How would you handle rollback planning in an AWS migration where cutover windows are short and business tolerance is low?
138. How would you redesign observability and incident response during a transition state where workloads run across old and new platforms?
139. How would you migrate identity and access patterns from static credentials to role-based ephemeral AWS access?
140. How would you prioritize migration waves when technical dependencies and business criticality conflict?

## Cost / Scale / Reliability Tradeoffs

141. How would you reduce AWS cost in a fast-growing product platform without increasing operational risk?
142. How would you choose between Savings Plans, Reserved Instances, Spot, autoscaling, and on-demand capacity for different workloads?
143. How would you design observability so signal quality stays high while log, metric, and trace costs remain controlled?
144. How would you decide whether a workload should be active-active or active-passive across AWS regions?
145. How would you optimize cost for EKS or ECS without creating capacity, latency, or availability risk?
146. How would you reduce storage cost across S3, EBS, snapshots, backups, and analytics retention without weakening recovery?
147. How would you right-size Aurora, RDS, DynamoDB, ElastiCache, OpenSearch, and compute under unpredictable demand?
148. How would you balance platform standardization against workload-specific optimization in AWS?
149. How would you explain to leadership why some resilience or security choices are expensive but necessary?
150. How would you detect when cost optimization efforts are starting to create hidden reliability debt?

## Hard Production Outage and Troubleshooting Questions

151. Explain three serious AWS production incidents you have handled or would expect at senior level.
152. An EKS workload loses access to Secrets Manager or S3 after a deployment even though pods are healthy. How would you triage identity, network, and runtime layers?
153. CloudFront is healthy, but users in one geography get intermittent failures while others do not. How would you isolate the path?
154. An ALB shows healthy targets, but customers still receive 502 or 504 responses. How would you investigate the full request path?
155. A private RDS instance works from some workloads but not others. How would you troubleshoot DNS, security groups, routing, IAM auth, and client behavior?
156. A managed node group or Auto Scaling rollout succeeds, but instances never become healthy behind the load balancer. What would you investigate first?
157. SQS backlog grows sharply after release. How would you determine whether the issue is consumer scaling, visibility timeout, poison messages, or downstream latency?
158. DynamoDB latency spikes only for a subset of tenant traffic. How would you investigate key design, throttling, adaptive capacity, and client retry behavior?
159. IAM or SCP changes suddenly break previously working deployments. How would you investigate what changed and how far the blast radius extends?
160. Applications in one private subnet intermittently lose outbound access through NAT Gateway, firewall appliances, or VPC endpoints. How would you trace the dependency path?
161. A private EKS ingress path works internally but fails externally through CloudFront, ALB, or WAF. How would you isolate the issue?
162. Logs exist, but engineers still cannot reach root cause quickly during incidents. How would you improve the AWS troubleshooting model?
163. A failover test succeeds at compute level, but the application is unusable in the target region. How would you investigate hidden dependencies?
164. A production outage occurs during certificate rotation. How would you isolate whether the issue is ACM, ingress, service reload, DNS, or client trust?
165. A canary rollout shows only small metric changes, but customer complaints spike. How would you investigate hidden impact across application and AWS layers?

## Advanced AWS Networking

166. How would you choose between Transit Gateway, VPC peering, PrivateLink, and centralized egress for a multi-account AWS platform?
167. How would you design multi-account DNS using Route 53 private hosted zones, Resolver endpoints, and hybrid forwarding?
168. How would you decide between gateway endpoints and interface endpoints for S3, DynamoDB, and private AWS service access?
169. How would you troubleshoot asymmetric routing in a multi-VPC, Transit Gateway-based architecture?
170. How would you design ingress and east-west traffic controls across CloudFront, ALB, NLB, service mesh, and private services?
171. How would you model blast radius when centralizing egress, inspection, and shared services VPC patterns?
172. How would you design hybrid DNS and routing across Direct Connect, VPN, Route 53 Resolver, and on-prem networks?
173. How would you handle overlapping CIDR ranges during post-acquisition AWS network integration?
174. How would you design highly available outbound internet access for private subnets across multiple AZs and accounts?
175. How would you troubleshoot intermittent failures caused by NACLs, security groups, route tables, prefix lists, or endpoint policies?
176. How would you design service-to-service connectivity for private APIs exposed across accounts and VPC boundaries?
177. How would you evaluate when centralized networking becomes an availability risk instead of a governance benefit?
178. How would you design a zero-trust-ish east-west network model inside AWS without overcomplicating operations?
179. How would you compare Route 53 latency routing, failover routing, geolocation, geoproximity, and weighted routing in a global design?
180. How would you debug a situation where PrivateLink connectivity works at TCP level but application calls still fail?

## Deep Multi-Account Security and Governance

181. How would you compare AWS Control Tower with a custom landing zone for a fast-growing product company?
182. How would you design delegated administrator patterns for security, backup, detective controls, and observability in AWS Organizations?
183. How would you handle SCP design so guardrails are strong without breaking platform automation or incident response?
184. How would you troubleshoot an authorization failure involving IAM policy, resource policy, session policy, permission boundary, and SCP interaction?
185. How would you design break-glass access that is auditable, time-bound, and resistant to misuse across accounts?
186. How would you structure security ownership for IAM, KMS, GuardDuty, Security Hub, Config, and incident response in a multi-account estate?
187. How would you govern cross-account CI/CD role assumption without creating excessive trust relationships?
188. How would you design centralized logging that remains immutable, queryable, and isolated from workload accounts?
189. How would you investigate suspected lateral movement across multiple AWS accounts after credential compromise?
190. How would you design organization-wide detective controls for risky IAM activity, public exposure, encryption drift, and data exfiltration?
191. How would you enforce tag, backup, encryption, and network standards across many AWS accounts without relying on manual review?
192. How would you handle third-party vendor access to AWS while minimizing privilege and preserving auditability?
193. How would you manage KMS key ownership and cross-account encryption for shared services and application workloads?
194. How would you validate that preventive guardrails and detective controls actually align with business-critical risks?
195. How would you redesign a multi-account governance model after repeated friction between security and platform teams?

## Advanced EKS Operations

196. How would you compare Cluster Autoscaler and Karpenter for different EKS workload patterns?
197. How would you troubleshoot IP exhaustion and VPC CNI scaling limits in large EKS clusters?
198. How would you design topology spread constraints, pod disruption budgets, and autoscaling behavior for highly available EKS workloads?
199. How would you decide whether to use Bottlerocket, managed node groups, self-managed nodes, or Fargate in EKS?
200. How would you design image pull performance, caching, and startup reliability for large EKS fleets?
201. How would you choose between ALB ingress, NLB, service mesh ingress, and Gateway API approaches on EKS?
202. How would you troubleshoot IRSA failures caused by trust policy, OIDC issuer, token projection, or SDK behavior?
203. How would you design multi-tenant isolation inside EKS across namespaces, node groups, IAM, and network policies?
204. How would you evaluate service mesh adoption in EKS from both reliability and operational-cost perspectives?
205. How would you handle stateful workload storage design in EKS using EBS, EFS, FSx, and backup requirements?
206. How would you troubleshoot noisy-neighbor behavior in shared EKS clusters affecting latency-sensitive services?
207. How would you design cluster upgrade sequencing, add-on compatibility checks, and rollback strategy for EKS?
208. How would you diagnose packet loss or latency inside EKS when node metrics appear normal?
209. How would you decide when a workload should leave EKS and move to ECS or Lambda for operational simplicity?
210. How would you build a platform contract for EKS so teams can deploy safely without needing cluster-admin access?

## Aurora / DynamoDB / S3 Deep Design

211. How would you choose between Aurora, RDS, DynamoDB, ElastiCache, and S3-backed patterns for a high-scale product workload?
212. How would you compare Aurora Global Database, cross-region replicas, and application-level failover strategies?
213. How would you troubleshoot Aurora failover behavior when database metrics look healthy but application latency spikes?
214. How would you design connection management and pooling for Aurora in highly concurrent containerized environments?
215. How would you design DynamoDB partition keys and GSIs for uneven tenant traffic and hot-partition risk?
216. How would you investigate DynamoDB throttling when provisioned capacity appears sufficient?
217. How would you choose between DynamoDB transactions, conditional writes, streams, and event-driven compensating logic?
218. How would you design S3 access patterns for multi-account analytics, application assets, and secure document workflows?
219. How would you compare S3 bucket policies, IAM policies, access points, VPC endpoint policies, and SCPs in a complex access model?
220. How would you design replication, lifecycle, retention, object lock, and recovery strategy for S3 in regulated environments?
221. How would you troubleshoot cross-account S3 access failures where IAM appears correct but requests are still denied?
222. How would you compare Aurora Serverless, provisioned Aurora, and DynamoDB for bursty product workloads?
223. How would you design data durability and recovery when application state spans Aurora, DynamoDB, S3, and SQS?
224. How would you decide when a workload belongs in DynamoDB versus Aurora from both operational and consistency perspectives?
225. How would you explain the operational consequences of poor DynamoDB key design in an interview?

## Migration Coexistence and Strangler Patterns

226. How would you design a strangler migration from a monolith to AWS microservices without breaking business workflows?
227. How would you manage coexistence when old and new systems both serve production traffic during migration?
228. How would you use shadow traffic, dual reads, or replay patterns to validate a new AWS platform safely?
229. How would you design contract compatibility between legacy systems and new AWS-native services during phased migration?
230. How would you migrate authentication and authorization from legacy identity models to IAM and federated AWS access?
231. How would you handle database migration where dual writes introduce correctness and rollback risk?
232. How would you phase event-driven migration when some consumers remain on legacy brokers and others move to AWS messaging?
233. How would you redesign observability so incidents remain debuggable during mixed legacy-plus-AWS states?
234. How would you manage data synchronization, reconciliation, and source-of-truth transitions during migration?
235. How would you decide cutover boundaries for domains, APIs, and data ownership in a long-running AWS migration?
236. How would you handle certificate, DNS, firewall, and private connectivity dependencies during coexistence migration?
237. How would you communicate migration risk and success criteria to product, operations, and leadership stakeholders?
238. How would you validate rollback feasibility before every migration wave instead of assuming it exists?
239. How would you detect when a phased migration is creating more architectural debt than it is removing?
240. How would you know when to stop incremental migration and instead redesign a capability natively on AWS?

## Staff-Level Strategy and Operating Model

241. How would you define platform KPIs that actually measure developer productivity, reliability, security posture, and cost efficiency?
242. How would you sequence a two-year AWS platform roadmap when reliability debt, security gaps, and developer friction all compete for priority?
243. How would you decide which platform investments deserve headcount before they show direct revenue impact?
244. How would you redesign team boundaries when platform, security, SRE, and product teams repeatedly collide?
245. How would you create architecture review processes that improve quality without becoming delivery bottlenecks?
246. How would you drive AWS standardization across autonomous teams that already have different technology preferences?
247. How would you measure whether central platform controls are helping or harming product delivery speed?
248. How would you build trust with product teams that see platform engineering as bureaucracy?
249. How would you decide which outages justify platform redesign versus local service fixes?
250. How would you handle a situation where leadership wants faster delivery while recent incidents show the platform is too fragile?
251. How would you align FinOps, security, reliability, and developer experience into one AWS operating model?
252. How would you design a platform maturity model for teams adopting AWS at different levels of cloud capability?
253. How would you plan a migration from ticket-based infrastructure delivery to API-driven or self-service platform workflows?
254. How would you evaluate whether a shared platform should remain centralized or split into domain-aligned capabilities?
255. How would you institutionalize learning from incidents so the AWS platform improves instead of only patching symptoms?

## War-Room Scenarios

256. A major region remains up, but customers experience high error rates only through one edge path. How would you run the war room and isolate AWS edge, network, and application layers?
257. After an SCP rollout, CI/CD pipelines across several accounts begin failing unpredictably. How would you triage blast radius, recovery, and root cause?
258. A platform-wide certificate rotation breaks only certain mobile clients in production. How would you lead diagnosis across ACM, CloudFront, ALB, and application teams?
259. EKS clusters are healthy, but a subset of critical services cannot reach S3, STS, or Secrets Manager. How would you drive investigation under time pressure?
260. A major DynamoDB-backed service shows rising latency only for premium tenants during peak traffic. How would you coordinate database, application, and platform teams in the incident?
261. During failover, Route 53 shifts traffic correctly, but the target region cannot serve authenticated users. How would you uncover hidden dependencies quickly?
262. An internal shared services VPC becomes a bottleneck and causes widespread timeouts across accounts. How would you stabilize service and redesign for lower blast radius?
263. An AWS cost-control change unexpectedly removes capacity headroom and triggers cascading failures under traffic surge. How would you investigate and communicate tradeoffs?
264. A migration cutover partially succeeds, leaving legacy and AWS systems both accepting writes. How would you coordinate containment, reconciliation, and customer impact decisions?
265. A suspected credential compromise affects multiple production accounts during business hours. How would you contain blast radius while preserving essential service availability?
266. CloudFront, ALB, and application metrics disagree about where errors originate. How would you structure the war room so teams converge on evidence instead of opinion?
267. A latent DNS issue causes only some private workloads to fail after a network change. How would you organize technical triage across networking, platform, and application owners?
268. A canary looked healthy, but customer journeys fail due to downstream dependencies not covered by standard metrics. How would you redesign release verification after the incident?
269. A DR exercise reveals that backups are intact but restoration workflows are too slow to meet business objectives. How would you handle immediate remediation and longer-term redesign?
270. An incident is prolonged because every team owns part of the stack but nobody owns the end-to-end service. How would you fix both the outage and the operating model?
