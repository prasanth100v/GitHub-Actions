# 🛑 Manual Approval for Production
   * 👉 Prevent accidental deployments to production
   * I use GitHub Environments with required `reviewers` to enforce `manual approval` before production deployments, ensuring safe and controlled releases.
   * Flow : `Deploy Trigger → Waiting for Approval 🛑 → Approved ✅ → Deployment 🚀`

```yaml
deploy-prod:
  environment:
    name: prod
    url: https://yourapp.com
    reviewers:
      - your-team
```

---

# 📦 How do you share artifacts between jobs?

 * I Use `upload-artifact` and `download-artifact` to share build outputs between jobs.
 * 🚀 Why Use Artifacts? : 👉 Share files between jobs (`build → deploy`)
 * `Build Job → Upload Artifact 📦 → Next Job → Download 📥 → Use`

### 📤 Upload Artifact
```hcl
- uses: actions/upload-artifact@v3
  with:
    name: build
    path: dist/
```
### 📥 Download Artifact In another job :
```hcl
- uses: actions/download-artifact@v3
  with:
    name: build
```
---

# 🔁 Can you run jobs sequentially or conditionally?
 * Yes, in GitHub Actions, we can run jobs both sequentially and conditionally.
 * ⏭️ For sequential execution, I use the `needs keyword` to define dependencies between jobs.
 * ⚡ For example, in a typical CI/CD pipeline, I run `build → test → deploy`, where each job waits for the previous one to succeed.
 * 🔀 For conditional execution, I use the `if condition`. For instance, I run deployment jobs only when the branch is `main` or `staging`, using a condition like:
     * `if: github.ref == 'refs/heads/main'` 🚫 This helps prevent unwanted deployments from feature branches.
 * ⚙️ Combining `needs and if` gives us full control over job flow based on environment or event triggers.
 * 👉 I use `needs` for sequential execution and `if conditions` to control when jobs run, such as deploying only from main or staging branches.

## 🚀 needs vs if in GitHub Actions
| 🧩 **Keyword** | 🎯 **Purpose**                | 🧠 **How It Works**                                                    | 💡 **Example**                       |
| -------------- | ----------------------------- | ----------------------------------------------------------------------- | ------------------------------------- |
| 🔗 **`needs`** | Control job execution order   | Makes a job wait until one or more dependent jobs complete successfully | `needs: build`                        |
| ❓ **`if`**     | Control conditional execution | Runs a job or step only if the specified condition evaluates to `true`  | `if: github.ref == 'refs/heads/main'` |

## ✨ Optional Add-on (Real-time project):
   * 📝 In one of my projects, we had separate workflows for `develop`, `staging`, and `main`.
   * ⚡ We used `if conditions` to check branch names and deployed to respective EKS clusters.
   * ⚡ And `needs` ensured that deployment happened only after successful `builds and tests`.

---

## 🎯 Scenario Overview:

### 🌍 Real-World Multi-Environment Deployment (Branch-Based) 
| 🌿 Branch    | 🧩 Environment | 🚀 Deployment Target     |
| ------------ | -------------- | -------------------------- |
| 🌱 `develop` | 🛠️ Dev        | ☸️ Deploy to Dev EKS     |
| 🧪 `staging` | 🔍 Staging     | ☸️ Deploy to Staging EKS |
| 🚀 `main`    | 🏢 Production  | ☸️ Deploy to Prod EKS    |

 * 🔄 Workflow Logic : `Push Code → Detect Branch → Deploy to Matching Environment`
 * You used `if conditions` to detect the branch and run the corresponding deployment logic.

# 📄 Example Workflow
## 🚀 CI/CD Pipeline to Deploy into Amazon EKS
```yaml
name: CI/CD to EKS

on:
  push:
    branches:
      - develop           # 🌱 Dev environment
      - staging           # 🧪 Staging environment
      - main              # 🚀 Production environment

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code                                            # 📥 Step 1: Checkout repository code
        uses: actions/checkout@v3

      - name: Set up kubectl                                           # ⚙️ Step 2: Install kubectl CLI
        uses: azure/setup-kubectl@v3

      - name: Configure AWS credentials                                 # 🔐 Step 3: Configure AWS credentials using IAM Role (OIDC)
        uses: aws-actions/configure-aws-credentials@v3
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_NAME>          # 🎯 Replace with your IAM Role
          aws-region: us-east-1                                               # 🌎 AWS Region

      - name: Deploy to Dev EKS                                   # 🌱 Deploy to Development Environment
        if: github.ref == 'refs/heads/develop'
        run: |
          echo "Deploying to dev EKS"
          kubectl apply -f k8s/dev/

      - name: Deploy to Staging EKS                               # 🧪 Deploy to Staging Environment
        if: github.ref == 'refs/heads/staging'
        run: |
          echo "Deploying to staging EKS"
          kubectl apply -f k8s/staging/

      - name: Deploy to Prod EKS                                   # 🚀 Deploy to Production Environment
        if: github.ref == 'refs/heads/main'
        run: |
          echo "Deploying to prod EKS"
          kubectl apply -f k8s/prod/ 

```
### What Happens?
   * 📝 Push code
   * 🔔 Workflow triggers
   * 🔍 Checks branch
   * 🚀 Deploys to correct environment

### 🔐 Best Practices Used
   * 🔑 `OIDC` for AWS authentication
   * 🔐 GitHub Secrets for `credentials`
   * 🛑 Manual approval for production
   * ⚡ `needs + if` for control
   * 📦 `Artifacts` for sharing


## 🎯 Why this is good (for interviews):
### 🧱 Environment Isolation
  * 🔒 Changes made in the development (dev) environment do not directly impact the production (live) environment.
  * 🧪 Enables safe, independent testing in each environment.
  * We used a `GitOps approach` where each branch mapped to an environment, ensuring controlled deployments and clear separation between `dev`, `staging`, and `production`.

### 🚀 Safe Releases
 - 🔄 Code flows through `dev → staging → main (prod)`, so you can catch issues and 🐞 Bugs early in lower environments.  

## ⚙️ Clean GitOps-style Automation
  - 🌿 Branch = Source of Truth for a particular environment  
  - 🤖 GitHub Actions = Deployment orchestrator  (`Automation engine`)

## 🎤 Interview Highlights You Can Mention:

 * 🔍 Used `github.ref in if` : to detect current branch, 👉 Used to control deployments.
 * ☁️ Environment-Based Access
      * Separate EKS clusters per environment
      * Role-based access control
 * 📁 Kubernetes manifests stored in separate folders (`k8s/dev`, `k8s/staging`, `k8s/prod`).
 * 🔐 Used aws-actions/configure-aws-credentials to assume IAM roles `OIDC (no hardcoded keys)`.
 * 🛠️ Used kubectl CLI to apply manifests.  

## 💡 Bonus Tips for Advanced Setups:

- 🛑 Use `environments + protection rules` for approvals  
- 🧩 Split jobs into separate workflows using on.push.branches `branch-based triggers `.
- 🔑 Use `OIDC` instead of static AWS credentials  

---

## ⚠️ What’s the use of `continue-on-error` in a GitHub Action step?
  🚀 It allows the workflow to continue even if a step fails.
```hcl
- name: Run optional lint  
  run: npm run lint  
  continue-on-error: true  
```

---

## 🏁 Final Summary

 * ✨ Environments → Control deployments
 * ✨ Artifacts → Share data
 * ✨ needs → Order execution
 * ✨ if → Conditional logic
 * ✨ continue-on-error → Flexibility
