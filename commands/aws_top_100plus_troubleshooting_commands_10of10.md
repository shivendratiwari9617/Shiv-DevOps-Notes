# AWS Top 100+ Production Troubleshooting Commands
**10/10 senior-level runbook pack for AWS Platform Engineers, DevOps Engineers, and SREs in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic AWS CLI cheat sheet.

Each command includes:

- the command itself
- a concise technical explanation
- a **risk level**
- a **use when** line so responders know where it fits in incident flow

## Command Risk Levels

- **Low risk** — read-only or generally safe diagnostic action
- **Medium risk** — controlled local-context change, scoped access step, or limited-impact runtime action
- **High risk** — can mutate service state, instance state, message flow, storage state, or production traffic behavior
- **Emergency-only** — use only in tightly controlled recovery situations with explicit confirmation

## Fast “First 5 Commands” by Incident Type

### Wrong account, role, or region suspicion
1. `aws sts get-caller-identity`
2. `aws configure list`
3. `aws configure list-profiles`
4. `aws configure get region --profile <profile>`
5. `AWS_PROFILE=<profile> aws sts get-caller-identity`

### EC2 unreachable or SSH failure
1. `aws ec2 describe-instances --instance-ids <instance-id>`
2. `aws ec2 describe-instance-status --instance-ids <instance-id> --include-all-instances`
3. `aws ec2 get-console-output --instance-id <instance-id> --latest`
4. `aws ssm get-connection-status --target <instance-id>`
5. `aws ssm start-session --target <instance-id>`

### SSM not connected or remote access broken
1. `aws ssm describe-instance-information`
2. `aws ssm get-connection-status --target <instance-id>`
3. `aws iam get-instance-profile --instance-profile-name <name>`
4. `aws ec2 describe-instances --instance-ids <instance-id>`
5. `aws ec2 describe-instance-status --instance-ids <instance-id> --include-all-instances`

### ALB/NLB 502, 503, or unhealthy targets
1. `aws elbv2 describe-load-balancers`
2. `aws elbv2 describe-target-groups`
3. `aws elbv2 describe-target-health --target-group-arn <tg-arn>`
4. `aws elbv2 describe-listeners --load-balancer-arn <lb-arn>`
5. `aws elbv2 describe-rules --listener-arn <listener-arn>`

### ECS service outage
1. `aws ecs describe-services --cluster <cluster-name> --services <service-name>`
2. `aws ecs list-tasks --cluster <cluster-name> --service-name <service-name>`
3. `aws ecs describe-tasks --cluster <cluster-name> --tasks <task-arn>`
4. `aws logs tail /aws/ecs/<service> --follow`
5. `aws elbv2 describe-target-health --target-group-arn <tg-arn>`

### EKS workload outage or pod crash loop
1. `aws eks update-kubeconfig --name <cluster-name>`
2. `kubectl config current-context`
3. `kubectl get pods -A -o wide`
4. `kubectl get events -A --sort-by=.lastTimestamp`
5. `kubectl describe pod <pod> -n <namespace>`

### Lambda failure or missing invocations
1. `aws lambda get-function-configuration --function-name <name>`
2. `aws lambda list-event-source-mappings --function-name <name>`
3. `aws logs tail /aws/lambda/<function-name> --follow`
4. `aws lambda get-policy --function-name <name>`
5. `aws lambda invoke --function-name <name> out.json`

### RDS or Aurora instability
1. `aws rds describe-db-instances --db-instance-identifier <id>`
2. `aws rds describe-events --source-identifier <id> --source-type db-instance`
3. `aws rds describe-db-log-files --db-instance-identifier <id>`
4. `aws rds download-db-log-file-portion --db-instance-identifier <id> --log-file-name <file>`
5. `aws rds describe-db-snapshots --db-instance-identifier <id>`

### S3 access denied or missing object
1. `aws s3api get-bucket-location --bucket <bucket>`
2. `aws s3api get-bucket-policy --bucket <bucket>`
3. `aws s3api get-public-access-block --bucket <bucket>`
4. `aws s3api head-object --bucket <bucket> --key <key>`
5. `aws sts get-caller-identity`

### SQS backlog, DLQ growth, or event delivery issue
1. `aws sqs list-queues`
2. `aws sqs get-queue-attributes --queue-url <url> --attribute-names All`
3. `aws sqs receive-message --queue-url <url> --max-number-of-messages 10`
4. `aws sns list-subscriptions-by-topic --topic-arn <topic-arn>`
5. `aws logs filter-log-events --log-group-name <group> --filter-pattern 'ERROR'`

### Route 53 or DNS mismatch
1. `aws route53 list-hosted-zones`
2. `aws route53 list-resource-record-sets --hosted-zone-id <zone-id>`
3. `aws route53 list-health-checks`
4. `dig <fqdn>`
5. `dig @8.8.8.8 <fqdn>`

### Quota or launch failure
1. `aws service-quotas list-service-quotas --service-code ec2`
2. `aws service-quotas get-service-quota --service-code vpc --quota-code <quota-code>`
3. `aws autoscaling describe-scaling-activities --auto-scaling-group-name <asg>`
4. `aws ec2 describe-launch-template-versions --launch-template-id <lt-id> --versions '$Latest'`
5. `aws health describe-events --filter services=EC2`

## Recommended Troubleshooting Flow

1. confirm account, role, profile, and region before every major action
2. separate IAM, control-plane, network, runtime, and application failure planes
3. prefer read-only inspection before mutation
4. collect console output, logs, CloudTrail evidence, and target health before restart or purge actions
5. use resource IDs, tags, and scoped filters aggressively to reduce noise
6. verify recovery from both the AWS control-plane view and the workload view

---

# AWS CLI, Auth, Profiles, and Caller Context


## 1. `aws --version`


```bash

aws --version

```

**Risk:** Low risk

**Technical explanation:** Prints the installed AWS CLI version so you can rule out client-version drift and feature mismatches between operator environments.

**Use when:** Use when commands behave differently across laptops, bastions, or CI runners.


## 2. `aws configure list`


```bash

aws configure list

```

**Risk:** Low risk

**Technical explanation:** Shows the active credential, region, and config resolution chain so you can confirm what the CLI is actually using.

**Use when:** Use at the start of an incident to validate local execution context.


## 3. `aws configure list-profiles`


```bash

aws configure list-profiles

```

**Risk:** Low risk

**Technical explanation:** Lists configured AWS CLI profiles so you can detect whether the wrong account profile is being used.

**Use when:** Use when you work across multiple accounts or environments from one machine.


## 4. `aws sts get-caller-identity`


```bash

aws sts get-caller-identity

```

**Risk:** Low risk

**Technical explanation:** Returns the current AWS account, user, or role identity, which is one of the fastest checks against wrong-account execution.

**Use when:** Use before any potentially high-impact production command.


## 5. `aws configure get region --profile <profile>`


```bash

aws configure get region --profile <profile>

```

**Risk:** Low risk

**Technical explanation:** Shows the configured default region for a profile so you can detect region drift before troubleshooting resources.

**Use when:** Use when resources appear missing or commands return empty results.


## 6. `aws ec2 describe-regions --all-regions`


