# GCP Top 100+ Production Troubleshooting Commands
**10/10 senior-level runbook pack for GCP Platform Engineers, DevOps Engineers, and SREs in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic gcloud cheat sheet.

Each command includes:

- the command itself
- a concise technical explanation
- a **risk level**
- a **use when** line so responders know where it fits in incident flow

## Command Risk Levels

- **Low risk** — read-only or generally safe diagnostic action
- **Medium risk** — controlled local-context change, scoped access action, or limited-impact operational step
- **High risk** — can mutate IAM, traffic, metadata, service state, message flow, or production configuration
- **Emergency-only** — use only in tightly controlled recovery situations with explicit confirmation

## Fast “First 5 Commands” by Incident Type

### SSH failure to a GCE VM
1. `gcloud config list`
2. `gcloud compute instances describe <instance-name> --zone <zone>`
3. `gcloud compute ssh <instance-name> --zone <zone> --troubleshoot`
4. `gcloud compute firewall-rules list --filter='name~ssh'`
5. `gcloud compute instances get-serial-port-output <instance-name> --zone <zone>`

### Unhealthy load balancer backend or 502/503 symptoms
1. `gcloud compute backend-services list`
2. `gcloud compute backend-services get-health <backend-service-name> --global`
3. `gcloud compute health-checks describe <health-check-name>`
4. `gcloud compute backend-services describe <backend-service-name> --global`
5. `gcloud compute forwarding-rules describe <rule-name> --global`

### GKE workload outage or pod crash loop
1. `gcloud container clusters get-credentials <cluster-name> --region <region>`
2. `kubectl config current-context`
3. `kubectl get pods -A -o wide`
4. `kubectl get events -A --sort-by=.lastTimestamp`
5. `kubectl logs <pod-name> -n <namespace> --previous`

### Cloud Run service outage or bad rollout
1. `gcloud run services describe <service-name> --platform managed --region <region>`
2. `gcloud run revisions list --service <service-name> --region <region>`
3. `gcloud run revisions describe <revision-name> --region <region>`
4. `gcloud run services logs read <service-name> --region <region> --limit 100`
5. `gcloud logging read 'resource.type="cloud_run_revision"' --limit 50 --project <project-id>`

### Cloud SQL connectivity failure
1. `gcloud sql instances describe <instance-name>`
2. `gcloud sql operations list --instance <instance-name>`
3. `gcloud sql users list --instance <instance-name>`
4. `gcloud sql connect <instance-name> --user=<db-user>`
5. `gcloud logging read 'severity>=ERROR' --freshness=1h --limit 100 --project <project-id>`

### Pub/Sub delivery lag or missing events
1. `gcloud pubsub topics list --project <project-id>`
2. `gcloud pubsub subscriptions list --project <project-id>`
3. `gcloud pubsub subscriptions describe <subscription-name>`
4. `gcloud pubsub subscriptions pull <subscription-name> --limit=10 --auto-ack`
5. `gcloud pubsub snapshots list --project <project-id>`

### API disabled, permission denied, or service account issue
1. `gcloud auth list`
2. `gcloud config get-value project`
3. `gcloud services list --enabled --project <project-id>`
4. `gcloud iam service-accounts list --project <project-id>`
5. `gcloud projects get-iam-policy <project-id>`

### Quota exceeded or create-operation failure
1. `gcloud services quota list --service=compute.googleapis.com --consumer=projects/<project-number>`
2. `gcloud services quota list --service=container.googleapis.com --consumer=projects/<project-number>`
3. `gcloud logging read 'textPayload:"quota" OR protoPayload.status.message:"Quota"' --limit 50 --project <project-id>`
4. `gcloud beta billing projects describe <project-id>`
5. `gcloud services list --enabled --project <project-id>`

### DNS mismatch or resolution failure
1. `gcloud dns managed-zones list --project <project-id>`
2. `gcloud dns record-sets list --zone <zone-name> --project <project-id>`
3. `gcloud dns policies list --project <project-id>`
4. `dig <fqdn>`
5. `dig @8.8.8.8 <fqdn>`

## Recommended Troubleshooting Flow

1. confirm account, config, project, region, and zone before touching resources
2. separate IAM, API, billing, quota, network, runtime, and application failure planes
3. prefer read-only inspection before mutation
4. collect serial, audit, and service logs before resets or traffic changes
5. use project-scoped filters aggressively to reduce noise
6. verify recovery from the control-plane perspective and the workload perspective

---

# gcloud CLI, Auth, and Context


## 1. `gcloud version`


```bash

gcloud version

```

**Risk:** Low risk

**Technical explanation:** Prints installed SDK component versions so you can rule out CLI drift, missing components, or outdated behavior on the operator workstation.

**Use when:** Use when command behavior differs across laptops, bastions, or CI runners.


## 2. `gcloud info`


```bash

gcloud info

```

**Risk:** Low risk

**Technical explanation:** Shows active config, account, project, installation paths, and diagnostic environment details for fast CLI context validation.

**Use when:** Use at the start of an incident to confirm local operator context.


## 3. `gcloud config list`


```bash

gcloud config list

```

**Risk:** Low risk

**Technical explanation:** Displays the current gcloud configuration values so you can verify project, account, region, and zone before touching production.

**Use when:** Use before any project-scoped or regional operation.


## 4. `gcloud config configurations list`


```bash

gcloud config configurations list

```

**Risk:** Low risk

**Technical explanation:** Lists named gcloud configurations so you can detect whether the wrong profile is active during an incident.

**Use when:** Use when you manage multiple projects or environments from one workstation.


## 5. `gcloud config configurations activate <config-name>`


```bash

gcloud config configurations activate <config-name>

```

**Risk:** Medium risk

**Technical explanation:** Switches to a different saved configuration, which is useful when reproducing an issue in the correct project context.

**Use when:** Use when the active profile is wrong and you need to change working context deliberately.


## 6. `gcloud auth list`


```bash

gcloud auth list

```

**Risk:** Low risk

**Technical explanation:** Shows authenticated accounts and the active account so you can detect identity mismatch before debugging IAM or API failures.

**Use when:** Use when permission denied errors may stem from the wrong principal.


## 7. `gcloud auth login`


```bash

gcloud auth login

```

**Risk:** Medium risk

**Technical explanation:** Starts interactive user authentication when current credentials are stale or missing and you need to test access with a human principal.

**Use when:** Use when local user auth is broken and service behavior must be tested with a fresh login.


## 8. `gcloud auth application-default print-access-token`


```bash

gcloud auth application-default print-access-token

```

**Risk:** Low risk

**Technical explanation:** Verifies that Application Default Credentials are present and can mint a token, which helps separate ADC issues from gcloud user auth issues.

**Use when:** Use when SDK-based apps work differently from direct gcloud commands.


## 9. `gcloud auth print-access-token`


```bash

gcloud auth print-access-token

```

**Risk:** Low risk

**Technical explanation:** Prints an OAuth access token for the active account so you can test raw API calls and confirm token issuance.

**Use when:** Use when API calls fail and you want to prove token minting works.


## 10. `gcloud config get-value project`


