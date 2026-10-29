# Week 1 — Linux Deep Troubleshooting — Candidate Answers (60 LPA+)

------------------------------------------------------------------------

## Q1 \| High CPU / Process Management \| P1 \| Production Troubleshooting

**Interview Question:**

A critical production API node suddenly reaches 95% CPU without any new deployment, while customer latency and error rate are increasing. How would you isolate whether the root cause is an application thread, runaway process, kernel activity, dependency retry storm, or noisy neighbor issue, and what recovery decision would you make first?

**Candidate Answer:**

I would first treat this as a live customer-impacting incident, not only a CPU issue. I would confirm blast radius from latency, 5xx rate, queue depth, and whether only one node or the whole pool is affected. If the node is behind a load balancer and capacity allows it, my first recovery decision would be to drain or reduce traffic from that node before deep debugging, so investigation does not continue hurting customers.

For isolation, I would compare top, pidstat, ps sorted by CPU, run queue, steal time, interrupts, and cgroup usage. If one process is hot, I would capture thread-level evidence using top -H, jstack/gdb/perf depending on runtime, and map the hot thread to code path. If CPU is in system time, I would check kernel activity, network interrupts, conntrack, disk interrupts, or excessive context switching. If many app workers are hot but no release happened, I would check dependency latency, retry amplification, traffic spike, cache miss rate, and connection pool behavior.

Mitigation would be rollback only if a recent config or dependency change is found; otherwise I would throttle, scale horizontally, restart only the bad process after evidence capture, and validate recovery through latency, error rate, and CPU normalization. RCA would add retry budgets, CPU/thread dashboards, and runbook automation.

------------------------------------------------------------------------

## Q2 \| Load Average / CPU Saturation \| P1 \| SRE

**Interview Question:**

A Linux server shows a very high load average, but CPU utilization is not proportionally high. How would you distinguish CPU saturation from blocked IO, uninterruptible sleep, process queue buildup, or kernel-level contention during a live incident?

**Candidate Answer:**

I would not assume high load means CPU saturation. Load average includes runnable tasks and tasks stuck in uninterruptible sleep, so I would first check whether the load is coming from R-state processes waiting for CPU or D-state processes blocked on IO. I would use vmstat 1 for run queue, blocked processes, context switches, si/so, and wa; then iostat -xz 1 and pidstat -d to identify disk latency and per-process IO pressure.

During the incident I would inspect ps -eo state,pid,ppid,comm,wchan sorted by state and look for D-state waits such as nfs, ext4, xfs, blk, futex, or network storage calls. If CPU idle is high but load is high, I would suspect blocked IO, NFS, storage degradation, lock contention, or a kernel wait path. If runnable queue is high and CPU is near 100%, then it is CPU pressure.

For mitigation, I would stop or throttle backup, batch, logging, or compaction jobs if they are causing IO waits, drain traffic from the host if customer-facing latency is impacted, and avoid killing D-state processes because they usually cannot exit until the kernel wait completes. RCA would include IO wait alerts, per-disk latency dashboards, workload isolation, and capacity limits for noisy jobs.

------------------------------------------------------------------------

## Q3 \| Zombie Processes \| P2 \| Production Troubleshooting

**Interview Question:**

A production batch worker is accumulating zombie processes and the process count keeps increasing after every job run. How would you prove the parent process is not reaping children, assess the risk to the node, and decide whether to restart the parent, drain traffic, or patch the application?

**Candidate Answer:**

I would prove the zombie issue by showing processes in Z state and mapping them to the parent. I would run ps -eo pid,ppid,state,comm,args and inspect /proc/\<zombie\>/status to confirm State: Z. Then I would identify the parent process using PPID and check whether it has a bug in child reaping, missing wait/waitpid handling, signal handling issues around SIGCHLD, or a job-launch pattern that forks without collecting exit status.

The risk is not CPU or memory from zombies themselves; the risk is PID table exhaustion, process-count limits, confused monitoring, and eventual inability to fork new workers. I would check current pid usage, TasksMax, pids.current if cgroups are used, and whether job throughput is degrading.

If the service is customer-facing, I would first stop scheduling new jobs or drain traffic from that worker. If the parent can be restarted safely, I would restart only the parent after capturing evidence and ensuring in-flight jobs are either completed or recoverable. I would not try to kill zombies directly because they are already dead. Permanent fix would be an application patch to reap children correctly, plus alerts on zombie count, pids usage, and a runbook for controlled parent restart.

------------------------------------------------------------------------

## Q4 \| Stuck Processes / D-State \| P1 \| Production Troubleshooting

**Interview Question:**

Several production processes are stuck and cannot be killed even with forced termination, and the node is becoming unstable. How would you investigate uninterruptible sleep, storage or NFS dependency impact, and decide whether a controlled reboot is safer than waiting?

**Candidate Answer:**

I would treat unkillable processes as a possible kernel or storage dependency problem, not as a normal process-management issue. First I would confirm the state with ps -eo pid,state,wchan,comm,args and look for D-state. Then I would collect kernel evidence from dmesg, journalctl -k, blocked task warnings, /proc/\<pid\>/stack where available, and IO signals from iostat, mount status, NFS metrics, and storage or cloud volume health.

If the wait channel points to NFS, block storage, filesystem, or device mapper, I would validate whether the backing dependency is degraded. I would avoid repeated kill -9 attempts because D-state processes cannot handle signals until the kernel call returns, and aggressive action may corrupt in-flight writes or make the host worse.

For recovery, if the node is part of a pool, I would cordon/drain or remove it from the load balancer first. If storage recovers quickly, processes may unwind. If the host is blocking critical service recovery, has stuck mounts, or kubelet/systemd is also impacted, I would choose a controlled reboot after evidence capture and stakeholder communication. RCA would focus on storage timeout settings, NFS mount options, dependency SLOs, and safer fail-fast behavior.

------------------------------------------------------------------------

## Q5 \| systemd Service Startup Failure \| P1 \| Production Troubleshooting

**Interview Question:**

A business-critical service fails to start after a scheduled reboot, but the same binary starts manually from an interactive shell. How would you debug the systemd execution context, environment, dependencies, permissions, working directory, and startup ordering gap?

**Candidate Answer:**

I would compare the interactive shell environment with the systemd execution environment because manual success often hides missing unit assumptions. I would start with systemctl status, journalctl -u service, and systemctl show to inspect ExecStart, User, Group, WorkingDirectory, Environment, Limit settings, Restart behavior, and dependency ordering.

Then I would validate the exact runtime user using sudo -u \<user\> and run the same binary with the same working directory and environment file. I would check missing PATH entries, secret files, relative paths, permissions on parent directories using namei -l, executable permissions, SELinux/AppArmor denials, and whether required mounts or network dependencies are ready when the unit starts. I would also inspect Type=simple/notify/forking because a wrong type can make systemd think startup failed even when the process exists.

The safe fix is to make the unit explicit rather than relying on shell behavior: absolute paths, EnvironmentFile, WorkingDirectory, RuntimeDirectory, required mounts, After/Wants dependencies, and correct user ownership. After the change I would run daemon-reload, restart during a controlled window or on a drained node, and validate boot-time startup through journal logs and service health checks.

------------------------------------------------------------------------

## Q6 \| systemd Restart Loop \| P1 \| SRE

**Interview Question:**

A service enters a rapid restart loop under systemd and then stops due to restart limits during peak traffic. How would you identify whether the loop is caused by bad config, dependency readiness, resource limits, health assumptions, or restart policy design?

**Candidate Answer:**

I would first stop the restart storm from amplifying the incident. If the service is flapping during peak traffic, I would remove the node from rotation or stop the unit after capturing enough logs, because continuous restarts can overload dependencies, fill logs, and hide the real failure.

