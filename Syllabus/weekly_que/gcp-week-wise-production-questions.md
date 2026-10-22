# GCP Week-Wise Production Question Bank

FAANG-level, Principal/Staff-scenario questions generated from `gcp-week-wise-cloud-coverage.md`. Each week's questions are grounded only in that week's own topics. No answers included by design.

## GCP Week 1: Resource Hierarchy, Landing Zone, And Governance

1. A newly vended project inherits an org policy that blocks external IPs, but a data science team's Compute Engine job needs temporary egress to a public API — what's the safest way to grant this without weakening the org-wide policy?
2. A Shared VPC host project's network admin role was accidentally granted at the organization level instead of the host project — what blast radius does this create and how would you detect it?
3. Billing alerts show a 300% month-over-month spend increase traced to a single service project with no cost labels — how do you retroactively attribute the spend and prevent recurrence?
4. A team requests a new GCP project outside the project factory pipeline and self-provisions IAM bindings directly — what governance gap does this expose, and how do you bring it back under control?
5. A quota increase request for regional CPUs was approved in us-central1 but a deploy target changed to us-east1 mid-migration — how do you avoid this quota mismatch causing a failed rollout?
6. An engineer's service account key was checked into a public repository — walk through the immediate containment steps and what audit log evidence you'd pull first.
7. Org Policy Constraints block service account key creation org-wide, but a legacy on-prem integration still authenticates using a downloaded JSON key — how do you migrate this off keys without an outage?
8. A folder-level IAM Condition meant to restrict access to business hours is silently not applying to a subset of projects — what's the likely cause and how would you confirm it?
9. Security Command Center flags a public Cloud Storage bucket in a project that passed org policy checks at creation time — how did this drift happen and what continuous control would catch it going forward?
10. A break-glass IAM role was used during an incident but the audit trail shows no ticket or approval linked to the access — how do you redesign the break-glass process to prevent unauditable use?
11. Two business units share a folder and one team's failed Terraform apply left orphaned IAM bindings granting broader access than intended — how do you detect and remediate this safely?
12. A Workload Identity Federation setup for a CI/CD pipeline starts failing intermittently after a project migration — what are the first three things you'd check?
13. During a landing-zone review, you discover project labels are inconsistent across 40% of projects, breaking automated cost and ownership reporting — how do you retrofit labeling without disrupting running workloads?
14. A dependency inventory for a planned org restructuring reveals a critical production service quietly depends on a "sandbox" folder meant for experimentation — how do you unwind this safely?
15. An org policy change to enforce domain-restricted sharing broke a legitimate external vendor integration in production — how do you balance the security control with the business need?
16. A quota exhaustion in a shared services project caused a cascading failure across five dependent service projects — how do you redesign quota allocation to isolate this blast radius?
17. During a merger, two GCP organizations need to be consolidated under one resource hierarchy — what's your approach to project and folder mapping to avoid IAM collisions?
18. An audit finds several service accounts with organization-level Owner role that were never rotated or reviewed — how do you build a sustainable service account lifecycle process?
19. A project factory pipeline provisions a new project with default org policies, but a compliance requirement demands stricter constraints for one specific business unit — how do you handle this exception without forking the whole pipeline?
20. A rollback of a failed landing-zone migration requires reverting folder structure changes, but several projects have already been moved and IAM re-bound — what's your rollback strategy to minimize further disruption?
21. IAM Conditions were used to scope access by resource tag, but a bulk resource retagging script silently broke access for an entire team mid-sprint — how do you diagnose and recover from this?
22. A hybrid connectivity requirement means an on-prem identity provider needs to federate into GCP IAM for a subset of projects only — how do you scope this without exposing the rest of the org?
23. Billing account access was granted broadly to "Billing Account Administrator" for an entire platform team, and one member accidentally disabled billing on a production project — how do you prevent this class of incident going forward?
24. A shared VPC service project team requests firewall rule changes but doesn't have host project permissions — how do you design a self-service model that doesn't require host project access for every network change?
25. An org policy inheritance chain across organization, folder, and project resulted in a more permissive effective policy than intended at the project level — how do you trace and fix the inheritance conflict?
26. A security review finds Security Command Center Premium findings are being generated but nobody owns triage for a third of the org's projects — how do you fix this ownership gap operationally?
27. A team's Terraform state for project provisioning drifted from actual GCP state after manual console changes were made during an incident — how do you reconcile this without causing further outage risk?
28. An external auditor requests evidence of who accessed a production billing account and why over the last quarter — what GCP-native sources would you pull this from, and what gaps might you find?
29. A newly onboarded team was granted the standard project-vending IAM baseline, but their workload requires org-level Organization Policy Administrator to manage custom constraints — how do you avoid over-provisioning this one exception?
30. A folder used exclusively for decommissioned projects still shows active billing for two projects nobody remembers creating — how do you build a process to catch this before it becomes routine waste?
31. A service running under a shared service account in one project calls an API in another project using Workload Identity, but after an IAM Conditions update calls start failing with permission-denied intermittently only for certain resource tags — how do you isolate whether this is an IAM Conditions issue or a Workload Identity binding issue?
32. Two projects under the same folder need service-to-service calls authenticated without long-lived keys or broad project-level roles — how would you scope IAM and service accounts to support least-privilege service-to-service auth across project boundaries?
33. A production incident retrospective finds that the org's default policy restricting VM external IP access was overridden at the project level six months ago and nobody knows why — how do you find who made this change and whether it should be reverted?
34. A cost anomaly is traced to a test project mistakenly using a production-tier committed use discount pool — how do you separate billing scopes to prevent this cross-contamination going forward?
35. During a compliance audit, you find that break-glass access logs exist in Cloud Audit Logs but retention is set to the default 30 days, well short of the 1-year requirement — how do you fix retention without manually re-configuring every project?

