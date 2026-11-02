# AWS Production Issues and Troubleshooting Runbook

This runbook covers 10 common AWS production issues with symptoms, root causes, troubleshooting steps, verification commands, solutions, and rollback guidance.

---

## 1. EC2 application is down after deployment

### Symptoms
- Website or API returns 5xx
- SSH works but app is unreachable
- Load balancer marks instance unhealthy

### Common root causes
- App process failed to start
- Wrong port binding
- Bad environment variables
- Systemd service failed
- Security group or NACL issue

### Troubleshooting steps
1. Check whether the issue affects one instance or all.
2. Verify the app process is running.
3. Confirm the app is listening on the expected port.
4. Review service and deployment logs.
5. Check target group health.

### Verification commands
```bash
sudo systemctl status myapp
sudo journalctl -u myapp -n 100 --no-pager
ps -ef | grep myapp
ss -tulpn | grep 8080
curl -I http://localhost:8080/health
aws elbv2 describe-target-health --target-group-arn <target-group-arn>
```

### Solution
- Restart the service
- Fix env vars or config
- Rebind the app to the correct port
- Re-register healthy instances
- Roll back to the last stable release

### Rollback
```bash
sudo systemctl stop myapp
sudo ln -sfn /opt/myapp/releases/<previous_release> /opt/myapp/current
sudo systemctl start myapp
```

---

## 2. ALB returns 502 or 503

### Symptoms
- Users see 502 or 503
- ALB is reachable, backend is not
- Health checks fail

### Common root causes
- App not listening on target port
- Wrong health check path
- Target group mismatch
- Security group blocks ALB-to-target traffic
- Backend unhealthy

### Troubleshooting steps
1. Check target health.
2. Confirm the app responds locally.
3. Validate health path and target port.
4. Review security groups.

### Verification commands
```bash
aws elbv2 describe-target-health --target-group-arn <target-group-arn>
curl -I http://localhost:8080/health
ss -tulpn | grep 8080
```

### Solution
- Fix health check path
- Align target group port with app port
- Open SG rule from ALB to backend
- Replace unhealthy targets

---

## 3. ECS Fargate service is not stabilizing

### Symptoms
- ECS deployment stuck
- Tasks keep restarting
- Desired count not met

### Common root causes
- Container crash on startup
- Wrong image tag
- Missing secrets or env vars
- CPU or memory mis-sized
- Health checks failing

### Troubleshooting steps
1. Describe the service.
2. Inspect stopped task reasons.
3. Review logs.
4. Verify task execution role permissions.

### Verification commands
```bash
aws ecs describe-services --cluster <cluster> --services <service>
aws ecs list-tasks --cluster <cluster> --service-name <service>
aws ecs describe-tasks --cluster <cluster> --tasks <task-arn>
aws logs tail /aws/ecs/<service-log-group> --follow
```

### Solution
- Fix image reference
- Correct secrets/config
- Increase memory if needed
- Repair target group health
- Redeploy previous task definition

### Rollback
```bash
aws ecs update-service   --cluster <cluster>   --service <service>   --task-definition <previous-task-def>
```

---

## 4. EKS pods stuck in Pending or CrashLoopBackOff

### Symptoms
- Pods never start or keep restarting
- Release does not become ready
- Partial outage after rollout

### Common root causes
- No node capacity
- Bad image or startup command
- Secret or config error
- Probe failures
- IAM role/service account problem

### Troubleshooting steps
1. Describe the pod.
2. Check node capacity.
3. Review previous logs.
4. Validate secrets and configmaps.
5. Check service and ingress wiring.

### Verification commands
```bash
kubectl get pods -A
kubectl describe pod <pod> -n <namespace>
kubectl logs <pod> -n <namespace> --previous
kubectl top nodes
kubectl get events -A --sort-by=.lastTimestamp
```

### Solution
- Scale node group
- Fix config or secret
- Tune probes
- Roll back deployment
- Correct IAM/service account mapping

### Rollback
```bash
kubectl rollout undo deployment/<deployment> -n <namespace>
kubectl rollout status deployment/<deployment> -n <namespace>
```

---

## 5. RDS connectivity failure

### Symptoms
- Connection timeouts
- Authentication failure
- Sudden DB-related 5xx spike

### Common root causes
- Wrong credentials
- Security group blocks DB path
- Max connections reached
- Wrong endpoint
- Failover or maintenance event

### Troubleshooting steps
1. Confirm DB status.
2. Review SG rules.
3. Test DNS and TCP reachability.
4. Check DB events and connection pressure.

### Verification commands
```bash
aws rds describe-db-instances --db-instance-identifier <db-id>
nslookup <rds-endpoint>
nc -zv <rds-endpoint> 5432
```

### Solution
- Fix credentials
- Open the correct SG path
- Add connection pooling
- Scale DB if under pressure
- Fail over or restore if needed

---

## 6. Lambda function timing out

### Symptoms
- API Gateway times out
- Duration spikes
- Partial request failures

### Common root causes
- Slow downstream service
- VPC or NAT issue
- Cold starts
- Too little memory
- External API latency

