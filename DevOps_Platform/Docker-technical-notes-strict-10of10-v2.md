# Docker Technical Notes

## 1. Architecture and Runtime Model

Docker is a container platform centered on image-based packaging, layered filesystems, registry distribution, and isolated process execution.  
Its practical model separates image construction, image distribution, and container runtime execution.

### 1.1 Client, Daemon, and API

Docker commonly operates through a client-server model.

#### Docker client

The Docker CLI is the primary user interface for building images, running containers, inspecting objects, and interacting with registries.  
Most CLI operations are translated into API requests rather than acting on containers directly.

#### Docker daemon

The Docker daemon manages images, containers, networks, volumes, build operations, and API requests.  
It is the long-running background service that performs the actual work requested by the client.

#### Docker API

The Docker API is the control surface exposed by the daemon.  
The CLI, automation, and remote tools all interact with Docker through this API.

**Commands**

```sh
docker version
docker info
docker context ls
docker system info
```

### 1.2 Core Object Types

Docker manages several core object types.

#### images

An image is a read-only packaged filesystem plus metadata used to create containers.  
It is typically built from a Dockerfile and distributed through a registry.

#### containers

A container is a running or stopped instance created from an image plus runtime configuration.  
It adds a writable container layer on top of image layers.

#### networks

A network defines how containers communicate with each other and with external systems.  
Different network drivers provide different isolation and connectivity models.

#### volumes

A volume is Docker-managed persistent storage decoupled from the container writable layer.  
It is the preferred mechanism for durable application data in most Docker deployments.

### 1.3 OCI Relationship

Docker builds on standards that are broader than Docker itself.

#### OCI image format

The OCI image format standardizes how container images are structured and distributed.  
Docker images commonly align with OCI-compatible registries and tooling.

#### OCI runtime

The OCI runtime specification standardizes how a container runtime executes a container bundle.  
Docker commonly relies on lower-level runtime components under the daemon.

### 1.4 Runtime Chain

A Docker command usually travels through multiple layers before a container process starts.

#### practical chain

A typical flow is: Docker CLI -> Docker API -> Docker daemon -> container management layer -> OCI runtime -> Linux kernel primitives.  
This matters because failures can originate in image resolution, daemon behavior, storage setup, OCI runtime execution, or kernel enforcement rather than in the application alone.

### 1.5 Build and Runtime Separation

Image construction and container execution are distinct phases.  
A good production workflow keeps build-time concerns out of runtime images whenever possible.

---

## 2. Images, Layers, Manifests, and Registries

Docker images are composed of layers and metadata.  
Understanding image structure is critical for build performance, distribution efficiency, and security review.

### 2.1 Image Layers

Each Dockerfile instruction that changes filesystem state usually creates a new image layer.  
Layers are content-addressed and reused across images when their contents match.

#### immutability

Image layers are immutable after creation.  
A new build does not edit an existing layer; it creates a new one and references prior reusable layers where possible.

#### layer caching

Build caching reuses previous layers when the relevant instruction and its inputs have not changed.  
Instruction ordering therefore has major effects on rebuild speed.

### 2.2 Writable Container Layer

A running container adds a writable layer above image layers.  
This layer is ephemeral unless data is written to mounted persistent storage.

#### production caveat

Application state written only to the container writable layer is usually lost when the container is removed.  
Durable data should live in volumes, bind mounts where appropriate, or external storage systems.

### 2.3 Image Tags and Digests

Tags are human-friendly references, while digests are immutable content references.

#### tag

A tag such as `nginx:1.27` is a mutable label pointing to an image manifest.  
Tags are convenient but should not be treated as immutable guarantees.

#### digest

A digest such as `sha256:...` identifies image content immutably.  
Production-sensitive deployments often prefer digests for supply-chain determinism.

**Commands**

```sh
docker image ls
docker pull nginx:1.27
docker inspect nginx:1.27
docker images --digests
```

### 2.4 Image Manifests and Manifest Lists

An image reference does not always point to a single platform image blob.

#### manifest

A manifest describes the config object and layers for a specific platform image.  
It is the concrete metadata the registry serves for that image variant.

#### manifest list

A manifest list, or OCI image index, points to multiple platform-specific manifests.  
This allows the same image name to resolve differently on `linux/amd64`, `linux/arm64`, and other targets.

#### production caveat

