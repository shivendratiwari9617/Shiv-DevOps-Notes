# Senior Docker and Docker Compose Platform, DevOps, and SRE Interview Answers

## Runtime and isolation

### 1. How do Linux namespaces, cgroups, and the container runtime provide isolation? Which security boundaries do they not provide by themselves?

Namespaces isolate views of processes, mounts, networking, users, and other resources; cgroups account for and limit CPU, memory, and I/O. The runtime configures those primitives and starts the container process, but containers on a host still share its kernel. A privileged container, host mount, excessive capabilities, or a kernel vulnerability can cross intended boundaries. For an untrusted tenant I would combine non-root execution, user namespaces where feasible, least capabilities, seccomp, mandatory access controls, restricted mounts, and host-level separation. I would test escape-relevant permissions and noisy-neighbor impact rather than treating an image as a security perimeter.

### 2. What happens when a container starts from an image, from image layers through the runtime to the application process?

The engine resolves an image reference to a digest, downloads missing content-addressed layers, and assembles a read-only image filesystem with a writable container layer. It creates mounts, networking, namespaces, cgroup limits, and a process with the configured user, entrypoint, arguments, and environment. The process runs on the host kernel; the image does not include its own kernel. I would verify the running digest, configuration, and PID 1 during a release because a successful pull or create is not proof the application is ready. Readiness and customer-path checks decide whether traffic is safe to send.

### 3. A container exits immediately even though the image builds successfully. How would you investigate its entrypoint, exit status, environment, filesystem, and dependencies?

Inspect the exit status and startup logs, then compare the resolved entrypoint, command, user, working directory, and environment with the image's intent. Check whether a required file was excluded by build context, a secret or mount is missing, or a dependency is unreachable; distinguish an application exit from OOM or signal termination. Reproduce with the exact image digest and deployment settings, not a local mutable tag. Fix the smallest cause and prove both stable startup and a representative customer request before continuing rollout.

### 4. Why do PID 1, signal handling, and child-process reaping matter for graceful shutdown? How would you test termination behavior?

The entrypoint becomes PID 1 inside the container; a shell wrapper may fail to forward termination signals or reap children. I would use exec-form startup or an appropriate init when child reaping is needed, and ensure the application stops accepting traffic, drains requests, and exits within its grace period. Test with `docker stop` and a production-like termination grace period while work is in flight. Inspect signal receipt, child processes, exit code, and interrupted transactions. A container that disappears on schedule but drops paid orders is not shutting down gracefully.

### 5. A container is slow under load despite low host-wide CPU utilization. How would you investigate cgroup throttling, memory pressure, I/O, and application contention?

Compare p99 request latency with cgroup CPU throttled time, quota and period, memory usage and OOM events, I/O latency, and application queue or lock waits. Host idle CPU does not mean a container with a low CPU quota can use it. Check whether a neighbor or shared storage is the bottleneck before increasing limits, and compare the same digest under controlled load. A capacity change must improve customer latency without exhausting host headroom or degrading neighboring products.

### 6. How would you compare rootless containers, user namespaces, capabilities, seccomp, and mandatory access controls for a workload requiring limited privileges?

Rootless execution limits daemon and container privileges; user namespaces map container users to less privileged host IDs. Dropping capabilities and applying seccomp plus SELinux or AppArmor policies reduce the available kernel and filesystem surface, but each protects a different boundary. I would first identify the exact privileged operation the workload needs, then test a minimal permission set instead of granting `--privileged`. Verify mounts, networking, and observability still work and document any exception with owner and expiry. Separate hosts or stronger isolation may be needed for genuinely untrusted workloads.

## Builds and supply chain

### 7. How do Dockerfile instruction ordering, build context, and cache invalidation affect reproducibility and build performance?

Order stable dependency installation before frequently changing application files, use a small `.dockerignore`, and pin base images and dependency versions. A changed input invalidates that layer's cache and downstream layers; caching can accelerate builds without proving they are reproducible. Compare image digests from controlled clean builds using the same architecture, lockfiles, build arguments, and trusted base digest. Record provenance so the release can be tied to source and dependencies. Optimize cache only after ensuring secrets and unintended files do not enter the build context.

