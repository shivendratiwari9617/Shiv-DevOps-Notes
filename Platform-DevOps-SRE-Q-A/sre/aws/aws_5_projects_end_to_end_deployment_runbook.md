# AWS 5 Projects End-to-End Deployment Runbook
## Implementation-Grade Markdown Notes with Prerequisites, Deployment Commands, Rollback Procedures, and Pipeline Step Files

This runbook covers five representative AWS production deployment patterns:

1. **Static website on S3 + CloudFront + Route 53 + ACM**
2. **Containerized web app on ECS Fargate + ALB + ECR + RDS**
3. **Kubernetes microservices platform on EKS + ALB Ingress + ECR + RDS/ElastiCache**
4. **Serverless API on API Gateway + Lambda + DynamoDB + SQS/EventBridge**
5. **Data processing pipeline on S3 + Glue + Athena + Step Functions + SNS**

The goal is implementation clarity: prerequisites, commands, validation, rollback, and pipeline flow.

---

# 1. Global AWS Landing Zone Strategy

## 1.1 Account Model

Use separate AWS accounts for:
- `shared-network`
- `shared-security`
- `shared-logging`
- `shared-cicd`
- `dev`
- `test`
- `stage`
- `prod`

## 1.2 Organizational Structure

```text
AWS Organization
|
+-- Infrastructure OU
|   +-- shared-network
|   +-- shared-security
|   +-- shared-logging
|   +-- shared-cicd
|
+-- NonProd OU
|   +-- dev
|   +-- test
|   +-- stage
|
+-- Prod OU
    +-- prod
```

## 1.3 Baseline Controls

- centralized CloudTrail
- centralized Config rules
- GuardDuty enabled
- Security Hub enabled
- IAM Identity Center / SSO
- SCP guardrails
- central KMS ownership where required
- VPC flow logs
- log archive lifecycle retention
- mandatory tagging policy

---

# 2. Naming, Tagging, and Environment Standards

## 2.1 Naming Convention

```text
<org>-<env>-<domain>-<service>-<region>-<suffix>
```

Examples:

```text
acme-prod-web-cloudfront
acme-stage-api-orders-lambda
acme-prod-app-ecs-cluster
acme-prod-data-glue-job
acme-prod-core-eks-us-east-1
```

## 2.2 Required Tags

Apply to all supported resources:

- `Environment`
- `Application`
- `Owner`
- `CostCenter`
- `Criticality`
- `DataClassification`
- `ManagedBy`
- `Region`

## 2.3 Environment Variables Convention

```text
APP_NAME
APP_ENV
AWS_REGION
ACCOUNT_ID
IMAGE_TAG
DB_HOST
DB_PORT
DB_NAME
SECRET_ARN
QUEUE_URL
TOPIC_ARN
TABLE_NAME
```

---

# 3. Global Prerequisites

## 3.1 Workstation Tools

Install:
- AWS CLI v2
- Docker
- Git
- jq
- kubectl
- eksctl
- helm
- Terraform
- OpenSSL

## 3.2 AWS CLI Setup

```bash
aws configure sso
aws sso login --profile prod-admin
aws sts get-caller-identity --profile prod-admin
aws configure list --profile prod-admin
```

## 3.3 Export Useful Variables

```bash
export AWS_PROFILE=prod-admin
export AWS_REGION=us-east-1
export ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
export APP_NAME=acmeapp
export ENV=prod
```

## 3.4 ECR Login

```bash
aws ecr get-login-password --region $AWS_REGION | \
docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
```

## 3.5 ACM Notes

- ACM certificates for **CloudFront** must be created in `us-east-1`
- ACM certificates for **regional ALB/API Gateway** should be created in the same region as the service

## 3.6 IAM Assumptions

Use least privilege. Separate roles for:
- platform admin
- network admin
- security admin
- deployer
- developer
- runtime roles

Do not use broad admin privileges for CI/CD.

## 3.7 Terraform Option

All deployments may also be managed with Terraform. Recommended module groups:
- networking
- IAM
- ACM/Route 53
- ECR
- ECS
- EKS
- Lambda/API Gateway
- DynamoDB/SQS/EventBridge
- Glue/Athena/Step Functions
- observability
- security guardrails

---

# 4. Shared Networking Reference

## 4.1 VPC Layout

Use:
- 2 public subnets minimum
- 2 private app subnets minimum
- 2 private data subnets minimum

