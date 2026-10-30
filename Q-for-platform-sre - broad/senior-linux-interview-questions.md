Senior Linux, Bash, and PowerShell Interview Question Set
1. Core senior screening questions across Linux, Bash, and PowerShell
1. How do you decide whether a problem should be solved with a shell script, a PowerShell script, Python, or a configuration management tool?
2. What makes a script “production-grade” in a platform engineering or SRE environment?
3. How do you distinguish between a host issue, a service issue, and a deployment issue when production is degraded?
4. What are the most common reasons a script works interactively but fails in automation?
5. How do you approach building operational tooling that must work across different Linux distributions and Windows versions?
6. What is your strategy for writing automation that is safe to run repeatedly in production?
7. How do you balance speed of delivery with maintainability when building operational scripts?
8. How do you design scripts so they are observable, debuggable, and auditable in incident scenarios?
9. What are the biggest reliability pitfalls you have seen in Bash and PowerShell automation?
10. How would you standardize shell-based operations across multiple teams without blocking local flexibility?

2. Linux internals, process model, scheduling, memory, filesystem, permissions, and kernel-facing troubleshooting
1. How do Linux processes transition between running, sleeping, uninterruptible sleep, stopped, zombie, and dead states?
2. What causes zombie processes, and when are they a symptom of a deeper problem?
3. How does reparenting work in Linux, and what role does PID 1 play in orphaned processes?
4. What would you check first if a process is stuck in D state for a long time?
5. How do signals interact with multi-threaded processes, process groups, and session leadership?
6. What are the practical effects of nice values, scheduling policy, and priority on production workloads?
7. How do cgroups affect CPU, memory, and I/O behavior in modern Linux systems?
8. How would you investigate a host that shows high CPU load but no single obvious top consumer?
9. What does it mean when load average is high but CPU utilization appears moderate?
10. How do you reason about page cache, swap, and memory pressure when free memory appears low?
11. What symptoms suggest reclaim pressure, thrashing, or memory fragmentation rather than a true leak?
12. How would you investigate a suspected memory leak in a long-running service without immediately restarting it?
13. What is NUMA awareness, and when does it matter operationally?
14. How do file descriptors behave across forks, execs, and service restarts?
15. What are ulimits, and what production problems can arise when file descriptor, process, or memory limits are too low?
16. How do you identify whether a filesystem problem is at the block layer, filesystem layer, or application layer?
17. What is journaling intended to protect against, and what problems does it not solve?
18. How do mount options influence service behavior, performance, or failure modes?
19. What is inode exhaustion, and how does it differ from disk space exhaustion?
20. How do permissions, ownership, ACLs, sticky bits, setuid, and setgid interact in real systems?
21. How would you troubleshoot a permission issue that only appears after deployment to production?
22. What kinds of bugs come from package version drift, library mismatches, or kernel/userland assumptions?
23. How do you investigate why a binary behaves differently on two hosts that appear to be similarly configured?
24. What is the operational impact of overlay or union filesystems in containerized environments?
25. How do you distinguish between an OS kernel issue and an application bug when a host becomes unstable under load?

3. Advanced Bash scripting, shell behavior, process control, text processing, safety, and maintainability
1. How do quoting, word splitting, globbing, and command substitution affect correctness in Bash scripts?
2. What is the difference between $* and $@, and when can misuse cause production bugs?
3. How do you structure a Bash script so it remains maintainable after the first incident use case?
4. When do set -e, set -u, and pipefail help, and when can they create misleading behavior?
5. How do you handle traps for cleanup, signal handling, and error reporting in Bash?
6. What are the pitfalls of subshells, especially when modifying variables or state?
7. How do pipeline exit codes work, and how do you preserve the correct failure signal in multi-stage pipelines?
8. When would you use arrays in Bash, and when would you avoid them?
9. How do you safely parse command output that may contain spaces, tabs, newlines, or special characters?
10. How do you choose between grep, sed, awk, cut, sort, jq, and shell built-ins for operational parsing?
11. When does text-processing logic become fragile enough that you would rewrite it in a different language?
12. How do you handle retries in Bash without causing duplicate side effects?
13. How do you implement locking in shell scripts to prevent concurrent execution issues?
14. How do you safely use temporary files and directories in scripts that may fail partway through?
15. What race conditions have you seen in shell automation, and how would you avoid them?
16. How do you write defensive Bash around destructive operations such as deletes, moves, and truncation?
17. What is the danger of assuming xargs will handle all input safely, and how do you mitigate it?
18. How do you manage logging, verbosity, and structured output in Bash for incident diagnostics?
19. How do you test Bash scripts before they are trusted in production automation?
20. What patterns make Bash scripts easier to review, maintain, and reuse by other engineers?

