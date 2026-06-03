## 🚀 How can you run Terraform in GitHub Actions using GitOps principles?
## 🔍 What is GitOps?

 * ✨ GitOps = Git + Automation
 * 👉 Core idea: Git = `Source of Truth` 📚  Automation = `Apply Changes` ⚙️
 * 📦 You store infrastructure code (`Terraform`) in Git. GitHub Actions watches for changes.
 * ⚙️ When code changes, it automatically runs Terraform to `update` your cloud (e.g., `AWS`, `Azure`).
 * 🎯 How Terraform + GitHub Actions Work Together
      * 👉 You store Terraform code in `GitHub`
      * 👉 Use `GitHub Actions` to run Terraform automatically

## 🌍 Real-life Example: 
 Create an S3 Bucket using Terraform + GitHub Actions

### 🧩 Step 1: Terraform Code in Repo
It has Terraform code to create an S3 bucket  
```yaml
resource "aws_s3_bucket" "demo" {
  bucket = "my-gitops-bucket"
}
```

### 🔐 Step 2: Add AWS credentials to GitHub Secrets  
 * In GitHub repo → Settings → Secrets and variables → Actions → Add:
     * 🔑 `AWS_ACCESS_KEY_ID`
     * 🔑 `AWS_SECRET_ACCESS_KEY`

### ⚙️ Step 3: Create GitHub Actions Workflow  
In your repo, create this file : 📁 `.github/workflows/terraform.yml  `
```yaml
name: Terraform GitOps S3  

on:  
  push:  
    branches:  
      - main  

jobs:  
  terraform:  
    runs-on: ubuntu-latest  

    steps:  
      - name: Checkout code  
        uses: actions/checkout@v3  

      - name: Set up Terraform  
        uses: hashicorp/setup-terraform@v3  

      - name: Terraform Init  
        run: terraform init  

      - name: Terraform Plan  
        run: terraform plan  
        env:  
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}  
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}  

      - name: Terraform Apply  
        run: terraform apply -auto-approve  
        env:  
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}  
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}  
```

## ⚡ What Happens?

1. 📝 You commit & push your Terraform code to `GitHub` (main.tf).  
2. 🔔 GitHub Actions triggers
3. ⚙️ Workflow runs:
    - ▶️ Runs `terraform init`
    - 📊 Runs `terraform plan` (checks what will be created)  
    - 🚀 Runs `terraform apply` (creates S3 bucket on AWS)  
4. 🤖 No manual step — it’s automatic via `GitOps`!
5. 🛡️ Production-Grade GitOps (**IMPORTANT ⭐**)
     - 👉 Don’t directly apply `on push` — 🔄 use safer flow: `PR Created → Plan → Review → Merge → Apply`


## 🎤 In an Interview, Say:
  * I followed GitOps principles for infrastructure automation.
  * I stored Terraform code in GitHub and used GitHub Actions to automatically run `Terraform plan` runs on `pull requests` for safe preview,
  * And `Terraform apply` runs only after merging to `main` branch.
  * This ensures version controlled, automated and safe deployments of AWS resources like S3 or EKS."

---

## ✅ This approach ensured:  
- 📚 Version control for infrastructure  
- ⚙️ Automation of provisioning  
- 🔐 Safe and auditable deployments through code review and approval  
- 🚫 It helped us eliminate manual steps and reduce the risk of misconfigurations.  

---
      
# 🌈🚀 Terraform CI/CD Workflow using GitHub Actions
  * I implement `GitOps` by storing Terraform code in `GitHub` and using `GitHub Actions` to automate deployments.
  * When a pull request is created, Terraform plan runs for `review`, and after `approval and merge`, Terraform apply is triggered.
  * This ensures `version control`, `automation`, and `safe infrastructure` changes.
      * 📌 Runs `terraform plan` on every pull request targeting the `main` branch.
      * 📌 Runs `terraform apply` only when code is pushed to `main` (i.e., after a PR merge).
      * 🔐 Uses AWS OIDC authentication (no static credentials)