```text
VPC 10.0.0.0/16
|
+-- Public-A   10.0.0.0/24
+-- Public-B   10.0.1.0/24
+-- Private-App-A  10.0.10.0/24
+-- Private-App-B  10.0.11.0/24
+-- Private-Data-A 10.0.20.0/24
+-- Private-Data-B 10.0.21.0/24
```

## 4.2 Network Rules

- ALB in public subnets
- ECS/EKS app tasks/pods in private app subnets
- RDS/ElastiCache in private data subnets
- NAT Gateway for private outbound access
- security groups strictly scoped
- no direct public DB exposure

## 4.3 Security Group Model

- `sg-alb-public`
- `sg-ecs-app`
- `sg-eks-nodes`
- `sg-rds-db`
- `sg-redis-cache`
- `sg-lambda-vpc`

---

# 5. Standard CI/CD Pipeline Stages

All projects should follow:

1. source checkout
2. dependency install
3. static checks
4. unit/integration tests
5. package/build
6. deploy
7. verify
8. rollback trigger if verification fails

## 5.1 Generic GitHub Actions Pipeline Step File

```yaml
name: deploy

on:
  workflow_dispatch:
  push:
    branches: [ main ]

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/gh-deployer
          aws-region: us-east-1

      - name: Install dependencies
        run: echo "install project dependencies"

      - name: Test
        run: echo "run tests"

      - name: Package
        run: echo "build/package application"

      - name: Deploy
        run: echo "deploy to aws"

      - name: Verify
        run: echo "run smoke checks"

      - name: Rollback trigger
        if: failure()
        run: echo "invoke rollback procedure"
```

---

# 6. Project 1: Static Website on S3 + CloudFront + Route 53 + ACM

## 6.1 Architecture Summary

```text
User
 |
 v
Route 53
 |
 v
CloudFront
 |
 v
S3 static website bucket
```

## 6.2 Prerequisites

- domain hosted in Route 53 or delegated there
- ACM certificate requested in `us-east-1`
- static site build output available
- S3 bucket naming approved
- CloudFront OAC/OAI strategy decided

## 6.3 Folder Structure

```text
project1-static-site/
  site/
    dist/
  infra/
    cloudfront/
    route53/
  .github/workflows/deploy.yml
```

## 6.4 Deployment Commands

### Create S3 bucket
```bash
aws s3api create-bucket \
  --bucket acme-prod-web-static \
  --region us-east-1
```

### Enable bucket versioning
```bash
aws s3api put-bucket-versioning \
  --bucket acme-prod-web-static \
  --versioning-configuration Status=Enabled
```

### Sync site content
```bash
aws s3 sync ./site/dist s3://acme-prod-web-static --delete
```

### Request ACM certificate
```bash
aws acm request-certificate \
  --domain-name www.example.com \
  --subject-alternative-names example.com \
  --validation-method DNS \
  --region us-east-1
```

### Create CloudFront distribution
```bash
aws cloudfront create-distribution --distribution-config file://infra/cloudfront/distribution.json
```

### Create Route 53 record
```bash
aws route53 change-resource-record-sets \
  --hosted-zone-id <HOSTED_ZONE_ID> \
  --change-batch file://infra/route53/alias-record.json
```

## 6.5 Validation Checks

```bash
aws s3 ls s3://acme-prod-web-static
aws cloudfront list-distributions
aws route53 list-resource-record-sets --hosted-zone-id <HOSTED_ZONE_ID>
curl -I https://www.example.com
```

## 6.6 Rollback Procedure

### Fast rollback by restoring previous site build
```bash
aws s3 sync ./rollback-build/dist s3://acme-prod-web-static --delete
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"
```

### Full rollback
- repoint Route 53 alias to previous distribution if blue/green edge exists
- restore prior S3 object versions if required
- invalidate cache

## 6.7 Common Failure Points

- ACM certificate in wrong region
- bucket policy blocking CloudFront access
- stale cache
- Route 53 alias target wrong
- SPA routing not handled correctly

## 6.8 Observability / Security / Cost

- enable CloudFront access logs
- block public S3 access where using controlled origin access
- set lifecycle policy for logs
- use compression and caching headers to reduce cost

## 6.9 Pipeline Step File

