# Linux Top 200+ Production Troubleshooting Commands
**10/10 senior-level runbook pack for Linux Platform Engineers, DevOps Engineers, and SREs in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic Linux cheat sheet.

Each command includes:

- the command itself
- a concise technical explanation
- a **risk level**
- a **use when** line so responders know where it fits in incident flow

## Command Risk Levels

- **Low risk** — read-only or generally safe diagnostic action
- **Medium risk** — controlled observation or scoped execution that may add minor load or change execution context
- **High risk** — can mutate runtime state, permissions, services, logging retention, or host behavior
- **Emergency-only** — use only in tightly controlled recovery situations with explicit confirmation

## Fast “First 5 Commands” by Incident Type

### High load or CPU saturation
1. `uptime`
2. `top`
3. `mpstat -P ALL 1 5`
4. `ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head`
5. `pidstat 1 5`

### Memory exhaustion or OOM
1. `free -h`
2. `vmstat 1 5`
3. `journalctl -k | grep -i oom`
4. `ps -eo pid,ppid,cmd,rss,vsz,%mem --sort=-rss | head`
5. `sar -S 1 5`

### Disk full or inode exhaustion
1. `df -h`
2. `df -i`
3. `du -xhd1 / | sort -h`
4. `lsof +L1`
5. `find / -xdev -type f -size +1G -ls | head -50`

### Slow disk or storage latency
1. `iostat -xz 1 5`
2. `iotop -oPa`
3. `pidstat -d 1 5`
4. `lsblk -f`
5. `smartctl -a /dev/<device>`

### Service will not start or keeps failing
1. `systemctl status <service>`
2. `journalctl -u <service> -n 100 --no-pager`
3. `systemctl cat <service>`
4. `systemctl list-dependencies <service>`
5. `journalctl -b -p err`

### Port open issue or app unreachable
1. `ss -lntp`
2. `ip addr`
3. `ip route`
4. `iptables -L -n -v`
5. `tcpdump -ni any port <port>`

### DNS or name resolution failure
1. `cat /etc/resolv.conf`
2. `getent hosts <name>`
3. `dig <name>`
4. `dig @<dns_server> <name>`
5. `tcpdump -ni any port 53`

### Permission denied or identity mismatch
1. `stat <path>`
2. `namei -l <path>`
3. `id <user>`
4. `getfacl <path>`
5. `restorecon -Rv /path`

### SELinux or AppArmor denial
1. `getenforce`
2. `sestatus`
3. `ausearch -m AVC,USER_AVC -ts recent`
4. `sealert -a /var/log/audit/audit.log`
5. `apparmor_status`

### Package manager lock or package drift
1. `rpm -V <package>`
2. `dnf history`
3. `apt-cache policy <package>`
4. `fuser /var/lib/dpkg/lock-frontend`
5. `lsof /var/lib/rpm/.rpm.lock`

### Boot issue after restart
1. `who -b`
2. `journalctl -b -p err`
3. `journalctl -b -1 -p err`
4. `systemd-analyze critical-chain`
5. `efibootmgr -v`

## Recommended Troubleshooting Flow

1. confirm host identity, boot context, and incident timing first
2. determine whether the problem is CPU, memory, IO, network, filesystem, process, or policy related
3. prefer read-only observation before mutation
4. collect evidence before restart, cleanup, reload, or kill actions
5. correlate kernel, service, audit, and application-level timelines
6. use rescue, sysctl, ACL, SELinux relabel, and service restart actions deliberately

---

# System Identity, Version, and Boot Context


## 1. `uname -a`


```bash

uname -a

```

**Risk:** Low risk

**Technical explanation:** Prints kernel, architecture, and hostname details so you can baseline the OS and kernel quickly.

**Use when:** Use when you need to confirm basic host identity and runtime kernel context.


## 2. `hostnamectl`


```bash

hostnamectl

```

**Risk:** Low risk

**Technical explanation:** Shows static hostname, OS, kernel, virtualization, and architecture in one high-signal summary.

**Use when:** Use when host identity or virtualization context may explain behavior.


## 3. `cat /etc/os-release`


```bash

cat /etc/os-release

```

**Risk:** Low risk

**Technical explanation:** Displays distribution identity and version fields, which is essential when package or service behavior is distro-specific.

**Use when:** Use when commands, packages, or service behavior may vary by distribution.


## 4. `lsb_release -a`


```bash

lsb_release -a

```

**Risk:** Low risk

**Technical explanation:** Reports Linux distribution metadata on systems where lsb-release is installed.

**Use when:** Use when you want distro details in a standard helper format.


## 5. `uname -r`


```bash

uname -r

```

**Risk:** Low risk

**Technical explanation:** Prints the running kernel release so you can compare runtime and installed kernel versions.

**Use when:** Use when kernel-specific bugs, modules, or drivers are suspected.


## 6. `uptime`


```bash

uptime

```

**Risk:** Low risk

**Technical explanation:** Shows current time, system uptime, logged-in users, and load averages for fast health context.

**Use when:** Use early in almost any host incident for quick pressure context.


## 7. `who -b`


```bash

who -b

```

**Risk:** Low risk

**Technical explanation:** Shows the last system boot time so you can correlate incidents with recent reboots.

**Use when:** Use when instability may have started after reboot.


## 8. `last reboot | head`


```bash

last reboot | head

```

**Risk:** Low risk

**Technical explanation:** Lists recent reboot events so you can see whether instability aligns with restarts.

**Use when:** Use when reboot history may explain recurring incidents.


## 9. `systemd-analyze`


```bash

systemd-analyze

```

**Risk:** Low risk

**Technical explanation:** Reports kernel-space and user-space boot time so you can spot boot slowness at a glance.

**Use when:** Use when the host booted slowly or startup sequencing is in question.


## 10. `systemd-analyze blame | head -20`


```bash

systemd-analyze blame | head -20

```

**Risk:** Low risk

**Technical explanation:** Shows the slowest systemd units during boot so startup bottlenecks are easier to identify.

**Use when:** Use when one or more units may be delaying availability after boot.


## 11. `systemd-analyze critical-chain`


```bash

systemd-analyze critical-chain

```

**Risk:** Low risk

**Technical explanation:** Displays the dependency chain of boot-critical services so you can trace delayed startup paths.

**Use when:** Use when boot dependencies, not just one slow unit, matter.


## 12. `timedatectl`


```bash

timedatectl

```

**Risk:** Low risk

**Technical explanation:** Shows time, timezone, NTP state, and synchronization status, which matters for auth, certs, and distributed systems.

**Use when:** Use when timestamps, tokens, TLS, or cluster coordination look wrong.


## 13. `date -Ins`


```bash

date -Ins

```

**Risk:** Low risk

**Technical explanation:** Prints the current time in ISO-like format for precise incident correlation.

**Use when:** Use when aligning logs and event timelines precisely.


## 14. `arch`


```bash

arch

```

**Risk:** Low risk

**Technical explanation:** Displays the machine hardware name, which is useful when debugging architecture-specific binaries.

**Use when:** Use when binary compatibility or architecture drift is possible.


## 15. `virt-what`


```bash

virt-what

```

**Risk:** Low risk

**Technical explanation:** Detects virtualization platform where available, helping explain guest-specific behavior or hypervisor constraints.

**Use when:** Use when host behavior may depend on cloud, VM, or hypervisor platform.


# CPU, Load, Scheduling, and Performance


## 16. `lscpu`


```bash

lscpu

```

**Risk:** Low risk

**Technical explanation:** Shows CPU topology, flags, sockets, threads, and NUMA-related data for host capacity and placement analysis.

**Use when:** Use when CPU topology or NUMA layout may affect performance.


## 17. `nproc`


```bash

nproc

```

**Risk:** Low risk

**Technical explanation:** Prints the number of available processing units, which is useful in quick capacity scripts.

**Use when:** Use when validating CPU count from scripts or automation.


## 18. `uptime`


```bash

uptime

```

**Risk:** Low risk

**Technical explanation:** Shows 1, 5, and 15 minute load averages for fast scheduler pressure assessment.

**Use when:** Use when high load is suspected but root cause is still unknown.


## 19. `top`


```bash

top

```

**Risk:** Low risk

**Technical explanation:** Provides an interactive live view of CPU, memory, and process activity across the system.

**Use when:** Use when you need fast live triage on a busy host.


## 20. `htop`


```bash

htop

```

**Risk:** Low risk

**Technical explanation:** Shows an interactive process view with clearer CPU and memory breakdown when available.

**Use when:** Use when a richer process view helps during active triage.


## 21. `mpstat -P ALL 1 5`


```bash

mpstat -P ALL 1 5

```

**Risk:** Low risk

**Technical explanation:** Reports per-CPU utilization over time so you can detect imbalance or one-hot-core conditions.

**Use when:** Use when one CPU or NUMA imbalance may explain latency.


## 22. `pidstat 1 5`


```bash

pidstat 1 5

```

**Risk:** Low risk

**Technical explanation:** Shows per-process CPU and task activity over intervals, which helps identify bursty consumers.

**Use when:** Use when top-style snapshots miss intermittent CPU spikes.


## 23. `vmstat 1 5`


```bash

vmstat 1 5

```

**Risk:** Low risk

**Technical explanation:** Summarizes runnable tasks, memory, swap, block IO, and CPU waits at fixed intervals.

**Use when:** Use when you need a compact multi-signal performance view.


## 24. `sar -u 1 5`


