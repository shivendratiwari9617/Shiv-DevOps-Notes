# Linux Subtopics and Bullet-Level Subpoints

## 1. Introduction to Linux

### 1.1 What is Linux
Linux is an open-source operating system ecosystem centered on the Linux kernel.
It provides process, memory, file, device, and network abstractions to user-space software.

- **Definition — Definition of Linux:** Linux is a Unix-like operating system kernel used as the core of complete operating system distributions. 
 **Concrete example:** Ubuntu uses the Linux kernel to manage hardware and run applications.

- **Definition — Open-source nature:** Linux source code is publicly available for inspection, modification, and redistribution under open licenses. 
 **Concrete example:** A vendor can customize the Linux kernel for an embedded router.

- **Definition — Unix-like operating system:** Linux follows core Unix design patterns such as hierarchical files, processes, permissions, and shells. 
 **Concrete example:** Linux uses paths like `/etc/passwd` and commands like `ls` and `grep`.

- **Definition — Multiuser and multitasking design:** Linux supports multiple concurrent users and multiple running processes with isolation and scheduling. 
 **Concrete example:** Two users can log in over SSH while background services continue running.

- **Definition — Portability across architectures:** Linux is designed to run on many CPU architectures through portable code and architecture-specific support layers. 
 **Concrete example:** Linux runs on x86_64 servers, ARM phones, and RISC-V boards.

### 1.2 Linux History
Linux history describes the development of the Linux kernel and its integration with GNU user-space tooling.
It explains how Linux evolved from a small kernel project into a general-purpose production platform.

- **Definition — GNU project background:** The GNU project created free user-space tools and system software that complemented the Linux kernel. 
 **Concrete example:** GNU Bash and GCC are commonly used on Linux systems.

- **Definition — Creation of the Linux kernel:** The Linux kernel began as a personal Unix-like kernel project and later became a global open-source effort. 
 **Concrete example:** Linus Torvalds announced the first Linux kernel in 1991.

- **Definition — Role of Linus Torvalds:** Linus Torvalds initiated Linux and remains a central maintainer of the upstream kernel project. 
 **Concrete example:** Kernel patches are still merged through the upstream Linux development process he leads.

- **Definition — Evolution of Linux distributions:** Distributions assembled the kernel with package managers, libraries, and administration tools into complete systems. 
 **Concrete example:** Debian, Red Hat, and Arch each package Linux differently.

- **Definition — Community and enterprise adoption:** Linux gained broad use through volunteer collaboration and commercial support models. 
 **Concrete example:** Linux powers cloud servers, enterprise databases, and consumer devices.

### 1.3 Linux Distributions
A Linux distribution packages the kernel with libraries, package tools, defaults, and system utilities.
Distributions differ in package format, release model, support lifecycle, and administration workflow.

- **Definition — Debian-based distributions:** Debian-based systems use `.deb` packages and APT-style package management. 
 **Concrete example:** Ubuntu installs software with `apt install nginx`.

- **Definition — Red Hat-based distributions:** Red Hat-based systems use `.rpm` packages and DNF or YUM package workflows. 
 **Concrete example:** Fedora installs software with `dnf install nginx`.

- **Definition — Arch-based distributions:** Arch-based systems emphasize rolling release updates and minimal default installation. 
 **Concrete example:** Arch Linux installs packages with `pacman -S vim`.

- **Definition — SUSE-based distributions:** SUSE-based systems use RPM packages with SUSE administration tooling and policies. 
 **Concrete example:** openSUSE manages packages with `zypper`.

- **Definition — Source-based distributions:** Source-based distributions build software from source code rather than relying only on binary packages. 
 **Concrete example:** Gentoo compiles packages using Portage.

- **Definition — Embedded and specialized distributions:** Specialized distributions target appliances, IoT devices, security testing, or minimal deployments. 
 **Concrete example:** OpenWrt is a Linux distribution designed for routers.

### 1.4 Linux Architecture Overview
Linux architecture separates privileged kernel space from user space components such as shells and applications.
Programs use libraries and system calls to request controlled services from the kernel.

- **Definition — Kernel space:** Kernel space is the privileged execution domain where the kernel and core drivers run. 
 **Concrete example:** The memory manager executes in kernel space.

- **Definition — User space:** User space is the unprivileged execution domain where applications and utilities run. 
 **Concrete example:** The `vim` editor runs in user space.

- **Definition — Shell:** A shell is a command interpreter that parses input and launches programs. 
 **Concrete example:** Bash interprets `ls -l /var/log`.

- **Definition — System libraries:** System libraries provide reusable APIs that wrap low-level kernel functionality for applications. 
 **Concrete example:** glibc exposes functions such as `open()` and `printf()`.

- **Definition — System utilities:** System utilities are standard command-line tools used to inspect and manage the system. 
 **Concrete example:** `ps`, `cp`, and `mount` are system utilities.

- **Definition — Init system:** The init system starts and supervises user-space services after kernel boot. 
 **Concrete example:** systemd launches `sshd` during startup.

---

## 2. Linux Kernel

### 2.1 Kernel Fundamentals
Kernel fundamentals define the kernel as the privileged control layer that manages core machine resources.
They explain how the kernel enforces protection, scheduling, hardware control, and process isolation.

- **Definition — Definition of the kernel:** The kernel is the central operating system component that mediates access to hardware and protected resources. 
 **Concrete example:** The kernel decides which process gets CPU time next.

- **Definition — Monolithic kernel design:** A monolithic kernel places core services such as scheduling, memory management, and drivers in one privileged address space. 
 **Concrete example:** Linux network stack and VFS run in the kernel address space.

- **Definition — Modular kernel support:** Linux supports dynamic extension of kernel functionality through loadable modules. 
 **Concrete example:** A storage driver can be loaded without rebuilding the kernel.

- **Definition — Kernel responsibilities:** The kernel manages CPU scheduling, memory, devices, filesystems, interrupts, and system calls. 
 **Concrete example:** It maps virtual memory for each process.

- **Definition — Privileged execution model:** Kernel code executes with elevated hardware privileges unavailable to user-space programs. 
 **Concrete example:** User applications cannot directly program page tables.

### 2.2 Kernel Components
Kernel components are specialized subsystems that implement distinct operating-system functions.
They cooperate through internal kernel interfaces to deliver process, memory, I/O, and networking behavior.

- **Definition — Process scheduler:** The process scheduler selects runnable tasks and assigns CPU execution time. 
 **Concrete example:** Completely Fair Scheduler balances CPU time among user processes.

- **Definition — Memory manager:** The memory manager handles virtual memory, page allocation, reclamation, and address translation. 
 **Concrete example:** It maps a process heap into virtual memory pages.

- **Definition — Device drivers:** Device drivers translate generic kernel operations into hardware-specific control sequences. 
 **Concrete example:** A NIC driver moves packets between memory and the network card.

- **Definition — Virtual file system:** The VFS provides a common filesystem interface independent of the underlying filesystem type. 
 **Concrete example:** `open()` works on EXT4 and XFS through the VFS layer.

- **Definition — Network stack:** The network stack implements packet processing, sockets, routing, and transport protocols. 
 **Concrete example:** TCP connections are handled by the kernel network stack.