```bash

gcloud config get-value project

```

**Risk:** Low risk

**Technical explanation:** Prints the active project ID directly so you can confirm you are pointed at the correct project in scripts and shells.

**Use when:** Use before any potentially high-impact command.


## 11. `gcloud config set project <project-id>`


```bash

gcloud config set project <project-id>

```

**Risk:** Medium risk

**Technical explanation:** Changes the active project context, which is often the fastest fix for wrong-project command output but changes subsequent command targets.

**Use when:** Use only when you have confirmed the current project is wrong.


## 12. `gcloud config set compute/region <region>`


```bash

gcloud config set compute/region <region>

```

**Risk:** Medium risk

**Technical explanation:** Sets a default region to reduce command ambiguity when troubleshooting regional resources.

**Use when:** Use when repeated regional commands are failing or defaulting incorrectly.


# Projects, APIs, IAM, and Organization Context


## 13. `gcloud projects list`


```bash

gcloud projects list

```

**Risk:** Low risk

**Technical explanation:** Lists visible projects for the current principal so you can confirm project visibility and access scope.

**Use when:** Use when the expected project seems missing or inaccessible.


## 14. `gcloud projects describe <project-id>`


```bash

gcloud projects describe <project-id>

```

**Risk:** Low risk

**Technical explanation:** Returns project metadata including numeric project number, labels, and lifecycle state needed for debugging cross-product references.

**Use when:** Use when a service expects the numeric project number or project metadata.


## 15. `gcloud services list --enabled --project <project-id>`


```bash

gcloud services list --enabled --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Shows enabled APIs so you can confirm whether a failing service is blocked by a disabled API.

**Use when:** Use when resource creation or control-plane calls fail unexpectedly.


## 16. `gcloud services list --available --project <project-id>`


```bash

gcloud services list --available --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists services that can be enabled, which helps when validating naming and service availability.

**Use when:** Use when you are unsure of the exact API service name.


## 17. `gcloud services enable <service-api> --project <project-id>`


```bash

gcloud services enable <service-api> --project <project-id>

```

**Risk:** High risk

**Technical explanation:** Enables a required API when resource creation or control-plane operations fail due to missing service activation, but it changes project service state.

**Use when:** Use only when disabled API state is confirmed as the blocker.


## 18. `gcloud iam service-accounts list --project <project-id>`


```bash

gcloud iam service-accounts list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists service accounts in a project so you can verify whether the expected workload identity exists.

**Use when:** Use when workloads fail with missing or wrong service account references.


## 19. `gcloud iam service-accounts describe <sa-email> --project <project-id>`


```bash

gcloud iam service-accounts describe <sa-email> --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Shows detailed metadata for a service account, which helps validate existence and unique ID references.

**Use when:** Use when one service account may have been deleted, renamed, or misreferenced.


## 20. `gcloud projects get-iam-policy <project-id>`


```bash

gcloud projects get-iam-policy <project-id>

```

**Risk:** Low risk

**Technical explanation:** Fetches the project IAM policy so you can inspect role bindings and confirm who should have access.

**Use when:** Use when a user, service account, or group may be missing permissions.


## 21. `gcloud projects add-iam-policy-binding <project-id> --member='user:user@example.com' --role='roles/viewer'`


```bash

gcloud projects add-iam-policy-binding <project-id> --member='user:user@example.com' --role='roles/viewer'

```

**Risk:** High risk

**Technical explanation:** Adds a project IAM binding during controlled access remediation when a principal is missing required rights, but it changes authorization state immediately.

**Use when:** Use only when the missing binding is confirmed and blast radius is understood.


## 22. `gcloud organizations list`


```bash

gcloud organizations list

```

**Risk:** Low risk

**Technical explanation:** Lists organizations visible to the current identity, which is important in multi-org enterprise estates.

**Use when:** Use when folder, org policy, or project placement issues may span organizations.


## 23. `gcloud resource-manager folders list --organization <org-id>`


```bash

gcloud resource-manager folders list --organization <org-id>

```

**Risk:** Low risk

**Technical explanation:** Lists folders under an organization so you can navigate enterprise hierarchy during policy or project-placement issues.

**Use when:** Use when the incident depends on folder-level policy or project location.


## 24. `gcloud beta billing projects describe <project-id>`


```bash

gcloud beta billing projects describe <project-id>

```

**Risk:** Low risk

**Technical explanation:** Shows billing linkage for a project, which is useful when resource creation fails because billing is absent or suspended.

**Use when:** Use when otherwise-correct create operations fail unexpectedly across services.


# Compute Engine Instances and VM State


## 25. `gcloud compute instances list --project <project-id>`


```bash

gcloud compute instances list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists VM instances so you can confirm whether the expected host exists, where it runs, and what state it is in.

**Use when:** Use when a VM may be missing, stopped, or deployed in a different zone.


## 26. `gcloud compute instances describe <instance-name> --zone <zone>`


```bash

gcloud compute instances describe <instance-name> --zone <zone>

```

**Risk:** Low risk

**Technical explanation:** Shows instance metadata, NICs, disks, service account, tags, and status for deep VM troubleshooting.

**Use when:** Use when one VM’s config may explain network, disk, or identity issues.


## 27. `gcloud compute instances get-serial-port-output <instance-name> --zone <zone>`


```bash

gcloud compute instances get-serial-port-output <instance-name> --zone <zone>

```

**Risk:** Low risk

**Technical explanation:** Retrieves serial console output, which is essential when SSH fails or the guest is stuck during boot.

**Use when:** Use when guest boot or kernel/user-space startup may be broken.


## 28. `gcloud compute instances reset <instance-name> --zone <zone>`


```bash

gcloud compute instances reset <instance-name> --zone <zone>

```

**Risk:** High risk

**Technical explanation:** Performs a hard reset on a VM, which is useful when the guest is unresponsive but can interrupt workloads abruptly.

**Use when:** Use only after collecting logs or serial evidence and confirming reset is justified.


## 29. `gcloud compute instances stop <instance-name> --zone <zone>`


```bash

gcloud compute instances stop <instance-name> --zone <zone>

```

**Risk:** High risk

**Technical explanation:** Stops a VM cleanly when you need to preserve state, detach disks, or halt a problematic workload, but it impacts service immediately.

**Use when:** Use only when downtime is acceptable or already in progress.


## 30. `gcloud compute instances start <instance-name> --zone <zone>`


```bash

gcloud compute instances start <instance-name> --zone <zone>

```

**Risk:** Medium risk

**Technical explanation:** Starts a stopped VM so you can validate whether a guest recovers after configuration or dependency fixes.

**Use when:** Use when the instance is intentionally stopped or failed previously and restart is planned.


## 31. `gcloud compute instances tail-serial-port-output <instance-name> --zone <zone>`


```bash

gcloud compute instances tail-serial-port-output <instance-name> --zone <zone>

```

**Risk:** Low risk

**Technical explanation:** Streams serial output in near real time, which helps during boot debugging and live recovery.

**Use when:** Use when boot or guest startup is still in progress and evidence is evolving.


## 32. `gcloud compute machine-types list --zones <zone>`


```bash