```bash

sar -u 1 5

```

**Risk:** Low risk

**Technical explanation:** Collects CPU usage samples over time when sysstat is available.

**Use when:** Use when trend sampling is more useful than a single snapshot.


## 25. `ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head`


```bash

ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head

```

**Risk:** Low risk

**Technical explanation:** Lists top CPU consumers in a sortable non-interactive format.

**Use when:** Use when scripting or sharing top CPU offenders quickly.


## 26. `ps -eLo pid,tid,psr,pcpu,comm --sort=-pcpu | head`


```bash

ps -eLo pid,tid,psr,pcpu,comm --sort=-pcpu | head

```

**Risk:** Low risk

**Technical explanation:** Shows hot threads and the CPUs they run on, which is useful in thread-level CPU investigations.

**Use when:** Use when one multithreaded service is pegging CPU.


## 27. `top -H -p <pid>`


```bash

top -H -p <pid>

```

**Risk:** Low risk

**Technical explanation:** Displays thread-level activity for one process so you can identify the busy thread inside a service.

**Use when:** Use when per-thread hotspots matter more than whole-process averages.


## 28. `taskset -pc <pid>`


```bash

taskset -pc <pid>

```

**Risk:** Low risk

**Technical explanation:** Shows CPU affinity for a process so you can detect unintended pinning.

**Use when:** Use when performance looks constrained by CPU affinity.


## 29. `chrt -p <pid>`


```bash

chrt -p <pid>

```

**Risk:** Low risk

**Technical explanation:** Displays the scheduler policy and priority of a process, which matters in latency-sensitive troubleshooting.

**Use when:** Use when real-time or custom scheduling may be influencing behavior.


## 30. `perf stat -p <pid> sleep 10`


```bash

perf stat -p <pid> sleep 10

```

**Risk:** Medium risk

**Technical explanation:** Collects performance counters for one process over time when low-level CPU analysis is needed.

**Use when:** Use when higher-level CPU tools are insufficient and low-level measurement is justified.


# Memory, Swap, and OOM Investigation


## 31. `free -h`


```bash

free -h

```

**Risk:** Low risk

**Technical explanation:** Shows total, used, free, shared, buff/cache, and available memory in human-readable units.

**Use when:** Use as a first-pass memory pressure check.


## 32. `cat /proc/meminfo`


```bash

cat /proc/meminfo

```

**Risk:** Low risk

**Technical explanation:** Displays the full kernel memory accounting view for deep memory analysis.

**Use when:** Use when summary memory output is not enough.


## 33. `vmstat -s`


```bash

vmstat -s

```

**Risk:** Low risk

**Technical explanation:** Prints cumulative memory, swap, paging, and CPU counters for a compact summary.

**Use when:** Use when you need a compact one-shot memory and paging view.


## 34. `sar -r 1 5`


```bash

sar -r 1 5

```

**Risk:** Low risk

**Technical explanation:** Samples memory usage over time when you need trend rather than point-in-time data.

**Use when:** Use when memory pressure may be intermittent.


## 35. `sar -S 1 5`


```bash

sar -S 1 5

```

**Risk:** Low risk

**Technical explanation:** Samples swap usage and swap activity to help identify memory pressure spillover.

**Use when:** Use when swapping may be contributing to latency.


## 36. `swapon --show`


```bash

swapon --show

```

**Risk:** Low risk

**Technical explanation:** Lists active swap devices and files so you can confirm swap configuration.

**Use when:** Use when swap availability or location may matter.


## 37. `cat /proc/swaps`


```bash

cat /proc/swaps

```

**Risk:** Low risk

**Technical explanation:** Shows active swap areas from the kernel’s view.

**Use when:** Use when you want the kernel-reported swap list directly.


## 38. `grep -i oom /var/log/messages`


```bash

grep -i oom /var/log/messages

```

**Risk:** Low risk

**Technical explanation:** Searches for OOM-related events in traditional system logs where present.

**Use when:** Use when older syslog-style logs may contain OOM evidence.


## 39. `journalctl -k | grep -i oom`


```bash

journalctl -k | grep -i oom

```

**Risk:** Low risk

**Technical explanation:** Searches kernel logs for OOM killer activity and related memory failures.

**Use when:** Use when the kernel may have killed processes under pressure.


## 40. `dmesg -T | grep -i 'Out of memory'`


```bash

dmesg -T | grep -i 'Out of memory'

```

**Risk:** Low risk

**Technical explanation:** Finds kernel OOM messages with readable timestamps.

**Use when:** Use when correlating OOM events with incident time.


## 41. `ps -eo pid,ppid,cmd,rss,vsz,%mem --sort=-rss | head`


```bash

ps -eo pid,ppid,cmd,rss,vsz,%mem --sort=-rss | head

```

**Risk:** Low risk

**Technical explanation:** Lists the largest resident-memory processes to quickly identify consumers.

**Use when:** Use when one process may be exhausting RAM.


## 42. `pmap -x <pid> | tail -1`


```bash

pmap -x <pid> | tail -1

```

**Risk:** Low risk

**Technical explanation:** Summarizes one process’s mapped memory usage for targeted analysis.

**Use when:** Use when one suspect process needs closer memory inspection.


## 43. `smem -rk`


```bash

smem -rk

```

**Risk:** Low risk

**Technical explanation:** Reports proportional set size and other memory metrics when installed, improving shared-memory visibility.

**Use when:** Use when RSS alone is misleading across shared libraries.


## 44. `cat /proc/<pid>/status | egrep 'VmRSS|VmSize|Threads|FDSize'`


```bash

cat /proc/<pid>/status | egrep 'VmRSS|VmSize|Threads|FDSize'

```

**Risk:** Low risk

**Technical explanation:** Shows focused per-process memory and thread information directly from procfs.

**Use when:** Use when a quick structured view of one process is needed.


## 45. `numastat`


```bash

numastat

```

**Risk:** Low risk

**Technical explanation:** Displays NUMA allocation statistics when locality or remote memory access may matter.

**Use when:** Use when NUMA imbalance may explain memory latency.


# Disk, Filesystems, Inodes, and Space Pressure


## 46. `df -h`


```bash

df -h

```

**Risk:** Low risk

**Technical explanation:** Shows filesystem capacity and usage in human-readable form for rapid disk-pressure checks.

**Use when:** Use first when the host or app reports no space left.


## 47. `df -i`


```bash

df -i

```

**Risk:** Low risk

**Technical explanation:** Shows inode usage so you can detect inode exhaustion even when bytes remain free.

**Use when:** Use when writes fail despite free disk space.


## 48. `lsblk`


```bash

lsblk

```

**Risk:** Low risk

**Technical explanation:** Lists block devices, sizes, mount relationships, and transport information.

**Use when:** Use when validating storage topology or attached disks.


## 49. `lsblk -f`


```bash

lsblk -f

```

**Risk:** Low risk

**Technical explanation:** Adds filesystem type, label, and UUID details to block-device inspection.

**Use when:** Use when mount or filesystem identity may be wrong.


## 50. `blkid`


```bash

blkid

```

**Risk:** Low risk

**Technical explanation:** Prints block-device UUIDs and filesystem signatures for mount and fstab debugging.

**Use when:** Use when UUID-based mounts or recovery work are involved.


## 51. `findmnt`


```bash

findmnt

```

**Risk:** Low risk

**Technical explanation:** Shows the current mount tree in a readable format, which helps validate active mount topology.

**Use when:** Use when mounts, bind mounts, or propagation may be wrong.


## 52. `mount | column -t`


```bash

mount | column -t

```

**Risk:** Low risk

**Technical explanation:** Displays mounted filesystems with aligned fields for quick inspection.

**Use when:** Use when a simple readable mount list is enough.


## 53. `du -sh /var/* | sort -h`


```bash

du -sh /var/* | sort -h

```

**Risk:** Low risk

**Technical explanation:** Summarizes top-level disk usage under /var, a common pressure hotspot.

**Use when:** Use when /var growth may be filling the host.


## 54. `du -xhd1 / | sort -h`


```bash

du -xhd1 / | sort -h

```

**Risk:** Low risk

**Technical explanation:** Shows top disk usage on the root filesystem without crossing mount points.

**Use when:** Use when you need top-level space attribution safely.


## 55. `find / -xdev -type f -size +1G -ls | head -50`


```bash

find / -xdev -type f -size +1G -ls | head -50

```

**Risk:** Low risk

**Technical explanation:** Finds very large files on the current filesystem that may be consuming space.

**Use when:** Use when large single files may explain sudden disk growth.


## 56. `lsof +L1`


```bash

lsof +L1

```

**Risk:** Low risk

**Technical explanation:** Finds deleted-but-still-open files, a classic cause of missing reclaimed disk space.

**Use when:** Use when cleanup did not actually free disk.


## 57. `ncdu /`


```bash

ncdu /

```

**Risk:** Low risk

**Technical explanation:** Provides an interactive disk-usage browser when installed, useful for fast space triage.

**Use when:** Use when interactive space analysis is acceptable.


## 58. `tune2fs -l /dev/<device> | egrep 'Filesystem state|Mount count|Check interval'`


```bash

tune2fs -l /dev/<device> | egrep 'Filesystem state|Mount count|Check interval'

```

**Risk:** Low risk

**Technical explanation:** Shows ext filesystem metadata relevant to mount health and fsck policy.

**Use when:** Use when ext filesystem state or maintenance policy matters.


## 59. `xfs_info /mountpoint`


