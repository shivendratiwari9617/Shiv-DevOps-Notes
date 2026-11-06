# Ansible Top 100+ Production Troubleshooting Commands
**10/10 senior-level runbook pack for Ansible Platform Engineers, DevOps Engineers, and SREs in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic Ansible cheat sheet.

Each command includes:

- the command itself
- a concise technical explanation
- a **risk level**
- a **use when** line so responders know where it fits in incident flow

## Command Risk Levels

- **Low risk** — read-only or generally safe diagnostic action
- **Medium risk** — changes execution context or performs controlled non-destructive probing
- **High risk** — can broaden blast radius, mutate systems, or expose sensitive behavior
- **Emergency-only** — use only in tightly controlled recovery situations with strong confirmation

## Fast “First 5 Commands” by Incident Type

### Inventory mismatch or wrong host targeting
1. `ansible-inventory -i inventory.ini --graph`
2. `ansible-inventory -i inventory.ini --list`
3. `ansible all -i inventory.ini --list-hosts`
4. `ansible 'web:&prod' -i inventory.ini --list-hosts`
5. `ansible-inventory -i inventory.ini --host app01`

### Unreachable hosts or SSH failure
1. `ansible all -i inventory.ini -m ping -vvv`
2. `ssh -vvv deploy@app01`
3. `ansible all -i inventory.ini -m ping -u deploy`
4. `ansible all -i inventory.ini -m ping --private-key ~/.ssh/prod.pem`
5. `ansible all -i inventory.ini -m wait_for_connection -a 'timeout=30'`

### Become or permission failure
1. `ansible all -i inventory.ini -b -m ping`
2. `ansible all -i inventory.ini -m command -a 'sudo -n true'`
3. `ansible all -i inventory.ini -b -m command -a 'whoami'`
4. `ansible all -i inventory.ini -m command -a 'sudo -l'`
5. `ansible all -i inventory.ini -m stat -a 'path=/etc/myapp.conf'`

### Variable precedence or templating issue
1. `ansible-inventory -i inventory.ini --host app01`
2. `ansible all -i inventory.ini -m debug -a 'var=hostvars[inventory_hostname]'`
3. `ansible all -i inventory.ini -m setup -a 'filter=ansible_default_ipv4'`
4. `ansible all -i inventory.ini -m template -a 'src=templates/app.conf.j2 dest=/tmp/app.conf' --check --diff`
5. `ansible localhost -m debug -a 'var=groups'`

### Idempotence or unexpected change behavior
1. `ansible-playbook -i inventory.ini site.yml --check --diff --limit app01`
2. `ansible-playbook -i inventory.ini site.yml --list-tasks`
3. `ansible all -i inventory.ini -m stat -a 'path=/etc/nginx/nginx.conf checksum_algorithm=sha256'`
4. `ansible-playbook -i inventory.ini site.yml --flush-cache`
5. `ansible-playbook -i inventory.ini site.yml -vvv`

### Slow or risky rollout behavior
1. `time ansible-playbook -i inventory.ini site.yml --check`
2. `ANSIBLE_CALLBACKS_ENABLED=profile_tasks ansible-playbook -i inventory.ini site.yml`
3. `ansible-playbook -i inventory.ini site.yml --forks 1`
4. `ansible-playbook -i inventory.ini site.yml --limit 'web[0:9]'`
5. `ansible-playbook -i inventory.ini site.yml -T 30`

### Vault or secret decryption issue
1. `ansible-config dump | grep vault`
2. `grep -R 'ANSIBLE_VAULT' -n group_vars host_vars`
3. `ansible-playbook -i inventory.ini site.yml --ask-vault-pass`
4. `ansible localhost -m debug -a 'var=db_password' -e '@group_vars/prod/vault.yml' --ask-vault-pass`
5. `ansible-playbook -i inventory.ini site.yml --vault-password-file ~/.vault_pass.txt`

## Recommended Troubleshooting Flow

1. confirm inventory, limits, and effective target hosts first
2. validate SSH and remote user behavior before blaming play logic
3. prefer `--check`, `--diff`, `--list-hosts`, and `--list-tasks` before risky execution
4. limit to one host or one batch when reproducing production issues
5. verify become, facts, variables, and templates explicitly
6. preserve secrets safely while debugging vault and `no_log` paths

---

# CLI, Version, and Environment


## 1. `ansible --version`


```bash

ansible --version

```

**Risk:** Low risk

**Technical explanation:** Prints the Ansible core version, config file path, module search path, and Python runtime so you can rule out controller drift first.

**Use when:** Use when play behavior differs across laptops, bastions, or CI runners.


## 2. `ansible-config dump --only-changed`


```bash

ansible-config dump --only-changed

```

**Risk:** Low risk

**Technical explanation:** Shows only non-default configuration values, which helps identify controller-side behavior overrides quickly.

**Use when:** Use when Ansible seems to behave differently from defaults or documentation.


## 3. `ansible-config view`


```bash

ansible-config view

```

**Risk:** Low risk

**Technical explanation:** Displays the active ansible.cfg content so you can verify which settings are actually in effect.

**Use when:** Use when multiple config files may exist or path precedence is unclear.


## 4. `ansible-config list`


```bash

ansible-config list

```

**Risk:** Low risk

**Technical explanation:** Lists all configurable options and defaults, which is useful when debugging an unfamiliar setting.

**Use when:** Use when you know the behavior but not the exact config key controlling it.


## 5. `ansible-doc -l`


```bash

ansible-doc -l

```

**Risk:** Low risk

**Technical explanation:** Lists available modules so you can verify whether the expected module exists on the control node.

**Use when:** Use when a task fails because a module or plugin may be missing.


## 6. `ansible-doc ansible.builtin.copy`


```bash

ansible-doc ansible.builtin.copy

```

**Risk:** Low risk

**Technical explanation:** Shows module documentation, accepted parameters, and examples so you can validate task syntax quickly.

**Use when:** Use when a task may be passing invalid options to a module.


## 7. `ansible-galaxy collection list`


```bash

ansible-galaxy collection list

```

**Risk:** Low risk

**Technical explanation:** Lists installed collections so you can confirm the required vendor or community collection is present.

**Use when:** Use when a fully qualified collection module or plugin cannot be found.


## 8. `ansible-galaxy role list`


