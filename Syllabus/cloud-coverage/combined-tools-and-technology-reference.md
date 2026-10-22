# Combined Tools And Technology Reference

Use this as a provider-neutral study guide for platform engineering, SRE, DevOps, security, observability, data, MLOps, and production operations interviews. Cloud-provider services remain in the AWS, Azure, and Google Cloud week-wise files.

## 1. Source Control And Collaboration

### Tool And Entity Coverage

- [Priority: Very High] Git: commits, branches, tags, remotes, staging area, object database, merge commits, rebases, cherry-picks, resets, reverts, reflog, hooks, signed commits, and submodules.
- [Priority: Very High] GitHub, GitHub Enterprise Cloud, GitHub Enterprise Server, GitLab, GitLab Self-Managed, Atlassian Bitbucket, and Bitbucket Data Center: repositories, pull requests, merge requests, branch protection, code owners, checks, runners, artifacts, permissions, audit logs, webhooks, and secret scanning.
- [Priority: High] Conventional Commits and Semantic Versioning: commit types, scopes, breaking-change markers, version numbers, release notes, changelogs, and compatibility signals.

### Controls

- Branch protection, required reviews, signed commits, CODEOWNERS, status checks, merge restrictions, repository permissions, audit logging, and secret scanning.
- Release traceability through commit history, tags, versioning, changelogs, and artifact provenance.

### Operations

- Recover lost commits with `git reflog`, locate regressions with `git bisect`, inspect history with `git log --graph --oneline --decorate`, and review changes with `git diff`.
- Triage failed pull-request checks, enforce code-owner review, resolve merge conflicts, and prepare rollback or revert commits.

## 2. CI/CD, Release Engineering, And Artifacts

### Tool And Entity Coverage

- [Priority: Very High] GitHub Actions, GitLab CI/CD, Jenkins, CircleCI, and Buildkite: workflows, pipelines, jobs, stages, runners, agents, caches, artifacts, environments, approvals, concurrency, matrices, secrets, and logs.
- [Priority: High] Argo Rollouts and Spinnaker: canary deployments, blue/green deployments, analysis templates, promotion gates, bake stages, rollout state, rollback, and traffic routing.
- [Priority: Very High] Sonatype Nexus Repository, JFrog Artifactory, AWS CodeArtifact-like repositories, npm, pnpm, Yarn, Apache Maven, Gradle, pip, Poetry, and NuGet: repositories, packages, package versions, proxy repositories, retention policies, provenance, and promotion.
- [Priority: High] LaunchDarkly, Unleash, Flipt, and OpenFeature: feature flags, targeting rules, segments, environments, SDK keys, audit logs, kill switches, stale flags, and rollout percentages.
- [Priority: High] OpenAPI Specification, Swagger UI, AsyncAPI, Postman, Insomnia, Pact, JSON Schema, gRPC, Protocol Buffers, GraphQL, REST, and HTTP semantics: schemas, contracts, mocks, collections, versions, and compatibility rules.
- [Priority: Medium] Java, JVM, .NET, Node.js, Python, Go, JUnit, pytest, Jest, Cypress, Playwright, and build systems: runtimes, package managers, test suites, fixtures, coverage reports, and build outputs.

### Controls

- Pipeline permissions, secret handling, OpenID Connect (OIDC), environment approvals, artifact immutability, dependency pinning, cache boundaries, and failure isolation.
- Release gates, progressive rollout policy, rollback rules, contract compatibility, test coverage thresholds, and stale feature-flag cleanup.

### Operations

- Inspect workflow logs, rerun failed stages, reproduce builds locally, promote artifacts, roll back deployments, and verify release traceability.
- Practice `gh run list`, `gh run view <run-id> --log`, `glab ci view`, `docker build`, and contract-test failure triage.

## 3. Infrastructure Automation And Configuration

### Tool And Entity Coverage

- [Priority: Very High] HashiCorp Terraform, HCP Terraform, Terraform Enterprise, OpenTofu, Atlantis, and Spacelift: providers, resources, data sources, modules, variables, outputs, dependency graphs, state files, backends, workspaces, plans, applies, imports, moved blocks, run tasks, and policy checks.
- [Priority: High] Terragrunt: root configuration, dependency blocks, generated backends, stacks, caches, inputs, and environment inheritance.
- [Priority: High] Red Hat Ansible Automation Platform and Ansible Core: inventories, playbooks, roles, variables, handlers, facts, vault secrets, check mode, and privilege escalation.
- [Priority: Medium] Puppet, Chef Infra, HashiCorp Packer, and Pulumi: manifests, cookbooks, desired state, image templates, builders, stacks, component resources, secrets, and previews.

