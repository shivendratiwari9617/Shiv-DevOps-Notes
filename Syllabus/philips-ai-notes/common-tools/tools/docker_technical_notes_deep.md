# Docker Technical Notes — Deep Concept Guide, Dockerfile, Build Process, Lifecycle, Compose, and Commands

This file is a **deep technical-notes document** for Docker.

It is designed to explain:

- what Docker is and why it exists
- Docker architecture and core concepts
- the difference between **images** and **containers**
- the **Dockerfile** in detail
- the **image build process**
- the **container lifecycle**
- **Docker Compose**
- storage, networking, volumes, and registries
- common Docker commands with examples

This file is written for **technical understanding** and **interview preparation**.

---

# 1) What Docker is

Docker is a platform for **building, packaging, distributing, and running applications in containers**.

A container is a lightweight, isolated runtime environment that packages:
- application code
- runtime dependencies
- libraries
- system tools
- configuration needed to run the application

Docker became popular because it made application environments more consistent across:
- developer laptops
- CI/CD systems
- test environments
- production systems

A concise technical definition is:

> Docker is a containerization platform that packages applications and their dependencies into portable images and runs them as isolated processes called containers.

---

# 2) Why Docker exists

Before containers were commonly used, teams often faced problems like:

- “it works on my machine”
- dependency mismatches across environments
- conflicting runtime versions on shared hosts
- slow and inconsistent environment setup
- harder application packaging

Docker addresses these by creating a portable unit called an **image**, which can be run consistently as a **container**.

Key benefits:
- environment consistency
- portability
- reproducibility
- fast startup
- better resource efficiency than full virtual machines
- easier CI/CD packaging

---

# 3) Virtual machines vs containers

## 3.1 Virtual machines

A virtual machine includes:
- guest OS
- full userspace
- application
- dependencies

VMs run on a hypervisor and each VM has its own full operating system.

## 3.2 Containers

Containers share the host OS kernel but isolate:
- process space
- filesystem view
- network namespace
- resource limits
- users and permissions depending on configuration

### Important difference
A VM virtualizes hardware.  
A container virtualizes the application execution environment at the OS level.

This is why containers are generally:
- smaller
- faster to start
- more resource-efficient

But they are not identical to VMs in isolation model.

---

# 4) Docker architecture

Docker has a client-server style architecture.

Main components:
- Docker CLI
- Docker daemon
- Docker API
- images
- containers
- registries

---

## 4.1 Docker CLI

The `docker` command-line tool is what users interact with.

Examples:
- `docker build`
- `docker run`
- `docker ps`
- `docker exec`
- `docker compose up`

The CLI sends requests to the Docker daemon.

---

## 4.2 Docker daemon

The Docker daemon is the background service that manages Docker objects.

It is responsible for:
- building images
- creating containers
- starting/stopping containers
- handling networks
- handling volumes
- pulling/pushing images

It listens for Docker API requests from the CLI or other tools.

---

## 4.3 Docker images

An **image** is a read-only template used to create containers.

An image contains:
- filesystem layers
- application code
- metadata
- configuration defaults
- entrypoint/cmd information

Images are immutable artifacts once built.

---

## 4.4 Docker containers

A **container** is a runnable instance of an image.

If an image is the blueprint, the container is the running or stopped instance created from that blueprint.

Important idea:
- image = packaged artifact
- container = runtime instance

---

## 4.5 Docker registries

A registry stores images.

Examples:
- Docker Hub
- Amazon ECR
- Google Artifact Registry
- Azure Container Registry
- GitHub Container Registry

Common workflow:
1. build image locally
2. tag image
3. push image to registry
4. pull image elsewhere
5. run container from that image

---

# 5) Core Docker concepts

## 5.1 Image

An image is a layered, immutable package.

Images are composed of layers.  
Each instruction in a Dockerfile often creates a new layer.

This layering improves:
- caching
- distribution efficiency
- reuse

Example:
If two images share the same base image, they can share those layers.

---

## 5.2 Container

A container is a running or stopped instance of an image with a writable layer added on top.