```yaml
name: static-site-deploy
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/gh-static-site-deployer
          aws-region: us-east-1
      - name: Build
        run: |
          npm ci
          npm run build
      - name: Deploy to S3
        run: aws s3 sync ./dist s3://acme-prod-web-static --delete
      - name: Invalidate CloudFront
        run: aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"
      - name: Verify
        run: curl -I https://www.example.com
      - name: Rollback
        if: failure()
        run: aws s3 sync ./rollback-build/dist s3://acme-prod-web-static --delete
```

---

# 7. Project 2: Containerized Web App on ECS Fargate + ALB + ECR + RDS

## 7.1 Architecture Summary

```text
User
 |
 v
ALB
 |
 v
ECS Fargate Service
 |
 +--> ECR image
 |
 +--> RDS PostgreSQL/MySQL
```

## 7.2 Prerequisites

- VPC with public/private/data subnets
- ALB security group
- ECS cluster
- ECR repository
- RDS instance created
- Secrets Manager entries for DB credentials
- task execution role and task role configured

## 7.3 Folder Structure

```text
project2-ecs-app/
  app/
  ecs/
    taskdef.json
    service.json
  alb/
  db/
  .github/workflows/deploy.yml
```

## 7.4 Deployment Commands

### Create ECR repository
```bash
aws ecr create-repository --repository-name acme-prod-webapp
```

### Build and push image
```bash
docker build -t acme-prod-webapp:latest .
docker tag acme-prod-webapp:latest $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/acme-prod-webapp:latest
docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/acme-prod-webapp:latest
```

### Register task definition
```bash
aws ecs register-task-definition --cli-input-json file://ecs/taskdef.json
```

### Create cluster
```bash
aws ecs create-cluster --cluster-name acme-prod-webapp-cluster
```

### Create service
```bash
aws ecs create-service --cli-input-json file://ecs/service.json
```

## 7.5 Validation Checks

```bash
aws ecs describe-services --cluster acme-prod-webapp-cluster --services acme-prod-webapp-service
aws ecs list-tasks --cluster acme-prod-webapp-cluster
aws logs describe-log-groups
curl -I http://<ALB_DNS_NAME>/health
```

## 7.6 Rollback Procedure

### Roll back to previous task definition
```bash
aws ecs update-service \
  --cluster acme-prod-webapp-cluster \
  --service acme-prod-webapp-service \
  --task-definition acme-prod-webapp:<PREVIOUS_REVISION>
```

### Force stable deployment
```bash
aws ecs wait services-stable \
  --cluster acme-prod-webapp-cluster \
  --services acme-prod-webapp-service
```

## 7.7 Common Failure Points

- task cannot pull image from ECR
- app container not listening on expected port
- target group health checks failing
- DB security group not allowing ECS access
- secret ARN missing in task definition

## 7.8 Blue/Green Guidance

Use CodeDeploy for ECS blue/green when downtime risk is low tolerance:
- deploy new task set
- shift 10%
- verify health
- shift 100%
- auto-rollback on alarm breach

## 7.9 Observability / Security / Cost

- CloudWatch logs for app and ALB
- RDS enhanced monitoring
- ECS Service Auto Scaling
- least privilege task roles
- right-size CPU/memory to avoid overpaying

## 7.10 Pipeline Step File

```yaml
name: ecs-fargate-deploy
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/gh-ecs-deployer
          aws-region: us-east-1
      - name: Build
        run: docker build -t acme-prod-webapp:${GITHUB_SHA} .
      - name: Login ECR
        run: aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
      - name: Push
        run: |
          docker tag acme-prod-webapp:${GITHUB_SHA} <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/acme-prod-webapp:${GITHUB_SHA}
          docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/acme-prod-webapp:${GITHUB_SHA}
      - name: Render task definition
        run: echo "update taskdef image"
      - name: Deploy ECS service
        run: aws ecs update-service --cluster acme-prod-webapp-cluster --service acme-prod-webapp-service --force-new-deployment
      - name: Verify
        run: curl -f http://<ALB_DNS_NAME>/health
      - name: Rollback
        if: failure()
        run: aws ecs update-service --cluster acme-prod-webapp-cluster --service acme-prod-webapp-service --task-definition acme-prod-webapp:<PREVIOUS_REVISION>
```

---

# 8. Project 3: Kubernetes Microservices Platform on EKS

## 8.1 Architecture Summary