```bash

xfs_info /mountpoint

```

**Risk:** Low risk

**Technical explanation:** Displays XFS geometry and configuration when XFS-specific behavior matters.

**Use when:** Use when troubleshooting XFS-backed storage.


## 60. `filefrag -v /path/to/file`


```bash

filefrag -v /path/to/file

```

**Risk:** Low risk

**Technical explanation:** Shows extent layout for a file when fragmentation or storage layout is under investigation.

**Use when:** Use when one large file’s layout may impact IO.


# Block Devices, IO, and Storage Latency


## 61. `iostat -xz 1 5`


```bash

iostat -xz 1 5

```

**Risk:** Low risk

**Technical explanation:** Shows extended per-device IO statistics including utilization and await latency.

**Use when:** Use when storage latency may be driving application slowness.


## 62. `sar -d 1 5`


```bash

sar -d 1 5

```

**Risk:** Low risk

**Technical explanation:** Samples block-device throughput and latency over time.

**Use when:** Use when you need IO trends rather than a one-time snapshot.


## 63. `iotop -oPa`


```bash

iotop -oPa

```

**Risk:** Low risk

**Technical explanation:** Shows live processes performing IO, helping identify write-heavy or read-heavy offenders.

**Use when:** Use when one process may be saturating storage.


## 64. `pidstat -d 1 5`


```bash

pidstat -d 1 5

```

**Risk:** Low risk

**Technical explanation:** Reports per-process IO rates over time for workload attribution.

**Use when:** Use when storage-heavy behavior is bursty.


## 65. `lsblk -o NAME,MAJ:MIN,SIZE,ROTA,TYPE,MOUNTPOINT`


```bash

lsblk -o NAME,MAJ:MIN,SIZE,ROTA,TYPE,MOUNTPOINT

```

**Risk:** Low risk

**Technical explanation:** Shows device type and rotational hints for storage-class context.

**Use when:** Use when SSD vs HDD characteristics may matter.


## 66. `cat /proc/diskstats`


```bash

cat /proc/diskstats

```

**Risk:** Low risk

**Technical explanation:** Displays raw kernel block-device counters for low-level storage analysis.

**Use when:** Use when higher-level tools are unavailable.


## 67. `blockdev --getsize64 /dev/<device>`


```bash

blockdev --getsize64 /dev/<device>

```

**Risk:** Low risk

**Technical explanation:** Prints exact block-device size in bytes.

**Use when:** Use when precise device capacity validation is needed.


## 68. `hdparm -I /dev/<device>`


```bash

hdparm -I /dev/<device>

```

**Risk:** Medium risk

**Technical explanation:** Queries device identity and capabilities on supported block devices.

**Use when:** Use when device-level features or transport details matter.


## 69. `smartctl -a /dev/<device>`


```bash

smartctl -a /dev/<device>

```

**Risk:** Low risk

**Technical explanation:** Reads SMART health data when physical disk reliability is a concern.

**Use when:** Use when hardware disk failure may underlie errors.


## 70. `nvme list`


```bash

nvme list

```

**Risk:** Low risk

**Technical explanation:** Lists NVMe devices and namespaces on NVMe-based hosts.

**Use when:** Use when the host uses NVMe storage.


## 71. `nvme smart-log /dev/nvme0`


```bash

nvme smart-log /dev/nvme0

```

**Risk:** Low risk

**Technical explanation:** Shows NVMe health metrics for targeted SSD investigation.

**Use when:** Use when NVMe wear, temp, or media errors are suspected.


## 72. `dmsetup ls --tree`


```bash

dmsetup ls --tree

```

**Risk:** Low risk

**Technical explanation:** Shows device-mapper relationships, which helps explain LVM, dm-crypt, or multipath stacks.

**Use when:** Use when logical storage layering may hide the true device path.


## 73. `multipath -ll`


```bash

multipath -ll

```

**Risk:** Low risk

**Technical explanation:** Displays multipath device topology and path state in SAN-backed environments.

**Use when:** Use when path redundancy or SAN issues may be involved.


## 74. `fio --name=randread --filename=/tmp/fio.test --size=1G --rw=randread --bs=4k --iodepth=32 --runtime=30 --time_based`


```bash

fio --name=randread --filename=/tmp/fio.test --size=1G --rw=randread --bs=4k --iodepth=32 --runtime=30 --time_based

```

**Risk:** High risk

**Technical explanation:** Runs a controlled synthetic IO test when you must reproduce storage performance behavior, but it generates real load.

**Use when:** Use only when test load is acceptable and production impact is understood.


## 75. `blktrace -d /dev/<device> -o - | blkparse -i -`


```bash

blktrace -d /dev/<device> -o - | blkparse -i -

```

**Risk:** Medium risk

**Technical explanation:** Traces block-level IO on a device for advanced latency analysis.

**Use when:** Use when deeper storage tracing is needed and the overhead is acceptable.


# Processes, Services, and Lifecycle


## 76. `ps -ef`


```bash

ps -ef

```

**Risk:** Low risk

**Technical explanation:** Shows the full process list in traditional format for broad process inspection.

**Use when:** Use when you need a complete process baseline.


## 77. `ps auxf`


```bash

ps auxf

```

**Risk:** Low risk

**Technical explanation:** Shows processes in a tree-like hierarchy, which helps identify parent-child relationships.

**Use when:** Use when process ancestry matters.


## 78. `pstree -ap`


```bash

pstree -ap

```

**Risk:** Low risk

**Technical explanation:** Displays process ancestry with PIDs and arguments, useful for tracing service launch chains.

**Use when:** Use when wrappers or supervisors obscure the real parent process.


## 79. `pgrep -a <pattern>`


```bash

pgrep -a <pattern>

```

**Risk:** Low risk

**Technical explanation:** Finds matching processes and prints full command lines.

**Use when:** Use when locating one service or process family quickly.


## 80. `pkill -f <pattern>`


```bash

pkill -f <pattern>

```

**Risk:** High risk

**Technical explanation:** Signals processes matching a pattern, which is useful but can affect multiple workloads unexpectedly.

**Use when:** Use only when the pattern is precise and the target scope is confirmed.


## 81. `systemctl list-units --type=service --state=running`


```bash

systemctl list-units --type=service --state=running

```

**Risk:** Low risk

**Technical explanation:** Lists active services so you can confirm systemd service state at a glance.

**Use when:** Use when establishing service baseline on a host.


## 82. `systemctl status <service>`


```bash

systemctl status <service>

```

**Risk:** Low risk

**Technical explanation:** Shows service state, recent logs, main PID, and unit metadata for one service.

**Use when:** Use when one service is failing or degraded.


## 83. `systemctl cat <service>`


```bash

systemctl cat <service>

```

**Risk:** Low risk

**Technical explanation:** Displays the full effective unit definition including drop-ins.

**Use when:** Use when unit overrides or drop-ins may explain service behavior.


## 84. `systemctl show <service>`


```bash

systemctl show <service>

```

**Risk:** Low risk

**Technical explanation:** Prints unit properties in key=value form for detailed service introspection.

**Use when:** Use when you need exact systemd properties rather than human output.


## 85. `systemctl list-dependencies <service>`


```bash

systemctl list-dependencies <service>

```

**Risk:** Low risk

**Technical explanation:** Shows dependencies for a service so you can trace upstream or downstream startup requirements.

**Use when:** Use when service ordering or dependency failures may be involved.


## 86. `systemctl restart <service>`


```bash

systemctl restart <service>

```

**Risk:** High risk

**Technical explanation:** Restarts a service, which is useful for recovery but can interrupt active traffic and should follow evidence collection.

**Use when:** Use only after logs, state, and dependencies have been inspected.


## 87. `systemctl reload <service>`


```bash

systemctl reload <service>

```

**Risk:** Medium risk

**Technical explanation:** Requests config reload without full restart when supported by the service.

**Use when:** Use when applying config changes with lower impact than restart.


## 88. `systemctl is-enabled <service>`


```bash

systemctl is-enabled <service>

```

**Risk:** Low risk

**Technical explanation:** Shows whether a service is enabled at boot.

**Use when:** Use when service persistence across reboot is in question.


## 89. `systemctl daemon-reload`


```bash

systemctl daemon-reload

```

**Risk:** Medium risk

**Technical explanation:** Reloads systemd unit files after unit changes or drop-in modifications.

**Use when:** Use when service definition changes are not being recognized.


## 90. `loginctl list-sessions`


```bash

loginctl list-sessions

```

**Risk:** Low risk

**Technical explanation:** Shows active user sessions, which is helpful in session-leak or login issues.

**Use when:** Use when user sessions or lingering sessions may matter.


# Journals, Syslog, and Log Investigation


## 91. `journalctl -xe`


```bash

journalctl -xe

```

**Risk:** Low risk

**Technical explanation:** Shows recent high-priority journal entries with explanatory context.

**Use when:** Use when you need recent systemd-aware error context quickly.


## 92. `journalctl -u <service> -n 100 --no-pager`


```bash

journalctl -u <service> -n 100 --no-pager

```

**Risk:** Low risk

**Technical explanation:** Displays the most recent logs for one systemd service.

**Use when:** Use when a specific service is failing now.


## 93. `journalctl -u <service> --since '1 hour ago'`


```bash

journalctl -u <service> --since '1 hour ago'

```

**Risk:** Low risk

**Technical explanation:** Filters service logs to a recent incident window.

**Use when:** Use when the incident start time is known.