### 8. Design a multi-stage build that separates build dependencies from runtime dependencies. How would you verify that the final image contains only the intended artifacts?

Compile and test in a pinned builder stage, then copy only the executable and required runtime assets into a pinned, minimal runtime stage. Do not copy the whole builder filesystem or package-manager caches; run as a non-root user where practical. Inspect the final filesystem, package inventory, SBOM, and image history, and scan the final digest rather than only the builder. Start the resulting image in a clean environment and exercise the real application path. A smaller image is useful only if it remains debuggable and patchable in the team's operating model.

### 9. How would you compare distroless, minimal, and general-purpose base images for security, compatibility, debugging, and maintenance?

Distroless reduces installed utilities and often attack surface but makes in-container debugging harder; minimal images balance size with some operational tools; general-purpose images improve compatibility at the cost of more packages to maintain. Verify libc, CA certificates, time zone, DNS, shell requirements, and supported architectures for the actual app. A low package count is not a substitute for timely rebuilds, scans, and provenance. I would standardize a small set of supported bases with a debug procedure and time-bound exceptions for incompatible teams.

### 10. A build passes on a laptop but fails on CI with the same commit. How would you investigate architecture, build arguments, dependencies, cache, and build context?

Compare builder versions, target architecture, base digests, build arguments, environment variables, lockfiles, network access, and `.dockerignore` in both environments. Rebuild without cache to expose hidden dependencies and check whether local untracked files were accidentally included. Inspect the first divergent layer or failing command, not just the final error message. Pin inputs and build in a controlled environment, then test the artifact on the target architecture. The business goal is one promotable digest, not a build that only works on a developer machine.

### 11. How would you manage vulnerability scanning, image signing, provenance, and base-image updates across hundreds of services without treating any one control as sufficient?

Build an approved base-image pipeline with pinned digests, SBOMs, vulnerability triage, signed provenance, and a measurable rebuild cadence. Scan both dependencies and final images, but prioritize exploitability, reachability, and customer exposure rather than blocking on every unfixable finding. Enforce artifact verification at deployment and stage base updates through representative consumers before broad rollout. Track time to remediate critical exposure, unsupported bases, exceptions, and release failures. Signing says where an artifact came from; it does not prove its contents are safe.

### 12. A secret was accidentally copied into an image layer. How would you determine exposure, contain it, rotate the secret, and prevent recurrence?

Assume the secret remains recoverable from the affected image's layers even if a later Dockerfile step deletes the file. Identify digest, registry and cache copies, pull permissions, deployments, and log or artifact propagation; restrict access and revoke or rotate the credential immediately. Preserve incident metadata without reproducing the secret, then remove the affected artifact where feasible and rebuild from a clean context. Use build secrets or runtime secret injection, `.dockerignore`, and scanning to prevent reintroduction. Verify the replacement digest works and the old credential no longer authorizes access.

### 13. How would you build once and promote the same image through environments? Why is an immutable digest more useful for rollback than a mutable tag?

Build and test one image, record its digest, provenance and scan results, and deploy that exact digest in each environment with environment-specific configuration injected at runtime. A tag can move to a new manifest without changing the deployment declaration; a digest identifies the actual artifact. Record which digest runs on every target and tie approvals to that digest. Roll back to a known good digest only after checking whether data or schema changes make the earlier version unsafe. Measure customer errors and readiness during each promotion step.

## Networking and storage

### 14. Two containers can resolve each other's names but cannot establish a connection. How would you investigate listeners, container networks, published ports, and host firewalls?

Check whether the server listens on the container interface and correct port rather than only `127.0.0.1`. Verify both containers share the intended network, then test TCP connectivity by name and IP from the client and inspect firewall and network policy rules. Published host ports matter for host-to-container traffic, not ordinary peer traffic on the same user-defined network. Separate connection refusal, timeout, and application authorization from name resolution. Restore the smallest broken path and retest a real request.

### 15. How would you compare bridge and host networking for isolation, port management, performance, and debugging?