### Controls

- Remote state security, state locking, provider version constraints, module versioning, plan review, policy-as-code, secret handling, backend migration, and drift detection.
- Idempotency, immutable image provenance, inventory scope, privilege escalation, and partial-failure handling.

### Operations

- Run `terraform fmt -check`, `terraform validate`, `terraform init`, `terraform plan`, `terraform show`, `terraform state list`, and `terraform import`.
- Run `ansible-inventory --graph`, `ansible all -m ping`, `ansible-playbook --check`, and `ansible-playbook --diff`.
- Review drift, recover failed applies, import existing resources, migrate state, and roll back through versioned templates or images.

## 4. Containers, Images, And Supply Chain Security

### Tool And Entity Coverage

- [Priority: Very High] Docker Engine and Open Container Initiative (OCI) image standards: Dockerfiles, build contexts, images, layers, tags, digests, manifests, registries, volumes, networks, health checks, and logs.
- [Priority: High] BuildKit, Podman, Buildah, Kaniko, and Skopeo: daemonless builds, rootless builds, build caches, image copies, registry inspection, and unprivileged Kubernetes builds.
- [Priority: Very High] Sigstore Cosign, Fulcio, Rekor, in-toto, SLSA, SPDX, CycloneDX, and software bill of materials (SBOM): signatures, attestations, transparency logs, provenance, components, licenses, and vulnerability metadata.
- [Priority: Very High] SonarQube, Semgrep, GitHub CodeQL, Snyk, Trivy, Grype, Checkov, Gitleaks, ESLint, Prettier, Ruff, Black, ShellCheck, Hadolint, markdownlint, and yamllint: scans, rules, findings, policies, exceptions, and reports.

### Controls

- Image immutability, tag-to-digest promotion, vulnerability scanning, secret detection, image signing, SBOM freshness, provenance verification, admission enforcement, and exception expiry.
- Lint/format rules, CI quality gates, false-positive handling, and rule ownership.

### Operations

- Debug containers with `docker ps`, `docker logs`, `docker inspect`, `docker stats`, `docker exec`, and `docker history`.
- Verify image provenance, triage vulnerability findings, rotate leaked secrets, clean stale tags, and enforce build-once-promote-many release flow.

## 5. Kubernetes And Platform Engineering

### Tool And Entity Coverage

- [Priority: Very High] Kubernetes: API server, etcd, scheduler, controller manager, kubelet, Container Runtime Interface (CRI), Container Network Interface (CNI), Container Storage Interface (CSI), namespaces, pods, deployments, services, ingress, Gateway API, ConfigMaps, Secrets, RBAC, CustomResourceDefinitions (CRDs), and operators.
- [Priority: Very High] Helm and Kustomize: charts, values, templates, hooks, releases, bases, overlays, patches, generators, and rendered manifests.
- [Priority: Very High] Argo CD and Flux CD: applications, sources, sync policies, health checks, reconciliation loops, Kustomizations, HelmReleases, drift, and rollbacks.
- [Priority: High] Karpenter, Cluster Autoscaler, Horizontal Pod Autoscaler, and Vertical Pod Autoscaler: node pools, pending pods, metrics, recommendations, requests, limits, disruption budgets, and scale policies.
- [Priority: High] Open Policy Agent Gatekeeper and Kyverno: constraints, constraint templates, policies, validation, mutation, audit mode, enforce mode, and exceptions.
- [Priority: High] Cilium, Istio, Linkerd, External Secrets Operator, Sealed Secrets, SOPS, ExternalDNS, cert-manager, Crossplane, and Backstage: network policies, service mesh identity, mTLS, secret sync, certificates, DNS automation, claims, compositions, catalog entities, and templates.
- [Priority: Medium] Consul, etcd, ZooKeeper, Argo Workflows, Tekton, Temporal, Apache Airflow, Prefect, Dagster, and Rundeck: service discovery, leader election, workflows, jobs, schedules, retries, state, workers, and queues.

### Controls

- RBAC, admission control, network policy, pod security, resource requests/limits, probes, disruption budgets, upgrade policy, GitOps reconciliation, and multi-tenancy boundaries.
- Chart trust, secret encryption, certificate renewal, DNS automation permissions, service mesh rollout safety, and platform API ownership.