gcloud compute machine-types list --zones <zone>

```

**Risk:** Low risk

**Technical explanation:** Lists available machine types in a zone when capacity or resize operations are failing.

**Use when:** Use when instance resize or zonal capacity constraints may be the problem.


## 33. `gcloud compute instances list --filter='status!=RUNNING'`


```bash

gcloud compute instances list --filter='status!=RUNNING'

```

**Risk:** Low risk

**Technical explanation:** Filters instances not in RUNNING state so you can focus immediately on degraded or stopped hosts.

**Use when:** Use when broad fleet triage is needed across one project.


## 34. `gcloud compute disks list --project <project-id>`


```bash

gcloud compute disks list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists persistent disks so you can locate detached, large, or zone-specific storage relevant to an incident.

**Use when:** Use when disk attachment, orphaned storage, or zone locality matters.


## 35. `gcloud compute disks describe <disk-name> --zone <zone>`


```bash

gcloud compute disks describe <disk-name> --zone <zone>

```

**Risk:** Low risk

**Technical explanation:** Shows disk size, type, users, and source image references for storage and attachment debugging.

**Use when:** Use when one disk’s attachment state or tier may explain application behavior.


## 36. `gcloud compute snapshots list --project <project-id>`


```bash

gcloud compute snapshots list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists snapshots so you can verify whether recovery points exist before risky actions.

**Use when:** Use before destructive remediation or recovery planning.


# SSH, OS Login, and Guest Access


## 37. `gcloud compute ssh <instance-name> --zone <zone>`


```bash

gcloud compute ssh <instance-name> --zone <zone>

```

**Risk:** Medium risk

**Technical explanation:** Attempts SSH using Google-managed helpers, which is the standard first access test for a VM incident.

**Use when:** Use when validating whether host access works from the control-plane side.


## 38. `gcloud compute ssh <instance-name> --zone <zone> --troubleshoot`


```bash

gcloud compute ssh <instance-name> --zone <zone> --troubleshoot

```

**Risk:** Low risk

**Technical explanation:** Runs SSH diagnostics including IAM, firewall, network, and guest checks to explain why normal SSH fails.

**Use when:** Use when standard SSH is failing and you need a structured root-cause hint.


## 39. `gcloud compute scp ./localfile <instance-name>:/tmp/ --zone <zone>`


```bash

gcloud compute scp ./localfile <instance-name>:/tmp/ --zone <zone>

```

**Risk:** Medium risk

**Technical explanation:** Copies a file to a VM for controlled debugging or recovery steps when direct editing on the host is needed.

**Use when:** Use when you need to place a script, config, or binary on a reachable host.


## 40. `gcloud compute os-login describe-profile`


```bash

gcloud compute os-login describe-profile

```

**Risk:** Low risk

**Technical explanation:** Shows the current user's OS Login profile so you can validate whether OS Login identities are provisioned correctly.

**Use when:** Use when guest access depends on OS Login and the login mapping may be wrong.


## 41. `gcloud compute project-info describe`


```bash

gcloud compute project-info describe

```

**Risk:** Low risk

**Technical explanation:** Returns common project-wide metadata including SSH keys and settings that may affect guest access.

**Use when:** Use when inherited project metadata may be affecting all instances.


## 42. `gcloud compute instances add-metadata <instance-name> --zone <zone> --metadata enable-oslogin=TRUE`


```bash

gcloud compute instances add-metadata <instance-name> --zone <zone> --metadata enable-oslogin=TRUE

```

**Risk:** High risk

**Technical explanation:** Updates instance metadata to control OS Login behavior during guest-access remediation, but it changes instance login behavior.

**Use when:** Use only when OS Login state is confirmed as the access blocker.


## 43. `gcloud compute instances describe <instance-name> --zone <zone> --format='get(metadata.items)'`


```bash

gcloud compute instances describe <instance-name> --zone <zone> --format='get(metadata.items)'

```

**Risk:** Low risk

**Technical explanation:** Prints instance metadata items so you can inspect access-related flags and troubleshooting breadcrumbs.

**Use when:** Use when instance-local metadata may override project defaults.


## 44. `gcloud compute firewall-rules list --filter='name~ssh'`


```bash

gcloud compute firewall-rules list --filter='name~ssh'

```

**Risk:** Low risk

**Technical explanation:** Finds SSH-related firewall rules quickly when inbound access may be blocked.

**Use when:** Use when SSH appears blocked before reaching the guest.


## 45. `gcloud compute ssh <instance-name> --zone <zone> -- -vvv`


```bash

gcloud compute ssh <instance-name> --zone <zone> -- -vvv

```

**Risk:** Low risk

**Technical explanation:** Passes verbose SSH flags through gcloud so you can separate raw SSH issues from control-plane setup problems.

**Use when:** Use when you need packet- or auth-level SSH verbosity.


## 46. `gcloud compute instances get-guest-attributes <instance-name> --zone <zone>`


```bash

gcloud compute instances get-guest-attributes <instance-name> --zone <zone>

```

**Risk:** Low risk

**Technical explanation:** Retrieves guest attributes when the guest environment writes troubleshooting data there.

**Use when:** Use when guest agents or startup scripts publish state to guest attributes.


# VPC, Subnets, Firewall Rules, and Routes


## 47. `gcloud compute networks list`


```bash

gcloud compute networks list

```

**Risk:** Low risk

**Technical explanation:** Lists VPC networks so you can verify whether workloads are attached to the intended network.

**Use when:** Use when a workload may be placed in the wrong VPC.


## 48. `gcloud compute networks subnets list --project <project-id>`


```bash

gcloud compute networks subnets list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists subnets with regions and CIDRs, which helps validate IP space and subnet placement.

**Use when:** Use when routing, peering, or IP exhaustion may be subnet-related.


## 49. `gcloud compute networks subnets describe <subnet-name> --region <region>`


```bash

gcloud compute networks subnets describe <subnet-name> --region <region>

```

**Risk:** Low risk

**Technical explanation:** Shows subnet configuration including secondary ranges, private Google access, and flow logs.

**Use when:** Use when GKE, Private Google Access, or IP range behavior may be involved.


## 50. `gcloud compute firewall-rules list --project <project-id>`


```bash

gcloud compute firewall-rules list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists firewall rules so you can inspect whether expected allow or deny rules exist.

**Use when:** Use when traffic is not flowing and policy may block it.


## 51. `gcloud compute firewall-rules describe <rule-name>`


```bash

gcloud compute firewall-rules describe <rule-name>

```

**Risk:** Low risk

**Technical explanation:** Displays one firewall rule in detail, including priority, direction, tags, ranges, and ports.

**Use when:** Use when one specific rule may override intended traffic behavior.


## 52. `gcloud compute firewall-rules list --filter='network:<network-name>'`


```bash

gcloud compute firewall-rules list --filter='network:<network-name>'

```

**Risk:** Low risk

**Technical explanation:** Filters firewall rules to one VPC so noise is reduced during network incident triage.

**Use when:** Use when the project has many networks or rules.


## 53. `gcloud compute routes list --project <project-id>`


```bash