```bash

aws ec2 describe-regions --all-regions

```

**Risk:** Low risk

**Technical explanation:** Lists AWS regions so you can confirm regional availability and validate where the service may actually be deployed.

**Use when:** Use when regional placement or failover location is unclear.


## 7. `AWS_PROFILE=<profile> aws sts get-caller-identity`


```bash

AWS_PROFILE=<profile> aws sts get-caller-identity

```

**Risk:** Low risk

**Technical explanation:** Tests an explicit profile context without changing your shell defaults, which is useful in multi-account incident response.

**Use when:** Use when you want to validate another profile safely without modifying current shell state.


## 8. `aws sts assume-role --role-arn <role-arn> --role-session-name debug-session`


```bash

aws sts assume-role --role-arn <role-arn> --role-session-name debug-session

```

**Risk:** Medium risk

**Technical explanation:** Requests temporary role credentials when incident access must be obtained through cross-account delegation.

**Use when:** Use when the correct operational access path is a role assumption, not a direct user credential.


## 9. `aws iam list-account-aliases`


```bash

aws iam list-account-aliases

```

**Risk:** Low risk

**Technical explanation:** Shows account aliases so you can add a human-readable check against account confusion.

**Use when:** Use when numeric account IDs are easy to confuse across estates.


## 10. `aws organizations describe-account --account-id <account-id>`


```bash

aws organizations describe-account --account-id <account-id>

```

**Risk:** Low risk

**Technical explanation:** Returns account metadata in Organizations-backed estates where account placement matters for incident scoping.

**Use when:** Use when OU placement, ownership, or account lifecycle state matters.


# IAM, Service Roles, and Access Troubleshooting


## 11. `aws iam get-user`


```bash

aws iam get-user

```

**Risk:** Low risk

**Technical explanation:** Returns details about the calling IAM user when the identity is user-based and access behavior needs confirmation.

**Use when:** Use when the operator identity is a user and not a role.


## 12. `aws iam list-roles`


```bash

aws iam list-roles

```

**Risk:** Low risk

**Technical explanation:** Lists IAM roles so you can verify whether the expected workload or admin role exists.

**Use when:** Use when an instance, task, or function may point at a non-existent role.


## 13. `aws iam get-role --role-name <role-name>`


```bash

aws iam get-role --role-name <role-name>

```

**Risk:** Low risk

**Technical explanation:** Shows one role’s trust policy and metadata, which is critical when assume-role behavior is failing.

**Use when:** Use when a service cannot assume its execution role or a human cannot assume an admin role.


## 14. `aws iam list-attached-role-policies --role-name <role-name>`


```bash

aws iam list-attached-role-policies --role-name <role-name>

```

**Risk:** Low risk

**Technical explanation:** Lists managed policies attached to a role so you can inspect effective permission sources.

**Use when:** Use when a role seems underprivileged or unexpectedly powerful.


## 15. `aws iam list-role-policies --role-name <role-name>`


```bash

aws iam list-role-policies --role-name <role-name>

```

**Risk:** Low risk

**Technical explanation:** Lists inline policies on a role, which helps uncover hidden or one-off permissions.

**Use when:** Use when attached managed policies do not fully explain access behavior.


## 16. `aws iam simulate-principal-policy --policy-source-arn <arn> --action-names <action>`


```bash

aws iam simulate-principal-policy --policy-source-arn <arn> --action-names <action>

```

**Risk:** Low risk

**Technical explanation:** Simulates whether a principal is allowed to call a specific action, which helps separate IAM denial from service-side failure.

**Use when:** Use when access denied errors need policy-level proof.


## 17. `aws iam get-policy --policy-arn <policy-arn>`


```bash

aws iam get-policy --policy-arn <policy-arn>

```

**Risk:** Low risk

**Technical explanation:** Retrieves policy metadata so you can validate the correct managed policy object.

**Use when:** Use when a policy ARN is referenced but its existence or attachment is in doubt.


## 18. `aws iam get-policy-version --policy-arn <policy-arn> --version-id <version>`


```bash

aws iam get-policy-version --policy-arn <policy-arn> --version-id <version>

```

**Risk:** Low risk

**Technical explanation:** Returns the actual policy document for a managed policy version during permission debugging.

**Use when:** Use when you need the exact allow/deny statements, not just the policy name.


## 19. `aws iam list-instance-profiles`


```bash

aws iam list-instance-profiles

```

**Risk:** Low risk

**Technical explanation:** Lists instance profiles so you can verify whether EC2 workloads have the intended IAM bridge object.

**Use when:** Use when EC2 role credentials are missing or attached incorrectly.


## 20. `aws iam get-instance-profile --instance-profile-name <name>`


```bash

aws iam get-instance-profile --instance-profile-name <name>

```

**Risk:** Low risk

**Technical explanation:** Shows roles bound to an instance profile when EC2 credential delivery is in question.

**Use when:** Use when an instance profile exists but may reference the wrong role.


# EC2 Instances, Status Checks, and Recovery


## 21. `aws ec2 describe-instances --filters Name=instance-state-name,Values=running`


```bash

aws ec2 describe-instances --filters Name=instance-state-name,Values=running

```

**Risk:** Low risk

**Technical explanation:** Lists running instances so you can baseline active compute in a region quickly.

**Use when:** Use when a fleet-level view of currently live EC2 is needed.


## 22. `aws ec2 describe-instances --instance-ids <instance-id>`


```bash

aws ec2 describe-instances --instance-ids <instance-id>

```

**Risk:** Low risk

**Technical explanation:** Returns detailed instance metadata including networking, IAM profile, state, and tags for one VM.

**Use when:** Use when one instance’s full control-plane config may explain the outage.


## 23. `aws ec2 describe-instance-status --instance-ids <instance-id> --include-all-instances`


```bash

aws ec2 describe-instance-status --instance-ids <instance-id> --include-all-instances

```

**Risk:** Low risk

**Technical explanation:** Shows system and instance status checks, which is essential when a host is unreachable or degraded.

**Use when:** Use when EC2 reachability may be impacted by AWS-side or guest-side health issues.


## 24. `aws ec2 get-console-output --instance-id <instance-id> --latest`


```bash

aws ec2 get-console-output --instance-id <instance-id> --latest

```

**Risk:** Low risk

**Technical explanation:** Retrieves console output so you can inspect boot or kernel messages when SSH is failing.

**Use when:** Use when the guest may be failing before network or SSH comes up.


## 25. `aws ec2 get-console-screenshot --instance-id <instance-id>`


```bash

aws ec2 get-console-screenshot --instance-id <instance-id>

```

**Risk:** Low risk

**Technical explanation:** Captures a screenshot of the instance console, which helps when boot is visually stuck or kernel panic symptoms exist.

**Use when:** Use when you need visual proof of a frozen or broken boot state.


## 26. `aws ec2 reboot-instances --instance-ids <instance-id>`


```bash

aws ec2 reboot-instances --instance-ids <instance-id>

```

**Risk:** High risk

**Technical explanation:** Performs an operating-system-level reboot request when a host may recover through a clean restart, but it interrupts the workload.

**Use when:** Use only after health checks, logs, and console evidence have been reviewed.


## 27. `aws ec2 stop-instances --instance-ids <instance-id>`


