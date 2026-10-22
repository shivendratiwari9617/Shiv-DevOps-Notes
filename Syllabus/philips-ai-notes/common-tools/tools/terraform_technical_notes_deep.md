# Terraform Technical Notes — Deep Concept Guide and Commands with Examples

This file is a **technical notes document** for **deep understanding of Terraform**.

It is intentionally focused on:

- the **core architecture** of Terraform
- the **technical meaning** of each major Terraform part
- the **language building blocks**
- the **workflow and execution model**
- the **most important commands**
- **examples** for each major command and concept

It is **not** organized as troubleshooting notes or production-incident notes.  
The goal is to help you understand **how Terraform really works** and explain it well in interviews.

---

# 1) What Terraform is

Terraform is an **Infrastructure as Code (IaC)** tool created by HashiCorp.  
It allows you to define infrastructure in a **declarative language** and then create, update, or destroy that infrastructure using provider APIs.

In simple terms:

- you write the desired infrastructure in configuration files
- Terraform compares that desired state with what it already knows
- it builds an execution plan
- it applies the required changes through cloud or platform APIs

Terraform is called **declarative** because you describe **what** you want, not the step-by-step shell procedure to create it.

Example idea:

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "my-app-logs-prod"
}
```

This does not say:

1. call API A
2. then call API B
3. then wait
4. then retry

Instead, it declares that a bucket should exist, and Terraform decides how to interact with the provider to make that true.

---

# 2) Terraform core mental model

To understand Terraform deeply, always think in terms of **four layers**:

1. **Configuration**  
   The HCL code you write.

2. **State**  
   Terraform’s record of managed resources.

3. **Provider/API reality**  
   The real infrastructure that exists in the platform.

4. **Execution graph**  
   The dependency graph Terraform builds to determine ordering.

A strong technical explanation is:

> Terraform reads configuration, loads providers, reads state, refreshes or compares against provider reality, constructs a dependency graph, produces a plan, and then applies graph operations to converge real infrastructure toward the declared configuration.

This is the heart of Terraform.

---

# 3) Terraform architecture

Terraform is not just “HCL files plus apply.”  
It has a real internal structure.

## 3.1 Terraform Core

Terraform Core is the engine that:

- reads configuration
- evaluates expressions
- loads provider schemas
- reads and writes state
- builds the dependency graph
- computes the plan
- orchestrates apply/destroy operations

Terraform Core itself does **not** directly create cloud resources.  
It delegates that to **providers**.

So you can think of Terraform Core as the planner and coordinator.

---

## 3.2 Providers

A **provider** is a plugin that knows how to communicate with an external API.

Examples:
- AWS provider
- AzureRM provider
- Google provider
- Kubernetes provider
- GitHub provider

A provider exposes:
- resource types
- data source types
- schema definitions
- CRUD behavior for infrastructure objects

Example:

```hcl
provider "aws" {
  region = "us-east-1"
}
```

This tells Terraform to use the AWS provider and target `us-east-1`.

A provider defines things like:

- `aws_instance`
- `aws_vpc`
- `aws_subnet`
- `aws_s3_bucket`

Those resource types do not come from Terraform Core.  
They come from the provider plugin.

### Key point
Terraform Core knows **how to plan**, but the provider knows **how to talk to the target system**.

---

## 3.3 Resources

A **resource** is a managed infrastructure object.

Examples:
- VM
- VPC
- subnet
- database
- DNS record
- storage bucket
- IAM role

General syntax:

```hcl
resource "<TYPE>" "<NAME>" {
  ...
}
```

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-1234567890abcdef0"
  instance_type = "t3.micro"
}
```

Here:

- `aws_instance` is the resource type
- `web` is the local name in Terraform
- the block body contains arguments

Terraform tracks this resource by its **resource address**, such as:

```text
aws_instance.web
```

This identity matters a lot in Terraform because resource addresses connect configuration to state.

---

## 3.4 Data sources

A **data source** reads existing information from a provider but does not manage lifecycle of that object.

Syntax:

```hcl
data "<TYPE>" "<NAME>" {
  ...
}
```

Example:

```hcl
data "aws_ami" "ubuntu" {
  most_recent = true

  owners = ["099720109477"]

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
  }
}
```

You can then use the result:

```hcl
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
}
```

### Difference between resource and data source

A resource:
- creates or manages an object