Cross-platform builds and pulls can appear to work locally while failing in CI or production if the platform target is not explicit.  
Platform awareness matters especially in mixed ARM and x86 environments.

### 2.5 Registries

A registry stores and distributes images.  
Public and private registries support image push, pull, tagging, access control, and metadata handling.

#### Docker Hub

Docker Hub is the default public registry in many Docker workflows.  
It is convenient but subject to access policy, rate limits, and supply-chain trust considerations.

#### private registries

Private registries are commonly used for internal images, controlled access, and regulated environments.  
They are often integrated with vulnerability scanning, retention rules, and signing workflows.

**Commands**

```sh
docker login
docker tag myapp:1.0 registry.example.com/team/myapp:1.0
docker push registry.example.com/team/myapp:1.0
docker pull registry.example.com/team/myapp:1.0
```

### 2.6 docker history and Layer Review

Layer review helps explain image size, secrets exposure risk, and unexpected filesystem contents.

**Commands**

```sh
docker history myapp:1.0
docker image inspect myapp:1.0
```

#### production caveat

Deleting a secret in a later layer does not remove it from earlier committed layers.  
If the secret ever entered the image layer graph, it must be treated as exposed.

---

## 3. Dockerfile Semantics and Build Behavior

A Dockerfile defines how an image is built.  
Its instructions are simple individually but have important caching, security, and lifecycle implications.

### 3.1 Core Instructions

#### FROM

`FROM` selects the base image for a build stage.  
Every build stage starts from its own `FROM`.

#### WORKDIR

`WORKDIR` sets the default working directory for following instructions and container startup context.  
It is preferable to repeated `cd`-style shell constructs.

#### COPY

`COPY` copies files from build context into the image.  
It is deterministic relative to the provided build context and should usually be preferred over `ADD` unless extra behavior is required intentionally.

#### ADD

`ADD` can copy local files, extract local archives, and fetch remote URLs.  
Because it has extra behavior, it should be used only when that behavior is explicitly desired.

#### RUN

`RUN` executes commands during build and creates a new layer based on the result.  
It is used for package installation, compilation, and image filesystem preparation.

#### ENV

`ENV` sets environment variables in image metadata for later build steps and runtime defaults.  
It persists into containers unless overridden.

#### ARG

`ARG` defines build-time variables available during the build.  
Unlike `ENV`, it does not automatically persist into the final runtime environment.

#### CMD

`CMD` defines the default command for the container when no command override is provided.  
It is commonly used together with `ENTRYPOINT`.

#### ENTRYPOINT

`ENTRYPOINT` defines the main executable for the container.  
It is useful when the image should always behave like a specific program.

#### EXPOSE

`EXPOSE` documents intended listening ports in image metadata.  
It does not publish ports by itself.

#### USER

`USER` sets the default runtime user for following instructions and container startup.  
Running as a non-root user is a major hardening baseline.

#### HEALTHCHECK

`HEALTHCHECK` defines a command used to assess container health.  
It supplements but does not replace process supervision or application readiness design.

#### VOLUME

`VOLUME` marks a mount point as externally managed storage.  
It can be useful, but explicit runtime mounts are usually easier to reason about operationally.

#### SHELL

`SHELL` changes the default shell used for shell-form instructions.  
This can affect quoting, error handling, and portability in subtle ways.

#### STOPSIGNAL

`STOPSIGNAL` defines which signal Docker should use for normal stop requests.  
It is useful when an application expects something other than the default signal behavior.

### 3.2 Exec Form vs Shell Form

Instruction form changes signal handling, quoting behavior, and process structure.

#### exec form

Exec form uses JSON-array syntax, for example:

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

It launches the target process directly without wrapping it in a shell.  
This is generally better for predictable signal delivery and argument handling.

#### shell form

Shell form uses a string, for example:

```dockerfile
CMD nginx -g 'daemon off;'
```

It runs through a shell, which changes quoting semantics and can complicate signal propagation.

### 3.3 Build Context

The build context is the directory tree sent to the daemon for the build.  
Large or noisy build contexts slow builds and can accidentally leak files into images.

#### .dockerignore

`.dockerignore` excludes files from the build context.  
It is critical for performance, smaller context transfer, and preventing secrets or irrelevant artifacts from entering builds.

**Example `.dockerignore`**

```text
.git
node_modules
dist
.env
*.log
```

#### production caveat