That writable layer holds runtime changes such as:
- temporary files
- process-generated data
- changes inside the container filesystem

Important:
If the container is deleted, that writable layer is usually lost unless data is stored in volumes or bind mounts.

---

## 5.3 Registry

A registry is a remote image store.

You push images to a registry and pull them from a registry.

Example image reference:

```text
nginx:1.27
```

More complete form:

```text
docker.io/library/nginx:1.27
```

Private registry example:

```text
my-registry.example.com/team/api:1.0.0
```

---

## 5.4 Repository and tag

An image reference typically includes:
- registry
- repository
- tag

Example:

```text
my-registry.example.com/team/api:1.0.0
```

Where:
- `my-registry.example.com` = registry
- `team/api` = repository
- `1.0.0` = tag

Tags are human-readable references.  
For stronger immutability, image digests are even more precise.

---

## 5.5 Layers

Docker images are layered.

Example concept:
- base OS layer
- package install layer
- application dependency layer
- app code layer

This matters because:
- repeated builds can reuse cached layers
- image distribution is more efficient
- changing one layer does not require rebuilding all lower layers from scratch

---

# 6) Dockerfile — the blueprint for images

## 6.1 What a Dockerfile is

A **Dockerfile** is a text file containing instructions used to build a Docker image.

It defines:
- base image
- files to copy
- commands to run during build
- environment variables
- default process to run

A Dockerfile is the core build recipe.

---

## 6.2 Basic Dockerfile example

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8080

