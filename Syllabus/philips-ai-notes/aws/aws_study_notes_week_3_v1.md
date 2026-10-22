# AWS Study Notes — Week 3
## Compute, Kubernetes, CI/CD, and Platform Delivery

> This is the **deep technical Week 3 guide**.  
> It is written as a **service-by-service study README** with:
> - technical explanations
> - key entities
> - architecture usage
> - controls
> - examples
> - common mistakes
> - best practices
> - troubleshooting cues
> - official AWS references
>
> The goal is not just to know what each service is, but to understand **how AWS compute and platform delivery services are selected, operated, secured, and connected into delivery workflows**.

---

# 1. Why Week 3 Matters

Week 3 is where AWS starts to feel like a real platform.

If Week 1 created:
- governance
- account structure
- identity
- control boundaries

and Week 2 created:
- VPC design
- private connectivity
- edge entry
- network control paths

then Week 3 creates the **runtime and delivery layer**:
- where applications actually execute
- how containers are built and deployed
- how Kubernetes is run at scale
- how images and packages are promoted
- how CI/CD pipelines are structured
- how infrastructure becomes reproducible
- how application teams consume platform services safely

The central Week 3 idea is:

> **AWS compute is not one service. It is a portfolio of execution models, each with different control, operational burden, scaling behavior, deployment mechanics, and security implications.**

A strong Week 3 understanding lets you answer questions like:

- When should I use EC2 vs ECS vs EKS vs Lambda?
- What is the difference between self-managed nodes, managed node groups, and Fargate?
- How should container images be stored, promoted, and scanned?
- How does CI/CD work across build, approval, deploy, and rollback stages?
- How do CloudFormation, SAM, CDK, and Proton fit together?
- How does a platform team deliver reusable infrastructure and services?
- How do SageMaker and Bedrock fit into modern application platforms?

---

# 2. Learning Outcomes

By the end of this week, you should be able to:

- explain the technical role of **EC2** and **EC2 Auto Scaling**
- explain **AMIs**, **launch templates**, **Auto Scaling groups**, **instance refresh**, and **lifecycle hooks**
- explain **ECR** repositories, image metadata, scanning, immutability, and replication
- explain **ECS** clusters, services, task definitions, tasks, capacity providers, and Fargate
- explain **EKS** clusters, managed node groups, Fargate profiles, add-ons, access entries, Pod Identity, IRSA, and Karpenter
- explain **Lambda** versions, aliases, event source mappings, layers, function URLs, and concurrency controls
- explain **App Runner**, **Elastic Beanstalk**, and **Lightsail** and where they fit
- explain **AWS Batch** for queued compute workloads
- explain **CodeBuild**, **CodePipeline**, **CodeDeploy**, and **CodeArtifact**
- explain **CloudFormation**, **SAM**, **CDK**, and **Proton**
- explain major **SageMaker** and **Bedrock** entities relevant to platform delivery
- connect compute, images, pipelines, infrastructure-as-code, and deployment safety into one architecture model

---

# 3. Week 3 Architectural Mental Model

Think about Week 3 in six layers:

1. **Execution layer**  
   EC2, ECS, EKS, Lambda, App Runner, Elastic Beanstalk, Lightsail, Batch

2. **Artifact layer**  
   ECR, CodeArtifact, AMIs, build outputs, packages, templates

3. **Delivery layer**  
   CodeBuild, CodePipeline, CodeDeploy

4. **Infrastructure definition layer**  
   CloudFormation, SAM, CDK, Proton

5. **Platform abstraction layer**  
   ECS service templates, EKS platform standards, Proton service templates, reusable golden AMIs

6. **AI/ML platform extension layer**  
   SageMaker, Bedrock

When application delivery breaks, at least one of these layers is usually the real source of the issue.

---

# 4. Amazon EC2 and EC2 Auto Scaling

Amazon EC2 is the foundational virtual machine service in AWS. It gives the most control, but also the highest operational ownership among the common compute options.

## 4.1 Amazon EC2

### Technical purpose
EC2 provides virtual machines with configurable compute, memory, storage, networking, and operating system control.

### Core entities

#### Instance
A running virtual server.

#### AMI
An Amazon Machine Image used as the template for launching instances.

#### Instance type
Defines the CPU, memory, networking, accelerator, and storage profile.

#### Key pair
Used for SSH access in classic Linux administration patterns.

#### ENI
Network interface attached to the instance.

#### Instance profile
The IAM role container attached to an EC2 instance for AWS API access.

#### User data
Bootstrap script or cloud-init data executed during launch.

#### Elastic IP
Static public IPv4 address associated with eligible resources.

### Example
A legacy Java application runs on EC2 with:
- a hardened AMI
- launch template
- ALB target registration
- Auto Scaling group
- Systems Manager access instead of direct SSH

### Best practices
- use IAM instance profiles instead of static credentials
- prefer Systems Manager over broad SSH/RDP exposure
- standardize instance bootstrap via user data or image pipelines
- use launch templates rather than older launch configurations
- pick instance families intentionally: general-purpose, compute-optimized, memory-optimized, storage-optimized, accelerators

### Common mistakes
- manually configuring instances after launch instead of making changes reproducible
- using old hand-built AMIs with unclear ownership
- leaving IMDS settings weak
- placing long-lived pets in an architecture that should be immutable

---

## 4.2 AMIs

An AMI is the launch template image for EC2 instances.

### Technical role
It packages:
- operating system
- installed software
- startup state and configuration baseline

