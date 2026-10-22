# Linux Technical Notes — Process Management, Networking, Storage, Troubleshooting, and Common Commands

This file is a **deep technical-notes document** for Linux.

It is designed to explain:

- what Linux is from a systems perspective
- the Linux process model
- process management commands
- networking concepts and commands
- storage and filesystem concepts
- troubleshooting workflows
- common commands with practical examples
- technical explanations useful for learning and interview preparation

This file is written as a **technical notes file**, not a distribution-specific installation guide.

---

# 1) What Linux is

Linux is a **Unix-like operating system kernel**, and in common usage the term “Linux” often refers to an entire operating system distribution built around that kernel.

A Linux system usually includes:
- the kernel
- system libraries
- a shell
- utilities
- daemons/services
- package management tools
- filesystems and storage tools
- networking tools

Examples of Linux distributions:
- Ubuntu
- Debian
- RHEL
- Rocky Linux
- CentOS Stream
- Fedora
- Arch Linux
- SUSE

From an engineering perspective, Linux is important because it provides:
- process isolation
- memory management
- filesystems
- networking stack
- permissions and security model
- service management
- observability interfaces through `/proc`, `/sys`, logs, and command-line tooling

---

# 2) Linux system architecture at a high level

A simplified Linux stack looks like this:

1. **Hardware**
2. **Kernel**
3. **System libraries**
4. **System services and daemons**
5. **Shell and user-space utilities**
6. **Applications**

The kernel is responsible for:
- scheduling processes
- memory management
- device management
- filesystem interaction
- networking
- system calls
- security boundaries

User-space programs interact with the kernel through **system calls**.

---

# 3) Linux filesystem layout basics

Linux uses a single directory tree rooted at `/`.

Important directories:

- `/` → root of filesystem hierarchy
- `/bin` → essential user binaries
- `/sbin` → essential system binaries
- `/etc` → system configuration
- `/home` → user home directories
- `/var` → variable data such as logs and spool files
- `/tmp` → temporary files
- `/usr` → user-space programs and libraries
- `/opt` → optional application software
- `/dev` → device files
- `/proc` → process and kernel information
- `/sys` → sysfs kernel/device information
- `/boot` → bootloader and kernel-related files
- `/mnt` and `/media` → mount points

Understanding the filesystem layout helps with troubleshooting because many Linux administration tasks involve locating configuration, logs, sockets, devices, or mount points.

---

# 4) Shell and command-line basics

The shell is the command interpreter.

Common shells:
- `bash`
- `zsh`
- `sh`
- `fish`

Useful shell concepts:
- current working directory
- standard input, output, and error
- pipes
- redirection
- environment variables
- exit codes

Example:

```bash
echo "hello" > file.txt
cat file.txt | grep hello
echo $HOME
echo $?
```

Meaning:
- `>` redirects output to a file
- `|` pipes output to another command
- `$HOME` reads an environment variable
- `$?` gives the previous command’s exit code

---

# 5) Process management

Process management is one of the most important Linux topics.

## 5.1 What a process is

A **process** is a running instance of a program.

A process has:
- a PID
- memory space
- file descriptors
- environment
- scheduling state
- parent-child relationships
- privileges and ownership

Linux systems are multi-process systems.

---

## 5.2 Process states conceptually

Common process states include:
- running
- sleeping
- stopped
- zombie

A **zombie** process is a process that has exited but still has an entry in the process table because its parent has not yet reaped its exit status.

---

## 5.3 View running processes with `ps`

Basic usage:

```bash
ps
```

Detailed full-format view:

```bash
ps -ef
```

Show process tree:

```bash
ps -ejH
```

Search for a process:

```bash
ps -ef | grep nginx
```

`ps` gives a snapshot of processes at the moment the command runs.

---

## 5.4 Monitor processes with `top`

```bash
top
```

This shows dynamic system information such as:
- CPU usage
- memory usage
- running processes
- load averages

`top` is useful for quick live inspection.

---

## 5.5 Modern interactive process viewers

Examples:
- `htop`
- `btop`

Example:

```bash
htop
```

These are often easier to use than `top`, though availability depends on the system.

---

## 5.6 Find process IDs with `pgrep`

```bash
pgrep nginx
```

Match with full command line:

```bash
pgrep -af python
```

