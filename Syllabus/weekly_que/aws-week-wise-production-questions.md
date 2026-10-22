# AWS Week-Wise Production Question Bank

FAANG-level, Principal/Staff-scenario questions generated from `aws-week-wise-cloud-coverage.md`. Each week's questions are grounded only in that week's own topics. No answers included by design.

## AWS Week 1: Landing Zone, Account Strategy, And Governance

1. A newly vended AWS account inherits an SCP that denies all EC2 instance types except a specific list, but a team's workload requires a GPU instance type not on that list — how do you grant the exception without loosening the SCP org-wide?
2. Control Tower's account factory provisions a new account with the standard guardrails, but a team bypasses the pipeline and creates resources directly in the management account — what risk does this create and how do you detect it?
3. Cost and Usage Report data shows a 250% spend increase traced to one account with no cost allocation tags applied — how do you retroactively attribute this spend and enforce tagging going forward?
4. An SCP meant to deny public S3 bucket creation was scoped to the wrong OU during a reorg, leaving a production OU without that guardrail for several weeks — how do you detect this kind of silent guardrail gap?
5. A quota increase for EC2 vCPUs was approved in us-east-1 but a deploy target shifted to us-west-2 mid-migration — how do you avoid this quota mismatch causing a failed rollout?
6. An engineer's long-lived IAM access key was committed to a public repository — walk through the immediate containment steps and what CloudTrail evidence you'd pull first.
7. SCPs block IAM user access key creation org-wide, but a legacy on-prem integration still authenticates using a long-lived access key — how do you migrate this off static credentials without an outage?
8. A permission set in IAM Identity Center meant to restrict access to business hours isn't applying to a subset of accounts — what's the likely cause and how would you confirm it?
9. Security Hub flags a public S3 bucket in an account that passed Control Tower guardrail checks at provisioning time — how did this drift happen and what continuous control would catch it going forward?
10. A break-glass IAM role was used during an incident but CloudTrail shows no linked ticket or approval — how do you redesign break-glass access to prevent unauditable use?
11. Two business units share an OU, and one team's failed CloudFormation stack left orphaned IAM roles granting broader access than intended — how do you detect and remediate this safely?
12. An IAM Identity Center federation setup for a CI/CD pipeline starts failing intermittently after an account was moved between OUs — what are the first three things you'd check?
13. During a landing-zone review, you discover resource tagging is inconsistent across 40% of accounts, breaking automated cost and ownership reporting — how do you retrofit tagging without disrupting running workloads?
14. A dependency discovery exercise for a planned OU restructuring reveals a production service quietly depends on a "sandbox" account meant for experimentation — how do you unwind this safely?
15. An SCP change to enforce region restrictions broke a legitimate DR failover to a previously allowed region — how do you balance the security control with the business continuity need?
16. A shared-services account's rate limits caused a cascading failure across five dependent accounts during a traffic spike — how do you redesign the shared-services architecture to isolate this blast radius?
17. During a company merger, two AWS Organizations need to be consolidated under one management account — what's your approach to account and OU mapping to avoid IAM/SCP collisions?
18. An audit finds several IAM roles with AdministratorAccess that were never rotated or reviewed — how do you build a sustainable IAM role lifecycle process?
19. Control Tower's account factory provisions new accounts with default guardrails, but a compliance requirement demands stricter SCPs for one specific business unit — how do you handle this exception without forking the whole baseline?
20. A rollback of a failed landing-zone migration requires reverting OU structure changes, but several accounts have already been moved and SCPs reapplied — what's your rollback strategy to minimize further disruption?
21. Permission sets scoped by IAM Identity Center attribute-based access control silently broke access for an entire team after a bulk attribute update — how do you diagnose and recover from this?
22. A hybrid identity requirement means an on-prem identity provider needs to federate into AWS IAM Identity Center for a subset of accounts only — how do you scope this without exposing the rest of the org?
23. Billing access was granted broadly to an entire platform team, and one member accidentally disabled a payment method tied to a production account — how do you prevent this class of incident going forward?
24. A team requests changes to a shared Transit Gateway route table but doesn't have permissions in the network account — how do you design a self-service model that doesn't require network-account access for every routing change?
25. An SCP inheritance chain across Organization root, OU, and account resulted in a more permissive effective policy than intended at the account level — how do you trace and fix the inheritance conflict?
26. A security review finds Security Hub findings are being generated but nobody owns triage for a third of the org's accounts — how do you fix this ownership gap operationally?
27. A team's CloudFormation drift detection shows actual account state no longer matches the deployed template after manual console changes during an incident — how do you reconcile this without causing further outage risk?
28. An external auditor requests evidence of who accessed the AWS Organizations management account and why over the last quarter — what AWS-native sources would you pull this from, and what gaps might you find?
29. A newly onboarded team was granted the standard account-vending IAM baseline, but their workload requires org-level SCP management to enforce custom guardrails — how do you avoid over-provisioning this one exception?
30. An OU used exclusively for decommissioned accounts still shows active billing for two accounts nobody remembers creating — how do you build a process to catch this before it becomes routine waste?
31. A workload in one account calls an API in another account using an IAM role assumption chain, but after an SCP update calls start failing intermittently only for certain resource tags — how do you isolate whether this is an SCP evaluation issue or a role-assumption trust policy issue?
32. Two accounts under the same OU need service-to-service calls authenticated without long-lived access keys or broad account-level roles — how would you scope IAM roles and trust policies to support least-privilege cross-account access?
33. A production incident retrospective finds that an SCP restricting EC2 public IP assignment was overridden at the account level six months ago and nobody knows why — how do you find who made this change and whether it should be reverted?
34. A cost anomaly is traced to a test account mistakenly consuming a Savings Plan meant for production workloads — how do you separate billing scopes to prevent this cross-contamination going forward?
35. During a compliance audit, you find that break-glass CloudTrail events exist but log retention on the organization trail is set to the default period, well short of the 1-year requirement — how do you fix retention without manually reconfiguring every account?

