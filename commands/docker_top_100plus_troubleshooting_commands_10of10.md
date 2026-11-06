# Docker Top 100+ Production Troubleshooting Commands
**10/10 senior-level runbook pack for Docker Platform Engineers, DevOps Engineers, and SREs in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic Docker cheat sheet.

Each command includes:

- the command itself
- a concise technical explanation
- a **risk level**
- a **use when** line so responders know where it fits in incident flow

## Command Risk Levels

- **Low risk** — read-only or generally safe diagnostic action
- **Medium risk** — controlled execution that changes runtime context or performs limited intervention
- **High risk** — can mutate live runtime state, remove artifacts, or increase operational blast radius
- **Emergency-only** — use only in tightly controlled recovery situations with strong confirmation

## Fast “First 5 Commands” by Incident Type

### Container crash loop or repeated exits
1. `docker ps -a`
2. `docker inspect -f '{{.State.ExitCode}}' <container>`
3. `docker inspect -f '{{.RestartCount}}' <container>`
4. `docker logs --tail 200 <container>`
5. `docker events --since 30m --filter 'container=<container>'`

### Unhealthy container
1. `docker inspect -f '{{json .State.Health}}' <container>`
2. `docker inspect -f '{{.Config.Healthcheck.Test}}' <container>`
3. `docker logs --tail 100 <container>`
4. `docker exec <container> curl -I http://localhost:8080/health`
5. `docker exec <container> ss -lntp`

### Image pull, digest, or registry issue
1. `docker image ls --digests`
2. `docker manifest inspect <image>:<tag>`
3. `docker login <registry>`
4. `docker pull <image>:<tag>`
5. `docker image inspect <image>`

### Port mapping or connectivity failure
1. `docker port <container>`
2. `docker inspect -f '{{json .NetworkSettings.Ports}}' <container>`
3. `ss -lntp | grep ':80 '`
4. `docker network inspect <network>`
5. `iptables -t nat -L -n | grep DOCKER`

### DNS or service discovery failure
1. `docker exec <container> cat /etc/resolv.conf`
2. `docker exec <container> getent hosts myservice`
3. `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container>`
4. `docker network ls`
5. `docker network inspect <network>`

### Volume, bind mount, or storage-path issue
1. `docker inspect -f '{{json .Mounts}}' <container>`
2. `docker volume ls`
3. `docker volume inspect <volume>`
4. `docker exec <container> ls -lah /data`
5. `docker run --rm -v <volume>:/data alpine ls -lah /data`

### Host disk pressure from Docker
1. `docker system df`
2. `docker system df -v`
3. `du -sh /var/lib/docker`
4. `find /var/lib/docker/containers -name '*-json.log' -size +100M`
5. `du -sh /var/lib/docker/volumes/*`

### Daemon unresponsive or unhealthy
1. `docker context show`
2. `systemctl status docker`
3. `journalctl -u docker -n 100 --no-pager`
4. `ls -l /var/run/docker.sock`
5. `docker info`

### CPU or memory pressure in one container
1. `docker stats --no-stream`
2. `docker inspect -f '{{.HostConfig.Memory}}' <container>`
3. `docker inspect -f '{{.HostConfig.NanoCpus}}' <container>`
4. `top -H -p $(docker inspect -f '{{.State.Pid}}' <container>)`
5. `pidstat -p $(docker inspect -f '{{.State.Pid}}' <container>) 1 5`

## Recommended Troubleshooting Flow

1. confirm Docker context, host, and daemon health first
2. inspect container state, exit code, restart count, and logs before restarting
3. verify image digest and runtime config, not just image tag
4. separate image, runtime, network, storage, and host resource failure planes
5. preserve evidence before cleanup or destructive maintenance
6. treat daemon restarts, prunes, and log truncation as controlled operations

---

# Engine, Version, and Runtime Context


## 1. `docker version`


```bash

docker version

```

**Risk:** Low risk

**Technical explanation:** Prints client and server version details so you can detect CLI/daemon drift before deeper troubleshooting.

**Use when:** Use when Docker behavior differs across hosts, bastions, or CI runners.


## 2. `docker info`


```bash

docker info

```

**Risk:** Low risk

**Technical explanation:** Shows daemon configuration, storage driver, cgroup driver, runtimes, and resource totals, which is essential for baseline engine health.

**Use when:** Use when you need a first-pass daemon and host runtime snapshot.


## 3. `docker system info`


```bash

docker system info

```

**Risk:** Low risk

**Technical explanation:** Provides the same daemon-wide environment summary used to confirm runtime configuration and host-level Docker state.

**Use when:** Use when validating engine state in scripts or alternate command habits.


## 4. `docker context ls`


```bash

docker context ls

```

**Risk:** Low risk

**Technical explanation:** Lists available Docker contexts so you can verify whether commands are targeting the intended host or endpoint.

**Use when:** Use when wrong-environment execution is a real risk.


## 5. `docker context show`


```bash

docker context show

```

**Risk:** Low risk

**Technical explanation:** Prints the active Docker context, which is one of the fastest checks against wrong-host troubleshooting.

**Use when:** Use before any risky production Docker command.


## 6. `docker context inspect`


```bash

docker context inspect

```

**Risk:** Low risk

**Technical explanation:** Shows detailed context endpoint metadata so you can validate sockets, TLS settings, and target configuration.

**Use when:** Use when remote context connectivity or TLS settings may be wrong.


## 7. `docker system events --since 10m`


```bash

docker system events --since 10m

```

**Risk:** Low risk

**Technical explanation:** Streams recent daemon events so you can correlate restarts, pulls, kills, and network changes with an incident window.

**Use when:** Use when timeline reconstruction matters.


## 8. `docker system df`


```bash

docker system df

```

**Risk:** Low risk

**Technical explanation:** Shows Docker disk usage by images, containers, volumes, and build cache, which helps explain disk-pressure incidents.

**Use when:** Use when host storage pressure may be Docker-related.


## 9. `docker system df -v`


```bash

docker system df -v

```

**Risk:** Low risk