I would inspect journalctl -u, systemctl status, exit codes, core dumps, recent config or secret changes, dependency health, and systemd properties such as Restart, RestartSec, StartLimitBurst, StartLimitIntervalSec, TimeoutStartSec, User, EnvironmentFile, and resource limits. If the service exits immediately, I would validate config syntax and required files. If it starts and then dies, I would check readiness assumptions, failed dependency calls, port conflicts, permissions, OOM, and file descriptor limits.

The recovery decision depends on evidence. For bad config or secret, I would rollback the config and restart once. For dependency readiness, I would fix ordering or add a proper readiness gate, not blind sleeps. For resource exhaustion, I would tune limits and scale safely. The permanent fix is restart-policy design with backoff, actionable alerts before StartLimitHit, config validation in CI/CD, and a runbook that distinguishes bad deploy from infrastructure pressure.

------------------------------------------------------------------------

## Q7 \| systemd Unit Dependencies \| P2 \| Implementation

**Interview Question:**

After a patch cycle, an application intermittently starts before its mount, network, or secret-fetch dependency is ready. How would you redesign the systemd unit dependency model to prevent race conditions without adding fragile sleep-based workarounds?

**Candidate Answer:**

I would remove race conditions by making dependency expectations explicit in the unit instead of adding sleep commands. The unit should declare network-online, required mount paths, secret availability checks, correct runtime directory ownership, and failure behavior that is safe and observable. I would also ensure the dependency services themselves are enabled and monitored.

Snippet:

\[Unit\]

Description=Production API Service

Wants=network-online.target

After=network-online.target remote-fs.target secret-fetch.service

RequiresMountsFor=/opt/app /var/lib/app

Requires=secret-fetch.service

\[Service\]

User=appsvc

Group=appsvc

WorkingDirectory=/opt/app

EnvironmentFile=/etc/app/app.env

ExecStartPre=/usr/bin/test -r /etc/app/secrets.env

ExecStartPre=/usr/bin/test -d /var/lib/app

ExecStart=/opt/app/bin/server

Type=notify

Restart=on-failure

RestartSec=10s

TimeoutStartSec=60s

RuntimeDirectory=app

\[Install\]

WantedBy=multi-user.target

I would validate this by reboot-testing one canary node, checking critical-chain ordering, journal timestamps, and service health. This design is safer because it fails fast with a clear missing dependency instead of starting partially and causing customer-facing errors.

------------------------------------------------------------------------

## Q8 \| Service Running But Not Reachable \| P1 \| Production Troubleshooting

**Interview Question:**

An application service shows as active on the Linux host, but clients cannot reach it. How would you separate process health from socket binding, local firewall, bind address, routing, SELinux or AppArmor restrictions, and upstream load balancer health behavior?

**Candidate Answer:**

I would separate service process health from network reachability. An active systemd unit only proves the process is alive; it does not prove it is listening on the correct interface, allowed through firewall policy, or passing upstream health checks.

I would start locally with ss -ltnp or lsof -i to verify the expected port, protocol, and bind address. If the service is bound only to 127.0.0.1 but clients connect through the host IP, that is a config issue. I would then curl locally, curl the host IP from the same subnet, and test from an upstream/load-balancer perspective. In parallel I would inspect iptables/nftables/firewalld, route tables, SELinux/AppArmor denials, application logs, and whether the process is inside a different network namespace or container.

For mitigation, I would avoid restarting blindly. If the wrong bind address or firewall rule is confirmed, I would apply the smallest safe config change, reload the service, and validate from client and load balancer health checks. Permanent prevention would include socket-level synthetic checks, deployment validation for bind address, firewall policy as code, and alerting on active-but-unreachable states.

------------------------------------------------------------------------

## Q9 \| Port Binding Conflict \| P1 \| Production Troubleshooting

**Interview Question:**

A deployment fails because the service cannot bind to its expected port, but the previous release was supposedly stopped cleanly. How would you investigate stale processes, socket states, systemd orphaning, privileged port requirements, and safe recovery without causing duplicate service instances?

**Candidate Answer:**

I would first identify who is actually holding the port and whether it is a live process, an orphaned child, a previous systemd cgroup member, or a different service. I would use ss -ltnp, lsof -i :\<port\>, systemctl status for the old unit, and systemd-cgls to see if systemd still owns child processes. I would distinguish LISTEN from TIME_WAIT because TIME_WAIT does not normally block a new listener when SO_REUSEADDR behavior is correct.

I would not kill by port without understanding ownership, because that can create duplicate service instances or kill an unrelated critical process. If the previous release left an orphan, I would stop the unit, kill only the identified cgroup or PID after confirming it is safe, and validate that no listener remains before starting the new release. If the port is privileged, I would check whether the runtime user has cap_net_bind_service or whether the service should be fronted by a reverse proxy.

The permanent fix is better systemd process tracking, KillMode control, ExecStop behavior, graceful shutdown timeout, pre-start port validation, and pipeline gates that fail before traffic is shifted.

------------------------------------------------------------------------

## Q10 \| Permissions / Ownership \| P1 \| Security

**Interview Question:**

A production deployment starts failing with permission denied after a user/group ownership change, and teams are asking to temporarily make the directory world-writable. How would you troubleshoot the access path, preserve least privilege, and implement a secure permanent fix?

**Candidate Answer:**

I would reject the world-writable workaround because it solves the symptom by creating a production security risk. My first step would be to map the complete access path using id \<service-user\>, namei -l \<path\>, ls -ld on every parent directory, getfacl, and ls -Z if SELinux is enabled. Permission denied can come from parent execute bits, wrong group, ACL removal, umask changes, immutable attributes, mount options, or MAC policy, not only the final file.

I would reproduce the failure as the runtime user using sudo -u \<user\> and verify exactly which path operation fails: read, write, execute, rename, or create. The safe fix would be least privilege: correct owner/group, setgid directory for shared deployments, targeted ACLs for the service account, and a deployment umask that creates predictable permissions. If multiple teams need access, I would use a controlled group model, not chmod 777.

For prevention, I would put ownership and permission checks into the deployment pipeline, manage them through configuration management or IaC, and add audit logging for sensitive directories. I would also document emergency access that is time-bound and reviewed.

------------------------------------------------------------------------

## Q11 \| sudo / Privilege Delegation \| P2 \| Security

**Interview Question:**

A CI/CD runner service account can deploy successfully on some Linux nodes but fails on others due to sudo and group access differences. How would you validate privilege delegation, avoid over-permissioning, and standardize access safely across the fleet?

**Candidate Answer:**

I would treat this as a privilege-standardization and security-control issue, not as a reason to give broad sudo. First I would compare working and failing nodes using id, groups, sudo -l -U \<service-account\>, /etc/sudoers, /etc/sudoers.d, PAM rules, and directory permissions for deployment paths. I would also check whether group membership changes require a session refresh for the runner service.

The goal is to identify the minimum command set the runner needs. For example, it may need to restart one systemd unit, copy artifacts to one directory, or run a specific deployment script. I would avoid NOPASSWD: ALL and instead define command aliases with full paths, controlled arguments where possible, and no shell escape risk. I would validate sudoers syntax with visudo -c before rollout.

To standardize safely, I would manage sudoers and group membership through Ansible, cloud-init, or an image baseline, then canary it on a small runner group. I would add auditd or sudo logs forwarding, periodic drift checks, and pipeline preflight checks that fail clearly when privilege delegation differs across nodes.

------------------------------------------------------------------------

## Q12 \| Executable Permission / Runtime User \| P2 \| Production Troubleshooting

**Interview Question:**

A release artifact exists on the target Linux server, but systemd reports execution failure while manual inspection shows the file is present. How would you investigate execute permissions, mount options, interpreter paths, runtime user context, and artifact integrity?