## AWS Week 2: Networking, Private Connectivity, And Edge Traffic

1. An Application Load Balancer's target group health checks pass in every AZ except one, but user reports show intermittent 502s only from that AZ — what's your debugging sequence?
2. A NAT Gateway starts dropping outbound connections during a traffic spike even though its bandwidth metrics show headroom — what's the likely root cause and how do you confirm it?
3. Two VPCs being connected via VPC Peering have overlapping CIDR ranges discovered only after the peering request was accepted — what are your options to resolve this without re-IPing production workloads?
4. A security group audit finds a 0.0.0.0/0 ingress rule on port 22 that's been active in production for over a year — how do you safely remove it without breaking legitimate access paths you haven't identified yet?
5. PrivateLink endpoints for a shared service start returning connection timeouts after a subnet's route table was modified — what's your triage path?
6. AWS WAF is configured with a rate-based rule that's blocking legitimate traffic during a marketing campaign traffic surge — how do you tune this without exposing the service to the abuse it was meant to stop?
7. A Direct Connect circuit drops to a degraded state during a planned maintenance window your team wasn't notified about — how do you architect failover so this doesn't cause a customer-facing outage next time?
8. Route 53 resolution for an internal service starts failing only for clients in one VPC after a private hosted zone association was changed — what's your rollback and diagnosis plan?
9. A team requests a new subnet in a shared VPC but the network account owner is unavailable for 48 hours — how do you build a self-service model that doesn't bottleneck on a single team?
10. Egress traffic from a production VPC to an external SaaS API is being blocked by a newly added organization-wide network firewall policy — how do you scope an exception without reopening broad egress?
11. An AZ-level outage takes down one leg of a multi-AZ active-active setup, but the ALB doesn't fail traffic away from the unhealthy AZ as expected — what would you check first in target group and health check configuration?
12. An engineer reports their instance can reach the internet but not a peered VPC's internal service — walk through how you'd isolate whether this is a route table, security group, NACL, or peering configuration issue.
13. CloudFront cache hit ratio drops sharply after a client-side change adds a new query parameter to every request — how do you fix caching behavior without a full distribution reconfiguration?
14. A Site-to-Site VPN tunnel between on-prem and AWS shows as "UP" in the console but application-layer traffic is silently failing — what layers would you check beyond tunnel status?
15. During a phased DNS cutover for a migration, 5% of traffic is still hitting the old on-prem endpoint two days after the Route 53 change — how do you find where that stale resolution is coming from?
16. A security review finds that a security group rule intended to be temporary for a migration has been live in production for six months and is broader than needed — how do you audit and safely tighten it?
17. Two services in the same VPC need to communicate over mTLS, but only one is issuing client certificates correctly — how do you approach debugging the certificate handshake without direct packet capture access?
18. A NAT Gateway's port allocation is exhausted during a batch job's parallel outbound connections — how do you redesign this to avoid future exhaustion without simply adding more NAT Gateways?
19. A production incident is traced to a route table change that silently overrode a more specific route with a broader default route via Transit Gateway — how do you prevent this class of routing conflict?
20. An API Gateway enforcing service-to-service authorization starts rejecting valid requests after a backend service's IAM role trust policy was rotated — what's your diagnosis path?
21. VPC Flow Logs analysis flags unusually high egress volume from an account with no recent deployment changes — how do you investigate whether this is a misconfiguration or an actual compromise?
22. A migration wave requires cutting over Route 53 records for 200 services with minimal downtime — what's your strategy for validating each cutover before moving to the next wave?
23. AWS Shield Advanced is active, but a legitimate partner integration's traffic pattern resembles an attack signature and gets rate-limited by WAF — how do you allowlist this safely without disabling the broader protection?
24. An NLB in front of a service starts showing elevated connection errors after a target group's deregistration delay setting was changed — how do you diagnose the relationship between connection draining and the errors?
25. A cost review finds a Direct Connect circuit is significantly underutilized relative to its committed capacity — how do you validate whether to downgrade without risking future capacity needs?
26. An internal service-to-service call chain crosses three VPCs via Transit Gateway, and one hop is introducing 40ms of unexplained latency — how do you isolate which hop and why?
27. An ALB's SSL certificate renewal via ACM failed silently, and it wasn't caught until customers reported certificate warnings — how do you build detection for this before it becomes customer-visible?
28. A security group change deployed via CI/CD accidentally removed an implicit allow rule that internal health checks depended on — how do you prevent this class of unintended removal in automated network management?
29. Two regions need active-active traffic serving via Route 53, but session affinity requirements conflict with even load distribution across regional endpoints — how do you resolve this tradeoff?
30. A newly onboarded team's VPC peering request was approved without a review of transitive routing implications, and now three unrelated teams' services can reach each other unintentionally — how do you audit and correct this?
31. An mTLS certificate rotation for internal service mesh traffic wasn't automated, and an expired cert caused a partial outage — how do you redesign this to eliminate manual cert rotation as a single point of failure?
32. During migration, overlapping CIDR ranges between the on-prem network and the target VPC are discovered only after Direct Connect and VPN tunnels are already established — what's your remediation path that avoids re-architecting the whole connection?