CMD ["python", "app.py"]
```

This is one of the most useful examples to understand the build model.

---

## 6.3 Important Dockerfile instructions

### `FROM`
Defines the base image.

Example:

```dockerfile
FROM ubuntu:24.04
```

Or:

```dockerfile
FROM node:20-alpine
```

A Dockerfile usually begins with `FROM`.

---

### `WORKDIR`
Sets the working directory inside the image for subsequent instructions.

Example:

```dockerfile
WORKDIR /app
```

This is similar to doing `cd /app` for following steps.

---

### `COPY`
Copies files from the build context into the image.

Example:

```dockerfile
COPY . .
```

Or:

```dockerfile
COPY requirements.txt .
```

Important:
`COPY` uses files from the **build context**, not arbitrary host filesystem paths outside that context.

---

### `ADD`
Similar to `COPY`, but with extra features such as auto-extracting local tar files and supporting remote URLs in some contexts.

Example:

```dockerfile
ADD archive.tar.gz /app/
```

In most cases, `COPY` is preferred for predictability.

---

### `RUN`
Executes a command during the image build and creates a new image layer.

Example:

```dockerfile
RUN apt-get update && apt-get install -y curl
```

This changes the image itself, not just the runtime command.

---

### `ENV`
Sets environment variables inside the image.

Example:

```dockerfile
ENV APP_ENV=production
```

---

### `ARG`
Defines build-time variables.

Example:

```dockerfile
ARG APP_VERSION=1.0.0
```

Difference:
- `ARG` is mainly for build time
- `ENV` persists in the resulting image and runtime environment unless overridden

---

### `EXPOSE`
Documents that the container listens on a given port.

Example:

```dockerfile
EXPOSE 8080
```

Important:
`EXPOSE` does not publish the port by itself.  
It is metadata/documentation in the image.

---

### `CMD`
Defines the default command to run when a container starts.

Example:

```dockerfile
CMD ["python", "app.py"]
```

There can effectively be one final `CMD`.

---

### `ENTRYPOINT`
Defines the main executable for the container.

Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

This means:
- `ENTRYPOINT` sets the executable
- `CMD` provides default arguments

---

### `USER`
Sets the user used for following instructions or runtime default.

Example:

```dockerfile
USER appuser
```

This is important for security because running as non-root is often preferred.

---

### `LABEL`
Adds metadata to the image.

Example:

```dockerfile
LABEL maintainer="platform@example.com"
LABEL app="billing-api"
```

---

### `VOLUME`
Declares a mount point.

Example:

```dockerfile
VOLUME ["/data"]
```

This is less commonly used in modern production Dockerfiles than explicit runtime volume mounting, but still important conceptually.

---

## 6.4 Dockerfile instruction order matters

Instruction order matters because of **layer caching**.

Example:

```dockerfile
COPY . .
RUN pip install -r requirements.txt
```

This is usually worse than:

```dockerfile
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
```

Why?
Because dependency installation can be cached if application code changes but `requirements.txt` does not.

This is one of the most important Docker build optimization ideas.

---

## 6.5 Dockerfile best practices

Good practices include:
- use small base images when appropriate
- pin versions where possible
- copy dependency files before app code for better caching
- use `.dockerignore`
- avoid unnecessary packages
- minimize layers when sensible
- run as non-root when possible
- avoid secrets in Dockerfiles
- prefer exec-form `CMD` and `ENTRYPOINT`

---

# 7) The Docker build process

## 7.1 What happens during `docker build`

When you run:

```bash
docker build -t myapp:1.0 .
```

Docker does several things:

1. sends the build context to the daemon/build engine
2. reads the Dockerfile
3. processes instructions in order
4. creates layers
5. uses cached layers when possible
6. produces a final image

At a deeper level, the build engine evaluates each Dockerfile step as a transformation that may produce a reusable cache result.

---

## 7.2 Build context

The **build context** is the set of files sent to the build engine.

Example:

```bash
docker build .
```

The `.` means:
- current directory is the build context

Important:
All files in that context may be sent unless excluded by `.dockerignore`.

This is why `.dockerignore` matters.

A common conceptual mistake is thinking Docker can freely read any file on your machine during build.  
It cannot. It only sees what is included in the build context and what the build engine is allowed to mount or access.

---

## 7.3 `.dockerignore`

`.dockerignore` excludes files from the build context.

Example:

```text
.git
node_modules
__pycache__
.env
dist
coverage
```

This improves:
- build speed
- image cleanliness
- security
- cache behavior

If you forget to ignore large changing directories like `node_modules`, builds become slower and cache invalidation becomes more frequent.

---

## 7.4 Layer caching

Docker reuses cached layers if:
- the relevant instruction
- and all dependent earlier context
have not changed

This means caching is not only about the current line; it also depends on earlier layers.

Example Dockerfile:

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### Cache behavior explanation

- If only `app.py` changes:
  - `FROM`, `WORKDIR`, `COPY requirements.txt`, and `RUN pip install` may be reused
  - `COPY . .` and later steps may rebuild

- If `requirements.txt` changes:
  - the dependency install layer is invalidated
  - all later layers must rebuild too

This is why dependency manifests are usually copied early and application code later.

### Cache invalidation principle
Once a layer changes, the layers after it generally need to be rebuilt.

That is one of the most important build-performance concepts.

---

## 7.5 Advanced cache thinking

Good Docker build design often means thinking like this:

- which files change often?
- which steps are expensive?
- which expensive steps can be moved earlier and made cache-stable?
- which directories should never enter the build context?

Examples:
- dependency installation is often expensive
- application source changes more often than dependency lock files
- test artifacts and VCS metadata often do not belong in the image

A build is faster and cleaner when the Dockerfile is structured around cache stability.

---

## 7.6 BuildKit

Modern Docker builds often use **BuildKit**, which is the newer build engine.

BuildKit improves:
- build performance
- caching behavior
- parallel execution in some cases
- output control
- secrets and SSH forwarding
- advanced mounts during build

You do not need to memorize every internal detail, but you should know that BuildKit is the modern build implementation behind many Docker builds.

### Why BuildKit matters
Traditional builds were simpler but less flexible.  
BuildKit makes it easier to build images efficiently and securely.

### Common BuildKit ideas
- better cache management
- inline and remote cache usage
- build secrets
- mount-based optimizations
- more advanced frontends

---

## 7.7 BuildKit cache mounts

One advanced BuildKit feature is cache mounts for package managers or compilers.

Conceptual example:

```dockerfile
RUN --mount=type=cache,target=/root/.cache/pip \
    pip install -r requirements.txt