**Candidate Answer:**

I would start from the exact systemd error because execution failures often surface as 203/EXEC, permission denied, bad interpreter, or no such file even when the artifact exists. I would inspect journalctl -u, systemctl status, and the unit's ExecStart path. Then I would verify file mode, ownership, and parent directory execute permissions with ls -l and namei -l.

Next I would check runtime context: the User/Group in the unit, WorkingDirectory, EnvironmentFile, and whether the filesystem is mounted with noexec. For scripts, I would validate the shebang interpreter path, line endings, and execute bit. For binaries, I would check architecture, dynamic linker availability with ldd, checksum/artifact integrity, and whether the artifact was partially copied. On hardened nodes I would also check SELinux/AppArmor denials.

Mitigation would be to redeploy a verified artifact or correct the unit/runtime permission on a drained node, not chmod broadly. Long term, I would add artifact checksum validation, atomic deployment directories, execute-permission checks, and a systemd smoke test as part of the release pipeline.

------------------------------------------------------------------------

## Q13 \| Cron Failures \| P1 \| Production Troubleshooting

**Interview Question:**

A nightly reconciliation cron job silently stops processing records, but no one noticed until business reporting failed. How would you debug cron execution context, environment differences, locking, logging gaps, missed schedules, and build monitoring so this cannot fail silently again?

**Candidate Answer:**

I would first quantify business impact: which reconciliation window failed, how many records are pending, and whether a safe manual replay is possible. Then I would debug cron as a non-interactive runtime, because jobs often work manually but fail under cron due to PATH, HOME, shell, working directory, locale, credentials, or missing environment variables.

I would inspect the user's crontab, /etc/cron\*, cron service status, /var/log/cron or syslog, mail output, job stdout/stderr redirection, and the script's exit codes. I would check locking because a stale lock may make the job exit silently. I would also validate whether the machine was down during the schedule, whether time sync changed, and whether the job depends on mounted paths or network services.

For mitigation, I would replay the job in an idempotent mode with business approval and record exactly what was processed. For prevention, I would move critical cron jobs to systemd timers or a scheduler with observable status, add structured logs, explicit environment files, lock timeouts, exit-code alerts, last-success metrics, and a dashboard showing expected versus completed reconciliation runs.

------------------------------------------------------------------------

## Q14 \| Cron Overlap / Automation Failure \| P2 \| Platform Engineering

**Interview Question:**

A scheduled Linux maintenance job starts overlapping with previous runs and causes high CPU, lock contention, and degraded API performance. How would you detect overlapping execution, mitigate the current impact, and redesign the automation for concurrency safety?

**Candidate Answer:**

I would first stop the current overlap from damaging customer traffic. I would identify running instances with ps, pgrep -af, lock files, systemd timer state, and CPU/IO metrics. If the maintenance job is causing API degradation, I would pause new starts, kill or throttle the least risky duplicate process, and verify whether partial work is safe to resume.

For root cause, I would check why the previous run exceeded its normal duration: larger dataset, downstream slowness, stuck lock, missing timeout, or schedule frequency too aggressive. I would also check whether the job is idempotent, whether it uses shared temp files, and whether it competes with production API processes for CPU, IO, or database locks.

The redesign would use concurrency safety by default: flock or systemd timer Persistent/RandomizedDelaySec with a lock timeout, clear exit codes, max runtime, resource limits, and idempotent checkpoints. I would schedule heavy maintenance outside peak, put cgroup CPU/IO limits if needed, and emit metrics for duration, skipped due to lock, records processed, and last success. This converts hidden overlap into controlled, observable automation behavior.

------------------------------------------------------------------------

## Q15 \| Disk Full / Deleted Files \| P1 \| Production Troubleshooting

**Interview Question:**

A production server shows 100% disk usage, but directory-level usage does not explain the consumed space. How would you identify deleted files still held by running processes, decide whether to truncate or restart safely, and prevent recurrence?

**Candidate Answer:**

I would compare df and du first. If df shows 100% usage but du over top directories does not explain it, the classic cause is deleted files still held open by running processes. I would run lsof +L1 or lsof \| grep deleted and identify the process, file descriptor, original path, and size. I would also check journald, container logs, temp directories, and mount boundaries to avoid chasing the wrong filesystem.

For recovery, I would not randomly delete more files if the space is held by an open descriptor. If the process is safe to restart, I would drain traffic or stop the service gracefully so the descriptor is released. If restart is too risky and the file is only a log, I may truncate through /proc/\<pid\>/fd/\<fd\> after confirming with the application owner, preserving enough evidence first.

After space is recovered, I would validate log writes, service health, and no further growth. Permanent prevention would include correct log rotation with reopen/copytruncate behavior, disk and inode alerts, per-service log budgets, journald limits, and a runbook that explicitly checks deleted open files.

------------------------------------------------------------------------

## Q16 \| Inode Exhaustion \| P1 \| Production Troubleshooting

**Interview Question:**

A node has enough free disk capacity, but deployments and log writes fail with no space left errors. How would you investigate inode exhaustion, locate the responsible file pattern, and design cleanup or retention controls?

**Candidate Answer:**

I would confirm inode exhaustion with df -i because free GB does not matter if the filesystem cannot create new directory entries. Then I would locate where inodes are consumed by counting files per directory, usually under logs, cache, sessions, temp files, package cache, CI workspaces, or application-generated small files. I would use find carefully with mount boundaries so I do not overload an already unhealthy host.

During recovery, I would avoid broad rm -rf against unknown directories. I would identify the responsible file pattern, confirm retention requirements, preserve samples if needed for RCA, and remove the safest old files by age or pattern. If deployments are blocked, I would free enough inodes to restore critical service first, then run controlled cleanup.

The permanent fix would be retention and file-count control, not just larger disks. I would move high-cardinality small files to object storage or a managed store where appropriate, add inode usage alerts, enforce cleanup timers, and review filesystem layout for workloads that create millions of tiny files. Deployment and logging pipelines should fail early when inode thresholds are unsafe.

------------------------------------------------------------------------

## Q17 \| Log Growth / Disk Saturation \| P1 \| SRE

**Interview Question:**

A noisy application log grows rapidly and fills the root partition during a production incident. How would you recover the node without losing critical forensic evidence, reduce immediate logging pressure, and create a long-term log retention and alerting strategy?

**Candidate Answer:**

I would first preserve enough forensic evidence before reclaiming space. For a noisy log filling root, I would capture the failure window, top log lines or samples, current log level, process PID, and lsof output. Then I would reduce immediate pressure by lowering log verbosity if supported, rate-limiting the source, rotating or truncating the active log safely, or draining the node if writes are failing.

I would be careful not to delete the only evidence for a production incident. If the application keeps the file descriptor open, I would either signal it to reopen logs or use a safe truncate after copying the relevant section. I would also check whether journald or container stdout is duplicating logs into the root partition.

Long term, I would move logs off root or set partition quotas, configure logrotate or journald limits with tested reopen behavior, centralize logs to Loki/ELK or cloud logging, and alert on log growth rate before disk full. RCA would ask why the application produced noisy logs: debug mode, retry storm, exception loop, or dependency failure. Prevention includes log sampling, structured error budgets, and release gates for dangerous debug logging.

------------------------------------------------------------------------

## Q18 \| Log Rotation Failure \| P2 \| Production Troubleshooting

**Interview Question:**

Log rotation is configured but production logs are not rotating, and the application keeps writing to old files after rotation attempts. How would you debug ownership, copy-truncate versus reopen behavior, compression timing, and service signal handling?

**Candidate Answer:**