4. Advanced PowerShell scripting, objects vs text, remoting, modules, error handling, and enterprise automation
1. How does PowerShell’s object pipeline differ from a text pipeline, and why does that matter for automation quality?
2. How do you decide when to use properties, methods, filtering, and formatting in PowerShell?
3. What are advanced functions, and how do they compare with simple scripts and compiled modules?
4. How do you design reusable PowerShell modules for enterprise use?
5. What are the practical differences between profile-based customization and portable module-based tooling?
6. How do execution policy, script signing, and trust boundaries affect PowerShell deployment?
7. How do you handle structured error handling in PowerShell so failures are actionable and not silent?
8. What is the difference between terminating and non-terminating errors, and why does it matter in production scripts?
9. How do you approach remoting failures involving authentication, certificates, delegation, or constrained endpoints?
10. What are the main operational differences between WinRM-based remoting and other management patterns?
11. How do CIM and WMI differ operationally, and when would you prefer one over the other?
12. How do jobs, background tasks, and scheduled jobs differ in reliability and observability?
13. How do you manage transcripts and logging for compliance-heavy enterprise automation?
14. What are the limitations of serialized objects in remoting scenarios?
15. How do you design PowerShell scripts to work across Windows and Linux with PowerShell Core?
16. What cross-platform pitfalls have you seen with path handling, line endings, encoding, and command availability?
17. How do you securely handle credentials, secrets, and tokens in PowerShell automation?
18. What is Just Enough Administration, and how does it change how you design privileged automation?
19. How do you avoid overusing Invoke-Command or remote imperative scripts where declarative management would be safer?
20. How do you make PowerShell automation idempotent and safe to rerun in production?

5. Cross-platform operations: Linux vs Windows administration patterns, interoperability, and shell strategy
1. What are the main differences in operational thinking between Linux administration and Windows administration?
2. How do you design a single operational workflow that spans Linux and Windows without forcing lowest-common-denominator tooling?
3. When would you prefer native Linux tooling, Windows native administration, or a cross-platform abstraction?
4. How do path conventions, separators, environment variables, and quoting rules differ across shells and platforms?
5. What are the most common interoperability issues between Bash scripts and PowerShell scripts?
6. How do locale, encoding, and line-ending differences break automation in mixed estates?
7. How do you standardize identity, privilege, and access control across Linux and Windows systems?
8. How do you troubleshoot a cross-platform script that works on one OS but fails on the other because of command semantics rather than syntax?
9. What are the trade-offs between using PowerShell everywhere versus using Bash on Linux and PowerShell on Windows?
10. When is a platform-specific solution better than a “unified” cross-platform script?
11. How do you handle inventory, drift detection, and compliance across heterogeneous hosts?
12. How do you think about portability versus operational depth when selecting shell strategy?
13. What problems are created by relying too heavily on SSH or remoting as a universal control plane?
14. How would you design a runbook that is clear to operators working on both Linux and Windows?
15. What differences in service management, logging, and scheduling most often create cross-platform surprises?

6. Service management, boot, systemd, cron/timers, scheduled tasks, startup issues, and runtime control
1. How do you investigate a service that starts manually but fails under systemd or a scheduled task?
2. What differences in environment, user context, PATH, working directory, and permissions commonly explain startup failures?
3. How do you reason about systemd unit dependencies and ordering when a service starts too early or too late?
4. What is the practical difference between After=, Requires=, Wants=, and PartOf= in service design?
5. How do you determine whether a systemd restart policy is masking an underlying failure?
6. What evidence would you gather before changing a unit file in production?
7. How do you use journal data effectively during service startup troubleshooting?
8. What kinds of issues are exposed by systemctl status but not by the application logs alone?
9. How do you debug a service that behaves differently when launched by init, cron, a timer, or a human operator?
10. When would you choose cron over systemd timers, and when is the opposite true?
11. What are the trade-offs between distributed host-based scheduling and centralized job orchestration?
12. How do you troubleshoot a scheduled task that works manually but fails at the scheduled time?
13. What boot-time issues can delay or prevent services from becoming healthy?
14. How do you investigate startup failures caused by filesystem availability, network readiness, or name resolution dependencies?
15. What is your strategy for stopping, reloading, or restarting critical services during incident response without hiding evidence?
16. How do you reduce blast radius when a service must remain active but is misbehaving?
17. What operational risks come from poorly ordered boot scripts or legacy init assumptions?
18. How do you detect and prevent runtime drift caused by environment differences across restarts?