```bash

aws ec2 stop-instances --instance-ids <instance-id>

```

**Risk:** High risk

**Technical explanation:** Stops an instance when you need to preserve EBS-backed state, detach disks, or halt a faulty workload, but it causes immediate downtime.

**Use when:** Use only when downtime is intentional or already occurring.


## 28. `aws ec2 start-instances --instance-ids <instance-id>`


```bash

aws ec2 start-instances --instance-ids <instance-id>

```

**Risk:** Medium risk

**Technical explanation:** Starts a stopped instance so you can validate whether a host recovers after controlled intervention.

**Use when:** Use when the instance is meant to be brought back under controlled recovery.


## 29. `aws ec2 describe-instance-attribute --instance-id <instance-id> --attribute userData`


```bash

aws ec2 describe-instance-attribute --instance-id <instance-id> --attribute userData

```

**Risk:** Low risk

**Technical explanation:** Reads instance user data so you can verify bootstrap or startup-script logic.

**Use when:** Use when bad bootstrap logic or accidental reconfiguration may explain repeated host issues.


## 30. `aws ec2 describe-tags --filters Name=resource-id,Values=<instance-id>`


```bash

aws ec2 describe-tags --filters Name=resource-id,Values=<instance-id>

```

**Risk:** Low risk

**Technical explanation:** Lists instance tags, which helps validate discovery, ownership, automation, and routing assumptions.

**Use when:** Use when automation, cost allocation, or traffic selection depends on tags.


# SSM, Session Manager, and Remote Access


## 31. `aws ssm describe-instance-information`


```bash

aws ssm describe-instance-information

```

**Risk:** Low risk

**Technical explanation:** Lists SSM-managed instances so you can verify whether an EC2 host is connected to Systems Manager.

**Use when:** Use when SSH is unavailable and SSM may be the recovery path.


## 32. `aws ssm start-session --target <instance-id>`


```bash

aws ssm start-session --target <instance-id>

```

**Risk:** Medium risk

**Technical explanation:** Opens an SSM Session Manager shell when SSH is blocked or intentionally disabled.

**Use when:** Use when direct shell access is needed and SSM connectivity exists.


## 33. `aws ssm send-command --instance-ids <instance-id> --document-name AWS-RunShellScript --parameters commands='uptime'`


```bash

aws ssm send-command --instance-ids <instance-id> --document-name AWS-RunShellScript --parameters commands='uptime'

```

**Risk:** High risk

**Technical explanation:** Runs a remote command through SSM when you need non-interactive diagnostics on a managed host, but it executes code on the target.

**Use when:** Use only when remote execution is justified and command blast radius is understood.


## 34. `aws ssm list-command-invocations --details --command-id <command-id>`


```bash

aws ssm list-command-invocations --details --command-id <command-id>

```

**Risk:** Low risk

**Technical explanation:** Returns command execution output and status for an SSM Run Command request.

**Use when:** Use when a prior SSM command needs validation or output review.


## 35. `aws ssm get-connection-status --target <instance-id>`


```bash

aws ssm get-connection-status --target <instance-id>

```

**Risk:** Low risk

**Technical explanation:** Shows whether a target instance is currently connected for Session Manager operations.

**Use when:** Use when SSM access is expected but unavailable.


## 36. `aws ssm describe-sessions --state Active`


```bash

aws ssm describe-sessions --state Active

```

**Risk:** Low risk

**Technical explanation:** Lists active SSM sessions so you can identify current operator access during an incident.

**Use when:** Use when multiple responders may already be on the host.


## 37. `aws ssm get-parameter --name <param-name> --with-decryption`


```bash

aws ssm get-parameter --name <param-name> --with-decryption

```

**Risk:** Low risk

**Technical explanation:** Retrieves a Parameter Store value, which is useful when runtime config may be wrong or stale.

**Use when:** Use when configuration drift or bad secrets may explain behavior.


## 38. `aws ssm get-parameters-by-path --path /app/prod/ --recursive --with-decryption`


```bash

aws ssm get-parameters-by-path --path /app/prod/ --recursive --with-decryption

```

**Risk:** Low risk

**Technical explanation:** Recursively fetches configuration parameters for an application path during config drift analysis.

**Use when:** Use when an app depends on many hierarchical SSM parameters.


# EBS, AMIs, Snapshots, and Storage Attachments


## 39. `aws ec2 describe-volumes`


```bash

aws ec2 describe-volumes

```

**Risk:** Low risk

**Technical explanation:** Lists EBS volumes so you can locate unattached, oversized, or suspect storage devices.

**Use when:** Use when investigating storage topology or orphaned volumes.


## 40. `aws ec2 describe-volumes --volume-ids <volume-id>`


```bash

aws ec2 describe-volumes --volume-ids <volume-id>

```

**Risk:** Low risk

**Technical explanation:** Returns one volume’s attachment state, size, type, and AZ for focused storage troubleshooting.

**Use when:** Use when one disk may be detached, mis-sized, or in the wrong AZ.


## 41. `aws ec2 describe-volume-status --volume-ids <volume-id>`


```bash

aws ec2 describe-volume-status --volume-ids <volume-id>

```

**Risk:** Low risk

**Technical explanation:** Shows volume health and event status when EBS problems may underlie host instability.

**Use when:** Use when application IO issues may actually be EBS health issues.


## 42. `aws ec2 describe-snapshots --owner-ids self`


```bash

aws ec2 describe-snapshots --owner-ids self

```

**Risk:** Low risk

**Technical explanation:** Lists account-owned snapshots so you can verify recovery points before risky actions.

**Use when:** Use before making destructive or high-risk changes.


## 43. `aws ec2 create-snapshot --volume-id <volume-id> --description 'pre-change snapshot'`


```bash

aws ec2 create-snapshot --volume-id <volume-id> --description 'pre-change snapshot'

```

**Risk:** High risk

**Technical explanation:** Creates an EBS snapshot before mutation when you need a rollback point, but it changes account storage state and may incur cost.

**Use when:** Use when you need a pre-change safety point for a critical volume.


## 44. `aws ec2 describe-images --owners self`


```bash

aws ec2 describe-images --owners self

```

**Risk:** Low risk

**Technical explanation:** Lists self-owned AMIs so you can validate image provenance and deployment baselines.

**Use when:** Use when a rollout may be using the wrong AMI lineage.


## 45. `aws ec2 describe-image-attribute --image-id <ami-id> --attribute launchPermission`


```bash

aws ec2 describe-image-attribute --image-id <ami-id> --attribute launchPermission

```

**Risk:** Low risk

**Technical explanation:** Shows AMI launch permissions, which is useful when cross-account image use fails.

**Use when:** Use when another account or pipeline cannot launch the intended image.


## 46. `aws ec2 describe-instance-attribute --instance-id <instance-id> --attribute blockDeviceMapping`


```bash

aws ec2 describe-instance-attribute --instance-id <instance-id> --attribute blockDeviceMapping

```

**Risk:** Low risk

**Technical explanation:** Displays block device mapping so you can confirm expected root and data disks are attached.

**Use when:** Use when disk layout on the instance does not match expectation.


# VPC, Subnets, Security Groups, and Route Tables