```

This lets repeated builds reuse package-manager cache data without baking that cache permanently into the final image layer in the same way.

This is especially useful for:
- pip
- npm
- apt metadata flows
- Go build cache
- other dependency tooling

---

## 7.8 BuildKit secrets

BuildKit can provide secrets during build in a way that is safer than hardcoding them into the Dockerfile.

Conceptual example:

```dockerfile
RUN --mount=type=secret,id=mysecret \
    some-command-that-needs-a-secret
```

This is important because secrets should not be embedded directly into image layers.

---

## 7.9 Multi-stage builds

A multi-stage build uses multiple `FROM` instructions.

This is extremely important for modern image optimization.

Example:

```dockerfile
FROM golang:1.23 AS builder
WORKDIR /src
COPY . .
RUN go build -o app .

FROM debian:bookworm-slim
WORKDIR /app
COPY --from=builder /src/app .
CMD ["./app"]
```

### What happens here
- the first stage uses a full Go build environment
- the binary is compiled there
- the second stage starts from a smaller runtime image
- only the built artifact is copied over

### Why multi-stage builds matter
They let you:
- use a heavy build image
- keep only the final binary in the runtime image
- reduce image size
- reduce attack surface
- separate build-time dependencies from runtime dependencies

---

## 7.10 Multi-stage build patterns

### Pattern 1: Builder + runtime
The most common pattern.

Example:
- stage 1 compiles Java, Go, Rust, or Node assets
- stage 2 runs only the final binary or built assets

### Pattern 2: Test stage
You can add a test stage before the final image.

Example idea:

```dockerfile
FROM node:20 AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci

FROM deps AS test
COPY . .
RUN npm test

FROM node:20-alpine AS runtime
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
CMD ["node", "server.js"]
```

### Pattern 3: Separate dev and prod targets
A Dockerfile can define multiple build targets.

Example:

```dockerfile
FROM python:3.12-slim AS base
WORKDIR /app

FROM base AS dev
RUN pip install debugpy