A data source:
- reads an already existing object

That distinction is fundamental.

---

## 3.5 State

**State** is Terraform’s stored mapping between configuration and real infrastructure.

State contains:
- resource addresses
- provider metadata
- known attribute values
- dependency relationships
- outputs

Without state, Terraform would not reliably know:
- what it already manages
- what resource in the cloud corresponds to which block in code
- what must be created, changed, or destroyed

### Why state exists
Terraform is not re-discovering intent from scratch every time.  
State is its memory.

### Example
If you create:

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "my-app-logs-prod"
}
```

Terraform records in state that `aws_s3_bucket.logs` maps to that real bucket.

### Important distinction
State is **not** the desired config.  
State is **not** the live cloud API either.  
It is Terraform’s stored model of managed objects.

---

## 3.6 Backends

A **backend** determines where state is stored and how operations like locking are handled.

Examples:
- local backend
- S3 backend
- AzureRM backend
- GCS backend
- Terraform Cloud backend

Example:

```hcl
terraform {
  backend "s3" {
    bucket = "tf-state-prod"
    key    = "network/terraform.tfstate"
    region = "us-east-1"
  }
}
```

A backend is important because:

- local state is risky for teams
- remote state supports collaboration
- some backends support locking
- centralized backends improve consistency and recovery

### Local backend
State stored on local disk.

### Remote backend
State stored remotely, usually with locking and shared access.

---

## 3.7 Modules

A **module** is a container for multiple Terraform resources used together.

Every Terraform configuration has a **root module**.  
Any child module is included using a `module` block.

Example child module call:

```hcl
module "network" {
  source = "./modules/network"

  vpc_cidr = "10.0.0.0/16"
}
```

A module helps:
- reuse infrastructure patterns
- standardize configuration
- reduce duplication
- separate concerns

A module can contain:
- resources
- data sources
- variables
- outputs
- locals

### Root module vs child module

- **Root module**: the current working directory you run Terraform from
- **Child module**: another module called by the root or another module

---

## 3.8 Variables

Variables let you parameterize Terraform configuration.

Example:

```hcl
variable "environment" {
  type        = string
  description = "Deployment environment"
}
```

Usage:

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "my-app-logs-${var.environment}"
}
```

You can pass values:
- through `.tfvars`
- through CLI flags
- through environment variables
- through defaults

Example default:

```hcl
variable "region" {
  type    = string
  default = "us-east-1"
}
```

Variables make modules reusable and configurable.

---

## 3.9 Locals

**Locals** define named expressions for reuse inside a module.

Example:

```hcl
locals {
  common_tags = {
    app  = "billing"
    env  = "prod"
    team = "platform"
  }
}
```

Use:

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "billing-prod-logs"
  tags   = local.common_tags
}
```

Locals are useful when:
- the same expression is reused many times
- you want cleaner configuration
- you want derived values

---

## 3.10 Outputs

**Outputs** expose values after apply.

Example:

```hcl
output "bucket_name" {
  value = aws_s3_bucket.logs.bucket
}
```

Outputs are used for:
- showing useful values to humans
- feeding values into other modules or systems
- exposing resource IDs, endpoints, IPs, or names

You can view them with:

```bash
terraform output
```

Or a specific one:

```bash
terraform output bucket_name
```

---

## 3.11 Expressions

Terraform supports expressions to compute values.

Examples:
- string interpolation
- arithmetic
- conditionals
- collection operations
- references

Example string interpolation:

```hcl
bucket = "app-${var.environment}-logs"
```

Conditional example:

```hcl
instance_type = var.environment == "prod" ? "t3.large" : "t3.micro"
```

Terraform expressions are important because much of Terraform’s flexibility comes from data transformation and conditional configuration.

---

## 3.12 Functions

Terraform includes built-in functions.

Examples:
- `length()`
- `concat()`
- `merge()`
- `lookup()`
- `split()`
- `join()`
- `toset()`
- `tomap()`

Example:

```hcl
locals {
  subnet_names = ["app-a", "app-b"]
  full_name    = join("-", ["billing", var.environment, "api"])
}
```

Functions help process data before passing it into resources.

---

## 3.13 Meta-arguments

Meta-arguments change how resources behave.

Important ones:
- `count`
- `for_each`
- `depends_on`
- `provider`
- `lifecycle`

### `count`
Creates multiple instances by number.

Example:

```hcl
resource "aws_instance" "web" {
  count         = 2
  ami           = "ami-1234567890abcdef0"
  instance_type = "t3.micro"
}
```

Addresses become:
- `aws_instance.web[0]`
- `aws_instance.web[1]`

### `for_each`
Creates multiple instances from a map or set.

Example:

```hcl
resource "aws_s3_bucket" "app" {
  for_each = toset(["logs", "assets", "backups"])
  bucket   = "my-${var.environment}-${each.key}"
}
```

Addresses become:
- `aws_s3_bucket.app["logs"]`
- `aws_s3_bucket.app["assets"]`

### `depends_on`
Explicit dependency.

Example:

```hcl
resource "aws_instance" "app" {
  ami           = "ami-1234567890abcdef0"
  instance_type = "t3.micro"

  depends_on = [aws_iam_role.app]
}
```

### `provider`
Selects a specific provider configuration.

Example:

```hcl
resource "aws_s3_bucket" "east" {
  provider = aws.use1
  bucket   = "my-east-bucket"
}
```

### `lifecycle`
Changes lifecycle behavior.

Example:

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "my-logs"

  lifecycle {
    prevent_destroy = true
  }
}
```