## 47. `aws ec2 describe-vpcs`


```bash

aws ec2 describe-vpcs

```

**Risk:** Low risk

**Technical explanation:** Lists VPCs so you can confirm network boundaries and identify the correct virtual network.

**Use when:** Use when a workload may be deployed in the wrong VPC.


## 48. `aws ec2 describe-subnets`


```bash

aws ec2 describe-subnets

```

**Risk:** Low risk

**Technical explanation:** Lists subnets with CIDRs and AZs, which is useful when IP placement or AZ locality may explain behavior.

**Use when:** Use when subnet selection, capacity, or zone matters.


## 49. `aws ec2 describe-route-tables`


```bash

aws ec2 describe-route-tables

```

**Risk:** Low risk

**Technical explanation:** Shows route tables so you can validate how traffic should flow within a VPC.

**Use when:** Use when packets may not have the expected path.


## 50. `aws ec2 describe-route-tables --route-table-ids <rtb-id>`


```bash

aws ec2 describe-route-tables --route-table-ids <rtb-id>

```

**Risk:** Low risk

**Technical explanation:** Returns detailed route entries and associations for one route table during path debugging.

**Use when:** Use when one specific route table may be causing blackholing or asymmetric routing.


## 51. `aws ec2 describe-security-groups`


```bash

aws ec2 describe-security-groups

```

**Risk:** Low risk

**Technical explanation:** Lists security groups so you can inspect stateful network policy attached to workloads.

**Use when:** Use when traffic may be blocked by instance- or ENI-level policy.


## 52. `aws ec2 describe-security-groups --group-ids <sg-id>`


```bash

aws ec2 describe-security-groups --group-ids <sg-id>

```

**Risk:** Low risk

**Technical explanation:** Shows one security group’s ingress and egress rules in detail.

**Use when:** Use when one SG is the primary suspect.


## 53. `aws ec2 describe-network-interfaces --network-interface-ids <eni-id>`


```bash

aws ec2 describe-network-interfaces --network-interface-ids <eni-id>

```

**Risk:** Low risk

**Technical explanation:** Returns ENI attachment, security group, and IP details when interface-level drift may exist.

**Use when:** Use when IP, SG, or attachment issues may sit below the instance object.


## 54. `aws ec2 describe-network-acls`


```bash

aws ec2 describe-network-acls

```

**Risk:** Low risk

**Technical explanation:** Lists network ACLs so you can identify stateless subnet-level filtering that may block traffic.

**Use when:** Use when traffic is blocked despite apparently-correct security groups.


## 55. `aws ec2 describe-internet-gateways`


```bash

aws ec2 describe-internet-gateways

```

**Risk:** Low risk

**Technical explanation:** Lists internet gateways so you can confirm VPC external connectivity components.

**Use when:** Use when public subnet egress or ingress may be broken.


## 56. `aws ec2 describe-nat-gateways`


```bash

aws ec2 describe-nat-gateways

```

**Risk:** Low risk

**Technical explanation:** Shows NAT gateway state and addresses, which is key when private subnet egress is failing.

**Use when:** Use when outbound internet access from private subnets stops working.


# Elastic Load Balancing, Target Groups, and Health


## 57. `aws elbv2 describe-load-balancers`


```bash

aws elbv2 describe-load-balancers

```

**Risk:** Low risk

**Technical explanation:** Lists ALBs and NLBs so you can confirm the expected frontend objects exist.

**Use when:** Use when traffic entry points may be missing or misidentified.


## 58. `aws elbv2 describe-load-balancers --load-balancer-arns <lb-arn>`


```bash

aws elbv2 describe-load-balancers --load-balancer-arns <lb-arn>

```

**Risk:** Low risk

**Technical explanation:** Returns detailed load balancer properties including scheme, VPC, subnets, and state.

**Use when:** Use when one LB’s placement or scheme may explain user reachability issues.


## 59. `aws elbv2 describe-target-groups`


```bash

aws elbv2 describe-target-groups

```

**Risk:** Low risk

**Technical explanation:** Lists target groups so you can map listeners to backend pools during traffic incidents.

**Use when:** Use when you need to follow requests from frontend to backend.


## 60. `aws elbv2 describe-target-health --target-group-arn <tg-arn>`


```bash

aws elbv2 describe-target-health --target-group-arn <tg-arn>

```

**Risk:** Low risk

**Technical explanation:** Shows per-target health from the load balancer perspective, which is a primary signal during 502/503 incidents.

**Use when:** Use when the LB is reachable but backends appear unhealthy.


## 61. `aws elbv2 describe-listeners --load-balancer-arn <lb-arn>`


```bash

aws elbv2 describe-listeners --load-balancer-arn <lb-arn>

```

**Risk:** Low risk

**Technical explanation:** Lists listeners and ports so you can validate frontend protocol and certificate attachment.

**Use when:** Use when the wrong port, protocol, or TLS frontend may be configured.


## 62. `aws elbv2 describe-rules --listener-arn <listener-arn>`


```bash

aws elbv2 describe-rules --listener-arn <listener-arn>

```

**Risk:** Low risk

**Technical explanation:** Shows host/path routing rules when traffic may be routed to the wrong backend.

**Use when:** Use when only certain hosts or paths fail.


## 63. `aws elbv2 describe-load-balancer-attributes --load-balancer-arn <lb-arn>`


```bash

aws elbv2 describe-load-balancer-attributes --load-balancer-arn <lb-arn>

```

**Risk:** Low risk

**Technical explanation:** Displays LB attributes such as access logs and deletion protection for deeper troubleshooting.

**Use when:** Use when access logging or LB behavior flags may explain incident visibility gaps.


## 64. `aws elb describe-instance-health --load-balancer-name <name>`


```bash

aws elb describe-instance-health --load-balancer-name <name>

```

**Risk:** Low risk

**Technical explanation:** Shows Classic Load Balancer instance health for legacy estates still using CLB.

**Use when:** Use when operating older environments not yet on ALB/NLB.


# Route 53 and DNS Troubleshooting


## 65. `aws route53 list-hosted-zones`


```bash

aws route53 list-hosted-zones

```

**Risk:** Low risk

**Technical explanation:** Lists hosted zones so you can verify which DNS zones exist in the account.

**Use when:** Use when authoritative ownership of a hostname is unclear.


## 66. `aws route53 get-hosted-zone --id <zone-id>`


```bash

aws route53 get-hosted-zone --id <zone-id>

```

**Risk:** Low risk

**Technical explanation:** Shows details for one hosted zone, including name servers and configuration.

**Use when:** Use when zone delegation or name server data may matter.


## 67. `aws route53 list-resource-record-sets --hosted-zone-id <zone-id>`


```bash

aws route53 list-resource-record-sets --hosted-zone-id <zone-id>

```

**Risk:** Low risk

**Technical explanation:** Lists record sets in a zone so you can inspect authoritative DNS state.

**Use when:** Use when checking whether current Route 53 data matches expected endpoints.


## 68. `aws route53 list-health-checks`


```bash

aws route53 list-health-checks

```

**Risk:** Low risk

**Technical explanation:** Lists Route 53 health checks when failover or routing policy may depend on health status.