I would debug logrotate as both a configuration issue and an application file-handle issue. First I would run logrotate in debug mode, inspect /etc/logrotate.conf and the relevant file under /etc/logrotate.d, and check state files, permissions, ownership, missingok, size/time criteria, and whether the path matches the actual log. I would also validate that cron or systemd timer for logrotate is running.

If rotation occurs but the application keeps writing to the old file, I would check open descriptors with lsof. Some services need a HUP/USR1 signal or service reload to reopen logs; others require copytruncate when they cannot reopen. I would choose carefully because copytruncate can lose a small amount of log data during active writes, while postrotate signaling is cleaner if the app supports it.

I would also check compression timing because compressing the active or just-rotated file too early can conflict with readers or shippers. Permanent fix would be a tested logrotate stanza, correct postrotate command, service ownership alignment, log shipping validation, and monitoring for active file size and deleted-open logs after rotation.

------------------------------------------------------------------------

## Q19 \| Memory Leak / OOM \| P1 \| Production Troubleshooting

**Interview Question:**

A service is repeatedly killed under memory pressure after a release, but developers claim total server memory is sufficient. How would you confirm OOM behavior, identify the responsible process or cgroup, separate leak from workload growth, and choose mitigation?

**Candidate Answer:**

I would first prove whether the kernel or cgroup killed the service. I would inspect journalctl -k, dmesg, systemd status, kubelet/container logs if applicable, and OOM killer messages showing victim PID, score, memory usage, and cgroup. Developers saying total server memory is sufficient is not enough, because the process may be constrained by systemd MemoryMax, container limits, or a noisy neighbor.

Then I would separate leak from real workload growth. I would trend RSS, heap, open connections, request volume, cache size, garbage collection behavior, and memory after restarts. If memory grows monotonically per request or over time without returning, I would suspect a leak. If it correlates with traffic, batch size, or cache warmup, I would review sizing and limits.

Mitigation depends on risk: rollback the release if strongly correlated, restart with traffic draining to recover memory, scale out, reduce concurrency, disable a feature, or increase limits only if node headroom exists. Long term, I would add memory SLO alerts, heap profiling, canary memory comparison, systemd/container limits, and release gates that detect abnormal memory growth before full rollout.

------------------------------------------------------------------------

## Q20 \| Page Cache vs Memory Leak \| P2 \| Production Troubleshooting

**Interview Question:**

A monitoring dashboard shows very low free memory on Linux and leadership wants emergency scaling. How would you explain and validate the difference between usable page cache, real memory pressure, swap activity, and application memory growth?

**Candidate Answer:**

I would explain that Linux intentionally uses free memory for page cache, so low free memory alone is not an incident. The signal I trust more is available memory, swap in/out, major page faults, PSI memory pressure, OOM events, and process RSS growth. I would show free -h and emphasize the available column rather than only free.

To validate, I would compare buff/cache, MemAvailable, vmstat si/so, sar -B, and top processes by RSS. If page cache is high but swap is inactive, latency is normal, and MemAvailable is healthy, the system is likely using memory efficiently. If available memory is falling, swap is active, major faults are increasing, or OOM messages appear, then it is real pressure.

I would avoid emergency scaling only because a dashboard says free memory is low. Instead I would fix the dashboard to show available memory, cgroup memory, swap activity, and application RSS trends. If application memory is growing, I would investigate leak or cache configuration. This prevents unnecessary cost increases and avoids panic scaling based on a misleading Linux memory metric.

------------------------------------------------------------------------

## Q21 \| Swap Thrashing \| P1 \| Production Troubleshooting

**Interview Question:**

Users report severe latency while CPU usage remains moderate, and the Linux node shows heavy swap activity. How would you diagnose swap thrashing, identify the workload causing memory pressure, and decide between process restart, scaling, tuning, or node replacement?

**Candidate Answer:**

I would treat heavy swap as a latency incident even if CPU is moderate, because the service may be waiting on memory pages rather than compute. I would confirm swap thrashing with vmstat 1 showing si/so activity, sar -W, major page faults, PSI memory pressure, and high disk IO associated with swap. Then I would identify the responsible workload using ps sorted by RSS, smem if available, cgroup memory stats, and recent traffic or release changes.

For immediate mitigation, I would reduce customer impact first: drain the node, restart the leaking process gracefully, reduce concurrency, or scale out if the application is healthy but undersized. I would not simply disable swap during an incident because that may force OOM kills. If the node is already unhealthy, replacement can be safer than trying to tune live.

The right permanent fix depends on evidence. For a leak, rollback and profile. For workload growth, tune memory limits, cache size, and capacity. For noisy co-tenancy, isolate services. I would alert on swap-in rate and memory pressure, not only percentage swap used, because active swap movement is what hurts latency.

------------------------------------------------------------------------

## Q22 \| File Descriptor Exhaustion \| P1 \| Production Troubleshooting

**Interview Question:**

A high-traffic service starts failing with too many open files only during peak hours. How would you determine whether the issue is OS limits, systemd limits, application connection leaks, upstream retry behavior, or missing connection pooling?

**Candidate Answer:**

I would investigate file descriptor exhaustion at three layers: kernel, service manager, and application behavior. First I would confirm the error from logs and inspect /proc/\<pid\>/limits, systemctl show \<unit\> -p LimitNOFILE, ulimit from the runtime environment, and system-wide fs.file-max/file-nr. A service can fail under systemd even when a manual shell has higher limits.

Then I would count descriptors per process using ls /proc/\<pid\>/fd or lsof and classify them: sockets, files, pipes, epoll, deleted logs. If sockets dominate, I would check states such as CLOSE_WAIT, ESTABLISHED, and TIME_WAIT, connection pool limits, upstream retry behavior, keepalive settings, and whether clients are leaking connections. If files dominate, I would look for unclosed log, temp, or artifact handles.

Mitigation could be increasing the systemd limit only to a justified value, restarting the leaking process after draining, or reducing traffic/retry pressure. The permanent fix is application-side close handling, connection pooling, backpressure, fd usage dashboards, peak-hour load testing, and pre-alerts before limits are hit. Raising limits without fixing leaks only delays the outage.

------------------------------------------------------------------------

## Q23 \| ulimit / Process Limits \| P1 \| Implementation

**Interview Question:**

A production service works when launched manually but fails under systemd because it hits process or file descriptor limits. How would you identify the effective runtime limits and implement the change consistently without weakening the host security posture?

**Candidate Answer:**

I would first prove the effective limits of the service, because shell ulimit is often irrelevant for systemd-managed processes. I would inspect /proc/\<pid\>/limits, systemctl show, and the unit or drop-in configuration. Then I would set a bounded service-specific limit instead of changing global limits to unlimited.

Snippet:

svc=prod-api.service

systemctl show "\$svc" -p LimitNOFILE -p TasksMax

pid=\$(systemctl show "\$svc" -p MainPID --value)

cat /proc/\$pid/limits \| egrep 'open files\|max processes'

sudo systemctl edit "\$svc"

\# Drop-in content:

\# \[Service\]

\# LimitNOFILE=65535

\# TasksMax=4096

\# Restart=on-failure

sudo systemctl daemon-reload

sudo systemctl restart "\$svc"

pid=\$(systemctl show "\$svc" -p MainPID --value)

cat /proc/\$pid/limits \| egrep 'open files\|max processes'

I would rollout through canary nodes, validate fd usage under load, and keep the value tied to capacity planning. I would also retain least privilege through systemd hardening like User, Group, NoNewPrivileges, CapabilityBoundingSet, and ProtectSystem where applicable. The limit change should be documented with alert thresholds, not treated as a hidden emergency tweak.

------------------------------------------------------------------------

## Q24 \| High IO Wait / Disk Latency \| P1 \| Production Troubleshooting

**Interview Question:**

