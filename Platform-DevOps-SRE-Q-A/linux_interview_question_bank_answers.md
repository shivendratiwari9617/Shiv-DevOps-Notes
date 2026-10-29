# Senior Linux, Bash, and PowerShell Platform, DevOps, and SRE Interview Answers

## Linux internals and diagnostics

### 1. A Linux host has high load average but moderate CPU utilization. How would you distinguish runnable tasks, uninterruptible I/O, and resource contention?

Load includes runnable tasks and tasks blocked in uninterruptible sleep; it is not CPU utilization. Compare `uptime` with `vmstat` runnable/blocked counts, `ps` process states and wait channels, `iostat` latency, and CPU run-queue/steal data. Segment by service and storage device rather than restarting the host. Mitigate the bottleneck and confirm customer p99 and load return to baseline. A high load with idle CPU often points to I/O, but evidence must decide.

### 2. A process remains in uninterruptible sleep and does not respond to termination signals. What evidence would you collect before restarting the host?

Record `ps` state/wchan, kernel stack via `/proc/<pid>/stack` where permitted, `dmesg`, filesystem and block-device errors, mount type, and network storage health. A D-state process may be waiting in kernel I/O and cannot act on a signal until the operation completes. Check other affected services and whether a host reboot risks corrupting data or losing the only replica. Shift customer traffic first if possible. Reboot only after assessing storage and recovery ownership.

### 3. How do process states, parent-child relationships, PID 1, and signals affect diagnosis of zombie or orphaned processes?

A zombie has exited but its parent has not collected the exit status; signals cannot kill an already-exited process. Inspect `ps` PPID and process tree, parent behavior and child count; fix or restart the parent safely so it reaps children. Orphans are reparented to an init/subreaper that must collect them. In containers, PID 1 must handle signals and reaping. Test termination under load so a process leak does not silently exhaust PID slots.

### 4. A service's memory usage grows over days. How would you distinguish application leaks, page cache, swap, reclaim pressure, and cgroup limits?

Compare process RSS and heap with cgroup `memory.current`, `memory.events`, swap activity, `free` available memory, page cache and reclaim/major faults over time. Container limits can OOM a process while host memory remains available. Run a representative load test or inspect allocation profiles to determine whether retained application objects grow with work. Change limits only with node headroom and evidence. Verify OOM rate and customer latency after remediation.

### 5. A host runs out of file descriptors during peak traffic. How would you identify the affected process, verify limits, mitigate safely, and investigate the cause?

Inspect per-process FD counts in `/proc/<pid>/fd`, `lsof`, process `Limits`, system-wide allocation and logs for `EMFILE` or `ENFILE`. Identify whether sockets, files or leaked handles dominate and compare their growth with traffic. Raise a justified limit or shed traffic to recover, but fix the missing close/connection pool and validate under peak load. Watch neighboring services and the global limit. A limit increase without leak repair only delays failure.

### 6. A service is slow only under production load. How would you use CPU, memory, I/O, scheduler, and application signals to localize the bottleneck?

Correlate p99 customer latency and request queue depth with per-process CPU, run queue, throttling, memory faults/swap, I/O wait and disk latency. Add dependency timings and traces to distinguish host saturation from lock contention or slow remote calls. Compare a healthy and affected node at the same traffic rate. Mitigate the narrowest cause and verify both customer latency and neighbor health. Do not equate idle CPU with idle system capacity.

### 7. Two hosts run the same binary but exhibit different behavior. How would you compare libraries, environment, kernel, permissions, and configuration without assuming they are identical?

Compare binary and library hashes, dynamic loader output, kernel and container/cgroup settings, environment with secrets redacted, service unit, UID/GID, mounts, and network routing. Check config precedence and recent deployment or package changes. Reproduce the failing request with the same input on both hosts and find the first divergent syscall or application log. Fix the declared runtime inputs rather than copying an unknown stateful host. Test a clean replacement host.

## Networking, storage, and services