## AWS Week 3: Compute, Kubernetes, CI/CD, And Platform Delivery

1. An EKS cluster upgrade to a new Kubernetes minor version causes a subset of pods to enter CrashLoopBackOff due to a deprecated API removed in that version — how do you catch this before it hits production next time?
2. Karpenter provisions a node with an instance type that doesn't have the GPU driver support a workload needs, even though the workload scheduled successfully — how do you diagnose and prevent this class of mismatch?
3. An Auto Scaling Group's health check is marking instances unhealthy during a legitimate but slow startup sequence, causing a scaling death spiral — how do you fix this without just disabling health-based termination?
4. IRSA (IAM Roles for Service Accounts) for a pod stops resolving to the correct IAM role after a namespace was renamed — what's your debugging path?
5. ECR starts rejecting image pulls org-wide during a deployment window — how do you triage whether this is a throttling, permissions, or availability issue under time pressure?
6. ECR image scanning blocks a production deploy because of a newly disclosed CVE in a base image that was previously passing — how do you handle the tradeoff between shipping an urgent fix and the scan gate?
7. An Argo CD-managed cluster shows configuration drift where the live state no longer matches the Git source of truth — how do you determine whether this drift was manual intervention or a sync failure?
8. Managed node group autoscaling adds capacity correctly during a traffic spike, but pods still can't schedule due to a resource quota limit nobody remembered was set — how do you prevent this quota blind spot?
9. A CodePipeline's execution role has broader IAM permissions than the deploy actually needs, discovered during a security review — how do you scope this down without breaking the pipeline?
10. A canary deployment via CodeDeploy passes all automated checks but causes a latency regression only visible under real production load — how do you redesign the canary criteria to catch this?
11. A pod's access to Secrets Manager starts failing after an IRSA trust policy change, but only for pods scheduled on a specific managed node group — what would you check first?
12. A Fargate-backed service experiences a mass task replacement during a platform-level capacity event, and the workload wasn't designed to tolerate it — how do you redesign for this?
13. Migrating a monolith from EC2 to EKS, the team discovers the app relies on local instance-store state that doesn't translate to ephemeral pod storage — what's your migration approach?
14. A rollout using GitOps deploys a bad config to production because the automated Argo CD sync applied a change before a required manual approval gate ran — how do you close this gap in the pipeline?
15. EKS control plane upgrade windows aren't visible to the SRE team until after they've already started, causing surprise brief API unavailability — how do you get ahead of this operationally?
16. A workload's ECR image pull latency spikes during peak deploy hours, slowing rollouts org-wide — how do you diagnose whether this is registry-side or network-side (VPC endpoint vs NAT path)?
17. An engineer accidentally grants a CI IAM role cluster-admin via an EKS access entry instead of a scoped RBAC role, and it goes unnoticed for months — how do you build a process that would have caught this earlier?
18. A Fargate profile's built-in constraints block a legitimate workload requiring privileged container access for a hardware integration — how do you handle this exception given Fargate's constraints?
19. Two microservices communicate over the cluster's internal network without mTLS, and a security review flags this as a gap — how do you introduce service-to-service encryption in EKS without a full service mesh migration?
20. A rollback of a bad deployment via CodeDeploy takes 20 minutes because the previous image needs to be re-pulled from ECR, causing extended customer impact — how do you shorten this recovery time?
21. Pod disruption budgets weren't configured, and a managed node group upgrade evicted too many replicas of a stateful service simultaneously, causing an outage — how do you prevent this specific failure mode?
22. A blue/green EKS migration strategy has both old and new clusters serving production traffic during cutover, and a bug causes inconsistent behavior depending on which cluster handles the request — how do you validate consistency before full cutover?
23. Image signing verification via admission policy requires attestations from a scanner, but the scanner's findings pipeline has a backlog causing deploy delays during an incident that needs an urgent hotfix — how do you handle the tradeoff safely?
24. A workload's horizontal pod autoscaler is scaling based on CPU, but the actual bottleneck is a downstream dependency's connection pool — how do you redesign the scaling signal?
25. Image promotion from staging to production ECR repositories happens via a manual process that was skipped once, resulting in an untested image reaching production — how do you enforce this as an automated gate?
26. A GitHub Actions workflow using OIDC federation to assume an AWS IAM role starts failing after the role's trust policy condition was tightened — how do you diagnose and fix this without weakening the trust policy broadly?
27. CodeBuild's build cache becomes a source of a subtle bug where a stale cached layer ships an outdated dependency to production — how do you decide when caching aggressiveness should be reduced?
28. During a capacity incident, an EKS managed node group hits a maximum size limit configured months ago and no longer matches current traffic patterns — how do you catch stale capacity limits before they cause an incident?
29. A service mesh sidecar injection fails silently for pods in a specific namespace after an admission policy update, leaving traffic unencrypted between services without any visible error — how do you build detection for this kind of silent security regression?
30. An EKS managed node group was sized for average load, and a sudden 5x traffic spike from a marketing event causes cascading pod evictions before Karpenter can provision new capacity — how do you architect for this kind of burst?
31. A migration from EC2 to Fargate changes the workload's outbound IP behavior, breaking a partner's IP allowlist in production during the cutover — how do you plan around this dependency in advance?
32. CodePipeline triggers on every push to main, but a rebase force-push during an incident fix accidentally re-triggers dozens of stale pipeline executions — how do you make the pipeline resilient to this kind of Git event storm?