7. Networking, DNS, routing, sockets, firewall behavior, name resolution, and connectivity troubleshooting
1. How would you troubleshoot intermittent DNS failures that appear environment-specific?
2. What evidence would you gather before deciding a connectivity issue is caused by the network rather than the application?
3. How do you distinguish DNS resolution problems from routing, firewall, or service listener problems?
4. What are the common causes of a service being reachable locally but not remotely?
5. How do sockets, ports, listeners, and connection states help you reason about connectivity issues?
6. What are the practical differences between inbound firewall rules, outbound restrictions, and security groups?
7. How do you investigate connection failures that only happen under load or during failover?
8. What can cause name resolution to succeed interactively but fail in automation or on a scheduled job?
9. How do /etc/hosts, DNS search suffixes, resolver configuration, and caching interact in troubleshooting?
10. How do you reason about routing tables, default gateways, asymmetric routing, and multi-homed hosts?
11. How do you distinguish packet loss, latency, MTU issues, and application-level timeouts?
12. What clues indicate that a connection problem is caused by SSL/TLS or certificate validation rather than basic networking?
13. How do you troubleshoot intermittent failures through proxies, load balancers, or NAT?
14. What is your approach when a host can resolve names but cannot reach a specific service despite “open” firewall rules?
15. How do you verify whether the OS, the network path, or the remote endpoint is the limiting factor?

8. Storage, filesystems, mounts, permissions, ACLs, quotas, inodes, log growth, and disk-pressure scenarios
1. How do you distinguish disk full from inode full, and why does that distinction matter operationally?
2. What would you check first when a host reports low disk space but deleting files does not recover space?
3. How do deleted-but-open files create hidden disk pressure?
4. How do you identify which process is holding space or inodes open?
5. How do mount points, bind mounts, and remounts complicate storage troubleshooting?
6. What filesystem or mount options would you examine when investigating application writes, latency, or unexpected failures?
7. How do you handle log growth in a way that avoids losing evidence during an incident?
8. How do quotas affect production behavior, and what failure patterns do they produce?
9. How do permissions and ACL mismatches lead to “disk” or “filesystem” issues that are actually access issues?
10. How do sticky bits, setuid, and setgid affect operational safety on shared directories?
11. What are the implications of overlay or union filesystems for writes, visibility, and troubleshooting?
12. How do you investigate an application that writes successfully on one filesystem but fails on another?
13. What symptoms suggest underlying storage latency rather than a simple lack of free space?
14. How do journaled filesystems change your approach to crash recovery and post-incident analysis?
15. What evidence would you gather before clearing logs, truncating files, or moving data during an incident?
16. How do you recover safely from storage pressure without destroying the root cause evidence?

9. Performance, observability, logging, system metrics, CPU/memory/IO pressure, and debugging under load
1. How do you diagnose high CPU when aggregate metrics are misleading and no obvious owning process stands out?
2. What is the difference between load average, CPU utilization, run queue pressure, and user-visible latency?
3. How do you investigate memory pressure when free memory looks low but the system may still be healthy?
4. What symptoms suggest CPU contention versus I/O wait versus lock contention?
5. How do you analyze a system that is slow only under production load but appears normal in lower environments?
6. How do you determine whether performance symptoms originate in the OS, the service runtime, the filesystem, the network, or a downstream dependency?
7. What metrics would you prioritize during an incident involving process stalls or thread starvation?
8. How do you use logs, metrics, traces, and kernel-level evidence together during diagnosis?
9. What is the operational value of per-process, per-thread, and per-cgroup visibility?
10. How would you investigate a service that is healthy but causing the host to become unresponsive?
11. How do you reason about I/O pressure, blocked processes, and device saturation?
12. What evidence would you gather before killing a process that appears to be the top CPU consumer?
13. How do you detect whether a reboot is likely to fix symptoms temporarily but not the underlying problem?
14. What logging patterns make post-incident analysis easier versus harder?
15. How do you design observability into scripts and operational tooling so they are useful during degradation?
16. What changes would you make after the incident to reduce mean time to diagnose future issues?