### Operations

- Use `kubectl get pods -A`, `kubectl describe pod`, `kubectl logs --previous`, `kubectl get events -A --sort-by=.lastTimestamp`, `kubectl top pods -A`, `kubectl rollout status`, and `kubectl auth can-i`.
- Use `helm template`, `helm lint`, `helm history`, `helm rollback`, `kubectl diff -k`, `argocd app diff`, `argocd app history`, and `flux reconcile`.
- Troubleshoot scheduling, rollouts, image pulls, policy denials, certificate failures, secret sync failures, DNS drift, mesh routing, and autoscaling behavior.

## 6. Operating Systems, Proxies, And Network Runtime

### Tool And Entity Coverage

- [Priority: Very High] Linux: processes, threads, CPU, memory, filesystems, disk I/O, sockets, permissions, cgroups, namespaces, systemd, logs, and kernel limits.
- [Priority: High] NGINX, Envoy Proxy, HAProxy, Kong Gateway, Tyk Gateway, Traefik Proxy, and Envoy Gateway: listeners, routes, upstreams, backends, health checks, TLS certificates, retries, timeouts, rate limits, and access logs.
- [Priority: Very High] DNS, PKI, certificates, and name resolution: authoritative DNS, recursive DNS, split-horizon DNS, TTLs, DNSSEC, certificate authorities, chains, SANs, wildcard certificates, OCSP, trust stores, and mutual TLS.
- [Priority: High] Connection pools, retries, timeouts, circuit breakers, egress gateways, secure proxies, web application firewalls, and DDoS protections: pools, queues, backoff, jitter, deadlines, and dependency budgets.

### Controls

- OS permissions, service isolation, kernel limits, package patching, proxy TLS policy, timeout defaults, retry limits, rate limits, certificate rotation, and egress controls.
- DNS TTL planning, trust store governance, mTLS identity, circuit breaking, idempotency, and overload protection.

### Operations

- Practice `top`, `ps auxf`, `vmstat 1`, `iostat -xz 1`, `ss -s`, `df -h`, `free -m`, `journalctl -u <service>`, `lsof -i`, and `strace -p <pid>`.
- Validate proxy configs with `nginx -t` and `haproxy -c -f <config>`; inspect DNS/TLS with `dig`, `nslookup`, `openssl s_client`, and `curl -vk`.
- Troubleshoot latency, saturation, socket exhaustion, certificate expiry, DNS failures, retry storms, and proxy routing issues.

## 7. Messaging, Streaming, Databases, And Analytics

### Tool And Entity Coverage

- [Priority: Very High] Apache Kafka, Apache Pulsar, NATS, NATS JetStream, and RabbitMQ: brokers, topics, partitions, producers, consumers, consumer groups, offsets, subscriptions, acknowledgements, exchanges, queues, bindings, retention, replication, and dead-letter queues.
- [Priority: High] Redis, PostgreSQL, MySQL, Apache Cassandra, ScyllaDB, MongoDB, Elasticsearch, OpenSearch, and Apache Solr: keys, indexes, query plans, locks, replicas, shards, consistency levels, slow queries, snapshots, and failover state.
- [Priority: High] Flyway, Liquibase, Alembic, Debezium, Kafka Connect, gh-ost, and pt-online-schema-change: schema versions, migrations, change data capture, connectors, backfills, reconciliation, and restore points.
- [Priority: High] Snowflake, Databricks, Apache Spark, Apache Flink, Trino, Presto, Apache Hive, Apache Iceberg, Delta Lake, Apache Hudi, dbt, and Airbyte: warehouses, clusters, jobs, tables, partitions, catalogs, lineage, transformations, connectors, and backfills.
- [Priority: Medium] Object storage and backup systems: buckets, objects, versions, lifecycle policies, retention, immutability, encryption, replication, restore jobs, and recovery evidence.

### Controls

- Message retention, ordering, idempotence, consumer lag thresholds, dead-letter routing, schema compatibility, and replay safety.
- Database connection limits, index governance, backup retention, online schema change policy, CDC validation, access controls, encryption, and restore testing.
- Data warehouse partitioning, clustering, compaction, lineage, data contracts, cost limits, and backfill approvals.

### Operations