### 8. A host resolves a service name but connections time out. How would you trace routes, sockets, firewall rules, listeners, and the return path?

Verify resolved address, destination and port, then check listener with `ss`, route with `ip route get`, and TCP handshake from the client. A timeout differs from connection refusal; inspect firewalls, security policy, NAT, packet captures at both ends and asymmetric return routing. Compare a healthy source and target. Change the narrowest broken hop, then verify the real application transaction and TLS or auth rather than stopping at an open port.

### 9. A filesystem reports no free space, but visible files account for only half its capacity. How would you investigate deleted-but-open files, mounts, reserved space, and log growth?

Confirm `df` and `du` examine the same filesystem and mount namespace; check `lsof +L1` for deleted files held open, filesystem reserved blocks, hidden data under mount points, snapshots and log rotation. Do not blindly delete active files: a process holding an unlinked file will keep the space allocated. Rotate or restart the owner safely when justified. Preserve logs needed for the incident and verify available space remains stable under load.

### 10. A service cannot create files although disk space remains. How would you distinguish inode exhaustion, permissions, quotas, read-only mounts, and filesystem errors?

Check error code and path first: `df -i` for inodes, filesystem and user/project quotas, effective UID/GID and ACLs, mount flags, and `dmesg` for I/O or remount-read-only events. Compare with a known-good writable path under the same service identity. Do not remount read-write after filesystem errors without storage assessment. Mitigate safely and run a real application write; `df -h` free space alone does not establish writability.

### 11. A systemd service works interactively but fails at boot. How would you inspect its environment, dependencies, permissions, working directory, logs, and restart behavior?

Inspect `systemctl status` and `journalctl -u`, unit file and drop-ins, service identity, `WorkingDirectory`, `EnvironmentFile`, sandbox options, and dependency ordering. An interactive shell may have a different PATH, mounts, network or credentials. Verify the file path and permissions under the actual service identity, then test a clean boot and a dependency restart. Set bounded restarts rather than hiding a permanent failure in a loop.

### 12. How would you troubleshoot intermittent DNS failures across Linux hosts while separating resolver configuration, caching, network paths, and upstream behavior?

Capture failing query name, server, response code and time from affected and healthy hosts; inspect `/etc/resolv.conf`, systemd-resolved or other local cache, search domains and timeout settings. Compare UDP/TCP 53 reachability, upstream latency and packet loss across zones. Avoid substituting hosts-file entries for a systemic issue. Test internal and external names plus customer request success after repair; DNS success alone does not prove service availability.

### 13. How do ownership, mode bits, ACLs, sticky bits, and privilege elevation affect access to a shared operational directory?

Traversal needs execute permission on every parent directory; creating files requires write and execute on the parent. ACLs can grant or restrict specific identities beyond mode bits, and the sticky bit restricts deletion in a shared writable directory to owner or privileged users. Test with the actual service UID/GID and `getfacl`, not a root shell. Prefer a scoped group or ACL over blanket `chmod 777` or unrestricted sudo, and audit elevation paths.

## Bash and automation

### 14. How do quoting, word splitting, globbing, and command substitution affect a Bash script that processes filenames with spaces or newlines?

Unquoted expansions undergo splitting and globbing, and `$(...)` removes trailing newlines, so line-based loops mis-handle valid filenames. Use quoted variable expansion, arrays and NUL-delimited input such as `find -print0` with `read -r -d ''` for path lists. Use `--` before user-provided filenames where the command supports it. Test spaces, newlines, leading dashes and wildcard characters on a disposable directory before changing production files.

### 15. When can `set -e` and `pipefail` provide misleading confidence? How would you ensure an automation failure is visible and actionable?

`set -e` has context-dependent exceptions in conditionals, functions and command substitutions; without `pipefail`, a pipeline returns the last command's status even if an earlier stage failed. Check expected failure branches explicitly, validate outputs, use `set -o pipefail`, and capture statuses where recovery differs by stage. Log operation IDs and sanitized errors; make nonzero exit reach the scheduler and page owner. Exercise producer failure and partial remote effects, not only syntax.

