---
name: scaffold-module
description: Scaffold a new reusable Terraform module under modules/<name>/ with versions, variables, main, outputs and README files. Use when the user asks to create, scaffold or bootstrap a new Terraform module.
---

# Scaffold a Terraform module

## Inputs

- `name`: module name (kebab-case), e.g. `storage-account`.
- `resource`: primary `azurerm` resource, e.g. `azurerm_storage_account`.

## Steps

1. Create `modules/<name>/` with exactly these files, using the templates below.
2. Replace `<resource>` and `<name>` placeholders.
3. Run `terraform fmt -recursive modules/<name>`.

## Templates

### versions.tf

```hcl
terraform {
  required_version = ">= 1.6"
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
  }
}
```

### variables.tf

```hcl
variable "name" {
  description = "Name of the resource."
  type        = string
}

variable "resource_group_name" {
  description = "Resource group to deploy into."
  type        = string
}

variable "location" {
  description = "Azure region."
  type        = string
  default     = "westeurope"
}

variable "environment" {
  description = "Deployment environment (dev, tst, prd)."
  type        = string
}

variable "owner" {
  description = "Owning team."
  type        = string
}

variable "costcenter" {
  description = "Cost center code (CC-1234)."
  type        = string
}
```

### main.tf

```hcl
resource "<resource>" "this" {
  name                = var.name
  resource_group_name = var.resource_group_name
  location            = var.location

  tags = {
    environment = var.environment
    owner       = var.owner
    costcenter  = var.costcenter
  }
}
```

### outputs.tf

```hcl
output "id" {
  description = "Resource ID."
  value       = <resource>.this.id
}
```

### README.md

````markdown
# <name>

Creates a `<resource>`.

## Usage

```hcl
module "<name>" {
  source              = "../../modules/<name>"
  name                = "<name>-demo-dev-weu"
  resource_group_name = "rg-demo-dev-weu"
  environment         = "dev"
  owner               = "platform-team"
  costcenter          = "CC-1234"
}
```
````