## GCP Week 2: Networking, Private Connectivity, And Edge Traffic

1. A global HTTP(S) load balancer's health checks pass in every region except one, but user reports show intermittent 502s only from that region — what's your debugging sequence?
2. Cloud NAT starts dropping outbound connections during a traffic spike even though the NAT gateway shows available quota — what's the likely root cause and how do you confirm it?
3. Two VPCs being merged via VPC Peering have overlapping CIDR ranges discovered only after the peering request was approved — what are your options to resolve this without re-IPing production workloads?
4. A firewall rule audit finds a 0.0.0.0/0 ingress rule on port 22 that's been active in production for over a year — how do you safely remove it without breaking legitimate access paths you haven't identified yet?
5. Private Service Connect endpoints for a managed database start returning connection timeouts after a subnet was resized — what's your triage path?
6. Cloud Armor is configured with a rate-limiting rule that's blocking legitimate traffic during a marketing campaign traffic surge — how do you tune this without exposing the service to the abuse it was meant to stop?
7. A Cloud Interconnect circuit drops to degraded state during a planned maintenance window your team wasn't notified about — how do you architect failover so this doesn't cause a customer-facing outage next time?
8. DNS resolution for an internal service starts failing only for clients in one VPC after a Cloud DNS private zone was migrated — what's your rollback and diagnosis plan?
9. A Shared VPC service project team requests a new subnet but the host project's network admin is unavailable for 48 hours — how do you build a self-service model that doesn't bottleneck on a single team?
10. Egress traffic from a production VPC to an external SaaS API is being blocked by a newly added org-wide firewall policy — how do you scope an exception without reopening broad egress?
11. A regional outage takes down one leg of a multi-region active-active setup, but Cloud Load Balancing doesn't failover as expected — what would you check first in the health check and backend configuration?
12. An engineer reports their service can reach the internet but not a peered VPC's internal service — walk through how you'd isolate whether this is a routing, firewall, or peering configuration issue.
13. Cloud CDN cache hit ratio drops sharply after a client-side change adds a new query parameter to every request — how do you fix caching behavior without a full CDN reconfiguration?
14. A VPN tunnel between on-prem and GCP shows as "established" in the console but application-layer traffic is silently failing — what layers would you check beyond the tunnel status?
15. During a phased DNS cutover for a migration, 5% of traffic is still hitting the old on-prem endpoint two days after the change — how do you find where that stale resolution is coming from?
16. A security review finds that a firewall rule intended to be temporary for a migration has been live in production for six months and is broader than needed — how do you audit and safely tighten it?
17. Two services in the same VPC need to communicate over mTLS, but only one is issuing client certificates correctly — how do you approach debugging the certificate handshake without direct packet capture access?
18. Cloud NAT's port allocation is exhausted during a batch job's parallel outbound connections — how do you redesign this to avoid future exhaustion without simply over-provisioning NAT IPs?
19. A production incident is traced to a route table change that silently overrode a more specific route with a broader default route — how do you prevent this class of routing conflict?
20. An API gateway enforcing service-to-service authorization starts rejecting valid requests after a backend service's Workload Identity binding was rotated — what's your diagnosis path?
21. Data exfiltration monitoring flags unusually high egress volume from a project with no recent deployment changes — how do you investigate whether this is a misconfiguration or an actual compromise?
22. A migration wave requires cutting over DNS for 200 services with minimal downtime — what's your strategy for validating each cutover before moving to the next wave?
23. Cloud Armor's WAF rules are blocking a legitimate partner integration whose traffic pattern resembles an attack signature — how do you allowlist this safely without disabling the broader protection?
24. Private Google Access is enabled on a subnet, but a VM still can't reach a private API endpoint — what configuration layers would you check?
25. A cost review finds Cloud Interconnect is significantly underutilized relative to its committed capacity — how do you validate whether to downgrade without risking future capacity needs?
26. An internal service-to-service call chain crosses three VPCs via peering, and one hop is introducing 40ms of unexplained latency — how do you isolate which hop and why?
27. A global load balancer's SSL certificate renewal failed silently, and it wasn't caught until customers reported certificate warnings — how do you build detection for this before it becomes customer-visible?
28. A firewall rule change deployed via CI/CD accidentally removed an implicit allow rule that internal health checks depended on — how do you prevent this class of unintended removal in automated firewall management?
29. Two regions need active-active traffic serving, but session affinity requirements conflict with even load distribution across backends — how do you resolve this tradeoff?
30. A newly onboarded team's VPC peering request was approved without a review of transitive routing implications, and now three unrelated teams' services can reach each other unintentionally — how do you audit and correct this?
31. An mTLS certificate rotation for internal service mesh traffic wasn't automated, and an expired cert caused a partial outage — how do you redesign this to eliminate manual cert rotation as a single point of failure?
32. During migration, overlapping CIDR ranges between the on-prem network and the target VPC are discovered only after VPN tunnels are already established — what's your remediation path that avoids re-architecting the whole connection?