**Technical explanation:** Adds per-object disk-usage detail so you can identify exactly which images, volumes, or containers are consuming space.

**Use when:** Use when you need object-level storage attribution before cleanup.


## 10. `docker system dial-stdio`


```bash

docker system dial-stdio

```

**Risk:** Medium risk

**Technical explanation:** Verifies lower-level Docker context communication paths in advanced remote-context troubleshooting.

**Use when:** Use when context transport itself may be broken and standard CLI access is unreliable.


## 11. `docker plugin ls`


```bash

docker plugin ls

```

**Risk:** Low risk

**Technical explanation:** Lists installed plugins so you can confirm whether networking, logging, or volume plugins required by workloads are present.

**Use when:** Use when plugin-backed functionality fails unexpectedly.


## 12. `docker plugin inspect <plugin>`


```bash

docker plugin inspect <plugin>

```

**Risk:** Low risk

**Technical explanation:** Displays plugin configuration and capabilities so you can investigate plugin-related failures precisely.

**Use when:** Use when one plugin appears responsible for storage, network, or log path issues.


# Daemon Health and Service State


## 13. `systemctl status docker`


```bash

systemctl status docker

```

**Risk:** Low risk

**Technical explanation:** Shows whether the Docker daemon service is running and surfaces recent service-level failure messages.

**Use when:** Use when CLI commands fail or the daemon appears unhealthy.


## 14. `systemctl is-active docker`


```bash

systemctl is-active docker

```

**Risk:** Low risk

**Technical explanation:** Returns a concise active/inactive state so you can script or quickly confirm daemon availability.

**Use when:** Use when you need a fast health gate for the daemon service.


## 15. `systemctl restart docker`


```bash

systemctl restart docker

```

**Risk:** High risk

**Technical explanation:** Restarts the Docker daemon to recover from daemon-side faults, but it can disrupt running workloads depending on host setup.

**Use when:** Use only after collecting evidence and when daemon-level recovery is necessary.


## 16. `journalctl -u docker -n 100 --no-pager`


```bash

journalctl -u docker -n 100 --no-pager

```

**Risk:** Low risk

**Technical explanation:** Prints recent daemon logs so you can inspect startup errors, runtime failures, and plugin issues.

**Use when:** Use when Docker operations fail and daemon logs are the likely source of truth.


## 17. `journalctl -u docker --since '30 min ago'`


```bash

journalctl -u docker --since '30 min ago'

```

**Risk:** Low risk

**Technical explanation:** Filters daemon logs to the recent incident window, helping correlate engine behavior with production impact.

**Use when:** Use when the incident timing is known.


## 18. `ps -ef | grep dockerd | grep -v grep`


```bash

ps -ef | grep dockerd | grep -v grep

```

**Risk:** Low risk

**Technical explanation:** Confirms whether the dockerd process exists and how it was started.

**Use when:** Use when service state and actual process state may differ.


## 19. `ss -lntp | grep dockerd`


```bash

ss -lntp | grep dockerd

```

**Risk:** Low risk

**Technical explanation:** Checks whether Docker-related sockets or listeners are bound as expected.

**Use when:** Use when daemon socket exposure or listener presence is in doubt.


## 20. `ls -l /var/run/docker.sock`


```bash

ls -l /var/run/docker.sock

```

**Risk:** Low risk

**Technical explanation:** Inspects the daemon socket path and permissions when CLI access or automation is failing.

**Use when:** Use when users or automation receive Docker socket permission errors.


## 21. `stat /var/run/docker.sock`


```bash

stat /var/run/docker.sock

```

**Risk:** Low risk

**Technical explanation:** Shows socket ownership and mode so permission problems can be traced precisely.

**Use when:** Use when socket ACL details matter.


## 22. `docker info | grep -i 'Storage Driver'`


```bash

docker info | grep -i 'Storage Driver'

```

**Risk:** Low risk

**Technical explanation:** Extracts the active storage driver to help explain image-layer, inode, or filesystem behavior.

**Use when:** Use when overlay, devicemapper, or storage semantics may explain failures.


## 23. `docker info | grep -i 'Cgroup Driver'`


```bash

docker info | grep -i 'Cgroup Driver'

```

**Risk:** Low risk

**Technical explanation:** Shows the cgroup driver so you can diagnose runtime mismatches with the host init system.

**Use when:** Use when cgroup-related behavior differs across hosts.


## 24. `docker info | grep -i 'Logging Driver'`


```bash

docker info | grep -i 'Logging Driver'

```

**Risk:** Low risk

**Technical explanation:** Displays the default logging driver, which can explain log visibility or disk-usage differences across hosts.

**Use when:** Use when logs exist differently than expected.


# Images, Layers, and Registry Troubleshooting


## 25. `docker image ls`


```bash

docker image ls

```

**Risk:** Low risk

**Technical explanation:** Lists local images so you can confirm whether the expected image tag exists on the host.

**Use when:** Use when a workload may be starting from the wrong or missing image.


## 26. `docker image ls --digests`


```bash

docker image ls --digests

```

**Risk:** Low risk

**Technical explanation:** Shows image digests along with tags so you can verify exact image identity beyond mutable tags.

**Use when:** Use when release traceability requires digest-level confirmation.


## 27. `docker images --format '{{.Repository}}:{{.Tag}} {{.ID}} {{.Size}}'`


```bash

docker images --format '{{.Repository}}:{{.Tag}} {{.ID}} {{.Size}}'

```

**Risk:** Low risk

**Technical explanation:** Renders a concise custom image inventory that is useful during incident triage.

**Use when:** Use when a compact local image inventory is easier to reason about.


## 28. `docker image inspect <image>`


```bash

docker image inspect <image>

```

**Risk:** Low risk

**Technical explanation:** Displays image metadata including config, environment, entrypoint, and layers so you can validate what will actually run.

**Use when:** Use when the image itself may encode the problem.


## 29. `docker history <image>`


```bash

docker history <image>

```

**Risk:** Low risk