gcloud compute routes list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists routes so you can validate whether traffic should have a path to its destination.

**Use when:** Use when packets may be misrouted or blackholed.


## 54. `gcloud compute routes describe <route-name>`


```bash

gcloud compute routes describe <route-name>

```

**Risk:** Low risk

**Technical explanation:** Shows one route’s destination range, next hop, and priority for route debugging.

**Use when:** Use when one route may be unexpectedly winning traffic selection.


## 55. `gcloud compute routers list --regions <region>`


```bash

gcloud compute routers list --regions <region>

```

**Risk:** Low risk

**Technical explanation:** Lists Cloud Routers in a region, which is important for hybrid networking and BGP-related incidents.

**Use when:** Use when VPN, Interconnect, or hybrid pathing may be impacted.


## 56. `gcloud compute routers get-status <router-name> --region <region>`


```bash

gcloud compute routers get-status <router-name> --region <region>

```

**Risk:** Low risk

**Technical explanation:** Shows Cloud Router and BGP session status so you can diagnose route exchange problems.

**Use when:** Use when dynamic routing with on-prem or peer networks may be failing.


## 57. `gcloud compute addresses list --project <project-id>`


```bash

gcloud compute addresses list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists internal and external reserved IP addresses so you can verify expected VIP allocation.

**Use when:** Use when one address may be missing, unreserved, or attached elsewhere.


## 58. `gcloud compute forwarding-rules list --project <project-id>`


```bash

gcloud compute forwarding-rules list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists forwarding rules, which is useful when load balancer entry points are in doubt.

**Use when:** Use when external IP to target mapping may be wrong.


# Load Balancing, Health Checks, and Backend Services


## 59. `gcloud compute health-checks list`


```bash

gcloud compute health-checks list

```

**Risk:** Low risk

**Technical explanation:** Lists configured health checks so you can validate whether the expected probe object exists.

**Use when:** Use when backend health is unknown or misreported.


## 60. `gcloud compute health-checks describe <health-check-name>`


```bash

gcloud compute health-checks describe <health-check-name>

```

**Risk:** Low risk

**Technical explanation:** Shows health check intervals, timeouts, thresholds, and request parameters that may explain unhealthy backends.

**Use when:** Use when the probe itself may be too strict or misconfigured.


## 61. `gcloud compute backend-services list`


```bash

gcloud compute backend-services list

```

**Risk:** Low risk

**Technical explanation:** Lists backend services so you can confirm the LB service object and protocol configuration.

**Use when:** Use when tracing how traffic should reach the workload.


## 62. `gcloud compute backend-services describe <backend-service-name> --global`


```bash

gcloud compute backend-services describe <backend-service-name> --global

```

**Risk:** Low risk

**Technical explanation:** Displays backend service configuration including health checks, balancing mode, and attached backends.

**Use when:** Use when one backend service may route to the wrong instance group or NEG.


## 63. `gcloud compute backend-services get-health <backend-service-name> --global`


```bash

gcloud compute backend-services get-health <backend-service-name> --global

```

**Risk:** Low risk

**Technical explanation:** Returns backend health status from the load balancer perspective, which is one of the fastest checks during traffic incidents.

**Use when:** Use when the load balancer returns 502, 503, or partial outage symptoms.


## 64. `gcloud compute url-maps list`


```bash

gcloud compute url-maps list

```

**Risk:** Low risk

**Technical explanation:** Lists URL maps for HTTP(S) load balancers when routing rules may be misdirecting traffic.

**Use when:** Use when path-based or host-based routing may be wrong.


## 65. `gcloud compute url-maps describe <url-map-name>`


```bash

gcloud compute url-maps describe <url-map-name>

```

**Risk:** Low risk

**Technical explanation:** Shows host rules and path matchers so you can validate request routing behavior.

**Use when:** Use when only certain hosts or paths fail.


## 66. `gcloud compute target-http-proxies list`


```bash

gcloud compute target-http-proxies list

```

**Risk:** Low risk

**Technical explanation:** Lists HTTP proxies that front URL maps for classic or external HTTP load balancers.

**Use when:** Use when following the frontend chain through an HTTP LB.


## 67. `gcloud compute target-https-proxies list`


```bash

gcloud compute target-https-proxies list

```

**Risk:** Low risk

**Technical explanation:** Lists HTTPS proxies so you can inspect TLS frontends involved in user traffic.

**Use when:** Use when HTTPS-specific behavior or certificate attachment is in doubt.


## 68. `gcloud compute ssl-certificates list`


```bash

gcloud compute ssl-certificates list

```

**Risk:** Low risk

**Technical explanation:** Lists SSL certificate resources so you can verify which certs exist and whether they may be expired or replaced.

**Use when:** Use when TLS handshake or certificate rotation may be broken.


## 69. `gcloud compute ssl-certificates describe <cert-name>`


```bash

gcloud compute ssl-certificates describe <cert-name>

```

**Risk:** Low risk

**Technical explanation:** Shows certificate metadata including expiration and SANs for TLS incident debugging.

**Use when:** Use when one certificate may not match the hostname or may be near expiry.


## 70. `gcloud compute forwarding-rules describe <rule-name> --global`


```bash

gcloud compute forwarding-rules describe <rule-name> --global

```

**Risk:** Low risk

**Technical explanation:** Displays IP, port, and target binding for a global forwarding rule so you can follow the LB chain from entry point inward.

**Use when:** Use when the public IP or port appears correct but traffic still fails.


# Cloud DNS and Name Resolution


## 71. `gcloud dns managed-zones list --project <project-id>`


```bash

gcloud dns managed-zones list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists Cloud DNS zones so you can confirm that the expected zone exists in the target project.

**Use when:** Use when authoritative records seem absent or split across projects.


## 72. `gcloud dns managed-zones describe <zone-name> --project <project-id>`


```bash

gcloud dns managed-zones describe <zone-name> --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Shows zone visibility, DNS name, and DNSSEC settings that may affect resolution.

**Use when:** Use when private/public visibility or DNSSEC may explain failures.


## 73. `gcloud dns record-sets list --zone <zone-name> --project <project-id>`


```bash

gcloud dns record-sets list --zone <zone-name> --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists record sets in a zone so you can inspect current authoritative values.

**Use when:** Use when validating current records broadly in one zone.


## 74. `gcloud dns record-sets list --zone <zone-name> --name <fqdn>. --type A`


```bash

gcloud dns record-sets list --zone <zone-name> --name <fqdn>. --type A

```

**Risk:** Low risk

**Technical explanation:** Filters to a specific record name and type for fast validation of one endpoint.

**Use when:** Use when one hostname may be returning the wrong IP.


## 75. `gcloud dns policies list --project <project-id>`


```bash

gcloud dns policies list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists DNS policies, which is useful when inbound forwarding, logging, or alternative name resolution behavior is configured.

**Use when:** Use when resolver behavior differs across networks.


## 76. `gcloud dns policies describe <policy-name> --project <project-id>`