```bash

ansible-galaxy role list

```

**Risk:** Low risk

**Technical explanation:** Lists installed roles so you can verify dependency availability before blaming playbook logic.

**Use when:** Use when imported roles fail or seem absent on the controller.


## 9. `python3 --version`


```bash

python3 --version

```

**Risk:** Low risk

**Technical explanation:** Confirms the controller Python version, which matters because Ansible behavior and collection support can depend on it.

**Use when:** Use when version-sensitive collections or controller runtime issues are suspected.


## 10. `pip show ansible-core`


```bash

pip show ansible-core

```

**Risk:** Low risk

**Technical explanation:** Shows installed ansible-core package metadata so you can confirm the actual package version in the shell environment.

**Use when:** Use when venv, system Python, or pip confusion may exist.


## 11. `which ansible`


```bash

which ansible

```

**Risk:** Low risk

**Technical explanation:** Prints the ansible binary path so you can catch virtualenv, pyenv, or PATH confusion.

**Use when:** Use when the wrong Ansible installation may be executing.


## 12. `env | grep ^ANSIBLE_`


```bash

env | grep ^ANSIBLE_

```

**Risk:** Low risk

**Technical explanation:** Shows Ansible-related environment variables that may silently override config and alter behavior.

**Use when:** Use when CI or wrapper scripts might be injecting hidden overrides.


# Inventory and Host Targeting


## 13. `ansible-inventory -i inventory.ini --list`


```bash

ansible-inventory -i inventory.ini --list

```

**Risk:** Low risk

**Technical explanation:** Renders the parsed inventory as structured data so you can verify group membership and host variables.

**Use when:** Use when hosts are unexpectedly included, excluded, or missing variables.


## 14. `ansible-inventory -i inventory.ini --graph`


```bash

ansible-inventory -i inventory.ini --graph

```

**Risk:** Low risk

**Technical explanation:** Draws the inventory hierarchy so you can inspect parent-child group relationships quickly.

**Use when:** Use when nested groups or inheritance may be causing target-scope errors.


## 15. `ansible-inventory -i inventory.ini --host app01`


```bash

ansible-inventory -i inventory.ini --host app01

```

**Risk:** Low risk

**Technical explanation:** Shows the effective variables for one host after inventory processing.

**Use when:** Use when one host behaves differently and you need its final variable set.


## 16. `ansible all -i inventory.ini --list-hosts`


```bash

ansible all -i inventory.ini --list-hosts

```

**Risk:** Low risk

**Technical explanation:** Lists which hosts match the current pattern without executing anything, preventing accidental broad targeting.

**Use when:** Use before any risky ad hoc command or playbook run.


## 17. `ansible 'web:&prod' -i inventory.ini --list-hosts`


```bash

ansible 'web:&prod' -i inventory.ini --list-hosts

```

**Risk:** Low risk

**Technical explanation:** Validates complex host-pattern resolution before running production commands.

**Use when:** Use when intersections, exclusions, or compound host patterns are in play.


## 18. `ansible all -i inventory.ini -m ping`


```bash

ansible all -i inventory.ini -m ping

```

**Risk:** Low risk

**Technical explanation:** Performs a minimal connectivity and Python-availability test using the ping module.

**Use when:** Use when you must separate basic reachability from playbook logic failure.


## 19. `ansible app01 -i inventory.ini -m setup -a 'filter=ansible_distribution*'`


```bash

ansible app01 -i inventory.ini -m setup -a 'filter=ansible_distribution*'

```

**Risk:** Low risk

**Technical explanation:** Collects targeted facts from one host so you can confirm OS family assumptions safely.

**Use when:** Use when task branching depends on distro-specific logic.


## 20. `ansible all -i inventory.ini -m debug -a 'msg={{ inventory_hostname }}'`


```bash

ansible all -i inventory.ini -m debug -a 'msg={{ inventory_hostname }}'

```

**Risk:** Low risk

**Technical explanation:** Confirms that host selection and templating are resolving as expected.

**Use when:** Use when you want a harmless proof of execution scope.


## 21. `ansible localhost -i 'localhost,' -c local -m debug -a 'var=hostvars'`


```bash

ansible localhost -i 'localhost,' -c local -m debug -a 'var=hostvars'

```

**Risk:** Low risk

**Technical explanation:** Lets you inspect hostvars structure locally for inventory-debugging work.

**Use when:** Use when troubleshooting variable precedence without touching remotes.


## 22. `ansible-inventory -i inventory.ini --yaml`


```bash

ansible-inventory -i inventory.ini --yaml

```

**Risk:** Low risk

**Technical explanation:** Outputs inventory in YAML form, which can be easier to audit than JSON during incidents.

**Use when:** Use when humans need to inspect generated inventory quickly.


## 23. `ansible-inventory -i inventory.ini --toml`


```bash

ansible-inventory -i inventory.ini --toml

```

**Risk:** Low risk

**Technical explanation:** Outputs inventory in TOML form, which is useful when comparing generated inventory from tooling.

**Use when:** Use when another tool or pipeline emits TOML-like inventory structures.


## 24. `ansible-playbook -i inventory.ini site.yml --limit app01 --list-hosts`


```bash

ansible-playbook -i inventory.ini site.yml --limit app01 --list-hosts

```

**Risk:** Low risk

**Technical explanation:** Shows which hosts a playbook would target after limits are applied.

**Use when:** Use when validating that a canary or single-host reproduction is truly scoped.


# Connectivity, SSH, and Authentication


## 25. `ansible all -i inventory.ini -m ping -u deploy`


```bash

ansible all -i inventory.ini -m ping -u deploy

```

**Risk:** Low risk

**Technical explanation:** Tests remote connectivity as a specific user so you can isolate account-related failures.

**Use when:** Use when one SSH identity works but Ansible may be using another.


## 26. `ansible all -i inventory.ini -m ping --private-key ~/.ssh/prod.pem`


```bash

ansible all -i inventory.ini -m ping --private-key ~/.ssh/prod.pem

```

**Risk:** Low risk

**Technical explanation:** Validates that the expected SSH key works for Ansible connections.

**Use when:** Use when key path or agent behavior may be the blocker.


## 27. `ansible all -i inventory.ini -m ping -e 'ansible_ssh_common_args="-o StrictHostKeyChecking=no"'`