**Technical explanation:** Shows layer history so you can investigate image bloat, suspicious changes, or build-step regressions.

**Use when:** Use when the image build process may have introduced risk or size explosion.


## 30. `docker pull <image>:<tag>`


```bash

docker pull <image>:<tag>

```

**Risk:** Medium risk

**Technical explanation:** Pulls an image from a registry so you can verify registry reachability and image availability.

**Use when:** Use when the host may not actually have the needed image locally.


## 31. `docker pull --platform linux/amd64 <image>:<tag>`


```bash

docker pull --platform linux/amd64 <image>:<tag>

```

**Risk:** Medium risk

**Technical explanation:** Forces a specific platform pull, which helps diagnose architecture mismatches in multi-arch environments.

**Use when:** Use when ARM/AMD64 drift may explain startup failures.


## 32. `docker push <image>:<tag>`


```bash

docker push <image>:<tag>

```

**Risk:** Medium risk

**Technical explanation:** Pushes an image to a registry so you can validate auth, repository permissions, and tag publishing paths.

**Use when:** Use when image publication or promotion paths may be broken.


## 33. `docker tag <src_image> <dst_image>`


```bash

docker tag <src_image> <dst_image>

```

**Risk:** Low risk

**Technical explanation:** Creates a new local tag reference, which is useful when reproducing release-tag issues safely.

**Use when:** Use when you need local tag alignment without changing image content.


## 34. `docker login <registry>`


```bash

docker login <registry>

```

**Risk:** Medium risk

**Technical explanation:** Authenticates to a registry so you can test whether registry access failures are credential-related.

**Use when:** Use when pull or push failures may stem from auth.


## 35. `docker logout <registry>`


```bash

docker logout <registry>

```

**Risk:** Medium risk

**Technical explanation:** Removes cached registry credentials so you can clear stale auth context during troubleshooting.

**Use when:** Use when bad cached credentials may be masking the real auth state.


## 36. `docker manifest inspect <image>:<tag>`


```bash

docker manifest inspect <image>:<tag>

```

**Risk:** Low risk

**Technical explanation:** Shows multi-platform manifest data so you can verify which architectures and digests a tag resolves to.

**Use when:** Use when mutable tags and multi-arch resolution may hide the deployed artifact.


# Container Inventory and Lifecycle


## 37. `docker ps`


```bash

docker ps

```

**Risk:** Low risk

**Technical explanation:** Lists running containers so you can see which workloads are currently active on the host.

**Use when:** Use when you need the current runtime inventory first.


## 38. `docker ps -a`


```bash

docker ps -a

```

**Risk:** Low risk

**Technical explanation:** Lists all containers including exited ones, which is critical when troubleshooting crash loops or failed starts.

**Use when:** Use when stopped containers may hold the failure evidence.


## 39. `docker ps --no-trunc`


```bash

docker ps --no-trunc

```

**Risk:** Low risk

**Technical explanation:** Shows full container IDs and command lines so you can inspect exact process invocation details.

**Use when:** Use when short IDs or truncated commands hide useful detail.


## 40. `docker ps --filter 'status=exited'`


```bash

docker ps --filter 'status=exited'

```

**Risk:** Low risk

**Technical explanation:** Filters for exited containers to focus on failed or completed workloads.

**Use when:** Use when investigating recent failures rather than healthy workloads.


## 41. `docker ps --filter 'name=myapp'`


```bash

docker ps --filter 'name=myapp'

```

**Risk:** Low risk

**Technical explanation:** Filters containers by name so you can quickly isolate one workload family.

**Use when:** Use when one service family is under incident.


## 42. `docker inspect <container>`


```bash

docker inspect <container>

```

**Risk:** Low risk

**Technical explanation:** Shows full runtime metadata for a container including mounts, env vars, state, networks, and restart policy.

**Use when:** Use when you need the authoritative runtime configuration.


## 43. `docker container inspect <container>`


```bash

docker container inspect <container>

```

**Risk:** Low risk

**Technical explanation:** Provides the same detailed runtime inspection focused explicitly on container objects.

**Use when:** Use when using fully scoped Docker object syntax in scripts or runbooks.


## 44. `docker start <container>`


```bash

docker start <container>

```

**Risk:** Medium risk

**Technical explanation:** Starts a stopped container so you can test whether it can recover without recreation.

**Use when:** Use when a stopped workload may recover with minimal intervention.


## 45. `docker stop <container>`


```bash

docker stop <container>

```

**Risk:** Medium risk

**Technical explanation:** Stops a running container gracefully so you can control restart timing or isolate a workload.

**Use when:** Use when controlled shutdown is safer than letting instability continue.


## 46. `docker restart <container>`


```bash

docker restart <container>

```

**Risk:** Medium risk

**Technical explanation:** Restarts a container so you can test whether the failure is transient or tied to startup logic.

**Use when:** Use when evidence is already collected and a restart is warranted.


## 47. `docker rm <container>`


```bash

docker rm <container>

```

**Risk:** High risk

**Technical explanation:** Removes a stopped container, which can discard local runtime state and should follow evidence collection.

**Use when:** Use only when the container is no longer needed and recreation is planned.


## 48. `docker rename <old_name> <new_name>`


```bash

docker rename <old_name> <new_name>

```

**Risk:** Medium risk

**Technical explanation:** Renames a container so you can preserve or free a service name during controlled recovery.

**Use when:** Use when name-based replacement or side-by-side recovery is needed.


# Container State, Exit Codes, and Process Inspection


## 49. `docker inspect -f '{{.State.Status}}' <container>`


```bash

docker inspect -f '{{.State.Status}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Prints the container status directly so you can script or quickly confirm whether it is running, exited, or restarting.

**Use when:** Use when you need container state without full inspect noise.


## 50. `docker inspect -f '{{.State.ExitCode}}' <container>`


```bash

docker inspect -f '{{.State.ExitCode}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Returns the last process exit code, which is one of the fastest indicators of application failure mode.

**Use when:** Use when the app is exiting and you need immediate failure signal.