### Design significance
An AMI is often the boundary between:
- mutable infrastructure
- immutable, versioned infrastructure

### Example
A platform team publishes a monthly golden AMI with:
- CIS hardening
- CloudWatch agent
- SSM agent
- monitoring baseline
- approved runtime packages

### Best practices
- version AMIs clearly
- automate image creation
- use AMI promotion workflow across environments
- deprecate old images deliberately

### Common mistakes
- manual image creation with no reproducibility
- too many custom one-off images
- not testing image updates before rollout

---

## 4.3 Instance types

Instance types define resource profile.

### Common families
- general purpose
- compute optimized
- memory optimized
- storage optimized
- accelerated computing

### Selection logic
Choose based on workload:
- API service → general purpose or compute optimized
- in-memory analytics → memory optimized
- GPU inference/training → accelerated

### Common mistakes
- choosing oversized instances and paying for idle capacity
- selecting based only on vCPU count without understanding memory/network/storage behavior

---

## 4.4 Key pairs

Key pairs provide SSH key-based access for EC2 login patterns.

### Best practice
Use them sparingly in mature platforms. Prefer:
- Systems Manager Session Manager
- centralized access patterns
- no direct inbound admin exposure where possible

### Common mistake
Treating SSH key pairs as the long-term access model for enterprise operations.

---

## 4.5 Instance profiles

Instance profiles attach IAM roles to EC2 instances.

### Technical role
They provide temporary AWS credentials to workloads running on the instance.

### Example
An EC2-hosted app reads secrets from Secrets Manager using an instance role, not static access keys.

### Best practices
- one clear workload role per purpose
- least privilege
- no embedded credentials in instance images

### Common mistakes
- using administrator-style instance roles
- reusing one broad role across many unrelated workloads

---

## 4.6 User data

User data is launch-time bootstrap logic.

### Common uses
- install packages
- fetch config
- register agents
- join clusters
- mount filesystems

### Best practices
- keep it deterministic and idempotent
- avoid putting secrets directly in user data
- use it for bootstrap, not as a substitute for full config management at arbitrary scale

### Common mistakes
- massive fragile scripts with no retry logic
- hidden drift between launched instances due to external bootstrap dependencies

---

# 5. EC2 Auto Scaling

EC2 Auto Scaling provides controlled scaling, replacement, and rolling update behavior for fleets of EC2 instances.

## 5.1 Auto Scaling group (ASG)

### Technical role
An ASG maintains desired capacity of instances and can:
- launch
- terminate
- replace unhealthy instances
- scale based on policies
- support rolling refresh/update behavior

### Core entities

#### Launch template
The modern launch specification for ASGs.

#### Desired capacity
Target number of running instances.

#### Minimum capacity
Lower floor.

#### Maximum capacity
Upper ceiling.

#### Scaling policy
Rule for scale-out/scale-in behavior.

#### Scheduled action
Time-based capacity adjustment.

#### Lifecycle hook
Pause point during launch or termination workflows.

#### Warm pool
Pre-initialized instances to reduce scale-out latency.

#### Instance refresh
Controlled rolling replacement of ASG instances.

#### Health checks
Can be EC2-based or load-balancer-aware.

### Example
A web tier ASG:
- min 4
- desired 6
- max 20
- target tracking on CPU or request count
- ALB health checks
- instance refresh on AMI update

### Best practices
- use launch templates
- tie health checks to real application health via load balancer when appropriate
- use instance refresh for controlled replacement after AMI changes
- use lifecycle hooks for registration/drain workflows
- separate scaling signals for app tiers vs batch or background workers

### Common mistakes
- scaling on poor metrics
- forgetting connection draining behavior at deploy time
- replacing instances manually instead of using ASG mechanisms
- using one ASG for workloads with mixed scaling characteristics

---

## 5.2 Scaling policies

### Common types
- target tracking
- step scaling
- simple scaling
- scheduled scaling
- predictive scaling in relevant patterns

### Design example
- web tier scales on ALB request count per target
- worker tier scales on queue depth
- scheduled action pre-scales before business peak

### Common mistake
Using CPU alone for every workload, even when queue depth or request latency is the real signal.

---

## 5.3 Lifecycle hooks

Lifecycle hooks pause instance launch or terminate actions.

### Uses
- register in CMDB
- preload caches
- run compliance checks
- drain connections on termination
- export logs before shutdown

### Common mistake
Using lifecycle hooks without handling timeout and failure behavior.

---

## 5.4 Warm pools

Warm pools keep instances partially prepared for faster scaling.

### Best fit
Workloads with:
- heavy bootstrap time
- slow application startup
- large runtime preload needs

### Common mistake
Paying for warm capacity without verifying that startup time was actually the bottleneck.

---

## 5.5 Instance refresh

Instance refresh rolls instances in an ASG to a new template or AMI baseline.

### Best practice
Use it after:
- patching AMIs
- changing launch template version
- changing bootstrap logic

### Common mistake
Updating the launch template but forgetting that existing instances remain unchanged until explicitly refreshed or replaced.

---

# 6. EC2 Image Builder

EC2 Image Builder automates image creation pipelines.

## Technical purpose
Build, test, version, and distribute AMIs in a repeatable way.

## Core entities
- image pipeline
- recipe
- component
- infrastructure configuration
- distribution configuration

## Example
A monthly image pipeline:
1. base OS image
2. install agents
3. apply hardening
4. run validation tests
5. distribute AMI to multiple accounts/Regions