A database-adjacent Linux node shows high IO wait and application latency, but CPU and memory look normal. How would you identify whether the bottleneck is disk saturation, filesystem contention, journaling, noisy logs, backup jobs, or underlying storage degradation?

**Candidate Answer:**

I would focus on disk service time, queueing, and the process causing IO, because CPU and memory can look normal while requests wait on storage. I would check iostat -xz 1 for await, svctm if available, utilization, queue size, and read/write patterns. Then I would use pidstat -d, iotop, lsof, and application metrics to identify whether database writes, logging, backup, compaction, journaling, or a batch job is driving latency.

I would also check filesystem and storage health: dmesg for ext4/xfs errors, cloud volume burst credits or throttling, RAID or disk errors, mount options, inode usage, and whether logs or temp files share the same device as database-adjacent workloads. If backup or log compression is the culprit, I would throttle or pause it. If underlying storage is degraded, I would fail over or move traffic instead of tuning the app.

For recovery, I would reduce write pressure, isolate noisy workloads, scale storage IOPS, or drain the node depending on evidence. Permanent prevention includes separate volumes for logs/data, IO budgets for maintenance jobs, storage latency alerts, and capacity planning based on P95/P99 disk latency, not only disk fullness.

------------------------------------------------------------------------

## Q25 \| CI/CD Runner Disk Latency \| P2 \| Platform Engineering

**Interview Question:**

Self-hosted Linux CI runners become slow and flaky under parallel builds, with intermittent checkout and artifact failures. How would you investigate IO latency, workspace cleanup, cache strategy, inode usage, concurrent job limits, and runner isolation?

**Candidate Answer:**

I would investigate runners as production infrastructure, because flaky CI causes release risk. First I would collect host metrics during parallel builds: iostat, vmstat, df -h, df -i, docker system df, workspace size, container/image counts, and per-job duration. Checkout and artifact failures often come from disk latency, inode exhaustion, stale workspaces, or concurrent cleanup fighting active jobs.

I would validate runner lifecycle hygiene: whether each job starts from a clean workspace, whether Docker layers are pruned safely, whether caches are shared with proper locking, and whether failed jobs leave containers, mounts, or lock files. I would also check max parallel jobs versus CPU, memory, disk IOPS, and network throughput.

Immediate mitigation would be reducing concurrency, draining unhealthy runners, cleaning stale workspaces, and moving critical builds to isolated runners. Long term, I would prefer ephemeral runners or immutable golden images, remote caches with lock discipline, scheduled cleanup with safety checks, separate disks for workspace and Docker data, inode/disk latency alerts, and preflight health checks before accepting jobs. The goal is fewer random failures, predictable build time, and reproducible build isolation.

------------------------------------------------------------------------

## Q26 \| Package / Dependency Issues \| P1 \| Production Troubleshooting

**Interview Question:**

A security patch updated a shared library and several services fail at startup on only part of the fleet. How would you identify dependency mismatch, package version drift, dynamic linking issues, and plan a safe rollback or forward fix?

**Candidate Answer:**

I would first stop further rollout and compare working versus failing nodes. I would capture package versions, shared library versions, service logs, and dynamic linker output using ldd on the failing binary. I would also check ldconfig cache, LD_LIBRARY_PATH differences, symbol errors in journal logs, and whether only rebooted nodes picked up the new library.

Version drift is common in partial patch rollouts, so I would build a matrix of package versions across the fleet and correlate it with service failures. If the patch changed ABI compatibility, I would confirm whether the service binary was built against an older library or whether a dependency package is missing. I would avoid manually copying libraries because that creates untracked drift.

For mitigation, I would either rollback the package on affected nodes using the package manager or roll forward by rebuilding/redeploying services against the patched dependency, depending on security severity and compatibility. I would canary the fix, validate startup and smoke tests, then resume rollout. Prevention would include package pinning, SBOM awareness, pre-prod canary nodes, automated version drift detection, and service startup tests after patching.

------------------------------------------------------------------------

## Q27 \| Patch Management / Rollback \| P1 \| SRE

**Interview Question:**

A Linux patch rollout causes intermittent production failures across multiple nodes after reboot. How would you stop blast radius, identify the faulty package or kernel change, coordinate rollback, and improve patch canary strategy?

**Candidate Answer:**

I would immediately pause the patch rollout and remove impacted nodes from customer traffic or workload scheduling. The first objective is blast-radius control: know exactly which patch ring, kernel, package set, and reboot window are affected. I would compare healthy and unhealthy nodes using package history, kernel version, boot ID, dmesg, journal logs, service failures, and infrastructure change records.

Once a suspect package or kernel is identified, I would decide rollback versus forward fix based on severity. For kernel regressions, selecting the previous known-good kernel and rebooting can be safer. For a user-space package, package-manager downgrade or a fixed version may be enough. I would coordinate with application owners because rollback may require service restarts and traffic drains.

The recovery plan would be canary first, then expand only after service health, error rate, and node metrics are normal. Post-incident, I would redesign patching into rings: dev, non-critical prod, small prod canary, then fleet. I would require automated post-reboot validation, package diff reports, maintenance-window communication, rollback runbooks, and alerts on kernel/service regressions. Patching is reliability work, not just compliance execution.

------------------------------------------------------------------------

## Q28 \| Kernel Panic / Boot Failure \| P2 \| Production Troubleshooting

**Interview Question:**

After a kernel update, a production node fails to boot cleanly and enters a panic or emergency mode. How would you recover service capacity, preserve evidence, select a previous kernel safely, and define preventive controls for future kernel rollouts?

**Candidate Answer:**

I would first restore service capacity by ensuring workloads are running elsewhere or replacing the failed node from a known-good image. I would not spend the first minutes debugging a single dead node while capacity is below safe limits. Once impact is controlled, I would preserve console logs, boot messages, and any cloud or hypervisor events.

For recovery of the node, I would use serial console, rescue mode, or GRUB to boot the previous known-good kernel. If the system entered emergency mode due to filesystem or mount issues, I would inspect journalctl -xb, fstab, initramfs, disk UUIDs, and run fsck only when the filesystem is unmounted and the recovery plan is approved. I would avoid blind package changes that destroy evidence.

After booting, I would pin or hold the bad kernel, validate application health, and decide whether to rebuild the node instead of trusting a repaired host. Preventive controls include kernel canary rollout, automatic previous-kernel fallback where supported, snapshots before patching, tested rescue procedures, node replacement automation, and alerts when a host fails post-reboot validation.

------------------------------------------------------------------------

## Q29 \| Unexpected Reboot Investigation \| P1 \| SRE

**Interview Question:**

A Linux production node unexpectedly rebooted during business hours, and the application team suspects infrastructure instability. How would you build the incident timeline using system, kernel, hardware, cloud, and application evidence to prove the most likely cause?

**Candidate Answer:**

I would build a timeline from multiple independent sources instead of accepting the first explanation. I would start with last -x, uptime, journalctl --list-boots, journalctl -b -1, kernel logs, and shutdown/reboot messages. Then I would correlate cloud provider events, hypervisor maintenance, instance health checks, BMC/IPMI hardware logs, audit logs, and any automation that may have issued reboot.

I would specifically look for OOM killer activity, kernel panic, watchdog reset, power loss, manual sudo reboot, patch automation, disk errors, thermal or hardware events, and application-level crashes before the reboot. Time synchronization matters, so I would verify clock drift before aligning events across systems.

The incident output should be evidence-based: what happened before reboot, what initiated it, what symptoms were consequences, and what remains unknown. If infrastructure instability is likely, I would open a provider/hardware case with logs. If automation caused it, I would fix change controls. If kernel panic occurred, I would analyze crash dumps if enabled. Prevention would include kdump, auditd, centralized logs, reboot alerts, maintenance tagging, and post-reboot health validation.