**Use when:** Use when DNS failover behavior may be driven by Route 53 health logic.


## 69. `aws route53 get-health-check --health-check-id <id>`


```bash

aws route53 get-health-check --health-check-id <id>

```

**Risk:** Low risk

**Technical explanation:** Shows health-check configuration to confirm probe path, interval, and expected behavior.

**Use when:** Use when Route 53 failover does not match intended thresholds or endpoints.


## 70. `aws route53 get-health-check-status --health-check-id <id>`


```bash

aws route53 get-health-check-status --health-check-id <id>

```

**Risk:** Low risk

**Technical explanation:** Returns live Route 53 health probe observations from AWS health checkers.

**Use when:** Use when a health check may be failing from AWS vantage points but not locally.


## 71. `dig <fqdn>`


```bash

dig <fqdn>

```

**Risk:** Low risk

**Technical explanation:** Queries DNS resolution directly from the shell so you can compare live answers with Route 53 configuration.

**Use when:** Use when live resolver output may not match authoritative config.


## 72. `dig @8.8.8.8 <fqdn>`


```bash

dig @8.8.8.8 <fqdn>

```

**Risk:** Low risk

**Technical explanation:** Queries a specific resolver to distinguish authoritative issues from local or recursive resolver issues.

**Use when:** Use when comparing propagation or resolver-path behavior.


# CloudWatch Metrics, Alarms, and Logs


## 73. `aws cloudwatch list-metrics --namespace AWS/EC2`


```bash

aws cloudwatch list-metrics --namespace AWS/EC2

```

**Risk:** Low risk

**Technical explanation:** Lists available EC2 metrics so you can confirm whether the expected signal exists.

**Use when:** Use when dashboards seem incomplete or metric names may be wrong.


## 74. `aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=<instance-id> --start-time <ts> --end-time <ts> --period 300 --statistics Average`


```bash

aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=<instance-id> --start-time <ts> --end-time <ts> --period 300 --statistics Average

```

**Risk:** Low risk

**Technical explanation:** Retrieves metric datapoints for time-bounded analysis when a dashboard is insufficient.

**Use when:** Use when you need raw timeseries evidence for one incident window.


## 75. `aws cloudwatch describe-alarms`


```bash

aws cloudwatch describe-alarms

```

**Risk:** Low risk

**Technical explanation:** Lists CloudWatch alarms so you can inspect the monitoring surface tied to incidents.

**Use when:** Use when validating what should have alerted or what is currently alarming.


## 76. `aws cloudwatch describe-alarms-for-metric --metric-name CPUUtilization --namespace AWS/EC2`


```bash

aws cloudwatch describe-alarms-for-metric --metric-name CPUUtilization --namespace AWS/EC2

```

**Risk:** Low risk

**Technical explanation:** Shows alarms for one metric so you can verify coverage and thresholds.

**Use when:** Use when you want to know whether a specific signal is monitored properly.


## 77. `aws logs describe-log-groups`


```bash

aws logs describe-log-groups

```

**Risk:** Low risk

**Technical explanation:** Lists CloudWatch log groups to discover where a service or application is emitting logs.

**Use when:** Use when you do not yet know where the evidence lives.


## 78. `aws logs describe-log-streams --log-group-name <group>`


```bash

aws logs describe-log-streams --log-group-name <group>

```

**Risk:** Low risk

**Technical explanation:** Lists log streams in a group so you can identify current or recent emitters.

**Use when:** Use when one service writes many streams and you need the active one.


## 79. `aws logs get-log-events --log-group-name <group> --log-stream-name <stream>`


```bash

aws logs get-log-events --log-group-name <group> --log-stream-name <stream>

```

**Risk:** Low risk

**Technical explanation:** Reads raw log events from one stream for precise incident inspection.

**Use when:** Use when a single stream contains the critical evidence.


## 80. `aws logs filter-log-events --log-group-name <group> --filter-pattern 'ERROR'`


```bash

aws logs filter-log-events --log-group-name <group> --filter-pattern 'ERROR'

```

**Risk:** Low risk

**Technical explanation:** Filters logs by pattern so you can narrow high-volume streams to likely failure evidence.

**Use when:** Use when log volume is too high for manual scanning.


## 81. `aws logs tail <group> --follow`


```bash

aws logs tail <group> --follow

```

**Risk:** Low risk

**Technical explanation:** Streams recent CloudWatch logs live, which is useful during reproduction or rollout monitoring.

**Use when:** Use when watching a live deployment or reproducing an issue.


## 82. `aws cloudwatch describe-alarm-history --alarm-name <alarm-name>`


```bash

aws cloudwatch describe-alarm-history --alarm-name <alarm-name>

```

**Risk:** Low risk

**Technical explanation:** Shows state transitions and alarm history so you can reconstruct alert timing.

**Use when:** Use when correlating symptoms with when alerting actually changed state.


# CloudTrail, Change Tracking, and Audit


## 83. `aws cloudtrail describe-trails`


```bash

aws cloudtrail describe-trails

```

**Risk:** Low risk

**Technical explanation:** Lists CloudTrail trails so you can confirm audit coverage and trail presence.

**Use when:** Use when you need to know whether actions should be recorded.


## 84. `aws cloudtrail get-trail-status --name <trail-name>`


```bash

aws cloudtrail get-trail-status --name <trail-name>

```

**Risk:** Low risk

**Technical explanation:** Shows whether a trail is logging correctly, which matters when audit gaps are suspected.

**Use when:** Use when you suspect missing CloudTrail events.


## 85. `aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceName,AttributeValue=<resource>`


```bash

aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceName,AttributeValue=<resource>

```

**Risk:** Low risk

**Technical explanation:** Searches CloudTrail for actions on one resource to identify who changed it and when.

**Use when:** Use when you need resource-specific change history fast.


## 86. `aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventName,AttributeValue=AuthorizeSecurityGroupIngress`


```bash

aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventName,AttributeValue=AuthorizeSecurityGroupIngress

```

**Risk:** Low risk

**Technical explanation:** Finds specific API events when network policy changes may have caused an incident.

**Use when:** Use when security group changes are suspected.


## 87. `aws configservice describe-configuration-recorders`


```bash

aws configservice describe-configuration-recorders

```

**Risk:** Low risk

**Technical explanation:** Shows AWS Config recorder status when configuration history should exist for drift analysis.

**Use when:** Use when you expect configuration history and need to verify recording is active.


## 88. `aws configservice get-resource-config-history --resource-type AWS::EC2::Instance --resource-id <instance-id>`


```bash

aws configservice get-resource-config-history --resource-type AWS::EC2::Instance --resource-id <instance-id>

```

**Risk:** Low risk

**Technical explanation:** Returns historical configuration snapshots for a resource when you need change over time.

**Use when:** Use when comparing current and past resource state.


# ECS, Services, and Task Health


## 89. `aws ecs list-clusters`


```bash

aws ecs list-clusters

```

**Risk:** Low risk

**Technical explanation:** Lists ECS clusters so you can confirm the service is deployed in the expected cluster.

**Use when:** Use when service location or naming may be wrong.


## 90. `aws ecs describe-clusters --clusters <cluster-name>`