```yaml
name: Terraform CI/CD

on:
  pull_request:
    branches: [main]                         # 🔵 Trigger on PRs targeting main branch only
  push:
    branches: [main]                         # 🟢 Trigger on direct pushes or merges to main

permissions:
  id-token: write                            # 🔐 Required for AWS OIDC
  contents: read

env:
  TF_PLUGIN_CACHE_DIR: ~/.terraform.d/plugin-cache       # 📝 Store downloaded providers/plugins in this directory and reuse them in future runs.

jobs:
  terraform:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout                                    # 📥 Checkout code
        uses: actions/checkout@v4

      - name: Setup Terraform                              # ⚙️ Setup specific version of Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: 1.5.0                         # 📌 Version pinned                

      - name: Cache Terraform Plugins 🚀               # Add Terraform Version Cache (Performance Boost 🚀 & Speeds up provider/plugin downloads )
        uses: actions/cache@v3
        with:
          path: ~/.terraform.d/plugin-cache
          key: ${{ runner.os }}-terraform-${{ hashFiles('**/*.tf') }}
          restore-keys: |
            ${{ runner.os }}-terraform-

      - name: Configure AWS Credentials (OIDC)                                      # 🔐 Authenticate to AWS using OpenID Connect (OIDC) – no hardcoded secrets   
        uses: aws-actions/configure-aws-credentials@v4
        with:                                                                        # Requires an IAM role that trusts this GitHub repository.
          role-to-assume: arn:aws:iam::123456789012:role/gha-terraform-role
          aws-region: us-east-1

      - name: Terraform Init                              # 🚀 Initialize Terraform (downloads providers, sets up backend)
        run: terraform init -input=false

      - name: Terraform Format                            # 🎨 Format check
        run: terraform fmt -check

      - name: Terraform Validate                          # ✅ Validate config
        run: terraform validate

      - name: Terraform Plan (PR)                             # 🔍 Plan for PR
        if: github.event_name == 'pull_request'
        run: terraform plan

      - name: Terraform Plan before Apply                   # 🔍 Generate Plan before Apply (runs for both PR + push, but mainly useful for PR review)
        if: github.event_name == 'push'
        run: terraform plan -out=tfplan

      - name: Terraform Apply                                                  # 🚀 Apply only (on push to main branch)
        if: github.event_name == 'push' && github.ref == 'refs/heads/main'
        run: terraform apply -auto-approve tfplan
```
  * 🚀 High-Level Idea
  * This workflow follows GitOps:
      * 👨‍💻 Developer creates `PR` → Preview infra changes
      * ✅ Merge to main → `Deploy infra` automatically

## Optinal 
```yaml
jobs:
  terraform:
    runs-on: ubuntu-latest

    defaults:
      run:
        working-directory: .   # 🔁 Change if using subfolder like ./infra
```

---

## 🧩 What are composite actions in GitHub Actions? When would you use them?

 * Composite actions are `custom reusable workflows` that group `multiple steps` into a `single action`.
 * 🔁 You can define your `own custom logic` once and `reuse` it across different `workflows`.
 * ❓ Use when:
     * 🔄 You want to avoid `repeating the same steps` in multiple workflows.
     * 🌍 You want to `share logic` across repos.
     * ✨ I want to simplify workflows and make them `cleaner` and `more modular`.  

## 📄 Example:
  # 📁 `.github/actions/setup-python/action.yml`
```YAML
name: Setup Python Project  

runs:  
  using: "composite"  
  steps:  
    - uses: actions/checkout@v3  
    - uses: actions/setup-python@v4  
      with:  
        python-version: "3.10"  
    - run: pip install -r requirements.txt  
```

## 🔁 Then reuse it in your workflow:
```YAML
jobs:  
  setup:  
    runs-on: ubuntu-latest  
    steps:  
      - uses: ./.github/actions/setup-python  
```

## 💡 Bonus Interview Tip:  
Composite actions `don’t support runs-on`, Use runner from the calling workflow.  


---

# ⚡ How to Cache Dependencies in GitHub Actions ?
## 🚀 What is Caching?
  * 👉 Save `dependencies` to avoid reinstalling every time
  * Caching is used to speed up `GitHub workflows` by saving files like package dependencies so they don’t have to be reinstalled every time a workflow runs.
  * 📦 Use actions/cache to store and reuse dependencies (like `npm modules`, `Python packages`).
  * ❓ Why:
      * 🚀 Speeds up build time
      * 📉 Avoids re-downloading on every run  

## Terraform Version Cache 
```hcl
      - name: Cache Terraform Plugins 🚀               # Add Terraform Version Cache (Performance Boost 🚀 & Speeds up provider/plugin downloads )
        uses: actions/cache@v3
        with:
          path: ~/.terraform.d/plugin-cache
          key: ${{ runner.os }}-terraform-${{ hashFiles('**/*.tf') }}
          restore-keys: |
            ${{ runner.os }}-terraform- 
```

---

### 🏁 Final Summary

 * ✨ GitOps → Git as source of truth
 * ✨ GitHub Actions → Automation engine
 * ✨ Terraform → Infra provisioning
 * ✨ Composite Actions → Reusability

