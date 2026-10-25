# Linux Subtopics and Bullet-Level Subpoints

## 1. Introduction to Linux

### 1.1 What is Linux
Linux is an open-source operating system ecosystem built around the Linux kernel.
It provides process, memory, file, device, and network abstractions to user-space software.

- **Linux kernel:** The Linux kernel is the core software layer that manages hardware and system resources.  
  **Concrete example:** Ubuntu uses the Linux kernel to schedule processes and manage memory.

- **Open-source model:** Linux source code is publicly available to inspect, modify, and redistribute under open licenses.  
  **Concrete example:** A company can customize Linux for an embedded router.

- **Unix-like design:** Linux follows Unix-style concepts such as hierarchical filesystems, processes, shells, and permissions.  
  **Concrete example:** Linux uses paths like `/etc/passwd` and commands like `ls` and `grep`.

- **Multiuser support:** Linux allows multiple users to use the same system with separate identities and permissions.  
  **Concrete example:** Two administrators can log in to the same server through SSH.

- **Multitasking support:** Linux can run many processes concurrently under kernel scheduling control.  
  **Concrete example:** A web server, database, and backup job can run at the same time.

- **Hardware portability:** Linux is designed to run on many processor architectures with platform-specific support layers.  
  **Concrete example:** Linux runs on x86_64 servers, ARM boards, and RISC-V systems.

### 1.2 Linux History
Linux history covers the development of the Linux kernel and its pairing with GNU user-space software.
It explains how Linux evolved from a personal project into a major server, desktop, and embedded platform.

- **GNU project background:** The GNU project provided free system tools and utilities that complemented the Linux kernel.  
  **Concrete example:** GNU Bash and GCC are standard components on many Linux systems.

- **Linux kernel origin:** The Linux kernel began as a Unix-like kernel project and later became a global open-source effort.  
  **Concrete example:** Linus Torvalds announced the first Linux kernel in 1991.

- **Linus Torvalds' role:** Linus Torvalds started Linux and remains a central maintainer of the upstream kernel.  
  **Concrete example:** Kernel patches are merged through the Linux development process he oversees.

- **Distribution evolution:** Linux distributions combined the kernel with package managers, libraries, and admin tools.  
  **Concrete example:** Debian and Red Hat package Linux into complete operating systems.

- **Community adoption:** Linux grew through contributions from volunteers, researchers, and companies.  
  **Concrete example:** Thousands of developers contribute fixes and features to the kernel.

- **Enterprise adoption:** Linux became a standard platform for servers, cloud systems, and infrastructure products.  
  **Concrete example:** Major cloud providers run Linux extensively in production environments.

### 1.3 Linux Distributions
A Linux distribution packages the kernel with utilities, libraries, package tools, and default configuration.
Distributions differ in package formats, release cycles, support policies, and administration workflows.

- **Debian-based distributions:** Debian-based systems use `.deb` packages and APT-based package management.  
  **Concrete example:** Ubuntu installs software with `apt install nginx`.

- **Red Hat-based distributions:** Red Hat-based systems use `.rpm` packages and DNF or YUM workflows.  
  **Concrete example:** Fedora installs software with `dnf install nginx`.

- **Arch-based distributions:** Arch-based systems emphasize minimalism and rolling-release updates.  
  **Concrete example:** Arch Linux installs packages with `pacman -S vim`.

- **SUSE-based distributions:** SUSE-based systems use RPM packages with SUSE-specific tooling and policies.  
  **Concrete example:** openSUSE commonly uses `zypper` for package management.

- **Source-based distributions:** Source-based distributions build software from source code rather than only using binaries.  
  **Concrete example:** Gentoo compiles packages through Portage.

- **Specialized distributions:** Specialized distributions target appliances, security work, embedded devices, or recovery tasks.  
  **Concrete example:** OpenWrt is designed for network routers.

### 1.4 Linux Architecture Overview
Linux architecture separates privileged kernel space from unprivileged user space.
Applications use libraries and system calls to request controlled kernel services.

- **Kernel space:** Kernel space is the privileged execution area where the kernel and core drivers run.  
  **Concrete example:** The scheduler executes in kernel space.

- **User space:** User space is the execution area where applications, shells, and utilities run with limited privilege.  
  **Concrete example:** The `vim` editor runs in user space.

- **Shell:** A shell is a command interpreter that reads input and launches programs.  
  **Concrete example:** Bash runs `ls -l /var/log` after parsing the command line.

- **System libraries:** System libraries provide reusable programming interfaces to common OS services.  
  **Concrete example:** glibc provides functions such as `open()` and `printf()`.

- **System utilities:** System utilities are standard tools used to inspect and manage the operating system.  
  **Concrete example:** `ps`, `mount`, and `cp` are Linux system utilities.

- **Init system:** The init system starts and supervises user-space services after boot.  
  **Concrete example:** systemd starts `sshd` during system startup.

## 2. Linux Kernel

### 2.1 Kernel Fundamentals
Kernel fundamentals define the kernel as the privileged software layer that controls hardware and core system behavior.
They explain protection, scheduling, memory control, interrupt handling, and process isolation.

- **Kernel core:** The kernel is the central operating system component that mediates access to hardware.  
  **Concrete example:** The kernel decides which process gets CPU time next.

- **Monolithic design:** Linux uses a mostly monolithic design where core services run in one privileged address space.  
  **Concrete example:** The VFS and network stack both run inside the kernel.

- **Modular extension:** Linux can extend kernel functionality through dynamically loadable modules.  
  **Concrete example:** A NIC driver can be loaded without rebuilding the kernel.

- **Resource management:** The kernel manages CPU, memory, devices, filesystems, and process execution.  
  **Concrete example:** It maps virtual memory for each running process.

- **Privilege boundary:** Kernel code runs with hardware privileges that user-space programs do not have.  
  **Concrete example:** A text editor cannot directly modify page tables.

### 2.2 Kernel Components
Kernel components are specialized subsystems that implement specific operating-system functions.
They cooperate through internal interfaces to deliver process, memory, storage, and network behavior.

- **Process scheduler:** The scheduler selects runnable tasks and allocates CPU time among them.  
  **Concrete example:** The Completely Fair Scheduler balances CPU access across processes.

- **Memory manager:** The memory manager handles virtual memory, paging, and physical page allocation.  
  **Concrete example:** It maps a process heap into memory pages.

- **Device drivers:** Device drivers translate kernel requests into hardware-specific operations.  
  **Concrete example:** A disk driver issues read requests to a storage controller.

- **Virtual file system:** The VFS provides a common interface for multiple filesystem types.  
  **Concrete example:** `open()` works on both EXT4 and XFS through the VFS layer.

- **Network stack:** The network stack handles packet processing, sockets, and protocol implementation.  
  **Concrete example:** TCP connections are maintained by the kernel network stack.

- **System call interface:** The system call interface exposes controlled kernel services to user programs.  
  **Concrete example:** `read()` enters the kernel through the syscall mechanism.

- **IPC subsystem:** IPC mechanisms let processes exchange data and synchronize safely.  
  **Concrete example:** Two processes can communicate through a pipe.

### 2.3 Kernel Modules
Kernel modules are binary components that can be loaded into or removed from the running kernel.
They are commonly used for drivers, filesystems, and optional protocol support.

- **Loadable modules:** A loadable module is a kernel extension inserted at runtime.  
  **Concrete example:** `xfs.ko` adds support for the XFS filesystem.

- **Module insertion:** Module insertion loads a compiled module into the running kernel.  
  **Concrete example:** `modprobe loop` loads the loop device module.

- **Module removal:** Module removal unloads a module when it is no longer in use.  
  **Concrete example:** `modprobe -r loop` removes the loop module.

- **Dependency handling:** Module tools resolve prerequisite modules before loading a target module.  
  **Concrete example:** `modprobe` automatically loads helper crypto modules.

- **Module metadata:** Module metadata records details such as version, author, and aliases.  
  **Concrete example:** `modinfo e1000e` prints driver information.

- **Persistent configuration:** Persistent module configuration controls automatic loading and module options.  
  **Concrete example:** A file in `/etc/modprobe.d/` can blacklist a module.

### 2.4 System Calls
System calls are controlled transitions from user mode into kernel mode for privileged services.
They define the low-level interface for files, processes, memory, and networking.

- **Privilege transition:** A system call switches execution from user space to kernel space safely.  
  **Concrete example:** `write()` traps into the kernel to send data to a file.

- **Common syscalls:** Common syscalls implement core operating-system operations used by most programs.  
  **Concrete example:** `close()` releases an open file descriptor.

- **File syscalls:** File-related syscalls manage files, directories, and metadata.  
  **Concrete example:** `stat()` retrieves file attributes from the kernel.

- **Process syscalls:** Process-related syscalls create, replace, and control processes.  
  **Concrete example:** `fork()` creates a child process.

- **Memory syscalls:** Memory-related syscalls map and protect regions in a process address space.  
  **Concrete example:** `mmap()` maps a file into memory.

- **Networking syscalls:** Networking-related syscalls create sockets and transfer network data.  
  **Concrete example:** `socket()` creates a socket endpoint.

### 2.5 Boot and Kernel Initialization
Boot and kernel initialization describe the path from firmware handoff to the first user-space process.
The kernel initializes memory, drivers, hardware state, and root filesystem access during this sequence.

- **Firmware stage:** Firmware performs early hardware initialization and locates boot code.  
  **Concrete example:** UEFI loads a bootloader from the EFI partition.

- **Bootloader stage:** The bootloader loads the kernel and initramfs and passes boot parameters.  
  **Concrete example:** GRUB loads `vmlinuz` and an initramfs image.

- **Kernel loading:** Kernel loading places the kernel image into memory and transfers control to it.  
  **Concrete example:** GRUB hands execution to the Linux kernel entry point.

- **Initramfs stage:** initramfs provides early userspace tools needed to access the real root filesystem.  
  **Concrete example:** It can load drivers needed for an LVM root volume.

- **Early userspace:** Early userspace performs setup before the final root filesystem is mounted.  
  **Concrete example:** It can assemble RAID devices before continuing boot.

- **Init startup:** The init system becomes PID 1 and starts normal system services.  
  **Concrete example:** systemd starts logging and networking services after boot.

### 2.6 Kernel Information and Management
Kernel information interfaces expose runtime details, logs, tunables, and object state.
Administrators use them to inspect behavior and adjust kernel parameters.

- **`uname`:** `uname` reports kernel identity details such as release and architecture.  
  **Concrete example:** `uname -r` prints the running kernel version.

- **`/proc`:** `/proc` is a pseudo-filesystem that exposes process and kernel runtime data.  
  **Concrete example:** `/proc/meminfo` shows memory statistics.

- **`/sys`:** `/sys` is a pseudo-filesystem that exposes devices and kernel object state.  
  **Concrete example:** `/sys/class/net` lists detected network interfaces.

- **`dmesg`:** `dmesg` displays messages from the kernel ring buffer.  
  **Concrete example:** `dmesg | grep usb` shows USB device events.

- **Kernel parameters:** Kernel parameters modify kernel behavior at boot or runtime.  
  **Concrete example:** The `quiet` boot parameter reduces boot output.

- **`sysctl`:** `sysctl` reads and writes tunables under `/proc/sys`.  
  **Concrete example:** `sysctl net.ipv4.ip_forward=1` enables IP forwarding.

### 2.7 Kernel Scheduling and Timing
Kernel scheduling and timing define how tasks are selected, preempted, and measured over time.
These mechanisms combine scheduler policies, timers, and context switching.

- **Scheduler classes:** Scheduler classes define different task-selection policies.  
  **Concrete example:** `SCHED_FIFO` is used for real-time workloads.

- **Time slices:** A time slice is the CPU budget given to a task before rescheduling decisions.  
  **Concrete example:** Interactive tasks often receive short CPU bursts.

- **Context switching:** Context switching saves one task state and restores another on a CPU.  
  **Concrete example:** The CPU switches from a shell to a web server worker.

- **Preemption:** Preemption interrupts a running task so a more eligible task can run.  
  **Concrete example:** A real-time process can preempt a normal process.

- **Timer interrupts:** Timer interrupts trigger scheduling and timing events inside the kernel.  
  **Concrete example:** A timer interrupt can wake the scheduler.

- **High-resolution timers:** High-resolution timers support fine-grained timing beyond coarse ticks.  
  **Concrete example:** Audio software uses precise wakeups for low-latency playback.

### 2.8 Kernel Memory Concepts
Kernel memory concepts explain how Linux manages RAM, virtual memory, caches, and reclaim.
They show how the kernel isolates processes while using memory efficiently.

- **Physical memory:** Physical memory is the actual RAM installed in the machine.  
  **Concrete example:** A workstation may have 32 GB of physical memory.

- **Virtual memory:** Virtual memory gives each process an isolated logical address space.  
  **Concrete example:** Two processes can both map memory at the same virtual address.

- **Paging:** Paging divides memory into fixed-size pages for mapping and allocation.  
  **Concrete example:** Linux commonly uses 4 KB pages.

- **Slab allocator:** The slab allocator caches frequently used kernel objects for efficiency.  
  **Concrete example:** Inodes are often served from slab caches.

- **Page cache:** The page cache stores file data in memory to accelerate I/O.  
  **Concrete example:** Reading the same file twice is faster because it stays cached.

- **OOM handling:** OOM handling reclaims memory or kills processes during severe memory exhaustion.  
  **Concrete example:** The OOM killer may terminate a runaway process.

### 2.9 Namespaces and Control Groups
Namespaces isolate process views of global resources, while cgroups control resource usage.
They are foundational kernel features behind containers and workload isolation.

- **PID namespaces:** PID namespaces isolate process numbering and process-tree views.  
  **Concrete example:** A container can see its own init process as PID 1.

- **Mount namespaces:** Mount namespaces isolate the visible mount table for a process set.  
  **Concrete example:** A container can see a private filesystem layout.

- **Network namespaces:** Network namespaces isolate interfaces, routes, and firewall state.  
  **Concrete example:** A container can have its own virtual Ethernet interface.