```text
User
 |
 v
ALB Ingress
 |
 v
EKS
 |--- service-a
 |--- service-b
 |--- service-c
 |
 +--> RDS
 +--> ElastiCache Redis
 +--> ECR
```

## 8.2 Prerequisites

- VPC and subnets tagged for EKS
- IAM OIDC provider enabled
- EKS cluster role and node role
- kubectl access
- aws-load-balancer-controller IAM role
- ECR repos
- RDS and Redis provisioned
- namespace and secret strategy defined

## 8.3 Folder Structure

```text
project3-eks-platform/
  manifests/
    namespaces.yaml
    deployments/
    services/
    ingress/
    hpa/
  helm/
  eks/
  .github/workflows/deploy.yml
```

## 8.4 Deployment Commands

### Create EKS cluster
```bash
eksctl create cluster \
  --name acme-prod-eks \
  --region us-east-1 \
  --nodes 3 \
  --node-type t3.large \
  --managed
```

### Associate IAM OIDC provider
```bash
eksctl utils associate-iam-oidc-provider \
  --cluster acme-prod-eks \
  --approve
```

### Update kubeconfig
```bash
aws eks update-kubeconfig --name acme-prod-eks --region us-east-1
```

### Deploy namespaces
```bash
kubectl apply -f manifests/namespaces.yaml
```

### Deploy services
```bash
kubectl apply -f manifests/deployments/
kubectl apply -f manifests/services/
kubectl apply -f manifests/ingress/
kubectl apply -f manifests/hpa/
```

## 8.5 Validation Checks

```bash
kubectl get nodes
kubectl get pods -A
kubectl get svc -A
kubectl get ingress -A
kubectl rollout status deployment/service-a -n prod
curl -I http://<ALB_DNS_NAME>/health
```

## 8.6 Rollback Procedure

### Undo bad deployment
```bash
kubectl rollout undo deployment/service-a -n prod
kubectl rollout status deployment/service-a -n prod
```

### Helm rollback if Helm is used
```bash
helm history service-a -n prod
helm rollback service-a 1 -n prod
```

## 8.7 Common Failure Points

- worker nodes not joining cluster
- IRSA misconfigured
- ALB ingress controller missing permissions
- readiness/liveness failures
- wrong security group path to RDS/Redis
- insufficient node capacity

## 8.8 Canary / Progressive Delivery

For critical services:
- deploy canary version
- route small traffic slice
- verify latency, error rate, business KPI
- promote gradually

## 8.9 Observability / Security / Cost

- CloudWatch Container Insights
- Prometheus/Grafana optional
- IRSA instead of node-wide IAM
- network policies where possible
- cluster autoscaler / Karpenter
- control node sizing and idle node sprawl

## 8.10 Pipeline Step File

```yaml
name: eks-deploy
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/gh-eks-deployer
          aws-region: us-east-1
      - name: Build image
        run: docker build -t service-a:${GITHUB_SHA} .
      - name: Push image
        run: echo "push to ecr"
      - name: Update kubeconfig
        run: aws eks update-kubeconfig --name acme-prod-eks --region us-east-1
      - name: Deploy manifests
        run: kubectl apply -f manifests/
      - name: Verify rollout
        run: kubectl rollout status deployment/service-a -n prod
      - name: Smoke test
        run: curl -f http://<ALB_DNS_NAME>/health
      - name: Rollback
        if: failure()
        run: kubectl rollout undo deployment/service-a -n prod
```

---

# 9. Project 4: Serverless API on API Gateway + Lambda + DynamoDB + SQS/EventBridge

## 9.1 Architecture Summary

```text
Client
 |
 v
API Gateway
 |
 v
Lambda
 | \
 |  +--> DynamoDB
 |  +--> SQS
 |  +--> EventBridge
```

## 9.2 Prerequisites

- Lambda execution role
- DynamoDB table created
- SQS queue created
- EventBridge bus/rules planned
- API Gateway custom domain strategy decided
- CloudWatch logging enabled

## 9.3 Folder Structure

```text
project4-serverless-api/
  src/
  template/
  events/
  scripts/
  .github/workflows/deploy.yml
```

## 9.4 Deployment Commands

### Create DynamoDB table
```bash
aws dynamodb create-table \
  --table-name acme-prod-orders \
  --attribute-definitions AttributeName=pk,AttributeType=S AttributeName=sk,AttributeType=S \
  --key-schema AttributeName=pk,KeyType=HASH AttributeName=sk,KeyType=RANGE \
  --billing-mode PAY_PER_REQUEST
```