------------------------------------------------------------------------

## Q30 \| Time Sync / NTP \| P2 \| Production Troubleshooting

**Interview Question:**

A fleet has clock drift causing TLS errors, inconsistent logs, failed token validation, and confusing incident timelines. How would you detect time synchronization failure, mitigate business impact, and harden NTP or chrony operations?

**Candidate Answer:**

I would treat time drift as a security and observability incident because it can break TLS validation, signed tokens, distributed tracing, and log timelines. First I would quantify offset using timedatectl, chronyc tracking, chronyc sources -v, and compare against a trusted reference. I would check whether chrony/ntpd is running, blocked by firewall, pointed to invalid servers, or unable to step time due to policy.

For mitigation, I would remove severely drifted nodes from sensitive traffic if token or TLS validation is failing, then restore synchronization using approved NTP sources. I would be careful with large time jumps on database or queue nodes because abrupt clock changes can affect leases, certificates, and ordering. Application teams may need temporary token leeway only as a controlled mitigation, not a permanent weakening.

Hardening would include multiple reliable NTP sources, monitoring of offset and sync state, alerts before drift becomes business-impacting, firewall rules for NTP, standard chrony config through configuration management, and startup checks for services that require accurate time. RCA would also fix log correlation by normalizing timestamps and documenting the actual drift window.

------------------------------------------------------------------------

## Q31 \| SSH Access Failure \| P1 \| Security

**Interview Question:**

Engineers lose SSH access to a production Linux node after a hardening change, but the service is still serving traffic. How would you diagnose authentication, authorization, host key, PAM, firewall, disk, and shell issues without increasing outage risk?

**Candidate Answer:**

I would avoid making the outage worse because the service is still serving traffic. First I would preserve existing sessions and use out-of-band access such as cloud serial console, SSM, rescue console, or break-glass path rather than rebooting. Then I would inspect sshd logs, auth logs, PAM messages, sshd_config changes, AllowUsers/AllowGroups, authorized_keys permissions, host key changes, shell validity, account lockout, sudo/group drift, disk-full conditions, and firewall rules.

I would distinguish authentication failure from authorization failure. Authentication issues include key mismatch, expired certificates, wrong host key, or disabled password/key methods. Authorization issues include user not allowed by sshd_config, PAM, locked account, missing shell, or home directory permissions.

The mitigation should be the smallest reversible fix: restore the last known-good sshd config, correct key or permission ownership, reopen the firewall only from approved bastion ranges, or enable a time-bound break-glass user if policy allows. I would validate with a separate session before closing the current one. Prevention includes staged hardening rollout, sshd -t validation, canary nodes, out-of-band access testing, and audited emergency access.

------------------------------------------------------------------------

## Q32 \| journald / Syslog Analysis \| P1 \| Production Troubleshooting

**Interview Question:**

During a P1 incident, application logs are incomplete and system logs appear to have gaps around the failure window. How would you investigate journald retention, rate limiting, log forwarding, disk pressure, time drift, and application logging behavior?

**Candidate Answer:**

I would treat log gaps as an incident-quality problem because RCA depends on reliable timelines. First I would verify whether the logs were never written, dropped locally, rotated away, rate-limited, or failed to forward. For journald, I would check journalctl --verify, journal disk usage, Storage=persistent, SystemMaxUse, MaxRetentionSec, RateLimitIntervalSec, and RateLimitBurst. I would inspect messages indicating suppression or vacuuming.

I would also check disk pressure around the failure window, because full filesystems cause both application logging and log forwarding failures. Time drift can make logs appear missing, so I would validate chrony/NTP state and compare boot IDs and timestamps. For syslog or log shippers, I would check rsyslog/fluent-bit/vector status, queue backlog, permissions, and network errors to the central logging backend.

If application logs are incomplete, I would validate whether the app logs to stdout, files, journald, or an async logger that may drop messages under load. Permanent fixes include durable journald retention, central log buffering, alerting on log-drop/rate-limit events, disk safeguards, trace IDs across app and system logs, and an incident runbook for reconstructing timelines from multiple sources.

------------------------------------------------------------------------

## Q33 \| SELinux / AppArmor Restrictions \| P2 \| Security

**Interview Question:**

A service starts successfully after deployment but cannot read a required file or bind to an expected resource only on hardened Linux nodes. How would you differentiate standard UNIX permissions from SELinux or AppArmor enforcement and avoid disabling security globally?

**Candidate Answer:**

I would not disable SELinux or AppArmor globally just because the service works on non-hardened nodes. First I would prove whether standard UNIX permissions are sufficient by checking owner, group, mode, ACLs, and parent directory execute bits. If those look correct, I would inspect MAC enforcement: getenforce and ausearch/audit.log for SELinux, or aa-status and kernel audit messages for AppArmor.

For SELinux, I would check file context with ls -Z and compare it to the expected service domain. Many issues are solved by restoring correct context with restorecon or adding a narrowly scoped policy rule, not by changing file modes. For AppArmor, I would identify the profile denial and adjust the profile to allow the exact file or capability required.

During mitigation, a temporary permissive mode for one domain may be acceptable only to confirm diagnosis and only with approval, but the permanent fix must preserve hardening. I would canary the policy change, keep audit evidence, and add deployment checks that validate labels/profiles before rollout. This keeps production secure while making the application operable.

------------------------------------------------------------------------

## Q34 \| Privileged Ports / Linux Capabilities \| P2 \| Security

**Interview Question:**

A non-root service must bind to a privileged port in production, and the team proposes running it as root. How would you solve this using least-privilege Linux capabilities or fronting patterns while preserving auditability and rollback safety?

**Candidate Answer:**

I would avoid running the full service as root only to bind to port 80 or 443. The least-privilege option is to grant only the bind capability or use a fronting component. If the binary is stable and controlled, setcap cap_net_bind_service=+ep on the executable can allow binding to privileged ports. With systemd, I prefer AmbientCapabilities=CAP_NET_BIND_SERVICE and CapabilityBoundingSet=CAP_NET_BIND_SERVICE because it is visible in the unit and easier to audit.

I would also consider fronting the service with NGINX, Envoy, HAProxy, or a load balancer listening on the privileged port and forwarding to an unprivileged local port. This is often cleaner when TLS termination, rate limiting, or routing is needed.

For safety, I would test on a canary, document rollback by removing the capability or reverting the unit drop-in, and ensure NoNewPrivileges, User/Group, and filesystem protections remain in place. I would monitor bind failures and audit changes to the binary, because replacing the binary can remove file capabilities. The decision balances simplicity, security, observability, and operational ownership.

------------------------------------------------------------------------

## Q35 \| Filesystem Mount Options \| P2 \| Production Troubleshooting

**Interview Question:**

A build or deployment script fails on production Linux nodes because temporary files cannot execute, while the same workflow works in lower environments. How would you investigate mount options, temp directory policies, security hardening, and safer pipeline design?

**Candidate Answer:**

I would suspect noexec or other hardened mount options when scripts or temporary binaries fail only on production nodes. First I would identify the exact path used for temporary execution and inspect findmnt, mount, and /proc/mounts for noexec, nosuid, nodev, read-only, or restricted tmp policies. I would also check TMPDIR, build tool defaults, and whether the script is extracting binaries into /tmp.

I would not remove noexec from /tmp as a quick fix unless there is a formal risk review, because that hardening reduces malware and privilege-escalation risk. The safer fix is to change the workflow so executable artifacts run from an approved deployment or workspace path mounted with exec and controlled ownership. For CI/CD, I would set TMPDIR to a dedicated runner workspace if policy allows, or adjust tooling to avoid executing from temp.

