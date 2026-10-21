---
applyTo: "**/*.tf"
description: "Terraform code rules. Applied only when a .tf file is in context."
---

# Terraform instructions

## Multiple instances

- NEVER use `count` to create multiple instances of a resource or module.
- ALWAYS use `for_each` over a `map(object({...}))` keyed by a stable instance name:

  ```hcl
  variable "instances" {
    description = "Instances to create, keyed by name."
    type = map(object({
      location = optional(string, "westeurope")
    }))
  }

  resource "azurerm_storage_account" "this" {
    for_each = var.instances
    name     = each.key
    location = each.value.location
    # ...
  }
  ```

- Outputs return maps keyed by instance name: `{ for k, v in <resource>.this : k => v.id }`.

## Style

- One resource type per file where practical (`storage.tf`, `network.tf`, ...).
- Pin provider versions in `versions.tf`.
- Every variable and output has a `description`.