## GCP Week 3: Compute, GKE, DevOps, And Platform Delivery

1. A GKE cluster upgrade to a new minor version causes a subset of pods to enter CrashLoopBackOff due to a deprecated API removed in that version — how do you catch this before it hits production next time?
2. Autopilot's built-in resource limits are rejecting a workload's requested CPU/memory profile that ran fine under Standard — how do you decide whether to adjust the workload or move it back to Standard?
3. A Managed Instance Group's health check is marking instances unhealthy during a legitimate but slow startup sequence, causing an autoscaling death spiral — how do you fix this without just disabling health-based termination?
4. Workload Identity for a pod stops resolving to the correct GCP service account after a namespace was renamed — what's your debugging path?
5. Artifact Registry starts rejecting image pulls org-wide during a deployment window — how do you triage whether this is a quota, permissions, or availability issue under time pressure?
6. Binary Authorization blocks a production deploy because the image's attestation is missing, but the image was built through the approved Cloud Build pipeline — what broke in the attestation chain?
7. A GitOps-managed cluster shows configuration drift where the live state no longer matches the Git source of truth — how do you determine whether this drift was manual intervention or a sync failure?
8. Node pool autoscaling adds capacity correctly during a traffic spike, but pods still can't schedule due to a resource quota limit nobody remembered was set — how do you prevent this quota blind spot?
9. A Cloud Build pipeline's service account has broader IAM permissions than the deploy actually needs, discovered during a security review — how do you scope this down without breaking the pipeline?
10. A canary deployment via Cloud Deploy passes all automated checks but causes a latency regression only visible under real production load — how do you redesign the canary criteria to catch this?
11. Secret Manager access for a workload starts failing after a Workload Identity Pool configuration change, but only for pods scheduled on a specific node pool — what would you check first?
12. A node pool running spot/preemptible VMs experiences a mass eviction during a demand spike from other GCP customers, and the workload wasn't designed to tolerate it — how do you redesign for this?
13. Migrating a monolith from Compute Engine to GKE, the team discovers the app relies on local disk state that doesn't translate to ephemeral pod storage — what's your migration approach?
14. A rollout using GitOps deploys a bad config to production because the automated sync applied a change before a required manual approval gate ran — how do you close this gap in the pipeline?
15. GKE control plane upgrade windows aren't visible to the SRE team until after they've already started, causing surprise brief API unavailability — how do you get ahead of this operationally?
16. A workload's Artifact Registry image pull latency spikes during peak deploy hours, slowing rollouts org-wide — how do you diagnose whether this is registry-side or network-side?
17. An engineer accidentally grants a CI service account the Kubernetes Engine Admin role instead of a scoped custom role, and it goes unnoticed for months — how do you build a process that would have caught this earlier?
18. A GKE Autopilot cluster's built-in security policies block a legitimate workload requiring privileged container access for a hardware integration — how do you handle this exception within Autopilot's constraints?
19. Two microservices communicate over the cluster's internal network without mTLS, and a security review flags this as a gap — how do you introduce service-to-service encryption in GKE without a full service mesh migration?
20. A rollback of a bad deployment via Cloud Deploy takes 20 minutes because the previous image needs to be re-pulled from Artifact Registry, causing extended customer impact — how do you shorten this recovery time?
21. Node pool disruption budgets weren't configured, and a cluster upgrade evicted too many replicas of a stateful service simultaneously, causing an outage — how do you prevent this specific failure mode?
22. A parallel-cluster migration strategy has both old and new GKE clusters serving production traffic during cutover, and a bug causes inconsistent behavior depending on which cluster handles the request — how do you validate consistency before full cutover?
23. Binary Authorization policy requires attestations from a vulnerability scanner, but the scanner's findings pipeline has a backlog causing deploy delays during an incident that needs an urgent hotfix — how do you handle the tradeoff safely?
24. A workload's horizontal pod autoscaler is scaling based on CPU, but the actual bottleneck is a downstream dependency's connection pool — how do you redesign the scaling signal?
25. Image promotion from staging to production Artifact Registry repositories happens via a manual process that was skipped once, resulting in an untested image reaching production — how do you enforce this as an automated gate?
26. A GKE cluster's Workload Identity Federation setup for external CI/CD starts failing after a project's org policy was tightened to restrict identity pool providers — how do you diagnose and fix this without weakening the policy broadly?
27. Cloud Build's build cache becomes a source of a subtle bug where a stale cached layer ships an outdated dependency to production — how do you decide when caching aggressiveness should be reduced?
28. During a capacity incident, a GKE cluster hits the node pool's maximum size limit configured months ago and no longer matches current traffic patterns — how do you catch stale capacity limits before they cause an incident?
29. A service mesh sidecar injection fails silently for pods in a specific namespace after a policy update, leaving traffic unencrypted between services without any visible error — how do you build detection for this kind of silent security regression?
30. A GKE Standard cluster's node pool was sized for average load, and a sudden 5x traffic spike from a marketing event causes cascading pod evictions before autoscaling can catch up — how do you architect for this kind of burst?
31. A migration from App Engine to GKE changes the app's outbound IP, breaking a partner's IP allowlist in production during the cutover — how do you plan around this dependency in advance?
32. Cloud Build triggers on every push to main, but a rebase force-push during an incident fix accidentally re-triggers dozens of stale builds — how do you make the pipeline resilient to this kind of Git event storm?