---

## 3.14 Provisioners

Provisioners let you execute scripts or commands during resource creation or destruction.

Examples:
- `local-exec`
- `remote-exec`
- `file`

Example:

```hcl
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo hello"
  }
}
```

### Important note
Provisioners exist, but they are generally considered a last resort because they reduce declarative clarity and make infrastructure less predictable.

Use them carefully.

---

## 3.15 Workspace

A **workspace** is a named state instance for the same configuration.

You can use workspaces to separate environments such as:
- dev
- staging
- prod

Example commands:

```bash
terraform workspace list
terraform workspace show
terraform workspace new dev
terraform workspace select prod
```

Workspaces change which state Terraform uses.

They do **not** automatically guarantee safe environment separation by themselves.  
You still need clear design.

---

# 4) Terraform language and file structure

Terraform commonly uses these file types:

- `.tf` — main Terraform configuration files
- `.tfvars` — variable values
- `.tfstate` — state file
- `.tfstate.backup` — backup of state
- `.terraform.lock.hcl` — dependency lock file

Common files in a project:

- `main.tf`
- `variables.tf`
- `outputs.tf`
- `providers.tf`
- `versions.tf`
- `terraform.tfvars`

Example structure:

```text
terraform-project/
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
├── versions.tf
├── terraform.tfvars
└── modules/
    └── network/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

---

# 5) Terraform workflow

A typical Terraform workflow is:

1. write configuration
2. initialize directory
3. validate syntax
4. review plan
5. apply changes
6. inspect outputs/state
7. iterate

The important technical point is that Terraform always works by trying to **converge reality** toward the declared configuration.

---

# 6) Terraform commands — deep explanation with examples

## 6.1 `terraform init`

### Purpose
Initializes the Terraform working directory.

What it does:
- downloads required providers
- initializes backend
- prepares modules
- creates `.terraform/` directory
- creates or updates lock file

Example:

```bash
terraform init
```

Example output idea:

```text
Initializing the backend...
Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 5.0"...
- Installing hashicorp/aws v5.45.0...
Terraform has been successfully initialized!
```

### When to run it
- first time in a directory
- after changing provider requirements
- after changing backend config
- after adding modules
- after cloning a repo

### Important options

#### `terraform init -upgrade`
Upgrades provider and module versions within constraints.

```bash
terraform init -upgrade
```

#### `terraform init -reconfigure`
Forces backend reconfiguration.

```bash
terraform init -reconfigure
```

#### `terraform init -migrate-state`
Migrates state to a new backend configuration.

```bash
terraform init -migrate-state
```

### Interview explanation
`init` prepares Terraform’s execution environment. It is where backend initialization and provider installation happen.

---

## 6.2 `terraform validate`

### Purpose
Checks whether the configuration is syntactically valid and internally consistent.

Example:

```bash
terraform validate
```

Example success:

```text
Success! The configuration is valid.
```

### What it checks
- syntax correctness
- required arguments
- basic type consistency
- internal structure

### What it does not check
- cloud credentials
- whether resources really exist
- whether the plan is safe
- provider-side runtime issues

### Interview explanation
`validate` is a configuration correctness check, not an infrastructure safety check.

---

## 6.3 `terraform fmt`

### Purpose
Formats Terraform files consistently.

Example:

```bash
terraform fmt
```

Recursive example:

```bash
terraform fmt -recursive
```

This improves readability and keeps style consistent across teams.

---

## 6.4 `terraform plan`

### Purpose
Shows the execution plan without applying it.

Example:

```bash
terraform plan
```

Example output idea:

```text
Terraform will perform the following actions:

  # aws_s3_bucket.logs will be created
  + resource "aws_s3_bucket" "logs" {
      + bucket = "my-app-logs-prod"
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

### Why it matters
`plan` is the preview of how Terraform intends to change infrastructure.

### Save plan to file

```bash
terraform plan -out=tfplan
```

Later apply that plan:

```bash
terraform apply tfplan
```

### Variable example

```bash
terraform plan -var="environment=prod"
```

### Variable file example

```bash
terraform plan -var-file="prod.tfvars"
```

### Interview explanation
`plan` compares desired configuration with state and provider data to determine what actions are needed.

---

## 6.5 `terraform apply`

### Purpose
Executes changes.

Example:

```bash
terraform apply
```

With auto-approve:

```bash
terraform apply -auto-approve
```

Apply a saved plan:

```bash
terraform apply tfplan
```

### What happens
- Terraform evaluates configuration
- refreshes/compares state as needed
- builds graph
- performs create/update/destroy operations

### Interview explanation
`apply` is the mutation phase where Terraform converges real infrastructure toward the declared state.

---

## 6.6 `terraform destroy`

### Purpose
Destroys all resources managed by the current configuration/state.

Example:

```bash
terraform destroy
```

With variable file:

```bash
terraform destroy -var-file="dev.tfvars"
```

### Important note
`destroy` affects what Terraform manages in the selected state/workspace/backend context.

### Interview explanation
`destroy` is effectively an apply operation whose target state is resource absence.

---

## 6.7 `terraform show`

### Purpose
Displays state or saved plan in human-readable form.

Example:

```bash
terraform show
```

Show saved plan:

```bash
terraform show tfplan
```

JSON output:

```bash
terraform show -json
```

This command is useful when you want to inspect:
- saved plan details
- current state content
- automation output

---

## 6.8 `terraform output`

### Purpose
Displays outputs from the state.

Example:

```bash
terraform output
```

Specific output:

```bash
terraform output bucket_name
```

JSON form:

```bash
terraform output -json
```

Example output block:

```hcl
output "vpc_id" {
  value = aws_vpc.main.id
}
```

Result:

```text
vpc_id = "vpc-0123456789abcdef0"
```

---

## 6.9 `terraform console`

### Purpose
Interactive console for evaluating expressions.

Example:

```bash
terraform console
```

Sample session:

```text
> upper("prod")
"PROD"

> join("-", ["billing", "prod", "api"])
"billing-prod-api"

> length(["a", "b", "c"])
3
```

This is very useful for:
- testing expressions
- understanding types
- debugging locals and transforms
- learning Terraform functions

---

## 6.10 `terraform state list`

### Purpose
Lists resources currently tracked in state.

Example:

```bash
terraform state list
```

Example output:

```text
aws_s3_bucket.logs
aws_iam_role.app
module.network.aws_vpc.main
```

This tells you what Terraform currently manages.

---

## 6.11 `terraform state show`

### Purpose
Shows details for a specific state object.

Example:

```bash
terraform state show aws_s3_bucket.logs
```

Useful to inspect:
- stored attributes
- resource mapping
- current state-known values

---

## 6.12 `terraform state mv`

### Purpose
Moves a resource address in state.

This is used when the real infrastructure object stays the same, but the Terraform address changes.

Example:

```bash
terraform state mv aws_s3_bucket.logs module.storage.aws_s3_bucket.logs
```

This tells Terraform:
- same object
- new address

It is commonly used during:
- refactors
- module moves
- renames

---

## 6.13 `terraform state rm`

### Purpose
Removes a resource from state without destroying the real object.

Example:

```bash
terraform state rm aws_s3_bucket.logs
```

After this, Terraform no longer tracks it.

Important:  
This does **not** delete the real cloud object.

---

## 6.14 `terraform import`

### Purpose
Imports an existing real resource into Terraform state.

Example:

```bash
terraform import aws_s3_bucket.logs my-app-logs-prod
```

After import, Terraform knows that the real object belongs to that resource address.

Important:
- import adds it to state
- you still need matching configuration in code

---

## 6.15 `terraform taint` and `untaint` concept

Older workflows used `taint` to mark a resource for forced recreation.  
Modern Terraform workflows rely more on `-replace`.

### Replacement example

```bash
terraform apply -replace="aws_instance.web"
```

This tells Terraform to recreate that resource deliberately.

---

## 6.16 `terraform workspace`

### Purpose
Manages workspaces.

Examples:

```bash
terraform workspace list
terraform workspace show
terraform workspace new dev
terraform workspace select prod
terraform workspace delete dev
```

Example output:

```text
default
dev
* prod
```

The `*` marks the current workspace.

---

## 6.17 `terraform graph`

### Purpose
Outputs the dependency graph in DOT format.

Example:

```bash
terraform graph
```

Example usage:

```bash
terraform graph > graph.dot
```

This can help visualize resource dependencies.

---

## 6.18 `terraform providers`

### Purpose
Shows providers required by the configuration and state.

Example:

```bash
terraform providers
```

This is useful for understanding:
- provider inheritance
- module provider usage
- required provider tree

---

## 6.19 `terraform version`

### Purpose
Shows Terraform version and provider version information.

Example:

```bash
terraform version
```

This is important because behavior can change across versions.

---

# 7) Important Terraform concepts in depth

## 7.1 Desired state vs current state

Terraform works toward a **desired state**.

Desired state:
- what configuration declares

Current state:
- what Terraform knows and/or what provider reality shows

Terraform computes the difference and acts on that difference.

---

## 7.2 Declarative model

Terraform is declarative, not imperative.

Imperative approach:
- do step A
- then B
- then C

Declarative approach:
- describe the target system
- let the engine compute necessary actions

This is one reason Terraform can reason about drift, diffs, and replacement.

---

## 7.3 Dependency graph

Terraform builds a graph from references.

Example:

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "app" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
}
```

Terraform infers:
- VPC first
- subnet second

Because `aws_subnet.app` references `aws_vpc.main.id`.

This is how Terraform determines ordering.

---

## 7.4 Implicit dependency vs explicit dependency

### Implicit dependency
Created by references.

Example:

```hcl
vpc_id = aws_vpc.main.id
```

### Explicit dependency
Created with `depends_on`.

Example:

```hcl
depends_on = [aws_iam_role.app]
```

Use implicit dependencies whenever possible.  
Use explicit dependencies when Terraform cannot infer the ordering.

---

## 7.5 Known values and unknown values

Some values are known during plan.  
Some are only known after apply.

Example unknown value:

```text
id = (known after apply)
```

This happens when the provider can only return a value after resource creation.

Understanding unknown values is important because some constructs like `for_each` need stable keys during planning.

---

## 7.6 Immutable attributes and replacement

Some resource arguments can be updated in place.  
Others force replacement.

Example:

```text
~ name = "old" -> "new" # forces replacement
```

This depends on provider schema.

When a change forces replacement, Terraform may:
- destroy then create
- create before destroy
- or follow lifecycle rules

---

## 7.7 `count` vs `for_each`

### `count`
Numeric indexing.

Good for:
- simple repeated resources

Weakness:
- index-based identity can shift

### `for_each`
Key-based identity.

Good for:
- named resources
- stable identity
- map/set driven resources

Example:

```hcl
resource "aws_s3_bucket" "b" {
  for_each = {
    logs   = "logs"
    assets = "assets"
  }

  bucket = "my-${each.key}"
}
```

In practice, `for_each` is often better for stable resource identity.

---

## 7.8 Lifecycle rules

Lifecycle rules modify how Terraform handles resource changes.

Common lifecycle arguments:
- `prevent_destroy`
- `create_before_destroy`
- `ignore_changes`

Example:

```hcl
lifecycle {
  create_before_destroy = true
}
```

Example:

```hcl
lifecycle {
  ignore_changes = [tags]
}
```

These features are powerful, but should be used carefully because they can change plan behavior significantly.

---

## 7.9 Provider aliases

Provider aliases let you configure multiple instances of the same provider.

Example:

```hcl
provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "west"
  region = "us-west-2"
}
```

Use:

```hcl
resource "aws_s3_bucket" "west_logs" {
  provider = aws.west
  bucket   = "west-logs-example"
}
```

This is important for:
- multi-region deployments
- multi-account setups
- explicit environment targeting

---

## 7.10 Required versions and dependency lock file

Example:

```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