```bash

ansible all -i inventory.ini -m ping -e 'ansible_ssh_common_args="-o StrictHostKeyChecking=no"'

```

**Risk:** High risk

**Technical explanation:** Tests whether host key verification is the blocker, but weakens SSH safety for the run.

**Use when:** Use only in controlled troubleshooting when host-key mismatch is the likely issue.


## 28. `ansible all -i inventory.ini -m command -a 'whoami'`


```bash

ansible all -i inventory.ini -m command -a 'whoami'

```

**Risk:** Low risk

**Technical explanation:** Verifies remote command execution and effective remote user on target hosts.

**Use when:** Use when you need to prove which login identity Ansible is actually using.


## 29. `ansible all -i inventory.ini -m command -a 'hostname -f'`


```bash

ansible all -i inventory.ini -m command -a 'hostname -f'

```

**Risk:** Low risk

**Technical explanation:** Checks remote command execution and confirms host identity from the target side.

**Use when:** Use when DNS, inventory naming, or host targeting may be mismatched.


## 30. `ansible all -i inventory.ini -m shell -a 'id'`


```bash

ansible all -i inventory.ini -m shell -a 'id'

```

**Risk:** Medium risk

**Technical explanation:** Shows UID, GID, and groups when permission or sudo assumptions are failing, but invokes a shell remotely.

**Use when:** Use when user and group context must be inspected precisely.


## 31. `ansible all -i inventory.ini -m ping -vvv`


```bash

ansible all -i inventory.ini -m ping -vvv

```

**Risk:** Low risk

**Technical explanation:** Enables verbose SSH and connection debugging so lower-level transport problems are visible.

**Use when:** Use when basic ping fails and you need transport-level clues.


## 32. `ssh -vvv deploy@app01`


```bash

ssh -vvv deploy@app01

```

**Risk:** Low risk

**Technical explanation:** Tests raw SSH outside Ansible to separate transport issues from Ansible-specific issues.

**Use when:** Use when you need to prove whether the problem is SSH itself or controller logic.


## 33. `ansible all -i inventory.ini -m wait_for_connection -a 'timeout=30'`


```bash

ansible all -i inventory.ini -m wait_for_connection -a 'timeout=30'

```

**Risk:** Low risk

**Technical explanation:** Waits for hosts to become reachable, which is useful after reboot or scale-up events.

**Use when:** Use when infrastructure is still converging and transient unreachability is expected.


## 34. `ansible all -i inventory.ini -m command -a 'sudo -n true'`


```bash

ansible all -i inventory.ini -m command -a 'sudo -n true'

```

**Risk:** Low risk

**Technical explanation:** Checks whether passwordless sudo works on targets before privilege-escalated playbook runs.

**Use when:** Use when become is failing and you want a simple sudo-specific probe.


## 35. `ansible all -i inventory.ini -b -m ping`


```bash

ansible all -i inventory.ini -b -m ping

```

**Risk:** Low risk

**Technical explanation:** Tests connectivity with privilege escalation enabled so you can validate become behavior broadly.

**Use when:** Use when connection succeeds but privilege escalation may be broken.


## 36. `ansible all -i inventory.ini -m setup -a 'filter=ansible_user*'`


```bash

ansible all -i inventory.ini -m setup -a 'filter=ansible_user*'

```

**Risk:** Low risk

**Technical explanation:** Collects user-related facts to confirm what Ansible thinks the remote user context is.

**Use when:** Use when facts and observed SSH identity seem inconsistent.


# Ad Hoc Command Troubleshooting


## 37. `ansible all -i inventory.ini -m command -a 'uptime'`


```bash

ansible all -i inventory.ini -m command -a 'uptime'

```

**Risk:** Low risk

**Technical explanation:** Runs a safe ad hoc command to validate controller-to-host execution across a fleet.

**Use when:** Use when you need a low-risk proof that ad hoc execution is functional.


## 38. `ansible all -i inventory.ini -m shell -a 'systemctl is-active nginx'`


```bash

ansible all -i inventory.ini -m shell -a 'systemctl is-active nginx'

```

**Risk:** Medium risk

**Technical explanation:** Checks service state directly when application availability is in question, but uses a shell remotely.

**Use when:** Use when service status must be confirmed quickly and idempotence is not the concern.


## 39. `ansible all -i inventory.ini -m command -a 'df -h'`


```bash

ansible all -i inventory.ini -m command -a 'df -h'

```

**Risk:** Low risk

**Technical explanation:** Collects disk-usage data quickly when deployments or apps may be failing due to storage pressure.

**Use when:** Use when capacity or full-filesystem symptoms appear.


## 40. `ansible all -i inventory.ini -m command -a 'free -m'`


```bash

ansible all -i inventory.ini -m command -a 'free -m'

```

**Risk:** Low risk

**Technical explanation:** Checks memory status across hosts when OOM or capacity issues may explain instability.

**Use when:** Use when hosts are slow, evicting, or killing processes unexpectedly.


## 41. `ansible all -i inventory.ini -m command -a 'journalctl -u myservice -n 50 --no-pager'`


```bash

ansible all -i inventory.ini -m command -a 'journalctl -u myservice -n 50 --no-pager'

```

**Risk:** Low risk

**Technical explanation:** Pulls recent systemd service logs without opening interactive sessions.

**Use when:** Use when you need fast log evidence for a failing managed service.


## 42. `ansible all -i inventory.ini -m shell -a 'ps -ef | grep myservice | grep -v grep'`


```bash

ansible all -i inventory.ini -m shell -a 'ps -ef | grep myservice | grep -v grep'

```

**Risk:** Medium risk

**Technical explanation:** Checks process presence quickly when service managers or wrappers hide the real process state.

**Use when:** Use when direct process visibility matters more than shell-free purity.


## 43. `ansible all -i inventory.ini -m command -a 'curl -I http://localhost:8080/health'`


```bash

ansible all -i inventory.ini -m command -a 'curl -I http://localhost:8080/health'

```

**Risk:** Low risk

**Technical explanation:** Validates a local health endpoint directly from each host.

**Use when:** Use when service process status looks healthy but readiness may not be.


## 44. `ansible all -i inventory.ini -m command -a 'ss -lntp'`


```bash

ansible all -i inventory.ini -m command -a 'ss -lntp'

```

**Risk:** Low risk