10. Automation design, idempotency, script reliability, environment drift, and operational safety
1. What does idempotency mean in operational automation, and why is it critical for senior platform work?
2. How do you design scripts so repeated runs do not create partial state or hidden drift?
3. How do you manage environment drift across hosts, regions, or teams?
4. What patterns help ensure a script can fail safely and resume cleanly?
5. How do you separate validation, planning, execution, and verification in automation?
6. How do you build guardrails for destructive or high-blast-radius operations?
7. What is the risk of using one-off scripts in long-lived production environments?
8. How do you implement retries without causing duplicate side effects or inconsistent state?
9. How do you make automation observable enough to support incident response and auditability?
10. What is your approach to configuration drift caused by manual changes during emergencies?
11. How do you reduce operational risk when automating across large numbers of servers?
12. When should a script refuse to continue, and when should it degrade gracefully?
13. How do you handle concurrency, locking, and coordination in distributed automation?
14. What evidence should automation record to support later investigation?
15. How do you prevent “automation success” from masking a partially failed backend change?
16. What trade-offs do you make between reusability, strictness, and ease of use in automation design?

11. Security, privilege boundaries, sudo/UAC, secret handling, auditability, hardening, and compliance-focused scripting
1. How do you design shell automation with least privilege rather than defaulting to broad admin rights?
2. What are the operational drawbacks of sudo-heavy workflows?
3. How do you safely separate privileged and non-privileged steps in scripts?
4. How do UAC and Windows privilege boundaries change the behavior of PowerShell automation?
5. How do you handle secret storage, retrieval, rotation, and exposure prevention in operational scripts?
6. What are the risks of embedding credentials directly in Bash or PowerShell scripts?
7. How do you make automation auditable without leaking sensitive data into logs?
8. What is your approach to redacting secrets in transcripts, shell history, CI logs, and job output?
9. How do Just Enough Administration and constrained endpoints reduce blast radius?
10. How do file permissions, ACLs, and ownership affect secret exposure on Linux and Windows?
11. What are the security trade-offs between remote interactive administration and controlled automation?
12. How do you design scripts to avoid privilege escalation through path hijacking, injection, or unsafe temp file usage?
13. How do you harden production scripts against command injection and accidental destructive execution?
14. What compliance requirements have you had to account for in automation design?
15. How do you verify that a hardening change did not break essential operations?
16. What changes would you make to an operational workflow if auditability became as important as speed?

12. Incident response, live troubleshooting, degraded hosts, production recovery, and evidence-first diagnosis
1. A critical service is failing intermittently in production. What would you check first, and why?
2. What evidence would you gather before killing processes, clearing files, restarting services, changing unit files, rotating logs, or rebooting a host?
3. How do you decide whether to preserve the failing state for investigation or take immediate recovery action?
4. How would you determine whether the problem is in the OS, service manager, shell script, runtime environment, filesystem, network path, DNS, permissions model, package set, or human workflow?
5. How do you reduce blast radius while the service remains active?
6. What is your approach when a host becomes degraded but is still serving traffic?
7. How do you perform live troubleshooting without making the incident harder to understand later?
8. How do you avoid “fixing” symptoms in a way that destroys the root cause evidence?
9. What is your decision process for draining traffic, isolating a node, or keeping it in rotation?
10. How do you handle a service that fails only under production concurrency or load?
11. How do you investigate a host with hanging shutdowns, stuck processes, or locked resources?
12. What do you do when logs are incomplete, noisy, or misleading during an incident?
13. How do you coordinate with application teams, infrastructure teams, and responders during a war room?
14. What are the signs that a reboot is being used as a substitute for diagnosis?
15. How do you document the incident while still under pressure?
16. What changes would you recommend afterward to prevent recurrence?

13. Tooling trade-offs: shell scripting vs Python/config management/orchestration tools
1. When is Bash the right choice, and when is it the wrong choice?
2. When is PowerShell the right choice, and when is it the wrong choice?
3. When does Python become a better operational tool than either shell?
4. What are the trade-offs between shell scripting and configuration management systems?
5. When would you use orchestration platforms instead of remote imperative scripts?
6. What are the advantages and risks of a one-liner compared with a maintainable script?
7. When is parsing text acceptable, and when should you require structured output?
8. What makes a workflow more suitable for declarative management than imperative scripting?
9. How do you decide whether to build a reusable internal tool versus a narrow incident workaround?
10. What are the operational costs of too many “quick scripts” owned by different teams?
11. How do you think about portability, observability, testability, and safety when choosing tools?
12. What role do shell scripts still play in mature automation ecosystems?
13. How do you avoid overengineering a simple operational task while still keeping it supportable?
14. What are the risks of mixing shell, PowerShell, and Python in the same control flow without clear boundaries?
15. How do you choose tooling when the automation must work on both Linux and Windows estates?