```bash

aws ecs describe-clusters --clusters <cluster-name>

```

**Risk:** Low risk

**Technical explanation:** Shows cluster status and settings during ECS control-plane troubleshooting.

**Use when:** Use when a cluster-level issue may explain service instability.


## 91. `aws ecs list-services --cluster <cluster-name>`


```bash

aws ecs list-services --cluster <cluster-name>

```

**Risk:** Low risk

**Technical explanation:** Lists services in a cluster so you can identify the affected service quickly.

**Use when:** Use when triaging impact across one ECS cluster.


## 92. `aws ecs describe-services --cluster <cluster-name> --services <service-name>`


```bash

aws ecs describe-services --cluster <cluster-name> --services <service-name>

```

**Risk:** Low risk

**Technical explanation:** Returns desired count, running count, deployments, and events for an ECS service.

**Use when:** Use when a service may be under-replicated or stuck mid-deployment.


## 93. `aws ecs list-tasks --cluster <cluster-name> --service-name <service-name>`


```bash

aws ecs list-tasks --cluster <cluster-name> --service-name <service-name>

```

**Risk:** Low risk

**Technical explanation:** Lists tasks for a service so you can inspect placement and running state.

**Use when:** Use when desired count and actual running tasks differ.


## 94. `aws ecs describe-tasks --cluster <cluster-name> --tasks <task-arn>`


```bash

aws ecs describe-tasks --cluster <cluster-name> --tasks <task-arn>

```

**Risk:** Low risk

**Technical explanation:** Shows task-level container statuses, stop reasons, and attachment details.

**Use when:** Use when one task keeps stopping or failing health checks.


## 95. `aws logs tail /aws/ecs/<service> --follow`


```bash

aws logs tail /aws/ecs/<service> --follow

```

**Risk:** Low risk

**Technical explanation:** Follows ECS service logs in CloudWatch during live incident observation or rollout validation.

**Use when:** Use when ECS task logs are the fastest way to see current failure behavior.


## 96. `aws ecs execute-command --cluster <cluster> --task <task> --container <container> --interactive --command '/bin/sh'`


```bash

aws ecs execute-command --cluster <cluster> --task <task> --container <container> --interactive --command '/bin/sh'

```

**Risk:** Medium risk

**Technical explanation:** Opens an interactive shell into a running container when ECS Exec is enabled.

**Use when:** Use when in-container inspection is needed and you have confirmed the correct task target.


# EKS, Kubernetes, and Cluster Access


## 97. `aws eks list-clusters`


```bash

aws eks list-clusters

```

**Risk:** Low risk

**Technical explanation:** Lists EKS clusters so you can verify cluster presence and naming in the region.

**Use when:** Use when the issue may be targeting the wrong cluster or region.


## 98. `aws eks describe-cluster --name <cluster-name>`


```bash

aws eks describe-cluster --name <cluster-name>

```

**Risk:** Low risk

**Technical explanation:** Shows endpoint, status, version, VPC config, and logging settings for one cluster.

**Use when:** Use when cluster-level health or control-plane config may be involved.


## 99. `aws eks update-kubeconfig --name <cluster-name>`


```bash

aws eks update-kubeconfig --name <cluster-name>

```

**Risk:** Medium risk

**Technical explanation:** Writes kubeconfig for the target cluster so kubectl can reach the intended control plane, but it changes local kubectl context state.

**Use when:** Use when you need deliberate access to one cluster and have confirmed the target.


## 100. `kubectl config current-context`


```bash

kubectl config current-context

```

**Risk:** Low risk

**Technical explanation:** Shows the active kubectl context so you can confirm you are pointed at the correct EKS cluster.

**Use when:** Use before any mutating kubectl action.


## 101. `kubectl get nodes -o wide`


```bash

kubectl get nodes -o wide

```

**Risk:** Low risk

**Technical explanation:** Lists cluster nodes and readiness so you can baseline worker health quickly.

**Use when:** Use when cluster-wide health may be degraded.


## 102. `kubectl get pods -A -o wide`


```bash

kubectl get pods -A -o wide

```

**Risk:** Low risk

**Technical explanation:** Lists pods across namespaces to expose broad workload health, placement, and restart patterns.

**Use when:** Use when impact appears broader than one namespace.


## 103. `kubectl get events -A --sort-by=.lastTimestamp`


```bash

kubectl get events -A --sort-by=.lastTimestamp

```

**Risk:** Low risk

**Technical explanation:** Shows recent Kubernetes events, which often reveal scheduling, image pull, and readiness failures quickly.

**Use when:** Use when pod states alone do not explain the outage.


## 104. `kubectl describe pod <pod> -n <namespace>`


```bash

kubectl describe pod <pod> -n <namespace>

```

**Risk:** Low risk

**Technical explanation:** Provides pod-level detail, conditions, and events for a failing workload.

**Use when:** Use when one pod is CrashLoopBackOff, Pending, or Unready.


# Lambda, Event Sources, and Serverless


## 105. `aws lambda list-functions`


```bash

aws lambda list-functions

```

**Risk:** Low risk

**Technical explanation:** Lists Lambda functions so you can verify function presence and region placement.

**Use when:** Use when a serverless workload may be deployed in a different region or account.


## 106. `aws lambda get-function --function-name <name>`


```bash

aws lambda get-function --function-name <name>

```

**Risk:** Low risk

**Technical explanation:** Returns function configuration and code location for one Lambda during runtime debugging.

**Use when:** Use when code artifact identity or config may have drifted.


## 107. `aws lambda get-function-configuration --function-name <name>`


```bash

aws lambda get-function-configuration --function-name <name>

```

**Risk:** Low risk

**Technical explanation:** Shows runtime, timeout, memory, VPC config, and environment details that often explain failure behavior.

**Use when:** Use when timeouts, ENI issues, or env misconfiguration may be involved.


## 108. `aws lambda list-event-source-mappings --function-name <name>`


```bash

aws lambda list-event-source-mappings --function-name <name>

```

**Risk:** Low risk

**Technical explanation:** Lists triggers feeding a function so you can validate event delivery paths.

**Use when:** Use when the function works but is not being invoked correctly.


## 109. `aws logs tail /aws/lambda/<function-name> --follow`


```bash

aws logs tail /aws/lambda/<function-name> --follow

```

**Risk:** Low risk

**Technical explanation:** Streams Lambda logs live to observe invocation behavior and cold-start or exception patterns.

**Use when:** Use when reproducing a serverless failure or watching a fresh rollout.


## 110. `aws lambda invoke --function-name <name> out.json`


```bash

aws lambda invoke --function-name <name> out.json

```

**Risk:** Medium risk

**Technical explanation:** Invokes a function directly so you can test whether the runtime works independently of the event source.

**Use when:** Use when you need to separate function code/runtime failure from trigger failure.


## 111. `aws lambda get-policy --function-name <name>`


```bash

aws lambda get-policy --function-name <name>

```

**Risk:** Low risk

**Technical explanation:** Shows the resource policy on a Lambda function when cross-service invocation permissions may be failing.

**Use when:** Use when API Gateway, EventBridge, SNS, or S3 cannot invoke the function.


# RDS, Aurora, and Database Incidents


