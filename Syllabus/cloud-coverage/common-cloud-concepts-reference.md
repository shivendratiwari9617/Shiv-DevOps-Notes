# Common Cloud Concepts Reference

Use this as the provider-neutral reference for AWS, Azure, Google Cloud, multi-cloud, platform, SRE, DevOps, security, DR, migration, and interview revision.

## 1. Cloud Governance And Landing Zone Concepts

- Landing zone: a governed cloud foundation for accounts/projects/subscriptions, identity, network, security, logging, policy, and operations.
- Resource hierarchy: the parent-child structure used to organize business units, environments, applications, and shared services.
- Account/project/subscription strategy: the isolation model for environments, workloads, teams, security boundaries, billing, and quotas.
- Shared services: centralized services such as networking, DNS, identity integration, logging, CI/CD, security tooling, and private connectivity.
- Policy inheritance: rules applied at a higher hierarchy level and inherited by child scopes.
- Guardrails: preventive, detective, and corrective controls that reduce operational and security risk.
- Account/project vending: standardized creation of new workload environments with baseline identity, network, policy, logging, tags, and budgets.
- Ownership metadata: tags/labels naming the application, owner, cost center, environment, data classification, and support model.

## 2. Identity And Access Concepts

- Principal: an identity that can request access, such as a user, group, role, service account, workload identity, or application identity.
- Role: a set of permissions granted to a principal.
- Policy: a rule document or assignment that allows, denies, restricts, or conditions access.
- Least privilege: granting only the permissions needed for the task and nothing extra.
- Separation of duties: splitting responsibilities so no single identity can perform risky end-to-end actions alone.
- Break-glass access: tightly controlled emergency access used when normal access paths fail.
- Just-in-time access: temporary privileged access approved only when needed.
- Workload identity: an application or compute workload identity used instead of long-lived secrets.
- Federation: trusting an external identity provider for authentication and access decisions.
- Permission boundary: a maximum-permission guardrail that limits what a principal can receive.
- Trust policy: a rule defining who can assume or use a role-like identity.
- Access review: periodic validation that users, groups, roles, and applications still need their access.

## 3. Network And Connectivity Concepts

- CIDR planning: IP range design that avoids overlap across cloud, on-premises, partners, and future environments.
- Subnet segmentation: separating workloads by tier, trust boundary, routing, or operational ownership.
- North-south traffic: traffic entering or leaving the environment.
- East-west traffic: traffic between internal services or network segments.
- Private endpoint: a private network path to a service without public internet exposure.
- Private service publishing: exposing a service privately to consumers through provider-specific private connectivity.
- Hybrid connectivity: private or encrypted connectivity between cloud and on-premises networks.
- Route domain: a logical routing boundary used to isolate traffic between environments or tenants.
- DNS split horizon: serving different DNS answers based on source network or resolver path.
- Egress control: limiting and inspecting outbound traffic from workloads.
- Ingress control: controlling traffic that enters applications through load balancers, gateways, WAFs, or API gateways.
- Zero trust access: access based on identity, device, context, and policy rather than network location alone.

## 4. Security And Compliance Concepts

- Defense in depth: using layered controls across identity, network, compute, data, detection, and response.
- Data classification: labeling data by sensitivity, regulatory impact, and handling requirements.
- Encryption at rest: protecting stored data with encryption keys.
- Encryption in transit: protecting data moving across networks using TLS or equivalent protocols.
- Customer-managed key: an encryption key controlled by the customer rather than fully managed by the provider.
- Key rotation: periodically replacing cryptographic keys to reduce exposure risk.
- Secret rotation: regularly replacing passwords, tokens, certificates, or credentials.
- Audit trail: immutable or retained event history used for investigations and compliance evidence.
- Evidence retention: storing logs, findings, reports, and assessments for audit or regulatory review.
- Vulnerability management: discovering, prioritizing, and remediating vulnerable packages, images, hosts, or functions.
- Security posture management: measuring configuration and control health across environments.
- Threat detection: identifying suspicious activity, malware, data exposure, credential misuse, or anomalous behavior.

## 5. Platform Engineering Concepts

- Golden path: an approved, reusable way for teams to deploy workloads safely and quickly.
- Self-service provisioning: allowing teams to request environments, services, or pipelines through standard templates.
- Infrastructure as code: defining infrastructure in version-controlled code or configuration.
- Policy as code: enforcing governance/security rules through code, templates, or automated checks.
- Immutable infrastructure: replacing infrastructure rather than manually modifying it in place.
- Golden image: a standardized VM/container image with approved baseline configuration.
- Service catalog: a curated set of approved products, templates, modules, or deployment patterns.
- Developer platform: shared tooling for build, deploy, observe, secure, and operate applications.
- Drift: the difference between intended configuration and actual deployed state.
- Drift remediation: restoring actual state to intended state or updating the source of truth intentionally.

## 6. CI/CD And Release Concepts

- Build stage: compiling, packaging, testing, scanning, and producing deployable artifacts.
- Artifact: a versioned output such as a container image, package, binary, model, or template.
- Promotion: moving an artifact through environments such as dev, test, staging, and production.
- Deployment strategy: a release method such as rolling, blue/green, canary, recreate, or shadow deployment.
- Rollback: returning to a known-good version after a failed deployment or incident.
- Roll forward: fixing an issue by deploying a newer corrected version.
- Approval gate: a manual or automated checkpoint before release progression.
- Deployment traceability: the ability to connect a release to code, artifact, build, approval, and runtime state.
- GitOps: using Git as the source of truth for desired runtime state.