```bash

gcloud dns policies describe <policy-name> --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Shows DNS policy details so you can inspect forwarding and logging settings.

**Use when:** Use when forwarding rules or DNS policy overrides may be in play.


## 77. `dig <fqdn>`


```bash

dig <fqdn>

```

**Risk:** Low risk

**Technical explanation:** Queries DNS resolution directly from the shell to compare live answers with Cloud DNS configuration.

**Use when:** Use when you need to prove resolver output outside the control plane.


## 78. `dig @8.8.8.8 <fqdn>`


```bash

dig @8.8.8.8 <fqdn>

```

**Risk:** Low risk

**Technical explanation:** Queries a specific resolver to check whether the issue is authoritative, recursive, or local.

**Use when:** Use when comparing resolver paths or propagation behavior.


## 79. `nslookup <fqdn>`


```bash

nslookup <fqdn>

```

**Risk:** Low risk

**Technical explanation:** Performs a basic DNS lookup when a quick resolver test is sufficient.

**Use when:** Use when a lightweight check is enough.


## 80. `host <fqdn>`


```bash

host <fqdn>

```

**Risk:** Low risk

**Technical explanation:** Provides concise DNS answer output that is useful for quick verification in incidents.

**Use when:** Use when you want the shortest possible resolution output.


# Cloud Logging, Audit Logs, and Operations Visibility


## 81. `gcloud logging logs list --project <project-id>`


```bash

gcloud logging logs list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists available log names so you can discover where a service is emitting data.

**Use when:** Use when you do not yet know which logs hold the evidence.


## 82. `gcloud logging read 'resource.type="gce_instance"' --limit 50 --project <project-id>`


```bash

gcloud logging read 'resource.type="gce_instance"' --limit 50 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Reads recent Compute Engine logs with a filter, which is useful for narrowing noisy projects quickly.

**Use when:** Use when the issue appears VM-centric.


## 83. `gcloud logging read 'logName:"cloudaudit.googleapis.com"' --limit 50 --project <project-id>`


```bash

gcloud logging read 'logName:"cloudaudit.googleapis.com"' --limit 50 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Reads audit logs so you can determine who changed a resource or invoked an administrative API.

**Use when:** Use when a human or automation change may have caused the issue.


## 84. `gcloud logging read 'severity>=ERROR' --freshness=1h --limit 100 --project <project-id>`


```bash

gcloud logging read 'severity>=ERROR' --freshness=1h --limit 100 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Pulls recent error-level logs to get a high-signal incident snapshot.

**Use when:** Use early in incidents when you need a broad error overview.


## 85. `gcloud logging read 'resource.type="k8s_container" AND severity>=ERROR' --limit 100 --project <project-id>`


```bash

gcloud logging read 'resource.type="k8s_container" AND severity>=ERROR' --limit 100 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Reads recent Kubernetes container errors for GKE production debugging.

**Use when:** Use when cluster workloads are failing but control plane looks healthy.


## 86. `gcloud logging sinks list --project <project-id>`


```bash

gcloud logging sinks list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists log sinks so you can confirm whether logs are being exported to the expected destinations.

**Use when:** Use when SIEM, BigQuery, or storage exports are missing data.


## 87. `gcloud logging sinks describe <sink-name> --project <project-id>`


```bash

gcloud logging sinks describe <sink-name> --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Shows filter and destination details for a sink when log export behavior is in question.

**Use when:** Use when only some logs are exported or retained.


## 88. `gcloud logging metrics list --project <project-id>`


```bash

gcloud logging metrics list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists log-based metrics that may back alerts or dashboards during incident analysis.

**Use when:** Use when alert behavior depends on log-derived signals.


## 89. `gcloud alpha monitoring policies list --project <project-id>`


```bash

gcloud alpha monitoring policies list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists alerting policies so you can see which monitors may have fired or should have fired.

**Use when:** Use when alert coverage or missed paging is part of the incident review.


## 90. `gcloud alpha monitoring channels list --project <project-id>`


```bash

gcloud alpha monitoring channels list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists notification channels involved in incident alerting.

**Use when:** Use when alerts may have failed to reach the right destination.


## 91. `gcloud logging read 'protoPayload.status.code!=0' --limit 50 --project <project-id>`


```bash

gcloud logging read 'protoPayload.status.code!=0' --limit 50 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Finds failed API operations in logs, which helps when control-plane requests are erroring silently elsewhere.

**Use when:** Use when API calls fail but surface-level tooling is vague.


## 92. `gcloud logging read 'resource.type="cloud_run_revision"' --limit 50 --project <project-id>`


```bash

gcloud logging read 'resource.type="cloud_run_revision"' --limit 50 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Reads Cloud Run revision logs for service-level debugging.

**Use when:** Use when serverless traffic fails or a new revision may be broken.


# Monitoring, Metrics, and Alert-Oriented Checks


## 93. `gcloud monitoring dashboards list --project <project-id>`


```bash

gcloud monitoring dashboards list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists monitoring dashboards so you can locate operational views used by the team.

**Use when:** Use when you need to find the canonical dashboard quickly.


## 94. `gcloud monitoring uptime list-configs --project <project-id>`


```bash

gcloud monitoring uptime list-configs --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists uptime checks so you can verify whether endpoint availability is being monitored.

**Use when:** Use when monitoring coverage may be incomplete.


## 95. `gcloud monitoring uptime list-check-ips --project <project-id>`


```bash

gcloud monitoring uptime list-check-ips --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Shows uptime-check source IPs, which is useful when firewalling or allowlisting affects monitoring.

**Use when:** Use when uptime checks fail but user traffic works.


## 96. `gcloud beta monitoring channels list --project <project-id>`


```bash

gcloud beta monitoring channels list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists notification channels for alerts in environments using beta monitoring commands.

**Use when:** Use when paging or email notification paths may be broken.


## 97. `gcloud beta monitoring policies list --project <project-id>`


```bash

gcloud beta monitoring policies list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists alerting policies so you can review conditions during an incident or monitor gap.

**Use when:** Use when you need to validate that the right conditions were configured.


## 98. `gcloud logging read 'resource.type="uptime_url"' --limit 20 --project <project-id>`


```bash

gcloud logging read 'resource.type="uptime_url"' --limit 20 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Pulls recent uptime-check logs to verify if availability monitors observed the same issue.

**Use when:** Use when comparing monitor perspective with user reports.


## 99. `gcloud logging read 'metric.type="logging.googleapis.com/user"' --limit 20 --project <project-id>`


```bash

gcloud logging read 'metric.type="logging.googleapis.com/user"' --limit 20 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Surfaces logs related to user-defined log-based metrics when alerts depend on them.

**Use when:** Use when custom alert signals may have failed or drifted.


# GKE, Clusters, and Kubernetes Access


## 100. `gcloud container clusters list --project <project-id>`


```bash

gcloud container clusters list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists GKE clusters so you can confirm which clusters exist and where they run.

**Use when:** Use when cluster location or presence may be the problem.


## 101. `gcloud container clusters describe <cluster-name> --region <region>`


```bash

gcloud container clusters describe <cluster-name> --region <region>