14. Bar-raiser troubleshooting and war-room scenarios
1. A service shows elevated latency, but CPU, memory, and disk all look “fine.” How do you proceed?
2. A host is healthy until a scheduled job runs, after which the service degrades. What do you investigate first?
3. A Bash job succeeds when run by an engineer but fails in cron or systemd. What do you suspect and how do you verify it?
4. A PowerShell remoting command works from one admin workstation but fails from another. What factors do you compare?
5. A process leaks file descriptors over time, but the application team says nothing changed. How do you confirm the leak and localize it?
6. A containerized workload behaves differently from the same binary on bare metal. What do you examine?
7. DNS failures occur only for a subset of hosts after a seemingly unrelated change. What do you check before changing anything else?
8. A service can bind locally but not through the load balancer. How do you separate listener issues from network path issues?
9. A disk fills up, but df and du tell different stories. What are your next steps?
10. A Windows automation task fails only when run as a scheduled task, not interactively. What environment differences matter?
11. A Linux service starts manually but not under systemd. How do you compare the two execution contexts?
12. A reboot temporarily fixes the issue, but the problem returns. How do you avoid accepting the reboot as the answer?
13. A production host has multiple failures at once: high load, delayed IO, and zombie buildup. How do you prioritize investigation?
14. A script that deploys safely in staging causes drift in production after multiple runs. What design flaws are you looking for?
15. A team wants to standardize on a single shell across all platforms. What concerns would you raise?
16. You have 30 minutes in a war room with incomplete telemetry. What questions do you ask first?
17. What would make you stop troubleshooting and escalate to a full rollback or traffic shift?
18. How do you keep the team focused on evidence rather than speculation during a high-pressure incident?

15. Mock panel round questions grouped by architect round, platform/DevOps round, SRE/operations round, Windows/PowerShell round, and hiring manager round
Architect round
1. How would you design a cross-platform operations framework for Linux and Windows that remains secure, auditable, and maintainable?
2. What is your strategy for standardizing shell-based automation across many teams without sacrificing local productivity?
3. How do you decide what should be handled by scripts, configuration management, orchestration, or platform services?
4. How would you reduce environment drift across a large fleet while still allowing emergency changes?
5. How do you design for failure when operating mixed Linux and Windows estates at scale?

Platform/DevOps round
1. How would you build production-safe automation for deployments, remediation, and host maintenance?
2. How do you ensure Bash and PowerShell workflows are idempotent and observable?
3. How do you handle service restarts, configuration reloads, and scheduled jobs without creating incidents?
4. What is your approach to secrets, privilege boundaries, and auditability in operational pipelines?
5. How do you validate that automation behaves the same in CI, staging, and production?

SRE/operations round
1. A host is under pressure and the service is degrading. What are your first five checks?
2. How do you investigate CPU, memory, and IO contention when symptoms are ambiguous?
3. How do you debug intermittent DNS, permission, or filesystem problems in production?
4. What evidence do you preserve before making a risky operational change?
5. How do you decide whether to drain, isolate, restart, or reboot a node during an incident?

Windows/PowerShell round
1. How do you build enterprise-ready PowerShell automation that works across remoting, scheduled execution, and constrained endpoints?
2. How do you handle object serialization, remoting limitations, and module design in PowerShell Core?
3. What are the practical differences between CIM, WMI, jobs, and scheduled tasks from an operations perspective?
4. How do you design scripts that are secure, supportable, and compliant in a Windows estate?
5. How do you troubleshoot UAC, execution policy, certificate, or delegation issues in PowerShell automation?

Hiring manager round
1. Tell me about a time you had to choose between a quick shell fix and a more durable automation design.
2. How do you think about ownership when a production script outlives the team that created it?
3. Describe a high-severity incident where your troubleshooting approach changed the outcome.
4. How do you mentor teams toward safer Bash and PowerShell practices without slowing them down?
5. What principles guide your decisions when reliability, speed, and operational simplicity conflict?