- **Definition — System call interface:** The system call interface exposes controlled kernel services to user-space programs. 
 **Concrete example:** `read()` enters the kernel through the syscall interface.

- **Definition — IPC mechanisms:** IPC mechanisms enable controlled communication and synchronization between processes. 
 **Concrete example:** Two processes can exchange data through a pipe.

### 2.3 Kernel Modules
Kernel modules are dynamically loadable kernel extensions used to add or remove functionality at runtime.
They are commonly used for drivers, filesystems, and optional protocol support.

- **Definition — Loadable kernel modules:** A loadable kernel module is a binary object that can be inserted into the running kernel. 
 **Concrete example:** `xfs.ko` provides XFS filesystem support.

- **Definition — Module insertion:** Module insertion loads a compiled module into the running kernel with dependency resolution. 
 **Concrete example:** `modprobe loop` loads the loop device module.

- **Definition — Module removal:** Module removal unloads a module if it is no longer in active use. 
 **Concrete example:** `modprobe -r loop` removes the loop module.

- **Definition — Module dependency handling:** Dependency handling ensures required modules are loaded before a target module is activated. 
 **Concrete example:** `modprobe` loads prerequisite crypto modules automatically.

- **Definition — Module information:** Module information describes metadata such as author, version, aliases, and supported devices. 
 **Concrete example:** `modinfo e1000e` prints details about the Intel NIC driver module.

- **Definition — Persistent module configuration:** Persistent module configuration controls which modules load automatically and with what options. 
 **Concrete example:** `/etc/modprobe.d/blacklist.conf` can prevent a module from loading.

### 2.4 System Calls
System calls are controlled entry points from user mode into kernel mode for privileged operations.
They define the kernel ABI used by applications for files, processes, memory, and networking.

- **Definition — User space to kernel space transition:** This transition switches execution privilege so a process can request kernel services safely. 
 **Concrete example:** Calling `write()` traps into the kernel.

- **Definition — Common system calls:** Common system calls implement frequently used OS services such as I/O, process control, and timing. 
 **Concrete example:** `close()` releases an open file descriptor.

- **Definition — File-related system calls:** File-related syscalls open, read, write, seek, and manage file metadata. 
 **Concrete example:** `stat()` retrieves file metadata from the kernel.

- **Definition — Process-related system calls:** Process-related syscalls create processes, replace program images, and manage process state. 
 **Concrete example:** `fork()` creates a child process.

- **Definition — Memory-related system calls:** Memory-related syscalls map, protect, and unmap memory regions in a process address space. 
 **Concrete example:** `mmap()` maps a file into memory.

- **Definition — Networking-related system calls:** Networking-related syscalls create sockets and exchange data over network connections. 
 **Concrete example:** `socket()` creates a socket endpoint.

### 2.5 Boot and Kernel Initialization
Boot and kernel initialization cover the path from firmware handoff to the first user-space process.
The kernel initializes hardware state, memory, drivers, and initial root access during this sequence.

- **Definition — BIOS/UEFI role:** Firmware performs early hardware initialization and loads a bootloader from boot media. 
 **Concrete example:** UEFI loads GRUB from the EFI system partition.

- **Definition — Bootloader role:** The bootloader loads the kernel and initramfs and passes boot parameters. 
 **Concrete example:** GRUB selects a kernel entry and loads it into memory.

- **Definition — Kernel loading:** Kernel loading places the kernel image in memory and transfers control to it. 
 **Concrete example:** GRUB loads `vmlinuz` before handing execution to the kernel.

- **Definition — initramfs/initrd:** initramfs provides an early user-space environment needed to access the real root filesystem. 
 **Concrete example:** It may load storage drivers needed to mount an LVM root volume.

- **Definition — Early userspace:** Early userspace performs setup tasks before the final root filesystem is switched into place. 
 **Concrete example:** It can assemble RAID devices before the normal boot continues.

- **Definition — Init system startup:** The init system becomes PID 1 and starts regular system services after early boot. 
 **Concrete example:** systemd starts logging and networking services.

### 2.6 Kernel Information and Management
Kernel information interfaces expose runtime details, logs, tunables, and hardware views.
Administrators use them to inspect behavior and adjust kernel parameters.

- **Definition — `uname`:** `uname` reports kernel identity information such as name, release, and architecture. 
 **Concrete example:** `uname -r` prints the running kernel release.

- **Definition — `/proc`:** `/proc` is a pseudo-filesystem exposing process and kernel runtime information. 
 **Concrete example:** `/proc/meminfo` shows memory statistics.

- **Definition — `/sys`:** `/sys` is a pseudo-filesystem exposing kernel objects, devices, and driver state. 
 **Concrete example:** `/sys/class/net` lists network interfaces.

- **Definition — `dmesg`:** `dmesg` displays messages from the kernel ring buffer. 
 **Concrete example:** `dmesg | grep usb` shows USB detection events.

- **Definition — Kernel parameters:** Kernel parameters modify boot-time or runtime kernel behavior. 
 **Concrete example:** `quiet` reduces boot message verbosity.

- **Definition — `sysctl`:** `sysctl` reads and writes runtime kernel tunables exposed under `/proc/sys`. 
 **Concrete example:** `sysctl net.ipv4.ip_forward=1` enables IPv4 forwarding.

### 2.7 Kernel Scheduling and Timing
Kernel scheduling and timing define how tasks are selected, preempted, and measured over time.
These mechanisms combine scheduler classes, timers, and context switching to control execution.

- **Definition — Scheduler classes:** Scheduler classes define alternative task-selection policies such as normal and real-time scheduling. 
 **Concrete example:** `SCHED_FIFO` is used for real-time tasks.

- **Definition — Time slices:** A time slice is the CPU time budget a scheduler grants a task before reconsidering scheduling. 
 **Concrete example:** An interactive process may receive a short scheduling slice.

- **Definition — Context switching:** Context switching saves one task state and restores another task state on a CPU. 
 **Concrete example:** The CPU switches from a shell to a web server worker.

- **Definition — Preemption:** Preemption interrupts a running task so a more eligible task can run immediately. 
 **Concrete example:** A real-time task preempts a normal task.

- **Definition — Timer interrupts:** Timer interrupts trigger periodic or high-resolution scheduling and timing events. 
 **Concrete example:** A timer interrupt can wake the scheduler.

- **Definition — High-resolution timers:** High-resolution timers provide fine-grained timing beyond coarse periodic timer ticks. 
 **Concrete example:** Multimedia software can schedule low-latency wakeups.

### 2.8 Kernel Memory Concepts
Kernel memory concepts define how Linux manages RAM, virtual memory, caches, and low-memory recovery.
They explain how the kernel isolates processes while maximizing performance and utilization.

- **Definition — Physical memory:** Physical memory is the actual RAM installed in the system. 
 **Concrete example:** A server may have 64 GB of physical memory.

- **Definition — Virtual memory:** Virtual memory gives each process an isolated address space independent of physical layout. 
 **Concrete example:** Two processes can both map memory at the same virtual address.

- **Definition — Paging:** Paging divides memory into fixed-size pages for mapping and management. 
 **Concrete example:** Linux commonly uses 4 KB memory pages.

- **Definition — Slab allocator:** The slab allocator caches frequently used kernel objects to reduce allocation overhead. 
 **Concrete example:** Inode structures are often served from slab caches.