This controls version compatibility.

`.terraform.lock.hcl` records provider selections for reproducibility.

This matters because Terraform and provider versions can change behavior.

---

# 8) Complete mini example

Here is a small but complete Terraform example.

```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

variable "region" {
  type    = string
  default = "us-east-1"
}

variable "environment" {
  type = string
}

locals {
  bucket_name = "billing-${var.environment}-logs"
}

resource "aws_s3_bucket" "logs" {
  bucket = local.bucket_name

  tags = {
    env  = var.environment
    app  = "billing"
    team = "platform"
  }
}

output "bucket_name" {
  value = aws_s3_bucket.logs.bucket
}
```

### What this example shows
- required Terraform version
- required provider
- provider config
- variables
- locals
- resource
- output

### Example commands for this mini project

Initialize:

```bash
terraform init
```

Validate:

```bash
terraform validate
```

Plan:

```bash
terraform plan -var="environment=prod"
```

Apply:

```bash
terraform apply -var="environment=prod"
```

Read output:

```bash
terraform output bucket_name
```

---

# 9) Common interview-ready technical explanations

## What is Terraform state?

A strong answer:

> Terraform state is Terraform’s persisted mapping between resource addresses in configuration and real infrastructure objects, along with known metadata and attributes. It allows Terraform to track what it manages and compute safe diffs between configuration and reality.