- **User namespaces:** User namespaces isolate UID and GID mappings between environments.  
  **Concrete example:** Container root can map to an unprivileged host UID.

- **cgroups:** cgroups group processes for resource accounting and enforcement.  
  **Concrete example:** A service can be limited to 1 GB of RAM.

- **Resource isolation:** Resource isolation prevents one workload from monopolizing shared resources.  
  **Concrete example:** CPU quotas can stop one container from using all CPU time.

### 2.10 Kernel Diagnostics and Tracing
Kernel diagnostics and tracing provide visibility into syscalls, kernel events, and performance behavior.
They help troubleshoot failures, latency issues, and performance bottlenecks.

- **`strace`:** `strace` traces system calls and signals made by a process.  
  **Concrete example:** `strace ls` shows `openat()` and `getdents64()` calls.

- **`ltrace`:** `ltrace` traces dynamic library calls made by a program.  
  **Concrete example:** It can show calls into glibc functions.

- **`/proc/<pid>`:** `/proc/<pid>` exposes runtime state for one process.  
  **Concrete example:** `/proc/1234/cmdline` shows the process command line.

- **`perf`:** `perf` measures hardware and kernel performance events.  
  **Concrete example:** `perf top` highlights frequently sampled functions.

- **`ftrace`:** `ftrace` traces internal kernel functions and timing events.  
  **Concrete example:** It can trace scheduler activity during latency debugging.

- **Kernel logs:** Kernel logs record driver messages, warnings, and hardware events.  
  **Concrete example:** A disk I/O error appears in kernel logs.

## 3. Linux File System

### 3.1 File System Basics
File-system basics define paths, directories, names, and inode-based object identity in Linux.
They explain how hierarchical naming resolves to stored filesystem objects.

- **Files and directories:** Files store data, while directories map names to filesystem objects.  
  **Concrete example:** `/etc/passwd` is a file and `/etc` is a directory.

- **Hierarchy:** Linux organizes filesystem objects in a tree rooted at `/`.  
  **Concrete example:** `/var/log/nginx` is nested under `/var`.

- **Absolute paths:** An absolute path starts from the root directory.  
  **Concrete example:** `/home/alice/.bashrc` is an absolute path.

- **Relative paths:** A relative path is resolved from the current working directory.  
  **Concrete example:** `../logs/app.log` is a relative path.

- **Inodes:** An inode stores metadata and block references for a filesystem object.  
  **Concrete example:** Two hard links can refer to the same inode.

- **Directory entries:** A directory entry maps a filename to an inode number.  
  **Concrete example:** `report.txt` inside `/home/alice` is a directory entry.

### 3.2 File System Hierarchy Standard
The filesystem hierarchy standard assigns conventional roles to major top-level directories.
It improves consistency in software placement, administration, and portability.

- **`/`:** `/` is the root of the entire filesystem tree.  
  **Concrete example:** Every absolute path begins at `/`.

- **`/bin`:** `/bin` stores essential user commands needed for basic operation.  
  **Concrete example:** `ls` may exist as `/bin/ls`.

- **`/sbin`:** `/sbin` stores essential administrative binaries.  
  **Concrete example:** `fsck` may exist as `/sbin/fsck`.

- **`/etc`:** `/etc` stores system-wide configuration files.  
  **Concrete example:** `/etc/ssh/sshd_config` configures the SSH daemon.

- **`/home`:** `/home` stores normal user home directories.  
  **Concrete example:** `/home/alice` is Alice's home directory.

- **`/root`:** `/root` is the home directory of the root user.  
  **Concrete example:** Root shell history may be in `/root/.bash_history`.

- **`/var`:** `/var` stores variable data such as logs and spool files.  
  **Concrete example:** `/var/log` stores many system logs.

- **`/tmp`:** `/tmp` stores temporary files intended for short-term use.  
  **Concrete example:** Installers may unpack temporary data into `/tmp`.

- **`/usr`:** `/usr` stores user applications, libraries, and shared read-only data.  
  **Concrete example:** `/usr/bin/python3` may contain the Python interpreter.

- **`/opt`:** `/opt` stores optional third-party application packages.  
  **Concrete example:** A vendor application may install into `/opt/myapp`.

- **`/dev`:** `/dev` contains device nodes for hardware and pseudo-devices.  
  **Concrete example:** `/dev/null` discards all written data.

- **`/proc`:** `/proc` exposes process and kernel runtime information as files.  
  **Concrete example:** `/proc/cpuinfo` lists CPU information.

- **`/sys`:** `/sys` exposes kernel object and device state.  
  **Concrete example:** `/sys/class/block` lists block devices.

- **`/run`:** `/run` stores volatile runtime state created after boot.  
  **Concrete example:** PID files often appear in `/run`.

- **`/mnt`:** `/mnt` is a conventional mount point for temporary manual mounts.  
  **Concrete example:** An admin may mount a recovery disk under `/mnt/recovery`.

- **`/media`:** `/media` is commonly used for removable media mount points.  
  **Concrete example:** A USB drive may appear under `/media/alice/USB`.

### 3.3 File Types
Linux file types define how filesystem objects behave and what operations they support.
Different file types represent data storage, devices, links, and IPC endpoints.

- **Regular files:** Regular files store application or user data as byte streams.  
  **Concrete example:** `notes.txt` is a regular file.

- **Directories:** Directories store name-to-inode mappings for contained objects.  
  **Concrete example:** `/etc` is a directory.

- **Symbolic links:** Symbolic links store a pathname reference to another object.  
  **Concrete example:** `current -> /opt/app/v2` is a symlink.

- **Hard links:** Hard links create another directory entry for the same inode.  
  **Concrete example:** `ln a.txt b.txt` makes `b.txt` a hard link to `a.txt`.

- **Character devices:** Character devices provide stream-oriented access to devices.  
  **Concrete example:** `/dev/tty` is a character device.

- **Block devices:** Block devices provide buffered random-access access to storage.  
  **Concrete example:** `/dev/sda` is a block device.

- **Named pipes:** Named pipes are filesystem-visible FIFO communication channels.  
  **Concrete example:** `mkfifo /tmp/feed` creates a named pipe.

- **Sockets:** Sockets are communication endpoints for local or network IPC.  
  **Concrete example:** `/run/docker.sock` is a Unix-domain socket.

### 3.4 File Metadata
File metadata records ownership, permissions, size, timestamps, and inode identity.
It controls access and supports filesystem management and auditing.

- **Inode number:** The inode number uniquely identifies an object within one filesystem.  
  **Concrete example:** `ls -i file.txt` prints the inode number.

- **Link count:** The link count records how many directory entries reference an inode.  
  **Concrete example:** A file with two hard links has a link count of 2.

- **Ownership:** Ownership records the user ID and group ID associated with an object.  
  **Concrete example:** A web file may be owned by `www-data:www-data`.

- **Permission bits:** Permission bits encode read, write, and execute access for owner, group, and others.  
  **Concrete example:** Mode `755` gives execute access to everyone.

- **Timestamps:** Timestamps record access, modification, and metadata-change times.  
  **Concrete example:** `stat file.txt` shows modify and access times.

- **File size:** File size records the logical length of a file in bytes.  
  **Concrete example:** A 1 KB file has size 1024 bytes.

### 3.5 File System Operations
Filesystem operations create, remove, rename, and organize objects in the directory tree.
These actions map to kernel-managed changes in metadata and stored data.

- **File creation:** File creation allocates a new inode and directory entry.  
  **Concrete example:** `touch new.txt` creates an empty file.

- **File deletion:** File deletion removes directory entries and releases storage when no links remain.  
  **Concrete example:** `rm old.txt` deletes a file.

- **File copying:** File copying creates a new object with duplicated content.  
  **Concrete example:** `cp a.txt backup/a.txt` copies a file.

- **File moving:** File moving changes a file path, often by renaming a directory entry.  
  **Concrete example:** `mv report.txt archive/report.txt` moves the file.

- **File renaming:** File renaming changes the name of an object without rewriting contents on the same filesystem.  
  **Concrete example:** `mv draft.txt final.txt` renames a file.

- **Directory creation:** Directory creation creates a new container for child entries.  
  **Concrete example:** `mkdir projects` creates a directory.

- **Directory removal:** Directory removal deletes an empty directory entry from its parent.  
  **Concrete example:** `rmdir emptydir` removes an empty directory.

### 3.6 Links
Links provide alternate naming and indirection mechanisms for filesystem objects.
They are used for shared names, indirection, and deployment workflows.

- **Hard links:** Hard links create multiple names for one inode in the same filesystem.  
  **Concrete example:** `ln data.log data.current` creates another name for the same file.

- **Symbolic links:** Symbolic links store a textual path reference to another object.  
  **Concrete example:** `ln -s /var/log/app.log current.log` creates a symlink.

- **Link behavior:** Link behavior determines whether names share inode identity or use pathname indirection.  
  **Concrete example:** Deleting one hard link does not remove the data if another link remains.

- **Link limitations:** Link limitations define where each link type can be used.  
  **Concrete example:** Hard links typically cannot cross filesystem boundaries.

- **Symlink resolution:** Symlink resolution follows the stored pathname during lookup.  
  **Concrete example:** Opening `current` may resolve to `/opt/app/v3`.

- **Cross-filesystem constraints:** Cross-filesystem constraints affect operations tied to inode identity.  
  **Concrete example:** A hard link from EXT4 to XFS is not allowed.

### 3.7 Mounting and Storage
Mounting attaches a filesystem instance to a directory in the global namespace.
Storage management combines partitions, filesystems, mount points, and persistent mount configuration.

- **Partitions:** Partitions are logical subdivisions of a block device.  
  **Concrete example:** `/dev/sda1` is a partition on disk `/dev/sda`.

- **Filesystems:** A filesystem defines the on-disk structures used to store data and metadata.  
  **Concrete example:** EXT4 is a common Linux filesystem.

- **Mount points:** A mount point is the directory where a filesystem becomes accessible.  
  **Concrete example:** `/mnt/data` can be used as a mount point.

- **`mount`:** `mount` attaches a filesystem to the directory tree.  
  **Concrete example:** `mount /dev/sdb1 /mnt/data` mounts a partition.

- **`umount`:** `umount` detaches a mounted filesystem from the directory tree.  
  **Concrete example:** `umount /mnt/data` unmounts the filesystem.

- **`/etc/fstab`:** `/etc/fstab` defines persistent mount configuration.  
  **Concrete example:** An entry can auto-mount `/dev/sdb1` at boot.

- **Automatic mounting:** Automatic mounting activates mounts on boot, access, or device insertion.  
  **Concrete example:** A USB drive may be auto-mounted under `/media`.

### 3.8 Disk Usage and File System Inspection
Inspection tools expose capacity, layout, identity, and usage of filesystems and block devices.
They are used for troubleshooting, validation, and capacity planning.

- **`df`:** `df` reports filesystem-level used and available space.  
  **Concrete example:** `df -h` shows free space in human-readable units.

- **`du`:** `du` reports disk usage for files and directories.  
  **Concrete example:** `du -sh /var/log` summarizes log directory size.

- **`lsblk`:** `lsblk` shows block devices, partitions, and mount relationships.  
  **Concrete example:** `lsblk` displays `/dev/sda` and its partitions.

- **`blkid`:** `blkid` prints block-device identifiers such as UUID and filesystem type.  
  **Concrete example:** `blkid /dev/sdb1` shows a partition UUID.

- **`findmnt`:** `findmnt` shows mounted filesystems and their source and target paths.  
  **Concrete example:** `findmnt /home` shows what backs `/home`.

- **`stat`:** `stat` displays detailed metadata about a filesystem object.  
  **Concrete example:** `stat file.txt` prints mode, owner, and timestamps.

### 3.9 File Search and Discovery
Search tools locate filesystem objects by path, name, metadata, or database indexes.
They support efficient discovery across large directory trees and software installations.

- **`find`:** `find` recursively searches directory trees using tests and actions.  
  **Concrete example:** `find /etc -name '*.conf'` finds config files.

- **`locate`:** `locate` searches a filename database for fast path lookup.  
  **Concrete example:** `locate sshd_config` quickly finds SSH config paths.

- **`which`:** `which` shows the executable found in the current `PATH`.  
  **Concrete example:** `which python3` may print `/usr/bin/python3`.

- **`whereis`:** `whereis` locates binaries, source files, and manual pages for a name.  
  **Concrete example:** `whereis ls` shows command and man page locations.

- **`updatedb`:** `updatedb` refreshes the database used by `locate`.  
  **Concrete example:** New files become searchable by `locate` after `updatedb`.

### 3.10 File Descriptors and Streams
File descriptors are per-process integer handles for open files, pipes, sockets, and devices.
Standard streams provide default input, output, and error channels that can be redirected.

- **Standard input:** Standard input is the default input stream on file descriptor 0.  
  **Concrete example:** `sort < names.txt` reads from standard input.

- **Standard output:** Standard output is the default output stream on file descriptor 1.  
  **Concrete example:** `echo ok > out.txt` redirects standard output.

- **Standard error:** Standard error is the default error stream on file descriptor 2.  
  **Concrete example:** `cmd 2> err.log` redirects error output.

- **Descriptor numbers:** Descriptor numbers are small integers used to reference open resources.  
  **Concrete example:** File descriptor 3 can represent an additional open file.

- **Redirection:** Redirection changes where a stream reads from or writes to.  
  **Concrete example:** `cmd >out.txt 2>&1` sends both outputs to one file.

- **Inheritance:** Descriptor inheritance allows child processes to receive selected open descriptors.  
  **Concrete example:** A shell pipeline passes pipe descriptors to both child commands.

### 3.11 Text and Binary I/O Concepts
Text and binary I/O differ in how programs interpret and process byte streams.
Shell pipelines and redirection behave differently depending on the data form.

- **Character streams:** Character streams treat bytes as encoded text characters.  
  **Concrete example:** `grep` processes a log file as text.