- **Definition — Page cache:** The page cache stores file data in memory to accelerate repeated reads and writes. 
 **Concrete example:** Reading the same file twice is often faster due to page cache reuse.

- **Definition — Out-of-memory handling:** OOM handling reclaims memory or terminates processes when memory exhaustion becomes critical. 
 **Concrete example:** The OOM killer may terminate a memory-hungry process.

### 2.9 Namespaces and Control Groups
Namespaces isolate process views of global resources, and cgroups account for and limit resource consumption.
They are core kernel primitives used in containers and workload isolation.

- **Definition — PID namespaces:** PID namespaces give processes an isolated process-ID numbering view. 
 **Concrete example:** A container can see its init process as PID 1.

- **Definition — Mount namespaces:** Mount namespaces isolate a process set's filesystem mount table. 
 **Concrete example:** A container can mount filesystems unseen by the host namespace.

- **Definition — Network namespaces:** Network namespaces isolate interfaces, routing tables, and firewall state. 
 **Concrete example:** A container can have its own virtual Ethernet interface.

- **Definition — User namespaces:** User namespaces isolate user and group ID mappings between environments. 
 **Concrete example:** Container root can map to an unprivileged host UID.

- **Definition — cgroups:** cgroups group processes for resource accounting and enforcement. 
 **Concrete example:** A service can be limited to 1 GB of RAM.

- **Definition — Resource isolation:** Resource isolation prevents one workload from monopolizing shared system resources. 
 **Concrete example:** CPU quotas can stop one container from consuming all CPU time.

### 2.10 Kernel Diagnostics and Tracing
Kernel diagnostics and tracing provide observability into syscalls, performance counters, and internal events.
They help investigate correctness, crashes, latency, and bottlenecks on live systems.

- **Definition — `strace`:** `strace` traces system calls and signals made by a process. 
 **Concrete example:** `strace ls` shows `openat()` and `getdents64()` calls.

- **Definition — `ltrace`:** `ltrace` traces dynamic library calls made by a program. 
 **Concrete example:** It can show calls into glibc functions.

- **Definition — `/proc/<pid>`:** `/proc/<pid>` exposes per-process runtime state, memory maps, descriptors, and status. 
 **Concrete example:** `/proc/1234/cmdline` shows a process command line.

- **Definition — `perf`:** `perf` measures hardware and kernel performance events for profiling and analysis. 
 **Concrete example:** `perf top` shows frequently sampled functions.

- **Definition — `ftrace`:** `ftrace` traces internal kernel function activity and timing. 
 **Concrete example:** It can trace scheduler events during latency debugging.

- **Definition — Kernel logs:** Kernel logs record driver messages, hardware events, and internal warnings or errors. 
 **Concrete example:** A disk error may appear in the kernel log during I/O failure.

---

## 3. Linux File System

### 3.1 File System Basics
File-system basics define paths, names, directories, and inode-based object identity in Linux.
They explain how hierarchical naming resolves to stored filesystem objects.

- **Definition — Files and directories:** Files store data, and directories map names to filesystem objects. 
 **Concrete example:** `/etc/passwd` is a file and `/etc` is a directory.

- **Definition — Hierarchical directory structure:** Linux organizes objects in a tree rooted at `/`. 
 **Concrete example:** `/var/log/nginx` is nested under `/var`.

- **Definition — Absolute paths:** An absolute path starts from the root directory `/`. 
 **Concrete example:** `/home/alice/.bashrc` is an absolute path.

- **Definition — Relative paths:** A relative path is resolved from the current working directory. 
 **Concrete example:** `../logs/app.log` is a relative path.

- **Definition — Inodes:** An inode stores metadata and block references for a filesystem object. 
 **Concrete example:** Two hard links can reference the same inode.

- **Definition — Directory entries:** A directory entry maps a name to an inode number inside a directory. 
 **Concrete example:** `report.txt` in `/home/alice` is a directory entry.

### 3.2 File System Hierarchy Standard
The hierarchy standard assigns conventional roles to major top-level directories in Linux.
It improves consistency in software placement, administration, and portability.

- **Definition — `/`:** `/` is the root of the entire filesystem hierarchy. 
 **Concrete example:** Every absolute path begins from `/`.

- **Definition — `/bin`:** `/bin` stores essential user commands needed for basic system operation. 
 **Concrete example:** `ls` may reside in `/bin/ls`.

- **Definition — `/sbin`:** `/sbin` stores essential system-administration binaries. 
 **Concrete example:** `fsck` may reside in `/sbin/fsck`.

- **Definition — `/etc`:** `/etc` stores system-wide configuration files. 
 **Concrete example:** `/etc/ssh/sshd_config` configures the SSH daemon.

- **Definition — `/home`:** `/home` stores regular user home directories. 
 **Concrete example:** `/home/alice` is Alice's home directory.

- **Definition — `/root`:** `/root` is the home directory of the root user. 
 **Concrete example:** Root shell history may be stored in `/root/.bash_history`.

- **Definition — `/var`:** `/var` stores variable data such as logs, spool files, and caches. 
 **Concrete example:** System logs are often stored in `/var/log`.

- **Definition — `/tmp`:** `/tmp` stores temporary files intended for short-term use. 
 **Concrete example:** Installers may unpack temporary data under `/tmp`.

- **Definition — `/usr`:** `/usr` stores user applications, libraries, and shared read-only data. 
 **Concrete example:** `/usr/bin/python3` may contain the Python interpreter.

- **Definition — `/opt`:** `/opt` stores optional third-party application packages. 
 **Concrete example:** A vendor app may install to `/opt/myapp`.

- **Definition — `/dev`:** `/dev` contains device nodes representing hardware and pseudo-devices. 
 **Concrete example:** `/dev/null` discards all written data.

- **Definition — `/proc`:** `/proc` exposes process and kernel runtime information as files. 
 **Concrete example:** `/proc/cpuinfo` shows CPU details.

- **Definition — `/sys`:** `/sys` exposes kernel objects, devices, and sysfs attributes. 
 **Concrete example:** `/sys/class/block` lists block devices.

- **Definition — `/run`:** `/run` stores volatile runtime state created after boot. 
 **Concrete example:** PID files may appear under `/run`.

- **Definition — `/mnt`:** `/mnt` is a conventional mount point for temporary manual mounts. 
 **Concrete example:** An admin may mount a disk at `/mnt/recovery`.

- **Definition — `/media`:** `/media` is commonly used for removable-media mount points. 
 **Concrete example:** A USB drive may mount under `/media/alice/USB`.

### 3.3 File Types
Linux file types define how an inode is interpreted and what operations it supports.
Different file types represent data storage, naming, devices, or interprocess communication endpoints.

- **Definition — Regular files:** Regular files store user or application data as byte streams. 
 **Concrete example:** `notes.txt` is a regular file.

- **Definition — Directories:** Directories store name-to-inode mappings for contained filesystem objects. 
 **Concrete example:** `/etc` is a directory.

- **Definition — Symbolic links:** Symbolic links store a path reference to another filesystem object. 
 **Concrete example:** `current -> /opt/app/v2` is a symlink.

- **Definition — Hard links:** Hard links create additional directory entries for the same inode. 
 **Concrete example:** `ln a.txt b.txt` makes `b.txt` a hard link to `a.txt`.