### Troubleshooting steps
1. Check logs for duration and timeout.
2. Review errors, throttles, and duration metrics.
3. Verify VPC configuration.
4. Test downstream latency.

### Verification commands
```bash
aws lambda get-function-configuration --function-name <function>
aws logs tail /aws/lambda/<function> --follow
```

### Solution
- Increase timeout only if appropriate
- Increase memory
- Fix NAT/VPC routing
- Decouple slow steps with SQS
- Improve package/startup behavior

### Rollback
```bash
aws lambda update-alias   --function-name <function>   --name live   --function-version <previous-version>
```

---

## 7. S3 access denied in production

### Symptoms
- Uploads/downloads fail with 403
- App cannot read or write objects
- Static assets unavailable

### Common root causes
- Missing IAM S3 permissions
- Bucket policy deny
- Missing KMS permission
- Wrong bucket or prefix
- Cross-account trust issue

### Troubleshooting steps
1. Confirm runtime identity.
2. Inspect IAM and bucket policy.
3. Check KMS involvement.
4. Validate object path.

### Verification commands
```bash
aws sts get-caller-identity
aws s3 ls s3://<bucket-name>
aws s3api get-bucket-policy --bucket <bucket-name>
aws kms list-aliases
```

### Solution
- Add least-privilege S3 permissions
- Fix bucket policy
- Grant KMS access
- Repair cross-account trust

---

## 8. Route 53 or DNS misconfiguration

### Symptoms
- Domain resolves incorrectly
- New environment works internally but not publicly
- Traffic still goes to old stack

### Common root causes
- Wrong alias or CNAME
- High TTL
- Wrong hosted zone
- Record created in wrong account
- Failover routing misconfigured

### Troubleshooting steps
1. Review Route 53 records.
2. Check live DNS resolution.
3. Confirm zone ownership.
4. Validate target endpoint.

### Verification commands
```bash
aws route53 list-hosted-zones
aws route53 list-resource-record-sets --hosted-zone-id <zone-id>
dig www.example.com
nslookup www.example.com
```

### Solution
- Correct alias target
- Lower TTL for future cutovers
- Remove duplicate or stale records
- Use controlled DNS cutover

---

## 9. CloudFront serves stale or broken content

### Symptoms
- Old frontend still visible
- Some regions show different versions
- 403 or 404 on assets

### Common root causes
- Cache not invalidated
- Wrong origin path
- Bucket policy issue
- Incorrect build upload
- Domain or certificate issue

### Troubleshooting steps
1. Confirm files exist in S3.
2. Review CloudFront origin settings.
3. Invalidate cache.
4. Verify object paths and headers.

### Verification commands
```bash
aws s3 ls s3://<bucket-name> --recursive
aws cloudfront list-distributions
curl -I https://www.example.com
```

### Solution
- Re-upload correct build
- Invalidate CloudFront cache
- Fix origin access
- Use versioned assets

### Invalidate cache
```bash
aws cloudfront create-invalidation   --distribution-id <distribution-id>   --paths "/*"
```

---

## 10. CI/CD pipeline deployed bad code to production

### Symptoms
- Incident starts right after deployment
- Metrics degrade after pipeline success
- Deployment succeeds technically but behavior is wrong

### Common root causes
- Missing smoke tests
- Wrong env vars
- No approval gate
- No rollback automation
- Mutable artifacts such as `latest`
- Infra and app changes released together unsafely

### Troubleshooting steps
1. Identify the exact build, commit, image tag, and deployment time.
2. Compare current release to last known-good release.
3. Check deployment logs and verification stage results.
4. Roll back quickly if customer impact is high.

### Verification commands
```bash
git log --oneline -n 5
aws ecs describe-services --cluster <cluster> --services <service>
kubectl rollout history deployment/<deployment> -n <namespace>
aws lambda list-versions-by-function --function-name <function>
```

### Solution
- Roll back to previous version
- Add smoke tests and canary release
- Use immutable artifact versions
- Separate infra deploys from app deploys where practical
- Add prod approval gates

---

## Standard AWS Troubleshooting Workflow

1. Confirm blast radius
2. Check recent changes
3. Check edge layer:
   - Route 53
   - CloudFront
   - ALB/NLB
   - API Gateway
4. Check compute layer:
   - EC2
   - ECS
   - EKS
   - Lambda
5. Check dependencies:
   - RDS
   - ElastiCache
   - S3
   - SQS/SNS/EventBridge
   - external APIs
6. Mitigate first:
   - rollback
   - scale out
   - shift traffic
   - disable broken consumers
7. Verify recovery:
   - health endpoint
   - error rate
   - latency
   - business transactions

---

## High-Value AWS Commands

```bash
aws sts get-caller-identity
aws ec2 describe-instances
aws ecs describe-services --cluster <cluster> --services <service>
aws eks list-clusters
aws lambda get-function-configuration --function-name <function>
aws rds describe-db-instances
aws elbv2 describe-target-health --target-group-arn <tg-arn>
aws route53 list-hosted-zones
aws cloudfront list-distributions
aws logs tail <log-group> --follow
```