- **Binary streams:** Binary streams treat data as raw bytes without text interpretation.  
  **Concrete example:** `cat image.png > copy.png` preserves binary content exactly.

- **Buffering:** Buffering temporarily stores I/O in memory to reduce system-call overhead.  
  **Concrete example:** Output may appear late when a program uses block buffering.

- **Redirection targets:** Redirection targets are files, devices, or descriptors used for stream output or input.  
  **Concrete example:** `> /dev/null` discards output.

- **Here documents:** Here documents embed multiline input directly in shell syntax.  
  **Concrete example:** `cat <<EOF ... EOF` feeds text to a command.

### 3.12 File System Advanced Concepts
Advanced filesystem concepts address reliability, abstraction, snapshots, redundancy, and performance.
They include journaling, modern filesystems, volume management, and swap integration.

- **Journaling:** Journaling records updates in a log to improve crash recovery consistency.  
  **Concrete example:** EXT4 can replay its journal after an unclean shutdown.

- **EXT4:** EXT4 is a mature general-purpose Linux filesystem with journaling support.  
  **Concrete example:** Many distributions use EXT4 for the root partition.

- **XFS:** XFS is a high-performance journaling filesystem optimized for scalability.  
  **Concrete example:** Large enterprise data volumes are often formatted with XFS.

- **Btrfs:** Btrfs is a Linux filesystem with snapshots, checksums, and copy-on-write behavior.  
  **Concrete example:** A system can take a Btrfs snapshot before an upgrade.

- **ZFS concepts:** ZFS combines pooled storage, checksumming, snapshots, and copy-on-write data integrity.  
  **Concrete example:** A ZFS pool can combine several disks into one storage pool.

- **Swap:** Swap is disk-backed space used to extend virtual memory under pressure.  
  **Concrete example:** A system may use a swap partition when RAM is low.

- **RAID:** RAID combines multiple disks for redundancy, performance, or both.  
  **Concrete example:** RAID1 mirrors data across two disks.

- **LVM:** LVM abstracts physical disks into flexible logical volumes.  
  **Concrete example:** An admin can extend a logical volume without changing its mount point.

## 4. Linux Shell and Command Line

### 4.1 Shell Basics
Shell basics define the shell as a user-space command interpreter and control environment.
They explain how commands are read, parsed, and executed interactively or from scripts.

- **Shell role:** A shell is a program that interprets commands and launches other programs.  
  **Concrete example:** Bash reads a command line and executes `grep`.

- **Command interpretation:** Command interpretation tokenizes input, expands syntax, and prepares execution.  
  **Concrete example:** The shell expands `*.log` before calling `rm`.

- **Interactive shell:** An interactive shell reads commands directly from a terminal session.  
  **Concrete example:** A user typing commands over SSH is using an interactive shell.

- **Non-interactive shell:** A non-interactive shell runs commands from a script or redirected input.  
  **Concrete example:** `bash deploy.sh` starts a non-interactive shell.

- **Login shell:** A login shell initializes the session environment using login startup files.  
  **Concrete example:** SSH typically starts a login shell for the user.

- **Subshell:** A subshell is a child shell process used for isolated command execution.  
  **Concrete example:** Commands inside `(cd /tmp; ls)` run in a subshell.

### 4.2 Common Shells
Common shells provide similar execution models but differ in features, syntax, and usability.
Administrators choose shells based on scripting needs and interactive preferences.

- **`bash`:** Bash is a widely used GNU shell for interactive use and scripting.  
  **Concrete example:** Many Linux tutorials assume Bash syntax.

- **`sh`:** `sh` refers to the traditional POSIX shell interface or compatibility mode.  
  **Concrete example:** Portable scripts often begin with `#!/bin/sh`.

- **`zsh`:** Zsh is an advanced shell with rich completion and customization features.  
  **Concrete example:** Developers often use Zsh with enhanced prompts.

- **`ksh`:** Korn shell is a Unix shell known for scripting features and POSIX influence.  
  **Concrete example:** Legacy enterprise scripts may target `ksh`.

- **`fish`:** Fish is a user-friendly shell with autosuggestions and a distinct syntax style.  
  **Concrete example:** Fish shows suggestions as the user types.

### 4.3 Command Syntax
Command syntax defines the structure of command names, options, arguments, and quoting.
Correct syntax determines the final argument list passed for execution.

- **Command name:** The command name identifies the program or shell builtin to run.  
  **Concrete example:** In `ls -l`, `ls` is the command name.

- **Options and flags:** Options and flags modify command behavior using prefixed tokens.  
  **Concrete example:** `-l` requests long format from `ls`.

- **Arguments:** Arguments provide input values such as files, paths, or patterns.  
  **Concrete example:** In `cat file.txt`, `file.txt` is an argument.

- **Quoting:** Quoting preserves literal characters and suppresses unwanted expansion.  
  **Concrete example:** `'*.txt'` prevents glob expansion.

- **Escaping:** Escaping removes the special meaning of the next character.  
  **Concrete example:** `\ ` escapes a space in a filename.

- **Exit status:** Exit status is the numeric code returned by a command.  
  **Concrete example:** `0` usually means success.

### 4.4 Shell Features
Shell features let users combine commands, redirect streams, and build efficient CLI workflows.
These features make shells powerful without requiring full standalone programs.

- **Globbing:** Globbing expands wildcard filename patterns before command execution.  
  **Concrete example:** `ls *.log` lists all matching log files.

- **Redirection:** Redirection changes the source or destination of a command's standard streams.  
  **Concrete example:** `echo hi > out.txt` writes output to a file.

- **Pipes:** Pipes connect one command's output to another command's input.  
  **Concrete example:** `ps aux | grep nginx` filters process output.

- **Command substitution:** Command substitution replaces syntax with another command's output.  
  **Concrete example:** `echo $(date)` inserts the current date output.

- **History:** History stores previously executed commands for recall and reuse.  
  **Concrete example:** Pressing the up arrow recalls earlier commands.

- **Aliases:** Aliases define shorthand replacements for command strings.  
  **Concrete example:** `alias ll='ls -lah'` creates a shortcut.

- **Functions:** Shell functions define reusable blocks of shell logic.  
  **Concrete example:** A function can wrap `tar` backup steps into one command.

### 4.5 Environment and Variables
Shell variables and environment variables carry configuration and execution state.
Startup files initialize them to shape command lookup and program behavior.

- **Shell variables:** Shell variables store values local to the shell unless exported.  
  **Concrete example:** `name=alice` defines a shell variable.

- **Environment variables:** Environment variables are inherited by child processes.  
  **Concrete example:** `EDITOR=vim` affects application editor selection.

- **`PATH`:** `PATH` lists directories searched for executable command names.  
  **Concrete example:** `/usr/bin` is commonly part of `PATH`.

- **Exporting variables:** Exporting makes a shell variable visible to child processes.  
  **Concrete example:** `export PATH` passes the updated path to child commands.

- **Startup files:** Startup files are shell configuration scripts read during initialization.  
  **Concrete example:** Bash may read `~/.bashrc`.

- **Initialization order:** Initialization order determines which startup files are read in each shell mode.  
  **Concrete example:** Bash login shells may read `/etc/profile` then `~/.bash_profile`.

### 4.6 Shell Expansion
Shell expansion transforms typed command text into the final arguments used for execution.
Its ordered steps affect substitution, splitting, and resulting command behavior.

- **Brace expansion:** Brace expansion generates multiple strings from a compact pattern.  
  **Concrete example:** `echo file{1,2}.txt` expands to two filenames.

- **Tilde expansion:** Tilde expansion replaces `~` with a home directory path.  
  **Concrete example:** `cd ~/src` changes into a subdirectory of the user's home.

- **Parameter expansion:** Parameter expansion substitutes variable values into command text.  
  **Concrete example:** `echo $HOME` prints the home directory path.

- **Arithmetic expansion:** Arithmetic expansion evaluates integer expressions in shell syntax.  
  **Concrete example:** `echo $((2+3))` prints `5`.

- **Command substitution:** Command substitution inserts another command's output into a command line.  
  **Concrete example:** `files=$(ls /tmp)` stores command output in a variable.

- **Word splitting:** Word splitting breaks expanded text into arguments using delimiter rules.  
  **Concrete example:** An unquoted variable with spaces may become multiple arguments.

## 5. Basic Linux Commands

### 5.1 Navigation Commands
Navigation commands display the current location and move through the filesystem hierarchy.
They are foundational because many command operations are relative to the current directory.

- **`pwd`:** `pwd` prints the current working directory.  
  **Concrete example:** `pwd` may show `/home/alice/projects`.

- **`ls`:** `ls` lists the contents of a directory.  
  **Concrete example:** `ls /etc` shows files inside `/etc`.

- **`cd`:** `cd` changes the shell's current working directory.  
  **Concrete example:** `cd /var/log` moves into the log directory.

- **`tree`:** `tree` displays a recursive directory hierarchy in tree form.  
  **Concrete example:** `tree src` prints nested project folders.

### 5.2 File and Directory Commands
These commands create, remove, rename, copy, and link filesystem objects.
They are used constantly in daily Linux administration and shell work.

- **`touch`:** `touch` creates an empty file or updates file timestamps.  
  **Concrete example:** `touch notes.txt` creates a new file if it does not exist.

- **`cp`:** `cp` copies files or directories to another location.  
  **Concrete example:** `cp a.txt backup/a.txt` copies a file.

- **`mv`:** `mv` moves or renames files and directories.  
  **Concrete example:** `mv old.conf new.conf` renames a file.

- **`rm`:** `rm` removes files and, with options, entire directory trees.  
  **Concrete example:** `rm old.log` deletes a file.

- **`mkdir`:** `mkdir` creates one or more directories.  
  **Concrete example:** `mkdir -p app/config` creates nested directories.

- **`rmdir`:** `rmdir` removes empty directories.  
  **Concrete example:** `rmdir emptydir` removes an empty directory.

- **`ln`:** `ln` creates hard links or symbolic links.  
  **Concrete example:** `ln -s /opt/app/current app-link` creates a symlink.

### 5.3 Viewing File Content
Viewing commands display file contents directly, partially, or through a pager.
They are commonly used for logs, config files, and text inspection.

- **`cat`:** `cat` prints file contents to standard output.  
  **Concrete example:** `cat /etc/hostname` shows the hostname file.

- **`less`:** `less` opens a scrollable pager for viewing long text.  
  **Concrete example:** `less /var/log/syslog` opens a system log.

- **`more`:** `more` displays text page by page in a simpler pager.  
  **Concrete example:** `more readme.txt` paginates a text file.

- **`head`:** `head` prints the first lines of a file.  
  **Concrete example:** `head -n 5 access.log` shows the first five lines.

- **`tail`:** `tail` prints the last lines of a file and can follow updates.  
  **Concrete example:** `tail -f app.log` streams new log entries.

- **`nl`:** `nl` prints file contents with line numbers.  
  **Concrete example:** `nl script.sh` shows numbered script lines.

### 5.4 Text Processing Commands
Text-processing commands search, transform, filter, and summarize textual streams.
They are designed to compose well in pipes and shell scripts.

- **`echo`:** `echo` prints its arguments to standard output.  
  **Concrete example:** `echo hello` prints `hello`.

- **`grep`:** `grep` searches text for matching lines.  
  **Concrete example:** `grep ERROR app.log` finds error lines.

- **`sort`:** `sort` orders input lines according to lexical or numeric rules.  
  **Concrete example:** `sort names.txt` sorts names alphabetically.

- **`uniq`:** `uniq` filters or counts adjacent duplicate lines.  
  **Concrete example:** `sort names.txt | uniq` removes duplicates.

- **`cut`:** `cut` extracts character ranges or delimited fields from text.  
  **Concrete example:** `cut -d: -f1 /etc/passwd` prints usernames.

- **`tr`:** `tr` translates or deletes characters from input streams.  
  **Concrete example:** `tr a-z A-Z < file.txt` converts text to uppercase.

- **`wc`:** `wc` counts lines, words, and bytes in text input.  
  **Concrete example:** `wc -l app.log` counts lines.

- **`awk`:** `awk` processes text by records and fields using pattern-action rules.  
  **Concrete example:** `awk -F: '{print $1}' /etc/passwd` prints usernames.

- **`sed`:** `sed` performs stream editing such as substitution and selection.  
  **Concrete example:** `sed 's/foo/bar/g' file.txt` replaces text globally.

### 5.5 Search and Pattern Commands
Search commands locate files or matching content using recursion and patterns.
They help administrators and scripts find specific objects quickly.

- **`find`:** `find` traverses directories and selects entries using tests and actions.  
  **Concrete example:** `find /var/log -name '*.gz'` finds compressed logs.

- **`locate`:** `locate` performs fast path lookup using a prebuilt filename database.  
  **Concrete example:** `locate hosts` finds files named `hosts`.

- **`grep`:** `grep` matches line content against regular expressions.  
  **Concrete example:** `grep -R root /etc` searches recursively for `root`.

- **`egrep`:** `egrep` uses extended regular expressions for richer pattern matching.  
  **Concrete example:** `egrep 'error|fail' app.log` matches either word.

- **`xargs`:** `xargs` builds command arguments from standard input.  
  **Concrete example:** `find . -name '*.tmp' | xargs rm` removes found temporary files.

### 5.6 Archive and Copy Commands
Archive and copy commands package, compress, and synchronize data for transfer or backup.
They preserve structure and often preserve metadata needed for restoration.

- **`tar`:** `tar` creates, extracts, and lists archive files.  
  **Concrete example:** `tar -cf backup.tar /etc` archives `/etc`.

- **`gzip`:** `gzip` compresses files using the gzip format.  
  **Concrete example:** `gzip backup.tar` creates `backup.tar.gz`.

- **`bzip2`:** `bzip2` compresses files using a higher-ratio algorithm than gzip.  
  **Concrete example:** `bzip2 large.log` creates `large.log.bz2`.

- **`xz`:** `xz` compresses files using strong compression.  
  **Concrete example:** `xz image.iso` creates `image.iso.xz`.