## AWS Week 4: Data, Storage, Backup, And Stateful Reliability

1. An RDS instance hits max_connections during a traffic surge, causing new connections to be rejected even though CPU and memory have headroom — how do you diagnose and fix this?
2. A DynamoDB table shows severe hot-partitioning on a monotonically increasing partition key, causing throttling under load despite on-demand capacity mode — how do you redesign the key schema to fix this?
3. ElastiCache read latency degrades sharply after a new access pattern was introduced, but the cluster's node count hasn't changed — how do you determine if this is a hot-key or under-provisioning issue?
4. An Aurora failover to the reader instance takes longer than the documented RTO during an actual AZ-level incident — what would you investigate to understand the gap between expected and actual failover time?
5. An SQS queue's consumer backlog grows unbounded during a downstream outage, and messages start approaching their maximum retention period — how do you prevent data loss while the downstream system recovers?
6. A PITR restore of an RDS instance succeeds technically, but the restored data is missing transactions from the last 90 seconds before the target time — how do you validate a restore's completeness before declaring recovery successful?
7. ElastiCache experiences a failover during a maintenance event, and the application layer doesn't handle the brief connection drop gracefully, causing cascading errors — how do you make the app resilient to this?
8. A migration using DMS for an RDS cutover shows replication lag growing instead of shrinking as cutover approaches — what would cause this and how do you address it before cutover?
9. KMS key rotation for an S3 bucket breaks read access for a service that cached the old key's data key — how do you design key rotation to avoid this kind of breakage?
10. A dual-write strategy during a database migration results in data divergence between old and new systems after a partial write failure — how do you detect and reconcile this divergence?
11. DynamoDB read costs spike after an application change introduces an inefficient scan pattern instead of a targeted query — how do you identify and fix this without a full data model redesign?
12. An AWS Backup policy for EBS volumes is running successfully, but a restore test reveals the snapshots are application-inconsistent for a database workload — how do you fix this gap in backup validation?
13. An Aurora read replica's lag grows significantly during a bulk data load job on the writer — how do you architect around this without pausing analytics workloads entirely?
14. A data classification audit finds PII stored in an S3 bucket without the retention and access controls required by policy — how do you remediate this without breaking the application that depends on it?
15. An RDS automated backup window overlaps with a nightly batch job, causing performance degradation every night — how do you resolve this scheduling conflict?
16. An MSK cluster shows a spike in consumer lag after a partition rebalance triggered by a broker replacement — how do you diagnose whether this is a rebalancing storm or genuine under-provisioning?
17. S3 lifecycle policies moved a dataset to Glacier, and now a critical batch job that expects Standard-class retrieval latency starts timing out — how do you prevent lifecycle policy changes from silently breaking downstream jobs?
18. A schema conversion during a database migration via SCT silently changes a column's precision, and it isn't caught until a financial reconciliation report shows discrepancies weeks later — how do you build validation that would catch this before cutover?
19. An SNS-to-SQS fanout requires message ordering for a specific workload, but a subscriber's scaling configuration inadvertently breaks ordering guarantees under high throughput — how do you diagnose and fix this?
20. A production RDS instance's connectivity over a PrivateLink-based access path breaks after a security group change, but public endpoint access still works — what's your diagnosis path, and why is this a security concern in itself?
21. CDC replication during a migration falls behind during a peak traffic window, risking a cutover with stale data — how do you decide whether to delay cutover or proceed?
22. A restore validation process for a critical database only checks that the restore completes, not that the data is correct — how do you design a validation process that catches silent data corruption?
23. DynamoDB's time-to-live (TTL) deletion wasn't accounted for in a workload's access pattern, causing unexpected data gaps in a downstream report — how do you diagnose and correct this?
24. An audit log review finds an IAM role with direct database access bypassing the application layer entirely, discovered only after an anomalous query pattern — how do you prevent this kind of shadow access path?
25. An S3 bucket policy was accidentally set to allow public read during a migration script run, exposing data publicly for several hours before detection — how do you build faster detection for this exact failure mode, especially given S3 Block Public Access exists?
26. EFS performance degrades under a workload with many small file operations, even though the file system's throughput mode looks adequate on paper — how do you diagnose this?
27. A rollback plan during a database migration assumes the old system stayed untouched during cutover, but a background job continued writing to it — how do you design a rollback strategy that accounts for this?
28. Connection pooling misconfiguration in an application causes RDS to exhaust connections during a deploy that briefly doubles pod count — how do you fix this at both the application and database layers?
29. An Aurora cluster's instance class was sized for current load, but a planned product launch is expected to 10x write throughput — how do you validate capacity ahead of time without over-provisioning permanently?
30. Data-exfiltration monitoring flags a large, unusual export tied to an S3 bucket with no recent deployment changes — how do you investigate whether this is a legitimate analytics job or a genuine incident?
31. An RDS instance enforces SSL-required connections, but after a client library upgrade, some application pods silently fall back to unencrypted connections without erroring — how do you detect and prevent this silent downgrade in production?