I would validate the fix with the same runtime user and mount context as production. Long term, environment parity matters: lower environments should use the same mount hardening so this failure is found before production. I would document temp execution policy in the platform runbook.

------------------------------------------------------------------------

## Q36 \| Read-Only Filesystem \| P1 \| Production Troubleshooting

**Interview Question:**

A production Linux node suddenly reports a read-only filesystem and services start failing to write logs and state. How would you determine whether this is disk corruption, kernel protection behavior, storage backend failure, or filesystem exhaustion, and what recovery path would you choose?

**Candidate Answer:**

I would assume the kernel remounted the filesystem read-only to protect data until proven otherwise. First I would check dmesg, journalctl -k, mount output, df -h, df -i, smart or cloud disk health, and filesystem-specific errors such as ext4 or xfs metadata corruption. I would also check whether the underlying storage path disappeared or returned IO errors.

The recovery path depends on the role of the node. If it is in a pool, I would drain or remove it from traffic and replace it rather than trying risky live repair. If it holds unique state, I would stop writes, snapshot if possible, preserve logs, and plan fsck/xfs_repair from rescue mode or after unmounting. I would not blindly remount rw because that can worsen corruption.

If the root cause is disk full or inode exhaustion, freeing space may help but I would still verify filesystem health. If backend storage failed, I would escalate to the provider/storage team and fail over. Prevention includes disk health alerts, filesystem error alerts, separate state/log volumes, tested backup/restore, and immutable node replacement for stateless workloads.

------------------------------------------------------------------------

## Q37 \| Kubernetes Node Linux Pressure \| P1 \| Platform Engineering

**Interview Question:**

A Kubernetes node becomes NotReady and multiple pods are evicted, but the root cause appears to be Linux-level disk, memory, or PID pressure. How would you debug the node from the operating system layer and prevent the same failure across the cluster?

**Candidate Answer:**

I would debug from both Kubernetes and Linux because kubelet only reports symptoms like DiskPressure, MemoryPressure, PIDPressure, or NotReady. First I would cordon the node to stop new scheduling and decide whether to drain based on workload disruption budgets. Then I would inspect kubectl describe node, node events, kubelet logs, container runtime logs, df -h, df -i, free, vmstat, pid usage, and /var/lib/kubelet plus container image storage.

For disk pressure, I would check image layers, container logs, emptyDir usage, journal growth, and deleted open files. For memory pressure, I would compare pod cgroup usage with host memory, OOM events, and eviction logs. For PID pressure, I would look for fork bombs, zombie jobs, or sidecars spawning too many processes.

Immediate mitigation may be image/log cleanup, draining the node, restarting kubelet/containerd carefully, or replacing the node. Prevention belongs at platform level: kubeReserved/systemReserved, eviction thresholds, log limits, image garbage collection tuning, pod resource limits, PID limits, node health alerts, and auto-repair. RCA should identify which workload or host policy allowed node-level pressure to impact multiple pods.

------------------------------------------------------------------------

## Q38 \| kubelet / container runtime Host Issues \| P1 \| Platform Engineering

**Interview Question:**

A Linux worker node shows kubelet and container runtime instability after a host reboot, while application teams only see pod failures. How would you isolate host service failures, cgroup driver mismatch, disk pressure, certificate timing, and runtime socket issues?

**Candidate Answer:**

I would first translate application pod failures into host service health. On the node I would inspect systemctl status kubelet and containerd, journalctl -u kubelet, journalctl -u containerd, crictl info, crictl ps, and the runtime socket configured in kubelet. After a reboot, common failures include container runtime not starting, cgroup driver mismatch, disk pressure under /var/lib/containerd or /var/lib/kubelet, expired or not-yet-valid certificates due to time drift, and network plugin not ready.

I would cordon the node before making changes, then check kubelet configuration, cgroup driver alignment with the runtime, node certificates, chrony state, mount availability, and whether the CNI directories and binaries are intact. I would avoid restarting services repeatedly without reading logs because that can hide the first failure.

Mitigation could be freeing disk, restarting containerd then kubelet in order, fixing time sync, or replacing the node if host state is corrupted. Permanent controls include reboot canaries, node boot validation, kubelet/runtime health alerts, immutable node images, certificate expiry monitoring, and auto-repair that replaces bad nodes instead of leaving app teams to debug pod-level symptoms.

------------------------------------------------------------------------

## Q39 \| Ephemeral Runner Reliability \| P2 \| Platform Engineering

**Interview Question:**

Ephemeral Linux runners randomly fail builds with checkout, lock file, container startup, and cleanup errors. How would you investigate runner lifecycle hygiene, stuck processes, workspace isolation, disk cleanup, zombie jobs, and host resource contention?

**Candidate Answer:**

I would treat ephemeral runner failures as lifecycle hygiene problems until proven otherwise. Random checkout, lock file, container startup, and cleanup errors often mean the runner is not truly clean between jobs or the host is oversubscribed. I would collect evidence from runner service logs, job workspaces, process lists, Docker/container runtime state, mount points, disk and inode usage, and build concurrency at failure time.

I would specifically look for stale Git locks, leftover containers, orphaned mounts, zombie child processes, shared cache corruption, long-running cleanup racing with new jobs, and exhausted disk IO. For ephemeral runners, I would validate that registration, job execution, cleanup, and deregistration are atomic, and that failed jobs still trigger cleanup.

Immediate mitigation would be draining bad runners, reducing parallelism, cleaning workspaces and Docker state, or rebuilding from a golden image. Long term, I would make runners cattle: immutable image, one job per runner where feasible, preflight health checks, post-job cleanup verification, remote cache instead of unsafe shared local cache, resource quotas, and alerts on zombie processes, disk, inode, and container count. Reliability of CI runners directly affects release confidence.

------------------------------------------------------------------------

## Q40 \| Context Switching / Kernel Scheduling \| P3 \| Production Troubleshooting

**Interview Question:**

A latency-sensitive service shows degraded response time even though CPU usage is below capacity, and low-level metrics indicate excessive context switching. How would you investigate thread count, scheduler pressure, lock contention, interrupts, and runtime configuration?

**Candidate Answer:**

I would investigate latency below the CPU-utilization headline. Excessive context switching can mean too many runnable threads, lock contention, aggressive thread pools, interrupt storms, or runtime scheduling inefficiency. I would inspect vmstat for cs/in, pidstat -w for per-process voluntary and involuntary context switches, top -H for thread count, and perf or runtime-specific profiling to see where threads are blocking.

I would correlate with application metrics: request latency, queue depth, thread pool saturation, GC pauses, connection pool waits, and lock contention. If one process has thousands of threads, I would check thread pool configuration and whether retries or blocking IO are creating thread explosion. If interrupts are high, I would inspect NIC interrupts, softirq, IRQ balancing, and packet rate. If futex waits dominate, I would suspect application locks.

Mitigation depends on the source: reduce concurrency, tune thread pools, fix retry storms, enable async/non-blocking patterns, isolate noisy workloads, or tune IRQ affinity. I would validate using P95/P99 latency and context-switch rates. Permanent prevention includes runtime dashboards for threads, run queue, context switches, and load tests that catch scheduler pressure before production.

------------------------------------------------------------------------

## Q41 \| CPU Steal / Virtualized Linux \| P3 \| Cost Optimization

**Interview Question:**

A cloud Linux VM shows periodic latency spikes with moderate process CPU usage, and the issue appears correlated with CPU steal or host contention. How would you validate the finding and decide between rightsizing, placement changes, autoscaling, or instance family migration?

**Candidate Answer:**

I would validate CPU steal before changing architecture, because it indicates the VM wanted CPU time but the hypervisor scheduled another tenant or host activity. I would check top %st, mpstat, sar -u, cloud monitoring CPU steal metrics, and compare latency spikes with steal time across affected instances. I would also confirm it is not application CPU, GC, IO wait, or network latency causing the same symptom.