## 51. `docker inspect -f '{{.State.Error}}' <container>`


```bash

docker inspect -f '{{.State.Error}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Shows daemon-recorded start or runtime errors attached to the container state.

**Use when:** Use when a container never really starts or dies during initialization.


## 52. `docker inspect -f '{{.RestartCount}}' <container>`


```bash

docker inspect -f '{{.RestartCount}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Prints how many times the container has restarted, which helps quantify restart-loop severity.

**Use when:** Use when instability may be a crash loop rather than a single failure.


## 53. `docker top <container>`


```bash

docker top <container>

```

**Risk:** Low risk

**Technical explanation:** Shows processes running inside the container so you can confirm whether the expected application process exists.

**Use when:** Use when runtime state is unclear even though the container reports running.


## 54. `docker exec <container> ps -ef`


```bash

docker exec <container> ps -ef

```

**Risk:** Medium risk

**Technical explanation:** Runs ps inside the container to inspect the live process tree from the workload namespace.

**Use when:** Use when container-internal process relationships matter.


## 55. `docker exec <container> cat /proc/1/cmdline`


```bash

docker exec <container> cat /proc/1/cmdline

```

**Risk:** Low risk

**Technical explanation:** Shows the exact PID 1 command line so you can verify entrypoint and argument behavior.

**Use when:** Use when startup command composition may be wrong.


## 56. `docker exec <container> cat /proc/1/environ | tr '\0' '\n'`


```bash

docker exec <container> cat /proc/1/environ | tr '\0' '\n'

```

**Risk:** Medium risk

**Technical explanation:** Prints PID 1 environment variables so you can confirm the effective runtime configuration.

**Use when:** Use when env injection, secrets, or config expansion may be wrong.


## 57. `docker wait <container>`


```bash

docker wait <container>

```

**Risk:** Low risk

**Technical explanation:** Waits for a container to exit and returns its exit code, which is useful in controlled reproduction workflows.

**Use when:** Use when you are deliberately observing one run-to-exit cycle.


## 58. `docker pause <container>`


```bash

docker pause <container>

```

**Risk:** High risk

**Technical explanation:** Freezes processes in a container so you can isolate impact without fully stopping it, but it affects live workload execution.

**Use when:** Use only when temporary execution suspension is safer than letting the workload continue.


## 59. `docker unpause <container>`


```bash

docker unpause <container>

```

**Risk:** Medium risk

**Technical explanation:** Resumes a previously paused container after temporary isolation or inspection.

**Use when:** Use when a paused workload should return to service.


## 60. `docker update --restart unless-stopped <container>`


```bash

docker update --restart unless-stopped <container>

```

**Risk:** High risk

**Technical explanation:** Changes restart policy on a live container, which can alter recovery behavior and operational expectations.

**Use when:** Use when crash-loop containment or persistence behavior must be changed deliberately.


# Logs, Streams, and Real-Time Observation


## 61. `docker logs <container>`


```bash

docker logs <container>

```

**Risk:** Low risk

**Technical explanation:** Prints the stdout/stderr logs collected for a container so you can inspect application failures quickly.

**Use when:** Use as one of the first checks after state and exit-code inspection.


## 62. `docker logs --tail 100 <container>`


```bash

docker logs --tail 100 <container>

```

**Risk:** Low risk

**Technical explanation:** Shows only the latest log lines, which is useful when logs are large and the recent failure matters most.

**Use when:** Use when you need the freshest application evidence.


## 63. `docker logs -f <container>`


```bash

docker logs -f <container>

```

**Risk:** Low risk

**Technical explanation:** Follows container logs in real time so you can watch startup or failure behavior live.

**Use when:** Use when reproducing or observing a live restart cycle.


## 64. `docker logs --since 10m <container>`


```bash

docker logs --since 10m <container>

```

**Risk:** Low risk

**Technical explanation:** Filters logs to the recent time window, helping correlate output with a known incident period.

**Use when:** Use when the incident began recently.


## 65. `docker logs --timestamps <container>`


```bash

docker logs --timestamps <container>

```

**Risk:** Low risk

**Technical explanation:** Adds timestamps to log lines so you can align container output with host or platform events.

**Use when:** Use when timeline correlation across systems matters.


## 66. `docker service logs <service>`


```bash

docker service logs <service>

```

**Risk:** Low risk

**Technical explanation:** Shows logs for a Swarm service so you can inspect aggregated task output at service level.

**Use when:** Use when the workload runs under Swarm service management.


## 67. `docker service logs -f <service>`


```bash

docker service logs -f <service>

```

**Risk:** Low risk

**Technical explanation:** Follows service logs live, which is useful when multiple tasks are restarting or rolling.

**Use when:** Use when Swarm service behavior must be watched in real time.


## 68. `docker events --filter 'container=<container>'`


```bash

docker events --filter 'container=<container>'

```

**Risk:** Low risk

**Technical explanation:** Streams daemon events only for one container so you can watch starts, stops, kills, and health transitions.

**Use when:** Use when lifecycle transitions are central to the issue.


## 69. `docker inspect -f '{{.LogPath}}' <container>`


```bash

docker inspect -f '{{.LogPath}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Prints the underlying log file path for a container when direct file inspection is needed.

**Use when:** Use when host-level log inspection is required.


## 70. `tail -f $(docker inspect -f '{{.LogPath}}' <container>)`


```bash

tail -f $(docker inspect -f '{{.LogPath}}' <container>)

```

**Risk:** Medium risk

**Technical explanation:** Follows the raw log file at the host level, which can help when CLI log access behaves unexpectedly.

**Use when:** Use when Docker CLI logging is insufficient or lagging.


## 71. `docker logs --details <container>`


```bash

docker logs --details <container>

```

**Risk:** Low risk

**Technical explanation:** Includes extra log metadata when supported by the logging driver, which helps with advanced tracing.

**Use when:** Use when structured log metadata may explain behavior.


## 72. `docker logs --until 5m <container>`


```bash

docker logs --until 5m <container>