This is often cleaner than `ps | grep`.

---

## 5.7 Kill processes with `kill`

Send default termination signal:

```bash
kill 1234
```

Force kill:

```bash
kill -9 1234
```

Common signals:
- `SIGTERM` (15) → graceful termination
- `SIGKILL` (9) → immediate kill
- `SIGHUP` → often reload or hangup signal depending on process
- `SIGSTOP` → stop execution
- `SIGCONT` → continue execution

Important concept:
Use `SIGTERM` first when possible.  
`SIGKILL` should usually be the last resort.

---

## 5.8 Kill by name with `pkill` and `killall`

```bash
pkill nginx
```

Match full command:

```bash
pkill -f "python app.py"
```

Kill all processes by exact name:

```bash
killall nginx
```

Use carefully because name-based process termination can affect multiple processes.

---

## 5.9 Background and foreground jobs

Run a job in background:

```bash
sleep 300 &
```

List shell jobs:

```bash
jobs
```

Bring job to foreground:

```bash
fg %1
```

Send current foreground job to background after suspending:
1. press `Ctrl+Z`
2. then run:

```bash
bg
```

This is shell job control, which is different from full service management.

---

## 5.10 View parent-child process relationships

Use process tree:

```bash
pstree
```

Or with PIDs:

```bash
pstree -p
```

This helps when understanding service launchers, shells, and child worker processes.

---

## 5.11 Inspect process files under `/proc`

Each process has a directory under `/proc/<pid>/`.

Examples:

```bash
ls /proc/1234
cat /proc/1234/status
cat /proc/1234/cmdline
ls -l /proc/1234/fd
```

Useful information includes:
- command line
- memory maps
- open file descriptors
- status
- environment
- network details in some cases

---

## 5.12 Nice and renice

Process priority can be influenced with niceness.

Start with lower priority:

```bash
nice -n 10 long_running_job
```

Change priority of existing process:

```bash
renice 5 -p 1234
```

Niceness affects scheduling preference, not guaranteed CPU share.

---

## 5.13 Systemd service management

Many modern Linux systems use `systemd`.

Common commands:

Check service status:

```bash
systemctl status nginx
```

Start service:

```bash
sudo systemctl start nginx
```

Stop service:

```bash
sudo systemctl stop nginx
```

Restart service:

```bash
sudo systemctl restart nginx
```

Enable on boot:

```bash
sudo systemctl enable nginx
```

Disable on boot:

```bash
sudo systemctl disable nginx
```

This is essential for managing long-running services.

---

# 6) Networking

Networking is a core Linux administration area.

## 6.1 View IP addresses with `ip`

Show addresses:

```bash
ip addr
```

Show links/interfaces:

```bash
ip link
```

Show routes:

```bash
ip route
```

`ip` is the modern replacement for older tools like `ifconfig` and `route` in many workflows.

---

## 6.2 Bring interfaces up or down

Bring interface down:

```bash
sudo ip link set eth0 down
```

Bring interface up:

```bash
sudo ip link set eth0 up
```

Use carefully on remote systems because disabling the active interface may disconnect you.

---

## 6.3 Test connectivity with `ping`

```bash
ping 8.8.8.8
```

Ping a hostname:

```bash
ping example.com
```

This helps determine whether:
- IP connectivity works
- DNS resolution works
- the target is reachable and responding to ICMP

---

## 6.4 Trace route path

Examples:
- `traceroute`
- `tracepath`

```bash
traceroute example.com
```

This helps inspect network path hops between source and destination.

---

## 6.5 DNS lookup tools

Examples:
- `dig`
- `nslookup`
- `host`

Query A record with `dig`:

```bash
dig example.com
```

Short answer only:

```bash
dig +short example.com
```

Reverse lookup example:

```bash
dig -x 8.8.8.8
```

DNS troubleshooting often starts here.

---

## 6.6 View listening sockets and connections with `ss`

Show TCP listening ports:

```bash
ss -tuln
```

Show processes too:

```bash
ss -tulpn
```

Show established connections:

```bash
ss -tan
```

`ss` is one of the most useful Linux networking commands.

---

## 6.7 Older alternatives

Older or still-seen tools:
- `netstat`
- `ifconfig`
- `route`