FROM base AS prod
COPY . .
CMD ["python", "app.py"]
```

Build a target:

```bash
docker build --target dev -t myapp:dev .
docker build --target prod -t myapp:prod .
```

---

## 7.11 Build arguments and target selection

Build arguments let you parameterize builds.

Example:

```dockerfile
ARG APP_ENV=prod
ENV APP_ENV=${APP_ENV}
```

Build with argument:

```bash
docker build --build-arg APP_ENV=dev -t myapp:dev .
```

Target selection in multi-stage builds:

```bash
docker build --target builder -t myapp:builder .
```

This is useful for debugging intermediate stages.

---

## 7.12 Build output and tagging strategy

During build, it is common to tag images clearly.

Examples:

```bash
docker build -t myapp:1.0.0 .
docker build -t myapp:latest .
docker build -t registry.example.com/team/myapp:1.0.0 .
```

In professional workflows, teams often use:
- semantic version tags
- commit SHA tags
- branch/environment tags
- immutable digest references for deployment

---

# 8) Image tags and image IDs

## 8.1 Image tag

A tag is a human-friendly label for an image.

Example:

```bash
docker tag myapp:1.0 myregistry.example.com/team/myapp:1.0
```

Tags are references, not the actual immutable identity.

---

## 8.2 Image ID and digest

Images also have IDs and can be referenced by digests.

Digest-style references are stronger for immutability because they point to exact content.

---

# 9) Container lifecycle

## 9.1 Main lifecycle states

A container can be:
- created
- running
- paused
- stopped
- removed

### Created
The container object exists but is not started yet.

### Running
The main process is executing.

### Paused
Processes are suspended.

### Stopped / Exited
The main process ended.

### Removed
The container object no longer exists.

---

## 9.2 Lifecycle flow

Typical flow:

1. image exists
2. container is created from image
3. container starts
4. main process runs
5. process exits or container stops
6. container can be restarted or removed

Important:
A container’s lifecycle is tied to its **main process**.

If the main process exits, the container usually exits.

---

## 9.3 Why containers stop

A container stops when:
- the main process finishes
- the main process crashes
- it receives a stop signal
- it is killed manually

This is why long-running containers usually run a foreground process like:
- web server
- application server
- worker process

---

## 9.4 Restart behavior

Docker can restart containers based on restart policies.

Examples:
- `no`
- `on-failure`
- `always`
- `unless-stopped`

Example:

```bash
docker run -d --restart unless-stopped nginx
```

---

# 10) Docker networking

## 10.1 Default networking idea

Containers run in isolated network namespaces.

Docker provides networks so containers can communicate:
- with the host
- with each other
- with external systems

---

## 10.2 Common network types

Common Docker network types:
- bridge
- host
- none

### Bridge
Default for many local containers.  
Containers get internal IPs on a bridge network.

### Host
Container shares the host network stack.

### None
Container gets no network interface except loopback.

---

## 10.3 Port publishing

To expose a container port on the host:

```bash
docker run -p 8080:80 nginx
```

Meaning:
- host port `8080`
- container port `80`

This is extremely common.

---

## 10.4 User-defined networks

Create a network:

```bash
docker network create app-net
```

Run containers on it:

```bash
docker run -d --name db --network app-net postgres
docker run -d --name api --network app-net myapi:1.0
```

Containers on the same user-defined network can often resolve each other by name.

---

# 11) Docker storage and volumes

## 11.1 Writable container layer

Each container gets a writable layer on top of the image.

But this layer is ephemeral relative to the lifetime of the container object.

Do not treat container writable storage as durable application storage.

---

## 11.2 Volumes

Volumes are Docker-managed persistent storage.

Create a volume:

```bash
docker volume create app-data
```

Use it:

```bash
docker run -d -v app-data:/var/lib/app myapp:1.0
```

Volumes are preferred for persistent data.

---

## 11.3 Bind mounts

Bind mounts map a host path into a container.

Example:

```bash
docker run -v $(pwd):/app python:3.12
```

This is common in development.

Difference:
- volume = Docker-managed storage
- bind mount = host path directly mounted

---

# 12) Docker Compose

## 12.1 What Docker Compose is

Docker Compose is a tool and workflow for defining and running **multi-container applications**.

Instead of starting each container manually with long `docker run` commands, you describe an application stack in a YAML file and start the whole stack together.

Typical use cases:
- web app + database
- API + Redis + worker
- local development environments
- integration testing setups
- small self-contained multi-service applications

The modern command style is:

```bash
docker compose ...
```

rather than the older standalone `docker-compose` binary style.

---

## 12.2 Why Compose matters

Single-container Docker knowledge is not enough for many real applications.

Most real systems involve multiple connected services such as:
- frontend
- backend API
- database
- cache
- message broker
- worker

Compose helps define:
- services
- networks
- volumes
- environment variables
- dependencies
- port mappings
- build instructions

in one file.

---

## 12.3 Compose file structure

A Compose file is usually named something like:

```text
compose.yaml
```

or historically:

```text
docker-compose.yml
```

A typical Compose file contains sections such as:
- `services`
- `volumes`
- `networks`

---

## 12.4 Basic Compose example

```yaml
services:
  web:
    build: .
    ports:
      - "8080:8080"
    environment:
      APP_ENV: development
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

### What this means
- `web` is built from the current directory
- `web` publishes port `8080`
- `db` uses the Postgres image
- `db` stores data in a named volume
- `depends_on` expresses startup ordering intent between services

---

## 12.5 Services in Compose

In Compose, a **service** is a definition for a containerized component of the application.

Examples:
- `web`
- `db`
- `redis`
- `worker`

Each service can define:
- image or build source
- environment
- ports
- volumes
- command
- restart policy
- networks

---

## 12.6 Build vs image in Compose

A service can use:
- `image`
- `build`
- or both in some workflows

Example using `image`:

```yaml
services:
  api:
    image: myregistry.example.com/team/api:1.0.0
```

Example using `build`:

```yaml
services:
  api:
    build: .
```

Example with build context and Dockerfile:

```yaml
services:
  api:
    build:
      context: .
      dockerfile: Dockerfile.prod
```

This is important because Compose can orchestrate both:
- prebuilt images
- local build workflows

---

## 12.7 Networking in Compose