- **Definition — Character devices:** Character devices provide unbuffered sequential access to device data. 
 **Concrete example:** `/dev/tty` is a character device.

- **Definition — Block devices:** Block devices provide buffered random-access access to storage hardware. 
 **Concrete example:** `/dev/sda` is a block device.

- **Definition — Named pipes:** Named pipes are filesystem-visible FIFO IPC channels. 
 **Concrete example:** `mkfifo /tmp/feed` creates a named pipe.

- **Definition — Sockets:** Sockets are communication endpoints for local or networked IPC. 
 **Concrete example:** `/run/docker.sock` is a Unix-domain socket.

### 3.4 File Metadata
File metadata describes ownership, permissions, timestamps, size, and inode-level identity.
It controls access and provides the attributes needed for filesystem management and auditing.

- **Definition — Inode number:** The inode number uniquely identifies a filesystem object within one filesystem. 
 **Concrete example:** `ls -i file.txt` prints the inode number.

- **Definition — Link count:** The link count records how many directory entries reference an inode. 
 **Concrete example:** A file with two hard links has link count 2.

- **Definition — Ownership:** Ownership records the user ID and group ID associated with an object. 
 **Concrete example:** A web file may be owned by `www-data:www-data`.

- **Definition — Permission bits:** Permission bits encode read, write, and execute access for owner, group, and others. 
 **Concrete example:** Mode `755` grants execute access to everyone.

- **Definition — Timestamps:** Timestamps record modification, access, and metadata-change times. 
 **Concrete example:** `stat` shows `Modify` and `Access` times.

- **Definition — File size:** File size records the logical length of file data in bytes. 
 **Concrete example:** A 1 KB file has size 1024 bytes.

### 3.5 File System Operations
Filesystem operations change names, objects, and directory structure inside the namespace.
They map to kernel-managed changes in metadata and stored file data.

- **Definition — Creating files:** Creating files allocates a new inode and a directory entry for file data. 
 **Concrete example:** `touch new.txt` creates an empty file.

- **Definition — Deleting files:** Deleting files removes directory entries and releases storage when links reach zero. 
 **Concrete example:** `rm old.txt` deletes a file.

- **Definition — Copying files:** Copying files creates a new file with duplicated content from a source file. 
 **Concrete example:** `cp a.txt backup/a.txt` copies a file.

- **Definition — Moving files:** Moving files changes a file path, often by renaming its directory entry. 
 **Concrete example:** `mv report.txt archive/report.txt` moves the file.

- **Definition — Renaming files:** Renaming changes the filename or path without rewriting file contents on the same filesystem. 
 **Concrete example:** `mv draft.txt final.txt` renames a file.

- **Definition — Creating directories:** Creating directories makes a new container for child entries in the hierarchy. 
 **Concrete example:** `mkdir projects` creates a directory.

- **Definition — Removing directories:** Removing directories deletes an empty directory entry from its parent. 
 **Concrete example:** `rmdir emptydir` removes an empty directory.

### 3.6 Links
Links provide alternate naming or referencing mechanisms for filesystem objects.
They are used for sharing names, indirection, and version-switching workflows.

- **Definition — Hard links:** Hard links create multiple names for one inode within the same filesystem. 
 **Concrete example:** `ln data.log data.current` creates a second name for the same file.

- **Definition — Symbolic links:** Symbolic links store a textual pathname that resolves to another object. 
 **Concrete example:** `ln -s /var/log/app.log current.log` creates a symlink.

- **Definition — Link behavior:** Link behavior determines whether names share inode identity or resolve through indirection. 
 **Concrete example:** Deleting one hard link does not remove the data if another link remains.

- **Definition — Link limitations:** Link limitations define where and how each link type can be used. 
 **Concrete example:** Hard links usually cannot span filesystems.

- **Definition — Symlink path resolution:** Symlink resolution follows the stored pathname during filesystem lookup. 
 **Concrete example:** Accessing `current` may resolve to `/opt/app/v3`.

- **Definition — Cross-filesystem constraints:** Cross-filesystem constraints restrict operations that depend on inode identity within one filesystem. 
 **Concrete example:** A hard link from EXT4 to XFS is not allowed.

### 3.7 Mounting and Storage
Mounting attaches a filesystem instance to a directory in the global namespace.
Storage administration combines partitions, filesystems, mount points, and persistent mount configuration.

- **Definition — Partitions:** Partitions are logical subdivisions of a block device. 
 **Concrete example:** `/dev/sda1` is a partition on `/dev/sda`.

- **Definition — File systems:** A filesystem defines the on-disk data structures used to store files and metadata. 
 **Concrete example:** EXT4 is a common Linux filesystem.

- **Definition — Mount points:** A mount point is the directory where a filesystem becomes accessible. 
 **Concrete example:** `/mnt/data` can be used as a mount point.

- **Definition — `mount`:** `mount` attaches a filesystem to the directory tree. 
 **Concrete example:** `mount /dev/sdb1 /mnt/data` mounts a partition.

- **Definition — `umount`:** `umount` detaches a mounted filesystem from the directory tree. 
 **Concrete example:** `umount /mnt/data` unmounts the filesystem.

- **Definition — `/etc/fstab`:** `/etc/fstab` defines persistent filesystem mount configuration. 
 **Concrete example:** An entry can auto-mount `/dev/sdb1` at boot.

- **Definition — Automatic mounting:** Automatic mounting activates mounts on boot, access, or device insertion by policy. 
 **Concrete example:** A USB stick may be auto-mounted under `/media`.

### 3.8 Disk Usage and File System Inspection
Inspection tools expose capacity, topology, identity, and usage of filesystems and block devices.
They are used for capacity planning, troubleshooting, and validation of mount state.

- **Definition — `df`:** `df` reports filesystem-level used and available space. 
 **Concrete example:** `df -h` shows free space in human-readable units.

- **Definition — `du`:** `du` reports disk usage for files and directories. 
 **Concrete example:** `du -sh /var/log` summarizes log directory size.

- **Definition — `lsblk`:** `lsblk` displays block devices, partitions, and mount relationships. 
 **Concrete example:** `lsblk` shows `/dev/sda` and its partitions.

- **Definition — `blkid`:** `blkid` prints block-device identifiers such as UUID and filesystem type. 
 **Concrete example:** `blkid /dev/sdb1` shows the partition UUID.

- **Definition — `findmnt`:** `findmnt` shows mounted filesystems and their source and target paths. 
 **Concrete example:** `findmnt /home` shows what backs `/home`.

- **Definition — `stat`:** `stat` displays detailed metadata about a file or filesystem object. 
 **Concrete example:** `stat file.txt` prints mode, owner, and timestamps.

### 3.9 File Search and Discovery
Search tools locate filesystem objects by name, path, metadata, or indexed database state.
They support efficient discovery across large directory trees and installed software paths.

- **Definition — `find`:** `find` recursively searches directory trees using predicates such as name, type, or time. 
 **Concrete example:** `find /etc -name '*.conf'` finds config files.

- **Definition — `locate`:** `locate` searches a prebuilt filename database for fast pathname lookup. 
 **Concrete example:** `locate sshd_config` quickly finds SSH config paths.