## 94. `journalctl -k -n 100 --no-pager`


```bash

journalctl -k -n 100 --no-pager

```

**Risk:** Low risk

**Technical explanation:** Shows recent kernel messages from the journal.

**Use when:** Use when the kernel, drivers, or low-level subsystems may be involved.


## 95. `journalctl -b`


```bash

journalctl -b

```

**Risk:** Low risk

**Technical explanation:** Displays logs from the current boot only, reducing noise across reboots.

**Use when:** Use when current-boot context matters more than older history.


## 96. `journalctl -b -1`


```bash

journalctl -b -1

```

**Risk:** Low risk

**Technical explanation:** Shows logs from the previous boot to investigate failures before a reboot.

**Use when:** Use when the host rebooted during or after the incident.


## 97. `journalctl --disk-usage`


```bash

journalctl --disk-usage

```

**Risk:** Low risk

**Technical explanation:** Reports space used by the systemd journal, which matters during log-pressure incidents.

**Use when:** Use when journal size may contribute to disk pressure.


## 98. `journalctl --vacuum-time=7d`


```bash

journalctl --vacuum-time=7d

```

**Risk:** High risk

**Technical explanation:** Removes old journal data older than the given retention window, reclaiming space but deleting historical logs.

**Use when:** Use only when log retention can be reduced safely.


## 99. `grep -R 'ERROR' /var/log`


```bash

grep -R 'ERROR' /var/log

```

**Risk:** Low risk

**Technical explanation:** Searches recursively for error strings in classic log files.

**Use when:** Use when non-journal app logs may hold the failure evidence.


## 100. `tail -f /var/log/messages`


```bash

tail -f /var/log/messages

```

**Risk:** Low risk

**Technical explanation:** Follows the traditional system message log in real time where present.

**Use when:** Use when the host uses messages-style logging.


## 101. `tail -f /var/log/syslog`


```bash

tail -f /var/log/syslog

```

**Risk:** Low risk

**Technical explanation:** Follows the syslog file on distributions that use it.

**Use when:** Use when the host uses syslog-style logging.


## 102. `dmesg -T | tail -100`


```bash

dmesg -T | tail -100

```

**Risk:** Low risk

**Technical explanation:** Shows recent kernel ring buffer messages with readable timestamps.

**Use when:** Use when you want the latest kernel events quickly.


## 103. `logger 'test message from host'`


```bash

logger 'test message from host'

```

**Risk:** Medium risk

**Technical explanation:** Writes a message to syslog or journal to verify local log ingestion paths.

**Use when:** Use when testing whether local logging is functioning end to end.


## 104. `ausearch -m AVC -ts recent`


```bash

ausearch -m AVC -ts recent

```

**Risk:** Low risk

**Technical explanation:** Searches recent SELinux AVC denials when available.

**Use when:** Use when access failures may actually be policy denials.


## 105. `last -x | head -20`


```bash

last -x | head -20

```

**Risk:** Low risk

**Technical explanation:** Shows recent shutdowns, reboots, and runlevel changes for timeline reconstruction.

**Use when:** Use when host-state transitions need to be correlated with impact.


# Networking, Routes, and Connectivity


## 106. `ip addr`


```bash

ip addr

```

**Risk:** Low risk

**Technical explanation:** Shows network interfaces, addresses, and link state in modern iproute2 format.

**Use when:** Use when IP configuration may be wrong or missing.


## 107. `ip link`


```bash

ip link

```

**Risk:** Low risk

**Technical explanation:** Shows interface state and flags, useful for carrier or admin-state checks.

**Use when:** Use when a link may be down or administratively disabled.


## 108. `ip route`


```bash

ip route

```

**Risk:** Low risk

**Technical explanation:** Displays the routing table so you can validate default gateways and route selection.

**Use when:** Use when traffic leaves the host incorrectly.


## 109. `ip rule`


```bash

ip rule

```

**Risk:** Low risk

**Technical explanation:** Shows policy routing rules when multiple routing tables or source-based routing is in use.

**Use when:** Use when standard route output does not explain packet flow.


## 110. `ip neigh`


```bash

ip neigh

```

**Risk:** Low risk

**Technical explanation:** Displays ARP and neighbor cache entries for L2/L3 resolution troubleshooting.

**Use when:** Use when next-hop resolution may be failing.


## 111. `ss -lntp`


```bash

ss -lntp

```

**Risk:** Low risk

**Technical explanation:** Shows listening TCP sockets and owning processes, which is critical in service bind issues.

**Use when:** Use when an app should be listening but clients cannot connect.


## 112. `ss -s`


```bash

ss -s

```

**Risk:** Low risk

**Technical explanation:** Summarizes socket state counts for a quick networking health snapshot.

**Use when:** Use when socket exhaustion or connection imbalance may exist.


## 113. `ss -ant state established`


```bash

ss -ant state established

```

**Risk:** Low risk

**Technical explanation:** Lists established TCP sessions so you can confirm live client connections.

**Use when:** Use when validating whether clients are actually connected.


## 114. `ping -c 4 <host>`


```bash

ping -c 4 <host>

```

**Risk:** Low risk

**Technical explanation:** Tests basic ICMP reachability to a target.

**Use when:** Use when simple reachability is the first question.


## 115. `traceroute <host>`


```bash

traceroute <host>

```

**Risk:** Low risk

**Technical explanation:** Shows path hops to a target for route-path diagnosis when available.

**Use when:** Use when the path beyond the local host may be broken.


## 116. `tracepath <host>`


```bash

tracepath <host>

```

**Risk:** Low risk

**Technical explanation:** Provides a non-root path trace alternative on many systems.

**Use when:** Use when traceroute is unavailable or rootless use is preferred.


## 117. `mtr -rwzbc 20 <host>`


```bash

mtr -rwzbc 20 <host>

```

**Risk:** Low risk

**Technical explanation:** Combines ping and traceroute style statistics to identify packet loss or latency along the path.

**Use when:** Use when intermittent loss or latency needs stronger path evidence.


## 118. `arp -an`


```bash

arp -an

```

**Risk:** Low risk

**Technical explanation:** Displays ARP cache entries on systems where net-tools remains in use.

**Use when:** Use when legacy tooling is still standard on the host.


## 119. `ethtool <iface>`


```bash

ethtool <iface>

```

**Risk:** Low risk

**Technical explanation:** Shows NIC link state, speed, duplex, and driver-reported health data.

**Use when:** Use when physical or virtual NIC state may be the problem.


## 120. `nmcli device status`


```bash

nmcli device status

```

**Risk:** Low risk

**Technical explanation:** Shows NetworkManager device state on hosts that use NetworkManager.

**Use when:** Use when NetworkManager may be managing connectivity.


# DNS, Name Resolution, and Service Discovery


## 121. `resolvectl status`


```bash

resolvectl status

```

**Risk:** Low risk

**Technical explanation:** Shows systemd-resolved DNS configuration, links, and current resolver state.

**Use when:** Use when systemd-resolved behavior may explain lookup failures.


## 122. `cat /etc/resolv.conf`


```bash

cat /etc/resolv.conf

```

**Risk:** Low risk

**Technical explanation:** Displays the active resolver configuration seen by libc clients.

**Use when:** Use when DNS server settings may be wrong.


## 123. `getent hosts <name>`


```bash

getent hosts <name>

```

**Risk:** Low risk

**Technical explanation:** Resolves a host using the system’s configured name-service switch order.

**Use when:** Use when you need the actual libc/NSS resolution path.


## 124. `getent ahosts <name>`


```bash

getent ahosts <name>

```

**Risk:** Low risk

**Technical explanation:** Shows all resolved address families for a host through NSS.

**Use when:** Use when IPv4 vs IPv6 resolution behavior matters.


## 125. `dig <name>`


```bash

dig <name>

```

**Risk:** Low risk

**Technical explanation:** Performs detailed DNS lookups when dnsutils or bind-utils are installed.

**Use when:** Use when you need direct DNS query detail outside NSS.


## 126. `dig +short <name>`


```bash

dig +short <name>

```

**Risk:** Low risk

**Technical explanation:** Returns a concise DNS answer for quick checks.

**Use when:** Use when you want a fast answer value only.


## 127. `dig @<dns_server> <name>`


```bash

dig @<dns_server> <name>

```

**Risk:** Low risk

**Technical explanation:** Queries a specific DNS server to compare resolver behavior.

**Use when:** Use when one DNS server may be serving stale or wrong answers.


## 128. `nslookup <name>`


```bash

nslookup <name>

```

**Risk:** Low risk

**Technical explanation:** Performs a simple DNS query on systems where nslookup is preferred or readily available.

**Use when:** Use when nslookup is standard on the host.


## 129. `host <name>`


```bash

host <name>

```

**Risk:** Low risk

**Technical explanation:** Resolves a name with concise output using the host utility.

**Use when:** Use when you want a simple DNS answer quickly.


## 130. `grep '^hosts:' /etc/nsswitch.conf`


```bash

grep '^hosts:' /etc/nsswitch.conf

```

**Risk:** Low risk

**Technical explanation:** Shows hostname resolution order so you can explain NSS behavior.

**Use when:** Use when /etc/hosts, DNS, LDAP, or other NSS sources may conflict.


## 131. `systemd-resolve --statistics`


```bash

systemd-resolve --statistics

```

**Risk:** Low risk

**Technical explanation:** Displays DNS resolver cache and transaction statistics on supported hosts.

**Use when:** Use when resolver caching behavior matters.