By default, Compose creates an application network so services can communicate by service name.

In the earlier example:
- `web` can often reach `db` using hostname `db`

This is one of the biggest practical advantages of Compose: service discovery becomes simple for local stacks.

---

## 12.8 Volumes in Compose

Compose can define named volumes.

Example:

```yaml
volumes:
  db-data:
```

And use them in services:

```yaml
services:
  db:
    image: postgres:16
    volumes:
      - db-data:/var/lib/postgresql/data
```

This keeps database data persistent across container recreation.

---

## 12.9 Environment variables in Compose

Compose supports inline environment configuration.

Example:

```yaml
environment:
  APP_ENV: development
  LOG_LEVEL: debug
```

It can also use env files in real projects.

---

## 12.10 `depends_on`

`depends_on` expresses startup dependency order at the Compose orchestration level.

Example:

```yaml
depends_on:
  - db
```

Important nuance:
`depends_on` does not necessarily mean the dependency is fully ready to serve traffic unless health-check-aware patterns are used. It mainly controls start order.

---

## 12.11 Common Docker Compose commands

Start services in foreground:

```bash
docker compose up
```

Start in detached mode:

```bash
docker compose up -d
```

Build services:

```bash
docker compose build
```

Start and rebuild:

```bash
docker compose up --build
```

List services:

```bash
docker compose ps
```

Show logs:

```bash
docker compose logs
docker compose logs -f
```

Stop and remove stack resources:

```bash
docker compose down
```

Stop and remove with volumes:

```bash
docker compose down -v
```

Run a one-off command:

```bash
docker compose run web python manage.py migrate
```

Execute inside a running service container:

```bash
docker compose exec web sh
```

Pull images:

```bash
docker compose pull
```

Push built images where configured:

```bash
docker compose push
```

---

## 12.12 Example local development workflow with Compose

Suppose you have:
- a Python API
- a PostgreSQL database

Compose file:

```yaml
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgres://postgres:example@db:5432/app
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: example
      POSTGRES_DB: app
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

Common workflow:

```bash
docker compose up --build
docker compose ps
docker compose logs -f
docker compose exec api sh
docker compose down
```

This is the classic multi-service local development flow.

---

# 13) Docker registries and image distribution

## 13.1 Pulling images

Pull an image:

```bash
docker pull nginx:1.27
```

---

## 13.2 Tagging images

```bash
docker tag myapp:1.0 myregistry.example.com/team/myapp:1.0
```

---

## 13.3 Pushing images

```bash
docker push myregistry.example.com/team/myapp:1.0
```

This is the common publish workflow.

---

# 14) Important Docker commands with examples

## 14.1 `docker version`

Shows client and server version information.

```bash
docker version
```

---

## 14.2 `docker info`

Shows Docker system-wide information.

```bash
docker info
```

Useful for understanding:
- storage driver
- registries
- number of containers/images
- server details

---

## 14.3 `docker build`

Build image from Dockerfile.

```bash
docker build -t myapp:1.0 .
```

Build with specific Dockerfile:

```bash
docker build -f Dockerfile.prod -t myapp:prod .
```

Build with target:

```bash
docker build --target builder -t myapp:builder .
```

Build with build argument:

```bash
docker build --build-arg APP_ENV=dev -t myapp:dev .
```

---

## 14.4 `docker images`

List images.

```bash
docker images
```

---

## 14.5 `docker ps`

List running containers.

```bash
docker ps
```

List all containers including stopped ones:

```bash
docker ps -a
```

---

## 14.6 `docker run`

Create and start a container.

Run interactively:

```bash
docker run -it ubuntu bash
```

Run detached:

```bash
docker run -d nginx
```

Run with a name:

```bash
docker run -d --name web nginx
```

Run with published port:

```bash
docker run -d -p 8080:80 --name web nginx
```

Run with environment variable:

```bash
docker run -e APP_ENV=prod myapp:1.0
```

Run with volume:

```bash
docker run -d -v app-data:/data myapp:1.0
```

Run with explicit network:

```bash
docker run -d --network app-net --name api myapp:1.0
```

---

## 14.7 `docker start`, `stop`, `restart`

Start stopped container:

```bash
docker start web
```

Stop running container:

```bash
docker stop web
```

Restart:

```bash
docker restart web
```

---

## 14.8 `docker exec`

Run a command inside a running container.

```bash
docker exec -it web sh
```

Example:

```bash
docker exec -it web ls /usr/share/nginx/html
```

---

## 14.9 `docker logs`

Show container logs.

```bash
docker logs web
```

Follow logs:

```bash
docker logs -f web
```

---

## 14.10 `docker inspect`

Show detailed low-level information.

```bash
docker inspect web
```

This is useful for:
- environment variables
- mounts
- networking
- image metadata
- container state

---

## 14.11 `docker rm`

Remove a container.

```bash
docker rm web
```

Force remove:

```bash
docker rm -f web
```

---

## 14.12 `docker rmi`

Remove an image.

```bash
docker rmi myapp:1.0
```

---

## 14.13 `docker pull`

Pull image from registry.

```bash
docker pull python:3.12-slim
```

---

## 14.14 `docker push`

Push image to registry.

```bash
docker push myregistry.example.com/team/myapp:1.0
```

---

## 14.15 `docker tag`

Create another tag for an image.

```bash
docker tag myapp:1.0 myapp:latest
```

---

## 14.16 `docker cp`

Copy files between host and container.

Copy host -> container:

```bash
docker cp ./config.json web:/app/config.json
```

Copy container -> host:

```bash
docker cp web:/app/output.txt ./output.txt
```

---

## 14.17 `docker network`

List networks:

```bash
docker network ls
```

Inspect network:

```bash
docker network inspect bridge
```

Create network:

```bash
docker network create app-net
```

---

## 14.18 `docker volume`

List volumes:

```bash
docker volume ls
```

Create volume:

```bash
docker volume create app-data
```

Inspect volume:

```bash
docker volume inspect app-data
```

---

## 14.19 `docker system df`

See Docker disk usage.

```bash
docker system df
```

---

## 14.20 `docker system prune`

Remove unused data.

```bash
docker system prune
```

Be careful with prune commands because they can remove unused containers, networks, and images.

---

# 15) Example complete Docker workflow

Suppose you have this app:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8080

CMD ["python", "app.py"]
```

### Step 1: Build image

```bash
docker build -t my-python-app:1.0 .
```

### Step 2: Run container

```bash
docker run -d -p 8080:8080 --name my-python-app my-python-app:1.0
```

### Step 3: Check running containers

```bash
docker ps
```

### Step 4: View logs

```bash
docker logs my-python-app
```

### Step 5: Enter container if needed

```bash
docker exec -it my-python-app sh
```

### Step 6: Stop container

```bash
docker stop my-python-app
```

### Step 7: Remove container

```bash
docker rm my-python-app
```

---

# 16) Important technical distinctions

## 16.1 Image vs container

Image:
- immutable artifact
- build output
- template for runtime

Container:
- runtime instance of image
- has writable layer
- has lifecycle and state

---

## 16.2 `RUN` vs `CMD`

`RUN`:
- executes at build time
- changes the image

`CMD`:
- defines the default runtime command

Example:

```dockerfile
RUN pip install flask
CMD ["python", "app.py"]
```

---

## 16.3 `CMD` vs `ENTRYPOINT`

`CMD`:
- default command or arguments
- easier to override

`ENTRYPOINT`:
- main executable
- container behaves more like a fixed command

Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Running:

```bash
docker run myapp other.py
```

may replace `CMD` argument but keep the entrypoint.

---

## 16.4 `COPY` vs bind mount

`COPY`:
- adds files into the image during build

Bind mount:
- attaches host files into the container at runtime

These solve different problems.

---

## 16.5 Exposed port vs published port

`EXPOSE 8080`:
- image metadata
- documents intended listening port

`docker run -p 8080:8080`:
- publishes container port to host

This distinction is very important.

---

## 16.6 `docker run` vs `docker compose up`

`docker run` is ideal when:
- you want to start a single container directly
- you are testing one image quickly
- you want explicit one-command runtime control