### 16. Design a Bash cleanup script that may run concurrently and must never delete files outside an approved directory. How would you handle locking, path validation, dry runs, and failure?

Run with least privilege, acquire an exclusive `flock`, and use a fixed approved root with validated canonical path and explicit allowlisted targets. Reject empty, root, symlink and unexpected mount paths; prevent TOCTOU by constraining filesystem access or using safer per-entry operations rather than trusting a one-time string check. List exact targets in dry run, require an explicit execute flag and log deletions. Test concurrency and malicious path substitutions in a disposable sandbox before production use.

### 17. How would you safely retry a shell operation that creates remote resources when a timeout leaves the outcome unknown?

Persist a stable idempotency key and query the remote API for the resource or operation ID before submitting another create. If the result is unknown, stop or reconcile rather than blindly retrying; serialize conflicting requests. Use bounded backoff only for operations with known retry semantics, record each attempt, and verify final state in the target system. Test timeout-after-create-before-response to prove reruns do not duplicate customer resources.

### 18. What race conditions can occur with temporary files and signal handling? How would you test cleanup after interruption?

Predictable `/tmp` names permit collision or symlink attacks; use `mktemp` in a restricted directory, quote paths and set appropriate permissions. Register cleanup traps early, but understand SIGKILL and host failure cannot run them; use a later orphan-cleanup policy. Avoid writing incomplete output over a live file: write temp, sync if required and atomically rename on the same filesystem. Test concurrent runs and interrupts at several points while checking that valid prior state remains intact.

### 19. When would you replace a shell script with Python, configuration management, or an orchestrator? Which complexity and operational risks drive the decision?

Shell is suitable for small, local, well-bounded command sequences. Move to a language with structured APIs and tests when parsing, state reconciliation, retries, or error branches grow complex; use configuration management for repeatable host state and an orchestrator for multi-host deployment and scheduling. Evaluate ownership, observability, concurrency, secrets and rollback, not code length alone. Preserve the existing contract during migration and test crash and partial-fleet recovery.

## PowerShell and cross-platform operations

### 20. How does PowerShell's object pipeline differ from text processing in Bash, and when can formatting output too early break automation?

PowerShell pipelines pass typed objects with properties, unlike Bash's usual byte streams. Filter and select properties before rendering; `Format-Table` emits formatting objects, not the original resource objects, so piping its output into state-changing cmdlets breaks automation. Use API objects for configuration and explicit serialization for inter-process contracts. Test expected types and failures in scheduled execution, where formatted console output may hide missing data.

### 21. How do terminating and non-terminating PowerShell errors affect error handling? How would you ensure a failed remote operation is surfaced?

Many cmdlets emit non-terminating errors that do not enter `catch` by default. For critical remote calls use `-ErrorAction Stop` or scoped `$ErrorActionPreference = 'Stop'`, and inspect returned per-host results and process exit codes. Log target and operation ID without credentials, then fail the job if required hosts did not reach desired state. Test an unreachable host and a permission denial, not only a successful interactive run.

### 22. Design a PowerShell function that changes configuration on many servers safely. How would you handle credentials, retries, partial failures, logging, and reruns?

Discover current state and target inventory first, use scoped short-lived identity and a dry-run/WhatIf path, then apply in canary and bounded batches. Record per-host desired and observed values plus operation IDs; retry only idempotent reconciliation after verifying ambiguous outcomes. Stop on customer-impact thresholds and keep a documented rollback for compatible changes. Produce an auditable summary of succeeded, failed and unreachable hosts without dumping credentials. Test interruption halfway through the fleet.

### 23. A command works in an interactive PowerShell session but fails in a scheduled job. How would you compare identity, profiles, environment, paths, and module availability?