- **`zip`:** `zip` creates ZIP archives for cross-platform exchange.  
  **Concrete example:** `zip docs.zip *.txt` bundles text files.

- **`unzip`:** `unzip` extracts files from ZIP archives.  
  **Concrete example:** `unzip docs.zip` extracts the archive contents.

- **`rsync`:** `rsync` synchronizes files by transferring only changed data.  
  **Concrete example:** `rsync -a src/ backup/` mirrors a directory tree.

### 5.7 System Information Commands
System-information commands reveal host identity, time, uptime, and account context.
They are often used to verify assumptions before making changes.

- **`date`:** `date` prints or sets the system date and time.  
  **Concrete example:** `date` prints the current timestamp.

- **`cal`:** `cal` displays a formatted calendar.  
  **Concrete example:** `cal 10 2026` shows October 2026.

- **`whoami`:** `whoami` prints the current effective username.  
  **Concrete example:** `whoami` may print `root`.

- **`id`:** `id` prints user and group identity information.  
  **Concrete example:** `id alice` shows Alice's UID and groups.

- **`hostname`:** `hostname` prints or sets the system hostname.  
  **Concrete example:** `hostname` may print `web01`.

- **`uptime`:** `uptime` shows system runtime duration and load averages.  
  **Concrete example:** `uptime` may show `up 12 days`.

- **`uname`:** `uname` prints kernel and system identification details.  
  **Concrete example:** `uname -m` prints the machine architecture.

- **`env`:** `env` prints environment variables or runs a command with a modified environment.  
  **Concrete example:** `env | grep PATH` shows the `PATH` variable.

### 5.8 Help and Documentation
Documentation commands expose built-in help and installed manual resources.
They are the primary on-system reference for syntax and options.

- **`man`:** `man` opens the manual page for a command or topic.  
  **Concrete example:** `man chmod` explains file mode changes.

- **`info`:** `info` reads GNU hypertext-style documentation.  
  **Concrete example:** `info coreutils` opens GNU core utilities documentation.

- **`--help`:** `--help` prints a command's built-in usage summary.  
  **Concrete example:** `grep --help` shows grep options.

- **`whatis`:** `whatis` prints a one-line manual-page description.  
  **Concrete example:** `whatis tar` shows a short summary of `tar`.

- **`apropos`:** `apropos` searches manual-page descriptions by keyword.  
  **Concrete example:** `apropos password` finds related commands and APIs.

## 6. File Permissions and Ownership

### 6.1 Permission Model
The Linux permission model applies read, write, and execute bits to ownership classes.
It implements discretionary access control using user, group, and other categories.

- **Read permission:** Read permission allows viewing file contents or listing a directory.  
  **Concrete example:** A file mode with `r--` allows `cat file.txt`.

- **Write permission:** Write permission allows modifying file contents or directory entries.  
  **Concrete example:** A writable directory allows creation of new files.

- **Execute permission:** Execute permission allows program execution or directory traversal.  
  **Concrete example:** A script needs execute permission to run as `./script.sh`.

- **Permission classes:** Linux evaluates permissions separately for owner, group, and others.  
  **Concrete example:** `rwxr-x---` gives different access to each class.

- **Ownership model:** Each filesystem object has one owner UID and one owning GID.  
  **Concrete example:** A file owned by `alice:dev` uses Alice as owner and `dev` as group.

### 6.2 Permission Semantics
Permission semantics differ between regular files and directories.
Understanding the difference is critical for access control and troubleshooting.

- **File read semantics:** File read permission allows the file's stored bytes to be read.  
  **Concrete example:** A user with read permission can run `cat report.txt`.

- **File write semantics:** File write permission allows file contents to be changed.  
  **Concrete example:** A writable config file can be edited with `vim`.

- **File execute semantics:** File execute permission allows the kernel to run the file as a program.  
  **Concrete example:** `chmod +x deploy.sh` allows script execution.

- **Directory read semantics:** Directory read permission allows listing entry names.  
  **Concrete example:** `ls dir` needs read permission on `dir`.

- **Directory write semantics:** Directory write permission allows adding or removing entries.  
  **Concrete example:** Creating `dir/new.txt` needs directory write permission.

- **Directory execute semantics:** Directory execute permission allows traversal to named entries.  
  **Concrete example:** `cd dir` requires execute permission on `dir`.

### 6.3 Viewing Permissions
Viewing permissions reveals mode bits, ownership, and related metadata.
These commands help diagnose access problems and validate system state.

- **`ls -l`:** `ls -l` shows file type, permissions, ownership, size, and timestamp.  
  **Concrete example:** `ls -l script.sh` may show `-rwxr-xr-x`.

- **Symbolic mode view:** Symbolic mode displays permissions using `r`, `w`, and `x` characters.  
  **Concrete example:** `drwxr-xr-x` represents a directory with mode 755.

- **Numeric mode view:** Numeric mode represents permission bits using octal digits.  
  **Concrete example:** `644` means owner read-write and others read-only.

- **`stat`:** `stat` prints detailed metadata including numeric mode and ownership.  
  **Concrete example:** `stat file.txt` shows `Access: (0644/-rw-r--r--)`.

### 6.4 Changing Permissions
Changing permissions modifies mode bits to alter allowed operations.
Linux supports both symbolic and octal notation for mode changes.

- **`chmod`:** `chmod` changes permission bits on files and directories.  
  **Concrete example:** `chmod 600 secrets.txt` restricts access to the owner.

- **Symbolic mode:** Symbolic mode changes permissions by class and operation.  
  **Concrete example:** `chmod g+w file.txt` adds group write permission.

- **Numeric mode:** Numeric mode sets permissions directly using octal digits.  
  **Concrete example:** `chmod 755 run.sh` sets standard executable permissions.

- **Recursive changes:** Recursive mode changes apply through an entire directory tree.  
  **Concrete example:** `chmod -R o-rwx private/` removes public access recursively.

### 6.5 Ownership Management
Ownership management changes the user and group associated with filesystem objects.
It affects how standard permission checks are evaluated.

- **`chown`:** `chown` changes the owning user and optionally the group.  
  **Concrete example:** `chown alice file.txt` makes Alice the owner.

- **`chgrp`:** `chgrp` changes the owning group of a file or directory.  
  **Concrete example:** `chgrp dev shared.txt` assigns the file to group `dev`.

- **User ownership:** User ownership determines which user-mode bits apply first.  
  **Concrete example:** If Alice owns a file, her user bits are evaluated first.

- **Group ownership:** Group ownership determines which group-mode bits apply to matching members.  
  **Concrete example:** A user in group `dev` gains the group's file permissions.

- **Recursive ownership changes:** Recursive ownership changes apply through all descendants of a directory.  
  **Concrete example:** `chown -R www-data:www-data /var/www` fixes web-root ownership.

### 6.6 Special Permissions
Special permissions extend the standard mode bits with privilege and deletion behavior.
They are important for shared directories and controlled privilege inheritance.

- **setuid:** setuid makes an executable run with the file owner's effective UID.  
  **Concrete example:** `/usr/bin/passwd` often uses setuid root.

- **setgid:** setgid makes an executable run with the file group's GID or causes group inheritance on directories.  
  **Concrete example:** A shared project directory with setgid keeps new files in the same group.

- **Sticky bit:** The sticky bit restricts file deletion in a writable directory.  
  **Concrete example:** `/tmp` uses the sticky bit so users cannot delete each other's files.

- **Special directory semantics:** Special bits on directories affect group inheritance and deletion control.  
  **Concrete example:** A setgid directory causes new files to inherit the directory group.

### 6.7 Default Permissions
Default permissions are determined by creation mode and the process umask.
They prevent newly created files and directories from being overly permissive.

- **`umask`:** `umask` masks permission bits from default creation modes.  
  **Concrete example:** `umask 022` removes group and other write permission.

- **Permission calculation:** Final default permissions are derived by applying the umask to base modes.  
  **Concrete example:** A new regular file often becomes `644` under umask `022`.

- **Creation defaults:** Programs start from default creation modes before masking is applied.  
  **Concrete example:** A regular file commonly starts from `666` before umask.

### 6.8 Access Control Lists
ACLs extend the basic permission model with extra per-user and per-group rules.
They are useful when one owner and one group are not enough.

- **ACL model:** ACLs attach fine-grained access entries to files and directories.  
  **Concrete example:** A file can grant read access to one extra user.

- **`getfacl`:** `getfacl` displays ACL entries for filesystem objects.  
  **Concrete example:** `getfacl shared.txt` shows extra access rules.

- **`setfacl`:** `setfacl` creates or modifies ACL entries.  
  **Concrete example:** `setfacl -m u:alice:r file.txt` grants Alice read access.

- **Default ACLs:** Default ACLs on directories define inherited ACLs for new children.  
  **Concrete example:** New files in a shared directory can inherit group ACL entries.

### 6.9 Linux Capabilities
Linux capabilities split root privilege into smaller privilege units.
They allow fine-grained delegation without granting full superuser powers.

- **Capability model:** The capability model breaks privileged operations into independent units.  
  **Concrete example:** `CAP_NET_BIND_SERVICE` allows binding to low ports.

- **`setcap`:** `setcap` assigns capabilities to an executable file.  
  **Concrete example:** `setcap cap_net_bind_service=+ep /usr/local/bin/app` permits port 80 binding.

- **`getcap`:** `getcap` displays capabilities assigned to files.  
  **Concrete example:** `getcap /usr/local/bin/app` shows its configured capability.

- **Delegated privilege:** Fine-grained delegation grants only the privileges a program actually needs.  
  **Concrete example:** A backup tool can get file-read capability without full root access.

### 6.10 Symlink and Permission Edge Cases
Symlink behavior differs from regular files in access checks and pathname resolution.
These edge cases matter for secure traversal, cleanup, and access control.

- **Symlink mode behavior:** Symlink mode bits are generally ignored during access checks on Linux.  
  **Concrete example:** Most symlinks display as `lrwxrwxrwx` regardless of effective access.

- **Target permissions:** Access through a symlink is checked against the target object's permissions.  
  **Concrete example:** A symlink to `/etc/shadow` does not bypass target restrictions.

- **Traversal checks:** Path traversal still requires execute permission on each directory component.  
  **Concrete example:** A readable file remains inaccessible if a parent directory lacks execute permission.

- **Ownership edge cases:** Symlink ownership can matter for some administrative operations in shared directories.  
  **Concrete example:** Symlink ownership may matter during cleanup in `/tmp`.

## 7. Users and Groups

### 7.1 User Accounts
User accounts define login identity, UID, shell, and home-directory information.
They are used for interactive access, service execution, and ownership decisions.

- **Root user:** The root user is the superuser account with UID 0.  
  **Concrete example:** Root can change ownership of any file.

- **Regular users:** Regular users are standard interactive accounts with limited privileges.  
  **Concrete example:** `alice` edits files under `/home/alice`.

- **System users:** System users are non-human accounts created for system components.  
  **Concrete example:** `www-data` may run the web server.

- **Service accounts:** Service accounts are dedicated identities used by long-running daemons.  
  **Concrete example:** PostgreSQL commonly runs as the `postgres` user.

- **Account attributes:** User records store username, UID, GID, shell, home, and related metadata.  
  **Concrete example:** `/etc/passwd` stores these fields.

### 7.2 Group Management
Groups provide shared access control and collective identity assignment for users.
They simplify permission administration across teams and services.

- **Primary group:** The primary group is the default GID associated with new files a user creates.  
  **Concrete example:** Alice's files may default to group `alice`.

- **Secondary groups:** Secondary groups add extra memberships beyond the user's primary group.  
  **Concrete example:** Alice may also belong to `docker` and `dev`.

- **Group membership:** Group membership determines whether group permissions apply to a user.  
  **Concrete example:** A user in group `www` can read group-readable web files.

- **Supplementary groups:** Supplementary groups are additional groups listed for a user.  
  **Concrete example:** `id alice` lists Alice's supplementary groups.

### 7.3 User and Group Commands
These commands create, modify, remove, and inspect account and group records.
They provide standard administration interfaces for local identity management.

- **`useradd`:** `useradd` creates a new user account.  
  **Concrete example:** `useradd -m alice` creates Alice with a home directory.

- **`usermod`:** `usermod` modifies properties of an existing user account.  
  **Concrete example:** `usermod -aG sudo alice` adds Alice to the sudo group.

- **`userdel`:** `userdel` removes a user account.  
  **Concrete example:** `userdel bob` deletes the user Bob.

- **`passwd`:** `passwd` sets or changes a user's password.  
  **Concrete example:** `passwd alice` changes Alice's password.

- **`groupadd`:** `groupadd` creates a new group.  
  **Concrete example:** `groupadd devops` creates a `devops` group.

- **`groupmod`:** `groupmod` modifies an existing group.  
  **Concrete example:** `groupmod -n engineering devops` renames the group.

- **`groupdel`:** `groupdel` removes a group.  
  **Concrete example:** `groupdel tempgroup` deletes an unused group.

- **`groups`:** `groups` prints a user's group memberships.  
  **Concrete example:** `groups alice` lists Alice's groups.

- **`id`:** `id` prints UID, GID, and group memberships.  
  **Concrete example:** `id` prints the current user's identities.

### 7.4 Authentication Files
Authentication files store local user, group, and password metadata.
They form the core local account databases on a Linux system.

- **`/etc/passwd`:** `/etc/passwd` stores user account identity and login metadata.  
  **Concrete example:** Each line contains username, UID, GID, home, and shell.

- **`/etc/shadow`:** `/etc/shadow` stores password hashes and aging information.  
  **Concrete example:** Only privileged users can read `/etc/shadow`.

- **`/etc/group`:** `/etc/group` stores group definitions and membership lists.  
  **Concrete example:** The `sudo` group appears in `/etc/group`.

- **`/etc/gshadow`:** `/etc/gshadow` stores secure group password and admin metadata.  
  **Concrete example:** It complements `/etc/group` with restricted information.

### 7.5 Privilege Escalation
Privilege escalation tools allow approved users to run commands as another user or with elevated rights.
They are essential for administration but must be tightly controlled.