## GCP Week 4: Data, Storage, Backup, And Stateful Reliability

1. Cloud SQL connections spike to the max_connections limit during a traffic surge, causing new connections to be rejected even though CPU and memory have headroom — how do you diagnose and fix this?
2. A Spanner table shows severe hot-partitioning on a monotonically increasing primary key, causing write latency to spike under load — how do you redesign the schema to fix this?
3. Bigtable read latency degrades sharply after a new access pattern was introduced, but the cluster's node count hasn't changed — how do you determine if this is a hotspotting or under-provisioning issue?
4. A Cloud SQL failover to the standby replica takes longer than the documented RTO during an actual regional incident — what would you investigate to understand the gap between expected and actual failover time?
5. Pub/Sub subscriber backlog grows unbounded during a downstream outage, and messages start approaching their retention deadline — how do you prevent data loss while the downstream system recovers?
6. A PITR restore of a Cloud SQL instance succeeds technically, but the restored data is missing transactions from the last 90 seconds before the target time — how do you validate a restore's completeness before declaring recovery successful?
7. Memorystore for Redis experiences a failover during a maintenance event, and the application layer doesn't handle the brief connection drop gracefully, causing cascading errors — how do you make the app resilient to this?
8. A migration using Database Migration Service (DMS) for a Cloud SQL cutover shows replication lag growing instead of shrinking as cutover approaches — what would cause this and how do you address it before cutover?
9. CMEK key rotation for a Cloud Storage bucket breaks read access for a service that cached the old key version — how do you design key rotation to avoid this kind of breakage?
10. A dual-write strategy during a database migration results in data divergence between old and new systems after a partial write failure — how do you detect and reconcile this divergence?
11. Firestore document reads spike in cost after an application change introduces an N+1 query pattern — how do you identify and fix this without a full data model redesign?
12. A snapshot-based backup policy for Persistent Disks is running successfully, but a restore test reveals the snapshots are application-inconsistent for a database workload — how do you fix this gap in backup validation?
13. AlloyDB read replica lag grows significantly during a bulk data load job on the primary — how do you architect around this without pausing analytics workloads entirely?
14. A data classification audit finds PII stored in a Cloud Storage bucket without the retention and access controls required by policy — how do you remediate this without breaking the application that depends on it?
15. Cloud SQL's automated backup window overlaps with a nightly batch job, causing performance degradation every night — how do you resolve this scheduling conflict?
16. A Spanner multi-region instance shows a spike in commit latency after a new region's traffic pattern skews reads toward stale replicas — how do you diagnose whether this is a read-your-writes consistency issue?
17. Storage lifecycle policies moved a dataset to a colder storage class, and now a critical batch job that expects standard-class retrieval latency starts timing out — how do you prevent lifecycle policy changes from silently breaking downstream jobs?
18. A schema conversion during a database migration silently changes a column's precision, and it isn't caught until a financial reconciliation report shows discrepancies weeks later — how do you build validation that would catch this before cutover?
19. Pub/Sub message ordering is required for a specific workload, but a subscriber's scaling configuration inadvertently breaks ordering guarantees under high throughput — how do you diagnose and fix this?
20. A production Cloud SQL instance's private IP connectivity to an application breaks after a VPC peering change, but public IP access still works — what's your diagnosis path, and why is this a security concern in itself?
21. Change Data Capture (CDC) replication during a migration falls behind during a peak traffic window, risking a cutover with stale data — how do you decide whether to delay cutover or proceed?
22. A restore validation process for a critical database only checks that the restore completes, not that the data is correct — how do you design a validation process that catches silent data corruption?
23. Bigtable garbage collection policy wasn't tuned for a workload's actual access pattern, causing storage costs to grow unexpectedly — how do you diagnose and correct this?
24. An audit log review finds a service account with direct database access bypassing the application layer entirely, discovered only after an anomalous query pattern — how do you prevent this kind of shadow access path?
25. A Cloud Storage bucket's IAM policy was accidentally set to allUsers during a migration script run, exposing data publicly for several hours before detection — how do you build faster detection for this exact failure mode?
26. Filestore performance degrades under a workload with many small file operations, even though the instance's throughput tier looks adequate on paper — how do you diagnose this?
27. A rollback plan during a database migration assumes the old system stayed untouched during cutover, but a background job continued writing to it — how do you design a rollback strategy that accounts for this?
28. Connection pooling misconfiguration in an application causes Cloud SQL to exhaust connections during a deploy that briefly doubles pod count — how do you fix this at both the application and database layers?
29. A Spanner instance's node count was sized for current load, but a planned product launch is expected to 10x write throughput — how do you validate capacity ahead of time without over-provisioning permanently?
30. Data exfiltration monitoring flags a large, unusual export tied to a Pub/Sub ingestion pipeline — how do you investigate whether this is a legitimate analytics job or a genuine incident?
31. A Cloud SQL instance enforces SSL-required connections, but after a client library upgrade, some application pods silently fall back to unencrypted connections without erroring — how do you detect and prevent this silent downgrade in production?