Bridge networking provides per-container network namespaces and service DNS on user-defined networks, with explicit port publishing for host ingress. Host mode shares the host network namespace, removing port-mapping isolation and increasing the risk of port collisions; its performance benefit must be measured, not assumed. Compare source-IP needs, firewall model, supported platforms, and troubleshooting before choosing. For multi-team hosts I would prefer bridge unless a measured workload constraint justifies host mode. Test that the chosen path preserves customer traffic and tenant isolation.

### 16. An application works with a bind mount locally but fails with files copied into the image in CI. How would you investigate permissions, path assumptions, and hidden image contents?

A local bind mount can hide missing files in the image and substitute the developer's permissions and paths. Compare effective user and UID/GID, file modes, working directory, ownership after `COPY`, `.dockerignore`, and what each mount shadows. Inspect the final image without the bind mount and reproduce with the CI digest. Fix the build context or ownership rather than making the runtime privileged. Add a clean-image startup test so laptop-only files cannot mask a broken release.

### 17. How do named volumes, bind mounts, and a container's writable layer differ in lifecycle and portability? When would each be appropriate?

The writable layer belongs to the container and is unsuitable for durable data; replacing the container can discard it. Named volumes have a separate lifecycle but still need an explicit host-failure backup and migration strategy. Bind mounts expose host paths and permissions, useful for local development or deliberate host integration but fragile as an unstated production dependency. Choose based on data durability and operational ownership, then test replacement, restore, and access controls. A running container does not imply its state can survive a host failure.

### 18. A stateful container is recreated after a host failure. What must be true about persistent storage, backup, restore, and application consistency for recovery to succeed?

Data must live outside the discarded container layer, with backups or replication that survive the failed host. Validate application-consistent recovery, storage attachment or restore on another host, permissions, encryption keys, and dependent services. Measure RPO using the latest verified durable transaction and RTO when the customer workflow works, not when the container starts. Establish one active writer to prevent replay or split-brain. A Docker volume on the lost host alone is not disaster recovery.

### 19. Disk usage on a Docker host grows rapidly although running containers report little data. How would you investigate image layers, logs, volumes, and deleted-but-open files safely?

Compare filesystem usage with Docker's image and volume accounting, container log size, build cache, and `lsof +L1` for deleted-but-open files. Check which filesystem is full and whether a shared volume or active image is still referenced. Do not run broad prune or delete active log files during an incident; preserve evidence and identify safe reclaimable data first. Set log rotation, image retention, and build-cache limits after customer service is stable. Verify disk headroom and the root cause of growth, not just a one-time space recovery.

## Compose, delivery, and incidents

### 20. How would you design a Docker Compose environment for a multi-service application while keeping local conveniences from masking production failure modes?

Use pinned image digests, explicit service networks, healthchecks, required secrets, resource assumptions, and a reproducible data seed. Compose should make dependencies visible but not imply that startup order equals application readiness or that local bind mounts and permissive ports exist in production. Test database restarts, delayed dependencies, termination, and volume recovery locally, then run separate production-like integration tests for the real orchestrator. Product teams benefit when local iteration is fast and failure behavior remains honest. Document intentional differences rather than claiming Compose is a production clone.

### 21. A Compose service starts before its dependency is ready and fails intermittently. How would you distinguish startup order from actual readiness and design a resilient startup path?

Compare timestamps, connection errors, and the dependency's healthcheck result; name resolution and process start do not establish readiness. Compose health conditions can delay initial startup, but the client must still tolerate dependency loss after it has started. Add bounded retries with backoff and idempotent initialization, rather than a fixed sleep or infinite restart loop. Kill and restart the database during a test and verify the service recovers without losing or duplicating customer work.

### 22. How would you manage Compose configuration differences across developers and CI without committing credentials or allowing unexpected configuration overrides?

Keep a reviewed base Compose file and explicit environment overlays with documented precedence; inspect the resolved configuration using `docker compose config` in CI. Supply secrets through an approved runtime mechanism or protected files outside source control, not a committed `.env`. Pin image digests and validate required variables so missing or stale developer settings fail early. Test that CI cannot inherit laptop-specific bind mounts, ports, or credentials. Audit the deployed configuration without printing secret values.

### 23. When would Compose be sufficient for a workload, and when would you choose an orchestrator or a different local development workflow?