## 7. Observability And SRE Concepts

- Observability: ability to understand system behavior from telemetry such as metrics, logs, traces, events, and profiles.
- Metric: a numeric measurement over time.
- Log: a timestamped event or record emitted by a system.
- Trace: a request path across services and dependencies.
- Golden signals: latency, traffic, errors, and saturation.
- Service-level indicator (SLI): a measured signal representing service quality.
- Service-level objective (SLO): a target for acceptable service reliability.
- Service-level agreement (SLA): a formal commitment, often contractual.
- Error budget: allowed unreliability over a period, derived from the SLO.
- Burn rate: how quickly the error budget is being consumed.
- Alert fatigue: reduced response quality caused by too many low-value alerts.
- Runbook: documented operational steps for a known task or incident.
- Postmortem: blameless incident review focused on learning and prevention.

## 8. Reliability, Backup, And DR Concepts

- High availability: designing services to remain available despite component failures.
- Fault tolerance: continuing operation even when dependencies fail.
- Resilience: ability to absorb, recover from, and adapt to failures.
- Recovery time objective (RTO): maximum acceptable time to restore service.
- Recovery point objective (RPO): maximum acceptable data loss window.
- Backup: recoverable copy of data or configuration.
- Restore: using backup/recovery data to return a system to usable state.
- Immutable backup: backup protected from deletion or modification for a period.
- Multi-zone design: distributing resources across zones in a region.
- Multi-region design: distributing services or data across regions.
- Pilot light: minimal DR footprint that can scale during recovery.
- Warm standby: partially running DR environment ready to scale quickly.
- Active-active: multiple live environments serving traffic simultaneously.
- Failover: switching traffic or operations to a standby environment.
- Failback: returning operations to the original environment after recovery.
- Game day: planned failure exercise used to validate resilience assumptions.

## 9. Migration Concepts

- Discovery: collecting applications, servers, databases, dependencies, owners, and usage data.
- Dependency mapping: identifying upstream/downstream services and infrastructure dependencies.
- Wave planning: grouping workloads into migration batches by dependency, risk, and business priority.
- Landing-zone readiness: confirming identity, network, security, logging, quotas, and support paths before migration.
- Rehost: lift-and-shift migration with minimal application changes.
- Replatform: migration with small platform changes but limited code change.
- Refactor: application redesign to use cloud-native architecture.
- Retire: decommissioning unused workloads.
- Retain: keeping a workload where it is for business or technical reasons.
- Full load: initial bulk copy of data.
- Change data capture (CDC): continuous replication of changes after initial load.
- Cutover: switching users or traffic to the target environment.
- Rollback: returning to the prior environment if cutover fails.
- Data reconciliation: proving source and target data match after migration.

## 10. Data, Analytics, And Messaging Concepts

- Object storage: storing unstructured data as objects in buckets/containers.
- Block storage: low-level volumes attached to compute resources.
- File storage: shared file systems accessed by multiple clients.
- Relational database: structured database with tables, schemas, SQL, and transactions.
- NoSQL database: non-relational database optimized for key-value, document, graph, wide-column, or time-series access.
- Partition key: value used to distribute data across storage or compute partitions.
- Hot partition: overloaded partition caused by skewed access patterns.
- Stream: ordered or partitioned flow of events or records.
- Queue: message buffer for asynchronous processing.
- Topic: publish/subscribe event distribution channel.
- Dead-letter queue: destination for messages that could not be processed successfully.
- Data lake: centralized storage for structured, semi-structured, and unstructured data.
- Data catalog: metadata inventory for datasets, schemas, owners, and lineage.
- Data lineage: history of data origin, transformation, and movement.

## 11. FinOps Concepts

- Cost allocation: assigning cloud spend to teams, products, environments, or cost centers.
- Showback: reporting costs to teams without charging them directly.
- Chargeback: billing teams or departments for their actual usage.
- Budget: spending threshold with notifications or actions.
- Forecast: estimated future cost based on trends.
- Rightsizing: matching resource size to real utilization.
- Commitment discount: reduced pricing through committed usage or reservations.
- Unit economics: cost per customer, request, transaction, workload, or business metric.
- Anomaly detection: identifying unusual cost spikes or usage changes.
- Idle resource cleanup: removing unattached, unused, or overprovisioned resources.

## 12. AI, ML, And MLOps Concepts

- Training job: process that creates or updates a model using data.
- Processing job: data preparation, evaluation, or batch transformation step.
- Feature store: managed repository for reusable ML features.
- Model registry: versioned catalog of models and approval states.
- Endpoint: hosted model serving path for inference.
- Batch inference: running predictions on batches of data rather than live requests.
- Model monitoring: checking data drift, model drift, quality, and bias after deployment.
- Experiment tracking: recording parameters, metrics, artifacts, and results.
- Lineage: tracking data, code, model, and deployment relationships.
- Guardrail: policy or filter used to constrain AI/model output or behavior.
- Prompt evaluation: testing prompt/model output quality, safety, and reliability.
- Responsible AI: practices for fairness, explainability, privacy, safety, and governance.