## GCP Week 5: Observability, FinOps, And Operational Excellence

1. A burn-rate alert for an SLO fires repeatedly during a known, approved maintenance window, causing on-call fatigue — how do you redesign the alert to account for planned work without losing sensitivity to real regressions?
2. Cloud Monitoring's uptime checks report a service as healthy throughout an incident that customers clearly experienced — what gap in the health check design would explain this false negative?
3. A billing export to BigQuery shows a cost spike, but the standard dashboards can't attribute it to a specific team because cost labels weren't enforced at project creation — how do you retrofit attribution without re-provisioning every project?
4. An incident postmortem can't establish a clear timeline because logs from three different services use inconsistent timestamp formats and time zones — how do you standardize this across a large org without a big-bang migration?
5. Error Reporting shows a new error type spiking right after a deploy, but the deploy's change record isn't linked to the alert, slowing root-cause identification — how do you build automatic change correlation into your incident response?
6. A service's SLO dashboard shows healthy p50 latency throughout an incident, but customers reported severe slowness — what does this suggest about the choice of percentile, and how would you fix the SLO definition?
7. Cloud Logging costs grow significantly after a team enables verbose debug logging for troubleshooting and forgets to revert it — how do you catch this kind of cost regression before the next billing cycle?
8. A Recommender suggestion to right-size a set of Compute Engine instances is technically accurate but based on a short observation window that missed a monthly batch job's peak usage — how do you validate Recommender suggestions before acting on them?
9. An alert policy is configured to page on a single missed uptime check, causing false pages during brief, self-resolving network blips — how do you tune this for genuine signal without losing responsiveness to real outages?
10. A security review finds that audit log sinks for several projects stop exporting to the central SIEM after a service account's permissions were narrowed during a routine cleanup — how do you detect this kind of silent monitoring gap?
11. Trace data shows a service's latency is dominated by a downstream call, but the downstream team's own dashboards show no issue — how do you reconcile conflicting observability data between teams?
12. A migration wave's pre-cutover baseline dashboard didn't capture a seasonal traffic pattern, leading to a false "regression" alert two weeks after cutover that wasn't actually caused by the migration — how do you build baselines that account for this?
13. Golden signal dashboards exist per-service, but a cross-service dependency failure isn't visible until three separate teams independently notice their own symptoms — how do you build observability for cascading failures across service boundaries?
14. A budget alert fires at 100% of the monthly budget, well after meaningful action could have been taken — how do you redesign budget alerting to be actionable earlier in the cycle?
15. An anomaly detection rule built on Cloud Logging flags a legitimate but unusual admin action as suspicious, and the security team has started ignoring these alerts due to a high false-positive rate — how do you rebuild trust in the detection?
16. A profiler shows a service spending significant CPU time in garbage collection, but this wasn't visible in standard CPU utilization metrics — how do you decide when to invest in profiling versus relying on standard metrics?
17. Log retention was set to the platform default of 30 days, but a compliance requirement for one workload needs 1 year — how do you handle per-workload retention exceptions without fragmenting your logging architecture?
18. A service scorecard shows "green" for SLO compliance, but the underlying SLO's error budget was quietly redefined after repeated breaches to make it easier to hit — how do you govern against SLO definitions being gamed over time?
19. Post-cutover regression tracking for a migration flags a metric anomaly, but nobody can determine if it's a genuine regression or an artifact of the new monitoring setup itself — how do you validate monitoring parity before trusting post-migration dashboards?
20. A Recommender-driven cost optimization was applied automatically to downsize an instance type, and it caused a performance regression during the next peak traffic period — how do you decide which Recommender actions should require human review versus auto-apply?
21. SCC findings pile up in a queue with no clear ownership, and a genuinely critical finding sits untriaged for three weeks — how do you design a triage process that scales with finding volume?
22. An incident's root cause was a config change made through the console instead of the standard IaC pipeline, and it doesn't show up in the change-correlation dashboard because that only tracks pipeline-originated deploys — how do you close this blind spot?
23. Dashboards show aggregate error rate within SLO, but a specific customer segment is experiencing a much higher error rate that's masked by the aggregate — how do you build observability that catches this kind of segment-level regression?
24. A team's alert policy uses a static threshold that was appropriate at launch but hasn't been revisited as traffic grew 10x, causing it to be functionally useless now — how do you build a process to keep thresholds relevant over time?
25. A compliance audit requires proof that only specific roles accessed a sensitive dataset over the past six months, but audit logs were exported to a sink that itself doesn't have documented access controls — how do you close this evidentiary gap?
26. A migration-wave dashboard shows all waves as "successful" based on deployment completion, but doesn't capture whether user-facing metrics actually held steady post-cutover — how do you redefine "success" for a migration wave?