- **`sudo`:** `sudo` runs a command with privileges defined by policy.  
  **Concrete example:** `sudo systemctl restart nginx` restarts Nginx with elevation.

- **`su`:** `su` starts a shell or command as another user, commonly root.  
  **Concrete example:** `su -` opens a root login shell after authentication.

- **sudoers policy:** The sudoers policy defines who may run which commands as which targets.  
  **Concrete example:** A helpdesk account may be allowed to restart one service only.

- **Delegation rules:** Delegation rules limit administrative actions to approved users and tasks.  
  **Concrete example:** A deployment user may run only package-update commands.

### 7.6 Authentication and Session Controls
Authentication and session controls define how users are validated and how sessions start.
They influence login policy, environment setup, and account restrictions.

- **PAM:** PAM is a modular authentication framework used by login and service programs.  
  **Concrete example:** PAM can enforce password complexity policies.

- **Login shells:** A login shell initializes a user session using login startup files.  
  **Concrete example:** SSH typically starts a login shell.

- **Session environment:** The session environment contains variables and settings inherited by user processes.  
  **Concrete example:** `LANG` and `PATH` are part of a user's session environment.

- **Account policies:** Account policies define expiration, lockout, and password-aging rules.  
  **Concrete example:** An account can be configured to expire after a set date.

## 8. Process Management

### 8.1 Process Fundamentals
Process fundamentals define a process as a running program instance with its own execution context.
They explain identifiers, parentage, states, and the distinction between processes and threads.

- **Process definition:** A process is a program in execution with memory, credentials, and kernel state.  
  **Concrete example:** Running `python app.py` creates a process.

- **PID:** A PID is the numeric identifier assigned to a process.  
  **Concrete example:** `ps` may show Nginx master as PID 1012.

- **Parent-child relation:** Parent-child relationships arise when one process creates another.  
  **Concrete example:** A shell creates a child process to run `ls`.

- **Process states:** Process states describe whether a task is running, sleeping, stopped, or zombie.  
  **Concrete example:** `ps` may show `S` for a sleeping process.

- **Threads vs processes:** Threads share an address space, while separate processes do not.  
  **Concrete example:** A web server may use multiple threads inside one process.

### 8.2 Process Lifecycle
The process lifecycle covers creation, execution, waiting, termination, and cleanup.
Linux typically uses fork-exec semantics with exit-status collection by the parent.

- **Process creation:** Process creation allocates a new task and execution context.  
  **Concrete example:** A shell creates a child when launching a command.

- **`fork`:** `fork` creates a child process by duplicating the calling process.  
  **Concrete example:** A shell forks before running `ls`.

- **`exec`:** `exec` replaces the current process image with a new program image.  
  **Concrete example:** After `fork`, the child may call `execve()` for `/bin/ls`.

- **`wait`:** `wait` lets a parent collect the termination status of a child.  
  **Concrete example:** A shell waits for a foreground command to finish.

- **Termination:** Process termination ends execution and releases most resources.  
  **Concrete example:** A program calling `exit(0)` terminates normally.

- **Exit status:** Exit status is the numeric result code returned by a terminated process.  
  **Concrete example:** `0` commonly indicates success.

- **Zombie processes:** A zombie is a terminated process whose status has not yet been reaped.  
  **Concrete example:** `ps` can show a zombie with state `Z`.

- **Orphan processes:** An orphan is a child whose parent has exited.  
  **Concrete example:** Orphans are typically adopted by PID 1.

### 8.3 Process Inspection
Process inspection tools expose process state, ancestry, resource usage, and command identity.
They are essential for monitoring and troubleshooting running systems.

- **`ps`:** `ps` reports a snapshot of current process information.  
  **Concrete example:** `ps -ef` lists processes with parent PIDs.

- **`top`:** `top` shows live process and system resource usage interactively.  
  **Concrete example:** `top` highlights the process using the most CPU.

- **`htop`:** `htop` is an enhanced interactive process viewer.  
  **Concrete example:** `htop` shows CPU bars and lets users send signals.

- **`pgrep`:** `pgrep` finds process IDs by name or matching criteria.  
  **Concrete example:** `pgrep nginx` returns Nginx PIDs.

- **`/proc`:** `/proc` exposes per-process and kernel runtime information.  
  **Concrete example:** `/proc/1/status` shows PID 1 status.

- **`/proc/<pid>`:** `/proc/<pid>` contains runtime details for a specific process.  
  **Concrete example:** `/proc/1234/fd` lists open file descriptors.

- **`pstree`:** `pstree` displays processes as a parent-child tree.  
  **Concrete example:** `pstree -p` shows process ancestry with PIDs.

### 8.4 Process Control
Process control changes process behavior or state through signals and job mechanisms.
It is used to terminate, pause, resume, or manage active workloads.

- **`kill`:** `kill` sends a signal to a specific PID.  
  **Concrete example:** `kill -TERM 1234` requests graceful shutdown.

- **`pkill`:** `pkill` sends a signal to processes selected by name or attributes.  
  **Concrete example:** `pkill nginx` signals all matching Nginx processes.

- **`killall`:** `killall` sends a signal to processes by executable name.  
  **Concrete example:** `killall firefox` stops all Firefox processes.

- **Signals:** Signals are asynchronous notifications delivered to a process by the kernel.  
  **Concrete example:** `SIGKILL` immediately terminates a process.

- **Foreground and background jobs:** Foreground jobs control the terminal, while background jobs do not.  
  **Concrete example:** `sleep 100 &` starts a background job.

- **Signal semantics:** Signal semantics define which signals can be handled, ignored, or blocked.  
  **Concrete example:** `SIGTERM` can be handled, but `SIGKILL` cannot.

### 8.5 Job Control
Job control is a shell mechanism for handling terminal-associated process groups.
It supports suspension, background execution, and returning jobs to the foreground.

- **`jobs`:** `jobs` lists the shell's current jobs.  
  **Concrete example:** `jobs` may show `[1]+ Running sleep 100 &`.

- **`bg`:** `bg` resumes a stopped job in the background.  
  **Concrete example:** After Ctrl+Z, `bg` continues the stopped job.

- **`fg`:** `fg` brings a job back to the foreground.  
  **Concrete example:** `fg %1` returns job 1 to terminal control.

- **`nohup`:** `nohup` runs a command immune to terminal hangup signals.  
  **Concrete example:** `nohup longtask &` keeps running after logout.

- **`disown`:** `disown` removes a job from the shell's job table.  
  **Concrete example:** `disown %1` detaches job 1 from the shell.

- **`&`:** `&` starts a command in the background.  
  **Concrete example:** `backup.sh &` launches the script asynchronously.

### 8.6 Process Priority
Process priority influences how the scheduler treats competing runnable tasks.
Linux supports niceness and scheduler-specific priority models.

- **Nice values:** Nice values bias CPU scheduling priority for normal tasks.  
  **Concrete example:** A process with nice `10` runs with lower priority than one with nice `0`.

- **`nice`:** `nice` starts a process with a modified niceness level.  
  **Concrete example:** `nice -n 10 make` lowers build priority.

- **`renice`:** `renice` changes the niceness of an already running process.  
  **Concrete example:** `renice 5 -p 1234` changes a process priority.

- **Priority interaction:** Priority behavior depends on scheduler class as well as niceness.  
  **Concrete example:** A real-time task can outrank all normal nice-based tasks.

### 8.7 Scheduling and Affinity
Scheduling and affinity control CPU placement and policy for running tasks.
They matter for latency-sensitive and high-performance workloads.

- **Scheduler classes:** Scheduler classes define different execution policies and priority rules.  
  **Concrete example:** A normal service commonly uses `SCHED_OTHER`.

- **CPU affinity:** CPU affinity restricts a task to one or more CPU cores.  
  **Concrete example:** A process can be pinned to CPU 2.

- **`chrt`:** `chrt` displays or changes real-time scheduling policy and priority.  
  **Concrete example:** `chrt -f 50 ./task` starts a FIFO real-time task.

- **`taskset`:** `taskset` displays or sets CPU affinity masks.  
  **Concrete example:** `taskset -c 0,1 ./worker` limits a process to CPUs 0 and 1.

- **Real-time policies:** Real-time policies prioritize predictable scheduling for eligible tasks.  
  **Concrete example:** Audio processing may use `SCHED_FIFO`.

### 8.8 Process States and Scheduling Details
Detailed process states explain whether tasks are runnable, blocked, stopped, or finished.
These states help distinguish CPU pressure from I/O or synchronization waits.

- **Running state:** Running means a task is executing or ready to execute.  
  **Concrete example:** A CPU-bound process appears runnable in `top`.

- **Sleeping state:** Sleeping means a task is waiting for an event.  
  **Concrete example:** A web server worker may sleep waiting for a request.

- **Stopped state:** Stopped means a task has been paused by a signal or debugger.  
  **Concrete example:** Pressing Ctrl+Z stops a foreground job.

- **Zombie state:** Zombie means a task has exited but still has an unreaped process entry.  
  **Concrete example:** `ps` may show `<defunct>` for a zombie.

- **Interruptible vs uninterruptible sleep:** Sleep states differ in whether a signal can wake the task.  
  **Concrete example:** A process blocked on disk I/O may appear in `D` state.

### 8.9 Services and Daemons
Services and daemons are long-running background processes typically managed by an init system.
They provide system or application functionality independent of user logins.

- **Daemons:** Daemons are background service processes detached from interactive terminals.  
  **Concrete example:** `sshd` accepts remote logins as a daemon.

- **`systemd`:** systemd is an init and service manager used by many distributions.  
  **Concrete example:** systemd starts networking during boot.

- **`systemctl`:** `systemctl` controls and inspects systemd-managed units.  
  **Concrete example:** `systemctl restart nginx` restarts Nginx.

- **Service units:** Service units define how systemd starts and supervises daemons.  
  **Concrete example:** `nginx.service` defines the Nginx service behavior.

- **Targets:** Targets are groups of units representing broader system states.  
  **Concrete example:** `multi-user.target` represents normal non-graphical operation.

- **PID 1:** PID 1 is the first user-space process and adoptive parent of orphans.  
  **Concrete example:** On many systems, PID 1 is systemd.

### 8.10 Resource Control
Resource control limits and accounts for process consumption of CPU, memory, files, and other resources.
Linux implements this using shell limits, kernel limits, and cgroups.

- **cgroups:** cgroups group processes for resource accounting and enforcement.  
  **Concrete example:** A container can be capped at one CPU.

- **Resource limits:** Limits define maximum allowed usage for specific resources.  
  **Concrete example:** A user can have a cap on open files.

- **`ulimit`:** `ulimit` shows or changes shell resource limits for child processes.  
  **Concrete example:** `ulimit -n 4096` raises the open-file limit.

- **OOM interaction:** OOM handling may terminate processes when memory exhaustion becomes critical.  
  **Concrete example:** A memory-hungry process may be killed under severe RAM pressure.

- **Per-process ceilings:** Per-process ceilings bound usage such as CPU time or file size.  
  **Concrete example:** A process can be limited to a 1 GB core file.

## 9. Package Management

### 9.1 Package Concepts
Package concepts define software as versioned, managed units with metadata and dependencies.
Repositories and package databases enable controlled install and upgrade workflows.

- **Packages:** A package is a structured software unit containing files and metadata.  
  **Concrete example:** `nginx` is distributed as a package.

- **Repositories:** Repositories are indexed sources from which packages are downloaded.  
  **Concrete example:** Debian pulls packages from configured APT repositories.

- **Dependencies:** Dependencies are packages or capabilities required by another package.  
  **Concrete example:** Installing `git` may also install supporting libraries.

- **Package metadata:** Metadata records version, architecture, dependencies, scripts, and checksums.  
  **Concrete example:** RPM metadata includes package release and signature information.

- **GPG verification:** Signature verification helps confirm repository metadata and packages are trusted.  
  **Concrete example:** APT rejects unsigned repository metadata by default.

### 9.2 Debian Package Tools
Debian package tools manage `.deb` packages and APT repositories.
They separate dependency resolution from low-level package installation operations.

- **`apt`:** `apt` is a high-level tool for searching, installing, and upgrading packages.  
  **Concrete example:** `apt install curl` installs curl.

- **`apt-get`:** `apt-get` is a script-friendly APT command for package transactions.  
  **Concrete example:** `apt-get update` refreshes repository indexes.

- **`apt-cache`:** `apt-cache` queries package metadata stored in the local APT cache.  
  **Concrete example:** `apt-cache policy nginx` shows candidate versions.

- **`dpkg`:** `dpkg` is the low-level tool for installing and managing local `.deb` packages.  
  **Concrete example:** `dpkg -i package.deb` installs a local package file.

### 9.3 Red Hat Package Tools
Red Hat package tools manage `.rpm` packages and repository-based dependency resolution.
They are standard on enterprise RPM-based distributions.

- **`yum`:** `yum` is an older high-level package manager for RPM-based systems.  
  **Concrete example:** `yum install httpd` installs Apache on older RHEL systems.

- **`dnf`:** `dnf` is the modern RPM package manager for package transactions.  
  **Concrete example:** `dnf update` upgrades installed packages.

- **`rpm`:** `rpm` is the low-level tool for installing, querying, and verifying RPM packages.  
  **Concrete example:** `rpm -q bash` checks whether Bash is installed.

### 9.4 Other Package Managers
Other package managers implement alternative distribution or application-delivery models.
They may emphasize rolling releases, sandboxing, or cross-distribution packaging.

- **`pacman`:** `pacman` is Arch Linux's package manager for binary packages.  
  **Concrete example:** `pacman -S vim` installs Vim on Arch.

- **`zypper`:** `zypper` manages packages and repositories on SUSE-based systems.  
  **Concrete example:** `zypper install htop` installs htop on openSUSE.

- **`snap`:** `snap` distributes sandboxed application packages with bundled dependencies.  
  **Concrete example:** `snap install code --classic` installs VS Code.

