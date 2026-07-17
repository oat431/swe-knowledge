---
document_type: Infrastructure-as-Code
version: "1.0"
status: Active
author: "[DevOps Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [iac, terraform, infrastructure, swebok]
standard_ref:
  - SWEBOK v4 — Operations
---

# Infrastructure-as-Code

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines infrastructure as version-controlled code — ensuring reproducible, auditable, and automated infrastructure management.

## 2. IaC Stack

| Tool | Purpose | Version |
|------|---------|---------|
| [Terraform] | [Infrastructure provisioning] | [v1.x] |
| [Helm] | [Kubernetes package management] | [v3.x] |
| [Ansible] | [Configuration management] | [v2.x] |

## 3. Terraform Configuration

### Main Configuration

```hcl
# main.tf
terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  backend "s3" {
    bucket = "project-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "ap-southeast-1"
  }
}

provider "aws" {
  region = var.aws_region
}

module "vpc" {
  source = "./modules/vpc"
  environment = var.environment
}

module "eks" {
  source = "./modules/eks"
  environment = var.environment
  vpc_id = module.vpc.vpc_id
}

module "rds" {
  source = "./modules/rds"
  environment = var.environment
  vpc_id = module.vpc.vpc_id
}
```

### Variables

```hcl
# variables.tf
variable "environment" {
  description = "Environment name"
  type        = string
  default     = "production"
}

variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "ap-southeast-1"
}
```

## 4. Directory Structure

```
infra/
├── main.tf
├── variables.tf
├── outputs.tf
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── eks/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── rds/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
└── environments/
    ├── dev.tfvars
    ├── staging.tfvars
    └── prod.tfvars
```

## 5. Operations

| Operation | Command | When |
|---------|---------|------|
| [Plan] | [terraform plan -var-file=prod.tfvars] | [Before changes] |
| [Apply] | [terraform apply -var-file=prod.tfvars] | [After review] |
| [Destroy] | [terraform destroy -var-file=prod.tfvars] | [Never in prod] |
| [State List] | [terraform state list] | [Debugging] |
| [Import] | [terraform import] | [Existing resources] |

## 6. State Management

| Aspect | Configuration |
|--------|--------------|
| [Backend] | [S3 with DynamoDB locking] |
| [Encryption] | [AES-256] |
| [Versioning] | [Enabled] |
| [Access] | [IAM role-based] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Container-Configurations]] | Container specs |
| [[Deployment-Plan]] | Deployment procedures |
| [[Capacity-Plan]] | Infrastructure sizing |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** If it's not in code, it doesn't exist. Manual infrastructure changes will drift. Always use IaC.