- Use `kafka-topics --describe`, `kafka-consumer-groups --describe`, `kafka-configs --describe`, and carefully planned `kafka-consumer-groups --reset-offsets`.
- Use `redis-cli INFO`, `redis-cli SLOWLOG GET`, `redis-cli MEMORY STATS`, `redis-cli LATENCY DOCTOR`, PostgreSQL `EXPLAIN (ANALYZE, BUFFERS)`, `pg_stat_activity`, `SHOW PROCESSLIST`, and `mysqladmin status`.
- Troubleshoot lag, hot partitions, slow queries, lock contention, schema changes, reindexing, snapshot restore, data reconciliation, and backfill failures.

## 8. Observability, Telemetry, And Incident Response

### Tool And Entity Coverage

- [Priority: Very High] Prometheus, VictoriaMetrics, Thanos, Grafana Mimir, InfluxDB, Graphite, Telegraf, Node Exporter, kube-state-metrics, and Blackbox Exporter: metrics, labels, scrape targets, recording rules, alert rules, exporters, remote write, retention, federation, and long-term storage.
- [Priority: Very High] Grafana and Alertmanager: dashboards, panels, variables, data sources, alert rules, routing, grouping, inhibition, silences, and notification policies.
- [Priority: Very High] OpenTelemetry, Jaeger, Grafana Tempo, Zipkin, Datadog, New Relic, Dynatrace, Sentry, eBPF tracing, Grafana Pyroscope, and Parca: collectors, receivers, processors, exporters, traces, spans, profiles, errors, service maps, and sampling.
- [Priority: High] Grafana Loki, Elasticsearch, OpenSearch, Logstash, Kibana, OpenSearch Dashboards, Fluent Bit, Fluentd, Vector, Filebeat, and ClickHouse: logs, pipelines, parsers, buffers, indexes, schemas, retention, and redaction.
- [Priority: High] Grafana k6, Apache JMeter, Gatling, Locust, PagerDuty, Atlassian Opsgenie, Grafana OnCall, and Rootly: tests, scenarios, virtual users, thresholds, schedules, escalations, incidents, timelines, and runbooks.

### Controls

- Metric cardinality, scrape success, alert routing, retention, log redaction, sampling, dashboard permissions, synthetic check coverage, and incident escalation policy.
- Load-test limits, rollback thresholds, on-call schedules, deduplication, acknowledgement states, and incident audit trails.

### Operations

- Validate Prometheus rules with `promtool check config` and `promtool check rules`; test query endpoints with `curl /api/v1/query`.
- Validate OpenTelemetry Collector configs, troubleshoot missing spans/logs/metrics, tune sampling, and correlate logs, metrics, and traces.
- Run load, soak, stress, and spike tests; triage alert storms; create incident timelines; and run post-incident reviews.

## 9. Security, Identity, And Secrets

### Tool And Entity Coverage

- [Priority: Very High] Okta, OAuth 2.0, OpenID Connect, SAML, SCIM, JWT, and JWKS: clients, applications, claims, scopes, refresh tokens, introspection, federation, provisioning, sessions, and factors.
- [Priority: Very High] HashiCorp Vault and CyberArk Privileged Access Manager: secrets, leases, dynamic credentials, policies, authentication methods, audit devices, vault clusters, privileged accounts, and break-glass workflows.
- [Priority: High] Wiz, Prisma Cloud, Orca Security, Lacework, Cloud Custodian, Open Policy Agent, Gatekeeper, Kyverno, and admission security: assets, findings, attack paths, policies, bundles, constraints, audit mode, enforce mode, and exceptions.
- [Priority: Very High] Splunk Enterprise, Microsoft Sentinel, Elastic Security, OpenSearch Security Analytics, Wazuh, Graylog, TheHive, Cortex, LimaCharlie, osquery, and Velociraptor: events, detections, alerts, cases, enrichment, evidence, response actions, and immutable logs.
- [Priority: High] Falco, Cilium Tetragon, Pod Security Standards, seccomp, AppArmor, SELinux, mTLS, SBOM, vulnerability prioritization, and secret detection: runtime events, enforcement policies, profiles, labels, and exceptions.

### Controls

- Federation trust, token lifetime, MFA, phishing-resistant authentication, provisioning/deprovisioning, key rotation, session security, and auditability.
- Secret leases, rotation, policy scopes, privileged access approval, audit devices, posture policies, admission enforcement, runtime detection, and SIEM evidence retention.

### Operations

- Use `vault status`, `vault token lookup`, `vault policy read <name>`, and `vault audit list`.
- Investigate suspicious identity activity, rotate compromised secrets, triage posture findings, tune detection rules, preserve evidence, and review exception expiry.