- **`flatpak`:** `flatpak` distributes desktop applications in a sandboxed runtime-based format.  
  **Concrete example:** `flatpak install flathub org.gimp.GIMP` installs GIMP.

### 9.5 Package Management Workflows
Package management workflows govern safe installation, update, removal, and verification.
They reduce system drift and improve predictable maintenance.

- **Install:** Installation adds a package and its files to the system.  
  **Concrete example:** `apt install nginx` installs the web server package.

- **Upgrade:** Upgrade replaces installed versions with newer approved ones.  
  **Concrete example:** `dnf upgrade` updates all installed packages.

- **Remove:** Removal deletes package-managed files and unregisters package state.  
  **Concrete example:** `apt remove nginx` removes Nginx binaries.

- **Query:** Query operations inspect package state, versions, and ownership.  
  **Concrete example:** `rpm -ql bash` lists files in the Bash package.

- **Verify:** Verification checks installed files against recorded package metadata.  
  **Concrete example:** `rpm -V bash` validates file integrity and attributes.

- **Rollback concepts:** Rollback concepts describe recovery from problematic package changes.  
  **Concrete example:** A filesystem snapshot can restore the system after a bad update.

## 10. Text Editors

### 10.1 Terminal Editors
Terminal editors let users modify text directly from a shell session without a GUI.
They are essential for remote administration, recovery work, and server management.

- **`vi`:** `vi` is a modal terminal text editor optimized for keyboard-driven editing.  
  **Concrete example:** `vi /etc/hosts` opens the hosts file.

- **`vim`:** `vim` is an enhanced vi-compatible editor with extra features and scripting support.  
  **Concrete example:** `vim nginx.conf` opens a config file with syntax highlighting.

- **`nano`:** `nano` is a simple terminal editor with visible shortcut hints.  
  **Concrete example:** `nano notes.txt` opens a beginner-friendly editor.

- **`emacs` terminal mode:** Emacs can run in terminal mode while preserving much of its extensibility.  
  **Concrete example:** `emacs -nw file.txt` opens Emacs without a GUI.

### 10.2 Editor Basics
Editor basics cover the minimum actions needed to modify and save text safely.
They are foundational for configuration, scripting, and troubleshooting work.

- **Opening files:** Opening a file loads its contents into an editor buffer.  
  **Concrete example:** `vim /etc/fstab` opens the mount configuration file.

- **Editing text:** Editing modifies the current buffer by inserting, deleting, or replacing content.  
  **Concrete example:** A user changes an IP address in `/etc/hosts`.

- **Saving files:** Saving writes the modified buffer back to storage.  
  **Concrete example:** In Vim, `:w` saves the current file.

- **Exiting editors:** Exiting closes the editor with or without saving changes.  
  **Concrete example:** In Nano, `Ctrl+X` exits the editor.

- **Search and replace:** Search and replace locates matching text and substitutes new content.  
  **Concrete example:** `:%s/old/new/g` replaces all matches in Vim.

- **Line navigation:** Line navigation moves the cursor quickly to a target line or region.  
  **Concrete example:** `:120` jumps to line 120 in Vim.

## 11. Networking Basics

### 11.1 Network Concepts
Network concepts define addresses, host identity, ports, and protocol layering.
They are the foundation for Linux network configuration and troubleshooting.

- **IP address:** An IP address identifies a host or interface on an IP network.  
  **Concrete example:** `192.168.1.20` is an IPv4 address.

- **MAC address:** A MAC address identifies a network interface at the link layer.  
  **Concrete example:** `52:54:00:ab:cd:ef` is a MAC address.

- **Hostname:** A hostname is a human-readable system name used locally or through DNS.  
  **Concrete example:** `web01` can be the hostname of a server.

- **Ports:** Ports identify transport-layer service endpoints on a host.  
  **Concrete example:** HTTPS commonly uses TCP port 443.

- **Protocols:** Protocols define communication rules at different networking layers.  
  **Concrete example:** TCP provides reliable transport.

- **CIDR notation:** CIDR notation expresses a network and prefix length compactly.  
  **Concrete example:** `192.168.1.0/24` describes a subnet.

### 11.2 Network Commands
Network commands inspect interfaces, routes, sockets, DNS, and connectivity.
They are used for diagnostics, validation, and remote service testing.

- **`ip`:** `ip` manages and displays interfaces, addresses, routes, and neighbors.  
  **Concrete example:** `ip addr show` lists interface addresses.

- **`ifconfig`:** `ifconfig` is an older interface inspection and configuration tool.  
  **Concrete example:** `ifconfig eth0` shows legacy interface details.

- **`ping`:** `ping` tests reachability using ICMP echo messages.  
  **Concrete example:** `ping 8.8.8.8` checks basic IP connectivity.

- **`ss`:** `ss` displays socket and connection state information.  
  **Concrete example:** `ss -tulpn` lists listening TCP and UDP sockets.

- **`netstat`:** `netstat` is an older tool for connections, routes, and interface statistics.  
  **Concrete example:** `netstat -rn` shows the routing table.

- **`traceroute`:** `traceroute` shows the path packets take through intermediate routers.  
  **Concrete example:** `traceroute example.com` displays network hops.

- **`nslookup`:** `nslookup` queries DNS servers for name-resolution information.  
  **Concrete example:** `nslookup openai.com` shows DNS answers.

- **`dig`:** `dig` performs detailed DNS queries and shows protocol-level results.  
  **Concrete example:** `dig MX example.com` retrieves mail exchanger records.

- **`curl`:** `curl` transfers data to or from URLs using many protocols.  
  **Concrete example:** `curl -I https://example.com` fetches HTTP headers.

- **`wget`:** `wget` downloads files or web content from network locations.  
  **Concrete example:** `wget https://example.com/file.tar.gz` downloads a file.

### 11.3 Remote Access
Remote access tools provide encrypted login and file transfer over networks.
SSH-based tooling is the standard method for Linux administration.

- **`ssh`:** `ssh` opens an encrypted remote shell or runs a remote command.  
  **Concrete example:** `ssh admin@server1` logs into a remote host.

- **`scp`:** `scp` copies files between hosts over SSH.  
  **Concrete example:** `scp file.txt admin@server1:/tmp/` copies a file remotely.

- **`sftp`:** `sftp` provides interactive file transfer over SSH.  
  **Concrete example:** `sftp admin@server1` starts a secure transfer session.

- **SSH keys:** SSH keys are asymmetric credentials used for strong and passwordless SSH authentication.  
  **Concrete example:** A user logs in with `~/.ssh/id_ed25519`.

### 11.4 Network Configuration Files
Network configuration files store local host, resolver, and interface settings.
Their exact role depends on the distribution and network-management stack.

- **`/etc/hosts`:** `/etc/hosts` defines static hostname-to-IP mappings.  
  **Concrete example:** `127.0.0.1 localhost` appears in `/etc/hosts`.

- **`/etc/resolv.conf`:** `/etc/resolv.conf` defines DNS resolver settings.  
  **Concrete example:** `nameserver 8.8.8.8` specifies a DNS server.

- **`/etc/hostname`:** `/etc/hostname` stores the persistent local hostname.  
  **Concrete example:** It may contain `db01`.

- **Interface profiles:** Interface configuration files define addresses, routes, and profile settings for devices.  
  **Concrete example:** A NetworkManager profile can define a static IP for `eth0`.

### 11.5 Network Services and Management
Network services and management coordinate interfaces, routing, naming, and firewall interaction.
They connect kernel networking features with higher-level system configuration.

- **`NetworkManager`:** NetworkManager manages network connections and device profiles.  
  **Concrete example:** A laptop Wi-Fi profile is often handled by NetworkManager.

- **`nmcli`:** `nmcli` is the command-line interface to NetworkManager.  
  **Concrete example:** `nmcli connection show` lists configured connections.

- **Routing table:** The routing table determines where packets are sent by destination.  
  **Concrete example:** A default route sends unmatched traffic to the gateway.

- **DNS resolution:** DNS resolution maps hostnames to IP addresses through resolver logic.  
  **Concrete example:** Resolving `example.com` returns an IP address.

- **Firewall interaction:** Firewall rules determine whether packets are allowed or blocked.  
  **Concrete example:** A host firewall can allow SSH but block HTTP.

- **Resolution order:** Name-resolution order defines whether lookups use files, DNS, or other sources first.  
  **Concrete example:** `localhost` may resolve from `/etc/hosts` before DNS.

## 12. Archiving and Compression

### 12.1 Archiving Tools
Archiving tools bundle multiple files into a single structured container for storage or transfer.
They preserve directory structure and often preserve metadata.

- **`tar`:** `tar` creates, extracts, and lists archive contents.  
  **Concrete example:** `tar -cf etc.tar /etc` creates an archive.

- **Archive creation:** Archive creation packages selected files into one archive container.  
  **Concrete example:** `tar -cf logs.tar /var/log` creates a log archive.

- **Archive extraction:** Archive extraction restores archived files into the filesystem.  
  **Concrete example:** `tar -xf backup.tar` extracts files.

- **Archive listing:** Archive listing shows archive contents without extracting them.  
  **Concrete example:** `tar -tf backup.tar` prints member names.

- **Metadata preservation:** Metadata preservation keeps mode, ownership, and timestamps where possible.  
  **Concrete example:** Extracted backup files retain their original permissions.

### 12.2 Compression Tools
Compression tools reduce file size using reversible algorithms with different trade-offs.
They are commonly combined with archiving for backups and distribution.

- **`gzip`:** `gzip` compresses files using the DEFLATE algorithm.  
  **Concrete example:** `gzip server.log` creates `server.log.gz`.

- **`gunzip`:** `gunzip` decompresses gzip-compressed files.  
  **Concrete example:** `gunzip server.log.gz` restores the original log.

- **`bzip2`:** `bzip2` compresses files using a higher-ratio algorithm than gzip.  
  **Concrete example:** `bzip2 data.csv` creates `data.csv.bz2`.

- **`xz`:** `xz` compresses files with strong compression efficiency.  
  **Concrete example:** `xz image.tar` creates `image.tar.xz`.

- **`zip`:** `zip` creates ZIP archives with built-in compression.  
  **Concrete example:** `zip docs.zip *.md` compresses Markdown files.

- **`unzip`:** `unzip` extracts files from ZIP archives.  
  **Concrete example:** `unzip docs.zip` restores the archived files.

### 12.3 Synchronization and Backup Transfer
Synchronization tools copy directory trees efficiently by transferring only changed data.
They are central to backup, mirroring, and remote-update workflows.

- **`rsync`:** `rsync` synchronizes source and destination trees efficiently.  
  **Concrete example:** `rsync -a /data/ backup:/data/` mirrors data remotely.

- **Incremental copying:** Incremental copying transfers only differences instead of the full dataset.  
  **Concrete example:** A daily backup sends only changed files.

- **Metadata preservation:** Synchronization tools can preserve timestamps, modes, owners, and links.  
  **Concrete example:** `rsync -a` preserves timestamps and permissions.

- **Remote synchronization:** Remote synchronization updates a destination on another host over a transport such as SSH.  
  **Concrete example:** `rsync -e ssh` copies data to a backup server.

## 13. Scheduling Tasks

### 13.1 Cron Jobs
Cron jobs execute commands on recurring time-based schedules.
They are widely used for repetitive maintenance and automation tasks.

- **`crontab`:** `crontab` installs, edits, or lists a user's cron table.  
  **Concrete example:** `crontab -e` opens the current user's cron entries.

- **Cron syntax:** Cron syntax uses minute, hour, day, month, and weekday fields.  
  **Concrete example:** `0 2 * * *` means every day at 02:00.

- **User crontabs:** User crontabs are per-user recurring task definitions.  
  **Concrete example:** Alice schedules a nightly report job in her crontab.

- **System crontabs:** System crontabs define system-wide scheduled jobs and may include a user field.  
  **Concrete example:** `/etc/crontab` runs maintenance tasks as root.

- **Cron environment:** Cron runs commands with a minimal environment unless explicitly configured.  
  **Concrete example:** A cron job may need `/usr/bin/python3` instead of `python3`.

### 13.2 One-Time Scheduling
One-time scheduling queues commands for a single future execution.
It is useful for delayed work and ad hoc deferred tasks.

- **`at`:** `at` schedules a command to run once at a specified future time.  
  **Concrete example:** `echo 'reboot' | at 23:00` schedules a reboot.

- **`batch`:** `batch` schedules a command to run when system load permits.  
  **Concrete example:** A large report job can be queued with `batch`.

### 13.3 systemd Timers
systemd timers trigger services according to monotonic or calendar-based schedules.
They integrate scheduling with service management and structured logging.

- **Timer units:** Timer units define when a corresponding service should be activated.  
  **Concrete example:** `backup.timer` triggers `backup.service`.

- **Persistent timers:** Persistent timers can catch up missed runs after downtime.  
  **Concrete example:** A daily timer runs after boot if the system was off at the scheduled time.

- **Calendar expressions:** Calendar expressions define human-readable schedules for timer activation.  
  **Concrete example:** `OnCalendar=*-*-* 02:00:00` means every day at 02:00.

## 14. System Monitoring and Logs

### 14.1 Resource Monitoring
Resource monitoring measures compute, memory, storage, and network usage over time.
It helps identify saturation, leaks, and abnormal workload behavior.

- **CPU usage:** CPU usage measures how much processor time is being consumed.  
  **Concrete example:** `top` may show one process using 95% CPU.

- **Memory usage:** Memory usage measures RAM consumed by processes, caches, and the kernel.  
  **Concrete example:** `free -h` shows available memory.

- **Disk usage:** Disk usage measures space consumed on filesystems or by directories.  
  **Concrete example:** `df -h` may show a filesystem at 90% full.

- **Network usage:** Network usage measures transmitted and received traffic.  
  **Concrete example:** Monitoring may show heavy inbound traffic on `eth0`.

- **Process load:** Process load reflects runnable and waiting task pressure relative to CPU count.  
  **Concrete example:** A load average of 8 on a 2-core host indicates high pressure.