- **Definition — `which`:** `which` shows the executable found in the current `PATH` for a command name. 
 **Concrete example:** `which python3` may print `/usr/bin/python3`.

- **Definition — `whereis`:** `whereis` locates binaries, source files, and manual pages associated with a name. 
 **Concrete example:** `whereis ls` shows binary and man page locations.

- **Definition — `updatedb`:** `updatedb` refreshes the filename database used by `locate`. 
 **Concrete example:** Running `updatedb` makes newly created files searchable via `locate`.

### 3.10 File Descriptors and Streams
File descriptors are process-local integer references to open files, pipes, sockets, or devices.
Standard streams provide default input, output, and error channels that can be redirected.

- **Definition — Standard input:** Standard input is the default input stream for a process on file descriptor 0. 
 **Concrete example:** `sort < names.txt` reads from standard input.

- **Definition — Standard output:** Standard output is the default normal-output stream on file descriptor 1. 
 **Concrete example:** `echo ok > out.txt` redirects standard output.

- **Definition — Standard error:** Standard error is the default error-output stream on file descriptor 2. 
 **Concrete example:** `cmd 2> err.log` redirects error messages.

- **Definition — File descriptor numbers:** File descriptor numbers are small integers used by a process to reference open resources. 
 **Concrete example:** File descriptor 3 can hold an extra opened file.

- **Definition — Descriptor redirection:** Descriptor redirection changes where a stream reads from or writes to. 
 **Concrete example:** `cmd >out.txt 2>&1` sends both outputs to one file.

- **Definition — Descriptor inheritance:** Descriptor inheritance allows child processes to receive selected open descriptors from a parent. 
 **Concrete example:** A shell pipeline passes pipe descriptors to both child processes.

### 3.11 Text and Binary I/O Concepts
Text and binary I/O differ in how byte streams are interpreted and processed by programs.
Shell redirection and pipeline behavior depends on whether data is line-oriented or raw bytes.

- **Definition — Character streams:** Character streams treat data as encoded text characters. 
 **Concrete example:** `grep` processes a log file as text.

- **Definition — Binary streams:** Binary streams treat data as uninterpreted byte sequences. 
 **Concrete example:** `cat image.png > copy.png` preserves binary bytes.

- **Definition — Buffering:** Buffering temporarily stores I/O in memory to reduce system-call overhead. 
 **Concrete example:** Output may appear late when a program uses block buffering.

- **Definition — Redirection targets:** Redirection targets are files, devices, or descriptors that receive redirected I/O. 
 **Concrete example:** `> /dev/null` discards output.

- **Definition — Here documents:** Here documents embed multiline input directly in shell syntax. 
 **Concrete example:** `cat <<EOF... EOF` feeds text to a command.

### 3.12 File System Advanced Concepts
Advanced filesystem concepts address reliability, performance, abstraction, and scalable storage layout.
They include journaling, modern filesystems, volume management, redundancy, and swap integration.

- **Definition — Journaling:** Journaling logs updates to support faster and safer crash recovery. 
 **Concrete example:** EXT4 replays journal entries after power loss.

- **Definition — EXT4:** EXT4 is a general-purpose Linux filesystem with journaling and mature tooling. 
 **Concrete example:** Many Linux distributions use EXT4 for the root partition.

- **Definition — XFS:** XFS is a high-performance journaling filesystem optimized for scalability and large filesystems. 
 **Concrete example:** Enterprise servers often format data volumes with XFS.

- **Definition — Btrfs:** Btrfs is a Linux filesystem with snapshots, checksums, and copy-on-write semantics. 
 **Concrete example:** Btrfs can create a snapshot before a risky system update.

- **Definition — ZFS concepts:** ZFS concepts include pooled storage, checksumming, snapshots, and copy-on-write data integrity. 
 **Concrete example:** A ZFS pool can aggregate multiple disks into one storage pool.

- **Definition — Swap:** Swap is disk-backed space used to extend memory availability under pressure. 
 **Concrete example:** A system may use a swap partition when RAM is low.

- **Definition — RAID:** RAID combines multiple disks for redundancy, performance, or both. 
 **Concrete example:** RAID1 mirrors data on two disks.

- **Definition — LVM:** LVM abstracts physical storage into flexible logical volumes. 
 **Concrete example:** An admin can extend a logical volume without changing mount points.

---

## 4. Linux Shell and Command Line

### 4.1 Shell Basics
Shell basics define the shell as a user-space command interpreter and control environment.
They explain how commands are read, parsed, and executed interactively or from scripts.

- **Definition — What a shell is:** A shell is a program that interprets commands and launches other programs. 
 **Concrete example:** Bash reads a command line and executes `grep`.

- **Definition — Command interpretation:** Command interpretation tokenizes input, expands syntax, and prepares program execution. 
 **Concrete example:** The shell expands `*.log` before calling `rm`.

- **Definition — Interactive shell:** An interactive shell reads commands directly from a terminal session. 
 **Concrete example:** A user typing commands in an SSH session is using an interactive shell.

- **Definition — Non-interactive shell:** A non-interactive shell runs commands from a script or redirected input. 
 **Concrete example:** `bash deploy.sh` runs a non-interactive shell session.

- **Definition — Login shell:** A login shell initializes the environment for a user session using login startup files. 
 **Concrete example:** SSH usually starts a login shell for the user.

- **Definition — Subshell:** A subshell is a child shell process created by a parent shell for isolated execution. 
 **Concrete example:** Commands inside `(cd /tmp; ls)` run in a subshell.

### 4.2 Common Shells
Common shells provide similar command execution semantics but differ in syntax features and user experience.
Administrators standardize on shells because startup behavior and scripting details can vary.

- **Definition — `bash`:** Bash is a widely used GNU shell for interactive work and scripting. 
 **Concrete example:** Most Linux tutorials assume Bash syntax.

- **Definition — `sh`:** `sh` refers to the traditional POSIX-style shell interface or a shell compatibility mode. 
 **Concrete example:** Many portable scripts start with `#!/bin/sh`.

- **Definition — `zsh`:** Zsh is an advanced shell with rich completion and interactive customization features. 
 **Concrete example:** Developers often use Zsh with enhanced prompts.

- **Definition — `ksh`:** Korn shell is a Unix shell known for scripting features and POSIX influence. 
 **Concrete example:** Legacy enterprise scripts may target `ksh`.

- **Definition — `fish`:** Fish is a user-friendly shell with syntax and completion behavior distinct from POSIX shells. 
 **Concrete example:** Fish offers autosuggestions as the user types.

### 4.3 Command Syntax
Command syntax defines the structure of command names, options, arguments, and quoting.
Correct syntax determines how the shell builds the final argument vector for execution.

- **Definition — Command name:** The command name identifies the program or shell builtin to execute. 
 **Concrete example:** In `ls -l`, `ls` is the command name.

- **Definition — Options and flags:** Options and flags modify command behavior using prefixed tokens. 
 **Concrete example:** `-l` asks `ls` for long listing format.

- **Definition — Arguments:** Arguments supply input values such as filenames, patterns, or destinations. 
 **Concrete example:** In `cat file.txt`, `file.txt` is an argument.

- **Definition — Quoting:** Quoting preserves literal characters and suppresses unwanted shell expansion. 
 **Concrete example:** `'*.txt'` prevents glob expansion.