A secret omitted from `COPY` can still leak into build operations if it is present in the build context and not ignored.  
Context hygiene is therefore part of security, not only performance.

### 3.4 Layer Ordering for Cache Efficiency

Stable instructions should come before frequently changing instructions where possible.  
Dependency installation should often be separated from application source copy so that code changes do not invalidate expensive dependency layers unnecessarily.

### 3.5 Multi-Stage Builds

Multi-stage builds separate build tooling from runtime output.  
They are one of the most important Dockerfile practices for production image quality.

**Dockerfile**

```dockerfile
FROM golang:1.24 AS build
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /out/app ./cmd/app

FROM gcr.io/distroless/static-debian12
COPY --from=build /out/app /app
USER nonroot:nonroot
ENTRYPOINT ["/app"]
```

#### production value

Multi-stage builds reduce final image size, shrink attack surface, and avoid shipping compilers and package managers into runtime images.  
They also improve supply-chain review by making the runtime stage easier to inspect.

### 3.6 BuildKit

BuildKit is the modern build engine behind advanced Docker build capabilities.  
It improves caching, parallelism, frontend features, secret handling patterns, and remote build workflows.

#### why it matters

Build behavior, caching semantics, and supported features differ materially once advanced build features are used.  
BuildKit is not just a speed improvement; it changes what secure and efficient builds can do.

#### cache nuance

Build caches can be local or externalized, and they may outlive any one single build invocation.  
Cache strategy matters in CI because a build can be correct but still be unnecessarily slow or inconsistent across runners.

### 3.7 Build Secrets and Sensitive Material

Build-time secrets should not be copied into layers.  
If a secret enters a committed image layer, deleting the file in a later layer does not truly remove it from image history.

#### production caveat

Never place long-lived secrets in `ENV`, plain `ARG`, copied files, or committed layers unless that is explicitly acceptable.  
Secret handling during builds requires deliberate mechanisms and review.

### 3.8 Cache Busting Caution

Forcing cache invalidation can be useful, but careless cache busting destroys build efficiency and reproducibility.  
Cache bypass should be a conscious decision tied to dependency freshness or incident response, not a default habit.

---

## 4. Container Lifecycle and Process Model

A Docker container is fundamentally a process with isolation and filesystem packaging.  
Production correctness depends on understanding that the container lives and dies with its main process.

### 4.1 Create, Start, Stop, Remove

The container lifecycle includes object creation, process startup, optional stopping, and removal.

**Commands**

```sh
docker create --name web nginx:1.27
docker start web
docker stop web
docker rm web
```

### 4.2 Running vs Exited

A container is considered running while its main process is alive.  
If the main process exits, the container exits, even if background child processes remain conceptually useful.

### 4.3 PID 1 Behavior

Inside a container, the main process usually runs as PID 1.  
PID 1 has special signal-handling and child-reaping responsibilities in Linux.

#### signal handling caveat

If the main process does not handle termination signals correctly, graceful shutdown can fail or stall.  
This can lead to delayed stop operations, incomplete cleanup, or abrupt forced termination.

#### zombie reaping caveat

If PID 1 does not reap orphaned child processes properly, zombies can accumulate inside the container.  
Minimal wrapper scripts or poorly designed entrypoints can cause this in real deployments.

### 4.4 ENTRYPOINT, CMD, and Overrides

`ENTRYPOINT` defines the executable identity of the image, while `CMD` usually provides default arguments or a default command.  
Runtime overrides behave differently depending on how these are combined.

#### common pattern

```dockerfile
ENTRYPOINT ["python", "app.py"]
CMD ["--port", "8080"]
```

This allows default arguments to be overridden while preserving the base executable.

### 4.5 Init Process Options

Some applications benefit from a tiny init process in front of the main program.

#### practical purpose

An init helper can improve signal forwarding and zombie reaping behavior.  
This is useful when the application itself is not a good PID 1 citizen.

#### production caveat

Adding an init process is helpful when justified, but it should not become an excuse to ignore application shutdown behavior.  
The application still needs correct signal handling and cleanup logic.

### 4.6 Signal Delivery and Graceful Shutdown

`docker stop` sends a termination signal and waits before force-killing the container.  
Applications must be designed to flush work, close listeners, and stop cleanly during this window.

**Commands**

```sh
docker stop myapp
docker kill myapp
docker kill --signal=SIGTERM myapp
```