## AWS Week 5: Observability, FinOps, And Operational Excellence

1. A burn-rate alert for an SLO fires repeatedly during a known, approved maintenance window, causing on-call fatigue — how do you redesign the alert to account for planned work without losing sensitivity to real regressions?
2. A CloudWatch alarm tied to a Route 53 health check reports a service as healthy throughout an incident that customers clearly experienced — what gap in the health check design would explain this false negative?
3. A Cost and Usage Report (CUR) shows a cost spike, but standard dashboards can't attribute it to a specific team because cost allocation tags weren't enforced at resource creation — how do you retrofit attribution without re-provisioning every resource?
4. An incident postmortem can't establish a clear timeline because CloudWatch Logs from three different services use inconsistent timestamp formats and time zones — how do you standardize this across a large org without a big-bang migration?
5. X-Ray traces show a spike in a specific service segment right after a deploy, but the deploy's change record isn't linked to the alarm, slowing root-cause identification — how do you build automatic change correlation into your incident response?
6. A service's SLO dashboard shows healthy p50 latency throughout an incident, but customers reported severe slowness — what does this suggest about the choice of percentile, and how would you fix the SLO definition?
7. CloudWatch Logs costs grow significantly after a team enables verbose debug logging for troubleshooting and forgets to revert it — how do you catch this kind of cost regression before the next billing cycle?
8. A Compute Optimizer recommendation to downsize a set of EC2 instances is technically accurate but based on a short observation window that missed a monthly batch job's peak usage — how do you validate Compute Optimizer suggestions before acting on them?
9. A CloudWatch alarm is configured to page on a single missed health check, causing false pages during brief, self-resolving network blips — how do you tune this for genuine signal without losing responsiveness to real outages?
10. A security review finds that CloudTrail organization trail delivery to the central logging account stops after an S3 bucket policy was modified during a routine cleanup — how do you detect this kind of silent monitoring gap?
11. X-Ray shows a service's latency is dominated by a downstream call, but the downstream team's own Container Insights dashboard shows no issue — how do you reconcile conflicting observability data between teams?
12. A migration wave's pre-cutover baseline dashboard didn't capture a seasonal traffic pattern, leading to a false "regression" alert two weeks after cutover that wasn't actually caused by the migration — how do you build baselines that account for this?
13. Golden signal dashboards exist per-service, but a cross-service dependency failure isn't visible until three separate teams independently notice their own symptoms — how do you build observability for cascading failures across service boundaries?
14. A Budgets alert fires at 100% of the monthly budget, well after meaningful action could have been taken — how do you redesign budget alerting to be actionable earlier in the cycle?
15. GuardDuty flags a legitimate but unusual admin API call as suspicious, and the security team has started ignoring these findings due to a high false-positive rate — how do you rebuild trust in the detection?
16. Container Insights shows a service spending significant CPU time in a way standard CPU utilization metrics don't fully explain — how do you decide when to invest in deeper profiling versus relying on standard metrics?
17. CloudWatch Logs retention was set to the platform default of 30 days, but a compliance requirement for one workload needs 1 year — how do you handle per-workload retention exceptions without fragmenting your logging architecture?
18. A service scorecard shows "green" for SLO compliance, but the underlying SLO's error budget was quietly redefined after repeated breaches to make it easier to hit — how do you govern against SLO definitions being gamed over time?
19. Post-cutover regression tracking for a migration flags a metric anomaly, but nobody can determine if it's a genuine regression or an artifact of the new CloudWatch setup itself — how do you validate monitoring parity before trusting post-migration dashboards?
20. A Compute Optimizer-driven cost optimization was applied automatically to downsize an instance type, and it caused a performance regression during the next peak traffic period — how do you decide which automated recommendations should require human review versus auto-apply?
21. AWS Config rules flag drift findings into a queue with no clear ownership, and a genuinely critical finding sits untriaged for three weeks — how do you design a triage process that scales with finding volume?
22. An incident's root cause was a config change made through the console instead of the standard IaC pipeline, and it doesn't show up in the change-correlation dashboard because that only tracks pipeline-originated deploys — how do you close this blind spot?
23. Dashboards show aggregate error rate within SLO, but a specific customer segment is experiencing a much higher error rate that's masked by the aggregate — how do you build observability that catches this kind of segment-level regression?
24. A team's CloudWatch alarm uses a static threshold that was appropriate at launch but hasn't been revisited as traffic grew 10x, causing it to be functionally useless now — how do you build a process to keep thresholds relevant over time?
25. A compliance audit requires proof that only specific roles accessed a sensitive dataset over the past six months, but CloudTrail logs were exported to a bucket that itself doesn't have documented access controls — how do you close this evidentiary gap?
26. A migration-wave dashboard shows all waves as "successful" based on deployment completion, but doesn't capture whether user-facing metrics actually held steady post-cutover — how do you redefine "success" for a migration wave?