## 132. `tcpdump -ni any port 53`


```bash

tcpdump -ni any port 53

```

**Risk:** Medium risk

**Technical explanation:** Captures DNS packets on the host for low-level resolution debugging.

**Use when:** Use when configuration looks right but actual DNS traffic must be proven.


## 133. `dig -x <ip>`


```bash

dig -x <ip>

```

**Risk:** Low risk

**Technical explanation:** Performs reverse DNS lookup for an IP address.

**Use when:** Use when reverse mapping may affect auth, logging, or app behavior.


## 134. `resolvectl query <name>`


```bash

resolvectl query <name>

```

**Risk:** Low risk

**Technical explanation:** Queries a name through systemd-resolved directly.

**Use when:** Use when comparing systemd-resolved behavior against dig or NSS.


## 135. `grep -v '^#' /etc/hosts`


```bash

grep -v '^#' /etc/hosts

```

**Risk:** Low risk

**Technical explanation:** Shows active static host mappings that may override DNS unexpectedly.

**Use when:** Use when one host resolves differently from others.


# Ports, Packets, Firewalls, and Flow Analysis


## 136. `ss -ludp`


```bash

ss -ludp

```

**Risk:** Low risk

**Technical explanation:** Shows listening UDP sockets, important for DNS, syslog, and metrics agents.

**Use when:** Use when a UDP-based service may not actually be listening.


## 137. `lsof -i -P -n | head`


```bash

lsof -i -P -n | head

```

**Risk:** Low risk

**Technical explanation:** Lists network-open files and sockets without DNS or service-name resolution.

**Use when:** Use when you want process-to-socket mapping quickly.


## 138. `tcpdump -ni any host <ip>`


```bash

tcpdump -ni any host <ip>

```

**Risk:** Medium risk

**Technical explanation:** Captures packets to or from a host for direct traffic inspection.

**Use when:** Use when packet-level evidence is needed for one peer.


## 139. `tcpdump -ni any port <port>`


```bash

tcpdump -ni any port <port>

```

**Risk:** Medium risk

**Technical explanation:** Captures traffic for a specific port to confirm packets are arriving or leaving.

**Use when:** Use when traffic to one service port is in question.


## 140. `tcpdump -ni any 'tcp[tcpflags] & (tcp-syn|tcp-fin|tcp-rst) != 0'`


```bash

tcpdump -ni any 'tcp[tcpflags] & (tcp-syn|tcp-fin|tcp-rst) != 0'

```

**Risk:** Medium risk

**Technical explanation:** Captures TCP control flags to study handshake and reset behavior.

**Use when:** Use when connection setup or resets are the core symptom.


## 141. `iptables -L -n -v`


```bash

iptables -L -n -v

```

**Risk:** Low risk

**Technical explanation:** Shows filter table firewall rules with counters for packet-hit analysis.

**Use when:** Use when local firewall rules may be blocking traffic.


## 142. `iptables -t nat -L -n -v`


```bash

iptables -t nat -L -n -v

```

**Risk:** Low risk

**Technical explanation:** Shows NAT rules and counters when translation or port forwarding is involved.

**Use when:** Use when DNAT, SNAT, or port forwarding may be wrong.


## 143. `nft list ruleset`


```bash

nft list ruleset

```

**Risk:** Low risk

**Technical explanation:** Displays the full nftables ruleset on modern Linux hosts.

**Use when:** Use when nftables is the active firewall framework.


## 144. `firewall-cmd --list-all`


```bash

firewall-cmd --list-all

```

**Risk:** Low risk

**Technical explanation:** Shows firewalld zone configuration where firewalld is in use.

**Use when:** Use when firewalld-managed rules may be the cause.


## 145. `ufw status verbose`


```bash

ufw status verbose

```

**Risk:** Low risk

**Technical explanation:** Displays UFW firewall state and active rules on Ubuntu-style systems.

**Use when:** Use when UFW is the host firewall abstraction.


## 146. `conntrack -L | head`


```bash

conntrack -L | head

```

**Risk:** Low risk

**Technical explanation:** Shows connection tracking entries when stateful firewall or NAT behavior is relevant.

**Use when:** Use when state tables may explain connection behavior.


## 147. `conntrack -S`


```bash

conntrack -S

```

**Risk:** Low risk

**Technical explanation:** Reports conntrack table statistics for saturation or error analysis.

**Use when:** Use when conntrack exhaustion may be causing drops.


## 148. `iptables-save | less`


```bash

iptables-save | less

```

**Risk:** Low risk

**Technical explanation:** Exports the active iptables configuration for full-rule inspection.

**Use when:** Use when you need the full ordered ruleset, not just counters.


## 149. `nstat -az`


```bash

nstat -az

```

**Risk:** Low risk

**Technical explanation:** Shows kernel network stack counters such as retransmits and checksum issues.

**Use when:** Use when low-level TCP or IP anomalies are suspected.


## 150. `netstat -s`


```bash

netstat -s

```

**Risk:** Low risk

**Technical explanation:** Displays protocol statistics on systems where net-tools is available.

**Use when:** Use when older hosts still rely on netstat-based investigation.


# Files, Permissions, ACLs, and Ownership


## 151. `ls -lah`


```bash

ls -lah

```

**Risk:** Low risk

**Technical explanation:** Shows file listings with permissions, ownership, size, and timestamps in readable form.

**Use when:** Use when validating visible file metadata quickly.


## 152. `stat <path>`


```bash

stat <path>

```

**Risk:** Low risk

**Technical explanation:** Displays detailed inode metadata including mode bits, ownership, and timestamps.

**Use when:** Use when basic ls output is not detailed enough.


## 153. `namei -l <path>`


```bash

namei -l <path>

```

**Risk:** Low risk

**Technical explanation:** Expands each path component with permissions, helping debug directory traversal failures.

**Use when:** Use when the file looks correct but a parent directory blocks access.


## 154. `getfacl <path>`


```bash

getfacl <path>

```

**Risk:** Low risk

**Technical explanation:** Shows POSIX ACL entries on a file or directory when ACLs affect access.

**Use when:** Use when Unix mode bits do not fully explain permissions.


## 155. `setfacl -m u:<user>:rwx <path>`


```bash

setfacl -m u:<user>:rwx <path>

```

**Risk:** High risk

**Technical explanation:** Adds or modifies a POSIX ACL entry to test or restore access, but it changes security state.

**Use when:** Use only when ACL change is the intended remediation.


## 156. `chmod 640 <file>`


```bash

chmod 640 <file>

```

**Risk:** High risk

**Technical explanation:** Changes file mode bits, useful during controlled permissions correction but it mutates access behavior.

**Use when:** Use only when the correct desired mode is confirmed.


## 157. `chown <user>:<group> <path>`


```bash

chown <user>:<group> <path>

```

**Risk:** High risk

**Technical explanation:** Changes file ownership when service access depends on correct user or group mapping.

**Use when:** Use only when ownership drift is confirmed.


## 158. `find /path -type f -perm -4000 -ls`


```bash

find /path -type f -perm -4000 -ls

```

**Risk:** Low risk

**Technical explanation:** Finds setuid files for privilege and security review.

**Use when:** Use when security posture or unexpected privilege paths are under review.


## 159. `find /path -nouser -o -nogroup -ls`


```bash

find /path -nouser -o -nogroup -ls

```

**Risk:** Low risk

**Technical explanation:** Finds files whose user or group no longer exists.

**Use when:** Use when identity cleanup or broken ownership references are suspected.


## 160. `readlink -f <path>`


```bash

readlink -f <path>

```

**Risk:** Low risk

**Technical explanation:** Resolves a path to its canonical absolute form, which helps with symlink debugging.

**Use when:** Use when symlinks may hide the real target.


## 161. `file <path>`


```bash

file <path>

```

**Risk:** Low risk

**Technical explanation:** Identifies file type and format, useful when content differs from extension.

**Use when:** Use when a file is not what it claims to be.


## 162. `lsattr <path>`


```bash

lsattr <path>

```

**Risk:** Low risk

**Technical explanation:** Shows extended file attributes such as immutable flags on supported filesystems.

**Use when:** Use when even root cannot modify a file as expected.


## 163. `chattr +i <file>`


```bash

chattr +i <file>

```

**Risk:** High risk

**Technical explanation:** Sets the immutable attribute, useful in controlled protection scenarios but high impact if misused.

**Use when:** Use only when immutability is deliberately required.


## 164. `restorecon -Rv /path`


```bash

restorecon -Rv /path

```

**Risk:** High risk

**Technical explanation:** Restores SELinux security contexts recursively on a path, which can fix policy issues but changes labels broadly.

**Use when:** Use only when SELinux context drift is the confirmed problem.


## 165. `semanage fcontext -l | grep <path>`


```bash

semanage fcontext -l | grep <path>

```

**Risk:** Low risk

**Technical explanation:** Shows SELinux file context rules relevant to a path when available.

**Use when:** Use when you need to understand expected SELinux labeling.


# Users, Identity, PAM, and Access


## 166. `id <user>`


```bash

id <user>

```

**Risk:** Low risk

**Technical explanation:** Shows UID, primary GID, and supplementary groups for a user.

**Use when:** Use when user or group membership may explain access behavior.


## 167. `getent passwd <user>`


```bash

getent passwd <user>

```

**Risk:** Low risk

**Technical explanation:** Looks up a user through NSS so local and remote identity sources are both considered.

