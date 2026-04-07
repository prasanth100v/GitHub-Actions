# 🚀 11. How can you run Terraform in GitHub Actions using GitOps principles?

---

## 🔍 First, What is GitOps?

GitOps = Git + Automation  
📦 You store infrastructure code (Terraform) in Git. GitHub Actions watches for changes.  
⚙️ When code changes, it automatically runs Terraform to update your cloud (e.g., AWS).

---

## 🌍 Real-life Example: Create an S3 Bucket using Terraform + GitHub Actions

### 🧩 Step 1: Your GitHub Repo:
It has Terraform code to create an S3 bucket  

---

### 🔐 Step 2: Add AWS credentials to GitHub Secrets  
In GitHub repo → Settings → Secrets and variables → Actions → Add:  
- 🔑 AWS_ACCESS_KEY_ID  
- 🔑 AWS_SECRET_ACCESS_KEY  

---

### ⚙️ Step 3: Create GitHub Actions Workflow  

In your repo, create this file:  
📁 .github/workflows/terraform.yml  

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

---

## ⚡ What Happens?

1. 📝 You commit & push your Terraform code to GitHub (main.tf).  
2. 🔔 GitHub Actions detects a push to the main branch.  
3. ⚙️ It:  
   - ▶️ Runs terraform init  
   - 📊 Runs terraform plan (checks what will be created)  
   - 🚀 Runs terraform apply (creates S3 bucket on AWS)  
4. 🤖 No manual step — it’s automatic via GitOps!  

---

## 🎤 In an Interview, Say:

"I followed GitOps principles for infrastructure automation. I stored Terraform code in GitHub and used GitHub Actions to automatically run terraform plan and apply when a PR was merged. This ensured version control, automation, and safe deployments of AWS resources like S3 or EKS."

---

## ✅ This approach ensured:  
- 📚 Version control for infrastructure  
- ⚙️ Automation of provisioning  
- 🔐 Safe and auditable deployments through code review and approval  
- 🚫 It helped us eliminate manual steps and reduce the risk of misconfigurations.  

---

## 🧩 What are composite actions in GitHub Actions? When would you use them?

Composite actions are custom reusable workflows that group multiple steps into a single action.  
🔁 You can define your own custom logic once and reuse it across different workflows.

---

## ❓ Use when:  
- 🔄 You want to avoid repeating the same steps in multiple workflows.  
- 🌍 You want to share logic across repos.  

---

## 📄 Example:

# .github/actions/setup-python/action.yml
name: Setup Python Project  

runs:  
  using: "composite"  
  steps:  
    - uses: actions/checkout@v3  
    - uses: actions/setup-python@v4  
      with:  
        python-version: "3.10"  
    - run: pip install -r requirements.txt  

---

## 🔁 Then reuse it in your workflow:

jobs:  
  setup:  
    runs-on: ubuntu-latest  
    steps:  
      - uses: ./.github/actions/setup-python  

---

## 💡 Bonus Interview Tip:  
Composite actions don’t support runs-on, so they inherit the runner from the calling workflow.  