**Technical explanation:** Shows listening TCP ports so you can confirm whether the expected service is actually bound.

**Use when:** Use when the app is up but clients cannot connect.


## 45. `ansible all -i inventory.ini -m command -a 'timedatectl status'`


```bash

ansible all -i inventory.ini -m command -a 'timedatectl status'

```

**Risk:** Low risk

**Technical explanation:** Checks time synchronization state, which can matter for auth, cert, and cluster issues.

**Use when:** Use when token, TLS, or distributed-system timing anomalies are suspected.


## 46. `ansible all -i inventory.ini -m shell -a 'getenforce || true'`


```bash

ansible all -i inventory.ini -m shell -a 'getenforce || true'

```

**Risk:** Medium risk

**Technical explanation:** Inspects SELinux mode quickly when file access or service startup behaves unexpectedly.

**Use when:** Use when permission symptoms do not match Unix mode bits alone.


## 47. `ansible all -i inventory.ini -m command -a 'uname -a'`


```bash

ansible all -i inventory.ini -m command -a 'uname -a'

```

**Risk:** Low risk

**Technical explanation:** Collects kernel and OS identity data across hosts for fast platform validation.

**Use when:** Use when kernel, arch, or OS mismatch may explain failures.


## 48. `ansible all -i inventory.ini -m command -a 'ip addr show'`


```bash

ansible all -i inventory.ini -m command -a 'ip addr show'

```

**Risk:** Low risk

**Technical explanation:** Checks network interface state when routing, VIP, or binding issues are suspected.

**Use when:** Use when the host may have the wrong IP, interface, or address state.


# Playbook Parsing, Preview, and Runtime Control


## 49. `ansible-playbook -i inventory.ini site.yml --syntax-check`


```bash

ansible-playbook -i inventory.ini site.yml --syntax-check

```

**Risk:** Low risk

**Technical explanation:** Validates playbook YAML structure and basic parsing before execution.

**Use when:** Use first when a playbook may fail before real task execution begins.


## 50. `ansible-playbook -i inventory.ini site.yml --list-tasks`


```bash

ansible-playbook -i inventory.ini site.yml --list-tasks

```

**Risk:** Low risk

**Technical explanation:** Shows the tasks that would run so you can inspect play structure without executing anything.

**Use when:** Use when verifying play flow, includes, and task visibility safely.


## 51. `ansible-playbook -i inventory.ini site.yml --list-tags`


```bash

ansible-playbook -i inventory.ini site.yml --list-tags

```

**Risk:** Low risk

**Technical explanation:** Lists defined tags so you can target or skip the correct parts of a playbook safely.

**Use when:** Use when only one workflow segment should be tested or excluded.


## 52. `ansible-playbook -i inventory.ini site.yml --check`


```bash

ansible-playbook -i inventory.ini site.yml --check

```

**Risk:** Low risk

**Technical explanation:** Runs in dry-run mode to estimate changes without modifying remote systems.

**Use when:** Use when you need a first-pass safety preview of intended changes.


## 53. `ansible-playbook -i inventory.ini site.yml --check --diff`


```bash

ansible-playbook -i inventory.ini site.yml --check --diff

```

**Risk:** Low risk

**Technical explanation:** Combines dry-run with file diff output so you can preview templated or managed file changes.

**Use when:** Use when managed content or templates may render incorrectly.


## 54. `ansible-playbook -i inventory.ini site.yml --start-at-task='Restart app'`


```bash

ansible-playbook -i inventory.ini site.yml --start-at-task='Restart app'

```

**Risk:** Medium risk

**Technical explanation:** Resumes execution from a named task, which is useful after correcting a mid-play failure.

**Use when:** Use when rerunning the whole play is unnecessary or riskier than resuming.


## 55. `ansible-playbook -i inventory.ini site.yml --step`


```bash

ansible-playbook -i inventory.ini site.yml --step

```

**Risk:** Medium risk

**Technical explanation:** Prompts before each task so you can advance through a risky run interactively.

**Use when:** Use when you need human-controlled progression through a sensitive execution path.


## 56. `ansible-playbook -i inventory.ini site.yml --limit app01`


```bash

ansible-playbook -i inventory.ini site.yml --limit app01

```

**Risk:** Low risk

**Technical explanation:** Restricts execution to one host so you can reproduce or validate safely before fleet rollout.

**Use when:** Use when testing a fix on a canary host.


## 57. `ansible-playbook -i inventory.ini site.yml --tags deploy`


```bash

ansible-playbook -i inventory.ini site.yml --tags deploy

```

**Risk:** Low risk

**Technical explanation:** Runs only tagged tasks so you can isolate one workflow segment during debugging.

**Use when:** Use when only a subset of tasks is relevant to the incident.


## 58. `ansible-playbook -i inventory.ini site.yml --skip-tags restart`


```bash

ansible-playbook -i inventory.ini site.yml --skip-tags restart

```

**Risk:** Low risk

**Technical explanation:** Skips risky or known-problematic sections while testing the rest of the play.

**Use when:** Use when validation is needed without triggering disruptive handlers or restarts.


## 59. `ansible-playbook -i inventory.ini site.yml -e 'ansible_check_mode=true'`


```bash

ansible-playbook -i inventory.ini site.yml -e 'ansible_check_mode=true'

```

**Risk:** Low risk

**Technical explanation:** Injects a variable override useful in playbooks that branch on check-mode-like logic.

**Use when:** Use when custom play logic depends on an explicit variable rather than native check mode alone.


## 60. `ansible-playbook -i inventory.ini site.yml -vvv`


```bash

ansible-playbook -i inventory.ini site.yml -vvv

```

**Risk:** Low risk

**Technical explanation:** Enables verbose execution logs so task-level details, SSH transport, and module calls are easier to inspect.

**Use when:** Use when normal output hides the real cause of failure.


# Facts, Variables, and Templating


## 61. `ansible all -i inventory.ini -m setup`


```bash

ansible all -i inventory.ini -m setup

```

**Risk:** Low risk

**Technical explanation:** Collects the full fact set from targets so you can validate what the playbook actually knows about hosts.

**Use when:** Use when branching logic may be wrong because facts are stale or unexpected.


## 62. `ansible all -i inventory.ini -m setup -a 'filter=ansible_default_ipv4'`