```

**Risk:** Low risk

**Technical explanation:** Shows logs only up to a cutoff time, which is useful when comparing before-and-after behavior around a restart.

**Use when:** Use when isolating pre-restart evidence.


# Exec, Shell, and In-Container Debugging


## 73. `docker exec -it <container> /bin/sh`


```bash

docker exec -it <container> /bin/sh

```

**Risk:** Medium risk

**Technical explanation:** Opens an interactive shell inside the container so you can inspect filesystem, env, and process state directly.

**Use when:** Use when non-interactive inspection is insufficient.


## 74. `docker exec -it <container> /bin/bash`


```bash

docker exec -it <container> /bin/bash

```

**Risk:** Medium risk

**Technical explanation:** Starts an interactive Bash shell when the image includes it, which can simplify deep debugging.

**Use when:** Use when the image provides Bash and a richer shell helps.


## 75. `docker exec <container> env`


```bash

docker exec <container> env

```

**Risk:** Low risk

**Technical explanation:** Prints runtime environment variables so you can verify injected config quickly.

**Use when:** Use when environment-driven behavior looks wrong.


## 76. `docker exec <container> ls -lah /app`


```bash

docker exec <container> ls -lah /app

```

**Risk:** Low risk

**Technical explanation:** Inspects application files inside the container when builds or mounts may have produced wrong content.

**Use when:** Use when the container filesystem may not contain what the image or mount should provide.


## 77. `docker exec <container> netstat -tulpn`


```bash

docker exec <container> netstat -tulpn

```

**Risk:** Medium risk

**Technical explanation:** Shows listening ports inside the container if the tooling is available, helping confirm bind behavior.

**Use when:** Use when app startup appears healthy but socket binding is uncertain.


## 78. `docker exec <container> ss -lntp`


```bash

docker exec <container> ss -lntp

```

**Risk:** Low risk

**Technical explanation:** Checks socket listeners inside the container with a modern networking tool.

**Use when:** Use when verifying whether the process is bound inside the namespace.


## 79. `docker exec <container> curl -I http://localhost:8080/health`


```bash

docker exec <container> curl -I http://localhost:8080/health

```

**Risk:** Low risk

**Technical explanation:** Tests a local health endpoint from inside the container namespace.

**Use when:** Use when health checks fail but local app readiness is unclear.


## 80. `docker exec <container> sh -c 'id && whoami'`


```bash

docker exec <container> sh -c 'id && whoami'

```

**Risk:** Medium risk

**Technical explanation:** Confirms the effective user and identity inside the running container.

**Use when:** Use when user, UID, or file-permission assumptions may be wrong.


## 81. `docker exec <container> df -h`


```bash

docker exec <container> df -h

```

**Risk:** Low risk

**Technical explanation:** Shows filesystem usage inside the container so you can detect overlay or mount-space problems.

**Use when:** Use when write failures or space errors occur inside the workload.


## 82. `docker exec <container> free -m`


```bash

docker exec <container> free -m

```

**Risk:** Low risk

**Technical explanation:** Checks memory visibility inside the container for runtime-capacity investigation.

**Use when:** Use when the process may be under memory pressure.


## 83. `docker cp <container>:/path/to/file ./retrieved_file`


```bash

docker cp <container>:/path/to/file ./retrieved_file

```

**Risk:** Medium risk

**Technical explanation:** Copies a file from the container to the host for offline inspection or evidence preservation.

**Use when:** Use when config, logs, or artifacts should be preserved before mutation.


## 84. `docker cp ./local_file <container>:/tmp/local_file`


```bash

docker cp ./local_file <container>:/tmp/local_file

```

**Risk:** High risk

**Technical explanation:** Copies a file into the container for controlled debugging or config validation, but it mutates container state.

**Use when:** Use only when temporary in-container testing is justified.


# Networking, DNS, and Port Mapping


## 85. `docker network ls`


```bash

docker network ls

```

**Risk:** Low risk

**Technical explanation:** Lists Docker networks so you can confirm whether the expected bridge, overlay, or custom network exists.

**Use when:** Use when containers cannot reach each other or service networks seem missing.


## 86. `docker network inspect <network>`


```bash

docker network inspect <network>

```

**Risk:** Low risk

**Technical explanation:** Displays network configuration, connected containers, subnets, and gateway details.

**Use when:** Use when subnet, membership, or gateway configuration may be wrong.


## 87. `docker port <container>`


```bash

docker port <container>

```

**Risk:** Low risk

**Technical explanation:** Shows published port mappings so you can validate host-to-container exposure quickly.

**Use when:** Use when traffic cannot reach the container from outside.


## 88. `docker inspect -f '{{json .NetworkSettings.Ports}}' <container>`


```bash

docker inspect -f '{{json .NetworkSettings.Ports}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Prints raw port-binding data so you can inspect exact published-port mappings.

**Use when:** Use when you need structured port exposure details.


## 89. `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container>`


```bash

docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Returns the container IP address so you can test direct container-level reachability.

**Use when:** Use when debugging intra-host container connectivity.


## 90. `docker exec <container> cat /etc/resolv.conf`


```bash

docker exec <container> cat /etc/resolv.conf

```

**Risk:** Low risk

**Technical explanation:** Shows DNS resolver configuration inside the container so you can investigate name-resolution issues.

**Use when:** Use when service discovery or external DNS lookup fails.


## 91. `docker exec <container> getent hosts myservice`


```bash

docker exec <container> getent hosts myservice

```

**Risk:** Low risk

**Technical explanation:** Tests DNS resolution from inside the container using glibc name-service lookup.

**Use when:** Use when containerized name resolution is the suspected failure plane.


## 92. `docker exec <container> ping -c 3 db`


```bash

docker exec <container> ping -c 3 db

```

**Risk:** Medium risk

**Technical explanation:** Tests basic connectivity to another service name inside the container network, though ICMP may not always be allowed.

**Use when:** Use when simple name-plus-connectivity validation is needed.


## 93. `docker network connect <network> <container>`


```bash

docker network connect <network> <container>

```

**Risk:** High risk