## Best practices
- move from manual AMIs to pipeline-built images
- embed tests in the image lifecycle
- distribute approved images centrally

## Common mistakes
- building images without validation steps
- no promotion logic from dev to prod-ready images

---

# 7. Amazon ECR

Amazon Elastic Container Registry is the managed container image registry for AWS.

## 7.1 Technical purpose
Store, version, scan, replicate, and serve container images.

## 7.2 Core entities

### Private repository
Registry space for internal images.

### Public repository
Registry space for publicly shared images.

### Image
Container artifact stored in the repository.

### Tag
Human-readable label for an image version.

### Manifest
Metadata describing the image and its layers.

### Lifecycle policy
Rules for cleanup of old images.

### Pull through cache rule
Allows ECR to cache images from external registries.

### Replication configuration
Cross-Region or cross-account replication setup.

## 7.3 Example
A CI pipeline builds:
- `payments-api:1.7.3`
- `payments-api:prod`
and pushes both to ECR with vulnerability scanning enabled.

## 7.4 Best practices
- prefer immutable tags for release images
- enable scanning and review findings
- replicate to regions/accounts where needed
- use lifecycle policies to clean stale artifacts
- avoid “latest” as the only deployment selector

## 7.5 Common mistakes
- mutable release tags with unclear provenance
- no retention policy leading to registry sprawl
- not separating dev/test/prod image promotion semantics

---

## 7.6 ECR controls

Important controls include:
- repository policies
- encryption
- image scanning
- tag immutability
- lifecycle rules
- replication settings

### Common design
- developers can push to dev repositories
- only CI/CD roles can promote to prod repositories
- immutable prod tags
- vulnerability findings reviewed before approval

---

# 8. Amazon ECS and AWS Fargate

Amazon ECS is AWS’s managed container orchestration service. It is often the simplest way to run containers on AWS while staying more AWS-native than Kubernetes.

## 8.1 ECS core entities

### Cluster
Logical grouping of ECS capacity and services.

### Task definition
Versioned specification for containers, CPU, memory, roles, logging, and runtime settings.

### Task
A running instantiation of a task definition.

### Service
Long-running controller that keeps a desired number of tasks running.

### Capacity provider
Mechanism for deciding how tasks are placed onto capacity, such as EC2 or Fargate.

### Container instance
An ECS-registered EC2 instance in EC2-backed ECS mode.

### Service deployment
Rollout state of new task definition versions.

### Task set
Used in advanced deployment patterns such as blue/green.

## 8.2 ECS execution models

### ECS on EC2
You manage the EC2 fleet that runs the containers.

### ECS on Fargate
AWS manages the compute runtime; you focus on tasks and services.

### Selection logic
Choose ECS on EC2 when:
- you need daemon workloads
- you want custom instance-level control
- you need special hardware or capacity economics

Choose ECS on Fargate when:
- you want lower operational overhead
- task isolation matters
- you prefer per-task compute abstraction

## 8.3 Task definitions

Task definitions include:
- container images
- CPU/memory
- environment variables
- secrets
- logging config
- execution role
- task role
- networking mode
- ephemeral storage and related runtime settings as applicable

### Execution role vs task role
- **execution role**: lets ECS pull images, write logs, and handle platform operations
- **task role**: permissions available to the application code inside the task

### Common mistake
Putting app permissions in the execution role instead of the task role.

---

## 8.4 ECS services

An ECS service ensures the right number of tasks are running and can integrate with:
- ALB/NLB
- service discovery
- rolling deployments
- blue/green deployments

### Example
A payments service runs:
- 6 tasks
- on Fargate
- behind an internal ALB
- with rolling deployment
- using service discovery for internal DNS

### Best practices
- use separate task roles per workload
- define health checks properly
- use deployment configuration intentionally
- keep task definitions versioned and immutable

### Common mistakes
- poor health check settings causing flapping deployments
- under-sized tasks that OOM kill under normal traffic
- mixing too many containers in one task definition without a clear reason

---

## 8.5 ECS capacity providers

Capacity providers define how ECS places tasks onto available compute.

### Use cases
- blend EC2 and Fargate
- use managed scaling for ECS on EC2
- support fallback or cost-aware strategies

### Common mistake
Running ECS on EC2 without clear capacity ownership and then blaming ECS for host-level exhaustion.

---

## 8.6 ECS deployments

### Rolling deployment
Default-style replacement of tasks with new versions.

### Blue/green deployment
Advanced deployment with traffic shifting and safer cutover.

### Best practice
Use blue/green when:
- startup failure risk is meaningful
- application state compatibility needs cautious rollout
- production change control is strict

---

# 9. Amazon EKS and Karpenter

Amazon EKS is the managed Kubernetes service in AWS. It provides a managed control plane while leaving different degrees of worker-node and workload responsibility to you.

## 9.1 EKS core entities

### Cluster
Managed Kubernetes control plane and configuration boundary.

### Control plane
Managed Kubernetes API server and supporting control components operated by AWS.

### Managed node group
AWS-managed group of EC2 worker nodes.

### Self-managed nodes
Worker nodes you manage directly.

### Fargate profile
Rule-based mapping for running certain pods on Fargate.

### Add-on
Managed cluster extension such as networking or storage integrations.

### Access entry
Cluster access management entry integrated with AWS access control patterns.

### Kubernetes service account
Native Kubernetes identity object for pods.

### Pod identity association
Association allowing pods to receive AWS permissions via EKS Pod Identity.