`docker compose up` is ideal when:
- the application has multiple services
- you want the stack defined in YAML
- you want repeatable local orchestration
- you want built-in service networking and shared configuration

Both are part of Docker usage, but Compose is more natural for multi-service application stacks.

---

# 17) Security and good practices

Important Docker security and hygiene principles:

- use trusted base images
- keep images small
- pin versions when possible
- avoid embedding secrets in images
- run as non-root where possible
- use multi-stage builds
- clean package caches when needed
- scan images for vulnerabilities
- minimize installed tools
- use `.dockerignore`

---

# 18) Common interview-ready technical explanations

## What is Docker?

A strong answer:

> Docker is a containerization platform that packages applications and their dependencies into immutable images and runs them as isolated processes called containers. It improves portability, consistency, and reproducibility across environments.

---

## What is the difference between an image and a container?

A strong answer:

> An image is the immutable packaged artifact, while a container is a running or stopped instance created from that image. The container adds a writable runtime layer and lifecycle state on top of the image.

---

## What is a Dockerfile?

A strong answer:

> A Dockerfile is a declarative build recipe that defines how to create a Docker image, including the base image, files to copy, commands to run during build, environment variables, and the default process to run at container start.

---

## What happens during `docker build`?

A strong answer:

> During `docker build`, Docker sends the build context, reads the Dockerfile instruction by instruction, creates layers, reuses cached layers when possible, and produces a final image artifact that can later be run as a container.

---

## Why are multi-stage builds useful?

A strong answer:

> Multi-stage builds let you separate build-time and runtime environments. You can compile or package an application in one stage and copy only the final output into a smaller runtime image, which reduces image size and attack surface.

---

## What is Docker Compose?

A strong answer:

> Docker Compose is a way to define and run multi-container applications using a YAML file. It lets you describe services, networks, volumes, environment variables, and build settings in one place and then start the whole application stack with commands like `docker compose up`.

---

# 19) Open-source references

These are useful sources for deeper technical reading:

1. Docker overview  
   https://docs.docker.com/get-started/docker-overview/

2. Dockerfile reference  
   https://docs.docker.com/reference/dockerfile/

3. Docker build concepts  
   https://docs.docker.com/build/concepts/overview/

4. Docker build cache  
   https://docs.docker.com/build/cache/

5. Multi-stage builds  
   https://docs.docker.com/build/building/multi-stage/

6. Docker CLI reference  
   https://docs.docker.com/reference/cli/docker/

7. docker run reference  
   https://docs.docker.com/reference/cli/docker/container/run/

8. docker build reference  
   https://docs.docker.com/reference/cli/docker/buildx/build/

9. Docker Compose file reference  
   https://docs.docker.com/reference/compose-file/

10. Docker Compose CLI reference  
   https://docs.docker.com/reference/cli/docker/compose/

11. Docker volumes  
   https://docs.docker.com/engine/storage/volumes/

12. Bind mounts  
   https://docs.docker.com/engine/storage/bind-mounts/

13. Docker networking  
   https://docs.docker.com/engine/network/

14. Docker image tags and digests  
   https://docs.docker.com/dhi/core-concepts/digests/

15. Docker registry concepts  
   https://docs.docker.com/docker-hub/repos/

16. Docker best practices  
   https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

---

# 20) Final revision summary

The most important way to understand Docker is this:

- Docker packages applications into **images**
- images are built from a **Dockerfile**
- images are layered and immutable
- containers are runtime instances of images
- the build process uses **context**, **instructions**, and **cache**
- BuildKit improves modern build performance and flexibility
- multi-stage builds separate build-time and runtime concerns
- the container lifecycle follows the main process lifecycle
- volumes persist data beyond a container’s writable layer
- networks connect containers to each other and the host
- Compose defines and runs **multi-container applications**
- registries distribute images across environments

If you understand:
- images
- containers
- layers
- Dockerfile instructions
- build context
- caching
- BuildKit
- multi-stage builds
- container lifecycle
- Compose
- storage
- networking
- commands

then you understand the technical core of Docker very well.
