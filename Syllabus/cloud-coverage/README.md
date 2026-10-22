# Cloud Coverage Track For Platform, SRE, DevOps, Security, DR, And Migration

This folder is a supplemental cloud-study track. The main 106-week non-DSA plan already covers Amazon Web Services (AWS), Microsoft Azure, Google Cloud, multi-cloud, disaster recovery, governance, security, FinOps, and migration-related themes, but this folder makes cloud preparation easier to revise cloud-by-cloud.

Use this track for product-company Senior/Staff interviews where the role expects cloud judgment across:

- Platform engineering: landing zones, Kubernetes platforms, golden paths, shared services, self-service, governance.
- SRE: SLOs, incident response, telemetry, regional recovery, capacity, operational readiness.
- DevOps: continuous integration/continuous delivery (CI/CD), GitOps, artifact promotion, rollout safety, infrastructure as code (IaC), release governance.
- Cloud infrastructure: networking, compute, storage, databases, identity and access management, managed services, cost.
- Security: identity and access management, secrets, key management services, audit logs, policy, posture management, zero trust, incident containment.
- Disaster recovery and high availability: backup, restore, recovery time objective/recovery point objective (RTO/RPO), multi-AZ, multi-region, failover, game days.
- Migration: assessment, landing zone readiness, wave planning, connectivity, data migration, cutover, rollback, modernization.

## Files

- [aws-week-wise-cloud-coverage.md](aws-week-wise-cloud-coverage.md) - AWS-specific weekly points.
- [azure-week-wise-cloud-coverage.md](azure-week-wise-cloud-coverage.md) - Azure-specific weekly points.
- [gcp-week-wise-cloud-coverage.md](gcp-week-wise-cloud-coverage.md) - GCP-specific weekly points.
- [multi-cloud-dr-migration-week-wise.md](multi-cloud-dr-migration-week-wise.md) - cross-cloud comparison, DR, governance, and migration points.
- [combined-tools-and-technology-reference.md](combined-tools-and-technology-reference.md) - provider-neutral tools and technology revision checklist.

## Coverage Audit Rule

- Keep Amazon Web Services (AWS), Microsoft Azure, and Google Cloud files service-complete for frequently used production services in each week: governance, networking, compute/platform delivery, MLOps/AI platforms, data/stateful systems, observability/FinOps/security operations, and disaster recovery/migration.
- Keep the multi-cloud file pattern-focused: compare design choices, failure modes, migration risk, DR tradeoffs, and executive evidence without repeating full provider service catalogs.
- Keep the combined tools reference provider-neutral: use it for technologies, operating models, commands, and interview revision points that are not tied to one cloud provider.

## Main Plan Mapping

Already covered in the main roadmap:

- Week 27: AWS Kubernetes Platform Architecture
- Week 28: AWS Security, DR, And FinOps
- Week 29: Azure Kubernetes Platform Architecture
- Week 30: Azure Governance And Access Lifecycle
- Week 31: GCP Kubernetes Platform Architecture
- Week 32: GCP Governance, Observability, And Cost
- Week 33: Multi-Cloud Comparison And Landing-Zone Strategy
- Week 34: Disaster Recovery And High Availability
- Week 66: Multi-Region Platform Topology
- Week 77: Multi-Account Governance
- Week 78: Hub-Spoke And Private Networking
- Week 79: Zero Trust Architecture
- Week 80: Compliance Automation
- Week 85: Active-Active Architecture
- Week 86: Automated Failover Design
- Week 87: Chaos Engineering Maturity
- Week 88: DR Game-Day Governance
- Week 102: Cloud, Infrastructure As Code, Security, And Disaster Recovery Deep Revision

## Interview Rule

For every cloud point, prepare answers in this format:

1. Architecture decision.
2. Production failure mode.
3. Security and governance control.
4. Cost impact.
5. Migration or rollback plan.
6. SRE evidence: service-level objective, mean time to recover, recovery time objective/recovery point objective, change failure rate, or incident reduction.