Compare effective identity, token privileges, execution policy, working directory, PATH, PowerShell edition, profile loading, module versions and network credentials under the scheduled account. Use explicit absolute paths and module imports, not assumed user-profile setup. Capture structured errors without secrets, then reproduce through the actual scheduler context. Confirm remote resource access and end-to-end job result, not just command syntax.

### 24. How would you choose between PowerShell remoting and other automation mechanisms across Windows and Linux while protecting credentials and auditability?

Choose the mechanism supported by target OS, network policy and desired-state model: PowerShell remoting for managed hosts with secure WinRM or SSH, configuration management for repeatable drift correction, or an orchestrator for deployment lifecycles. Use short-lived scoped identities, TLS or host-key verification, least privilege and per-host audit. Avoid credential fan-out through scripts or logs. Test partial failure and rerun behavior on both Windows and Linux targets.

### 25. How would you test a cross-platform automation tool against different operating-system versions and avoid assumptions about paths, commands, and output formats?

Define supported OS and PowerShell versions, then run a matrix on clean hosts for paths, case sensitivity, permissions, encodings, line endings, and available commands. Use structured APIs and capability detection rather than parsing human-readable command output. Exercise locale changes, missing dependencies and non-admin identity. Canary a real configuration change and rollback before fleet rollout. Make unsupported platforms fail clearly instead of partially applying changes.

## Incidents and leadership

### 26. An automation job changes configuration successfully on half a fleet and then fails. How would you establish actual state, contain impact, and decide whether to resume or roll back?

Stop further batches, enumerate actual desired and observed state per host, and compare customer errors by host/version. Do not trust a count of API successes if acknowledgments could be lost. If the new configuration is safe, reconcile remaining hosts idempotently; if harmful, roll back already-changed hosts using recorded prior values and health gates. Track unreachable hosts separately. Verify uniform fleet state and a real customer path before closing.

### 27. A production host is degraded and a reboot may restore service but erase useful evidence. How would you prioritize customer impact, evidence capture, and safe recovery?

Shift traffic or fail over first if available, then collect bounded evidence: timestamps, kernel logs, process stacks, disk and network errors, resource pressure and service status. Assess whether reboot can worsen filesystem corruption or leave a single data writer un-fenced. If customer impact is severe and recovery is safe, reboot with owner approval while preserving the collected snapshot. Validate applications and data consistency afterward and document which evidence was lost.

### 28. Tell me about an operational script you improved after an incident. How did you demonstrate that the new version is safe to rerun and easier to debug?

Use a real incident: explain the original failure, customer risk and your change, such as idempotent target inspection, safe path validation or per-host results. Show a crash-and-rerun test plus observable before-and-after outcomes, including partial failures. State what you did personally and any remaining risk. Do not invent personal experience for a behavioral question; interviewers can probe exact evidence and trade-offs.

## Evidence-based exercises

### 29. `df -h` shows a filesystem at 100%, while `du` of visible files accounts for substantially less usage. What commands and checks would you use to investigate safely, and what evidence would justify releasing space?

Confirm mount and namespace using `findmnt` and `df`, compare `du -x` on the same filesystem, inspect `lsof +L1` for unlinked open files, reserved blocks, snapshots and hidden mount-point data. Check inode usage separately. Release space only after identifying owner, data-retention requirements and whether a restart or log rotation safely closes the file. Verify free space and customer service remain stable afterward.

### 30. A Bash job runs `producer | filter | uploader` and exits successfully even when `producer` fails. How would you reproduce the behavior, preserve the relevant exit status, and test that automation reports the failure?

Use a test producer that exits nonzero while downstream commands succeed; the pipeline normally reports only `uploader`'s status. Set `set -o pipefail` and explicitly inspect `${PIPESTATUS[@]}` immediately after the pipeline if the failure response depends on which stage failed. Avoid overwriting those statuses with another command. Test exit status, error logging and scheduler alert for each stage's failure, including when uploader partially accepted data.