### IRSA
IAM Roles for Service Accounts, a mechanism to grant pod-scoped AWS permissions.

## 9.2 EKS execution choices

### Managed node groups
Best default for many teams.

### Self-managed nodes
Use when you need deeper host customization.

### Fargate on EKS
Use when you want serverless pod execution for selected workloads.

### Selection logic
- managed node groups = simpler default
- self-managed = max control
- Fargate profiles = reduced node ops for specific pod classes

---

## 9.3 Cluster access and endpoint access

Important controls:
- private/public endpoint access
- access entries / cluster access model
- add-on lifecycle
- security group integration
- node access patterns

### Best practice
For sensitive clusters:
- prefer private endpoint access where architecture allows
- centralize admin access patterns
- explicitly manage cluster access rather than ad hoc broad admin mappings

---

## 9.4 IRSA and Pod Identity

These are critical Week 3 topics.

### IRSA
Lets Kubernetes service accounts assume scoped IAM roles through OIDC federation.

### EKS Pod Identity
Provides another AWS-native method to associate pods with AWS permissions.

### Why this matters
Pods need AWS access for:
- S3
- DynamoDB
- Secrets Manager
- SQS
- KMS
- many other integrations

The correct design is pod-scoped identity, not node-wide overprivileged roles.

### Example
A reporting pod uses:
- a dedicated service account
- a dedicated IAM role
- access only to one S3 bucket prefix

### Common mistakes
- giving worker node roles all application permissions
- sharing one broad pod role across many services
- not understanding which mechanism the cluster standard uses: IRSA, Pod Identity, or both in different phases

---

## 9.5 Add-ons

Add-ons are managed cluster components.

Common examples include:
- networking add-ons
- DNS
- storage drivers
- observability integrations depending on architecture

### Best practices
- treat add-ons as lifecycle-managed dependencies
- version and upgrade deliberately with cluster upgrades
- test compatibility with workloads

### Common mistakes
- upgrading the cluster before validating add-on compatibility

---

## 9.6 Karpenter

Karpenter is a Kubernetes-native node provisioning system.

### Core entities

#### NodePool
Defines provisioning and disruption behavior for a class of nodes.

#### EC2NodeClass
Defines AWS-specific configuration for launched EC2 capacity.

#### NodeClaim
Represents provisioned node instances for scheduling needs.

### Technical role
Karpenter dynamically provisions right-sized nodes based on pending pod requirements rather than relying only on fixed node groups.

### Example
A mixed workload cluster uses Karpenter to create:
- compute-optimized nodes for API pods
- memory-optimized nodes for analytics pods
- spot-based pools for batch workloads

### Best practices
- define NodePools by workload class
- control disruption behavior
- constrain instance families, zones, and capacity types carefully
- observe scale-out and consolidation behavior

### Common mistakes
- unconstrained provisioning leading to unexpected instance types
- misunderstanding consolidation/disruption effects on workloads
- not preparing scheduling labels, taints, and resource requests properly

---

## 9.7 EKS upgrades and operations

Typical upgrade workflow includes:
- control plane upgrade
- add-on compatibility review
- managed node group upgrade
- application validation
- pod identity and access regression checks

### Common mistakes
- upgrading all layers at once without staged validation
- neglecting disruption budgets and scheduling behavior during rollout

---

# 10. AWS Lambda

Lambda is AWS’s event-driven serverless compute service.

## 10.1 Technical purpose
Run code without managing servers, typically per request or event.

## 10.2 Core entities

### Function
The deployable unit of Lambda code and configuration.

### Version
An immutable published snapshot of a function.

### Alias
A stable named pointer to a specific version.

### Layer
Reusable dependency package shared across functions.

### Event source mapping
Connection between event source and Lambda polling/invocation behavior.

### Function URL
Direct HTTPS endpoint for a function.

### Execution role
IAM role assumed by the function at runtime.

### Provisioned concurrency
Pre-initialized execution environments to reduce cold start impact.

### Reserved concurrency
Dedicated concurrency limit assigned to a function.

## 10.3 Invocation models
- synchronous
- asynchronous
- poll-based through event source mappings

## 10.4 Versions and aliases

### Why they matter
They make controlled deployments possible.

### Example
- version 17 published
- alias `prod` points to version 16
- deployment shifts alias to version 17 after validation

### Best practices
- publish versions for release management
- deploy aliases, not raw mutable latest code
- separate config and rollout carefully

### Common mistakes
- using only the `$LATEST` version in production
- no versioned rollback path

---

## 10.5 Layers

Layers package shared dependencies.

### Best fit
- common libraries
- shared runtime dependencies
- standard utility bundles

### Common mistake
Overusing layers to hide poor dependency control or creating tight coupling across too many functions.

---

## 10.6 Event source mappings

Used for sources such as streams and queues where Lambda polls the source.

### Operational importance
Need to understand:
- batch size
- retry behavior
- failure handling
- partial batch handling in relevant sources
- throttling interactions

### Common mistake
Ignoring the interaction between event source polling speed, downstream dependency limits, and Lambda concurrency.

---

## 10.7 Concurrency controls

### Reserved concurrency
Protects capacity for a function and also caps it.

### Provisioned concurrency
Reduces cold starts for latency-sensitive functions.

### Example
A critical auth Lambda uses provisioned concurrency during peak hours.

### Common mistakes
- no concurrency planning, leading to downstream overload
- blaming Lambda for throttling when the function’s reserved concurrency was too low or account concurrency limits were reached

---

## 10.8 Lambda controls