### Create SQS queue
```bash
aws sqs create-queue --queue-name acme-prod-orders-queue
```

### Package Lambda
```bash
zip -r function.zip src/
```

### Create Lambda function
```bash
aws lambda create-function \
  --function-name acme-prod-orders-api \
  --runtime python3.11 \
  --handler app.handler \
  --zip-file fileb://function.zip \
  --role arn:aws:iam::<ACCOUNT_ID>:role/lambda-orders-runtime
```

### Create API Gateway HTTP API
```bash
aws apigatewayv2 create-api \
  --name acme-prod-orders-api \
  --protocol-type HTTP
```

## 9.5 Validation Checks

```bash
aws lambda get-function --function-name acme-prod-orders-api
aws dynamodb describe-table --table-name acme-prod-orders
aws sqs get-queue-attributes --queue-url <QUEUE_URL> --attribute-names All
curl -X GET https://<API_ID>.execute-api.us-east-1.amazonaws.com/orders/health
```

## 9.6 Rollback Procedure

### Roll back Lambda to previous version
```bash
aws lambda list-versions-by-function --function-name acme-prod-orders-api
aws lambda update-alias \
  --function-name acme-prod-orders-api \
  --name live \
  --function-version <PREVIOUS_VERSION>
```

### Disable bad event routing
```bash
aws events disable-rule --name <RULE_NAME>
```

## 9.7 Common Failure Points

- Lambda package too large
- wrong handler name
- IAM denies DynamoDB/SQS access
- API Gateway integration misconfigured
- timeout too low for downstream calls
- missing DLQ strategy

## 9.8 Canary / Versioning Guidance

- publish Lambda versions
- route via alias
- shift 10%
- verify errors and latency
- shift 100% on success

## 9.9 Observability / Security / Cost

- structured CloudWatch logs
- X-Ray tracing
- least privilege Lambda role
- reserved concurrency for critical functions
- DynamoDB on-demand or provisioned with autoscaling based on workload

## 9.10 Pipeline Step File

```yaml
name: lambda-api-deploy
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/gh-lambda-deployer
          aws-region: us-east-1
      - name: Test
        run: pytest
      - name: Package
        run: zip -r function.zip src/
      - name: Deploy code
        run: aws lambda update-function-code --function-name acme-prod-orders-api --zip-file fileb://function.zip
      - name: Publish version
        run: aws lambda publish-version --function-name acme-prod-orders-api
      - name: Verify
        run: curl -f https://<API_ID>.execute-api.us-east-1.amazonaws.com/orders/health
      - name: Rollback
        if: failure()
        run: aws lambda update-alias --function-name acme-prod-orders-api --name live --function-version <PREVIOUS_VERSION>
```

---

# 10. Project 5: Data Processing Pipeline on S3 + Glue + Athena + Step Functions + SNS

## 10.1 Architecture Summary

```text
Raw S3
 |
 v
Glue Crawler / Glue ETL
 |
 v
Processed S3
 |
 v
Athena
 |
 v
Step Functions orchestration
 |
 v
SNS notifications
```

## 10.2 Prerequisites

- raw and processed S3 buckets
- Glue IAM role
- Glue database
- Athena output bucket
- Step Functions role
- SNS topic
- schema and partition strategy defined

## 10.3 Folder Structure

```text
project5-data-pipeline/
  glue/
    scripts/
  stepfunctions/
    state-machine.json
  athena/
  s3/
  .github/workflows/deploy.yml
```

## 10.4 Deployment Commands

### Create raw and processed buckets
```bash
aws s3 mb s3://acme-prod-raw-data
aws s3 mb s3://acme-prod-processed-data
```

### Create SNS topic
```bash
aws sns create-topic --name acme-prod-data-pipeline-alerts
```

### Create Glue database
```bash
aws glue create-database \
  --database-input '{"Name":"acme_prod_data"}'
```

### Upload Glue script
```bash
aws s3 cp glue/scripts/transform.py s3://acme-prod-raw-data/scripts/transform.py
```

### Create Step Functions state machine
```bash
aws stepfunctions create-state-machine \
  --name acme-prod-data-pipeline \
  --definition file://stepfunctions/state-machine.json \
  --role-arn arn:aws:iam::<ACCOUNT_ID>:role/stepfunctions-data-pipeline
```