```

**Risk:** Low risk

**Technical explanation:** Shows cluster mode, network config, release channel, node pools, and control-plane settings.

**Use when:** Use when control-plane or cluster-level config may explain the issue.


## 102. `gcloud container clusters get-credentials <cluster-name> --region <region>`


```bash

gcloud container clusters get-credentials <cluster-name> --region <region>

```

**Risk:** Medium risk

**Technical explanation:** Fetches kubeconfig credentials so kubectl can target the intended cluster during debugging, but it changes local kubectl context state.

**Use when:** Use when you need cluster access and have confirmed the desired target cluster.


## 103. `kubectl config current-context`


```bash

kubectl config current-context

```

**Risk:** Low risk

**Technical explanation:** Shows the active kubectl context so you can verify you are pointed at the correct cluster before running commands.

**Use when:** Use before any mutating kubectl operation.


## 104. `kubectl get nodes -o wide`


```bash

kubectl get nodes -o wide

```

**Risk:** Low risk

**Technical explanation:** Lists cluster nodes and addresses so you can see readiness and node placement quickly.

**Use when:** Use when cluster-wide health or node readiness is in question.


## 105. `kubectl get pods -A -o wide`


```bash

kubectl get pods -A -o wide

```

**Risk:** Low risk

**Technical explanation:** Lists pods across namespaces so you can identify restarts, placement, and broad cluster symptoms.

**Use when:** Use when impact appears broader than one namespace.


## 106. `kubectl get events -A --sort-by=.lastTimestamp`


```bash

kubectl get events -A --sort-by=.lastTimestamp

```

**Risk:** Low risk

**Technical explanation:** Shows the latest Kubernetes events, which often expose scheduling, image pull, and readiness problems rapidly.

**Use when:** Use when pod state is unclear and recent cluster events may explain it.


## 107. `kubectl describe pod <pod-name> -n <namespace>`


```bash

kubectl describe pod <pod-name> -n <namespace>

```

**Risk:** Low risk

**Technical explanation:** Provides detailed pod state, events, mounts, and container statuses for one failing workload.

**Use when:** Use when one pod is crash-looping, pending, or unready.


## 108. `kubectl logs <pod-name> -n <namespace> --previous`


```bash

kubectl logs <pod-name> -n <namespace> --previous

```

**Risk:** Low risk

**Technical explanation:** Fetches logs from the previous container instance when a pod is crash-looping.

**Use when:** Use when the current container already restarted and original failure logs are needed.


## 109. `kubectl get svc -A`


```bash

kubectl get svc -A

```

**Risk:** Low risk

**Technical explanation:** Lists services so you can verify internal and external exposure objects.

**Use when:** Use when service discovery or exposure may be broken.


## 110. `kubectl get ingress -A`


```bash

kubectl get ingress -A

```

**Risk:** Low risk

**Technical explanation:** Lists ingresses for quick validation of north-south routing objects in cluster workloads.

**Use when:** Use when external HTTP entry may be misconfigured in cluster workloads.


## 111. `gcloud container node-pools list --cluster <cluster-name> --region <region>`


```bash

gcloud container node-pools list --cluster <cluster-name> --region <region>

```

**Risk:** Low risk

**Technical explanation:** Lists node pools so you can inspect pool-level separation, versions, and autoscaling groups.

**Use when:** Use when only certain pools, nodes, or workloads appear impacted.


# Cloud Run and Serverless Runtime


## 112. `gcloud run services list --platform managed --region <region>`


```bash

gcloud run services list --platform managed --region <region>

```

**Risk:** Low risk

**Technical explanation:** Lists Cloud Run services in a region so you can verify service presence and visibility.

**Use when:** Use when a service may have been deployed to the wrong region or project.


## 113. `gcloud run services describe <service-name> --platform managed --region <region>`


```bash

gcloud run services describe <service-name> --platform managed --region <region>

```

**Risk:** Low risk

**Technical explanation:** Shows URL, traffic splits, revision routing, and runtime settings for a Cloud Run service.

**Use when:** Use when traffic or runtime config may have drifted.


## 114. `gcloud run revisions list --service <service-name> --region <region>`


```bash

gcloud run revisions list --service <service-name> --region <region>

```

**Risk:** Low risk

**Technical explanation:** Lists revisions so you can inspect recent deployments and traffic targets.

**Use when:** Use when an outage may correlate with a rollout.


## 115. `gcloud run revisions describe <revision-name> --region <region>`


```bash

gcloud run revisions describe <revision-name> --region <region>

```

**Risk:** Low risk

**Technical explanation:** Shows one revision’s configuration, which is useful when only a recent revision is failing.

**Use when:** Use when one bad revision may need isolation.


## 116. `gcloud run services logs read <service-name> --region <region> --limit 100`


```bash

gcloud run services logs read <service-name> --region <region> --limit 100

```

**Risk:** Low risk

**Technical explanation:** Reads Cloud Run service logs directly for fast serverless troubleshooting.

**Use when:** Use when request or container errors need fast review.


## 117. `gcloud run services update-traffic <service-name> --to-latest --region <region>`


```bash

gcloud run services update-traffic <service-name> --to-latest --region <region>

```

**Risk:** High risk

**Technical explanation:** Moves traffic to the latest revision during controlled rollout validation or remediation, but it changes production traffic routing.

**Use when:** Use only when traffic movement is the intended remediation and revision health is understood.


## 118. `gcloud functions list --regions <region>`


```bash

gcloud functions list --regions <region>

```

**Risk:** Low risk

**Technical explanation:** Lists Cloud Functions in a region so you can confirm deployed functions and generations.

**Use when:** Use when event-driven workloads appear missing or region placement is uncertain.


## 119. `gcloud functions describe <function-name> --region <region>`


```bash

gcloud functions describe <function-name> --region <region>

```

**Risk:** Low risk

**Technical explanation:** Shows trigger, runtime, source, and service account details for one function.

**Use when:** Use when one function’s config may explain failures.


## 120. `gcloud functions logs read <function-name> --region <region> --limit 100`


```bash

gcloud functions logs read <function-name> --region <region> --limit 100

```

**Risk:** Low risk

**Technical explanation:** Reads function logs directly, which is useful when debugging invocation failures or cold-start issues.

**Use when:** Use when function invocation is failing or timing out.


## 121. `gcloud eventarc triggers list --location <region>`


```bash

gcloud eventarc triggers list --location <region>

```

**Risk:** Low risk

**Technical explanation:** Lists Eventarc triggers so you can validate event-routing paths into serverless destinations.

**Use when:** Use when events are not reaching Cloud Run or Functions.


# Cloud Storage, Data Access, and Transfer


## 122. `gcloud storage buckets list --project <project-id>`


```bash

gcloud storage buckets list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists buckets so you can verify bucket existence and naming in the target project.

**Use when:** Use when storage paths may point to the wrong bucket or project.


## 123. `gcloud storage buckets describe gs://<bucket-name>`


```bash

gcloud storage buckets describe gs://<bucket-name>

```

**Risk:** Low risk

**Technical explanation:** Shows bucket configuration including location, versioning, retention, and lifecycle details.

