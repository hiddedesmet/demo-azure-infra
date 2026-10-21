# AGENTS.md

Always-on guidance for AI agents working in this repository.

## Repository

- Azure infrastructure defined in Terraform (`azurerm` provider ~> 4.0).
- Reusable modules live in `modules/<name>/`; environments live in `envs/<env>/`.

## Conventions

- Resource names: `<type-abbreviation>-<workload>-<env>-<region>` (e.g. `st-demo-dev-weu`).
- Default region: `westeurope`.
- Every resource that supports tags gets at least these tags:
  - `environment`
  - `owner`
  - `costcenter` (format `CC-1234`)
- Never hardcode secrets; reference Key Vault or variables marked `sensitive = true`.

## Workflow

- Run `terraform fmt -recursive` and `terraform validate` after changes.
- Do not run `terraform apply`.