## GCP Week 6: DR, Resilience, And Migration Execution

1. A game day simulating a regional failure reveals that failback to the primary region takes three times longer than the documented RTO — what gaps in the runbook would explain this discrepancy?
2. A Cloud SQL HA failover completes successfully, but the application layer keeps connecting to the old primary's cached IP for several minutes, causing continued errors — how do you fix this application-level gap?
3. A DR plan assumes the standby region has equivalent capacity, but a game day reveals the standby region's quota was never actually validated at full production scale — how do you build this validation into regular DR testing?
4. A split-brain scenario occurs during a failover test when both regions briefly accept writes to a system without proper consensus, causing data divergence — how do you redesign the failover process to prevent this?
5. Backup immutability was assumed but not actually configured, and a ransomware-style incident is able to modify backup data — how do you validate that your backups are genuinely immutable, not just labeled as such?
6. A regulatory requirement mandates customer notification within a specific window after a data-impacting incident, but your team discovers the incident days after it actually started because DR-related monitoring wasn't wired into the main alerting system — how do you close this gap?
7. A Spanner multi-region instance's failover behavior during a regional outage causes a spike in commit latency for writes that now require cross-region consensus — how do you set customer expectations and SLOs for this tradeoff?
8. Emergency access procedures for a DR event exist on paper, but during an actual regional incident, the credentials needed are themselves stored in a system that's part of the outage — how do you break this circular dependency?
9. A wave-based migration plan doesn't account for a critical cross-wave dependency, and wave 3's cutover breaks a service still depending on a not-yet-migrated wave 5 component — how do you build dependency mapping that catches this before it happens?
10. A DR runbook was last validated a year ago, and a game day reveals it references an IAM role that no longer exists after a reorg — how do you keep DR documentation from silently going stale?
11. Migrate to Virtual Machines completes a lift-and-shift, but the migrated VM's performance is significantly worse than on-prem due to a storage type mismatch — how do you validate performance parity before declaring migration complete?
12. A rollback plan for a failed migration wave assumes the source system is still fully intact, but decommissioning had already started on part of it — how do you build safeguards against premature decommissioning?
13. Cloud DNS failover policies are configured correctly, but TTL settings on client-side caching mean a meaningful portion of traffic doesn't fail over for several minutes — how do you address this without fully controlling client behavior?
14. A KMS key used for encrypting DR backups is itself only available in the primary region, and during a full regional outage, backups become undecryptable — how do you redesign key architecture to avoid this single point of failure?
15. A game day is run only during business hours with full staffing, so it doesn't reveal that an actual off-hours incident would have far fewer people available to execute the runbook — how do you make DR testing more representative of real conditions?
16. Migration Center's assessment recommends a target GCP service, but a legacy application's undocumented dependency on OS-level behavior makes that target incompatible — how do you catch this class of gap before committing to a migration path?
17. A Cloud SQL PITR-based recovery during a DR test restores successfully, but nobody validates whether the RPO target was actually met versus just assuming it was — how do you build RPO validation into your recovery process?
18. A regional capacity crunch during an actual disaster means your documented failover region can't actually absorb full production load, contrary to what capacity planning assumed — how do you keep capacity assumptions current?
19. Customer communication during a DR event was delayed because the incident commander didn't know who owned the decision to notify customers — how do you build clear ownership into your DR runbooks before the next incident?
20. A dual-region setup for storage replication shows replication lag growing during a partial network degradation, and by the time failover is triggered, recent writes are lost — how do you tune failover triggers to account for replication lag?
21. An audit trail for a past DR event is incomplete because several actions were taken directly in the console by responders under time pressure, bypassing the standard change process — how do you preserve DR evidence quality without slowing down actual emergency response?
22. A migration cutover readiness checklist is marked complete, but a game-day-style pre-cutover test wasn't actually run for one of the services — how do you enforce that readiness criteria are genuinely validated, not just checked off?
23. A DR failover test succeeds for application tier services but reveals that a legacy stateful service was never included in any DR architecture at all — how do you catch these gaps in DR coverage across a large service inventory?
24. Database Migration Service handles the data layer cutover cleanly, but application-layer service-to-service authentication breaks post-cutover because certificates were scoped to the old region's endpoints — how do you plan cross-region identity continuity into a migration?
25. A postmortem for a failed failback finds that the primary region's infrastructure had configuration drift relative to the DR region during the outage window, so failback wasn't actually symmetric — how do you keep both regions in sync continuously rather than just at DR test time?