## 10.5 Validation Checks

```bash
aws glue get-databases
aws stepfunctions list-state-machines
aws athena list-work-groups
aws sns list-topics
aws s3 ls s3://acme-prod-raw-data
```

## 10.6 Rollback Procedure

- disable new scheduled executions
- revert Glue script to previous version
- restore prior state machine definition
- quarantine bad raw data prefix
- rerun last good pipeline on clean data window

### Example Disable Trigger
```bash
aws glue stop-trigger --name <TRIGGER_NAME>
```

### Update State Machine to Prior Definition
```bash
aws stepfunctions update-state-machine \
  --state-machine-arn <STATE_MACHINE_ARN> \
  --definition file://stepfunctions/previous-state-machine.json
```

## 10.7 Common Failure Points

- crawler schema drift
- partition mismatch
- missing IAM permissions for Glue job
- Athena querying wrong location
- malformed files in raw bucket
- state machine timeout or branch error

## 10.8 Observability / Security / Cost

- SNS alerts on state machine failure
- CloudWatch logs for Glue and Step Functions
- partitioned S3 layouts to reduce Athena cost
- lifecycle policies for raw/processed data
- encrypt S3 and Glue metadata where required

## 10.9 Pipeline Step File

```yaml
name: data-pipeline-deploy
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/gh-data-deployer
          aws-region: us-east-1
      - name: Validate JSON
        run: jq . stepfunctions/state-machine.json > /dev/null
      - name: Upload Glue script
        run: aws s3 cp glue/scripts/transform.py s3://acme-prod-raw-data/scripts/transform.py
      - name: Update Step Functions
        run: aws stepfunctions update-state-machine --state-machine-arn <STATE_MACHINE_ARN> --definition file://stepfunctions/state-machine.json
      - name: Verify
        run: aws stepfunctions list-executions --state-machine-arn <STATE_MACHINE_ARN>
      - name: Rollback
        if: failure()
        run: aws stepfunctions update-state-machine --state-machine-arn <STATE_MACHINE_ARN> --definition file://stepfunctions/previous-state-machine.json
```

---

# 11. Common Deployment Troubleshooting Notes

## 11.1 AWS Identity Problems

```bash
aws sts get-caller-identity
aws iam list-attached-role-policies --role-name <ROLE_NAME>
```

Check:
- wrong profile
- expired SSO session
- missing assume-role trust
- missing permissions boundary allowance

## 11.2 ECS / EKS Image Pull Problems

Check:
- ECR login
- repo policy
- image tag exists
- task execution role / node role permissions

```bash
aws ecr describe-images --repository-name acme-prod-webapp
kubectl describe pod <POD_NAME> -n <NAMESPACE>
```

## 11.3 ALB Health Check Problems

Check:
- app port
- health path
- SG path from ALB to targets
- target group registration

```bash
aws elbv2 describe-target-health --target-group-arn <TARGET_GROUP_ARN>
```

## 11.4 Database Connectivity Problems

Check:
- subnet group
- SG rules
- DNS
- secret values
- app connection pool

```bash
aws rds describe-db-instances
```

---

# 12. Production Readiness Checklist

- accounts separated by environment
- least privilege IAM reviewed
- tagging enforced
- logging enabled
- metrics and alarms created
- backups enabled
- rollback tested
- blue/green or canary defined where required
- secrets externalized
- certificates valid
- DNS ownership clear
- cost dashboards configured
- runbooks stored with repo
- smoke tests automated
- post-deploy verification documented

---

# 13. Common Anti-Patterns

- deploying prod from personal credentials
- using one AWS account for everything
- exposing databases publicly
- using latest image tags without digest control
- no rollback command documented
- no health endpoints
- no DLQ for async workloads
- no lifecycle management for logs and artifacts
- no partitioning strategy for Athena data
- overusing EKS where ECS or Lambda would be simpler

---

# 14. Final Senior Architect Notes

1. **Choose the runtime by workload shape, not by team habit.**
2. **Keep stateful services private and identity-driven.**
3. **Every deployment should have a verification step and a rollback command before go-live.**
4. **Blue/green or canary is worth the overhead for revenue-critical paths.**
5. **Tagging, logging, and IAM discipline matter as much as the deployment command itself.**
6. **The best runbook is the one that works at 2:00 AM during an outage.**