Important controls include:
- execution role
- resource-based policies
- VPC configuration
- environment variables
- concurrency limits
- code signing
- dead-letter handling in supported patterns

### VPC configuration note
Placing Lambda in a VPC is not inherently “more secure”; it is a connectivity decision. It also changes network behavior and can complicate outbound access.

---

# 11. App Runner, Elastic Beanstalk, and Lightsail

These services matter because not every team needs ECS/EKS/Lambda complexity.

## 11.1 AWS App Runner

### Technical purpose
Run containerized or source-based web apps with simplified deployment and scaling.

### Core entities
- service
- source configuration
- VPC connector
- custom domain

### Best fit
Teams that want:
- simpler app deployment than ECS/EKS
- managed HTTPS and app runtime
- less platform engineering overhead

### Example
A small internal dashboard is deployed from a container image to App Runner with a custom domain.

### Best practices
- use for simpler app services with moderate platform needs
- understand VPC egress/connectivity patterns

### Common mistakes
- choosing App Runner for highly specialized networking or orchestration needs better served by ECS/EKS

---

## 11.2 AWS Elastic Beanstalk

### Technical purpose
Managed application platform for supported runtime stacks.

### Core entities
- application
- application version
- environment
- platform
- deployment policy

### Best fit
Traditional app teams wanting managed platform features without building all infra directly.

### Common mistakes
- assuming Beanstalk removes the need to understand underlying AWS resources
- using it when container-native orchestration is the clearer long-term platform choice

---

## 11.3 Amazon Lightsail

### Technical purpose
Simplified compute and application hosting for smaller-scale workloads.

### Core entities
- instance
- blueprint
- bundle
- static IP
- snapshot
- container service

### Best fit
- lightweight apps
- prototypes
- small websites
- simple admin workloads

### Common mistakes
- forcing Lightsail into enterprise multi-service platform roles it was not meant for

---

# 12. AWS Batch

AWS Batch manages queued, schedulable batch compute workloads.

## Core entities
- job
- job queue
- job definition
- compute environment
- scheduling policy
- array job
- multi-node parallel job
- fair share identifier

## Technical role
Batch is best for:
- offline compute
- queue-driven workloads
- scientific or data processing jobs
- jobs with variable runtime and parallelism patterns

## Example
Nightly report generation submits array jobs for 5,000 independent data partitions.

## Best practices
- define queue priorities intentionally
- right-size compute environments
- separate latency-sensitive from bulk background queues
- define retry and timeout policies clearly

## Common mistakes
- using Batch for always-on APIs
- no queue prioritization for mixed workload classes
- no timeout controls causing stuck capacity consumption

---

# 13. CodeBuild, CodePipeline, CodeDeploy, and CodeArtifact

These services form the core AWS-native delivery toolchain.

---

## 13.1 AWS CodeBuild

### Technical purpose
Run builds, tests, packaging, and artifact generation.

### Core entities
- build project
- buildspec
- reports
- cache

### Example
A CodeBuild project:
1. pulls source
2. runs unit tests
3. builds Docker image
4. pushes to ECR
5. exports artifact metadata for pipeline promotion

### Best practices
- keep buildspecs in version control
- use build caches carefully
- run tests before artifact promotion
- isolate secrets and environment variables securely
- use least-privilege service roles

### Common mistakes
- giant monolithic build projects for unrelated apps
- mutable build environments with hidden dependency drift
- no test reporting or artifact traceability

---

## 13.2 AWS CodePipeline

### Technical purpose
Orchestrate CI/CD stages and actions.

### Core entities
- pipeline
- stage
- action
- source action
- approval action
- cross-account action

### Example pipeline
1. source
2. build
3. security scan
4. manual approval
5. deploy to staging
6. integration tests
7. deploy to production

### Best practices
- separate build from deploy responsibilities
- use approvals where risk justifies them
- model promotions explicitly
- support rollback paths
- use cross-account actions for environment separation

### Common mistakes
- one pipeline doing everything for all apps
- no approvals or gates for production
- deploying directly from developer branches into prod-style environments

---

## 13.3 AWS CodeDeploy

### Technical purpose
Automate deployments to supported compute targets.

### Core entities
- application
- deployment group
- deployment configuration
- AppSpec file

### Common deployment targets
- EC2/on-prem in supported models
- ECS blue/green patterns
- Lambda alias traffic shifting in supported patterns

### Example
A Lambda deployment shifts:
- 10% traffic to new version
- monitors alarms
- then completes full cutover

### Best practices
- use deployment configurations intentionally
- integrate alarms and rollback
- test AppSpec-driven workflows carefully

### Common mistakes
- no rollback hooks
- treating CodeDeploy as a black box without understanding target-specific deployment behavior

---

## 13.4 AWS CodeArtifact

### Technical purpose
Managed package repository for software dependencies.

### Core entities
- domain
- repository
- package
- package version
- external connection

### Why it matters
Platform maturity is not just containers; it also includes dependency governance.

### Example
A central platform team hosts:
- internal Python packages
- approved npm packages
- Maven artifacts  
with controlled upstream external connections.

### Best practices
- centralize trusted package sources
- control package origin and promotion
- separate internal packages from mirrored public dependencies

### Common mistakes
- allowing unrestricted direct public dependency pulls from every build environment
- no package provenance strategy

---

# 14. CloudFormation, SAM, CDK, and Proton

These are the infrastructure and platform delivery backbone.

---

## 14.1 AWS CloudFormation