**Use when:** Use when bucket policy or retention may block expected operations.


## 124. `gcloud storage ls gs://<bucket-name>`


```bash

gcloud storage ls gs://<bucket-name>

```

**Risk:** Low risk

**Technical explanation:** Lists objects in a bucket prefix so you can confirm data presence or absence quickly.

**Use when:** Use when an app claims data is missing.


## 125. `gcloud storage ls -L gs://<bucket-name>/<object-path>`


```bash

gcloud storage ls -L gs://<bucket-name>/<object-path>

```

**Risk:** Low risk

**Technical explanation:** Displays object metadata including generation, storage class, and timestamps for deep object debugging.

**Use when:** Use when one object’s metadata or generation matters.


## 126. `gcloud storage cp gs://<bucket-name>/<object> ./`


```bash

gcloud storage cp gs://<bucket-name>/<object> ./

```

**Risk:** Medium risk

**Technical explanation:** Copies an object locally for validation or incident evidence collection.

**Use when:** Use when you need to inspect or preserve one object outside the bucket.


## 127. `gcloud storage du -sh gs://<bucket-name>`


```bash

gcloud storage du -sh gs://<bucket-name>

```

**Risk:** Low risk

**Technical explanation:** Summarizes bucket size, which helps during storage growth or cost anomalies.

**Use when:** Use when sudden growth or retention drift may be occurring.


## 128. `gcloud storage buckets get-iam-policy gs://<bucket-name>`


```bash

gcloud storage buckets get-iam-policy gs://<bucket-name>

```

**Risk:** Low risk

**Technical explanation:** Fetches bucket IAM policy so you can debug data access failures.

**Use when:** Use when principals can see the bucket but cannot use it correctly.


## 129. `gcloud storage buckets update gs://<bucket-name> --uniform-bucket-level-access`


```bash

gcloud storage buckets update gs://<bucket-name> --uniform-bucket-level-access

```

**Risk:** High risk

**Technical explanation:** Enables uniform bucket-level access during controlled access-model remediation, but changes how authorization is evaluated.

**Use when:** Use only when ACL-vs-IAM model issues are the confirmed problem.


## 130. `gsutil ls -L -b gs://<bucket-name>`


```bash

gsutil ls -L -b gs://<bucket-name>

```

**Risk:** Low risk

**Technical explanation:** Shows detailed bucket metadata using gsutil, which is still common in legacy and mixed-operational environments.

**Use when:** Use when the team still relies on gsutil-oriented workflows.


## 131. `gsutil iam get gs://<bucket-name>`


```bash

gsutil iam get gs://<bucket-name>

```

**Risk:** Low risk

**Technical explanation:** Retrieves bucket IAM using gsutil for teams that still rely on gsutil in runbooks.

**Use when:** Use when comparing gsutil behavior with gcloud storage output.


# Cloud SQL, Databases, and Connectivity


## 132. `gcloud sql instances list --project <project-id>`


```bash

gcloud sql instances list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists Cloud SQL instances so you can confirm instance names, versions, and states.

**Use when:** Use when DB connectivity incidents may target the wrong instance or region.


## 133. `gcloud sql instances describe <instance-name>`


```bash

gcloud sql instances describe <instance-name>

```

**Risk:** Low risk

**Technical explanation:** Shows Cloud SQL configuration including region, availability, IPs, maintenance, and failover settings.

**Use when:** Use when one instance’s topology or network config may explain the outage.


## 134. `gcloud sql operations list --instance <instance-name>`


```bash

gcloud sql operations list --instance <instance-name>

```

**Risk:** Low risk

**Technical explanation:** Lists recent operations so you can see whether maintenance, restart, or admin tasks are in progress or failed.

**Use when:** Use when DB behavior may be explained by recent control-plane actions.


## 135. `gcloud sql backups list --instance <instance-name>`


```bash

gcloud sql backups list --instance <instance-name>

```

**Risk:** Low risk

**Technical explanation:** Lists backups to verify whether recoverable restore points exist.

**Use when:** Use before risky remediation or recovery planning.


## 136. `gcloud sql databases list --instance <instance-name>`


```bash

gcloud sql databases list --instance <instance-name>

```

**Risk:** Low risk

**Technical explanation:** Lists logical databases so you can validate expected DB presence.

**Use when:** Use when one application database may be missing or incorrectly named.


## 137. `gcloud sql users list --instance <instance-name>`


```bash

gcloud sql users list --instance <instance-name>

```

**Risk:** Low risk

**Technical explanation:** Lists database users managed by Cloud SQL for access debugging.

**Use when:** Use when auth failures may stem from wrong or missing DB principals.


## 138. `gcloud sql connect <instance-name> --user=<db-user>`


```bash

gcloud sql connect <instance-name> --user=<db-user>

```

**Risk:** Medium risk

**Technical explanation:** Attempts an interactive SQL connection through gcloud helpers when validating reachability and auth.

**Use when:** Use when you need to prove connectivity from operator context to the instance.


## 139. `gcloud sql instances patch <instance-name> --activation-policy=ALWAYS`


```bash

gcloud sql instances patch <instance-name> --activation-policy=ALWAYS

```

**Risk:** High risk

**Technical explanation:** Ensures an instance is configured to stay on when availability behavior is part of the incident, but it changes instance settings.

**Use when:** Use only when activation policy is confirmed as a contributing factor.


# Pub/Sub, Messaging, and Event Flow


## 140. `gcloud pubsub topics list --project <project-id>`


```bash

gcloud pubsub topics list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists Pub/Sub topics so you can verify that the expected event ingress objects exist.

**Use when:** Use when publishers or event sources may target the wrong project.


## 141. `gcloud pubsub subscriptions list --project <project-id>`


```bash

gcloud pubsub subscriptions list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists subscriptions so you can inspect downstream consumption paths.

**Use when:** Use when events seem published but not consumed.


## 142. `gcloud pubsub topics describe <topic-name>`


```bash

gcloud pubsub topics describe <topic-name>

```

**Risk:** Low risk

**Technical explanation:** Shows topic metadata, which is useful when permissions or schema bindings matter.

**Use when:** Use when one topic’s config may explain publish failures.


## 143. `gcloud pubsub subscriptions describe <subscription-name>`


```bash

gcloud pubsub subscriptions describe <subscription-name>

```

**Risk:** Low risk

**Technical explanation:** Displays ack deadline, retention, dead-letter, and push config details for one subscription.

**Use when:** Use when backlog, redelivery, or push delivery looks wrong.


## 144. `gcloud pubsub subscriptions pull <subscription-name> --limit=10 --auto-ack`


```bash

gcloud pubsub subscriptions pull <subscription-name> --limit=10 --auto-ack

```

**Risk:** Medium risk

**Technical explanation:** Pulls messages directly to validate whether events are present in a subscription, but it consumes and acknowledges messages.

**Use when:** Use only when message loss from this diagnostic read is acceptable.


## 145. `gcloud pubsub topics publish <topic-name> --message='test-event'`


```bash

gcloud pubsub topics publish <topic-name> --message='test-event'