**Technical explanation:** Attaches a running container to an additional network for controlled connectivity recovery, but changes live network topology.

**Use when:** Use only when network attachment is the intended remediation.


## 94. `docker network disconnect <network> <container>`


```bash

docker network disconnect <network> <container>

```

**Risk:** High risk

**Technical explanation:** Detaches a container from a network when isolating cross-network issues, but it can break traffic immediately.

**Use when:** Use only when deliberate network isolation is required.


## 95. `ss -lntp | grep ':80 '`


```bash

ss -lntp | grep ':80 '

```

**Risk:** Low risk

**Technical explanation:** Checks whether the host is listening on the expected published port and which process owns it.

**Use when:** Use when published-port behavior is unclear at the host layer.


## 96. `iptables -t nat -L -n | grep DOCKER`


```bash

iptables -t nat -L -n | grep DOCKER

```

**Risk:** Medium risk

**Technical explanation:** Inspects Docker-related NAT rules when published-port traffic is not flowing as expected.

**Use when:** Use when port mapping looks right in Docker but packets still do not traverse correctly.


# Storage, Mounts, and Volumes


## 97. `docker volume ls`


```bash

docker volume ls

```

**Risk:** Low risk

**Technical explanation:** Lists Docker-managed volumes so you can confirm whether expected persistent storage objects exist.

**Use when:** Use when the app depends on named volumes that may be missing.


## 98. `docker volume inspect <volume>`


```bash

docker volume inspect <volume>

```

**Risk:** Low risk

**Technical explanation:** Shows volume mountpoint and metadata so you can locate or validate persistent data paths.

**Use when:** Use when persistence or host-path mapping is in question.


## 99. `docker inspect -f '{{json .Mounts}}' <container>`


```bash

docker inspect -f '{{json .Mounts}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Prints the container's mount definitions so you can verify bind mounts and volumes precisely.

**Use when:** Use when the container may not have the mounts you expect.


## 100. `docker exec <container> mount`


```bash

docker exec <container> mount

```

**Risk:** Low risk

**Technical explanation:** Shows mounted filesystems inside the container so you can detect missing or unexpected mounts.

**Use when:** Use when runtime mount visibility must be verified from inside.


## 101. `docker exec <container> ls -lah /data`


```bash

docker exec <container> ls -lah /data

```

**Risk:** Low risk

**Technical explanation:** Inspects a mounted data path from inside the container to confirm visibility and content.

**Use when:** Use when the app cannot find expected files or directories.


## 102. `docker exec <container> touch /data/.write_test`


```bash

docker exec <container> touch /data/.write_test

```

**Risk:** High risk

**Technical explanation:** Performs a controlled write test on a mounted path to validate writeability, but it mutates data storage.

**Use when:** Use only when read-only checks are insufficient and write access must be proven.


## 103. `docker system df -v | grep volume`


```bash

docker system df -v | grep volume