#### shutdown nuance

Graceful shutdown is not only about exiting eventually.  
It also includes draining traffic, finishing in-flight work where appropriate, closing connections, and persisting final state safely.

### 4.7 Restart Policies

Restart policies govern what Docker should do after container exit.

#### no

Do not restart the container automatically.

#### on-failure

Restart only when the process exits with a non-zero status.  
This is useful for crash-like failures but not for deliberate stops.

#### unless-stopped

Restart the container automatically except when it has been explicitly stopped.  
This is a common local or simple host-level service pattern.

#### always

Always attempt to restart the container when it exits.  
This is simple but can obscure crash loops if monitoring is weak.

**Commands**

```sh
docker run -d --restart unless-stopped --name web nginx:1.27
docker update --restart always web
```

#### production caveat

Restart policies are not a substitute for orchestration, health-aware rollout, or proper observability.  
They only restart a container; they do not solve dependency ordering, readiness, placement, or traffic management.

### 4.8 Health Checks

Health checks allow Docker to report container health state based on a command.  
They help distinguish “process exists” from “application is functioning.”

**Dockerfile**

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --retries=3 CMD wget -qO- http://localhost:8080/health || exit 1
```

**Commands**

```sh
docker ps
docker inspect --format '{{json .State.Health}}' myapp
```

#### production caveat

A health check should be cheap, meaningful, and stable.  
Expensive or flaky health checks create false failure signals and can hide real application issues.

---

## 5. Storage, Mounts, and Filesystem Strategy

Container filesystems have multiple layers of persistence behavior.  
Production systems should distinguish clearly between image contents, writable layers, bind mounts, tmpfs, and managed volumes.

### 5.1 Container Writable Layer

The writable container layer stores runtime file changes not written to mounted storage.  
It disappears with container removal.

### 5.2 Volumes

Volumes are Docker-managed persistent storage objects.  
They are usually the preferred mechanism for application data that must survive container replacement.

**Commands**

```sh
docker volume create app-data
docker volume ls
docker volume inspect app-data
docker run -d -v app-data:/var/lib/app myapp:1.0
```

### 5.3 Bind Mounts

Bind mounts map a host filesystem path into the container.  
They are useful for local development, controlled host integration, and some operational cases.

**Commands**

```sh
docker run --rm -v "$PWD":/workspace -w /workspace alpine:3.21 ls
```

#### bind mount caveat

Bind mounts couple the container to host path layout, permissions, and host data lifecycle.  
This makes them less portable and sometimes less predictable than volumes.

### 5.4 Volumes vs Bind Mounts

Volumes are generally better for durable application data and portability.  
Bind mounts are often better for live source code, host config injection, or controlled host integration.

### 5.5 tmpfs Mounts

A `tmpfs` mount stores data in memory rather than on persistent disk.  
It is useful for ephemeral sensitive data or high-speed scratch space.

**Commands**

```sh
docker run --rm --tmpfs /run:rw,noexec,nosuid,size=64m alpine:3.21 mount
```

### 5.6 File Ownership and Permissions

Filesystem access inside the container depends on runtime UID and GID, mount options, and host-side ownership.  
Permission issues are especially common when non-root containers interact with host-backed files.

#### production caveat

A container that works as root may fail as a non-root user when mounted paths are introduced.  
Ownership strategy should be designed intentionally rather than patched after deployment failure.

### 5.7 Storage Driver Awareness

Docker relies on storage drivers to present layered filesystems efficiently.  
You do not need daily storage-driver tuning to use Docker, but storage behavior can affect performance, compatibility, and debugging.

#### practical note

Large image churn, copy-on-write behavior, and filesystem-heavy workloads can behave differently depending on the underlying storage driver and host filesystem.  
Deep performance anomalies are sometimes storage-stack issues rather than application bugs.

---

## 6. Networking

Docker networking determines how containers communicate internally and externally.  
The right network mode depends on isolation needs, service discovery expectations, and host integration constraints.

### 6.1 Bridge Network

The default bridge network provides NATed connectivity for containers on a host.  
It is suitable for many local and single-host use cases.

**Commands**

```sh
docker network ls
docker network inspect bridge
docker run -d --name web -p 8080:80 nginx:1.27
```

### 6.2 User-Defined Bridge Networks

User-defined bridge networks provide better service discovery and isolation than the default bridge.  
Containers on the same user-defined bridge can typically resolve each other by name.

**Commands**

```sh
docker network create app-net
docker run -d --name db --network app-net postgres:16
docker run -d --name api --network app-net myapi:1.0
docker network inspect app-net
```

### 6.3 Host Network

Host networking places the container in the host network namespace.  
It removes normal network isolation and avoids container-side NAT for that workload.

#### production caveat

Host networking can simplify some performance-sensitive or infrastructure use cases, but it increases collision risk and weakens isolation.  
It should be used deliberately, not as a default convenience shortcut.

### 6.4 None Network

The `none` network disables normal container networking.  
It is useful for tightly controlled jobs or highly isolated processing tasks.

### 6.5 Port Publishing

Port publishing maps a host port to a container port.

**Commands**

```sh
docker run -d -p 127.0.0.1:8080:80 nginx:1.27
docker port <container_id>
```

#### security caveat

Publishing to all interfaces exposes the service beyond localhost unless firewall policy says otherwise.  
Binding explicitly to loopback is safer for local-only services.

#### localhost misunderstanding

`localhost` inside a container is the container itself, not the host machine.  
This misunderstanding causes frequent connection failures in local development and troubleshooting.

### 6.6 DNS and Name Resolution

Containers on user-defined bridge networks typically receive embedded DNS-based service discovery by container name.  
DNS behavior differs from the default bridge and from external orchestration systems.

### 6.7 Network Isolation Caveat

Containers on the same Docker network can usually communicate unless additional controls are applied.  
Do not assume application-level separation simply because containers are different objects.

---

## 7. Compose and Multi-Container Topology

Docker Compose defines multi-container applications declaratively.  
It is especially useful for local development, integration environments, and simple host deployments.

### 7.1 Core Compose Model

A Compose application usually includes services, networks, volumes, environment configuration, and optional profiles.  
The file expresses desired local topology rather than imperative start-order scripting.

### 7.2 Minimal Example

**`compose.yaml`**

```yaml
services:
  web:
    image: nginx:1.27
    ports:
      - "8080:80"
    depends_on:
      - api

  api:
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      APP_ENV: dev
    healthcheck:
      test: ["CMD", "wget", "-qO-", "http://localhost:8080/health"]
      interval: 30s
      timeout: 3s
      retries: 3
    networks:
      - appnet

  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - dbdata:/var/lib/postgresql/data
    networks:
      - appnet