### 31. A PowerShell scheduled task succeeds interactively but fails with `Access is denied` under its service identity. How would you compare effective identity, resource permissions, environment, and remote-access context without exposing credentials?

Record the task's effective identity and token privileges, target ACL or share permissions, remoting authentication and whether double-hop access is involved. Compare working directory, loaded profile, mapped drives and module versions without printing tokens. Reproduce under the scheduled identity with a minimally scoped test resource and `-ErrorAction Stop`. Grant only required access and verify the full task, including remote writes and audit log, succeeds unattended.

### 32. A systemd service restarts repeatedly after a deployment; its journal reports that a configuration file cannot be opened, although the file exists. How would you investigate path resolution, ownership, permissions, service sandboxing, and the safest way to restore service?

Check the unit's effective `ExecStart`, working directory, root/chroot, mount namespace, `ProtectSystem` and `ReadOnlyPaths`, then test traversal and read permissions as the service user. Compare file ownership, symlink target, SELinux/AppArmor denials and deployment changes. Pause restart loops if they cause load, correct the narrow path or permission or roll back to compatible config. Verify healthy startup and a customer request after restoration.

## Product-company practical exercises

### 33. During a sale, `uptime` shows a high load average, but `top` shows substantial idle CPU and several processes in `D` state. What would you investigate first, how would you limit customer impact, and what evidence would you preserve?

Correlate blocked tasks and wait channels with disk or network storage latency and kernel errors; compare affected checkout endpoints and other hosts. Shift traffic to healthy capacity, avoid mass restarts that may pile up on the same I/O path, and preserve process stacks, `iostat`, kernel logs and filesystem health. Coordinate with storage owners before a reboot. Declare recovery after transaction success and I/O latency normalize, not when CPU looks idle.

### 34. A deployment script runs `rm -rf \"$TARGET\"/*` as a privileged user. What inputs and filesystem conditions could make this unsafe, and how would you redesign and test the operation before using it on a production fleet?

With an empty `TARGET`, `\"$TARGET\"/*` can expand to `/*`; whitespace, a replaced symlink, a bind mount, or a stale path can redirect privileged deletion into active data. The glob also omits dotfiles, so the script may report success without emptying the target. Stop using wildcard deletion for deployments: publish an immutable release directory, atomically switch a pointer, and have a dedicated low-privilege cleanup worker remove only retired release IDs from a fixed directory after a retention period. `realpath` and a one-time symlink or mount check are insufficient against concurrent path swaps; if adversarial writers can modify the directory, use directory-descriptor-relative, no-follow filesystem operations in a purpose-built tool or remove their write access. Lock against concurrent cleanups, log exact targets in dry run, and test empty/root paths, dotfiles, symlink or mount swaps and interruption in a disposable environment. Verify the live release and customer requests remain intact.

### 35. A PowerShell rollout changes settings on 60 servers and reports 45 successes, 10 failures, and five unreachable hosts. How would you establish the actual fleet state, recover safely, and make the operation auditable and repeatable?

Query observed settings on all 60 by stable host ID; a reported failure may have applied before the acknowledgment was lost, while unreachable hosts remain unknown. Stop further rollout, compare customer health across the changed 45 and the rest, and reconcile in controlled batches or revert harmful changes from recorded prior state. Store target, old/new values, operation ID and errors without secrets. Verify every reachable host and schedule a guarded check for the five unknowns.

### 36. A production Linux service is timing out while the host has high load, mostly idle CPU, and many tasks in `D` state. What kernel, storage, process, and application evidence would you collect first, how would you reduce customer impact, and what would make a reboot unsafe?

Collect D-state process wait channels/stacks, `dmesg` I/O errors, mount and block-device latency, and per-request application traces; compare affected dependencies and healthy hosts. Drain or redirect traffic first, preserving transactional data and one-writer guarantees. A reboot is unsafe if storage is failing, the only replica holds unflushed data, or failover fencing is uncertain. Restore the I/O path or fail over safely, then confirm customer requests and data consistency.