## 112. `aws rds describe-db-instances`


```bash

aws rds describe-db-instances

```

**Risk:** Low risk

**Technical explanation:** Lists RDS instances so you can locate the affected database and its current status.

**Use when:** Use when first scoping database impact in an account or region.


## 113. `aws rds describe-db-instances --db-instance-identifier <id>`


```bash

aws rds describe-db-instances --db-instance-identifier <id>

```

**Risk:** Low risk

**Technical explanation:** Returns detailed DB instance configuration including status, endpoint, engine, and maintenance windows.

**Use when:** Use when one database’s topology or current state may explain the outage.


## 114. `aws rds describe-db-clusters`


```bash

aws rds describe-db-clusters

```

**Risk:** Low risk

**Technical explanation:** Lists Aurora and cluster-level database resources for cluster-oriented troubleshooting.

**Use when:** Use when the estate uses Aurora or clustered RDS resources.


## 115. `aws rds describe-events --source-identifier <id> --source-type db-instance`


```bash

aws rds describe-events --source-identifier <id> --source-type db-instance

```

**Risk:** Low risk

**Technical explanation:** Shows recent RDS events such as failovers, restarts, maintenance, and storage warnings.

**Use when:** Use when database behavior may have changed without app-level visibility.


## 116. `aws rds describe-db-log-files --db-instance-identifier <id>`


```bash

aws rds describe-db-log-files --db-instance-identifier <id>

```

**Risk:** Low risk

**Technical explanation:** Lists available DB log files so you can see what server-side diagnostics exist.

**Use when:** Use when the database engine may hold the most useful evidence.


## 117. `aws rds download-db-log-file-portion --db-instance-identifier <id> --log-file-name <file>`


```bash

aws rds download-db-log-file-portion --db-instance-identifier <id> --log-file-name <file>

```

**Risk:** Low risk

**Technical explanation:** Downloads a slice of an RDS log file for detailed database error inspection.

**Use when:** Use when a particular RDS engine log needs targeted review.


## 118. `aws rds describe-db-snapshots --db-instance-identifier <id>`


```bash

aws rds describe-db-snapshots --db-instance-identifier <id>

```

**Risk:** Low risk

**Technical explanation:** Lists DB snapshots so you can verify restore points before risky remediation.

**Use when:** Use before any high-risk database recovery step.


# S3 Buckets, Object Access, and Policies


## 119. `aws s3 ls`


```bash

aws s3 ls

```

**Risk:** Low risk

**Technical explanation:** Lists buckets visible to the current principal so you can confirm account and S3 visibility quickly.

**Use when:** Use when account or permission context is uncertain.


## 120. `aws s3api list-buckets`


```bash

aws s3api list-buckets

```

**Risk:** Low risk

**Technical explanation:** Returns structured bucket metadata when scripting or exact creation timestamps matter.

**Use when:** Use when you need machine-readable bucket inventory.


## 121. `aws s3 ls s3://<bucket>`


```bash

aws s3 ls s3://<bucket>

```

**Risk:** Low risk

**Technical explanation:** Lists objects in a bucket or prefix so you can confirm data presence or recent deliveries.

**Use when:** Use when applications claim data is missing or delayed.


## 122. `aws s3api get-bucket-location --bucket <bucket>`


```bash

aws s3api get-bucket-location --bucket <bucket>

```

**Risk:** Low risk

**Technical explanation:** Shows a bucket’s region so you can explain unexpected redirect or access behavior.

**Use when:** Use when clients behave oddly due to region mismatch.


## 123. `aws s3api get-bucket-policy --bucket <bucket>`


```bash

aws s3api get-bucket-policy --bucket <bucket>

```

**Risk:** Low risk

**Technical explanation:** Retrieves the bucket policy for access troubleshooting.

**Use when:** Use when IAM seems correct but S3 access is still denied.


## 124. `aws s3api get-public-access-block --bucket <bucket>`


```bash

aws s3api get-public-access-block --bucket <bucket>

```

**Risk:** Low risk

**Technical explanation:** Shows public access block settings when policy behavior is not matching expectations.

**Use when:** Use when website hosting or public access behavior is surprising.


## 125. `aws s3api get-bucket-versioning --bucket <bucket>`


```bash

aws s3api get-bucket-versioning --bucket <bucket>

```

**Risk:** Low risk

**Technical explanation:** Shows whether object versioning is enabled, which matters in overwrite and recovery scenarios.

**Use when:** Use when data recovery depends on historical object versions.


## 126. `aws s3api head-object --bucket <bucket> --key <key>`


```bash

aws s3api head-object --bucket <bucket> --key <key>

```

**Risk:** Low risk

**Technical explanation:** Returns object metadata without downloading content, which is useful when validating presence and size.

**Use when:** Use when one object’s existence or metadata matters.


## 127. `aws s3 cp s3://<bucket>/<key> ./`


```bash

aws s3 cp s3://<bucket>/<key> ./

```

**Risk:** Medium risk

**Technical explanation:** Copies an object locally for evidence collection or data validation.

**Use when:** Use when local inspection is needed and downloading the object is acceptable.


## 128. `aws s3api get-bucket-encryption --bucket <bucket>`


```bash

aws s3api get-bucket-encryption --bucket <bucket>

```

**Risk:** Low risk

**Technical explanation:** Shows default encryption settings when write failures or compliance drift may involve SSE configuration.

**Use when:** Use when encryption policy may explain write or access problems.


# SQS, SNS, and Event Delivery


## 129. `aws sqs list-queues`


```bash

aws sqs list-queues

```

**Risk:** Low risk

**Technical explanation:** Lists SQS queues so you can verify expected messaging endpoints exist.

**Use when:** Use when producers or consumers may target the wrong queue.


## 130. `aws sqs get-queue-attributes --queue-url <url> --attribute-names All`


```bash

aws sqs get-queue-attributes --queue-url <url> --attribute-names All

```

**Risk:** Low risk

**Technical explanation:** Returns queue depth, visibility timeout, DLQ, and throughput-related attributes.

**Use when:** Use when backlog, redelivery, or dead-letter behavior needs confirmation.


## 131. `aws sqs receive-message --queue-url <url> --max-number-of-messages 10`


```bash

aws sqs receive-message --queue-url <url> --max-number-of-messages 10

```

**Risk:** Medium risk

**Technical explanation:** Pulls sample messages from a queue when you need to verify whether producers are delivering events.

**Use when:** Use when diagnostic message reads are acceptable and consumer semantics are understood.


## 132. `aws sqs purge-queue --queue-url <url>`


```bash

aws sqs purge-queue --queue-url <url>

```

**Risk:** Emergency-only

**Technical explanation:** Purges all visible and in-flight messages, which is destructive and used only in tightly controlled recovery situations.

**Use when:** Use only in explicit break-glass recovery with clear approval.


## 133. `aws sns list-topics`


```bash

aws sns list-topics

```

**Risk:** Low risk

**Technical explanation:** Lists SNS topics so you can verify publish endpoints exist in the account and region.

**Use when:** Use when fan-out architecture seems incomplete or mis-targeted.


## 134. `aws sns list-subscriptions-by-topic --topic-arn <topic-arn>`