---

## What is the difference between a resource and a data source?

A strong answer:

> A resource manages the lifecycle of an object, meaning Terraform can create, update, or destroy it. A data source only reads information about an existing object and does not manage that object’s lifecycle.

---

## What is a module in Terraform?

A strong answer:

> A module is a reusable container of Terraform configuration. The root module is the directory you run Terraform from, and child modules are called through module blocks to organize and standardize infrastructure patterns.

---

## Why is `for_each` often preferred over `count`?

A strong answer:

> `for_each` usually gives more stable resource identity because it keys instances by meaningful names instead of numeric positions. With `count`, list reordering can shift indexes and change resource identity unexpectedly.

---

## What does `terraform init` really do?

A strong answer:

> `terraform init` initializes the working directory by installing providers and modules and configuring the backend. It prepares Terraform to evaluate configuration and interact with state and providers.

---

# 10) Open-source references

These references are useful for deeper technical reading:

1. Terraform language documentation  
   https://developer.hashicorp.com/terraform/language

2. Terraform CLI documentation  
   https://developer.hashicorp.com/terraform/cli

3. Terraform configuration syntax  
   https://developer.hashicorp.com/terraform/language/syntax/configuration

4. Terraform providers  
   https://developer.hashicorp.com/terraform/language/providers

5. Terraform resource syntax  
   https://developer.hashicorp.com/terraform/language/resources/syntax