Compose is effective for local development, CI integration tests, and small workloads where a host failure and manual recovery fit the service objective. Choose an orchestrator when multi-host scheduling, rolling updates, self-healing, autoscaling, or stronger tenant isolation are required. Avoid adopting a cluster merely for perceived maturity: compare customer RTO, staffing, operational cost, and delivery frequency. If local Compose diverges badly from production, add focused contract tests in the production platform instead of pretending parity. The decision should follow measured failure requirements.

### 24. A new image starts successfully but receives termination signals incorrectly during deployment. How would you distinguish shell-form and exec-form startup behavior and verify a fix?

Inspect the container's actual PID 1 and process tree; a shell-form entrypoint may run a shell that does not forward SIGTERM to the application. Use exec-form startup or `exec` in a wrapper, and add an init only if child reaping requires it. Test graceful shutdown with in-flight traffic under the deployment grace period and observe signal receipt, drain, exit code, and interrupted writes. A successful process start proves nothing about safe rolling deployment. Promote only after the customer path remains healthy during replacement.

### 25. A container is repeatedly killed for memory pressure, but the application reports no error. Which host, runtime, and application evidence would you collect before changing its memory limit?

Inspect container OOM state, exit reason, cgroup memory events, kernel OOM logs, and host pressure before treating a nonzero exit as a container-limit kill. Compare process RSS, page cache, transient peaks, heap settings, and memory graphs' sampling interval; short spikes can disappear between samples. If the limit is too low, test a measured increase against host headroom; if memory leaks, adding a limit only delays recurrence. Verify customer latency and neighboring services during the fix. Preserve a clear distinction between container OOM and host-level termination.

### 26. A registry outage delays deployments. How would you determine the affected image and rollout scope, preserve service, and reduce future dependency on a single registry path?

Identify exactly which digests are missing from hosts or nodes and which deployments require a new pull; running containers may continue serving. Pause upgrades and restarts that would lose cached capacity, and check registry reachability, authentication, and mirror health. Use an approved replicated registry or tested mirror with digest verification, not an unreviewed rebuild under the same tag. Restore rollouts only after a test pull and customer-health gate. Record which provider failure exposed the dependency and test it in a later recovery exercise.

### 27. How would you balance container image standards with team-specific runtime requirements, and how would you handle exceptions?

Provide a small supported set of versioned bases with patch cadence, provenance, non-root defaults, and an easy template for common languages. Let teams choose supported variants based on libc, certificates, architecture, and debug needs rather than forcing one image that breaks legitimate workloads. Exceptions need an owner, expiration, compensating scan or isolation, and a migration plan. Measure adoption, build time, critical vulnerability age, and release failure rate. A platform standard succeeds when it lowers team effort without silently shifting security risk to product teams.

### 28. Tell me about a container incident where a host-level assumption differed from the application team's expectation. How did you identify the boundary and prevent recurrence?

Use your own incident, not an invented first-person story. Structure it as the host assumption, the app team's competing assumption, the evidence that resolved it, the safe customer mitigation, and the prevention check. Examples to choose from only if real include CPU quota despite idle host CPU, bind-mounted files missing from the image, or a host-local volume mistaken for durable storage. Explain what you personally changed and give before-and-after impact or detection time. An interviewer should be able to ask what you would do differently without exposing a scripted answer.

## Evidence-based exercises

### 29. A container exits with code 137 after its memory usage spikes, but no application exception appears. Which container, kernel, and cgroup evidence would distinguish an out-of-memory kill from another cause?

Exit 137 means termination by SIGKILL; it does not by itself prove OOM. Check `docker inspect` for OOMKilled, cgroup `memory.events`, kernel OOM logs, host memory pressure, and orchestrator events, then align timestamps with the memory spike. An explicit `kill -9` or a forced stop after grace-period expiry can produce the same exit code. Compare memory limits, peak usage, and sampling resolution before increasing capacity. Confirm the true cause and customer impact in a controlled restart test.

### 30. A Dockerfile copies the entire source tree before installing dependencies, and a one-line application change invalidates the dependency-install layer. How would you redesign and test the build cache while protecting secrets and reproducibility?