These still appear in older systems and documentation, but `ip` and `ss` are preferred in many environments.

---

## 6.8 Test application endpoints with `curl`

HTTP request:

```bash
curl http://example.com
```

Show headers:

```bash
curl -I http://example.com
```

Verbose request:

```bash
curl -v http://example.com
```

POST example:

```bash
curl -X POST -H "Content-Type: application/json" -d '{"name":"alice"}' http://example.com/api/users
```

`curl` is central to network and API troubleshooting.

---

## 6.9 Download files with `wget`

```bash
wget https://example.com/file.tar.gz
```

This is often used for non-interactive downloads.

---

## 6.10 Check host reachability and ports

Examples:
- `nc` (netcat)
- `telnet` on older systems

Check if port 443 is reachable:

```bash
nc -vz example.com 443
```

This is useful when testing whether a service port is open.

---

## 6.11 Firewall basics

Firewall management depends on distribution and tooling.

Examples:
- `ufw`
- `firewalld`
- `iptables`
- `nft`

Example with `ufw` status:

```bash
sudo ufw status
```

Example with `firewalld`:

```bash
sudo firewall-cmd --list-all
```

Example with `iptables`:

```bash
sudo iptables -L -n -v
```

Understanding which firewall framework is active is important during troubleshooting.

---

## 6.12 Common networking troubleshooting flow

A strong troubleshooting order is:

1. check local interface state
2. check IP address
3. check route table
4. check DNS resolution
5. check remote reachability
6. check open port or listener
7. check firewall rules
8. check application logs

Example command sequence:

```bash
ip addr
ip route
ping 8.8.8.8
dig example.com
ss -tulpn
curl -v http://localhost:8080
```

---

# 7) Storage and filesystems

Storage is another major Linux administration area.

## 7.1 Block devices and partitions

Common tools:
- `lsblk`
- `fdisk`
- `parted`
- `blkid`

Show block devices:

```bash
lsblk
```

Show filesystems too:

```bash
lsblk -f
```

List block device identifiers:

```bash
blkid
```

This helps identify disks, partitions, UUIDs, and mount relationships.

---

## 7.2 Disk usage with `df`

Show filesystem usage:

```bash
df -h
```

This tells you:
- total size
- used space
- available space
- mount point

This is one of the first commands to use when a system reports “disk full.”

---

## 7.3 Directory usage with `du`

Show directory sizes:

```bash
du -sh /var/log
```

Show top usage under a directory:

```bash
du -sh /var/* | sort -h
```

`df` answers filesystem-level usage.  
`du` answers directory/file-level usage.

---

## 7.4 Mounts

Show mounted filesystems:

```bash
mount
```

Better structured view:

```bash
findmnt
```

Mount a device:

```bash
sudo mount /dev/sdb1 /mnt/data
```

Unmount:

```bash
sudo umount /mnt/data
```

Persistent mounts are usually configured in `/etc/fstab`.

---

## 7.5 Filesystem check

Example tool:

```bash
fsck /dev/sdb1
```

Use carefully.  
A filesystem check on a mounted live filesystem can be dangerous depending on filesystem type and mode.

---

## 7.6 Inodes

A filesystem can run out of **inodes** even when space still exists.

Check inode usage:

```bash
df -i
```

This matters when systems have huge numbers of small files.

---

## 7.7 File and directory operations

Common commands:

List files:

```bash
ls -lah
```

Copy:

```bash
cp source.txt dest.txt
```

Move or rename:

```bash
mv old.txt new.txt
```

Remove file:

```bash
rm file.txt
```

Remove directory recursively:

```bash
rm -rf old_dir
```

Create directory:

```bash
mkdir -p /tmp/example/path
```

Create empty file:

```bash
touch file.txt
```

These are basic, but they matter constantly in real Linux workflows.

---

## 7.8 Find files

Find by name:

```bash
find /var/log -name "*.log"
```

Find files larger than 100 MB:

```bash
find / -type f -size +100M 2>/dev/null
```

Locate if indexed database exists:

```bash
locate nginx.conf
```

`find` is one of the most important Linux commands.

---

## 7.9 File permissions

View permissions:

```bash
ls -l
```

Change mode:

```bash
chmod 644 file.txt
```

Make executable:

```bash
chmod +x script.sh
```

Change owner:

```bash
sudo chown user:group file.txt
```

Permission model basics:
- read
- write
- execute

for:
- owner
- group
- others

---

## 7.10 Archive and compress files

Create tar archive:

```bash
tar -cvf archive.tar mydir
```

Create gzip-compressed archive:

```bash
tar -czvf archive.tar.gz mydir
```

Extract:

```bash
tar -xzvf archive.tar.gz
```

These are common operational commands.

---

## 7.11 LVM and RAID concepts

In many enterprise Linux environments you also encounter:
- LVM (Logical Volume Manager)
- RAID

LVM helps abstract storage into:
- physical volumes
- volume groups
- logical volumes

RAID helps with:
- redundancy
- performance
- fault tolerance

Even if you do not use them daily, understanding the concepts is important.

---

# 8) Logs and troubleshooting

Troubleshooting on Linux often means combining:
- system state inspection
- process inspection
- networking inspection
- storage inspection
- logs
- service status

## 8.1 View logs with `journalctl`

On systemd systems:

Show recent logs:

```bash
journalctl -xe
```

Show logs for a service:

```bash
journalctl -u nginx
```

Follow logs live:

```bash
journalctl -u nginx -f
```

This is often one of the fastest ways to understand why a service failed.

---

## 8.2 Traditional logs

Many systems also use log files under `/var/log`.

Examples:
- `/var/log/syslog`
- `/var/log/messages`
- `/var/log/auth.log`
- `/var/log/secure`
- application-specific logs

View with:

```bash
tail -f /var/log/syslog
```

or:

```bash
less /var/log/messages
```

---

## 8.3 Read files efficiently

Show first lines:

```bash
head -n 20 file.txt
```

Show last lines:

```bash
tail -n 50 file.txt
```

Follow appended content:

```bash
tail -f app.log
```

Page through file:

```bash
less app.log
```

Search within files:

```bash
grep "ERROR" app.log
```

Recursive grep:

```bash
grep -R "listen 80" /etc/nginx
```

---

## 8.4 Check memory usage

Show memory summary:

```bash
free -h
```

Kernel and memory details:

```bash
cat /proc/meminfo
```

This helps identify whether a system is memory-constrained.

---

## 8.5 Check CPU and load

Uptime and load average:

```bash
uptime
```

Detailed system activity:
- `top`
- `mpstat`
- `vmstat`
- `iostat`

Example:

```bash
vmstat 1
```

This helps distinguish:
- CPU pressure
- I/O wait
- runnable queue pressure
- memory pressure

---

## 8.6 Check open files with `lsof`

Show open files by process:

```bash
lsof -p 1234
```

Find which process uses a port:

```bash
sudo lsof -i :8080
```

Find deleted-but-still-open files:

```bash
lsof | grep deleted
```

This is extremely useful when disk space appears missing after log deletion.

---

## 8.7 Check why a port is busy

Example flow:

```bash
ss -tulpn | grep 8080
sudo lsof -i :8080
ps -fp <pid>
```

This lets you identify:
- what process owns the port
- whether it is expected
- how it was launched

---

## 8.8 Common troubleshooting scenarios

### Service fails to start

Typical commands:

```bash
systemctl status nginx
journalctl -u nginx -n 100
ss -tulpn
```

Questions to check:
- is configuration valid?
- is the port already in use?
- is the dependency available?
- are permissions correct?

---

### Disk full

Typical commands:

```bash
df -h
df -i
du -sh /var/* | sort -h
lsof | grep deleted
```

Questions to check:
- space exhaustion or inode exhaustion?
- log growth?
- deleted files still held open?
- unexpected large directories?

---

### Cannot reach a service

Typical commands:

```bash
ip addr
ip route
ping target-host
dig target-host
nc -vz target-host 443
curl -v http://target-host:8080
```

Questions to check:
- DNS issue?
- routing problem?
- firewall block?
- service not listening?
- application error?

---

### High CPU usage

Typical commands:

```bash
top
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head
pidstat 1
```

Questions to check:
- single runaway process?
- expected batch job?
- retry loop?
- CPU saturation or load from many runnable tasks?

---

### High memory usage

Typical commands:

```bash
free -h
top
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head
cat /proc/meminfo
```

Questions to check:
- actual memory exhaustion?
- page cache vs process RSS?
- memory leak?
- swap pressure?