## AWS Week 6: DR, Multi-Region, And Migration Execution

1. A game day simulating a regional failure reveals that failback to the primary region takes three times longer than the documented RTO — what gaps in the runbook would explain this discrepancy?
2. An Aurora Global Database failover completes successfully, but the application layer keeps connecting to the old writer's cached endpoint for several minutes, causing continued errors — how do you fix this application-level gap?
3. A pilot-light DR pattern assumes the standby region can scale up fast enough, but a game day reveals the standby region's service quotas were never actually validated at full production scale — how do you build this validation into regular DR testing?
4. A split-brain scenario occurs during a failover test when both regions briefly accept writes to a system without proper consensus, causing data divergence — how do you redesign the failover process to prevent this?
5. Backup immutability via AWS Backup Vault Lock was assumed but not actually configured, and a ransomware-style incident is able to modify backup data — how do you validate that your backups are genuinely immutable, not just labeled as such?
6. A regulatory requirement mandates customer notification within a specific window after a data-impacting incident, but your team discovers the incident days after it actually started because DR-related monitoring wasn't wired into the main alerting system — how do you close this gap?
7. An active-active multi-region setup's cross-region replication lag spikes during a partial network degradation, and by the time failover is triggered, recent writes are lost — how do you tune failover triggers to account for replication lag?
8. Emergency access procedures for a DR event exist on paper, but during an actual regional incident, the credentials needed are themselves stored in a system that's part of the outage — how do you break this circular dependency?
9. A wave-based migration plan doesn't account for a critical cross-wave dependency, and wave 3's cutover breaks a service still depending on a not-yet-migrated wave 5 component — how do you build dependency mapping that catches this before it happens?
10. A DR runbook was last validated a year ago, and a game day reveals it references an IAM role that no longer exists after a reorg — how do you keep DR documentation from silently going stale?
11. A migration factory approach completes a lift-and-shift, but the migrated instance's performance is significantly worse than on-prem due to an EBS volume type mismatch — how do you validate performance parity before declaring migration complete?
12. A rollback plan for a failed migration wave assumes the source system is still fully intact, but decommissioning had already started on part of it — how do you build safeguards against premature decommissioning?
13. Route 53 failover records are configured correctly, but TTL settings on client-side DNS caching mean a meaningful portion of traffic doesn't fail over for several minutes — how do you address this without fully controlling client behavior?
14. A KMS key used for encrypting DR backups is itself only available in the primary region, and during a full regional outage, backups become undecryptable — how do you redesign key architecture to avoid this single point of failure?
15. A game day is run only during business hours with full staffing, so it doesn't reveal that an actual off-hours incident would have far fewer people available to execute the runbook — how do you make DR testing more representative of real conditions?
16. A warm-standby architecture's cutover rehearsal reveals that the standby's Aurora replica needs manual promotion steps nobody had scripted — how do you catch this class of gap before an actual disaster?
17. An RDS PITR-based recovery during a DR test restores successfully, but nobody validates whether the RPO target was actually met versus just assuming it was — how do you build RPO validation into your recovery process?
18. A regional capacity crunch during an actual disaster means your documented failover region can't actually absorb full production load, contrary to what capacity planning assumed — how do you keep capacity assumptions current?
19. Executive reporting during a DR event was delayed because the incident commander didn't know who owned the decision to notify customers — how do you build clear ownership into your DR runbooks before the next incident?
20. S3 Cross-Region Replication shows lag growing during a partial network degradation, and by the time failover is triggered, recently written objects haven't replicated — how do you tune failover triggers to account for this replication lag?
21. An audit trail for a past DR event is incomplete because several actions were taken directly in the console by responders under time pressure, bypassing the standard change process — how do you preserve DR evidence quality without slowing down actual emergency response?
22. A migration cutover readiness checklist is marked complete, but a game-day-style pre-cutover rehearsal wasn't actually run for one of the services — how do you enforce that readiness criteria are genuinely validated, not just checked off?
23. A DR failover test succeeds for application-tier services but reveals that a legacy stateful service was never included in any DR architecture at all — how do you catch these gaps in DR coverage across a large service inventory?
24. A migration factory handles the data-layer cutover cleanly, but application-layer service-to-service authentication breaks post-cutover because IAM role trust policies were scoped to the old region's resources — how do you plan cross-region identity continuity into a migration?
25. A postmortem for a failed failback finds that the primary region's infrastructure had configuration drift relative to the DR region during the outage window, so failback wasn't actually symmetric — how do you keep both regions in sync continuously rather than just at DR test time?