## 10. Reliability, Recovery, And Capacity

### Tool And Entity Coverage

- [Priority: Very High] Incident command, escalation, runbooks, playbooks, postmortems, service catalogs, dependency maps, SLOs, SLIs, SLAs, and error budgets: severity states, incident roles, timelines, decision logs, action items, service metadata, dependencies, objectives, and burn-rate alerts.
- [Priority: High] Multi-zone and multi-region architecture, Velero, backup/restore systems, Chaos Mesh, LitmusChaos, failure injection, game days, split-brain controls, fencing, quorum, and recovery validation: backup schedules, restore points, experiments, targets, stop conditions, and recovery evidence.
- [Priority: High] Capacity planning, demand forecasting, saturation analysis, queue-depth modeling, load testing, and headroom planning: workload models, peaks, quotas, bottlenecks, latency, throughput, cost, and risk reports.

### Controls

- Severity model, escalation triggers, runbook ownership, SLO/error-budget policy, backup scope, retention, restore ordering, blast-radius limits, stop conditions, quorum safety, and capacity headroom targets.

### Operations

- Run incident command, maintain timelines, execute runbooks, verify mitigations, perform postmortems, and track corrective actions.
- Test backups with restore drills, run chaos/game-day exercises, validate failover/failback, and measure RTO/RPO.
- Forecast demand, run capacity tests, identify saturation points, and define scaling or cost tradeoffs.

## 11. MLOps, AI Engineering, And Data Quality

### Tool And Entity Coverage

- [Priority: High] MLflow, Kubeflow, Feast, Data Version Control (DVC), and model lifecycle tooling: experiments, runs, artifacts, model registry entries, feature stores, datasets, pipelines, approvals, and lineage.
- [Priority: High] KServe, Seldon Core, BentoML, NVIDIA Triton Inference Server, vLLM, Ray Serve, Hugging Face, LangChain, LlamaIndex, Ragas, Milvus, Weaviate, and Qdrant: model servers, inference endpoints, vector indexes, embeddings, prompts, retrieval pipelines, evaluations, and guardrails.
- [Priority: High] Evidently AI, WhyLabs, Great Expectations, Soda, Deequ, OpenLineage, Marquez, DataHub, OpenMetadata, and Amundsen: monitors, checks, expectations, datasets, lineage events, catalogs, glossary terms, data contracts, and quality reports.

### Controls

- Model approval, promotion policy, dataset versioning, feature governance, access controls, drift thresholds, evaluation datasets, prompt/version management, vector index access, data contracts, lineage, and sensitive-data classification.

### Operations

- Track experiments, register models, promote or roll back model versions, validate batch/online inference, monitor drift, evaluate prompts, and troubleshoot retrieval quality.
- Run data quality checks, investigate schema drift, validate freshness, review lineage, and roll back bad data releases.

## 12. Cross-Role Revision Metrics

### Metric Coverage

- [Priority: Very High] Reliability metrics: availability, latency percentiles, error-budget burn rate, recovery time objective (RTO), recovery point objective (RPO), incident recurrence, and capacity headroom.
- [Priority: Very High] Delivery metrics: deployment frequency, lead time for changes, change failure rate, rollback rate, build queue time, pipeline success, and deployment duration.
- [Priority: High] Observability metrics: mean time to detect (MTTD), mean time to recover (MTTR), alert precision, duplicate-alert rate, telemetry ingestion delay, metric scrape success, log loss, trace completeness, dashboard load time, and time to diagnose.
- [Priority: High] Security and platform metrics: vulnerability remediation age, policy compliance, public exposure count, access review completion, break-glass review completion, platform adoption, golden-path success rate, exception age, support-ticket deflection rate, and observability cost per service or tenant.

### Operations

- Pick metrics that connect to business risk, reliability, release safety, security posture, cost, and platform adoption.
- Use the same metric definitions across teams before comparing performance.
- Tie every metric to an action, owner, and review cadence.

## Recommended Revision Method

1. Learn the operating model and the main entities for each tool family.
2. Learn the controls that prevent security, reliability, cost, and release failures.
3. Practice the operational commands where command-line troubleshooting matters.
4. Prepare one production scenario per section involving failure, mitigation, rollback, metrics, and validation evidence.
5. Keep provider-specific services in the AWS, Azure, and Google Cloud files; keep shared principles in the common cloud concepts file.