### Technical purpose
Declaratively define and manage AWS infrastructure as code.

### Core entities
- stack
- stack set
- template
- change set
- nested stack

### How it works
CloudFormation compares desired state in the template with actual managed resources and performs create/update/delete operations.

### Example
A stack defines:
- ALB
- ECS service
- target groups
- IAM roles
- CloudWatch alarms

### Best practices
- version control templates
- use change sets for risky updates
- modularize with nested stacks where sensible
- use stack policies and termination protection for critical stacks
- use service roles to control execution permissions

### Common mistakes
- giant single stacks for unrelated systems
- direct console changes causing drift
- no rollback planning

---

## 14.2 Change sets

Change sets preview stack modifications before execution.

### Best use
Use for:
- risky production changes
- replacement-sensitive resources
- team review processes

### Common mistake
Skipping change set review and discovering destructive replacements during live deployment.

---

## 14.3 Drift

Drift occurs when actual resources differ from template-defined expectations.

### Why it matters
Drift breaks reproducibility and confidence.

### Common sources
- manual console edits
- ad hoc remediation
- unmanaged dependent changes

### Best practice
Review drift as part of platform hygiene.

---

## 14.4 AWS SAM

AWS SAM extends CloudFormation for serverless applications.

### Core entities
- SAM template
- AWS::Serverless resources

### Technical role
Simplify authoring of serverless stacks such as:
- Lambda
- API integrations
- event sources
- deployment preferences in supported patterns

### Example
A serverless app uses SAM to define:
- Lambda function
- API endpoint
- permissions
- deployment preference with gradual traffic shifting

### Common mistake
Treating SAM as a separate infrastructure universe when it ultimately synthesizes to CloudFormation resources.

---

## 14.5 AWS CDK

CDK lets you define infrastructure using programming languages that synthesize into CloudFormation templates.

### Core entities
- app
- construct
- asset
- synthesized template

### Technical role
Provide higher-level abstraction and reusable infrastructure components.

### Example
A platform team creates a reusable construct for:
- ECS service
- ALB integration
- alarms
- logging
- autoscaling

### Best practices
- treat synthesized templates as deployable truth
- version reusable constructs
- control bootstrap stacks and permissions boundaries
- keep abstraction clear, not magical

### Common mistakes
- writing overly complex imperative logic instead of clear infrastructure definitions
- not reviewing synthesized output
- hidden behavior inside homegrown constructs

---

## 14.6 AWS Proton

Proton is a platform delivery service for standardized environments and service templates.

### Core entities
- environment template
- service template
- environment
- service
- service instance
- component

### Technical role
Platform teams define golden templates; application teams consume them.

### Example
A platform team publishes:
- ECS service template
- Lambda service template
- standard VPC environment template  
Developers create service instances from approved platform patterns.

### Best practices
- use Proton where platform standardization is the priority
- keep templates opinionated but usable
- define versioning and rollout process for template evolution

### Common mistakes
- building platform templates with no ownership model
- too much customization, eliminating the benefit of standardization

---

# 15. Amazon SageMaker

SageMaker is AWS’s ML platform covering data prep, training, model registry, deployment, and monitoring.

## 15.1 Core entities

### Domain
Studio domain and shared ML working environment boundary.

### User profile
User identity/profile in the ML workspace environment.

### Space
Collaborative or isolated working area for ML activity.

### Training job
Managed model training execution.

### Processing job
Managed data processing/transformation job.

### Feature group
Managed feature store construct for ML features.

### Data flow
Data preparation flow artifact.

### Model
Deployable model artifact definition.

### Endpoint configuration
Specifies model hosting settings.

### Endpoint
Deployed inference endpoint.

### Pipeline
Orchestrated ML workflow.

### Model package group
Grouping for model version registry.

### Model package
Registered model version and metadata.

### Monitoring schedule
Periodic model/data monitoring process.

### Baseline
Reference dataset/statistics for monitoring comparison.

### Constraints
Thresholds and expectations used in monitoring.

### Experiment / Trial / Trial component
Experiment-tracking entities.

### Lineage artifacts / actions / contexts
Metadata chain for tracing ML asset relationships.

## 15.2 Technical role
SageMaker gives structure to ML platform delivery:
- training
- processing
- model registry
- deployment
- monitoring
- lineage

## 15.3 Example
An ML team:
1. runs processing job
2. trains model
3. registers model package
4. deploys staging endpoint
5. reviews monitoring baseline
6. promotes to production endpoint

## 15.4 Best practices
- control execution roles tightly
- use VPC/network isolation where appropriate
- register models formally before promotion
- monitor drift and bias where relevant
- capture lineage for auditability

## 15.5 Common mistakes
- training jobs without reproducible metadata
- direct production deployment with no registry step
- no monitoring after deployment
- broad execution roles with excessive data access

---

# 16. Amazon Bedrock

Bedrock provides managed access to foundation models and related generative AI platform services.

## 16.1 Core entities

### Foundation model
Underlying model offering available through the platform.

### Inference profile
Configuration/profile for invoking model inference patterns.

### Provisioned throughput
Reserved capacity model for predictable workloads in supported cases.

### Agent
Managed orchestration component for tool-using or workflow-driven AI behavior.

### Knowledge base
Retrieval-oriented knowledge integration construct.

### Guardrail
Policy control for model interaction safety and filtering.

### Model customization job
Model adaptation/customization workflow in supported cases.

### Model evaluation job
Evaluation workflow for model quality comparison.

### Invocation logs
Operational records of model invocation behavior.