- **Definition — Escaping:** Escaping removes special meaning from the next character. 
 **Concrete example:** `\ ` escapes a space in a filename.

- **Definition — Exit status:** Exit status is the numeric code a command returns to indicate success or failure. 
 **Concrete example:** `0` usually means success.

### 4.4 Shell Features
Shell features let users compose commands, redirect data, and reuse command history efficiently.
They enable expressive command-line workflows without writing full standalone programs.

- **Definition — Wildcards and globbing:** Globbing expands filename patterns into matching pathnames before execution. 
 **Concrete example:** `ls *.log` lists all `.log` files.

- **Definition — Redirection:** Redirection reroutes a command's standard streams to files or descriptors. 
 **Concrete example:** `echo hi > out.txt` writes output to a file.

- **Definition — Pipes:** Pipes connect one command's output to another command's input through the kernel pipe mechanism. 
 **Concrete example:** `ps aux | grep nginx` filters process output.

- **Definition — Command substitution:** Command substitution replaces syntax with the output of another command. 
 **Concrete example:** `echo $(date)` inserts the current date output.

- **Definition — History:** History records previously executed commands for later recall and reuse. 
 **Concrete example:** Pressing the up arrow recalls prior commands in Bash.

- **Definition — Aliases:** Aliases define shorthand replacements for command strings in the shell. 
 **Concrete example:** `alias ll='ls -lah'` creates a shortcut.

- **Definition — Functions:** Shell functions define reusable shell command blocks with a callable name. 
 **Concrete example:** A function can wrap `tar` backup steps into one command.

### 4.5 Environment and Variables
Shell variables and environment variables carry configuration and state through shell execution.
Startup files initialize these variables to shape command lookup and program behavior.

- **Definition — Shell variables:** Shell variables store values local to the shell unless exported. 
 **Concrete example:** `name=alice` defines a shell variable.

- **Definition — Environment variables:** Environment variables are inherited by child processes from the current shell. 
 **Concrete example:** `EDITOR=vim` can affect application editor selection.

- **Definition — `PATH`:** `PATH` lists directories searched for executable command names. 
 **Concrete example:** `/usr/bin` is commonly in `PATH`.

- **Definition — Exporting variables:** Exporting marks a shell variable for inheritance by child processes. 
 **Concrete example:** `export PATH` makes the variable visible to child commands.

- **Definition — Startup files:** Startup files are shell-specific configuration scripts read during shell initialization. 
 **Concrete example:** Bash may read `~/.bashrc`.

- **Definition — Shell initialization order:** Initialization order defines which startup files are loaded for login and non-login shells. 
 **Concrete example:** Bash may read `/etc/profile` before `~/.bash_profile` in a login shell.

### 4.6 Shell Expansion
Shell expansion transforms command text into final arguments before program execution.
Its ordered stages affect quoting, splitting, and the resulting command behavior.

- **Definition — Brace expansion:** Brace expansion generates multiple strings from a compact brace pattern. 
 **Concrete example:** `echo file{1,2}.txt` expands to two names.

- **Definition — Tilde expansion:** Tilde expansion replaces `~` with a home directory path. 
 **Concrete example:** `cd ~/src` changes into a directory under the user's home.

- **Definition — Parameter expansion:** Parameter expansion substitutes variable values into command text. 
 **Concrete example:** `echo $HOME` prints the home directory.

- **Definition — Arithmetic expansion:** Arithmetic expansion evaluates integer expressions inside shell syntax. 
 **Concrete example:** `echo $((2+3))` prints `5`.

- **Definition — Command substitution:** Command substitution inserts command output into the surrounding command line. 
 **Concrete example:** `files=$(ls /tmp)` stores output in a variable.

- **Definition — Word splitting:** Word splitting breaks expanded text into arguments using delimiter rules such as `IFS`. 
 **Concrete example:** An unquoted variable containing spaces may become multiple arguments.

---

## 5. Basic Linux Commands

### 5.1 Navigation Commands
Navigation commands inspect the current location in the filesystem and change directory context.
They are foundational because many command operations are relative to the working directory.

- **Definition — `pwd`:** `pwd` prints the current working directory. 
 **Concrete example:** `pwd` may output `/home/alice/projects`.

- **Definition — `ls`:** `ls` lists directory contents. 
 **Concrete example:** `ls /etc` shows files under `/etc`.

- **Definition — `cd`:** `cd` changes the current working directory of the shell. 
 **Concrete example:** `cd /var/log` moves into the log directory.

- **Definition — `tree`:** `tree` displays a recursive directory hierarchy in tree form. 
 **Concrete example:** `tree src` prints nested source directories.

### 5.2 File and Directory Commands
These commands create, remove, rename, and link filesystem objects in daily administration.
They operate on names and directories while the kernel enforces metadata and permissions.

- **Definition — `touch`:** `touch` creates an empty file or updates file timestamps. 
 **Concrete example:** `touch notes.txt` creates a new file if it does not exist.

- **Definition — `cp`:** `cp` copies files or directories to a new destination. 
 **Concrete example:** `cp a.txt backup/a.txt` copies a file.

- **Definition — `mv`:** `mv` moves or renames files and directories. 
 **Concrete example:** `mv old.conf new.conf` renames a file.

- **Definition — `rm`:** `rm` removes files, and with options can remove directories recursively. 
 **Concrete example:** `rm old.log` deletes a file.

- **Definition — `mkdir`:** `mkdir` creates one or more directories. 
 **Concrete example:** `mkdir -p app/config` creates nested directories.

- **Definition — `rmdir`:** `rmdir` removes empty directories. 
 **Concrete example:** `rmdir emptydir` removes an empty directory.

- **Definition — `ln`:** `ln` creates hard links or symbolic links. 
 **Concrete example:** `ln -s /opt/app/current app-link` creates a symlink.

### 5.3 Viewing File Content
Viewing commands display file contents directly, partially, or through a pager for inspection.
They are used heavily for logs, configuration files, and textual data validation.

- **Definition — `cat`:** `cat` prints file contents to standard output. 
 **Concrete example:** `cat /etc/hostname` shows the system hostname file.

- **Definition — `less`:** `less` views long text interactively with scrolling and searching. 
 **Concrete example:** `less /var/log/syslog` opens a log pager.

- **Definition — `more`:** `more` displays text page by page in a simpler pager interface. 
 **Concrete example:** `more readme.txt` paginates a text file.

- **Definition — `head`:** `head` prints the first lines of a file. 
 **Concrete example:** `head -n 5 access.log` shows the first five lines.

- **Definition — `tail`:** `tail` prints the last lines of a file and can follow appended output. 
 **Concrete example:** `tail -f app.log` streams new log lines.

- **Definition — `nl`:** `nl` prints file contents with line numbers. 
 **Concrete example:** `nl script.sh` shows numbered script lines.

### 5.4 Text Processing Commands
Text-processing commands transform, search, sort, and summarize textual streams efficiently.
They are designed to compose well in pipelines and shell scripts.

- **Definition — `echo`:** `echo` prints arguments to standard output. 
 **Concrete example:** `echo hello` prints `hello`.

- **Definition — `grep`:** `grep` searches text for lines matching a pattern. 
 **Concrete example:** `grep ERROR app.log` finds error lines.