### 14.2 Monitoring Commands
Monitoring commands expose current or historical system performance information.
They help narrow issues down to CPU, memory, disk, or per-process activity.

- **`top`:** `top` shows live CPU, memory, and process usage.  
  **Concrete example:** `top` identifies the process consuming the most resources.

- **`free`:** `free` displays memory and swap usage statistics.  
  **Concrete example:** `free -h` shows used and available memory.

- **`vmstat`:** `vmstat` reports virtual memory, process, and system activity.  
  **Concrete example:** `vmstat 1` shows per-second paging and CPU data.

- **`iostat`:** `iostat` reports CPU and block-device I/O statistics.  
  **Concrete example:** `iostat -xz 1` highlights a saturated disk.

- **`sar`:** `sar` collects and reports historical system activity metrics.  
  **Concrete example:** `sar -u` prints CPU usage history.

- **`iotop`:** `iotop` shows per-process disk I/O usage interactively.  
  **Concrete example:** `iotop` reveals which process is writing heavily to disk.

### 14.3 Log Management
Log management stores, rotates, queries, and retains service and kernel events.
It supports troubleshooting, auditing, and operational history analysis.

- **`/var/log`:** `/var/log` is the standard directory for system and application logs.  
  **Concrete example:** Authentication logs may be stored under `/var/log`.

- **`journalctl`:** `journalctl` queries logs stored by systemd-journald.  
  **Concrete example:** `journalctl -u sshd` shows SSH service logs.

- **Log rotation:** Log rotation archives, compresses, or removes old logs to control growth.  
  **Concrete example:** `logrotate` can rotate a log weekly.

- **`rsyslog`:** `rsyslog` is a syslog daemon for local and remote log collection.  
  **Concrete example:** It can forward logs to a central syslog server.

- **Persistent journal:** Persistent journal storage keeps systemd journal data across reboots.  
  **Concrete example:** Enabling `/var/log/journal` preserves journal history.

### 14.4 Performance Observation
Performance observation interprets metrics to identify the limiting subsystem.
It combines measurements and workload behavior to form root-cause hypotheses.

- **Load average:** Load average measures runnable and waiting tasks over time.  
  **Concrete example:** A load average of 20 on a 4-core host indicates overload.

- **I/O wait:** I/O wait is CPU idle time spent waiting for storage I/O completion.  
  **Concrete example:** High `%wa` suggests storage latency.

- **Context switches:** Context switches count task switches performed by the scheduler.  
  **Concrete example:** Excessive switches can indicate contention or oversubscription.

- **Interrupts:** Interrupts are hardware or software events that trigger immediate kernel handling.  
  **Concrete example:** A high network interrupt rate may indicate heavy packet traffic.

- **Bottleneck identification:** Bottleneck identification determines which subsystem most limits performance.  
  **Concrete example:** High CPU and low disk wait suggest a compute bottleneck.

## 15. Security Basics

### 15.1 Privilege Management
Privilege management controls when and how elevated rights are used on Linux.
Its goal is to minimize the scope and duration of privileged execution.

- **`sudo`:** `sudo` allows approved users to run specific commands with elevated privilege.  
  **Concrete example:** A user runs `sudo apt update`.

- **Root privileges:** Root privileges allow unrestricted administrative operations across the system.  
  **Concrete example:** Root can mount filesystems and kill any process.

- **Least privilege:** Least privilege grants only the minimum rights needed for a task.  
  **Concrete example:** A service account gets read access only to its own config files.

- **Delegated administration:** Delegated administration gives limited admin authority to non-root users.  
  **Concrete example:** A support user may only restart one service.

### 15.2 Firewall Basics
Firewall basics define packet-filtering policy at the host boundary.
Linux firewalls can permit, deny, or classify traffic based on rules.

- **`iptables`:** `iptables` manages legacy Linux packet-filtering and NAT rules.  
  **Concrete example:** A rule can allow inbound SSH on port 22.

- **`nftables`:** `nftables` is the modern Linux packet-filtering framework.  
  **Concrete example:** An nftables ruleset can allow HTTP and HTTPS traffic.

- **`ufw`:** `ufw` is a simplified firewall frontend commonly used on Ubuntu.  
  **Concrete example:** `ufw allow 22/tcp` opens SSH access.

- **`firewalld`:** `firewalld` is a dynamic firewall manager using zones.  
  **Concrete example:** A public zone may allow only SSH and HTTPS.

- **Rule persistence:** Rule persistence ensures firewall configuration survives reboot and reload.  
  **Concrete example:** Saved nftables rules are applied again at boot.

### 15.3 Access Control
Access control layers determine what users and processes are allowed to do.
Linux combines discretionary, mandatory, and authentication-based controls.

- **PAM:** PAM enforces authentication and session policy at login and service boundaries.  
  **Concrete example:** PAM can require stronger password rules.

- **ACLs:** ACLs grant additional per-user or per-group filesystem permissions.  
  **Concrete example:** Alice gets read access to one report through an ACL.

- **SELinux:** SELinux applies label-based mandatory access control.  
  **Concrete example:** SELinux can block Apache from reading arbitrary user files.

- **AppArmor:** AppArmor confines programs using path-based security profiles.  
  **Concrete example:** An AppArmor profile can restrict a service to specific directories.

- **DAC vs MAC:** DAC is controlled by object owners, while MAC is enforced by system policy.  
  **Concrete example:** `chmod` changes DAC, while SELinux enforces MAC rules.

### 15.4 Secure Practices
Secure practices reduce attack surface and improve operational resilience.
They apply to authentication, updates, service exposure, and file protection.

- **Strong passwords:** Strong passwords resist guessing and brute-force attacks.  
  **Concrete example:** A long unique passphrase is stronger than `Password123`.

- **SSH key authentication:** SSH keys provide stronger remote authentication than passwords.  
  **Concrete example:** Logging in with an Ed25519 key avoids password prompts.

- **Regular updates:** Regular updates patch vulnerabilities and software defects.  
  **Concrete example:** Applying kernel updates can close privilege-escalation bugs.

- **Service minimization:** Service minimization disables unnecessary daemons to reduce exposure.  
  **Concrete example:** Disabling an unused FTP server removes an open service.

- **Integrity awareness:** File-integrity awareness helps detect unauthorized changes.  
  **Concrete example:** A checksum mismatch on `/etc/ssh/sshd_config` may indicate tampering.

### 15.5 Auditing and Hardening
Auditing and hardening improve a system's security posture through review and controlled configuration.
They focus on verifying state, reducing risk, and enforcing secure defaults.

- **Login auditing:** Login auditing records and reviews authentication activity.  
  **Concrete example:** Security teams inspect repeated failed SSH logins.

- **Permission audits:** Permission audits verify that file and directory modes match policy.  
  **Concrete example:** Auditing finds world-writable config files.

- **Service hardening:** Service hardening reduces privileges, exposure, and unnecessary capabilities.  
  **Concrete example:** A daemon runs as a dedicated non-root user.

- **Secure defaults:** Secure defaults start systems from a safer baseline.  
  **Concrete example:** Password login over SSH is disabled on a hardened image.

## 16. Shell Scripting Basics

### 16.1 Script Fundamentals
Script fundamentals define how shell scripts are declared, executed, and documented.
They provide the base concepts for automating shell workflows.

- **Shebang:** A shebang specifies the interpreter used to run a script file.  
  **Concrete example:** `#!/bin/bash` selects Bash.

- **Executable scripts:** An executable script has the execute bit set so it can run directly.  
  **Concrete example:** `chmod +x backup.sh` makes the script executable.

- **Running scripts:** Scripts can be run directly or passed to a shell interpreter.  
  **Concrete example:** `./backup.sh` and `bash backup.sh` both run a script.

- **Comments:** Comments document script logic and are ignored by the shell.  
  **Concrete example:** `# rotate old logs` documents a script section.

- **Interpreter selection:** Interpreter selection determines which shell syntax rules apply.  
  **Concrete example:** A Bash script should not use `#!/bin/sh` if it relies on arrays.

### 16.2 Variables and Input
Variables and input are the main data mechanisms in shell scripts.
They include positional parameters, user input, and inherited environment values.

- **Positional parameters:** Positional parameters expose the command-line arguments passed to a script.  
  **Concrete example:** `$1` is the first script argument.

- **Command-line arguments:** Command-line arguments are external values supplied at invocation time.  
  **Concrete example:** `./deploy.sh prod` passes `prod` as an argument.

- **Reading input:** Reading input captures data from standard input into shell variables.  
  **Concrete example:** `read name` stores typed input in `name`.

- **Exit status:** Exit status reports whether a command or script succeeded.  
  **Concrete example:** `if [ $? -ne 0 ]; then ... fi` checks the previous command.

- **Environment inheritance:** Exported variables are inherited by child processes.  
  **Concrete example:** A script inherits `PATH` from its parent shell.

### 16.3 Control Structures
Control structures express decision-making and repetition in shell scripts.
They allow scripts to branch, iterate, and organize behavior.

- **`if`:** `if` conditionally executes commands based on a test result.  
  **Concrete example:** `if [ -f file.txt ]; then echo yes; fi`.

- **`case`:** `case` selects behavior based on pattern matching against a value.  
  **Concrete example:** `case "$1" in start|stop) ... esac`.

- **`for`:** `for` iterates over a list of values.  
  **Concrete example:** `for f in *.log; do gzip "$f"; done`.

- **`while`:** `while` repeats commands while a condition succeeds.  
  **Concrete example:** `while read line; do echo "$line"; done < file.txt`.

- **Functions:** Functions encapsulate reusable script logic under a name.  
  **Concrete example:** A function named `backup()` can wrap backup commands.

- **`until`:** `until` repeats commands until a condition succeeds.  
  **Concrete example:** `until ping -c1 host; do sleep 1; done`.

### 16.4 Script Utilities
Script utilities are common shell constructs used to test conditions, capture output, and return results.
They support concise, composable, and reusable script logic.

- **Test expressions:** Test expressions evaluate file, string, or numeric conditions.  
  **Concrete example:** `[ -d /var/log ]` checks whether the path is a directory.

- **Loops:** Loops repeat commands over data or while a condition remains true.  
  **Concrete example:** A loop compresses each rotated log file.

- **Functions:** Functions package repeated shell logic for reuse within a script.  
  **Concrete example:** A `log()` function standardizes status messages.

- **Return codes:** Return codes are numeric values indicating function or command outcome.  
  **Concrete example:** `return 1` signals failure from a shell function.

- **Command substitution:** Command substitution captures command output for reuse in variables or expressions.  
  **Concrete example:** `today=$(date +%F)` stores the current date.

### 16.5 Defensive Scripting Practices
Defensive scripting practices make shell scripts safer, clearer, and more predictable.
They reduce failures caused by missing variables, bad quoting, and hidden errors.

- **Variable quoting:** Quoting variables prevents unwanted splitting and glob expansion.  
  **Concrete example:** `cp "$src" "$dst"` safely handles spaces in filenames.

- **Exit-code checks:** Checking exit codes ensures scripts react correctly to failures.  
  **Concrete example:** A script aborts if `rsync` returns a nonzero code.

- **`set -e`:** `set -e` causes many failing commands to terminate the script.  
  **Concrete example:** A failed `cp` stops the script immediately.

- **`set -u`:** `set -u` treats unset variables as errors.  
  **Concrete example:** Referencing `$DEST` before setting it aborts the script.

- **`set -o pipefail`:** `pipefail` makes a pipeline fail if any command within it fails.  
  **Concrete example:** `grep pattern file | sort` returns failure if `grep` fails.

## 17. Device and Hardware Management

### 17.1 Device Files
Device files are special filesystem entries that expose kernel device interfaces to user space.
They represent hardware devices or virtual kernel services under `/dev`.

- **`/dev`:** `/dev` is the directory that contains device nodes.  
  **Concrete example:** `/dev/sda` appears under `/dev`.

- **Character devices:** Character devices provide stream-oriented access to hardware or pseudo-devices.  
  **Concrete example:** `/dev/ttyS0` is a serial character device.

- **Block devices:** Block devices provide buffered random-access I/O to storage hardware.  
  **Concrete example:** `/dev/nvme0n1` is a block device.

- **Major and minor numbers:** Major and minor numbers identify the driver and specific device instance.  
  **Concrete example:** `ls -l /dev/null` shows device numbers.

### 17.2 Hardware Inspection
Hardware inspection tools enumerate CPUs, buses, storage, memory, and peripheral devices.
They are used to verify inventory and driver recognition.

- **`lspci`:** `lspci` lists PCI devices detected by the kernel.  
  **Concrete example:** `lspci` shows the Ethernet controller model.

- **`lsusb`:** `lsusb` lists USB devices connected to the system.  
  **Concrete example:** A USB flash-drive controller appears in `lsusb`.

- **`lscpu`:** `lscpu` reports CPU architecture and topology information.  
  **Concrete example:** It shows core count and CPU model.

- **`lsmem`:** `lsmem` displays memory-block layout and size information.  
  **Concrete example:** `lsmem` shows online memory ranges.

- **`dmidecode`:** `dmidecode` reads firmware-provided hardware inventory data.  
  **Concrete example:** It can show the BIOS version and system serial number.

- **`lsblk`:** `lsblk` lists block devices, partitions, and mount relationships.  
  **Concrete example:** `lsblk` shows an NVMe disk and its partitions.

### 17.3 Disk and Partition Tools
Disk and partition tools create layouts, build filesystems, and verify integrity.
They operate near the storage layer and must be used carefully.

- **`fdisk`:** `fdisk` creates and edits partition tables on disks.  
  **Concrete example:** `fdisk /dev/sdb` modifies partitions on a disk.

- **`parted`:** `parted` manages partition tables and large-disk layouts.  
  **Concrete example:** `parted /dev/sdb print` shows partition layout.

- **`mkfs`:** `mkfs` creates a filesystem on a partition or device.  
  **Concrete example:** `mkfs.ext4 /dev/sdb1` formats a partition.