If steal spikes only on a few VMs, I would move workloads by stopping/starting or replacing instances, depending on cloud behavior. If the workload is latency-sensitive, I would evaluate larger instance sizes, compute-optimized families, dedicated hosts, placement groups, or autoscaling to reduce per-node pressure. If it is a batch workload, accepting some steal may be cheaper than overpaying for dedicated capacity.

The decision should balance cost and SLO. For customer-facing low-latency services, I would favor stable compute and autoscaling headroom. For non-critical jobs, I would use cheaper shared capacity with retry tolerance. I would add alerts on sustained steal percentage correlated with latency, and include host contention as a known failure mode in capacity planning.

------------------------------------------------------------------------

## Q42 \| Resource Saturation / Capacity Planning \| P1 \| SRE

**Interview Question:**

A Linux service repeatedly degrades every Monday morning due to combined CPU, memory, disk IO, and connection pressure. How would you build a capacity analysis, define SLO-linked alerts, and convert repeated firefighting into a proactive scaling plan?

**Candidate Answer:**

I would convert the Monday degradation into a capacity model rather than another weekly firefight. First I would collect several weeks of data for CPU run queue, memory pressure, swap, disk IO latency, file descriptors, connection counts, request rate, queue depth, dependency latency, and P95/P99 customer latency. I would align these with business events, batch schedules, deployments, and traffic patterns.

Then I would identify which resource becomes limiting first and whether the resources amplify each other. For example, connection pressure may increase memory, which triggers swap, which increases IO wait, which increases latency and retries. I would define SLO-linked alerts that fire on symptoms users feel, plus leading indicators like fd usage, memory pressure, disk await, and queue depth.

The proactive plan would include capacity headroom targets, Monday pre-scaling, autoscaling rules, concurrency limits, job rescheduling, connection pooling, and load tests that reproduce peak traffic. I would also publish a dashboard showing current utilization versus safe thresholds. RCA should produce owners and due dates for capacity fixes. The objective is to make Monday a planned scaling event, not a recurring incident.

------------------------------------------------------------------------

## Q43 \| Application Logs vs System Logs \| P2 \| SRE

**Interview Question:**

An application team blames the Linux platform for an outage, while system metrics look normal and application logs show intermittent failures. How would you correlate system logs, application logs, deployment events, and business impact to produce a defensible RCA?

**Candidate Answer:**

I would keep the RCA evidence-based and avoid platform-versus-application arguments. First I would build a single timeline with deployment events, traffic changes, application error logs, system logs, node metrics, dependency metrics, and business impact. If Linux metrics show normal CPU, memory, disk IO, network errors, and no kernel/systemd failures during the failure window, that is strong evidence the host was not the primary cause.

Then I would inspect application logs for error patterns, correlation IDs, dependency timeouts, connection pool exhaustion, retries, configuration changes, or feature flags. I would compare affected and unaffected nodes to see whether the failure follows a host, a version, a tenant, traffic segment, or dependency. I would also verify log timestamps and gaps to avoid false correlation.

The final RCA should state what evidence supports and rules out each hypothesis. If the platform contributed indirectly, such as missing alerts or insufficient host-level visibility, I would own that improvement. If the application caused the outage, I would still help define prevention: better health checks, deployment canaries, structured logs, dependency dashboards, and rollback criteria. Senior ownership means producing clarity, not assigning blame.

------------------------------------------------------------------------

## Q44 \| Automation / Runbook Improvement \| P1 \| Platform Engineering

**Interview Question:**

Your team has repeated Linux incidents for disk full, file descriptor exhaustion, service restart loops, and time drift. What automation, guardrails, runbooks, alert enrichment, and self-healing actions would you implement to reduce toil and improve recovery time?

**Candidate Answer:**

I would build a Linux reliability layer around the recurring failure modes. For disk full, I would add alerts on disk percent, inode percent, growth rate, deleted-open files, and journald/log directory usage, with safe cleanup runbooks and service-specific log budgets. For file descriptor exhaustion, I would alert on fd usage versus limit, CLOSE_WAIT growth, and systemd LimitNOFILE mismatches.

For restart loops, I would enrich alerts with recent unit changes, exit code, StartLimitHit state, last journal lines, config checksum, and dependency health. For time drift, I would alert on chrony offset, sync source, and unsynchronized state before TLS or token failures start. I would add preflight checks to deployments for permissions, disk, inode, fd limits, time sync, and port availability.

Self-healing should be conservative: rotate/truncate known safe logs, restart known stateless services after evidence capture, drain unhealthy nodes, and block new workloads on bad runners. Runbooks should include decision criteria, commands, rollback paths, and validation metrics. I would track toil by incident count and MTTR, then convert repeated manual steps into tested automation with audit logs and change control.

------------------------------------------------------------------------

## Q45 \| Incident Handling / Leadership \| P1 \| Leadership

**Interview Question:**

During a Linux node outage affecting revenue traffic, how would you lead the incident bridge, assign investigation streams, communicate with stakeholders, make rollback or reboot decisions, and ensure the postmortem produces platform-level improvements?

**Candidate Answer:**

I would act as incident commander rather than the deepest debugger. First I would declare severity, confirm business impact, assign a scribe, and create investigation streams: traffic mitigation, Linux host health, recent changes, application behavior, and dependency checks. I would ask each stream for evidence and timestamps, not opinions.

My first recovery bias would be to reduce customer impact. If the node is one of many, I would remove it from load balancing or cordon/drain it. If the outage follows a release or patch, I would prepare rollback. If the node is stuck in D-state, read-only filesystem, or kubelet/runtime instability and capacity is available elsewhere, I would choose controlled replacement or reboot after evidence capture rather than waiting indefinitely.

For stakeholders, I would communicate impact, current mitigation, next update time, and decisions made. I would avoid technical noise but be transparent about risk. After recovery, I would lead a blameless postmortem with timeline, root cause, contributing factors, detection gaps, and action items. Platform-level improvements could include safer patch canaries, node auto-repair, better Linux alerts, runbook automation, and ownership for each follow-up.

------------------------------------------------------------------------

## Q46 \| Resume Deep Dive / Production Ownership \| P1 \| Resume Deep Dive

**Interview Question:**

Pick one Linux production incident you personally owned end to end. What was the business impact, exact failure mode, your debugging path, mitigation decision, RCA evidence, automation added afterward, and measurable improvement in reliability or MTTR?

**Candidate Answer:**

I would answer this with a real incident, not a fabricated story. One strong example from my experience is a Linux self-hosted CI runner incident where parallel builds started failing randomly during checkout, container startup, and cleanup. The business impact was blocked release validation and repeated engineering interruption because teams could not trust the pipeline result.

The exact failure mode was host-level resource and lifecycle hygiene degradation: disk and inode pressure, stale workspaces, leftover container/runtime state, and occasional orphaned processes after failed jobs. My debugging path was to correlate failed job timestamps with runner host metrics, inspect df/df -i, iostat, Docker storage usage, workspace locks, process trees, and runner service logs. The mitigation was to drain unhealthy runners, clean stale state safely, reduce concurrency temporarily, and reroute critical builds to healthy runners.

For RCA evidence, I would show before/after job failure rate, disk/inode trend, cleanup logs, and examples of stale locks or containers. The permanent fix was preflight runner health checks, post-job cleanup validation, disk/inode alerts, workspace isolation, and a move toward more ephemeral runner behavior. In the interview I would quote the exact MTTR or failure-rate improvement from the incident tracker; for a senior answer, I would not invent numbers. The key improvement was converting random firefighting into measurable runner reliability controls.

------------------------------------------------------------------------