---

# 9) Common commands quick reference

## 9.1 File and directory commands

```bash
pwd
ls -lah
cd /path
mkdir -p newdir
touch file.txt
cp a.txt b.txt
mv old.txt new.txt
rm file.txt
rm -rf olddir
find /tmp -name "*.tmp"
```

---

## 9.2 Process and service commands

```bash
ps -ef
top
pgrep nginx
kill 1234
pkill nginx
systemctl status nginx
journalctl -u nginx -f
```

---

## 9.3 Networking commands

```bash
ip addr
ip route
ping 8.8.8.8
dig example.com
ss -tulpn
curl -v http://localhost:8080
nc -vz example.com 443
```

---

## 9.4 Storage commands

```bash
lsblk -f
df -h
df -i
du -sh /var/*
mount
findmnt
blkid
```

---

## 9.5 Text processing commands

```bash
cat file.txt
less file.txt
head -n 20 file.txt
tail -f app.log
grep ERROR app.log
grep -R "server_name" /etc/nginx
sort file.txt
uniq file.txt
wc -l file.txt
```

---

# 10) Interview-ready technical explanations

## What is a process in Linux?

A strong answer:

> A process is a running instance of a program with its own PID, memory space, file descriptors, execution state, credentials, and parent-child relationship. Linux manages processes through scheduling, signals, and system calls.

---

## What is the difference between `df` and `du`?

A strong answer:

> `df` reports filesystem-level space usage, while `du` reports space consumed by specific directories or files. `df` is used to understand overall filesystem capacity, and `du` helps identify where space is being used.

---

## What is the difference between `kill`, `pkill`, and `killall`?

A strong answer:

> `kill` sends a signal to a specific PID, `pkill` matches processes by name or pattern and signals them, and `killall` signals processes by name. `kill` is the most precise; name-based commands are more convenient but require more caution.

---

## What is the difference between `ping`, `dig`, and `curl`?

A strong answer:

> `ping` checks basic network reachability using ICMP, `dig` checks DNS resolution, and `curl` tests application-layer communication such as HTTP. They troubleshoot different layers of the connectivity path.

---

## Why is `/proc` important?

A strong answer:

> `/proc` is a virtual filesystem that exposes kernel and process state in a file-like form. It is important because Linux troubleshooting often relies on inspecting process, memory, CPU, and kernel information through `/proc`.

---

# 11) Open-source references

Useful sources for deeper learning:

1. Linux kernel documentation  
   https://www.kernel.org/doc/html/latest/

2. Filesystem Hierarchy Standard  
   https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html

3. `systemd` documentation  
   https://www.freedesktop.org/software/systemd/man/

4. `ip` man page  
   https://man7.org/linux/man-pages/man8/ip.8.html

5. `ss` man page  
   https://man7.org/linux/man-pages/man8/ss.8.html

6. `ps` man page  
   https://man7.org/linux/man-pages/man1/ps.1.html

7. `top` man page  
   https://man7.org/linux/man-pages/man1/top.1.html

8. `journalctl` man page  
   https://man7.org/linux/man-pages/man1/journalctl.1.html

9. `lsblk` man page  
   https://man7.org/linux/man-pages/man8/lsblk.8.html

10. `findmnt` man page  
   https://man7.org/linux/man-pages/man8/findmnt.8.html

11. `lsof` project and docs  
   https://github.com/lsof-org/lsof

12. `find` GNU documentation  
   https://www.gnu.org/software/findutils/manual/html_node/find_html/index.html

---

# 12) Final summary

The most important way to understand Linux is this:

- Linux is a kernel-centered operating system environment
- processes are fundamental runtime units
- the filesystem hierarchy organizes system structure
- `/proc` and `/sys` expose system state
- process management depends on PIDs, signals, scheduling, and services
- networking depends on interfaces, routing, DNS, sockets, and firewalls
- storage depends on block devices, filesystems, mounts, and usage analysis
- troubleshooting combines commands, logs, service state, and layered reasoning

If you understand:
- processes
- signals
- services
- IP addresses and routes
- DNS and sockets
- filesystems and mounts
- disk usage and inode usage
- logs and journals
- common troubleshooting commands

then you understand the practical technical core of Linux very well.