- **`fsck`:** `fsck` checks and repairs filesystem consistency.  
  **Concrete example:** `fsck /dev/sdb1` checks a filesystem after an improper shutdown.

- **`tune2fs`:** `tune2fs` adjusts EXT-family filesystem parameters.  
  **Concrete example:** `tune2fs -l /dev/sda1` lists EXT4 settings.

### 17.4 Device Events and Management
Device-event management responds to hardware addition, removal, and discovery.
It helps ensure stable naming, permissions, and policy-driven setup.

- **`udev`:** `udev` is the userspace device manager that handles device events and rules.  
  **Concrete example:** `udev` creates `/dev` nodes for a newly attached disk.

- **`udevadm`:** `udevadm` inspects and monitors device events and udev state.  
  **Concrete example:** `udevadm monitor` watches hotplug activity.

- **Hotplug handling:** Hotplug handling detects and configures devices added while the system is running.  
  **Concrete example:** Plugging in a USB drive triggers hotplug processing.

- **Persistent naming:** Persistent naming gives devices stable identifiers independent of discovery order.  
  **Concrete example:** `/dev/disk/by-uuid/...` identifies a filesystem consistently.

## 18. Boot and Startup

### 18.1 Boot Process
The boot process is the sequence from firmware startup to a usable Linux environment.
It transitions from hardware initialization into kernel control and then normal user space.

- **Firmware stage:** Firmware performs early hardware initialization and locates boot code.  
  **Concrete example:** UEFI initializes devices and loads a bootloader.

- **Bootloader stage:** The bootloader selects and loads the kernel and initramfs.  
  **Concrete example:** GRUB presents a menu of available kernels.

- **Kernel stage:** The kernel initializes memory, drivers, and core subsystems.  
  **Concrete example:** The kernel detects CPUs and storage controllers.

- **Init stage:** The init stage starts PID 1 and launches configured services.  
  **Concrete example:** systemd begins starting service dependencies.

- **User session startup:** User session startup begins after system services are available and a user logs in.  
  **Concrete example:** SSH login starts a user shell session.

### 18.2 Bootloaders
Bootloaders bridge firmware and kernel startup by loading the selected OS image.
They also expose menu selection and kernel-parameter control.

- **GRUB:** GRUB is a common Linux bootloader supporting multiple kernels and recovery options.  
  **Concrete example:** GRUB can boot an older kernel version.

- **Boot menu:** The boot menu presents available operating systems or kernels for selection.  
  **Concrete example:** A dual-boot machine shows Linux and Windows entries.

- **Kernel parameters:** Boot-time kernel parameters modify kernel behavior during startup.  
  **Concrete example:** `systemd.unit=rescue.target` changes boot mode.

- **Recovery entries:** Recovery entries boot the system in a reduced maintenance-oriented mode.  
  **Concrete example:** A recovery entry may start a minimal shell for repairs.

### 18.3 Startup Systems
Startup systems control PID 1 behavior, dependency ordering, and service supervision.
They determine how the system reaches its operational target after kernel boot.

- **SysVinit:** SysVinit is a traditional init system based on startup scripts and runlevels.  
  **Concrete example:** Services start from `/etc/init.d/` under SysVinit.

- **Upstart:** Upstart is an event-driven init system used historically on some distributions.  
  **Concrete example:** Older Ubuntu releases used Upstart before systemd.

- **systemd:** systemd is a modern init system with unit-based dependency management.  
  **Concrete example:** systemd starts `nginx.service` after networking is ready.

- **Dependency model:** Unit dependency rules control service ordering and requirements.  
  **Concrete example:** A service can declare that it requires the network target.

### 18.4 Runlevels and Targets
Runlevels and targets define named operational system states.
They represent historical SysV modes and their systemd equivalents.

- **Traditional runlevels:** Traditional runlevels are numbered boot states such as single-user or multi-user.  
  **Concrete example:** Runlevel 3 often meant text-mode multi-user operation.

- **systemd targets:** systemd targets are named synchronization points grouping related units.  
  **Concrete example:** `graphical.target` represents a graphical multi-user state.

- **Rescue mode:** Rescue mode starts a minimal environment for maintenance and repair.  
  **Concrete example:** An admin enters rescue mode to fix `/etc/fstab`.

- **Multi-user mode:** Multi-user mode provides normal non-graphical networked operation.  
  **Concrete example:** Servers often boot into `multi-user.target`.

- **Graphical mode:** Graphical mode includes the display manager and desktop services.  
  **Concrete example:** A workstation boots into a graphical login screen.

- **Emergency mode:** Emergency mode starts the smallest possible shell environment for critical recovery.  
  **Concrete example:** Emergency mode can help repair a broken root mount.

### 18.5 Boot Troubleshooting
Boot troubleshooting identifies failures that prevent the system from reaching its target state.
It uses logs, boot parameters, service failures, and early-boot diagnostics.

- **initramfs troubleshooting:** initramfs troubleshooting focuses on failures before the real root filesystem mounts.  
  **Concrete example:** Missing storage drivers can drop the system into an initramfs shell.

- **Failed unit analysis:** Failed unit analysis examines which startup units failed and why.  
  **Concrete example:** `systemctl --failed` lists units that failed after boot.

- **Boot logs:** Boot logs record kernel and init-system events during startup.  
  **Concrete example:** `journalctl -b` shows logs from the current boot.

- **Kernel panic basics:** A kernel panic is a fatal kernel error that halts normal operation.  
  **Concrete example:** An unrecoverable root-filesystem failure can trigger a panic.

## 19. Linux Administration Basics

### 19.1 System Configuration
System configuration sets foundational host attributes that affect system behavior and identity.
These settings influence naming, localization, timekeeping, and service defaults.

- **Hostname configuration:** Hostname configuration defines the persistent system name.  
  **Concrete example:** `hostnamectl set-hostname web01` changes the hostname.

- **Time configuration:** Time configuration controls system clock synchronization and accuracy.  
  **Concrete example:** NTP keeps the clock aligned with network time sources.

- **Locale configuration:** Locale configuration defines language, encoding, and regional formatting.  
  **Concrete example:** `LANG=en_US.UTF-8` selects a locale.

- **Environment configuration:** Environment configuration sets variables and defaults for shells and services.  
  **Concrete example:** `/etc/environment` can define global variables.

- **Timezone configuration:** Timezone configuration sets the local interpretation of wall-clock time.  
  **Concrete example:** `timedatectl set-timezone America/New_York` changes the timezone.

### 19.2 Backup and Restore
Backup and restore protect system data and configuration against loss or corruption.
A sound strategy includes creation, verification, retention, and recovery testing.

- **File backups:** File backups create copies of important files or directories for recovery.  
  **Concrete example:** `/etc` is copied nightly to backup storage.

- **Archive backups:** Archive backups package many files into one backup file.  
  **Concrete example:** `tar -czf etc-backup.tar.gz /etc` creates an archive backup.

- **Snapshot concepts:** Snapshots capture point-in-time views of data or logical volumes.  
  **Concrete example:** An LVM snapshot is created before a risky upgrade.

- **Restore operations:** Restore operations recover files or systems from backup copies.  
  **Concrete example:** A deleted config file is restored from last night's backup.

- **Backup verification:** Backup verification confirms that backup data is complete and recoverable.  
  **Concrete example:** Periodic test restores validate backup integrity.

### 19.3 Troubleshooting Basics
Troubleshooting basics apply structured checks to isolate faults efficiently and safely.
They emphasize verifying assumptions before deeper diagnostic work.

- **Reading logs:** Reading logs reveals service errors, warnings, and event context.  
  **Concrete example:** `journalctl -u nginx` shows why Nginx failed.

- **Checking service status:** Service status checks show whether a service is active, failed, or restarting.  
  **Concrete example:** `systemctl status sshd` shows SSH daemon state.

- **Verifying disk space:** Disk-space verification confirms whether capacity exhaustion is involved.  
  **Concrete example:** `df -h` shows `/var` at 100% usage.

- **Checking permissions:** Permission checks determine whether access denial is caused by mode or ownership.  
  **Concrete example:** `ls -l` reveals a missing execute bit on a script.

- **Network troubleshooting:** Network troubleshooting validates interfaces, routes, DNS, and connectivity.  
  **Concrete example:** `ping gateway` checks local connectivity.

- **Dependency troubleshooting:** Dependency troubleshooting finds missing services, libraries, or startup-order issues.  
  **Concrete example:** A web app fails because the database service is down.

### 19.4 Recovery Concepts
Recovery concepts provide controlled ways to restore access or repair boot failures.
They are used when standard login or normal startup is unavailable.

- **Single-user recovery:** Single-user recovery starts a minimal maintenance environment with limited services.  
  **Concrete example:** An admin repairs `/etc/fstab` in single-user mode.

- **Rescue environment:** A rescue environment is a stripped-down system used to repair another installation.  
  **Concrete example:** A live USB is used to chroot into a broken system.

- **Boot-parameter troubleshooting:** Boot parameters can change startup behavior to bypass or diagnose failures.  
  **Concrete example:** Booting with `systemd.unit=rescue.target` avoids full startup.

- **Password reset workflows:** Password reset workflows restore account access through approved recovery methods.  
  **Concrete example:** The root password is reset from rescue mode.

## 20. Advanced Linux Concepts

### 20.1 Virtualization and Containers
Virtualization and containers isolate workloads using different levels of abstraction.
Virtual machines emulate hardware, while containers isolate processes within one kernel.

- **KVM:** KVM is the Linux kernel virtualization module for hardware-assisted virtual machines.  
  **Concrete example:** A Linux host runs a guest VM using KVM.

- **QEMU:** QEMU emulates or virtualizes machine hardware and often pairs with KVM.  
  **Concrete example:** `qemu-system-x86_64` launches a VM.

- **Docker:** Docker is a platform for building and running containerized applications.  
  **Concrete example:** `docker run nginx` starts an Nginx container.

- **LXC:** LXC is a userspace interface for system containers built on namespaces and cgroups.  
  **Concrete example:** An LXC container can run a full init system.

- **Namespaces:** Namespaces isolate resource views such as PIDs, mounts, and networking.  
  **Concrete example:** A container sees its own process tree.

- **cgroups:** cgroups enforce and account for resource usage across grouped processes.  
  **Concrete example:** A container can be limited to 512 MB RAM.

- **Container runtime concepts:** Container runtimes create isolated process environments from images and policies.  
  **Concrete example:** A runtime mounts an image filesystem and launches PID 1 inside the container.

### 20.2 File System Advanced Concepts
Advanced storage concepts add abstraction, redundancy, and recovery features beyond basic filesystems.
They are used in scalable and fault-tolerant storage designs.

- **Journaling:** Journaling logs updates to support faster and safer crash recovery.  
  **Concrete example:** EXT4 replays journal entries after power loss.

- **EXT4:** EXT4 is a mature journaling filesystem widely used on Linux.  
  **Concrete example:** A root filesystem is formatted as EXT4.

- **XFS:** XFS is a high-performance journaling filesystem for large storage workloads.  
  **Concrete example:** A large data partition uses XFS.

- **Btrfs:** Btrfs supports snapshots, checksums, and copy-on-write semantics.  
  **Concrete example:** A system creates a Btrfs snapshot before updating.

- **ZFS concepts:** ZFS combines pooled storage, checksumming, snapshots, and integrated volume management.  
  **Concrete example:** A ZFS pool spans multiple disks.

- **Swap:** Swap extends virtual memory using disk-backed storage.  
  **Concrete example:** A busy system uses swap during RAM pressure.

- **RAID:** RAID combines multiple disks for redundancy or performance.  
  **Concrete example:** RAID1 mirrors data across two disks.

- **LVM:** LVM abstracts physical storage into flexible logical volumes.  
  **Concrete example:** A logical volume is extended online.

- **Snapshots:** Snapshots capture point-in-time states of filesystems or logical volumes.  
  **Concrete example:** A snapshot is used for rollback after a failed patch.

### 20.3 Performance Tuning
Performance tuning adjusts system behavior to improve throughput, latency, or efficiency.
Effective tuning depends on measured bottlenecks rather than guesswork.

- **CPU tuning:** CPU tuning adjusts scheduler behavior, affinity, and workload placement.  
  **Concrete example:** Pinning a latency-sensitive task to one core reduces jitter.

- **Memory tuning:** Memory tuning adjusts cache, reclaim, and swap behavior for workloads.  
  **Concrete example:** Swappiness is reduced on a database server.

- **I/O tuning:** I/O tuning adjusts block-layer or filesystem behavior to improve storage performance.  
  **Concrete example:** A queue scheduler is tuned for SSD workloads.

- **Kernel tuning:** Kernel tuning changes runtime parameters affecting networking, memory, or scheduling.  
  **Concrete example:** `sysctl` enables TCP parameter tuning.

- **Workload profiling:** Workload profiling measures where time and resources are spent in an application.  
  **Concrete example:** `perf top` identifies hot functions in a CPU-bound process.

### 20.4 Automation and Configuration Management
Automation and configuration management encode desired system state into repeatable workflows.
They improve consistency, auditability, and fleet-scale administration.

- **Bash automation:** Bash automation uses shell scripts to perform repeatable admin tasks.  
  **Concrete example:** A shell script rotates logs and uploads backups.

- **Ansible:** Ansible automates system configuration using declarative playbooks over SSH.  
  **Concrete example:** An Ansible playbook installs Nginx on multiple hosts.

- **Puppet:** Puppet manages desired state through manifests and an agent-based model.  
  **Concrete example:** Puppet enforces package and service state on servers.

- **Chef:** Chef automates infrastructure configuration through recipes and cookbooks.  
  **Concrete example:** Chef provisions a web application stack.

- **Infrastructure as code:** Infrastructure as code expresses system and infrastructure configuration in version-controlled files.  
  **Concrete example:** Server provisioning definitions are stored in Git.

- **Idempotent configuration:** Idempotent configuration ensures repeated application reaches the same final state.  
  **Concrete example:** Running the same Ansible playbook twice leaves the system unchanged after the first success.