**Use when:** Use when local files and centralized identity may disagree.


## 168. `getent group <group>`


```bash

getent group <group>

```

**Risk:** Low risk

**Technical explanation:** Looks up a group through NSS for accurate group-source resolution.

**Use when:** Use when group membership source matters.


## 169. `who`


```bash

who

```

**Risk:** Low risk

**Technical explanation:** Shows currently logged-in users and their terminals.

**Use when:** Use when interactive users may be affecting the host.


## 170. `w`


```bash

w

```

**Risk:** Low risk

**Technical explanation:** Shows logged-in users plus what they are doing and system load context.

**Use when:** Use when you want session and activity context together.


## 171. `last <user> | head`


```bash

last <user> | head

```

**Risk:** Low risk

**Technical explanation:** Shows recent login history for one user.

**Use when:** Use when one user’s recent access matters.


## 172. `lastlog | head`


```bash

lastlog | head

```

**Risk:** Low risk

**Technical explanation:** Displays the most recent login per account.

**Use when:** Use when stale or dormant accounts are relevant.


## 173. `faillock --user <user>`


```bash

faillock --user <user>

```

**Risk:** Low risk

**Technical explanation:** Shows failed login counters on systems using faillock.

**Use when:** Use when login failures may be due to lockout.


## 174. `passwd -S <user>`


```bash

passwd -S <user>

```

**Risk:** Low risk

**Technical explanation:** Shows password status and aging information for a user.

**Use when:** Use when password expiry or lock state may block login.


## 175. `chage -l <user>`


```bash

chage -l <user>

```

**Risk:** Low risk

**Technical explanation:** Displays password aging policy and expiry details.

**Use when:** Use when account age policy may explain access denial.


## 176. `sudo -l -U <user>`


```bash

sudo -l -U <user>

```

**Risk:** Low risk

**Technical explanation:** Shows sudo privileges for a user when troubleshooting access escalation.

**Use when:** Use when escalation behavior differs from expectation.


## 177. `grep -v '^#' /etc/sudoers`


```bash

grep -v '^#' /etc/sudoers

```

**Risk:** Low risk

**Technical explanation:** Shows active sudoers lines for local policy inspection.

**Use when:** Use when local sudo policy must be reviewed quickly.


## 178. `visudo -c`


```bash

visudo -c

```

**Risk:** Low risk

**Technical explanation:** Validates sudoers syntax safely before or after policy edits.

**Use when:** Use when sudoers changes may have broken privilege escalation.


## 179. `sssctl user-checks <user>`


```bash

sssctl user-checks <user>

```

**Risk:** Low risk

**Technical explanation:** Tests SSSD-based identity and access behavior where SSSD is used.

**Use when:** Use when SSSD-backed identity lookups or access are failing.


## 180. `pam_tally2 --user <user>`


```bash

pam_tally2 --user <user>

```

**Risk:** Low risk

**Technical explanation:** Shows PAM tally failure counts on older systems that still use pam_tally2.

**Use when:** Use when older PAM lockout tracking is in place.


# Security, SELinux, AppArmor, and Audit


## 181. `getenforce`


```bash

getenforce

```

**Risk:** Low risk

**Technical explanation:** Shows the current SELinux enforcement mode.

**Use when:** Use when you need to know immediately whether SELinux is enforcing.


## 182. `sestatus`


```bash

sestatus

```

**Risk:** Low risk

**Technical explanation:** Displays SELinux policy status, mode, and loaded configuration.

**Use when:** Use when broader SELinux status matters.


## 183. `ausearch -m AVC,USER_AVC -ts recent`


```bash

ausearch -m AVC,USER_AVC -ts recent

```

**Risk:** Low risk

**Technical explanation:** Searches recent SELinux denials in the audit log.

**Use when:** Use when access denied errors may be policy denials.


## 184. `auditctl -s`


```bash

auditctl -s

```

**Risk:** Low risk

**Technical explanation:** Shows audit subsystem status including enabled state and backlog.

**Use when:** Use when audit collection health may affect observability.


## 185. `auditctl -l`


```bash

auditctl -l

```

**Risk:** Low risk

**Technical explanation:** Lists active audit rules for security-event capture validation.

**Use when:** Use when expected audit rules may be missing.


## 186. `aureport -x --summary`


```bash

aureport -x --summary

```

**Risk:** Low risk

**Technical explanation:** Summarizes audited executable events when audit data is available.

**Use when:** Use when you want high-level execution activity from audit logs.


## 187. `apparmor_status`


```bash

apparmor_status

```

**Risk:** Low risk

**Technical explanation:** Shows AppArmor profile state on systems using AppArmor.

**Use when:** Use when Ubuntu-style policy enforcement may affect apps.


## 188. `aa-status`


```bash

aa-status

```

**Risk:** Low risk

**Technical explanation:** Displays AppArmor enforcement and complain-mode status.

**Use when:** Use when AppArmor state must be checked quickly.


## 189. `sealert -a /var/log/audit/audit.log`


```bash

sealert -a /var/log/audit/audit.log

```

**Risk:** Low risk

**Technical explanation:** Analyzes audit logs for SELinux-related recommendations when setroubleshoot is available.

**Use when:** Use when human-readable SELinux guidance is helpful.


## 190. `getcap -r / 2>/dev/null | head -100`


```bash

getcap -r / 2>/dev/null | head -100

```

**Risk:** Low risk

**Technical explanation:** Lists file capabilities across the filesystem for privilege review.

**Use when:** Use when capabilities may explain non-root privileged behavior.


## 191. `capsh --print`


```bash

capsh --print

```

**Risk:** Low risk

**Technical explanation:** Shows current process capabilities and bounding set.

**Use when:** Use when debugging privilege or container capability issues.


## 192. `sysctl kernel.kptr_restrict`


```bash

sysctl kernel.kptr_restrict

```

**Risk:** Low risk

**Technical explanation:** Checks kernel pointer restriction settings relevant to security posture.

**Use when:** Use when debugging tools or hardening settings may conflict.


## 193. `sysctl kernel.yama.ptrace_scope`


```bash

sysctl kernel.yama.ptrace_scope

```

**Risk:** Low risk

**Technical explanation:** Shows ptrace restriction policy, which affects debugging and security.

**Use when:** Use when attach or tracing tools fail due to security policy.


## 194. `journalctl -t sudo`


```bash

journalctl -t sudo

```

**Risk:** Low risk

**Technical explanation:** Shows sudo-related journal entries for access and escalation investigations.

**Use when:** Use when sudo use or failures need timeline context.


## 195. `ausearch -ua <uid> -ts today`


```bash

ausearch -ua <uid> -ts today

```

**Risk:** Low risk

**Technical explanation:** Searches audit events for one user ID in the current day.

**Use when:** Use when tracing one user’s audited actions.


# Kernel, Modules, Sysctl, and Hardware


## 196. `sysctl -a | head -100`


```bash

sysctl -a | head -100

```

**Risk:** Low risk

**Technical explanation:** Shows kernel tunables and current values for broad sysctl inspection.

**Use when:** Use when kernel runtime settings may explain behavior.


## 197. `sysctl net.ipv4.ip_forward`


```bash

sysctl net.ipv4.ip_forward

```

**Risk:** Low risk

**Technical explanation:** Prints one kernel parameter value quickly.

**Use when:** Use when validating one specific sysctl.


## 198. `sysctl -w net.ipv4.ip_forward=1`


```bash

sysctl -w net.ipv4.ip_forward=1

```

**Risk:** High risk

**Technical explanation:** Sets a kernel parameter at runtime, which is useful but changes live behavior immediately.

**Use when:** Use only when the runtime sysctl change is intentional and safe.


## 199. `lsmod`


```bash

lsmod

```

**Risk:** Low risk

**Technical explanation:** Lists loaded kernel modules and their usage counts.

**Use when:** Use when kernel functionality may depend on loaded modules.


## 200. `modinfo <module>`


```bash

modinfo <module>

```

**Risk:** Low risk

**Technical explanation:** Shows module metadata such as filename, version, and parameters.

**Use when:** Use when you need details about a specific module.


## 201. `modprobe <module>`


```bash

modprobe <module>

```

**Risk:** High risk

**Technical explanation:** Loads a kernel module and any dependencies when you need to activate functionality.

**Use when:** Use only when loading the module is the intended change.


## 202. `modprobe -r <module>`


```bash

modprobe -r <module>

```

**Risk:** High risk

**Technical explanation:** Unloads a module when safe and supported, which can affect live hardware or features.

**Use when:** Use only when you fully understand the live impact.


## 203. `dmesg -T | egrep -i 'error|fail|warn'`


```bash

dmesg -T | egrep -i 'error|fail|warn'

```

**Risk:** Low risk

**Technical explanation:** Searches recent kernel messages for warning or failure patterns.

**Use when:** Use when kernel-level warnings may explain symptoms.


## 204. `journalctl -k --since '1 hour ago'`


```bash

journalctl -k --since '1 hour ago'

```

**Risk:** Low risk

**Technical explanation:** Shows kernel logs from the last hour via journal.

**Use when:** Use when recent kernel events matter.


## 205. `lspci -nn`


```bash

lspci -nn

```

**Risk:** Low risk

**Technical explanation:** Lists PCI devices with numeric IDs for hardware and driver mapping.

**Use when:** Use when hardware identity or drivers are involved.


## 206. `lsusb`


```bash

lsusb

```

**Risk:** Low risk