- **Definition — `sort`:** `sort` orders input lines according to lexical or numeric rules. 
 **Concrete example:** `sort names.txt` sorts names alphabetically.

- **Definition — `uniq`:** `uniq` filters or counts adjacent duplicate lines. 
 **Concrete example:** `sort names.txt | uniq` removes duplicates.

- **Definition — `cut`:** `cut` extracts fixed-position or delimiter-separated fields from lines. 
 **Concrete example:** `cut -d: -f1 /etc/passwd` prints usernames.

- **Definition — `tr`:** `tr` translates or deletes characters from input streams. 
 **Concrete example:** `tr a-z A-Z < file.txt` uppercases text.

- **Definition — `wc`:** `wc` counts lines, words, and bytes in input. 
 **Concrete example:** `wc -l app.log` counts lines.

- **Definition — `awk`:** `awk` processes text by records and fields using pattern-action rules. 
 **Concrete example:** `awk -F: '{print \$1}' /etc/passwd` prints usernames.

- **Definition — `sed`:** `sed` performs stream editing such as substitution and line selection. 
 **Concrete example:** `sed 's/foo/bar/g' file.txt` replaces text globally.

### 5.5 Search and Pattern Commands
Search commands locate files or matching content using recursive traversal and pattern matching.
They help administrators and scripts find specific objects or text quickly.

- **Definition — `find`:** `find` traverses directories and selects entries using tests and actions. 
 **Concrete example:** `find /var/log -name '*.gz'` finds compressed logs.

- **Definition — `locate`:** `locate` performs fast path lookup using a database of filenames. 
 **Concrete example:** `locate hosts` finds files named `hosts`.

- **Definition — `grep`:** `grep` matches line content against regular-expression patterns. 
 **Concrete example:** `grep -R root /etc` searches recursively for `root`.

- **Definition — `egrep`:** `egrep` uses extended regular expressions for more expressive pattern matching. 
 **Concrete example:** `egrep 'error|fail' app.log` matches either word.

- **Definition — `xargs`:** `xargs` builds command arguments from standard input. 
 **Concrete example:** `find. -name '*.tmp' | xargs rm` removes found temp files.

### 5.6 Archive and Copy Commands
Archive and copy commands package, compress, and synchronize data for transfer or backup.
They preserve structure and often preserve metadata needed for restoration.

- **Definition — `tar`:** `tar` creates, extracts, and lists archive files. 
 **Concrete example:** `tar -cf backup.tar /etc` archives `/etc`.

- **Definition — `gzip`:** `gzip` compresses files using the gzip format. 
 **Concrete example:** `gzip backup.tar` creates `backup.tar.gz`.

- **Definition — `bzip2`:** `bzip2` compresses files with a higher-ratio algorithm than gzip. 
 **Concrete example:** `bzip2 large.log` creates `large.log.bz2`.

- **Definition — `xz`:** `xz` compresses files with high compression efficiency. 
 **Concrete example:** `xz image.iso` creates `image.iso.xz`.

- **Definition — `zip`:** `zip` creates ZIP archives often used for cross-platform exchange. 
 **Concrete example:** `zip docs.zip *.txt` bundles text files.

- **Definition — `unzip`:** `unzip` extracts files from ZIP archives. 
 **Concrete example:** `unzip docs.zip` extracts archive contents.

- **Definition — `rsync`:** `rsync` synchronizes files efficiently by transferring only detected changes. 
 **Concrete example:** `rsync -a src/ backup/` mirrors a directory tree.

### 5.7 System Information Commands
System-information commands reveal host identity, time, environment, and account context.
They are often used to verify assumptions before modifying a system.

- **Definition — `date`:** `date` prints or sets the system date and time. 
 **Concrete example:** `date` prints the current timestamp.

- **Definition — `cal`:** `cal` displays a formatted calendar. 
 **Concrete example:** `cal 10 2026` shows October 2026.

- **Definition — `whoami`:** `whoami` prints the current effective username. 
 **Concrete example:** `whoami` may print `root`.

- **Definition — `id`:** `id` prints user and group identity information. 
 **Concrete example:** `id alice` shows Alice's UID and groups.

- **Definition — `hostname`:** `hostname` prints or sets the system hostname. 
 **Concrete example:** `hostname` may print `web01`.

- **Definition — `uptime`:** `uptime` shows how long the system has been running and its load averages. 
 **Concrete example:** `uptime` prints `up 12 days`.

- **Definition — `uname`:** `uname` prints kernel and system identification information. 
 **Concrete example:** `uname -m` prints the machine architecture.

- **Definition — `env`:** `env` prints environment variables or runs a command with a modified environment. 
 **Concrete example:** `env | grep PATH` shows the PATH variable.

### 5.8 Help and Documentation
Documentation commands expose built-in help and installed manual resources on the system.
They are the primary source for syntax and option details during administration.

- **Definition — `man`:** `man` opens the manual page for a command or topic. 
 **Concrete example:** `man chmod` explains mode changes.

- **Definition — `info`:** `info` reads GNU info documentation in hypertext-style format. 
 **Concrete example:** `info coreutils` opens GNU core utilities docs.

- **Definition — `--help`:** `--help` prints built-in command usage and available options. 
 **Concrete example:** `grep --help` shows grep options.

- **Definition — `whatis`:** `whatis` prints a one-line manual-page description for a name. 
 **Concrete example:** `whatis tar` shows a short tar summary.

- **Definition — `apropos`:** `apropos` searches manual page descriptions by keyword. 
 **Concrete example:** `apropos password` finds related commands and APIs.

---

## 6. File Permissions and Ownership

### 6.1 Permission Model
The Linux permission model implements discretionary access control using ownership and mode bits.
It applies separate read, write, and execute rules to user, group, and others classes.

- **Definition — Read permission:** Read permission allows viewing file contents or listing directory entries. 
 **Concrete example:** A file mode with `r--` allows `cat file.txt`.

- **Definition — Write permission:** Write permission allows modifying file contents or directory entries. 
 **Concrete example:** A writable directory allows creating new files inside it.

- **Definition — Execute permission:** Execute permission allows running a file as a program or traversing a directory. 
 **Concrete example:** A script needs execute permission to run as `./script.sh`.

- **Definition — User, group, other classes:** Permission classes define access for owner, owning group, and everyone else. 
 **Concrete example:** `rwxr-x---` grants different access to each class.

- **Definition — Ownership model:** The ownership model assigns each object one owner UID and one owning GID. 
 **Concrete example:** A file owned by `alice:dev` uses Alice as owner and `dev` as group.

### 6.2 Permission Semantics
Permission semantics differ between regular files and directories in important ways.
Understanding these semantics is critical for access control and troubleshooting.

- **Definition — File read semantics:** File read permission allows reading the file's stored data. 
 **Concrete example:** A user with read permission can run `cat report.txt`.

- **Definition — File write semantics:** File write permission allows modifying file contents. 
 **Concrete example:** A writable config file can be edited with `vim`.

- **Definition — File execute semantics:** File execute permission allows the kernel to execute the file as a program. 
 **Concrete example:** `chmod +x deploy.sh` allows execution of the script.

- **Definition — Directory read semantics:** Directory read permission allows listing entry names within the directory. 
 **Concrete example:** `ls dir` needs read permission on `dir`.