networks:
  appnet:

volumes:
  dbdata:
```

**Commands**

```sh
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
```

### 7.3 depends_on Caveat

`depends_on` controls startup ordering, not full application readiness semantics.  
A started database container is not necessarily ready to accept connections.

### 7.4 Health and Dependency Nuance

Compose health checks improve visibility, but service success still depends on application retry behavior and failure handling.  
A robust service should tolerate dependent service delay rather than assuming perfect sequencing.

### 7.5 Profiles and Overrides

Compose supports configuration layering and targeted service activation.  
This is useful for optional infrastructure such as observability services, local mocks, debug tooling, or alternate developer flows.

### 7.6 Environment File and Precedence Nuance

Compose environment values can come from multiple places including inline values, environment files, and shell environment.  
Configuration precedence should be understood explicitly so production behavior is not inferred from accidental local shell state.

### 7.7 Restart Behavior in Compose

Restart behavior in Compose still depends on underlying container restart settings rather than Compose acting like a full orchestrator.  
Compose can define useful local service behavior, but operational expectations should remain modest.

### 7.8 Production Caveat

Compose is excellent for local workflows and some simple server use cases, but it is not a full replacement for cluster orchestration.  
Operational expectations around rollout control, scaling, placement, self-healing, and traffic management should remain realistic.

---

## 8. Logging, Events, Inspection, and Troubleshooting

Operational visibility depends on understanding what Docker surfaces directly and what must be handled by the application or external systems.

### 8.1 Logs

Docker commonly captures stdout and stderr from the main process.  
Applications should generally log to standard streams rather than hidden in-container files.

**Commands**

```sh
docker logs myapp
docker logs -f myapp
docker logs --tail 100 myapp
```

### 8.2 Logging Drivers

Docker supports multiple logging drivers with different retention, shipping, and operational tradeoffs.  
Driver choice affects where logs go, how they are stored, and how easily they can be queried later.

#### production caveat

Logging configuration is part of operational design, not an afterthought.  
A container that logs correctly to stdout can still be operationally weak if host retention, rotation, or downstream shipping is poor.

### 8.3 Inspect

`docker inspect` exposes detailed structured metadata for Docker objects.  
It is one of the most important troubleshooting tools.

**Commands**

```sh
docker inspect myapp
docker inspect --format '{{.State.Status}}' myapp
docker inspect --format '{{json .Mounts}}' myapp
```

### 8.4 Exec

`docker exec` runs a command inside a running container.  
It is useful for debugging but should not become a substitute for reproducible image design or declarative configuration.

**Commands**

```sh
docker exec -it myapp sh
docker exec myapp ps aux
```

### 8.5 Stats, Events, cp, diff

These commands are useful for operational visibility and incident response.

**Commands**

```sh
docker stats
docker events
docker cp myapp:/app/output.txt .
docker diff myapp
```

#### practical value

`docker stats` helps correlate resource pressure with failures.  
`docker events` helps reconstruct lifecycle transitions.  
`docker diff` helps reveal runtime filesystem mutation.  
`docker cp` helps extract artifacts for analysis without rebuilding.

### 8.6 Common Failure Patterns

#### container exits immediately

The main process probably finished, failed, or was defined incorrectly.  
Containers do not stay alive merely because an image exists.

#### permission denied

A non-root process, mount ownership mismatch, missing executable bit, or security restriction may be blocking access.  
This is common after hardening changes.

#### works locally but not in production

Differences in platform architecture, filesystem mounts, runtime users, environment variables, network assumptions, image tags, and daemon configuration are common causes.  
Reproducibility requires more than Dockerfile correctness alone.

#### OOM kill

A container can be killed by memory pressure even when application logs are incomplete or confusing.  
Memory-related failures should be checked through exit codes, host observability, and resource-limit review rather than only application logs.

### 8.7 Cleanup Commands

**Commands**

```sh
docker system df
docker system prune
docker image prune
docker container prune
docker volume prune
docker network prune
```

#### production caveat

Prune commands can remove unused-looking objects that are still operationally valuable, especially build cache or stopped debug containers.  
Cleanup should be deliberate on shared hosts.

---

## 9. Resource Controls and Kernel Isolation

Docker containers rely on Linux kernel primitives for isolation and resource control.  
Docker does not virtualize a full machine; it constrains and namespaces processes on the host.

### 9.1 Namespaces

Namespaces isolate views of system resources.

#### pid namespace

Isolates process IDs so containers see their own process tree.

#### net namespace

Isolates network interfaces, routing tables, and ports.

#### mount namespace

Isolates filesystem mount views.

#### uts namespace

Isolates hostname and domain name.

#### ipc namespace

Isolates shared memory and IPC mechanisms.

#### user namespace

Maps container users to different host-side user identities for stronger privilege separation in supported configurations.

### 9.2 cgroups

Control groups limit and account for resource usage.  
They are central to CPU, memory, and IO isolation.

#### cgroup version nuance

Operational behavior can differ depending on the host’s cgroup generation and configuration.  
Resource tuning assumptions copied between hosts are not always portable without checking the host environment.

### 9.3 CPU Controls

CPU shares, quotas, and cpuset settings constrain or prioritize CPU usage.

**Commands**

```sh
docker run --rm --cpus="1.5" alpine:3.21 sh -c 'nproc'
docker run --rm --cpuset-cpus="0,1" alpine:3.21 sh -c 'echo hello'
```

#### throttling caveat

CPU limits can create throttling-related latency rather than obvious crashes.  
An application may appear healthy but perform poorly because it is being constrained more than expected.

### 9.4 Memory Controls

Memory limits restrict container memory consumption.

**Commands**

```sh
docker run --rm --memory="512m" --memory-swap="512m" alpine:3.21 free -m
```

#### production caveat

Memory exhaustion does not behave like a graceful soft cap.  
It can trigger OOM kills, partial failures, or service instability, so limits must be tested under real load characteristics.

#### swap nuance

Swap-related behavior depends on host configuration and runtime settings.  
Assumptions about memory pressure handling should be validated on the actual target environment.

### 9.5 PIDs Limit

PIDs limits constrain how many processes a container may create.  
This helps control runaway fork behavior and denial-of-service scenarios.

**Commands**

```sh
docker run --rm --pids-limit=100 alpine:3.21 sh -c 'ulimit -u'
```

### 9.6 ulimits and File Descriptors

Containers can still be constrained by process-level limits such as open files and process counts.  
High-connection or file-heavy applications may fail due to these limits rather than due to CPU or memory alone.

---

## 10. Security Model and Hardening

Docker security is layered and partial.  
A container is not a security boundary equivalent to a VM by default, so runtime hardening and host security both matter.

### 10.1 Root in Container vs Root on Host

Root inside a container is powerful even though it is namespaced.  
A container compromise involving root can still be serious, especially if the host, daemon socket, or privileged features are exposed.

#### production caveat

Running as root because “it is only inside the container” is a weak security assumption.  
Least privilege should be the default design baseline.

### 10.2 Non-Root User

Running the application as a non-root user reduces blast radius and accidental filesystem ownership issues.

**Dockerfile**

```dockerfile
FROM python:3.13-slim
WORKDIR /app
COPY . .
RUN useradd -r -u 10001 appuser && chown -R appuser:appuser /app
USER appuser
CMD ["python", "app.py"]
```

### 10.3 Linux Capabilities

Capabilities split root privileges into smaller units.  
Dropping unnecessary capabilities reduces attack surface significantly.

**Commands**

```sh
docker run --rm --cap-drop ALL --cap-add NET_BIND_SERVICE nginx:1.27
```

#### production caveat

Default capability sets may still be broader than the application needs.  
Capability review is one of the most practical container hardening steps.

### 10.4 seccomp

seccomp restricts which syscalls a container process may use.  
Docker commonly applies a default seccomp profile that blocks selected dangerous operations.

#### production caveat

A seccomp failure can look like a mysterious application error rather than an obvious security denial.  
Troubleshooting hardened containers often requires awareness of syscall restrictions.

### 10.5 AppArmor and SELinux

AppArmor and SELinux provide kernel-enforced security policies beyond normal UNIX permissions.  
They can significantly strengthen isolation where supported and correctly configured.

### 10.6 User Namespaces

User namespaces remap container user IDs to unprivileged host-side user IDs.  
This reduces the host impact of a container breakout or misbehavior in supported configurations.

### 10.7 Rootless Mode

Rootless Docker runs the daemon and containers without requiring root privileges in the same way as traditional setups.  
It improves security posture for many development and some production scenarios.

#### rootless caveat

Rootless mode changes networking, filesystem, and privilege behavior in ways that can surprise existing workflows.  
Compatibility and operational assumptions should be tested explicitly rather than assumed.

### 10.8 Read-Only Root Filesystem

A read-only root filesystem prevents many classes of accidental or malicious in-container mutation.  
Applications should write only to explicit writable mount points.

**Commands**

```sh
docker run --rm --read-only --tmpfs /tmp nginx:1.27
```

### 10.9 no-new-privileges

The `no-new-privileges` setting prevents processes from gaining additional privileges through mechanisms such as setuid binaries.  
It is a useful defense-in-depth hardening option.

**Commands**

```sh
docker run --rm --security-opt no-new-privileges:true nginx:1.27
```

### 10.10 Privileged Containers

Privileged mode greatly expands container access to host capabilities and devices.  
It should be treated as a high-risk exception rather than a convenience setting.

### 10.11 Docker Socket Exposure

Mounting the Docker socket into a container effectively gives that container strong control over the host Docker environment.  
This is one of the most dangerous common anti-patterns.

#### production caveat

Treat access to the Docker socket as highly privileged host control.  
It should not be mounted casually into CI jobs, utility containers, or admin tools without a clear trust decision.

---

## 11. Secrets, Configuration, and Supply-Chain Discipline

Containers often fail security review not because of runtime exploits but because of poor secret handling and weak image provenance discipline.

### 11.1 Environment Variables

Environment variables are convenient for configuration and commonly used in Docker workflows.  
They are not ideal for highly sensitive long-lived secrets because they may appear in process metadata, inspect output, logs, or support tooling.

### 11.2 File-Based Secret Patterns

Mounting secrets as files is often safer operationally than embedding them into image layers or broad environment exposure.  
The exact implementation varies by tooling and deployment model.

### 11.3 Image Provenance and Trust

Production images should come from controlled sources with clear ownership and update policy.  
Blindly pulling floating tags from public registries is a supply-chain risk.

### 11.4 SBOM and Image Review Awareness

A mature supply-chain posture includes knowing what is inside the image, not only who built it.  
Software bill of materials practices improve visibility during audits, incident response, and vulnerability review.

### 11.5 Signing and Verification Awareness

Image signing and verification strengthen trust in image origin and integrity.  
They help distinguish “retrievable from a registry” from “approved for production use.”

### 11.6 Digest Pinning

Digest pinning reduces ambiguity in deployments and rebuilds.

**Example**

```dockerfile
FROM python:3.13-slim@sha256:<digest>
```

#### production caveat

Pinning by digest improves determinism, but it also creates an update responsibility.  
Teams must still rebuild intentionally when patched base images become available.

### 11.7 Scanning and Patch Discipline

Base images age.  
Vulnerability posture depends on rebuild cadence, dependency management, and prompt patch adoption rather than only on the initial Dockerfile.

#### production caveat

A clean scan today does not guarantee safety tomorrow.  
Container security is a lifecycle activity, not a one-time build property.

---

## 12. Image Optimization and Production Build Discipline

A production image should be small, intentional, inspectable, and reproducible.  
Optimization is not only about size; it is also about security, performance, and operational clarity.

### 12.1 Minimize Base Images

Choose a base image that matches application needs without carrying unnecessary packages.  
Smaller runtime images usually reduce attack surface and pull times.

### 12.2 Reduce Layer Noise

Combine related package-install and cleanup steps where appropriate, but do not sacrifice readability blindly.  
The goal is controlled layers, not cosmetic one-liners.

### 12.3 Pin Versions Intentionally

Pinning important dependency and base image versions improves reproducibility.  
Blind floating references increase drift and review difficulty.

### 12.4 Do Not Bake Runtime Secrets into Images

If a secret is present in an image, every pulled copy carries it.  
This is a major production and incident-response liability.

### 12.5 Keep Build Tooling Out of Runtime Images

Compilers, package managers, and debug tools often do not belong in the final runtime image.  
Multi-stage builds should be the default pattern for compiled and packaged workloads.

---

## 13. Common Anti-Patterns

### 13.1 Using `latest` as a production contract

`latest` is convenient but ambiguous.  
It weakens reproducibility, rollback confidence, and auditability.

### 13.2 Running everything as root

This enlarges blast radius and often hides permission issues until later hardening efforts.

### 13.3 Writing persistent data to the container layer

This causes fragile deployments and data loss on container replacement.

### 13.4 Using shell form unnecessarily

Shell wrappers complicate signal propagation, quoting, and PID 1 behavior.

### 13.5 Treating Compose as full orchestration

Compose is powerful within its scope, but it does not provide the same operational guarantees as a container orchestrator.

### 13.6 Debugging by editing running containers

Manual in-container changes are not durable, reproducible, or reviewable.  
Image changes should be rebuilt, not hand-patched interactively.

### 13.7 Mounting the Docker socket casually

This is an easy path to effective host control and is often far riskier than people realize.

### 13.8 Using restart policies as architecture

Automatic restarts can hide instability without solving root causes.  
Recovery strategy should include visibility, backoff awareness, and application correctness.

---

## 14. Practical Command Reference

```sh
docker version
docker info
docker images
docker ps
docker ps -a
docker pull nginx:1.27
docker build -t myapp:1.0 .
docker run --name myapp -d -p 8080:8080 myapp:1.0
docker exec -it myapp sh
docker logs -f myapp
docker inspect myapp
docker history myapp:1.0
docker stats
docker events
docker cp myapp:/tmp/file .
docker diff myapp
docker stop myapp
docker rm myapp
docker image rm myapp:1.0
docker network ls
docker network create app-net
docker volume create app-data
docker compose up -d
docker compose logs -f
docker system df
```

## 15. Final Production Framing

Docker is best understood as a process-isolation and packaging system, not a magic deployment boundary.  
The hardest production problems usually come from lifecycle misunderstanding, weak security assumptions, poor state handling, insufficient observability, and unrealistic expectations about what Docker alone does and does not solve.

A strong Docker design therefore emphasizes:

- clear PID 1 and signal behavior
- non-root execution
- minimal and reviewable images
- explicit persistent storage strategy
- controlled network exposure
- reproducible builds
- deliberate secret handling
- supply-chain discipline
- realistic expectations about operational boundaries