```bash

aws sns list-subscriptions-by-topic --topic-arn <topic-arn>

```

**Risk:** Low risk

**Technical explanation:** Shows subscriptions bound to a topic when fan-out delivery is failing.

**Use when:** Use when publishes succeed but downstream subscribers are not receiving events.


## 135. `aws sns publish --topic-arn <topic-arn> --message 'test'`


```bash

aws sns publish --topic-arn <topic-arn> --message 'test'

```

**Risk:** High risk

**Technical explanation:** Publishes a test message to confirm SNS delivery paths in a controlled validation scenario, but it injects live traffic into the system.

**Use when:** Use only on safe test topics or when downstream impact is understood.


# Auto Scaling, Capacity, and Fleet Drift


## 136. `aws autoscaling describe-auto-scaling-groups`


```bash

aws autoscaling describe-auto-scaling-groups

```

**Risk:** Low risk

**Technical explanation:** Lists Auto Scaling groups so you can inspect desired, min, max, and instance membership.

**Use when:** Use when fleet capacity or instance churn may explain impact.


## 137. `aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names <asg>`


```bash

aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names <asg>

```

**Risk:** Low risk

**Technical explanation:** Returns one ASG’s capacity, subnets, launch template, and health state.

**Use when:** Use when one workload’s fleet behavior is under investigation.


## 138. `aws autoscaling describe-scaling-activities --auto-scaling-group-name <asg>`


```bash

aws autoscaling describe-scaling-activities --auto-scaling-group-name <asg>

```

**Risk:** Low risk

**Technical explanation:** Shows recent scale-out and scale-in actions to explain sudden capacity changes.

**Use when:** Use when load, cost, or availability changed around the same time as scaling events.


## 139. `aws autoscaling describe-policies --auto-scaling-group-name <asg>`


```bash

aws autoscaling describe-policies --auto-scaling-group-name <asg>

```

**Risk:** Low risk

**Technical explanation:** Lists scaling policies so you can validate how the fleet should respond to load.

**Use when:** Use when the fleet is not scaling as operators expect.


## 140. `aws ec2 describe-launch-template-versions --launch-template-id <lt-id>`


```bash

aws ec2 describe-launch-template-versions --launch-template-id <lt-id>

```

**Risk:** Low risk

**Technical explanation:** Shows launch template versions, which is critical when a bad rollout affects new instances.

**Use when:** Use when newly launched instances behave differently from older ones.


## 141. `aws ec2 describe-launch-template-versions --launch-template-id <lt-id> --versions '$Latest'`


```bash

aws ec2 describe-launch-template-versions --launch-template-id <lt-id> --versions '$Latest'

```

**Risk:** Low risk

**Technical explanation:** Retrieves the latest launch template version so you can compare rollout intent with actual config.

**Use when:** Use when latest-template drift may be causing broken scale-outs.


# ELB Access Logs, VPC Flow Logs, and Network Evidence


## 142. `aws ec2 describe-flow-logs`


```bash

aws ec2 describe-flow-logs

```

**Risk:** Low risk

**Technical explanation:** Lists VPC Flow Logs so you can confirm whether network telemetry is enabled for the relevant VPC, subnet, or ENI.

**Use when:** Use when you need packet-path evidence from AWS networking telemetry.


## 143. `aws ec2 describe-flow-logs --filter Name=resource-id,Values=<eni-id>`


```bash

aws ec2 describe-flow-logs --filter Name=resource-id,Values=<eni-id>

```

**Risk:** Low risk

**Technical explanation:** Filters flow logs to one resource so you can reduce noise during path-level network analysis.

**Use when:** Use when one ENI or interface is the main suspect.


## 144. `aws logs filter-log-events --log-group-name <vpc-flow-log-group> --filter-pattern '<srcaddr> <dstaddr>'`


```bash

aws logs filter-log-events --log-group-name <vpc-flow-log-group> --filter-pattern '<srcaddr> <dstaddr>'

```

**Risk:** Low risk

**Technical explanation:** Searches VPC Flow Log events for one traffic path to confirm whether packets are accepted or rejected.

**Use when:** Use when you need source-destination evidence across AWS network controls.


## 145. `aws elbv2 describe-load-balancer-attributes --load-balancer-arn <lb-arn>`


```bash

aws elbv2 describe-load-balancer-attributes --load-balancer-arn <lb-arn>

```

**Risk:** Low risk

**Technical explanation:** Shows whether access logs are enabled on an ALB or NLB and where they are delivered.

**Use when:** Use when request-level evidence from access logs is needed.


## 146. `aws s3 ls s3://<elb-log-bucket>/AWSLogs/`


```bash

aws s3 ls s3://<elb-log-bucket>/AWSLogs/

```

**Risk:** Low risk

**Technical explanation:** Lists ELB access log objects in S3 so you can confirm delivery and inspect request-level evidence.

**Use when:** Use when verifying that LB access logs are present for the incident window.


# Health, Quotas, Cost, and Multi-Service Incident Context


## 147. `aws health describe-events --filter services=EC2`


```bash

aws health describe-events --filter services=EC2

```

**Risk:** Low risk

**Technical explanation:** Returns AWS Health events affecting EC2 so you can detect provider-side incidents or maintenance.

**Use when:** Use when symptoms may be caused by AWS-side events rather than your workloads.


## 148. `aws service-quotas list-service-quotas --service-code ec2`


```bash

aws service-quotas list-service-quotas --service-code ec2

```

**Risk:** Low risk

**Technical explanation:** Lists EC2 quotas when launches or allocations fail due to account limits.

**Use when:** Use when apparently valid create operations are being rejected.


## 149. `aws service-quotas get-service-quota --service-code vpc --quota-code <quota-code>`


```bash

aws service-quotas get-service-quota --service-code vpc --quota-code <quota-code>

```

**Risk:** Low risk

**Technical explanation:** Retrieves one service quota value for focused limit troubleshooting.

**Use when:** Use when one known quota may be the exact blocker.


## 150. `aws ce get-cost-and-usage --time-period Start=<date>,End=<date> --granularity DAILY --metrics UnblendedCost`


```bash

aws ce get-cost-and-usage --time-period Start=<date>,End=<date> --granularity DAILY --metrics UnblendedCost

```

**Risk:** Low risk

**Technical explanation:** Pulls cost data that can reveal runaway services, traffic spikes, or storage explosions during incident review.

**Use when:** Use when the incident includes unexpected spend or growth signals.


## 151. `aws resourcegroupstaggingapi get-resources --tag-filters Key=Environment,Values=prod`


```bash

aws resourcegroupstaggingapi get-resources --tag-filters Key=Environment,Values=prod

```

**Risk:** Low risk

**Technical explanation:** Searches resources by tag so you can scope production blast radius quickly across services.

**Use when:** Use when you need to inventory prod resources fast across AWS services.


## 152. `aws resource-explorer-2 search --query-string 'resourcetype:ec2:instance'`


```bash

aws resource-explorer-2 search --query-string 'resourcetype:ec2:instance'

```

**Risk:** Low risk

**Technical explanation:** Searches indexed resources across an account and region when manual service-by-service discovery is too slow.

**Use when:** Use when responders need fast cross-service discovery under time pressure.

