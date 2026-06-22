# infra/

The **infra** layer — Infrastructure-as-Code, deployment, and cloud wiring.

- **Default stack:** Terraform (Azure / Databricks / Snowflake / AWS — the platforms Dataside works
  on). Keep environment config and deploy pipelines here.
- **CI:** the `infra` job in [`../.github/workflows/ci.yml`](../.github/workflows/ci.yml) runs
  `terraform fmt -check` + `terraform validate`.
- **Gates (run from this folder):**

  ```bash
  terraform fmt -check -recursive && terraform validate
  ```

> Never commit secrets or state (constitution §5) — use a remote backend and a secret store
> (e.g. Azure Key Vault). **Deploying elsewhere?** Delete this folder.
