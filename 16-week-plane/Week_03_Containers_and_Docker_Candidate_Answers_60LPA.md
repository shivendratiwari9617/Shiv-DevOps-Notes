# Week 3 — Containers and Docker — Candidate Answers (60 LPA+)

## Q1 \| Dockerfile multi-stage builds \| P1 \| Implementation

**Interview Question:**

> A microservice image built from a single-stage Dockerfile is 2.8 GB and deployment rollout takes too long; how would you redesign the Dockerfile using multi-stage builds without breaking runtime behavior?

**Candidate Answer:**

I would not start by blindly changing the base image. First, I would capture the current runtime contract: entrypoint, exposed port, environment variables, OS packages, certificates, timezone data, user, health check behavior, and files copied into the image. Then I would compare image layers with \`docker history\` and identify build-only dependencies such as compilers, SDKs, package caches, test tools, and source folders.

My redesign would use a builder stage for dependency restore, compile, test, and artifact creation, and a slim runtime stage that only receives the compiled artifact, runtime libraries, CA certificates, and a non-root user. I would validate parity by running the same smoke tests locally and in CI, checking startup time, endpoints, logs, and container filesystem permissions before promoting it.

Example pattern:

FROM eclipse-temurin:21-jdk AS build

WORKDIR /src

COPY gradle\* settings.gradle build.gradle ./

COPY src ./src

RUN ./gradlew clean test bootJar --no-daemon

FROM eclipse-temurin:21-jre

RUN useradd -r -u 10001 appuser

WORKDIR /app

COPY --from=build /src/build/libs/app.jar /app/app.jar

USER 10001

EXPOSE 8080

ENTRYPOINT \["java","-jar","/app/app.jar"\]

The short-term win is smaller pull time and faster rollout. The long-term fix is a standard template, image-size budget, vulnerability gate, and rollback using immutable digests.

## Q2 \| Dockerfile build cache \| P1 \| Production Troubleshooting

**Interview Question:**

> A Docker build that usually completes in 6 minutes suddenly takes 45 minutes in CI after a dependency file change; how would you identify which Dockerfile layer invalidated the cache and prevent repeated full rebuilds?

**Candidate Answer:**

I would treat the 45-minute build as a pipeline regression, not only a Docker issue. First, I would check whether the Dockerfile, \`.dockerignore\`, base image tag, dependency lock files, runner image, BuildKit setting, or remote cache configuration changed recently. Then I would compare logs from a fast build and a slow build to see which layer stopped being cached.

The common causes are copying the entire source tree before dependency restore, missing \`.dockerignore\`, a changed package lock file, cache mounts not being used, base image tag resolving to a new digest, or the CI runner losing its local cache due to cleanup. I would inspect \`docker build --progress=plain\`, \`docker history\`, BuildKit cache metadata, runner disk pressure, and registry-cache hits.

For mitigation, I would restore a known good runner image or Dockerfile version, use \`--cache-from\` / registry-backed cache, and avoid \`--no-cache\` unless debugging. Permanently, I would reorder Dockerfile layers so dependency installation happens before source copy, pin base images by digest where needed, keep dependency lock files stable, and publish cache metrics such as build duration, cache hit ratio, image size, and runner disk usage. In an interview, I would emphasize that cache reliability is part of platform productivity, not just a build optimization.

## Q3 \| Dependency management \| P1 \| Implementation

**Interview Question:**

> Your container image works locally but fails in production because a runtime library was present in the build stage but missing in the final stage; how would you debug and fix the multi-stage Dockerfile design?

**Candidate Answer:**

I would first reproduce the failure using the same image digest, same command, same environment variables, and same runtime user as production. A container that works locally but fails in production often means the local machine is accidentally providing a dependency that the image does not contain, or the production runtime is stricter about user, architecture, CA certificates, or shared libraries.

My fix would be to make the runtime dependency explicit and test it inside the final stage, not only in the builder stage. I would inspect the binary with \`ldd\`, check package ownership, run a smoke command, and avoid copying broad system folders from the builder because that creates hidden coupling.

Example:

FROM golang:1.23 AS build

WORKDIR /src

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN CGO_ENABLED=1 go build -o /out/service ./cmd/service

FROM debian:bookworm-slim

RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates libssl3 tzdata && rm -rf /var/lib/apt/lists/\*

RUN useradd -r -u 10001 app

COPY --from=build /out/service /usr/local/bin/service

USER 10001

ENTRYPOINT \["service"\]

I would add CI smoke tests using the final image only. The permanent prevention is an image contract checklist, dependency scanning, and avoiding laptop-based validation as the source of truth.

## Q4 \| Image size optimization \| P1 \| Cost Optimization

**Interview Question:**

> A platform team owns hundreds of service images and registry storage plus pull time are increasing cloud cost and release time; what strategy would you use to reduce image size safely across teams?

**Candidate Answer:**

I would handle this as both a reliability and cost problem because large images increase registry storage, network egress, cold-start time, node disk pressure, and rollout MTTR. First, I would baseline image sizes by service, pull duration by environment, registry storage growth, and node image garbage collection events. Then I would identify the worst offenders using \`docker history\`, layer reuse analysis, and vulnerability reports.

My optimization approach would be standardized: multi-stage builds, smaller runtime bases, \`.dockerignore\`, no package caches in final layers, pinned dependency locks, and removal of build tools from runtime images. I would also push common organization base images so layers are reused across services, and set image-size budgets in CI with exceptions for justified cases such as ML workloads.

The short-term fix is to optimize the top 10 images causing the biggest pull and rollout delays. The permanent solution is platform governance: base-image catalog, automated size reports, registry retention policy, immutable promotion by digest, and dashboards for image size, pull latency, storage cost, and deployment duration. I would be careful not to chase minimal size at the cost of debuggability, compliance, or missing CA/timezone/runtime dependencies.

## Q5 \| Base image strategy \| P1 \| Security

**Interview Question:**

> A critical vulnerability is reported in the organization’s common base image used by 80 services; how would you assess impact, coordinate upgrades, validate compatibility, and avoid a production-wide regression?

**Candidate Answer:**

I would first assess blast radius: which images inherit the vulnerable base, which environments are deployed, whether the CVE is actually exploitable in our runtime, and whether public-facing or privileged workloads are affected. I would not approve a panic rebuild without understanding compatibility impact, but I would create an emergency path for internet-facing critical services.

My triage would include scanning results, CVE severity, package version, running container inventory, SBOM data, exposed ports, container privileges, and whether compensating controls exist such as non-root user, read-only filesystem, dropped capabilities, network policy, or WAF rules. For mitigation, I would rebuild the common base with the patched package, rebuild dependent images using immutable digests, run smoke/regression tests, and promote through staging before production. If the exploit is critical and active, I would prioritize rollback, isolation, or temporary traffic restrictions while patched images roll out.

Long term, I would enforce base image ownership, rebuild cadence, dependency provenance, SBOM generation, signed images, and CI gates that block unapproved critical vulnerabilities. I would also keep a compatibility test suite for base image upgrades, because the real senior risk is not only patching but safely patching hundreds of services without causing a second outage.

## Q6 \| Distroless images \| P2 \| Security

**Interview Question:**

> A security team mandates distroless images for production, but developers rely on shell-based debugging inside containers; how would you adopt distroless while keeping production troubleshooting practical?

**Candidate Answer:**

I would support the security goal but implement distroless adoption with a migration path, not a blanket mandate. First, I would separate production runtime needs from debugging needs. Distroless images reduce shell and package-manager attack surface, but they also remove tools teams commonly use during incidents. I would check whether the application requires CA certificates, timezone data, native libraries, a shell-based entrypoint, or dynamic linking.

For production, I would build a distroless image that runs as non-root and validates startup through smoke tests. For incident debugging, I would avoid putting shells back into the image. Instead, I would use ephemeral debug containers, sidecar debug tooling in controlled environments, logs/metrics/traces, and prebuilt debug variants only for non-production or break-glass use with approval.

The short-term mitigation is to pilot distroless on stateless services with strong observability. The permanent pattern is a platform template with documented constraints, SBOM/scanning, health checks, and a runbook for debugging distroless workloads. In an interview, I would clearly state the trade-off: distroless improves runtime security, but without observability and debug workflow it can increase MTTR. A senior answer balances both.

## Q7 \| Non-root containers \| P1 \| Security

**Interview Question:**

> A production container must stop running as root, but after switching to a non-root user the application fails with permission errors on logs, temp files, and mounted volumes; how would you fix it without weakening security?

**Candidate Answer:**

I would first identify whether the failure is caused by file ownership, privileged port binding, write paths, mounted volumes, or process assumptions about UID 0. I would reproduce with the exact UID/GID used in production and inspect logs, permission errors, and filesystem paths such as \`/tmp\`, application cache directories, log directories, and mounted secrets.

My fix would not be to revert to root. I would create a dedicated UID/GID, ensure application directories are owned correctly during build, avoid writing to the image filesystem when possible, and redirect runtime writes to \`/tmp\` or mounted volumes. If the issue is binding to port 80 or 443, I would either expose an unprivileged internal port like 8080 and let the ingress/load balancer terminate traffic, or grant only the minimal capability if absolutely required.

Short term, I would patch ownership and validate readiness, liveness, logs, and write paths. Long term, I would add CI tests that run the container as the production UID, enforce \`USER\` in Dockerfiles, block privileged containers, and document non-root patterns in platform templates. This answer shows I understand that non-root is a security control, but production safety requires compatibility testing.

## Q8 \| Runtime hardening \| P1 \| Security

**Interview Question:**

> How would you harden a production container runtime so that a compromised application has minimum filesystem, Linux capability, privilege escalation, and host-access risk?

**Candidate Answer:**

I would harden the runtime using defense in depth because container isolation is not a security boundary by itself. First, I would classify the workload: required network access, file writes, Linux capabilities, volume mounts, secrets, and whether it needs privileged operations. Then I would remove everything not required.

My baseline would be non-root user, read-only root filesystem where possible, dropped Linux capabilities, no privileged mode, no host PID/network/IPC unless justified, seccomp/AppArmor profiles, resource limits, immutable images, signed images, and secrets mounted through the orchestrator rather than baked into the image. I would also restrict egress, avoid Docker socket mounts, scan images, and ensure logs do not expose secrets.

During an incident, I would check container runtime flags, effective capabilities, mounts, user ID, process tree, unexpected outbound connections, and image provenance. The short-term mitigation for a suspected compromise is isolate the container, revoke credentials, rotate secrets, preserve evidence, and redeploy from a trusted digest. Long term, I would enforce policies through admission controls, CI gates, golden Dockerfile templates, and runtime detection. I would frame this as reducing blast radius, not claiming containers are fully secure by default.

## Q9 \| Image vulnerability scanning \| P1 \| Security

**Interview Question:**

> A CI pipeline blocks release because image scanning reports high and critical vulnerabilities, but the application team says the affected packages are not used at runtime; how would you make the release decision?

**Candidate Answer:**

I would first separate policy noise from actual release risk. I would review the scanner output for CVE severity, package name, installed version, fixed version, exploitability, whether the vulnerable package is in the runtime layer, and whether the workload exposes the vulnerable path. I would also verify scanner freshness because false positives or stale vulnerability databases can block releases unnecessarily.

For short-term release handling, I would classify findings into must-fix, compensating-control exception, and false positive. Critical exploitable runtime CVEs should block promotion; build-stage-only findings may require a different policy if they do not reach the final image. I would patch the base image or dependency, rebuild from clean cache, and rescan the final digest. If business urgency requires an exception, I would make it time-bound, approved, and tied to compensating controls.

Long term, I would shift scanning left into PRs, maintain approved base images, generate SBOMs, pin versions, rebuild regularly, and create dashboards for CVE age, exception count, and mean time to remediate. I would avoid the junior mistake of either ignoring scanner results or blindly blocking releases without exploitability and production context.

## Q10 \| SBOM and supply chain \| P2 \| Security

**Interview Question:**

> A customer asks for proof of container supply-chain integrity for a regulated production deployment; what controls would you implement around SBOM, image signing, provenance, and registry policy enforcement?

**Candidate Answer:**

I would provide supply-chain proof as a traceable chain from source commit to deployed image digest. For a regulated customer, a tag alone is not enough. I would show the Git commit, build workflow run, builder identity, dependency lock files, SBOM, vulnerability scan result, image signature or attestation, registry digest, promotion approval, and deployment evidence.

Operationally, I would generate SBOMs during CI, sign images with a trusted identity, store provenance attestations, and promote the same immutable digest across environments instead of rebuilding per environment. I would also enforce branch protection, least-privilege registry access, and separation between build, approve, and deploy roles. For Kubernetes, I would validate that workloads reference approved registries and image digests, not mutable tags.

The short-term response is to collect evidence for the specific release and confirm whether the running image digest matches the approved artifact. The permanent solution is a platform supply-chain standard using SBOM, image signing, vulnerability gates, provenance, audit logs, and admission control. In an interview, I would highlight that supply-chain integrity is not one tool; it is a controlled promotion process with verifiable evidence.

## Q11 \| Registry authentication \| P1 \| Production Troubleshooting

**Interview Question:**

> A deployment starts failing with image pull authorization errors only in production while dev continues to work; how would you isolate whether the issue is registry credentials, token expiry, repository permissions, or image tag mismatch?

**Candidate Answer:**

I would first determine whether the failure is authentication, authorization, image reference, registry availability, or environment drift. Since it fails only in production, I would compare the production service account, imagePullSecret, registry path, namespace, node egress, and recent secret rotation with staging. I would inspect pod events because Kubernetes usually shows whether the pull failed due to unauthorized, denied, not found, TLS, or network timeout.

The immediate mitigation is to restore a known-good imagePullSecret or registry credential, roll back to a digest already present on nodes if safe, or temporarily promote a previously approved image. I would avoid manually logging into random nodes as a permanent fix because that bypasses Kubernetes secret management.

For root cause, I would check whether the secret expired, was created in the wrong namespace, the production registry policy changed, the image tag was not promoted, or the workload used a different service account. Validation includes successful pod pull, event clearance, deployment rollout status, and matching running image digest. Long term, I would automate registry credential rotation, use workload identity/OIDC where possible, add pre-deploy image pull validation, and alert before credential expiry.

## Q12 \| ImagePullBackOff \| P1 \| Production Troubleshooting

**Interview Question:**

> A release is blocked because workloads cannot pull a newly pushed image and show pull-backoff behavior; how would you debug registry access, image naming, tag immutability, rate limits, and network path issues?

**Candidate Answer:**

I would start from the pod events and node-level pull path, not from the application logs, because the container has not started yet. I would run \`kubectl describe pod\`, check events for \`ErrImagePull\` and \`ImagePullBackOff\`, and classify the message: image not found, unauthorized, TLS error, rate limit, network timeout, architecture mismatch, or manifest unknown.

Then I would verify the image exists in the registry by digest, confirm the tag was pushed, check registry permissions, imagePullSecret, namespace, service account, node egress, proxy/CA settings, and whether the platform allows that registry. If the image was recently pushed, I would check promotion replication delays or immutable tag policy. If production uses amd64 nodes and the image was built on arm64, I would inspect the manifest list.

Short term, I would roll back to the last known-good digest or fix the secret/reference and restart rollout. Long term, I would enforce immutable digests, pre-deployment image existence checks, registry replication health checks, and admission policies for approved registries. I would communicate that the release is blocked at artifact distribution, not application startup, which helps incident triage focus correctly.

## Q13 \| Push/pull failures \| P1 \| Production Troubleshooting

**Interview Question:**

> A self-hosted runner builds an image successfully but intermittently fails while pushing layers to the private registry; how would you troubleshoot authentication, network stability, layer reuse, registry throttling, and retry behavior?

**Candidate Answer:**

I would treat intermittent push/pull failures on self-hosted runners as a combination of runner health, registry reliability, network path, and credential handling. First, I would compare failure timestamps across runners to see whether the problem is isolated to one host or widespread. Then I would check Docker daemon logs, runner disk space, network errors, registry HTTP status codes, authentication expiry, proxy logs, and whether large layer uploads are timing out.

Immediate mitigation would be to retry safely for transient 5xx or network resets, move the job to a healthy runner pool, prune disk if the runner is full, or push smaller layers. I would avoid infinite retries because they hide platform instability and can overload the registry.

For permanent resolution, I would implement registry-backed build cache, runner health checks before accepting jobs, disk cleanup policy, credential refresh automation, concurrency limits, and alerts for daemon errors, failed pushes, and registry latency. I would also make image pushes content-addressable and promote by digest. In the RCA, I would include runner name, layer size, error class, registry status, network path, and whether failures correlate with high concurrency or cleanup jobs.

## Q14 \| Tagging and immutability \| P2 \| Release Engineering

**Interview Question:**

> A production rollback pulls the same image tag but gets a different image than the previous release; how would you redesign image tagging, digest pinning, promotion, and rollback governance?

**Candidate Answer:**

I would immediately stop relying on mutable tags for rollback. If rollback pulls the same tag but gets a different image, the release process has broken artifact immutability. First, I would identify the image digest used by the previous stable deployment from deployment history, registry metadata, GitOps commit, or cluster state. Then I would redeploy that exact digest, not the tag.

For root cause, I would check whether the registry allows tag overwrite, whether CI rebuilds the same tag for multiple commits, whether environments rebuild separately, or whether \`latest\` or branch tags are used in production. I would also verify whether Kubernetes image pull policy caused a fresh pull instead of using cached content.

The permanent fix is an immutable promotion workflow: build once, scan once, sign once, then promote the same digest from dev to staging to prod. Human-friendly tags can exist for discovery, but deployment manifests should pin digests. I would enforce registry tag immutability for release tags, block \`latest\` in production, and store digest-to-commit mapping. The interview point is that rollback must be deterministic; otherwise the rollback procedure itself becomes a production risk.

## Q15 \| Container exits immediately \| P1 \| Production Troubleshooting

**Interview Question:**

> A container starts and exits immediately with no obvious application error in standard logs; how would you determine whether the root cause is entrypoint, command override, missing environment variable, signal handling, or process lifecycle design?

**Candidate Answer:**

I would first confirm whether the container is exiting because the main process completed successfully, crashed, failed entrypoint execution, could not read configuration, or was killed by the runtime. I would inspect exit code, logs, \`docker inspect\`, Kubernetes pod status if applicable, termination reason, and whether the command differs between local and production.

Common causes are using a shell command that exits, running a background process instead of PID 1, missing executable permission, wrong working directory, missing environment variables, bad entrypoint arguments, or a config file not mounted. I would reproduce with \`docker run --entrypoint sh\` for debug images, or override command temporarily if the image has no shell. If it is distroless, I would use logs, termination message, or an ephemeral debug container.

Short term, I would roll back or fix command/config and redeploy with clear validation: container stays running, readiness succeeds, logs show application started, and the port is listening. Long term, I would add container startup smoke tests in CI, validate required environment variables at startup, define proper health checks, and document the expected command/entrypoint contract. I would not “fix” it by adding sleep loops, because that hides a broken process model.

## Q16 \| Runtime crash debugging \| P1 \| Production Troubleshooting

**Interview Question:**

> A service container crashes only under production traffic but not during startup checks; how would you investigate runtime dependencies, memory limits, file descriptors, thread pools, and application crash behavior?

**Candidate Answer:**

I would treat production-traffic crashes as a runtime capacity, dependency, or data-path issue rather than a simple startup failure. First, I would check whether crashes correlate with traffic spikes, specific endpoints, memory growth, CPU throttling, thread count, connection pool exhaustion, file descriptors, or downstream timeouts. I would collect previous container logs, exit codes, OOMKilled status, metrics, traces, and deployment timeline.

The immediate mitigation depends on blast radius: rollback if it started after a release, scale out if capacity-related, disable a feature flag if one endpoint is causing crashes, or rate-limit traffic to protect dependencies. I would also capture diagnostic data before restart loops erase evidence.

For root cause, I would compare resource requests/limits with actual usage, inspect application heap/thread dumps where applicable, verify native library compatibility, and check whether the container is receiving SIGTERM/SIGKILL. Validation means crash-free window, normal P95/P99 latency, error rate back to baseline, and no abnormal restarts. Long term, I would add load tests, memory profiling, better readiness behavior, graceful shutdown, resource-limit tuning, and alerts on restart count and OOM events. A senior candidate should connect container crashes to application and platform signals together.

## Q17 \| Missing runtime dependencies \| P1 \| Production Troubleshooting

**Interview Question:**

> After optimizing an image, the container fails with missing shared library or certificate errors only in the final runtime image; how would you identify what was removed and restore only the minimum required runtime components?

**Candidate Answer:**

I would first identify which dependency is missing and whether it is required at startup or only on a specific code path. For native errors, I would inspect the binary with \`ldd\`, check dynamic linker paths, architecture, libc compatibility, and whether the dependency existed in the builder stage but not the final runtime stage. For interpreted apps, I would check virtual environment path, package install location, and file permissions.

Short term, I would rebuild the image with the minimal required runtime packages and roll out the fixed digest after smoke testing. I would not copy the entire builder filesystem into production because that reverses the security and size benefit of multi-stage builds.

For permanent prevention, I would add a final-image smoke test in CI, not just unit tests in the builder stage. The test should run the same entrypoint, validate required libraries, and hit a health endpoint. I would also maintain a documented runtime dependency list, lock package versions, and include CA certificates/timezone data when needed. In the RCA, I would explain that optimization changed the runtime contract and prevention is to test the final artifact exactly as it will run in production.

## Q18 \| Container logs \| P1 \| Production Troubleshooting

**Interview Question:**

> During a production incident, the container is restarting too quickly and current logs do not show the failure; how would you retrieve useful evidence and prevent log loss in future incidents?

**Candidate Answer:**

I would first preserve evidence because restart loops can overwrite or rotate logs quickly. I would collect current logs and previous logs using the orchestrator, inspect termination messages, container exit codes, restart counts, and node events. If logs are missing, I would check whether the application writes to stdout/stderr or to a file inside the container, whether the logging driver is healthy, and whether log volume or sidecar configuration changed.

Short term, I would slow the restart loop only in a controlled way if needed for diagnosis, or roll back if a recent release caused the issue. I would avoid exec-based debugging as the first step if the container dies too quickly; previous logs and events are more reliable.

For root cause, I would correlate logs with deployment time, config changes, secrets rotation, dependency errors, and resource pressure. Validation includes stable container lifecycle, expected log flow, and dashboards showing error rate and restarts normalized. Long term, I would enforce stdout/stderr logging, structured logs with correlation IDs, termination messages, log retention for previous containers, and alerts for restart bursts. The senior point is that logging is part of operability; without it, MTTR increases significantly.

## Q19 \| Inspect-based debugging \| P1 \| Production Troubleshooting

**Interview Question:**

> An application team claims Docker networking is broken, but the container configuration may be wrong; how would you use container metadata to validate ports, environment variables, mounts, DNS, health checks, and restart policy?

**Candidate Answer:**

I would not accept “Docker networking is broken” without proving the packet path. First, I would inspect container network settings: network name, IP address, gateway, DNS config, exposed ports, published ports, aliases, and whether the container is attached to the expected bridge or user-defined network. Then I would test from host to container, container to host, and container to dependency separately.

I would use \`docker inspect\`, \`docker network inspect\`, \`ss -tulnp\`, \`curl\` from inside a debug container on the same network, and host firewall rules. Common root causes are the app binding to \`127.0.0.1\` instead of \`0.0.0.0\`, missing port publish, wrong target port, two containers on different networks, DNS alias mismatch, or host firewall/proxy interference.

Short term, I would attach the service to the correct network or fix port binding/publish settings. Long term, I would define network contracts in Compose/Kubernetes manifests, add integration tests, and expose connectivity checks in deployment validation. In an interview, I would show that inspect-based debugging is about narrowing the failure domain: application bind, container network, host NAT, DNS, or external firewall.

## Q20 \| Exec-based debugging \| P2 \| Production Troubleshooting

**Interview Question:**

> A production-only issue requires interactive debugging inside a running container, but security policy restricts shell access; how would you troubleshoot safely without violating production controls?

**Candidate Answer:**

I would first ask whether interactive debugging is allowed for that environment and whether it risks changing production state. My default is to use logs, metrics, traces, dumps, and ephemeral debug containers before entering a running production container. If I must exec, I would make it read-only, time-bound, approved, and documented in the incident timeline.

I would check the container user, shell availability, mounted secrets, and whether commands could alter files or trigger side effects. For distroless images, I would prefer an ephemeral debug container sharing network and process namespace rather than adding tools to the production image. During debugging, I would inspect environment, DNS, file permissions, network connectivity, process state, and dependency reachability, but avoid editing application files or installing packages.

The short-term goal is evidence collection and mitigation, not manual repair inside the container. Long term, if interactive debugging was required, I would improve observability, add diagnostic endpoints, better startup validation, and runbooks so future incidents do not require shell access. A senior answer should explicitly balance speed of diagnosis with auditability, security, and repeatability.

## Q21 \| Port exposure \| P1 \| Production Troubleshooting

**Interview Question:**

> A containerized API is healthy inside the container but unreachable from the host and load balancer; how would you debug bind address, exposed ports, published ports, firewall rules, and health-check configuration?

**Candidate Answer:**

I would divide the problem into four layers: application bind, container port, host publish/service mapping, and upstream routing. First, I would exec or use logs to confirm the application is listening inside the container and whether it binds to \`0.0.0.0\` or only \`127.0.0.1\`. Then I would inspect \`EXPOSE\`, runtime \`-p\` mapping or Compose ports, host firewall rules, and load balancer target port.

A frequent root cause is that the application is healthy inside the container but listening only on localhost, so Docker port publishing cannot route external traffic. Another common issue is mapping host port 8080 to container port 80 while the app actually listens on 3000. I would test from inside the container, from the host to the container IP, from the host to published port, and from the external client.

Short term, I would correct the bind address or port mapping and redeploy. Validation includes successful health check from outside the host, correct logs, and no connection refused. Long term, I would standardize service port configuration, add startup checks that print bind address, and include network smoke tests in CI/CD.

## Q22 \| Bridge networking \| P1 \| Production Troubleshooting

**Interview Question:**

> Two containers on the same host cannot communicate over the expected service name after a network refactor; how would you troubleshoot Docker bridge networks, DNS resolution, aliases, port publishing, and network attachment?

**Candidate Answer:**

I would first confirm both containers are on the same Docker network and whether it is the default bridge or a user-defined bridge. On the default bridge, service-name DNS behavior is limited compared with user-defined networks, so I would inspect \`docker network inspect\`, container aliases, IP addresses, and application connection strings.

I would test connectivity by container name, alias, and IP using a temporary debug container on the same network. Then I would check whether the target service is listening on the expected interface and port, whether the source uses localhost incorrectly, and whether firewall or Docker daemon network rules were modified. If Compose is involved, I would verify project/network names and whether containers were recreated on different networks.

Short term, I would attach both containers to a user-defined bridge network and use service names instead of host ports for container-to-container communication. Long term, I would codify the network topology in Compose or platform templates, avoid hardcoded IPs, add integration tests, and document that \`localhost\` inside a container means the container itself, not another service. This answer demonstrates practical container networking knowledge beyond just naming bridge mode.

## Q23 \| Host networking \| P2 \| Architecture

**Interview Question:**

> A latency-sensitive service team wants to use host networking in production containers; what trade-offs and risks would you evaluate before approving or rejecting that design?

**Candidate Answer:**

I would challenge the request before approving host networking because it trades isolation and port management for lower network overhead. First, I would ask what latency target is being missed, whether the bottleneck is actually Docker NAT, and whether measurements compare bridge, host, and production orchestrator networking under realistic traffic. For many services, application latency, TLS, database calls, or retries dominate more than container networking.

If host networking is justified, I would assess blast radius: port conflicts, loss of network namespace isolation, access to host network interfaces, observability changes, firewall rules, and security policy exceptions. I would also confirm whether the deployment platform supports it consistently across environments.

My recommendation would usually be to optimize bridge/user-defined networking first, tune connection pooling, keep services close to dependencies, and use proper load balancing. Host networking may be acceptable for specific agents, telemetry collectors, or ultra-low-latency workloads with strict controls, not as a general application pattern. The permanent governance is an exception process: performance evidence, threat model, port allocation, monitoring, and rollback plan. In an interview, I would show that architecture decisions require measured trade-offs, not “host network is faster” as a blanket answer.

## Q24 \| Overlay networking basics \| P3 \| Architecture

**Interview Question:**

> A multi-host container platform shows intermittent cross-node connectivity failures after a network plugin or MTU change; how would you reason through container overlay networking, encapsulation overhead, and packet fragmentation?

**Candidate Answer:**

I would treat intermittent cross-node connectivity as a platform networking incident and first establish whether it affects all services, specific nodes, specific overlays, or only high-traffic paths. I would collect failures by source node, destination node, container IP, service name, and time window. Then I would inspect overlay driver status, MTU mismatch, encapsulation, firewall rules between nodes, gossip/control-plane health, and node-level packet drops.

Short-term mitigation is to drain or isolate affected nodes, roll back recent network plugin or daemon changes, reduce traffic to unstable paths, or move critical workloads to known-good nodes. I would validate with cross-node connectivity tests, packet captures, overlay network inspection, and service-level error/latency metrics.

For root cause, I would look for MTU fragmentation, blocked VXLAN/overlay ports, kernel module issues, conntrack exhaustion, Docker daemon instability, or cloud security group changes. Long term, I would standardize node images, enforce MTU settings, monitor overlay health, run continuous synthetic network tests, and document network port requirements. I would also consider whether a production orchestrator with mature CNI observability is more appropriate than ad-hoc multi-host Docker networking for critical workloads.

## Q25 \| DNS inside containers \| P1 \| Production Troubleshooting

**Interview Question:**

> A container can reach services by IP but fails by DNS name only in CI and production runners; how would you identify whether the issue is Docker DNS configuration, host resolver settings, corporate proxy, or network policy?

**Candidate Answer:**

I would first prove that the network path works by IP and the failure is specifically name resolution. Then I would inspect \`/etc/resolv.conf\` inside the container, Docker daemon DNS configuration, corporate DNS/proxy settings, search domains, and whether CI/prod use different Docker networks or runners. I would compare \`nslookup\` or \`dig\` results from the host, from a debug container, and from the affected container.

Common causes include broken internal DNS forwarding on the runner, Docker daemon using stale nameservers, corporate split-horizon DNS not reachable from container networks, missing search domain, or a container overriding DNS settings. If the issue is intermittent, I would check DNS latency, retries, TTL behavior, and resolver saturation.

Short term, I would configure Docker daemon DNS to approved resolvers, restart affected runner Docker daemons if stale, or add correct Compose/Kubernetes DNS configuration. I would avoid hardcoding IPs except as an emergency workaround because that creates drift and failover risk. Long term, I would add DNS synthetic checks in CI runners, document split-DNS requirements, monitor resolver errors, and keep runtime images with CA/resolver compatibility. This keeps the answer focused on DNS as a dependency, not generic connectivity.

## Q26 \| Docker daemon on runners \| P1 \| Production Troubleshooting

**Interview Question:**

> Docker builds randomly fail on self-hosted CI runners with daemon unavailable, permission denied, or container network creation errors; how would you stabilize the runner fleet and prevent recurrence?

**Candidate Answer:**

I would first identify whether the daemon issue is host-level, runner orchestration, or job-level misuse. I would check the runner host health, Docker service status, daemon logs, socket permissions, concurrent job count, disk/inode usage, cgroup errors, kernel logs, and whether cleanup jobs are killing Docker while builds are running. If failures are random, I would correlate them with runner autoscaling, reboots, image pruning, or high build concurrency.

Immediate mitigation is to move jobs to healthy runners, restart the Docker daemon only after draining the runner, reduce concurrency, or disable the faulty runner from the pool. I would avoid restarting Docker during active builds because it causes secondary failures.

Permanent fixes include runner pre-flight health checks, controlled Docker daemon lifecycle, disk/inode cleanup, BuildKit configuration, standardized runner images, alerts for daemon restarts, and isolation between jobs. For Kubernetes-based runners, I would prefer rootless/buildkit or remote builders where possible instead of mounting the Docker socket broadly. In the RCA, I would include the daemon error class, affected runner IDs, build count, disk state, and whether the problem was capacity, permissions, or daemon crash.

## Q27 \| Runner disk pressure \| P1 \| Production Troubleshooting

**Interview Question:**

> A CI runner intermittently fails Docker builds because disk space is exhausted by images, build cache, and stopped containers; how would you design safe cleanup without deleting artifacts required by parallel jobs?

**Candidate Answer:**

I would treat runner disk pressure as a predictable platform hygiene issue, not a random CI failure. First, I would check \`docker system df\`, filesystem usage, inode usage, build cache size, stopped containers, dangling layers, old volumes, and workspace directories. I would also correlate failures with large images, high parallel builds, and missing cleanup after failed jobs.

Short term, I would drain the runner, clean unused images/build cache safely, expand disk if required, and rerun affected pipelines. I would be careful with \`docker system prune -a\` on shared runners because it may remove useful caches and slow every build or break jobs relying on local images.

Long term, I would implement scheduled cleanup with retention rules, per-runner disk alerts, concurrency limits based on disk size, registry-backed cache, and separate volumes for Docker data and workspaces. I would set SLOs for runner availability and build success rate. I would also optimize images and use \`.dockerignore\` because massive contexts and layers amplify disk pressure. A strong answer shows prevention: disk cleanup should be automated and observable, not a manual command executed after incidents.

## Q28 \| Build cache corruption \| P1 \| Production Troubleshooting

**Interview Question:**

> A Docker build fails only in CI with inconsistent checksum, missing layer, or corrupted cache errors while local builds pass; how would you prove cache corruption and recover without disabling caching permanently?

**Candidate Answer:**

I would first assume cache corruption only after excluding deterministic causes. I would compare the exact commit, Dockerfile, base image digest, dependency lock files, BuildKit version, runner image, and registry cache source. Inconsistent checksum or missing layer errors often point to corrupted local BuildKit cache, interrupted pulls, registry cache issues, disk pressure, proxy corruption, or concurrent jobs sharing a broken cache.

Immediate mitigation is to retry on a clean runner, clear only the affected builder cache, disable cache import temporarily, or switch to a known-good registry cache. I would avoid making \`--no-cache\` permanent because it hides the issue and increases build time.

For root cause, I would inspect Docker/BuildKit logs, disk/inode health, registry errors, proxy logs, and whether the runner was terminated during cache write. Validation is a successful clean build and repeated cached builds across multiple runners. Long term, I would use isolated BuildKit builders, cache scopes per repository/branch, registry-backed cache with retention, runner health checks, and alerts for cache error patterns. The key interview point is that build cache is production infrastructure for delivery speed, so it needs reliability controls.

## Q29 \| CI-only build failures \| P1 \| Production Troubleshooting

**Interview Question:**

> A Docker image builds on developer laptops but fails only on Linux CI runners; how would you investigate differences in architecture, filesystem case sensitivity, proxy configuration, build context, secrets, and Docker engine version?

**Candidate Answer:**

I would compare the laptop and CI environments instead of assuming the code is bad. Important differences include OS, CPU architecture, filesystem case sensitivity, Docker/BuildKit version, network/proxy/CA configuration, available secrets, file permissions, line endings, and whether \`.dockerignore\` excludes files differently than expected.

My first checks would be CI build logs with plain progress, build context contents, Dockerfile path, missing files, dependency download failures, UID/GID issues, and architecture-specific packages. If the laptop is macOS/Windows and CI is Linux, case-sensitive import paths or scripts with CRLF endings are common. If the developer uses ARM and CI uses amd64, native dependency compilation can fail.

Short term, I would reproduce locally using the CI platform target, for example \`docker buildx build --platform linux/amd64\`, and fix the deterministic difference. Long term, I would standardize builds through CI as the source of truth, provide dev containers, pin Docker/BuildKit versions where needed, add preflight checks for line endings and missing files, and ensure build secrets/proxy/CA are configured consistently. I would emphasize that “works on my machine” is solved by making the build environment reproducible, not by weakening CI.

## Q30 \| Build secrets \| P1 \| Security

**Interview Question:**

> A private dependency token is needed during image build, but security review finds the token can be recovered from image layers; how would you redesign the build process to prevent secret leakage?

**Candidate Answer:**

I would not pass private dependency tokens using \`ARG\`, \`ENV\`, or \`RUN echo\` because they can leak into image layers, build logs, and history. I would use BuildKit secrets or the CI platform’s secret mount mechanism so the token is available only during the specific build step and is not persisted in the final image.

I would also verify that dependency managers do not write the token into config files copied into later layers. After the build, I would inspect \`docker history\`, scan the final image filesystem, and search logs for accidental exposure.

Example pattern:

\# syntax=docker/dockerfile:1.7

FROM python:3.12-slim AS build

WORKDIR /src

COPY requirements.txt .

RUN --mount=type=secret,id=pip_token PIP_EXTRA_INDEX_URL="\$(cat /run/secrets/pip_token)" pip wheel --no-cache-dir -r requirements.txt -w /wheels

COPY . .

RUN python -m compileall .

FROM python:3.12-slim

COPY --from=build /wheels /wheels

RUN pip install --no-cache-dir /wheels/\*

COPY . /app

WORKDIR /app

CMD \["python","app.py"\]

Short term, I would rotate any token that may have leaked. Long term, I would enforce secret scanning, block secret usage in Dockerfile \`ARG/ENV\`, and require provenance evidence for private dependency access.

## Q31 \| Layer history leakage \| P2 \| Security

**Interview Question:**

> A security audit finds credentials and internal URLs in image history even though they are not present in the final filesystem; how would you identify the leak path and enforce safer Dockerfile patterns across teams?

**Candidate Answer:**

I would treat credentials in image history as a security incident because deleting them from a later layer does not remove them from the image history or registry blobs. First, I would identify exactly what leaked: credentials, internal URLs, tokens, or repository paths. Then I would determine where the image was pushed, who pulled it, whether it reached production, and whether the credential is still valid.

Immediate mitigation is to revoke and rotate exposed secrets, quarantine affected images, remove them from registries where possible, and redeploy clean images built from a corrected Dockerfile. I would not assume \`docker rmi\` on one machine solves the issue because the layer may exist in remote registries, caches, and downstream environments.

For root cause, I would inspect Dockerfile instructions such as \`ARG\`, \`ENV\`, \`RUN echo\`, package-manager config, and copied files. Long term, I would use BuildKit secrets, multi-stage builds that do not copy credential files, secret scanning on Dockerfiles and image layers, registry retention controls, and CI policies blocking suspicious patterns. I would also educate teams that Docker layers are immutable history, so secrets must never enter the build layer in the first place.

## Q32 \| Health checks \| P2 \| SRE

**Interview Question:**

> A container is marked healthy while the application is unable to serve real user traffic due to downstream dependency failure; how would you redesign health checks to balance fast recovery and false-positive avoidance?

**Candidate Answer:**

I would redefine health checks around user-serving readiness, not just process existence. First, I would identify what “healthy” currently checks: PID running, TCP port open, local endpoint, dependency connectivity, or business-critical operation. A container can be marked healthy while it cannot serve users if the check ignores database connectivity, message queue access, cache dependency, configuration load, or degraded thread pools.

Short term, I would adjust the check to fail when the app cannot safely receive traffic, and drain or restart unhealthy containers depending on the failure. I would avoid checks that are too heavy or depend on every downstream dependency, because that can cause cascading restarts during dependency incidents.

My preferred pattern is separate endpoints: liveness for “should this process be restarted,” readiness for “should this instance receive traffic,” and startup for slow initialization. Validation would use load balancer target health, application success rate, P95 latency, error rate, and dependency health. Long term, I would standardize health-check contracts, dashboard them, and review them during production readiness. The senior point is that bad health checks create false confidence and can make outages worse by routing traffic to broken instances.

## Q33 \| Restart policies \| P2 \| SRE

**Interview Question:**

> A bad release causes containers to restart continuously and hides the original failure signal; how would you tune restart behavior, alerting, and crash evidence collection for faster incident response?

**Candidate Answer:**

I would first stop the restart policy from destroying evidence. Restart policies are useful for resilience, but during a bad release they can hide the first failure, flood logs, overload dependencies, and make MTTR worse. I would collect previous logs, exit codes, termination reasons, deployment version, and resource metrics before applying any fix.

Short term, if the release is clearly bad, I would roll back to the last good digest and confirm stability. If the root cause is unknown, I might temporarily reduce replicas, pause rollout, or disable automatic restarts in a controlled non-production reproduction to capture failure details. I would never add aggressive restart loops as the primary fix.

Long term, I would define restart policies based on workload type. Stateless services can restart, but jobs should fail visibly, and stateful services need careful recovery rules. I would add alerts on restart count, CrashLoopBackOff, exit code patterns, and log spikes. I would also ensure the application handles SIGTERM gracefully and writes useful termination messages. In an interview, I would explain that restart policy is a mitigation mechanism, not a substitute for proper health checks, rollback, and RCA.

## Q34 \| Resource limits \| P1 \| SRE

**Interview Question:**

> A containerized service shows random latency spikes and occasional exits after resource limits were tightened for cost optimization; how would you determine whether CPU throttling, memory pressure, or application tuning is responsible?

**Candidate Answer:**

I would investigate latency spikes and exits through the lens of CPU throttling, memory pressure, garbage collection, file descriptors, and noisy-neighbor effects. First, I would compare container limits/requests with actual usage and look at CPU throttling metrics, OOM kills, memory working set, restart count, and P95/P99 latency. A container can have low average CPU but still suffer throttling if limits are too tight.

Immediate mitigation depends on impact: increase CPU/memory limits, scale replicas, roll back a resource-heavy release, or reduce traffic. I would also check whether the application has correct heap settings relative to container memory; JVM and similar runtimes can behave badly if they assume host memory.

For root cause, I would correlate spikes with deployment, traffic, GC pauses, downstream latency, and node pressure. Validation means throttling drops, latency returns to baseline, no restarts/OOMs, and error rate normalizes. Long term, I would define resource sizing from load tests, add VPA recommendations or capacity reviews, set alerts for throttling and memory saturation, and tune runtime settings. I would avoid the junior answer of “just increase limits” without understanding cost and stability trade-offs.

## Q35 \| Volume mounts and permissions \| P1 \| Production Troubleshooting

**Interview Question:**

> A container loses data after restart in one environment but not another; how would you investigate bind mounts, named volumes, ownership, lifecycle assumptions, and backup expectations?

**Candidate Answer:**

I would first determine whether the application data was written to the container writable layer, a named volume, a bind mount, or an external storage backend. If data disappears after restart in one environment, the most likely difference is volume configuration, mount path, permissions, or whether the container was recreated instead of restarted.

My checks would include \`docker inspect\` mounts, Compose/Kubernetes volume definitions, application data path, UID/GID permissions, host path existence, storage driver behavior, and logs showing write failures. I would also check whether a non-root change caused writes to silently fail or fall back to a temporary directory.

Short term, I would stop writes if data loss is ongoing, restore from backup if needed, and redeploy with the correct persistent volume or external storage configuration. Long term, I would move critical state out of ephemeral containers, standardize volume mounts, enforce permission checks at startup, add backup/restore validation, and include persistence tests in environment certification. In the interview, I would clearly state that containers should be treated as ephemeral; persistent data must have an explicit storage contract.

## Q36 \| Corporate proxy and CA certificates \| P2 \| Production Troubleshooting

**Interview Question:**

> Docker build and runtime outbound calls fail only behind the corporate proxy with TLS verification errors; how would you handle proxy settings and internal CA certificates securely across build and runtime images?

**Candidate Answer:**

I would first separate build-time failures from runtime outbound failures because proxy and CA handling may differ. For builds, I would check whether \`HTTP_PROXY\`, \`HTTPS_PROXY\`, \`NO_PROXY\`, and custom CA certificates are passed to the builder and dependency managers. For runtime, I would inspect container environment variables, trust store, application-specific CA configuration, and whether internal endpoints are wrongly routed through the proxy.

Common root causes are missing corporate root CA in the image, incorrect \`NO_PROXY\` for internal services, proxy variables not passed into BuildKit, or dependency tools using their own config. Short term, I would inject the approved CA bundle, configure proxy variables securely, and validate with \`curl -v\`, package manager downloads, and application outbound calls.

Long term, I would publish corporate-ready base images with CA trust configured, provide standard build arguments/secrets for proxy use, and add CI smoke tests for internal and external endpoints. I would also avoid baking proxy credentials into images. Metrics to watch include dependency download failures, TLS handshake errors, HTTP 407/502 rates, and runner-specific failures. A senior answer shows that enterprise networking must be standardized in the container platform.

## Q37 \| Architecture compatibility \| P2 \| Production Troubleshooting

**Interview Question:**

> An image built on an ARM developer machine fails on amd64 production runners with an executable format error; how would you design a multi-architecture build and validation process?

**Candidate Answer:**

I would first inspect the image manifest and the production node architecture. An image built on an ARM laptop can work locally but fail on amd64 runners if it was pushed as a single-architecture ARM image or if native dependencies were compiled for the wrong platform. I would check \`docker buildx imagetools inspect\`, registry manifest list, CI build platform, and error messages such as exec format error.

Short term, I would rebuild the image for the production platform, usually \`linux/amd64\`, and redeploy the corrected digest. If the organization runs mixed architecture nodes, I would publish a multi-arch manifest and ensure each architecture is tested separately.

Long term, I would move release builds to CI-controlled builders, specify \`--platform\`, avoid laptop-built production images, and ensure base images and dependencies support required architectures. I would also add admission or pipeline checks that block single-arch images when multi-arch is required. Validation includes successful container startup on amd64, correct manifest entries, and smoke tests on representative nodes. The senior angle is that architecture compatibility is part of artifact reproducibility and should be enforced before deployment.

## Q38 \| Base image upgrade regression \| P1 \| Implementation

**Interview Question:**

> A minor base image upgrade changes OS packages and breaks application behavior in production; how would you test, pin, promote, and roll back base image upgrades at platform scale?

**Candidate Answer:**

I would treat a minor base image upgrade like any other production dependency change. First, I would compare the old and new base image digests, OS package versions, libc/OpenSSL changes, timezone/CA bundle changes, default user, shell availability, and package-manager behavior. Then I would rebuild the application image in an isolated branch and run smoke, security, and regression tests before promotion.

I would also keep rollback simple by retaining the previous image digest and deployment manifest. The implementation pattern is to pin and test, not float silently.

ARG BASE_IMAGE=eclipse-temurin:21-jre@sha256:OLD_DIGEST

FROM \${BASE_IMAGE}

WORKDIR /app

RUN useradd -r -u 10001 appuser

COPY build/app.jar /app/app.jar

USER 10001

HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://127.0.0.1:8080/health \|\| exit 1

ENTRYPOINT \["java","-jar","/app/app.jar"\]

Short term, I would roll back if the upgrade breaks behavior and capture the package delta. Long term, I would maintain a base-image compatibility test suite, canary rollout, SBOM comparison, and release notes for base images. I would not let teams consume mutable base tags directly in production builds without visibility.

## Q39 \| Image promotion workflow \| P2 \| Platform Engineering

**Interview Question:**

> Multiple teams rebuild the same source separately for dev, staging, and production, causing artifact drift; how would you design a build-once-promote-many container image workflow?

**Candidate Answer:**

I would replace rebuild-per-environment with build-once, promote-many. Rebuilding the same source separately for dev, staging, and production creates non-determinism because dependency resolution, base image tags, build cache, and timestamps can differ. It also makes compliance harder because the production artifact may not be the one tested in staging.

My design would build the image once from a trusted commit, run tests and scans, generate SBOM/provenance, sign the image, and push it to an artifact registry. Promotion would move the same immutable digest through environments with environment-specific configuration supplied outside the image. Approvals, deployment records, and GitOps commits would reference the digest.

Short term, I would identify services currently rebuilding and start with critical production workloads. Long term, I would enforce immutable release tags, block mutable tags in production, create a promotion API or pipeline template, and maintain audit dashboards showing commit, digest, scan status, approver, and environment. Rollback becomes selecting a previous approved digest. In an interview, I would emphasize that the artifact is the unit of release; environment differences belong in configuration, not rebuilt binaries.

## Q40 \| Developer platform guardrails \| P2 \| Platform Engineering

**Interview Question:**

> As a platform owner, how would you create reusable Dockerfile standards and CI checks that enforce small, secure, non-root images without blocking developer productivity?

**Candidate Answer:**

As a platform owner, I would create guardrails that make the secure path easier than custom one-off Dockerfiles. First, I would analyze common language stacks and production requirements: Java, Node, Python, Go, GPU/ML workloads, corporate CA/proxy, non-root user, health checks, logging, and vulnerability scanning. Then I would publish versioned Dockerfile templates and base images with documented contracts.

The CI guardrails would check for required patterns: multi-stage builds where applicable, \`.dockerignore\`, non-root user, no secrets in ARG/ENV, no \`latest\` in production, approved base images, image size budget, SBOM generation, vulnerability threshold, and immutable digest promotion. I would start in warning mode, fix top offenders with teams, then move to blocking mode for high-risk controls.

To avoid slowing developers, I would provide examples, dev containers, reusable pipeline actions, and an exception process with expiry. Metrics would include image size reduction, CVE age, build duration, rollback reliability, and adoption rate. The senior point is that platform standards should improve developer productivity and production safety together. Guardrails should be automated, observable, and pragmatic, not just a wiki page.

## Q41 \| MLOps container images \| P3 \| MLOps

**Interview Question:**

> A model-serving image includes large model artifacts, GPU libraries, and Python dependencies, causing slow rollout and cold-start latency; how would you redesign the container packaging and release approach for production ML serving?

**Candidate Answer:**

I would handle MLOps images differently from normal microservice images because model artifacts, GPU libraries, CUDA compatibility, and Python dependency trees can make images huge and fragile. First, I would separate what must be in the image from what can be pulled as a versioned model artifact at startup or mounted from object storage. For regulated or latency-sensitive serving, baking a specific model version may be acceptable, but for frequent model updates it slows promotion and rollback.

I would pin CUDA, cuDNN, Python, and framework versions, validate against target GPU node drivers, and build images on controlled CI builders for the target architecture. I would keep runtime minimal, avoid notebooks/dev tools in serving images, use model registry metadata, and generate SBOMs for Python and OS packages.

Short term, if deployments are slow, I would split base runtime, serving code, and model artifact lifecycle. Long term, I would standardize ML base images, cache large layers, use canary/shadow deployment for models, monitor P95/P99 inference latency, GPU memory, cold-start time, and model version correctness. In an interview, I would show that ML image design is a release-management problem as much as a Docker-size problem.

## Q42 \| Incident RCA and ownership \| P1 \| Leadership

**Interview Question:**

> A production outage is traced to an unscanned image built from an outdated Dockerfile pattern owned by another team; how would you lead the RCA, define ownership, and implement platform-level prevention?

**Candidate Answer:**

I would lead the RCA without making it a blame exercise against the other team. First, I would stabilize production, confirm the running image digest, identify the outdated Dockerfile pattern, assess the vulnerability or failure mechanism, and ensure the affected image is replaced with a scanned and approved digest. Then I would build a timeline from commit, CI build, scan status, approval, deployment, alert, mitigation, and recovery.

For ownership, I would separate service ownership from platform guardrail ownership. The service team owns its Dockerfile and application runtime behavior, but the platform team owns paved-road templates, CI scanning controls, registry policies, and deployment gates. If an unscanned outdated image reached production, the system allowed it, so the RCA action items must include platform prevention.

My corrective actions would be: block production promotion without scan results, enforce approved base images, generate SBOMs, require immutable digests, alert on stale base images, and create an exception process with expiry. I would also help the service team migrate to the standard template. The measurable outcome I would target is zero unscanned production images, reduced CVE remediation age, and full digest traceability across environments. The communication should be factual, ownership-oriented, and focused on preventing repeat incidents.