**Technical explanation:** Lists USB devices for peripheral visibility.

**Use when:** Use when USB-attached devices matter.


## 207. `dmidecode -t system`


```bash

dmidecode -t system

```

**Risk:** Low risk

**Technical explanation:** Shows hardware platform and BIOS/vendor information when supported.

**Use when:** Use when hardware vendor or model context is needed.


## 208. `sensors`


```bash

sensors

```

**Risk:** Low risk

**Technical explanation:** Displays hardware temperature and voltage data on systems with lm-sensors configured.

**Use when:** Use when thermal stress or hardware conditions may affect performance.


## 209. `hwinfo --short`


```bash

hwinfo --short

```

**Risk:** Low risk

**Technical explanation:** Summarizes hardware inventory on systems where hwinfo is installed.

**Use when:** Use when a compact hardware inventory helps.


## 210. `cat /proc/cmdline`


```bash

cat /proc/cmdline

```

**Risk:** Low risk

**Technical explanation:** Shows the kernel boot command line, which can explain runtime behavior and early boot settings.

**Use when:** Use when boot parameters may have altered runtime behavior.


# Scheduling, Cron, Timers, and Background Jobs


## 211. `crontab -l`


```bash

crontab -l

```

**Risk:** Low risk

**Technical explanation:** Lists the current user’s cron entries.

**Use when:** Use when checking user-level scheduled jobs.


## 212. `crontab -u <user> -l`


```bash

crontab -u <user> -l

```

**Risk:** Low risk

**Technical explanation:** Lists cron jobs for another user with sufficient privilege.

**Use when:** Use when a service account’s jobs may be relevant.


## 213. `ls -lah /etc/cron.d`


```bash

ls -lah /etc/cron.d

```

**Risk:** Low risk

**Technical explanation:** Shows system-wide cron definitions stored in /etc/cron.d.

**Use when:** Use when centrally managed cron jobs may be involved.


## 214. `ls -lah /etc/cron.daily`


```bash

ls -lah /etc/cron.daily

```

**Risk:** Low risk

**Technical explanation:** Lists daily cron job scripts.

**Use when:** Use when recurring maintenance may be affecting the host.


## 215. `systemctl list-timers --all`


```bash

systemctl list-timers --all

```

**Risk:** Low risk

**Technical explanation:** Shows systemd timers, next run times, and associated units.

**Use when:** Use when systemd timers may have triggered work.


## 216. `systemctl status <timer>`


```bash

systemctl status <timer>

```

**Risk:** Low risk

**Technical explanation:** Displays timer state and recent activity for one timer unit.

**Use when:** Use when one timer-backed job may be failing or firing unexpectedly.


## 217. `systemctl cat <timer>`


```bash

systemctl cat <timer>

```

**Risk:** Low risk

**Technical explanation:** Shows the effective definition of a timer unit including drop-ins.

**Use when:** Use when timer configuration may explain schedule behavior.


## 218. `atq`


```bash

atq

```

**Risk:** Low risk

**Technical explanation:** Lists pending at jobs on systems using atd.

**Use when:** Use when one-off scheduled jobs may be queued.


## 219. `at -c <jobid>`


```bash

at -c <jobid>

```

**Risk:** Low risk

**Technical explanation:** Displays the script content for a queued at job.

**Use when:** Use when a queued at job may be suspicious or relevant.


## 220. `batch`


```bash

batch

```

**Risk:** Medium risk

**Technical explanation:** Queues a command to run when system load levels permit.

**Use when:** Use when low-priority work should be deferred automatically.


## 221. `systemd-run --on-active=5m /usr/bin/date`


```bash

systemd-run --on-active=5m /usr/bin/date

```

**Risk:** Medium risk

**Technical explanation:** Creates a transient timer-backed unit for controlled delayed execution.

**Use when:** Use when you need a short-lived delayed command without editing unit files.


## 222. `systemd-run --scope -p CPUQuota=50% <command>`


```bash

systemd-run --scope -p CPUQuota=50% <command>

```

**Risk:** Medium risk

**Technical explanation:** Runs a command in a transient scope with runtime limits for controlled testing.

**Use when:** Use when testing a command under explicit resource caps.


## 223. `journalctl -u crond -n 100 --no-pager`


```bash

journalctl -u crond -n 100 --no-pager

```

**Risk:** Low risk

**Technical explanation:** Shows recent cron daemon logs on systems using crond.

**Use when:** Use when cron jobs may not have executed correctly.


## 224. `journalctl -u cron -n 100 --no-pager`


```bash

journalctl -u cron -n 100 --no-pager

```

**Risk:** Low risk

**Technical explanation:** Shows recent cron logs on systems naming the service cron.

**Use when:** Use when Debian-style cron service logs are needed.


## 225. `grep CRON /var/log/syslog | tail -50`


```bash

grep CRON /var/log/syslog | tail -50

```

**Risk:** Low risk

**Technical explanation:** Finds recent cron activity in syslog-style logs.

**Use when:** Use when cron activity is logged through syslog rather than journal service filters.


# Packages, Repos, and Update Troubleshooting


## 226. `rpm -qa | wc -l`


```bash

rpm -qa | wc -l

```

**Risk:** Low risk

**Technical explanation:** Counts installed RPM packages for a rough package inventory size.

**Use when:** Use when you need a rough package footprint quickly.


## 227. `rpm -q <package>`


```bash

rpm -q <package>

```

**Risk:** Low risk

**Technical explanation:** Checks whether a specific RPM package is installed.

**Use when:** Use when one package may be missing.


## 228. `rpm -V <package>`


```bash

rpm -V <package>

```

**Risk:** Low risk

**Technical explanation:** Verifies installed RPM-managed files for drift against package metadata.

**Use when:** Use when package-managed files may have been modified.


## 229. `yum repolist`


```bash

yum repolist

```

**Risk:** Low risk

**Technical explanation:** Lists configured YUM repositories and their availability on older RHEL-family systems.

**Use when:** Use when repo access is failing on yum-based hosts.


## 230. `dnf repolist`


```bash

dnf repolist

```

**Risk:** Low risk

**Technical explanation:** Lists configured DNF repositories and their state on newer RHEL-family systems.

**Use when:** Use when repo access is failing on dnf-based hosts.


## 231. `dnf history`


```bash

dnf history

```

**Risk:** Low risk

**Technical explanation:** Shows package transaction history for change correlation.

**Use when:** Use when a recent package change may have caused the issue.


## 232. `dnf provides */<binary>`


```bash

dnf provides */<binary>

```

**Risk:** Low risk

**Technical explanation:** Finds which package provides a given file path or binary.

**Use when:** Use when a command or file is missing and ownership must be found.


## 233. `apt list --installed | head`


```bash

apt list --installed | head

```

**Risk:** Low risk

**Technical explanation:** Shows installed Debian packages in APT-managed environments.

**Use when:** Use when checking package presence on Debian-style hosts.


## 234. `dpkg -l | head`


```bash

dpkg -l | head

```

**Risk:** Low risk

**Technical explanation:** Lists Debian packages using dpkg format.

**Use when:** Use when dpkg-native listing is preferred.


## 235. `apt-cache policy <package>`


```bash

apt-cache policy <package>

```

**Risk:** Low risk

**Technical explanation:** Shows candidate and installed versions along with repository sources.

**Use when:** Use when version skew or repo priority may explain behavior.


## 236. `apt-cache madison <package>`


```bash

apt-cache madison <package>

```

**Risk:** Low risk

**Technical explanation:** Lists available versions of a package across configured repos.

**Use when:** Use when selecting or validating an exact package version.


## 237. `dpkg -S /path/to/file`


```bash

dpkg -S /path/to/file

```

**Risk:** Low risk

**Technical explanation:** Finds which Debian package owns a file.

**Use when:** Use when a file must be mapped back to its package.


## 238. `journalctl -u packagekit -n 100 --no-pager`


```bash

journalctl -u packagekit -n 100 --no-pager

```

**Risk:** Low risk

**Technical explanation:** Shows PackageKit activity when package locks or GUI-managed updates interfere.

**Use when:** Use when package manager locks may come from PackageKit.


## 239. `fuser /var/lib/dpkg/lock-frontend`


```bash

fuser /var/lib/dpkg/lock-frontend

```

**Risk:** Low risk

**Technical explanation:** Shows which process holds the APT frontend lock.

**Use when:** Use when apt reports the lock is busy.


## 240. `lsof /var/lib/rpm/.rpm.lock`


```bash

lsof /var/lib/rpm/.rpm.lock

```

**Risk:** Low risk

**Technical explanation:** Shows who holds the RPM database lock in RPM-managed systems.

**Use when:** Use when rpm or dnf reports a database lock issue.


# Containers, Cgroups, and Namespaces


## 241. `systemd-cgls`


```bash

systemd-cgls

```

**Risk:** Low risk

**Technical explanation:** Shows the systemd cgroup hierarchy and where services live within it.

**Use when:** Use when resource usage must be mapped to units or slices.


## 242. `systemd-cgtop`


```bash

systemd-cgtop

```

**Risk:** Low risk

**Technical explanation:** Displays live cgroup resource usage by service or slice.

**Use when:** Use when one cgroup may be consuming disproportionate resources.


## 243. `cat /proc/<pid>/cgroup`


```bash

cat /proc/<pid>/cgroup

```

**Risk:** Low risk

**Technical explanation:** Shows a process’s cgroup membership directly from procfs.

**Use when:** Use when a process may be inside unexpected cgroups.