```

**Risk:** High risk

**Technical explanation:** Publishes a test message when validating whether publishers, subscriptions, and consumers are wired correctly, but it injects live traffic into the system.

**Use when:** Use only in controlled testing paths or non-production-impacting topics.


## 146. `gcloud pubsub snapshots list --project <project-id>`


```bash

gcloud pubsub snapshots list --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists Pub/Sub snapshots so you can see whether rewind or message-state recovery points exist.

**Use when:** Use when replay or rollback may be needed.


## 147. `gcloud pubsub subscriptions seek <subscription-name> --time='<timestamp>'`


```bash

gcloud pubsub subscriptions seek <subscription-name> --time='<timestamp>'

```

**Risk:** High risk

**Technical explanation:** Moves a subscription cursor to a point in time during controlled replay or recovery workflows, which can alter message delivery state significantly.

**Use when:** Use only when replay, rewind, or recovery semantics are fully understood.


# Artifact Registry, Images, and Release Artifacts


## 148. `gcloud artifacts repositories list --project <project-id> --location <region>`


```bash

gcloud artifacts repositories list --project <project-id> --location <region>

```

**Risk:** Low risk

**Technical explanation:** Lists Artifact Registry repositories so you can confirm repository presence and region.

**Use when:** Use when deployment tooling cannot find the expected repository.


## 149. `gcloud artifacts repositories describe <repo-name> --location <region>`


```bash

gcloud artifacts repositories describe <repo-name> --location <region>

```

**Risk:** Low risk

**Technical explanation:** Shows repository format, description, and configuration when image or package retrieval fails.

**Use when:** Use when repository format or region mismatch may be involved.


## 150. `gcloud artifacts docker images list <region>-docker.pkg.dev/<project-id>/<repo-name>`


```bash

gcloud artifacts docker images list <region>-docker.pkg.dev/<project-id>/<repo-name>

```

**Risk:** Low risk

**Technical explanation:** Lists images in a Docker repository so you can validate expected tags and digests.

**Use when:** Use when the deployed image tag may not exist or may point to the wrong build.


## 151. `gcloud artifacts files list --project <project-id> --location <region> --repository <repo-name>`


```bash

gcloud artifacts files list --project <project-id> --location <region> --repository <repo-name>

```

**Risk:** Low risk

**Technical explanation:** Lists artifact files for deeper repository inventory inspection.

**Use when:** Use when package-level contents, not just image tags, need validation.


## 152. `gcloud container images list --repository gcr.io/<project-id>`


```bash

gcloud container images list --repository gcr.io/<project-id>

```

**Risk:** Low risk

**Technical explanation:** Lists images in Container Registry for legacy environments still using GCR.

**Use when:** Use when older projects still deploy from Container Registry.


## 153. `gcloud container images list-tags gcr.io/<project-id>/<image-name>`


```bash

gcloud container images list-tags gcr.io/<project-id>/<image-name>

```

**Risk:** Low risk

**Technical explanation:** Shows tags and digests for one GCR image, which is useful for release identity confirmation.

**Use when:** Use when mutable tags may hide which digest actually shipped.


## 154. `gcloud auth configure-docker`


```bash

gcloud auth configure-docker

```

**Risk:** Medium risk

**Technical explanation:** Configures Docker credential helpers for Google registries so you can rule out local auth issues during image pulls and pushes.

**Use when:** Use when registry access works in gcloud but fails in Docker clients.


# Quotas, APIs, Billing, and Incident-Scale Inventory


## 155. `gcloud compute project-info describe --project <project-id>`


```bash

gcloud compute project-info describe --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Shows project-level compute metadata including common limits and SSH metadata used in compute operations.

**Use when:** Use when compute-wide settings or metadata may influence multiple VMs.


## 156. `gcloud services quota list --service=compute.googleapis.com --consumer=projects/<project-number>`


```bash

gcloud services quota list --service=compute.googleapis.com --consumer=projects/<project-number>

```

**Risk:** Low risk

**Technical explanation:** Lists service quotas so you can confirm whether resource creation failures are quota-related.

**Use when:** Use when compute operations fail despite valid config and permissions.


## 157. `gcloud services quota list --service=container.googleapis.com --consumer=projects/<project-number>`


```bash

gcloud services quota list --service=container.googleapis.com --consumer=projects/<project-number>

```

**Risk:** Low risk

**Technical explanation:** Shows GKE-related quotas when cluster scaling or creation is blocked.

**Use when:** Use when cluster or node-pool scale-up fails unexpectedly.


## 158. `gcloud recommender recommendations list --project <project-id> --location=global --recommender=google.compute.instance.MachineTypeRecommender`


```bash

gcloud recommender recommendations list --project <project-id> --location=global --recommender=google.compute.instance.MachineTypeRecommender

```

**Risk:** Low risk

**Technical explanation:** Lists machine type recommendations, which can help explain sustained underprovisioning or waste.

**Use when:** Use when chronic capacity or cost imbalance may be contributing to incidents.


## 159. `gcloud asset search-all-resources --scope=projects/<project-id> --query='name:instance'`


```bash

gcloud asset search-all-resources --scope=projects/<project-id> --query='name:instance'

```

**Risk:** Low risk

**Technical explanation:** Searches Cloud Asset Inventory for resources across a project when manual listing is too narrow.

**Use when:** Use when the exact resource location or type is unclear.


## 160. `gcloud asset search-all-iam-policies --scope=projects/<project-id> --query='roles/editor'`


```bash

gcloud asset search-all-iam-policies --scope=projects/<project-id> --query='roles/editor'

```

**Risk:** Low risk

**Technical explanation:** Searches IAM policies broadly so you can find where a role is granted.

**Use when:** Use when inherited or scattered access grants must be discovered.


## 161. `gcloud billing accounts list`


```bash

gcloud billing accounts list

```

**Risk:** Low risk

**Technical explanation:** Lists billing accounts visible to the user, which is useful when linking project availability to financial configuration.

**Use when:** Use when resource errors may actually be billing-related.


## 162. `gcloud beta billing projects list --billing-account=<billing-account-id>`


```bash

gcloud beta billing projects list --billing-account=<billing-account-id>

```

**Risk:** Low risk

**Technical explanation:** Lists projects attached to a billing account so you can assess blast radius of billing issues.

**Use when:** Use when one billing account problem may affect multiple projects.


## 163. `gcloud logging read 'textPayload:"quota" OR protoPayload.status.message:"Quota"' --limit 50 --project <project-id>`


```bash

gcloud logging read 'textPayload:"quota" OR protoPayload.status.message:"Quota"' --limit 50 --project <project-id>

```

**Risk:** Low risk

**Technical explanation:** Searches logs for quota-related failures to correlate resource errors with control-plane limits.

**Use when:** Use when quota errors are suspected but not surfaced consistently by clients.


## 164. `gcloud projects list --filter='lifecycleState:ACTIVE'`


```bash

gcloud projects list --filter='lifecycleState:ACTIVE'

```

**Risk:** Low risk

**Technical explanation:** Lists only active projects, which is useful for broad operational inventory and incident scoping.

**Use when:** Use when you need to scope active estate impact quickly.