Copy dependency manifests and lockfiles first, install pinned dependencies, then copy application sources; use `.dockerignore` to exclude credentials, build outputs, and local caches. Use BuildKit secrets for private registries instead of baking tokens into layers. Test one clean build and a second build with only an application file changed, comparing layer reuse and final runtime behavior. Then build cleanly again with pinned base and dependency inputs to assess reproducibility; a cache hit is not proof of identical outputs. Promote the tested digest, not a freshly rebuilt tag.

### 31. A service launched with Compose resolves `db` but receives `connection refused` immediately after startup; the database becomes reachable later. What evidence would you gather to distinguish readiness from networking, and how would you test recovery after a database restart?

`Connection refused` after successful DNS generally means the destination was reached but no process accepted that port at that moment; inspect the DB listener, startup logs, health status, and network namespace. A health-based initial dependency order can help, but the client must reconnect when the DB restarts. Add bounded retries and verify no duplicate or lost transactions on reconnection. Stop and start `db` during a test and watch both application success rate and database state. Do not mask the problem with a fixed delay.

### 32. An image is tagged `release`, deployed successfully, and later rebuilt using the same tag. How would you identify the exact image running on each host and design a verifiable rollback?

Freeze tag reuse and inspect each running container's immutable image ID (`docker inspect --format '{{.Image}}' <container>`), then resolve that local ID against recorded pull/deployment provenance and the registry manifest digest for its platform. The local image ID is a config digest, **not necessarily the registry manifest digest**; `RepoDigests` may be missing for locally built images. Compare the digest and architecture with the build's commit, signature, and approval, and inventory hosts receiving each build. Roll back to the previously verified platform-specific manifest digest only if schema and data remain compatible, then check customer errors and transactions on every host. Pin future promotion to immutable digests and record the actual running image IDs after deployment.

## Product-company practical exercises

### 33. A production Dockerfile contains `FROM node:latest`, `COPY . /app`, and `RUN npm install`; two builds from the same commit produce different images. How would you investigate the inputs and redesign the build so the result can be reproduced and verified?

Compare the resolved `node:latest` digest, architecture, dependency lockfiles, npm registry resolution, build context, and build-time environment for both runs. Pin the base by digest, use `npm ci` with a committed lockfile, exclude local secrets and artifacts, and fix build arguments and architecture. Build in a controlled CI environment and compare SBOMs and runtime behavior; byte-identical digests may also require controlling timestamps and tooling. Store provenance linking the digest to commit and builder. Verify a customer smoke test before promoting the resulting image.

### 34. During a product launch, containers show increasing request latency while host CPU appears available; `docker stats` shows each container close to its configured CPU limit. What would you measure before changing limits, and how would you verify that the change does not harm neighboring services?

Correlate p99 latency and request queue depth with cgroup CPU throttling, quota, process CPU, node headroom, and adjacent containers' usage. If throttling aligns with customer harm, canary a higher limit or remove an unjustified cap while monitoring host contention and costs. Check downstream latency so a faster container does not overload a database or payment service. Roll back if neighbor SLIs regress or the launch traffic exceeds safe capacity. Idle host CPU alone is not a reason to raise every container limit at once.

### 35. Several teams need consistent container builds but use different languages and release cycles. How would you design an image and build standard with an exception path and measurable adoption?

Offer versioned language-specific starter builds with common provenance, scanning, SBOM, base update, and digest promotion requirements, not one universal Dockerfile. Test each template with representative products and publish a supported maintenance window so fast and slow release teams can plan upgrades. Exceptions should specify unsupported runtime needs, an owner, compensating controls, and an expiry. Measure time to first reproducible build, base-version adoption, patch latency, and build-related incidents. A standard is valuable when product teams ship faster without hiding supply-chain risk.

### 36. The mutable `release` tag resolves to different image digests in staging and production, and errors rise after promotion. How would you identify what is running, verify image provenance, stop further rollout, and perform a digest-pinned rollback?

Pause promotion and query actual running image IDs/digests in both environments; record customer errors by host and digest. Compare registry manifest history, build source, and provenance to determine whether the production artifact was tested at all. If the previous production digest remains compatible with current data, deploy it explicitly and verify error and transaction recovery before resuming rollout. Investigate how tag movement bypassed approval, then bind promotion and rollback to immutable digests. Do not rebuild the same source and assume it reproduces the former image.