```bash

ansible all -i inventory.ini -m setup -a 'filter=ansible_default_ipv4'

```

**Risk:** Low risk

**Technical explanation:** Collects only default IPv4 facts, which is useful when networking assumptions may be wrong.

**Use when:** Use when IP-based templating or routing decisions look suspicious.


## 63. `ansible all -i inventory.ini -m debug -a 'var=inventory_hostname'`


```bash

ansible all -i inventory.ini -m debug -a 'var=inventory_hostname'

```

**Risk:** Low risk

**Technical explanation:** Prints a simple variable to validate templating and execution context.

**Use when:** Use when confirming core Jinja substitution or host loop behavior.


## 64. `ansible all -i inventory.ini -m debug -a 'var=hostvars[inventory_hostname]'`


```bash

ansible all -i inventory.ini -m debug -a 'var=hostvars[inventory_hostname]'

```

**Risk:** Low risk

**Technical explanation:** Dumps effective host variables for deep inventory and precedence troubleshooting.

**Use when:** Use when one host sees different vars than expected.


## 65. `ansible all -i inventory.ini -m debug -a 'msg={{ ansible_facts.os_family }}'`


```bash

ansible all -i inventory.ini -m debug -a 'msg={{ ansible_facts.os_family }}'

```

**Risk:** Low risk

**Technical explanation:** Confirms fact-derived templating decisions that may branch task behavior.

**Use when:** Use when tasks are taking the wrong OS-specific path.


## 66. `ansible-playbook -i inventory.ini site.yml -e env=prod`


```bash

ansible-playbook -i inventory.ini site.yml -e env=prod

```

**Risk:** Medium risk

**Technical explanation:** Overrides variables at runtime so you can test whether bad input values are driving failures.

**Use when:** Use when CI, wrappers, or environment-specific vars may be wrong.


## 67. `ansible all -i inventory.ini -m template -a 'src=templates/app.conf.j2 dest=/tmp/app.conf' --check --diff`


```bash

ansible all -i inventory.ini -m template -a 'src=templates/app.conf.j2 dest=/tmp/app.conf' --check --diff

```

**Risk:** Low risk

**Technical explanation:** Previews rendered template output to validate Jinja logic and resulting file content.

**Use when:** Use when managed config content may be malformed before touching live paths.


## 68. `ansible localhost -m debug -a 'msg={{ lookup("env", "HOME") }}'`


```bash

ansible localhost -m debug -a 'msg={{ lookup("env", "HOME") }}'

```

**Risk:** Low risk

**Technical explanation:** Tests lookup behavior locally when environment-driven templating is in doubt.

**Use when:** Use when controller environment values influence rendered output.


## 69. `ansible localhost -m debug -a 'msg={{ "10.0.0.0/24" | ansible.utils.ipaddr("network") }}'`


```bash

ansible localhost -m debug -a 'msg={{ "10.0.0.0/24" | ansible.utils.ipaddr("network") }}'

```

**Risk:** Low risk

**Technical explanation:** Validates a filter plugin result so you can isolate data-transformation issues.

**Use when:** Use when Jinja filter behavior may be producing wrong derived values.


## 70. `ansible-playbook -i inventory.ini site.yml -e '@vars/prod.yml'`


```bash

ansible-playbook -i inventory.ini site.yml -e '@vars/prod.yml'

```

**Risk:** Medium risk

**Technical explanation:** Loads variables from a file so you can reproduce environment-specific behavior exactly.

**Use when:** Use when a specific vars file used in automation must be mirrored locally.


## 71. `ansible-inventory -i inventory.ini --host app01 | jq`


```bash

ansible-inventory -i inventory.ini --host app01 | jq

```

**Risk:** Low risk

**Technical explanation:** Pretty-prints effective host inventory data, which is useful for variable precedence investigation.

**Use when:** Use when raw inventory output is too dense to inspect quickly.


## 72. `ansible localhost -m debug -a 'var=groups'`


```bash

ansible localhost -m debug -a 'var=groups'

```

**Risk:** Low risk

**Technical explanation:** Inspects group structures locally so you can verify inventory-derived group logic.

**Use when:** Use when group membership drives templating or conditional paths.


# Privilege Escalation and Permissions


## 73. `ansible all -i inventory.ini -b -m command -a 'whoami'`


```bash

ansible all -i inventory.ini -b -m command -a 'whoami'

```

**Risk:** Low risk

**Technical explanation:** Confirms the effective user after become so you can validate privilege escalation behavior.

**Use when:** Use when tasks succeed remotely but may not be running as the intended user.


## 74. `ansible all -i inventory.ini -b -m command -a 'id'`


```bash

ansible all -i inventory.ini -b -m command -a 'id'

```

**Risk:** Low risk

**Technical explanation:** Shows identity details under become, which helps explain permission-denied failures.

**Use when:** Use when sudo works inconsistently across hosts.


## 75. `ansible all -i inventory.ini -b --become-user root -m ping`


```bash

ansible all -i inventory.ini -b --become-user root -m ping

```

**Risk:** Low risk

**Technical explanation:** Tests explicit escalation to root when default become behavior is unclear.

**Use when:** Use when become defaults may not actually target root.


## 76. `ansible all -i inventory.ini -K -b -m ping`


```bash

ansible all -i inventory.ini -K -b -m ping

```

**Risk:** Medium risk

**Technical explanation:** Prompts for the become password interactively so you can test sudo paths that require credentials.

**Use when:** Use when passwordless sudo is not configured but escalation should still work.


## 77. `ansible all -i inventory.ini -m file -a 'path=/etc/myapp.conf state=file mode=0644' --check`


```bash

ansible all -i inventory.ini -m file -a 'path=/etc/myapp.conf state=file mode=0644' --check

```

**Risk:** Low risk

**Technical explanation:** Validates whether Ansible can inspect and manage a privileged file path.

**Use when:** Use when file tasks fail and you want a safe permissions probe.


## 78. `ansible all -i inventory.ini -m stat -a 'path=/etc/myapp.conf'`


```bash

ansible all -i inventory.ini -m stat -a 'path=/etc/myapp.conf'

```

**Risk:** Low risk

**Technical explanation:** Checks file existence, ownership, and mode when tasks fail against managed files.

**Use when:** Use when target file state may differ across hosts.


## 79. `ansible all -i inventory.ini -m command -a 'namei -l /etc/myapp.conf'`