## 16.2 Technical role
Bedrock is part of platform delivery when teams need:
- managed model access
- RAG-style knowledge integration
- agent workflows
- guardrail enforcement
- centralized model invocation governance

## 16.3 Example
A customer support assistant uses:
- knowledge base for internal docs
- guardrails for response boundaries
- invocation logging for audit and quality review

## 16.4 Best practices
- treat model access as governed platform capability
- restrict IAM permissions carefully
- use guardrails for safety and policy control
- log invocations where governance requires it
- evaluate models instead of assuming one model fits every workload

## 16.5 Common mistakes
- no guardrails
- direct broad production use without evaluation
- not controlling who can access which model capabilities

---

# 17. Service Selection Guide

## 17.1 EC2 vs ECS vs EKS vs Lambda

### EC2
Choose when you need:
- full OS control
- legacy runtimes
- special drivers or agents
- custom host behavior

### ECS
Choose when you need:
- container orchestration
- simpler AWS-native operations
- less control-plane burden than Kubernetes

### EKS
Choose when you need:
- Kubernetes portability/ecosystem
- advanced platform patterns
- cluster-standardized container orchestration

### Lambda
Choose when you need:
- event-driven execution
- minimal server management
- function-level scaling
- short-lived or request-triggered workloads

### Common mistake
Choosing the most fashionable option rather than the simplest operationally correct one.

---

## 17.2 ECS vs EKS

### ECS
- simpler
- more AWS-native
- less control-plane complexity

### EKS
- Kubernetes-native ecosystem
- more flexibility and portability
- more operational complexity

### Best practice
Use EKS only when Kubernetes capabilities are genuinely needed.

---

## 17.3 App Runner vs ECS/Fargate

### App Runner
- simpler app deployment
- fewer knobs
- less platform overhead

### ECS/Fargate
- more flexibility
- deeper control
- better for complex service fleets

---

## 17.4 CloudFormation vs CDK vs SAM vs Proton

### CloudFormation
Declarative base layer.

### CDK
Higher-level programmable abstraction that synthesizes CloudFormation.

### SAM
Serverless-focused abstraction on CloudFormation.

### Proton
Platform template delivery for standardized service consumption.

### Best practice
Understand how they complement each other rather than treating them as direct duplicates.

---

# 18. End-to-End Platform Delivery Example

A platform team wants to support:
- Java API services
- containerized workers
- event-driven functions
- EKS-based data services
- repeatable environments
- controlled image promotion
- production-safe deployment

A strong design could be:

### Build and artifacts
- CodeBuild builds container images
- ECR stores images with immutable tags and scanning
- CodeArtifact stores internal language packages
- EC2 Image Builder produces hardened AMIs

### Infrastructure definition
- CDK defines shared platform components
- CloudFormation deploys infrastructure
- SAM defines Lambda-based apps
- Proton publishes standardized service templates

### Runtime choices
- ECS/Fargate for standard API and worker services
- EKS for Kubernetes-dependent platform workloads
- Lambda for event-driven integrations
- EC2 only for workloads needing special host control

### Deployment
- CodePipeline orchestrates build, test, approval, and deploy
- CodeDeploy handles traffic-shifted deployment for Lambda/ECS/EC2-supported patterns
- alarms trigger rollback logic

### ML/AI extension
- SageMaker for model training and endpoint deployment
- Bedrock for managed foundation model access in application features

This is the Week 3 principle:
> delivery maturity comes from linking compute, artifacts, infrastructure definitions, and safe rollout controls into one system.

---

# 19. Troubleshooting Framework for Week 3

When compute or deployment fails, troubleshoot in this order.

## 19.1 Artifact integrity
- correct image tag?
- image exists in ECR?
- package exists in CodeArtifact?
- AMI version correct?

## 19.2 Identity and permissions
- instance profile right?
- ECS task role right?
- Lambda execution role right?
- EKS pod identity/IRSA right?
- CI/CD service role right?

## 19.3 Runtime config
- right environment variables?
- enough CPU/memory?
- startup command valid?
- container health checks correct?
- Lambda concurrency configured properly?

## 19.4 Deployment mechanics
- CodePipeline stage failure?
- CodeBuild buildspec issue?
- CodeDeploy AppSpec issue?
- ECS deployment unhealthy?
- ASG instance refresh stalled?
- EKS rollout blocked by scheduling constraints?

## 19.5 Platform definition
- CloudFormation drift?
- change set replacing critical resource?
- CDK synthesized unexpected output?
- Proton template version mismatch?

## 19.6 Service-specific observability
- EC2 status checks
- ECS task events
- EKS pod/node events
- Lambda logs and throttles
- build/deploy logs
- training job logs
- Bedrock invocation logs where relevant

---

# 20. Common Week 3 Mistakes

- choosing EKS for simple workloads that fit ECS
- using EC2 for workloads that should be immutable or serverless
- broad IAM roles for tasks, pods, or functions
- mutable tags like `latest` as production truth
- no image scanning or lifecycle control in ECR
- weak deployment rollback strategy
- no separation between build artifact creation and prod promotion
- unmanaged CloudFormation drift
- hidden logic inside CDK constructs with poor review
- cluster upgrades without add-on compatibility checks
- Lambda functions deployed without version/alias discipline
- no model registry or monitoring for ML workloads
- no guardrails or evaluation flow for Bedrock usage

---

# 21. Week 3 Revision Checklist

You should be able to explain clearly:

- what EC2 gives you that higher-level compute services abstract away
- how AMIs, launch templates, and ASGs work together
- what lifecycle hooks, warm pools, and instance refresh do
- how ECR scanning, tag immutability, and replication affect release quality
- the difference between ECS task role and execution role
- the difference between ECS on EC2 and ECS on Fargate
- the difference between EKS managed node groups, self-managed nodes, and Fargate profiles
- why IRSA and Pod Identity matter
- what Karpenter NodePools, EC2NodeClasses, and NodeClaims do
- why Lambda versions and aliases matter in production
- where App Runner, Elastic Beanstalk, Lightsail, and Batch fit
- what CodeBuild, CodePipeline, CodeDeploy, and CodeArtifact each do
- how CloudFormation, SAM, CDK, and Proton differ and connect
- what SageMaker domains, training jobs, model packages, endpoints, and monitoring schedules are
- what Bedrock guardrails, knowledge bases, agents, and invocation logging are

---

# 22. Memory Anchors

- **EC2** = full VM control
- **AMI** = launch image baseline
- **Launch template** = instance launch specification
- **ASG** = fleet scaling and replacement controller
- **Lifecycle hook** = pause during launch/terminate workflow
- **Warm pool** = preinitialized scaling buffer
- **Instance refresh** = rolling ASG replacement
- **ECR** = container image registry
- **ECS** = AWS-native container orchestration
- **Fargate** = serverless container compute
- **EKS** = managed Kubernetes control plane
- **IRSA / Pod Identity** = pod-scoped AWS permissions
- **Karpenter** = dynamic Kubernetes node provisioning
- **Lambda** = event-driven serverless compute
- **App Runner** = simplified managed app deployment
- **Elastic Beanstalk** = managed app platform
- **Lightsail** = simplified small-scale hosting
- **Batch** = queued batch compute orchestration
- **CodeBuild** = build and test
- **CodePipeline** = CI/CD orchestration
- **CodeDeploy** = deployment and rollout control
- **CodeArtifact** = package repository
- **CloudFormation** = declarative infra as code
- **SAM** = serverless IaC abstraction
- **CDK** = code-defined infra that synthesizes CloudFormation
- **Proton** = platform service/environment templates
- **SageMaker** = ML platform lifecycle
- **Bedrock** = managed foundation model platform

---

# 23. Official AWS Documentation References

## Compute
- Amazon EC2 Documentation  
  https://docs.aws.amazon.com/ec2/

- Amazon EC2 Auto Scaling Documentation  
  https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html

- EC2 Image Builder Documentation  
  https://docs.aws.amazon.com/imagebuilder/

## Containers
- Amazon ECR Documentation  
  https://docs.aws.amazon.com/ecr/

- Amazon ECS Documentation  
  https://docs.aws.amazon.com/ecs/

- AWS Fargate on ECS  
  https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html

- Amazon EKS Documentation  
  https://docs.aws.amazon.com/eks/

- Karpenter Documentation  
  https://karpenter.sh/

## Serverless and simplified app services
- AWS Lambda Documentation  
  https://docs.aws.amazon.com/lambda/

- AWS App Runner Documentation  
  https://docs.aws.amazon.com/apprunner/

- AWS Elastic Beanstalk Documentation  
  https://docs.aws.amazon.com/elasticbeanstalk/

- Amazon Lightsail Documentation  
  https://docs.aws.amazon.com/lightsail/

- AWS Batch Documentation  
  https://docs.aws.amazon.com/batch/

## CI/CD and artifacts
- AWS CodeBuild Documentation  
  https://docs.aws.amazon.com/codebuild/

- AWS CodePipeline Documentation  
  https://docs.aws.amazon.com/codepipeline/

- AWS CodeDeploy Documentation  
  https://docs.aws.amazon.com/codedeploy/

- AWS CodeArtifact Documentation  
  https://docs.aws.amazon.com/codeartifact/

- AWS CodeConnections Documentation  
  https://docs.aws.amazon.com/dtconsole/latest/userguide/connections.html

## Infrastructure as code and platform delivery
- AWS CloudFormation Documentation  
  https://docs.aws.amazon.com/cloudformation/

- AWS SAM Documentation  
  https://docs.aws.amazon.com/serverless-application-model/

- AWS CDK Documentation  
  https://docs.aws.amazon.com/cdk/

- AWS Proton Documentation  
  https://docs.aws.amazon.com/proton/

## AI/ML platform
- Amazon SageMaker Documentation  
  https://docs.aws.amazon.com/sagemaker/

- Amazon Bedrock Documentation  
  https://docs.aws.amazon.com/bedrock/

---

# 24. Final Summary

Week 3 is about **where applications run and how they are delivered safely**.

To master Week 3, you must understand both:
- the **execution models**
- the **delivery mechanics**

The strongest mental model is:

- **EC2** gives maximum host control
- **ECS and EKS** give container orchestration at different complexity levels
- **Lambda** gives function-level serverless execution
- **App Runner, Elastic Beanstalk, Lightsail, and Batch** solve more specific execution patterns
- **ECR and CodeArtifact** hold the artifacts that runtime systems consume
- **CodeBuild, CodePipeline, and CodeDeploy** turn source into deployable, auditable rollouts
- **CloudFormation, SAM, CDK, and Proton** define and standardize the platform
- **SageMaker and Bedrock** extend the platform into ML and generative AI delivery

When you can explain how those layers connect, Week 3 is no longer just a list of AWS services — it becomes a coherent platform architecture model.