```

**Risk:** Low risk

**Technical explanation:** Helps quantify disk space tied to volumes when persistent storage is consuming host capacity.

**Use when:** Use when volumes may be the primary source of disk growth.


## 104. `du -sh /var/lib/docker/volumes/*`


```bash

du -sh /var/lib/docker/volumes/*

```

**Risk:** Low risk

**Technical explanation:** Measures individual volume disk usage on the host for large-volume investigation.

**Use when:** Use when one or more named volumes may be abnormally large.


## 105. `findmnt | grep docker`


```bash

findmnt | grep docker

```

**Risk:** Low risk

**Technical explanation:** Shows Docker-related mounts on the host, which helps explain overlay and bind-mount behavior.

**Use when:** Use when host-level mount propagation or overlay details matter.


## 106. `docker run --rm -v <volume>:/data alpine ls -lah /data`


```bash

docker run --rm -v <volume>:/data alpine ls -lah /data

```

**Risk:** Medium risk

**Technical explanation:** Inspects a volume via a disposable container without touching the primary workload.

**Use when:** Use when you need safe out-of-band visibility into a volume.


## 107. `docker run --rm -v /host/path:/mnt alpine sh -c 'ls -lah /mnt && id'`


```bash

docker run --rm -v /host/path:/mnt alpine sh -c 'ls -lah /mnt && id'

```

**Risk:** Medium risk

**Technical explanation:** Tests a bind mount in an isolated throwaway container to validate host-path behavior.

**Use when:** Use when bind-mount issues should be reproduced without changing the main container.


## 108. `docker volume prune`


```bash

docker volume prune

```

**Risk:** High risk

**Technical explanation:** Removes unused volumes to reclaim space, which is useful during disk-pressure incidents but risky if volume assumptions are wrong.

**Use when:** Use only after confirming the targeted volumes are truly unused.


# Resource Usage, Limits, and Performance


## 109. `docker stats`


```bash

docker stats

```

**Risk:** Low risk

**Technical explanation:** Streams live CPU, memory, network, and block I/O metrics for running containers.

**Use when:** Use when workload pressure is ongoing and needs live observation.


## 110. `docker stats --no-stream`


```bash

docker stats --no-stream

```

**Risk:** Low risk

**Technical explanation:** Prints a one-time resource snapshot so you can capture usage without a persistent stream.

**Use when:** Use when you need a quick usage point-in-time sample.


## 111. `docker inspect -f '{{.HostConfig.Memory}}' <container>`


```bash

docker inspect -f '{{.HostConfig.Memory}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Shows the configured memory limit so you can validate runtime resource constraints.

**Use when:** Use when OOM or memory throttling is suspected.


## 112. `docker inspect -f '{{.HostConfig.NanoCpus}}' <container>`


```bash

docker inspect -f '{{.HostConfig.NanoCpus}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Prints CPU quota configuration so you can detect unexpected CPU throttling limits.

**Use when:** Use when performance symptoms may be quota-driven.


## 113. `docker inspect -f '{{.HostConfig.CpusetCpus}}' <container>`


```bash

docker inspect -f '{{.HostConfig.CpusetCpus}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Shows pinned CPU sets when container placement or performance looks constrained.

**Use when:** Use when CPU pinning may be unintentionally limiting the workload.


## 114. `docker inspect -f '{{.HostConfig.OomKillDisable}}' <container>`


```bash

docker inspect -f '{{.HostConfig.OomKillDisable}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Reveals whether OOM kill behavior was modified, which matters in memory-failure analysis.

**Use when:** Use when memory-exhaustion behavior looks nonstandard.


## 115. `docker inspect -f '{{.HostConfig.LogConfig.Type}}' <container>`


```bash

docker inspect -f '{{.HostConfig.LogConfig.Type}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Shows the container's logging driver so you can explain differing log retention or access behavior.

**Use when:** Use when logs behave differently across containers or hosts.


## 116. `docker update --memory 1g --cpus 1.5 <container>`


```bash

docker update --memory 1g --cpus 1.5 <container>

```

**Risk:** High risk

**Technical explanation:** Adjusts resource limits on a live container for controlled mitigation during capacity incidents, but changes runtime behavior immediately.

**Use when:** Use only when live resource tuning is the intended mitigation.


## 117. `docker exec <container> cat /sys/fs/cgroup/memory.max 2>/dev/null || true`


```bash

docker exec <container> cat /sys/fs/cgroup/memory.max 2>/dev/null || true

```

**Risk:** Low risk

**Technical explanation:** Reads cgroup memory limits from inside the container when runtime constraints need low-level verification.

**Use when:** Use when Docker inspect and in-container reality must be reconciled.


## 118. `docker exec <container> cat /sys/fs/cgroup/cpu.max 2>/dev/null || true`


```bash

docker exec <container> cat /sys/fs/cgroup/cpu.max 2>/dev/null || true

```

**Risk:** Low risk

**Technical explanation:** Reads cgroup CPU quota data from inside the container for quota verification.

**Use when:** Use when CPU quota behavior needs kernel-level confirmation.


## 119. `top -H -p $(docker inspect -f '{{.State.Pid}}' <container>)`


```bash

top -H -p $(docker inspect -f '{{.State.Pid}}' <container>)

```

**Risk:** Medium risk

**Technical explanation:** Attaches host top to the container's main PID so you can inspect thread-level resource usage.

**Use when:** Use when one process or thread may be causing host pressure.


## 120. `pidstat -p $(docker inspect -f '{{.State.Pid}}' <container>) 1 5`


```bash

pidstat -p $(docker inspect -f '{{.State.Pid}}' <container>) 1 5

```

**Risk:** Medium risk

**Technical explanation:** Collects per-process performance data for the container's init PID during high-load analysis.

**Use when:** Use when sustained CPU or I/O usage must be quantified over time.


# Health Checks, Restart Loops, and Recovery


## 121. `docker inspect -f '{{json .State.Health}}' <container>`


```bash

docker inspect -f '{{json .State.Health}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Prints health-check status, failing output, and timing so you can diagnose unhealthy containers precisely.

**Use when:** Use when a container is running but marked unhealthy.


## 122. `docker inspect -f '{{.Config.Healthcheck.Test}}' <container>`


```bash

docker inspect -f '{{.Config.Healthcheck.Test}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Shows the configured health-check command so you can verify what Docker is actually executing.

**Use when:** Use when the health probe itself may be wrong.


## 123. `docker inspect -f '{{.HostConfig.RestartPolicy.Name}}' <container>`


```bash

docker inspect -f '{{.HostConfig.RestartPolicy.Name}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Prints the restart policy so you can explain why a container is or is not coming back automatically.

**Use when:** Use when restart behavior differs from expectation.


## 124. `docker inspect -f '{{.RestartCount}}' <container>`


```bash

docker inspect -f '{{.RestartCount}}' <container>

```

**Risk:** Low risk

**Technical explanation:** Shows cumulative restart count, which is key in crash-loop analysis.

**Use when:** Use when one failure may actually be repeated instability.


## 125. `docker logs --tail 200 <container>`


```bash

docker logs --tail 200 <container>

```

**Risk:** Low risk

**Technical explanation:** Provides recent application output that often explains repeated restart behavior.

**Use when:** Use when the app is looping and log evidence is needed immediately.


## 126. `docker events --since 30m --filter 'container=<container>'`


```bash

docker events --since 30m --filter 'container=<container>'

```

**Risk:** Low risk

**Technical explanation:** Shows recent lifecycle events so you can correlate restarts, kills, and health transitions.

**Use when:** Use when restart timing and cause correlation matter.


## 127. `docker update --restart=no <container>`


```bash

docker update --restart=no <container>

```

**Risk:** High risk

**Technical explanation:** Disables automatic restarts temporarily so you can contain a noisy crash loop during debugging.

**Use when:** Use only when preventing automatic recovery is necessary to stabilize investigation.


## 128. `docker start -ai <container>`


```bash

docker start -ai <container>

```

**Risk:** Medium risk

**Technical explanation:** Starts a stopped container attached to its stdio so you can observe immediate startup failure directly.

**Use when:** Use when you need direct startup evidence from a stopped container.


## 129. `docker run --rm --entrypoint sh <image> -c 'command -v <binary>'`


```bash

docker run --rm --entrypoint sh <image> -c 'command -v <binary>'

```

**Risk:** Low risk

**Technical explanation:** Checks whether an expected binary exists in the image when entrypoint failures are suspected.

**Use when:** Use when image contents may not match assumptions.


## 130. `docker run --rm --entrypoint sh <image>`


```bash

docker run --rm --entrypoint sh <image>

```

**Risk:** Medium risk

**Technical explanation:** Starts a disposable shell in the image so you can inspect filesystem and defaults without touching production containers.

**Use when:** Use when image-level debugging should be isolated from the live workload.


## 131. `docker commit <container> debug/<name>:snapshot`


```bash

docker commit <container> debug/<name>:snapshot

```

**Risk:** High risk

**Technical explanation:** Creates an image snapshot of a live container so you can preserve state before risky recovery work, but it captures mutable runtime state.

**Use when:** Use only when forensic preservation outweighs image immutability concerns.


## 132. `docker export <container> -o container_fs.tar`


```bash

docker export <container> -o container_fs.tar

```

**Risk:** High risk

**Technical explanation:** Exports the container filesystem to a tar archive for offline forensic inspection, preserving evidence before cleanup.

**Use when:** Use when container state must be archived before replacement or deletion.


# Cleanup, Space Reclamation, and Safe Maintenance


## 133. `docker container prune`


```bash

docker container prune

```

**Risk:** High risk

**Technical explanation:** Removes all stopped containers to reclaim space and reduce stale-object clutter, but deletes historical container objects.

**Use when:** Use only after confirming stopped containers no longer contain needed evidence.


## 134. `docker image prune`


```bash

docker image prune

```

**Risk:** Medium risk

**Technical explanation:** Removes dangling images so you can reclaim space without deleting all unused images.

**Use when:** Use when build or pull churn created dangling layers.


## 135. `docker image prune -a`


```bash

docker image prune -a

```

**Risk:** High risk

**Technical explanation:** Removes all unused images, which can recover significant disk but may slow future pulls and remove useful cache.

**Use when:** Use only when disk pressure is severe and image reuse is understood.


## 136. `docker volume prune`


```bash

docker volume prune

```

**Risk:** High risk

**Technical explanation:** Deletes unused volumes to reclaim space, which is effective but risky if volume usage assumptions are wrong.

**Use when:** Use only after validating that unused really means safe to remove.


## 137. `docker network prune`


```bash

docker network prune

```

**Risk:** Medium risk

**Technical explanation:** Deletes unused Docker networks so stale networking objects do not accumulate.

**Use when:** Use when old networks are cluttering the host and are confirmed unused.


## 138. `docker builder prune`


```bash

docker builder prune

```

**Risk:** Medium risk

**Technical explanation:** Removes unused build cache to reclaim disk during image-build pressure incidents.

**Use when:** Use when build cache, not live runtime objects, is the disk problem.


## 139. `docker builder prune -a`


```bash

docker builder prune -a

```

**Risk:** High risk

**Technical explanation:** Deletes all unused build cache, which frees more disk but may impact future build speed significantly.

**Use when:** Use only when deeper cache reclamation is necessary.


## 140. `docker system prune`


```bash

docker system prune

```

**Risk:** High risk

**Technical explanation:** Cleans stopped containers, unused networks, dangling images, and build cache in one action.

**Use when:** Use only with clear scope understanding during controlled maintenance.


## 141. `docker system prune -a --volumes`


```bash

docker system prune -a --volumes

```

**Risk:** Emergency-only

**Technical explanation:** Performs the broadest Docker cleanup including unused images and volumes, which is powerful but carries substantial data-loss risk.

**Use when:** Use only in tightly controlled emergency disk-recovery situations.


## 142. `du -sh /var/lib/docker`


```bash

du -sh /var/lib/docker

```

**Risk:** Low risk

**Technical explanation:** Measures total Docker storage usage on the host to confirm whether Docker is the source of disk pressure.

**Use when:** Use before cleanup so the storage problem is proven.


## 143. `find /var/lib/docker/containers -name '*-json.log' -size +100M`


```bash

find /var/lib/docker/containers -name '*-json.log' -size +100M

```

**Risk:** Low risk

**Technical explanation:** Finds oversized json-file logs that may be consuming unexpected disk space.

**Use when:** Use when log growth may be the fastest reclaim target.


## 144. `truncate -s 0 $(docker inspect -f '{{.LogPath}}' <container>)`


```bash

truncate -s 0 $(docker inspect -f '{{.LogPath}}' <container>)

```

**Risk:** High risk

**Technical explanation:** Zeroes a container log file to reclaim space quickly, which is effective but destroys log evidence and should be coordinated carefully.

**Use when:** Use only when disk exhaustion is urgent and log preservation is no longer required.


# Recovery Verification by Scenario

## After container restart or crash-loop recovery
Re-run:

- `docker ps`
- `docker inspect -f '{{.State.Status}}' <container>`
- `docker inspect -f '{{.RestartCount}}' <container>`
- `docker logs --tail 100 <container>`

Verify that the container is stable, restart count has stopped climbing unexpectedly, and recent logs no longer show the same startup failure.

## After health-check repair
Re-run:

- `docker inspect -f '{{json .State.Health}}' <container>`
- `docker inspect -f '{{.Config.Healthcheck.Test}}' <container>`
- `docker exec <container> curl -I http://localhost:8080/health`

Verify that the configured probe is correct and the container now reports healthy behavior.

## After image or registry correction
Re-run:

- `docker image ls --digests`
- `docker manifest inspect <image>:<tag>`
- `docker image inspect <image>`

Verify that the expected digest, architecture, and runtime image metadata now align.

## After network or port-mapping repair
Re-run:

- `docker port <container>`
- `docker inspect -f '{{json .NetworkSettings.Ports}}' <container>`
- `docker network inspect <network>`
- `ss -lntp | grep ':80 '`

Verify that published ports, container network membership, and host listeners now match the intended exposure path.

## After storage cleanup or volume correction
Re-run:

- `docker system df`
- `docker system df -v`
- `docker inspect -f '{{json .Mounts}}' <container>`
- `docker volume inspect <volume>`

Verify that space pressure is relieved and required mounts and volumes still exist correctly.

## After daemon recovery
Re-run:

- `docker context show`
- `systemctl status docker`
- `docker info`
- `docker ps`

Verify that the daemon is healthy, the CLI reaches the right target, and workloads are visible again.

## Total Commands

This pack contains **144 commands**.

# Troubleshooting Discipline

Strong Docker incident response is not about memorizing the most commands. It is about:

- distinguishing daemon problems from container problems
- separating image issues from runtime issues
- proving network and storage behavior rather than guessing
- preserving logs and filesystem evidence before cleanup
- using recovery steps that restore service without creating a second outage

That is the standard expected from a strong senior Docker Platform, DevOps, or SRE engineer.