```bash

ansible all -i inventory.ini -m command -a 'namei -l /etc/myapp.conf'

```

**Risk:** Low risk

**Technical explanation:** Shows ownership and permissions across the full path, helping debug parent-directory access issues.

**Use when:** Use when the file itself looks correct but path traversal may be blocked.


## 80. `ansible all -i inventory.ini -m command -a 'ls -ld /var/lib/myapp /var/lib/myapp/data'`


```bash

ansible all -i inventory.ini -m command -a 'ls -ld /var/lib/myapp /var/lib/myapp/data'

```

**Risk:** Low risk

**Technical explanation:** Inspects directory ownership and mode on critical app paths.

**Use when:** Use when nested directory permissions may prevent writes or startup.


## 81. `ansible all -i inventory.ini -b -m shell -a 'touch /root/ansible_perm_test && rm -f /root/ansible_perm_test'`


```bash

ansible all -i inventory.ini -b -m shell -a 'touch /root/ansible_perm_test && rm -f /root/ansible_perm_test'

```

**Risk:** High risk

**Technical explanation:** Validates write access as the escalated user in a controlled way, but does mutate remote state briefly.

**Use when:** Use only when you need proof of privileged write capability and safer checks are insufficient.


## 82. `ansible all -i inventory.ini -m shell -a 'getenforce || true'`


```bash

ansible all -i inventory.ini -m shell -a 'getenforce || true'

```

**Risk:** Medium risk

**Technical explanation:** Checks SELinux mode because permission problems can come from policy, not just Unix mode bits.

**Use when:** Use when classic ownership checks do not explain the denial.


## 83. `ansible all -i inventory.ini -m command -a 'sudo -l'`


```bash

ansible all -i inventory.ini -m command -a 'sudo -l'

```

**Risk:** Low risk

**Technical explanation:** Lists sudo privileges so you can confirm what the remote user is actually allowed to do.

**Use when:** Use when escalation rights vary across hosts or environments.


## 84. `ansible all -i inventory.ini -m command -a 'umask'`


```bash

ansible all -i inventory.ini -m command -a 'umask'

```

**Risk:** Low risk

**Technical explanation:** Shows the effective umask when created files have unexpected permissions.

**Use when:** Use when resulting file modes are wrong despite correct module parameters.


# Modules, Idempotence, Handlers, and Task Behavior


## 85. `ansible-playbook -i inventory.ini site.yml --check --diff --limit app01`


```bash

ansible-playbook -i inventory.ini site.yml --check --diff --limit app01

```

**Risk:** Low risk

**Technical explanation:** Combines dry-run, diff, and single-host targeting to inspect idempotence safely.

**Use when:** Use when one host should prove whether the play converges cleanly.


## 86. `ansible all -i inventory.ini -m copy -a 'src=/etc/hosts dest=/tmp/hosts.test backup=yes' --check --diff`


```bash

ansible all -i inventory.ini -m copy -a 'src=/etc/hosts dest=/tmp/hosts.test backup=yes' --check --diff

```

**Risk:** Low risk

**Technical explanation:** Tests file-copy behavior and diff output without changing the target.

**Use when:** Use when copy tasks may produce unexpected content or ownership.


## 87. `ansible all -i inventory.ini -m lineinfile -a 'path=/tmp/app.conf line="feature=true" create=yes' --check --diff`


```bash

ansible all -i inventory.ini -m lineinfile -a 'path=/tmp/app.conf line="feature=true" create=yes' --check --diff

```

**Risk:** Low risk

**Technical explanation:** Previews a lineinfile mutation so you can verify match and insertion logic.

**Use when:** Use when line-level config edits may duplicate or land in the wrong place.


## 88. `ansible all -i inventory.ini -m replace -a 'path=/tmp/app.conf regexp="foo" replace="bar"' --check --diff`


```bash

ansible all -i inventory.ini -m replace -a 'path=/tmp/app.conf regexp="foo" replace="bar"' --check --diff

```

**Risk:** Low risk

**Technical explanation:** Validates regex-based file replacement behavior before editing live configuration.

**Use when:** Use when a replace pattern may be too broad or too narrow.


## 89. `ansible all -i inventory.ini -m package -a 'name=nginx state=present' --check`


```bash

ansible all -i inventory.ini -m package -a 'name=nginx state=present' --check

```

**Risk:** Low risk

**Technical explanation:** Confirms whether package state would change without touching the system.

**Use when:** Use when package drift may explain failures but you want safe inspection first.


## 90. `ansible all -i inventory.ini -m service -a 'name=nginx state=started enabled=yes' --check`


```bash

ansible all -i inventory.ini -m service -a 'name=nginx state=started enabled=yes' --check

```

**Risk:** Low risk

**Technical explanation:** Tests intended service-state enforcement and can expose unsupported check-mode behavior.

**Use when:** Use when service tasks behave oddly and you need a non-invasive first look.


## 91. `ansible all -i inventory.ini -m stat -a 'path=/etc/nginx/nginx.conf checksum_algorithm=sha256'`


```bash

ansible all -i inventory.ini -m stat -a 'path=/etc/nginx/nginx.conf checksum_algorithm=sha256'

```

**Risk:** Low risk

**Technical explanation:** Computes file metadata and checksum so you can detect drift precisely.

**Use when:** Use when managed files may differ subtly across hosts.


## 92. `ansible all -i inventory.ini -m slurp -a 'src=/etc/nginx/nginx.conf'`


```bash

ansible all -i inventory.ini -m slurp -a 'src=/etc/nginx/nginx.conf'

```

**Risk:** Medium risk

**Technical explanation:** Retrieves remote file content encoded in base64 for deeper debugging when direct shell access is restricted.

**Use when:** Use when you must inspect actual remote content centrally.


## 93. `ansible all -i inventory.ini -m fetch -a 'src=/var/log/myapp/error.log dest=./collected/ flat=yes'`


```bash

ansible all -i inventory.ini -m fetch -a 'src=/var/log/myapp/error.log dest=./collected/ flat=yes'

```

**Risk:** Medium risk

**Technical explanation:** Pulls remote files back to the controller so logs or artifacts can be inspected centrally.

**Use when:** Use when log collection is needed from one or many hosts.


## 94. `ansible-playbook -i inventory.ini site.yml --force-handlers`