6. Terraform data sources  
   https://developer.hashicorp.com/terraform/language/data-sources

7. Terraform state  
   https://developer.hashicorp.com/terraform/language/state

8. Terraform modules  
   https://developer.hashicorp.com/terraform/language/modules

9. Terraform expressions  
   https://developer.hashicorp.com/terraform/language/expressions

10. Terraform functions  
   https://developer.hashicorp.com/terraform/language/functions

11. Terraform meta-arguments  
   https://developer.hashicorp.com/terraform/language/meta-arguments

12. Terraform workspaces  
   https://developer.hashicorp.com/terraform/language/state/workspaces

13. Terraform CLI commands  
   https://developer.hashicorp.com/terraform/cli/commands

14. Terraform import  
   https://developer.hashicorp.com/terraform/cli/import

15. Terraform state commands  
   https://developer.hashicorp.com/terraform/cli/commands/state

---

# 11) Final revision summary

The most important way to understand Terraform is this:

- Terraform is a **declarative infrastructure engine**
- the code defines **desired state**
- providers connect Terraform to **real APIs**
- state is Terraform’s **memory**
- the graph determines **ordering**
- plan computes the **difference**
- apply performs the **convergence**

If you understand:
- providers
- resources
- data sources
- state
- backends
- modules
- variables
- outputs
- expressions
- graph
- commands

then you understand the technical core of Terraform well.