## 244. `lsns`


```bash

lsns

```

**Risk:** Low risk

**Technical explanation:** Lists active namespaces on the system for container and isolation analysis.

**Use when:** Use when namespace isolation is part of the problem.


## 245. `nsenter -t <pid> -n ip addr`


```bash

nsenter -t <pid> -n ip addr

```

**Risk:** Medium risk

**Technical explanation:** Enters a process’s network namespace to inspect its interfaces.

**Use when:** Use when network behavior differs inside a containerized namespace.


## 246. `nsenter -t <pid> -m mount`


```bash

nsenter -t <pid> -m mount

```

**Risk:** Medium risk

**Technical explanation:** Enters a process’s mount namespace to inspect its mounts.

**Use when:** Use when mount behavior inside a namespace must be proven.


## 247. `cat /sys/fs/cgroup/cgroup.controllers`


```bash

cat /sys/fs/cgroup/cgroup.controllers

```

**Risk:** Low risk

**Technical explanation:** Shows available cgroup v2 controllers on unified hierarchies.

**Use when:** Use when cgroup v2 capabilities must be confirmed.


## 248. `cat /sys/fs/cgroup/<path>/memory.current`


```bash

cat /sys/fs/cgroup/<path>/memory.current

```

**Risk:** Low risk

**Technical explanation:** Shows current memory usage for a cgroup v2 path.

**Use when:** Use when one cgroup may be near its memory limit.


## 249. `cat /sys/fs/cgroup/<path>/cpu.max`


```bash

cat /sys/fs/cgroup/<path>/cpu.max

```

**Risk:** Low risk

**Technical explanation:** Shows the CPU quota and period for a cgroup v2 path.

**Use when:** Use when quota enforcement may explain throttling.


## 250. `crictl ps`


```bash

crictl ps

```

**Risk:** Low risk

**Technical explanation:** Lists CRI-managed containers on hosts that use CRI tools.

**Use when:** Use when the host runs Kubernetes or CRI-managed workloads.


## 251. `ctr -n k8s.io containers ls`


```bash

ctr -n k8s.io containers ls

```

**Risk:** Low risk

**Technical explanation:** Lists containerd-managed containers in Kubernetes namespaces when containerd is in use.

**Use when:** Use when containerd is the underlying runtime.


## 252. `runc list`


```bash

runc list

```

**Risk:** Low risk

**Technical explanation:** Shows running containers managed directly by runc where applicable.

**Use when:** Use when low-level OCI runtime state is needed.


## 253. `podman ps`


```bash

podman ps

```

**Risk:** Low risk

**Technical explanation:** Lists running Podman containers on systems using Podman.

**Use when:** Use when the host uses Podman rather than Docker.


## 254. `podman inspect <container>`


```bash

podman inspect <container>

```

**Risk:** Low risk

**Technical explanation:** Shows Podman container metadata when Podman-managed workloads are involved.

**Use when:** Use when container runtime metadata is needed on Podman hosts.


## 255. `journalctl -u containerd -n 100 --no-pager`


```bash

journalctl -u containerd -n 100 --no-pager

```

**Risk:** Low risk

**Technical explanation:** Displays recent containerd logs for runtime troubleshooting.

**Use when:** Use when container runtime behavior may be the failure plane.


# Boot, Rescue, and Crash Troubleshooting


## 256. `grubby --info=ALL`


```bash

grubby --info=ALL

```

**Risk:** Low risk

**Technical explanation:** Shows installed boot entries and kernel arguments on supported distributions.

**Use when:** Use when boot entry selection or kernel args may be wrong.


## 257. `cat /boot/grub2/grub.cfg | head -100`


```bash

cat /boot/grub2/grub.cfg | head -100

```

**Risk:** Low risk

**Technical explanation:** Lets you inspect generated GRUB configuration cautiously.

**Use when:** Use when bootloader-generated config needs quick inspection.


## 258. `efibootmgr -v`


```bash

efibootmgr -v

```

**Risk:** Low risk

**Technical explanation:** Displays UEFI boot entries and order on EFI-based systems.

**Use when:** Use when EFI boot order or entry drift may be involved.


## 259. `lsinitrd`


```bash

lsinitrd

```

**Risk:** Low risk

**Technical explanation:** Shows initramfs contents or metadata on supported distributions.

**Use when:** Use when early boot failures may involve missing initramfs content.


## 260. `journalctl -b -p err`


```bash

journalctl -b -p err

```

**Risk:** Low risk

**Technical explanation:** Displays error-priority logs from the current boot only.

**Use when:** Use when current boot contains unresolved errors.


## 261. `journalctl -b -1 -p err`


```bash

journalctl -b -1 -p err

```

**Risk:** Low risk

**Technical explanation:** Shows error-priority logs from the previous boot.

**Use when:** Use when the problem happened before the last reboot.


## 262. `kdumpctl status`


```bash

kdumpctl status

```

**Risk:** Low risk

**Technical explanation:** Shows whether kdump is configured and active on supported systems.

**Use when:** Use when crash-dump readiness matters.


## 263. `coredumpctl list | head`


```bash

coredumpctl list | head

```

**Risk:** Low risk

**Technical explanation:** Lists stored core dumps managed by systemd-coredump.

**Use when:** Use when crashed processes may have left core evidence.


## 264. `coredumpctl info <pid>`


```bash

coredumpctl info <pid>

```

**Risk:** Low risk

**Technical explanation:** Shows metadata and stack information for one captured core dump.

**Use when:** Use when one crash event needs closer inspection.


## 265. `coredumpctl gdb <pid>`


```bash

coredumpctl gdb <pid>

```

**Risk:** Medium risk

**Technical explanation:** Opens a core dump in gdb for deep crash analysis when symbols are available.

**Use when:** Use when stack-level crash debugging is justified.


## 266. `sysctl kernel.core_pattern`


```bash

sysctl kernel.core_pattern

```

**Risk:** Low risk

**Technical explanation:** Shows how core dumps are routed or named on the host.

**Use when:** Use when expected core dumps are missing.


## 267. `ulimit -c`


```bash

ulimit -c

```

**Risk:** Low risk

**Technical explanation:** Displays the current shell’s core file size limit.

**Use when:** Use when a process may not be allowed to dump core.


## 268. `systemctl get-default`


```bash

systemctl get-default

```

**Risk:** Low risk

**Technical explanation:** Shows the default systemd target to which the system boots.

**Use when:** Use when boot target selection may be wrong.


## 269. `systemctl isolate rescue.target`


```bash

systemctl isolate rescue.target

```

**Risk:** Emergency-only

**Technical explanation:** Switches the system to rescue mode, which is powerful and disruptive and should be used only in tightly controlled recovery.

**Use when:** Use only in explicit rescue or break-glass recovery scenarios.


## 270. `last -x | head -20`


```bash

last -x | head -20

```

**Risk:** Low risk

**Technical explanation:** Shows recent shutdowns, reboots, and runlevel changes for timeline reconstruction.

**Use when:** Use when crash, reboot, or target transitions must be correlated.


# Recovery Verification by Scenario

## After CPU or load stabilization
Re-run:

- `uptime`
- `mpstat -P ALL 1 5`
- `pidstat 1 5`

Verify that load is trending down, CPU saturation is no longer concentrated, and no new top CPU offenders have emerged.

## After memory or OOM mitigation
Re-run:

- `free -h`
- `sar -S 1 5`
- `journalctl -k | grep -i oom`

Verify that memory pressure has eased, swap churn is reduced, and no new OOM kills are appearing.

## After disk cleanup or inode recovery
Re-run:

- `df -h`
- `df -i`
- `lsof +L1`

Verify that both byte usage and inode usage are healthy again, and deleted-but-open files are no longer hiding consumed space.

## After service recovery
Re-run:

- `systemctl status <service>`
- `journalctl -u <service> -n 100 --no-pager`
- `ss -lntp`

Verify that the service is active, recent logs are clean, and the expected listening socket exists.

## After network, route, or firewall repair
Re-run:

- `ip addr`
- `ip route`
- `ss -s`
- `iptables -L -n -v`

Verify that interface state, routing, socket posture, and firewall counters now align with intended traffic flow.

## After DNS or resolver repair
Re-run:

- `cat /etc/resolv.conf`
- `getent hosts <name>`
- `dig <name>`
- `resolvectl status`

Verify that local resolver config, NSS resolution, and direct DNS query results are now consistent.

## After permission or SELinux/AppArmor correction
Re-run:

- `stat <path>`
- `namei -l <path>`
- `getfacl <path>`
- `ausearch -m AVC,USER_AVC -ts recent`

Verify that file metadata is correct, directory traversal is allowed, ACLs are expected, and policy denials have stopped.

## After boot or crash recovery
Re-run:

- `who -b`
- `journalctl -b -p err`
- `systemd-analyze`
- `systemctl --failed`

Verify that the new boot is stable, current-boot errors are reduced, boot timing is acceptable, and failed units are not accumulating.

## Total Commands

This pack contains **270 commands**.

# Troubleshooting Discipline

Strong Linux incident response is not about memorizing the most commands. It is about:

- separating kernel symptoms from service symptoms
- distinguishing compute, memory, IO, network, filesystem, identity, and policy failure planes
- preserving evidence before disruptive action
- choosing the lowest-risk command that proves the next hypothesis
- restoring service without creating a second outage

That is the standard expected from a strong senior Linux Platform, DevOps, or SRE engineer.