```bash

ansible-playbook -i inventory.ini site.yml --force-handlers

```

**Risk:** High risk

**Technical explanation:** Forces notified handlers to run even after task failure, which can aid recovery but may trigger changes after partial failure.

**Use when:** Use only when handler execution is required to restore consistency and the risk is understood.


## 95. `ansible-playbook -i inventory.ini site.yml --flush-cache`


```bash

ansible-playbook -i inventory.ini site.yml --flush-cache

```

**Risk:** Low risk

**Technical explanation:** Clears cached facts before execution so stale cached data does not mislead task logic.

**Use when:** Use when cached facts may explain inconsistent branching.


## 96. `ansible-playbook -i inventory.ini site.yml --diff`


```bash

ansible-playbook -i inventory.ini site.yml --diff

```

**Risk:** Medium risk

**Technical explanation:** Shows file-level diffs for supported modules during a real run, improving visibility but potentially exposing sensitive content.

**Use when:** Use when change transparency matters and secrets are handled safely.


# Performance, Parallelism, Strategy, and Scale


## 97. `ansible-playbook -i inventory.ini site.yml --forks 50`


```bash

ansible-playbook -i inventory.ini site.yml --forks 50

```

**Risk:** High risk

**Technical explanation:** Increases parallelism so you can test whether controller throughput or host batching affects rollout behavior, but raises blast radius.

**Use when:** Use only when broader concurrency is intentional and safe for the target fleet.


## 98. `ansible-playbook -i inventory.ini site.yml --forks 1`


```bash

ansible-playbook -i inventory.ini site.yml --forks 1

```

**Risk:** Low risk

**Technical explanation:** Serializes execution to expose race conditions, shared-resource contention, or ordering assumptions.

**Use when:** Use when concurrency may be hiding the root cause.


## 99. `ansible-playbook -i inventory.ini site.yml --limit 'web[0:9]'`


```bash

ansible-playbook -i inventory.ini site.yml --limit 'web[0:9]'

```

**Risk:** Low risk

**Technical explanation:** Targets a small slice of hosts so you can canary a change before wider rollout.

**Use when:** Use when a subset should validate the fix safely.


## 100. `ansible-playbook -i inventory.ini site.yml -e 'serial=1'`


```bash

ansible-playbook -i inventory.ini site.yml -e 'serial=1'

```

**Risk:** Low risk

**Technical explanation:** Lets you inject a rollout-control variable in playbooks designed to parameterize serial behavior.

**Use when:** Use when staged rollout speed should be reduced without editing code.


## 101. `ansible-playbook -i inventory.ini site.yml -T 30`


```bash

ansible-playbook -i inventory.ini site.yml -T 30

```

**Risk:** Low risk

**Technical explanation:** Sets the connection timeout, which helps when slow hosts are causing controller stalls.

**Use when:** Use when some hosts are sluggish but not fully unreachable.


## 102. `ansible all -i inventory.ini -f 100 -m ping`


```bash

ansible all -i inventory.ini -f 100 -m ping

```

**Risk:** Medium risk

**Technical explanation:** Runs high-parallelism connectivity checks to estimate fleet reachability quickly.

**Use when:** Use when scale-wide reachability must be assessed fast and the network can tolerate it.


## 103. `ANSIBLE_CALLBACKS_ENABLED=profile_tasks ansible-playbook -i inventory.ini site.yml`


```bash

ANSIBLE_CALLBACKS_ENABLED=profile_tasks ansible-playbook -i inventory.ini site.yml

```

**Risk:** Low risk

**Technical explanation:** Enables task timing output so you can identify the slowest parts of a play.

**Use when:** Use when controller or task runtime slowness must be quantified.


## 104. `ANSIBLE_STDOUT_CALLBACK=yaml ansible-playbook -i inventory.ini site.yml`


```bash

ANSIBLE_STDOUT_CALLBACK=yaml ansible-playbook -i inventory.ini site.yml

```

**Risk:** Low risk

**Technical explanation:** Changes stdout formatting to YAML, which can make complex task output easier to inspect.

**Use when:** Use when default output hides important nested task details.


## 105. `ansible-playbook -i inventory.ini site.yml --limit batch1`


```bash

ansible-playbook -i inventory.ini site.yml --limit batch1

```

**Risk:** Low risk

**Technical explanation:** Restricts a run to a predefined batch group for controlled staged execution.

**Use when:** Use when the fleet is already partitioned into rollout-safe groups.


## 106. `ansible all -i inventory.ini -m setup -a 'gather_subset=!all,!min,network'`


```bash

ansible all -i inventory.ini -m setup -a 'gather_subset=!all,!min,network'

```

**Risk:** Low risk

**Technical explanation:** Collects only a subset of facts, which can reduce slowness from broad fact gathering.

**Use when:** Use when fact collection is the bottleneck and only network data matters.


## 107. `ansible-playbook -i inventory.ini site.yml --ssh-common-args='-o ControlMaster=no'`


```bash

ansible-playbook -i inventory.ini site.yml --ssh-common-args='-o ControlMaster=no'

```

**Risk:** Medium risk

**Technical explanation:** Disables SSH multiplexing for a run to isolate connection-mux related issues.

**Use when:** Use when intermittent SSH multiplexing bugs may explain transport failures.


## 108. `time ansible-playbook -i inventory.ini site.yml --check`


```bash

time ansible-playbook -i inventory.ini site.yml --check

```

**Risk:** Low risk

**Technical explanation:** Measures dry-run duration so you can quantify controller-side slowness or scale regressions.

**Use when:** Use when execution time itself is part of the incident.


# Vault, Secrets, and Sensitive Data


## 109. `ansible-vault view group_vars/prod/vault.yml`


```bash

ansible-vault view group_vars/prod/vault.yml

```

**Risk:** Medium risk

**Technical explanation:** Opens an encrypted vault file so you can verify that the expected secret values exist.

**Use when:** Use when a secret may be missing or wrong and read access is justified.


## 110. `ansible-vault edit group_vars/prod/vault.yml`


```bash

ansible-vault edit group_vars/prod/vault.yml

```

**Risk:** High risk

**Technical explanation:** Edits a vault file securely when a secret correction is part of incident recovery.

**Use when:** Use only when a secret value truly must be changed during recovery.


