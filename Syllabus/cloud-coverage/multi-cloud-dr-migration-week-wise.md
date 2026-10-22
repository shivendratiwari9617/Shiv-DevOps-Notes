# Multi-Cloud, DR, And Migration Week-Wise Coverage

Use this file to compare Amazon Web Services (AWS), Microsoft Azure, and Google Cloud from a product-company Senior/Staff perspective. Keep this file pattern-focused: do not repeat the cloud-specific service catalogs already listed in the AWS, Azure, and Google Cloud files.

## Cross-Cloud Week 1: Cloud Comparison And Provider Selection

- Compare cloud choice by region availability, latency, managed-service maturity, compliance, data residency/sovereignty, quota model, network egress, migration path, team skill, support model, cost model, lock-in, exit strategy, and strategic vendor risk.
- Compare managed Kubernetes offerings by identity model, networking model, Gateway API support, upgrade control, node operations, add-ons, policy enforcement, observability integration, and operational burden.
- Compare managed database choices by consistency model, availability model, replication options, failover semantics, backup behavior, performance limits, quota model, commercial constraints, and operational ownership.
- Compare managed AI/MLOps platforms by model lifecycle, feature/data integration, private networking, GPU availability, accelerator quotas, governance, model monitoring, evaluation workflow, registry portability, and operational ownership.
- Compare observability stacks by metric/log/trace coverage, OpenTelemetry support, alerting model, retention, query language, cardinality limits, service-map maturity, and incident evidence quality.
- Compare security posture management by asset inventory, policy coverage, control evidence, attack-path analysis, alert quality, remediation workflow, runtime visibility, and compliance mapping.

## Cross-Cloud Week 2: Landing Zone And Governance Standardization

- Provider-neutral landing-zone model: hierarchy, tenancy, environment isolation, policy inheritance, network baseline, logging baseline, identity baseline, and cost baseline.
- Account/subscription/project vending, tagging/labels, budgets, audit logs, quotas, service catalog entries, and exception lifecycle.
- Standardization boundaries: naming, tags/labels, identity and access management (IAM) patterns, policy-as-code, network segmentation, logging schemas, golden paths, exception workflow, and decision records.
- Platform: one governance model with provider-specific implementations, reusable landing-zone modules, and provider capability matrices.
- SRE: shared-services dependency risk, operational access, quota governance, audit evidence, control-plane blast radius, and landing-zone readiness checks.
- Security: guardrails, identity and access management boundaries, federation, emergency access standards, compliance mapping, encryption baselines, and drift detection.
- Migration: account/subscription/project mapping, identity and network prerequisites, shared-services sequencing, landing-zone validation, and rollback boundaries.

## Cross-Cloud Week 3: Network, Identity, And Private Connectivity

- Compare hub-spoke and shared-network patterns by ownership boundary, routing control, inspection point, blast radius, route propagation, and operational support model.
- Compare private connectivity patterns by bandwidth, latency, tunnel protection, route propagation, third-party dependency, failover behavior, encryption, and cost.
- Compare identity federation, workload identity, DNS, certificates, ingress, firewalls, web application firewalls (WAF), egress controls, mutual TLS, and service-to-service identity.
- Migration: hybrid connectivity, CIDR conflicts, DNS migration, certificate migration, firewall rule translation, access trust migration, and staged traffic cutover.
- Failure modes: route drift, DNS split-brain, NAT exhaustion, asymmetric routing, certificate expiry, trust failure, and policy mismatch.

## Cross-Cloud Week 4: Migration Strategy And Execution

- Migration assessment: application inventory, dependency mapping, data classification, traffic flows, identity dependencies, compliance constraints, network prerequisites, and cost baseline.
- Migration patterns: rehost, replatform, refactor, retire, retain, repurchase, and relocate, with compatibility and rollback constraints.
- Wave planning: low-risk pilots, shared-service migration, stateful dependencies, critical dependency paths, validation gates, freeze windows, and rollback groups.
- DevOps/platform migration: continuous integration/continuous delivery (CI/CD) changes, artifact registry migration, secrets migration, infrastructure as code (IaC) state migration, observability migration, policy migration, and GitOps cutover.
- Data and AI migration: dataset movement, schema conversion, feature-store migration, model registry portability, pipeline migration, batch/online inference cutover, evaluation parity, and drift monitoring after cutover.
- Validation: synthetic tests, data reconciliation, performance baseline, telemetry parity, security checks, recovery time objective/recovery point objective (RTO/RPO), application validation signoff, and rollback criteria.

## Cross-Cloud Week 5: DR, HA, And Active-Active Decision Making

- Compare high availability (HA) and disaster recovery (DR) patterns across providers: multi-AZ/multi-zone, multi-region, backup-restore, pilot light, warm standby, active-passive, and active-active.
- Compare failover semantics: DNS failover, global load balancing, regional health checks, readiness gates, fencing, manual override, client retry behavior, and rollback routing.
- Data strategy: replication mode, consistency model, point-in-time recovery (PITR), backups, immutable storage, conflict handling, data residency, and restore ordering.
- SRE evidence: recovery time objective (RTO), recovery point objective (RPO), failover time, recovery-point validation, data-loss measurement, dependency readiness, and impact validation.
- Security: emergency access, key management service/key recovery, backup access, ransomware recovery, backup immutability, and audit trails.

## Cross-Cloud Week 6: Cloud Security, Compliance, And Incident Response

- Identity federation, workload identity, least privilege, privileged access, break-glass, short-lived credentials, audit logging, and access reviews across Amazon Web Services (AWS), Microsoft Azure, and Google Cloud.
- Posture management by asset inventory, control coverage, attack-path analysis, finding quality, suppression/exception governance, remediation routing, and compliance evidence.
- Incident response: cloud audit timeline, containment, credential rotation, network isolation, evidence preservation, key rotation, and regulatory reporting evidence.
- Compliance evidence: policy-as-code, control mapping, log retention, encryption, vulnerability remediation, runtime protection, exception expiry, and service metadata.
- Migration security: temporary access, data-transfer protection, secret rotation, dual-run exposure, source-environment hardening, destination controls, and post-cutover cleanup.

## Cross-Cloud Week 7: FinOps, Reliability, And Decision Reporting

- Cost allocation, budgets, anomaly detection, committed-use planning, spot/preemptible strategy, GPU/accelerator cost, storage lifecycle, data-transfer cost, licensing, support plans, observability cost, and unit cost per workload/request.
- Reliability reporting: service-level objectives (SLOs), error-budget burn, incident trends, mean time to recover (MTTR), change failure rate, disaster recovery readiness, dependency risk, capacity headroom, and saturation.
- Migration reporting: wave status, risk register, cost variance, defects, telemetry parity, cutover readiness, rollback readiness, and application validation.
- Decision and risk reporting: tradeoffs, decision records, lock-in/exit risk, failure-domain analysis, risk acceptance, investment asks, and measurable technical outcomes.