- **Definition — Directory write semantics:** Directory write permission allows adding, removing, or renaming entries inside the directory. 
 **Concrete example:** Creating `dir/new.txt` needs directory write permission.

- **Definition — Directory execute semantics:** Directory execute permission allows traversal and access to entries by name. 
 **Concrete example:** `cd dir` requires execute permission on `dir`.

### 6.3 Viewing Permissions
Viewing permissions reveals effective mode bits, ownership, and related file metadata.
These views are used to diagnose access failures and validate system state.

- **Definition — `ls -l`:** `ls -l` shows file type, permissions, ownership, size, and timestamp. 
 **Concrete example:** `ls -l script.sh` may show `-rwxr-xr-x`.

- **Definition — Symbolic permission representation:** Symbolic representation encodes permissions as `r`, `w`, `x`, and special-bit characters. 
 **Concrete example:** `drwxr-xr-x` describes a directory with mode 755.

- **Definition — Numeric permission representation:** Numeric representation encodes mode bits using octal digits. 
 **Concrete example:** `644` means owner read-write and others read-only.

- **Definition — Metadata inspection with `stat`:** `stat` prints detailed metadata including numeric mode, owner, and timestamps. 
 **Concrete example:** `stat file.txt` shows `Access: (0644/-rw-r--r--)`.

### 6.4 Changing Permissions
Changing permissions modifies file mode bits to alter allowed operations.
Linux supports symbolic and octal notation for precise mode changes.

- **Definition — `chmod`:** `chmod` changes permission bits on files and directories. 
 **Concrete example:** `chmod 600 secrets.txt` restricts access to the owner.

- **Definition — Symbolic mode:** Symbolic mode changes permissions by class and operation such as add or remove. 
 **Concrete example:** `chmod g+w file.txt` adds group write permission.

- **Definition — Numeric mode:** Numeric mode sets permissions directly using octal digits. 
 **Concrete example:** `chmod 755 run.sh` sets standard executable permissions.

- **Definition — Recursive permission changes:** Recursive changes apply permission updates through a directory tree. 
 **Concrete example:** `chmod -R o-rwx private/` removes public access recursively.

### 6.5 Ownership Management
Ownership management changes the user and group associated with filesystem objects.
It directly affects access resolution under the standard permission model.

- **Definition — `chown`:** `chown` changes the owning user and optionally group of a file. 
 **Concrete example:** `chown alice file.txt` makes Alice the owner.

- **Definition — `chgrp`:** `chgrp` changes the owning group of a file. 
 **Concrete example:** `chgrp dev shared.txt` assigns the file to group `dev`.

- **Definition — User ownership:** User ownership identifies the owner whose user-mode bits apply first. 
 **Concrete example:** If Alice owns a file, her user bits are evaluated for access.

- **Definition — Group ownership:** Group ownership identifies the group whose mode bits apply to matching group members. 
 **Concrete example:** A user in group `dev` gains the group's file permissions.

- **Definition — Recursive ownership changes:** Recursive ownership changes apply through all descendants of a directory tree. 
 **Concrete example:** `chown -R www-data:www-data /var/www` fixes web-root ownership.

### 6.6 Special Permissions
Special permissions extend standard mode bits with privilege and deletion semantics.
They are important for shared directories and controlled privilege inheritance.

- **Definition — setuid:** setuid makes an executable run with the file owner's effective UID. 
 **Concrete example:** `/usr/bin/passwd` often uses setuid root.

- **Definition — setgid:** setgid makes an executable run with the file group's effective GID or causes directories to inherit group ownership. 
 **Concrete example:** A shared project directory with setgid keeps new files in the same group.

- **Definition — sticky bit:** The sticky bit on a directory restricts file deletion to owners, directory owner, or root. 
 **Concrete example:** `/tmp` uses the sticky bit to protect user files.

- **Definition — Special bits on directories:** Special directory bits affect group inheritance and deletion control in shared spaces. 
 **Concrete example:** A setgid directory causes new files to inherit the directory group.

### 6.7 Default Permissions
Default permissions are determined by creation mode and the process umask.
They prevent new files and directories from starting with overly permissive access.

- **Definition — `umask`:** `umask` masks permission bits from default creation modes. 
 **Concrete example:** `umask 022` removes group and other write permission.

- **Definition — Permission calculation:** Final default permissions are derived by subtracting the umask from base creation modes. 
 **Concrete example:** A new regular file often becomes `644` under umask `022`.

- **Definition — Default creation modes:** Programs start from default creation modes before umask is applied. 
 **Concrete example:** A regular file commonly starts from mode `666` before masking.

### 6.8 Access Control Lists
ACLs extend the basic permission model with extra per-user and per-group rules.
They are useful when access needs exceed one owner and one group.

- **Definition — ACL concepts:** ACLs attach fine-grained access entries to filesystem objects. 
 **Concrete example:** A file can grant read access to one extra user.

- **Definition — `getfacl`:** `getfacl` displays ACL entries for files and directories. 
 **Concrete example:** `getfacl shared.txt` shows extra access entries.

- **Definition — `setfacl`:** `setfacl` creates or modifies ACL entries on filesystem objects. 
 **Concrete example:** `setfacl -m u:alice:r file.txt` grants Alice read access.

- **Definition — Default ACLs:** Default ACLs on directories define inherited ACL entries for newly created children. 
 **Concrete example:** Files created in a shared directory can automatically inherit group ACL entries.

### 6.9 Linux Capabilities
Linux capabilities split root privilege into smaller privilege units.
They enable fine-grained privilege delegation without full superuser rights.

- **Definition — Capability model:** The capability model decomposes privileged operations into independently assignable kernel capabilities. 
 **Concrete example:** `CAP_NET_BIND_SERVICE` allows binding to low ports.

- **Definition — `setcap`:** `setcap` assigns file capabilities to an executable. 
 **Concrete example:** `setcap cap_net_bind_service=+ep /usr/local/bin/app` permits port 80 binding.

- **Definition — `getcap`:** `getcap` displays file capabilities assigned to executables. 
 **Concrete example:** `getcap /usr/local/bin/app` shows its configured capability.

- **Definition — Fine-grained privilege delegation:** Fine-grained delegation grants only the privileges a program needs rather than full root power. 
 **Concrete example:** A backup tool may get filesystem-read capability but not full root shell access.

### 6.10 Symlink and Permission Edge Cases
Symlink behavior differs from regular files in access checks and path resolution.
These cases matter for secure traversal, deletion, and pathname handling.

- **Definition — Symlink permission behavior:** Symlink mode bits are generally ignored during access checks on Linux. 
 **Concrete example:** `lrwxrwxrwx` appears for most symlinks regardless of effective access.

- **Definition — Target permission enforcement:** Access to a symlink target is checked against the target object's permissions. 
 **Concrete example:** A symlink to `/etc/shadow` does not bypass the file's access controls.

- **Definition — Traversal checks:** Path traversal through directories still requires execute permission on each directory component. 
 **Concrete example:** A readable target is inaccessible if the parent directory lacks execute permission.

- **Definition — Ownership implications:** Symlink ownership can matter for some administrative operations even if target access checks use target metadata. 
 **Concrete example:** Ownership may matter when cleaning up links in shared writable directories.

... 