## 111. `ansible-vault decrypt group_vars/prod/vault.yml`


```bash

ansible-vault decrypt group_vars/prod/vault.yml

```

**Risk:** Emergency-only

**Technical explanation:** Decrypts a vault file to plaintext, which greatly increases exposure risk during troubleshooting.

**Use when:** Use only in tightly controlled recovery situations with explicit secret-handling discipline.


## 112. `ansible-vault encrypt group_vars/prod/vault.yml`


```bash

ansible-vault encrypt group_vars/prod/vault.yml

```

**Risk:** High risk

**Technical explanation:** Re-encrypts a plaintext secrets file so secret handling returns to a safe state after troubleshooting.

**Use when:** Use immediately after any controlled plaintext inspection workflow.


## 113. `ansible-vault rekey group_vars/prod/vault.yml`


```bash

ansible-vault rekey group_vars/prod/vault.yml

```

**Risk:** High risk

**Technical explanation:** Changes the vault password used for a file, which is useful during secret-rotation or credential-compromise events.

**Use when:** Use when vault credentials are rotated or suspected compromised.


## 114. `ansible-playbook -i inventory.ini site.yml --ask-vault-pass`


```bash

ansible-playbook -i inventory.ini site.yml --ask-vault-pass

```

**Risk:** Low risk

**Technical explanation:** Prompts interactively for the vault password so secret decryption can be tested manually.

**Use when:** Use when reproducing secret access manually outside CI.


## 115. `ansible-playbook -i inventory.ini site.yml --vault-password-file ~/.vault_pass.txt`


```bash

ansible-playbook -i inventory.ini site.yml --vault-password-file ~/.vault_pass.txt

```

**Risk:** Medium risk

**Technical explanation:** Uses a password file so you can reproduce non-interactive secret handling as CI would.

**Use when:** Use when local troubleshooting must mirror pipeline secret decryption behavior.


## 116. `ansible-vault encrypt_string 'supersecret' --name 'db_password'`


```bash

ansible-vault encrypt_string 'supersecret' --name 'db_password'

```

**Risk:** Medium risk

**Technical explanation:** Generates an encrypted variable value inline for safer insertion into vars files.

**Use when:** Use when a new secret must be added without leaving plaintext in editor history.


## 117. `ansible localhost -m debug -a 'var=db_password' -e '@group_vars/prod/vault.yml' --ask-vault-pass`


```bash

ansible localhost -m debug -a 'var=db_password' -e '@group_vars/prod/vault.yml' --ask-vault-pass

```

**Risk:** Medium risk

**Technical explanation:** Confirms that a vaulted variable decrypts and loads as expected.

**Use when:** Use when secret resolution succeeds in theory but may fail in practice.


## 118. `ansible-playbook -i inventory.ini site.yml -e 'ansible_no_log=false' -vvv`


```bash

ansible-playbook -i inventory.ini site.yml -e 'ansible_no_log=false' -vvv

```

**Risk:** Emergency-only

**Technical explanation:** Temporarily reduces log redaction only if the playbook is designed for it, which can expose hidden failure context and secrets.

**Use when:** Use only when secret-safe alternatives failed and the debugging need outweighs exposure risk.


## 119. `grep -R 'ANSIBLE_VAULT' -n group_vars host_vars`


```bash

grep -R 'ANSIBLE_VAULT' -n group_vars host_vars

```

**Risk:** Low risk

**Technical explanation:** Finds vaulted files quickly so you can locate where a secret is actually defined.

**Use when:** Use when you know a secret exists but not where it lives.


## 120. `ansible-config dump | grep vault`


```bash

ansible-config dump | grep vault

```

**Risk:** Low risk

**Technical explanation:** Shows active vault-related configuration so you can verify password-file and identity settings.

**Use when:** Use when vault behavior differs between local runs and automation.


# Recovery Verification by Scenario

## After inventory or targeting repair
Re-run:

- `ansible-inventory -i inventory.ini --graph`
- `ansible all -i inventory.ini --list-hosts`
- `ansible-playbook -i inventory.ini site.yml --limit app01 --list-hosts`

Verify that group hierarchy, matching hosts, and playbook limits now resolve exactly as intended.

## After SSH or authentication repair
Re-run:

- `ansible all -i inventory.ini -m ping`
- `ansible all -i inventory.ini -m ping -u deploy`
- `ssh -vvv deploy@app01`

Verify that both raw SSH and Ansible connectivity are healthy with the intended identity.

## After become or permission repair
Re-run:

- `ansible all -i inventory.ini -b -m ping`
- `ansible all -i inventory.ini -b -m command -a 'whoami'`
- `ansible all -i inventory.ini -m stat -a 'path=/etc/myapp.conf'`

Verify that escalation succeeds consistently and protected paths are now inspectable or writable as intended.

## After variable or template correction
Re-run:

- `ansible-inventory -i inventory.ini --host app01`
- `ansible all -i inventory.ini -m debug -a 'var=hostvars[inventory_hostname]'`
- `ansible all -i inventory.ini -m template -a 'src=templates/app.conf.j2 dest=/tmp/app.conf' --check --diff`

Verify that effective variables and rendered output now match expectations.

## After idempotence or handler recovery
Re-run:

- `ansible-playbook -i inventory.ini site.yml --check --diff --limit app01`
- `ansible-playbook -i inventory.ini site.yml --list-tasks`
- `ansible-playbook -i inventory.ini site.yml -vvv`

Verify that the play converges predictably, the task flow is still correct, and no hidden runtime surprises remain.

## After vault or secret handling repair
Re-run:

- `ansible-config dump | grep vault`
- `ansible-playbook -i inventory.ini site.yml --ask-vault-pass`
- `ansible localhost -m debug -a 'var=db_password' -e '@group_vars/prod/vault.yml' --ask-vault-pass`

Verify that vault configuration, decryption path, and secret variable resolution are working again.

## Total Commands

This pack contains **120 commands**.

# Troubleshooting Discipline

Strong Ansible incident response is not about knowing the most flags. It is about:

- proving target scope before execution
- reducing rollout blast radius deliberately
- separating controller, inventory, SSH, become, variable, and module failures
- using check, diff, and canary patterns before mutation
- handling secrets safely even during urgent recovery

That is the standard expected from a strong senior Ansible Platform, DevOps, or SRE engineer.
