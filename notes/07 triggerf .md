# 🚀 What is on: in GitHub Actions?

The on: keyword tells GitHub when to run your workflow.  
Think of it as the trigger for your CI/CD pipeline.

---

## 🔔 Common Triggers

- 📤 push – When someone pushes code directly to the main branch.  
- 🔀 pull_request – When someone opens, updates, or reopens a pull request into the main branch.  
- 🖱️ workflow_dispatch – When you run the workflow manually (button)  
- ⏰ schedule – Runs at specific time like cron (e.g., nightly)  

---

## 📤 Push Trigger

on:  
  push:  
    branches: [main]  

Runs when code is pushed to main branch.

---

## 🔀 Pull Request Trigger

on:  
  pull_request:  
    branches: [main]  

Runs when someone opens or updates a pull request targeting main.

---

Pull requests give teams a safe, controlled way to update code, improve quality, and collaborate.  

It’s like saying:  
“Here’s my work — can someone check and approve before we add it to the main Branch?”

---

## 🖱️ Manual Trigger (workflow_dispatch)

on:  
  workflow_dispatch:  

Adds a "Run workflow" button in the GitHub UI. Useful for manual deployments.

---

## ⏰ Scheduled Trigger (like cron job)

on:  
  schedule:  
    - cron: "0 3 * * *"   # Runs every day at 3 AM UTC  

Good for backups, health checks, or daily reports.

---

## 🔁 Multiple Triggers Together

on:  
  push:  
    branches: [main]  
  pull_request:  
    branches: [main]  
  workflow_dispatch:  

This workflow runs:  
on push to main, on pull requests to main, when triggered manually  

---

# 🔐 Secret

Encrypted environment variables stored in GitHub → Settings → Secrets.

### 🧾 Example:

with:  
  aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}  

---

# ⚠️ continue-on-error: true in GitHub Actions

continue-on-error: true allows a step or job to continue execution even if the previous step/job fails.

❌ Without continue-on-error: true: If the workflow stops there and won’t run.  

⚠️ Be Careful: Use it only when failure is acceptable. It can hide problems if overused.

---

- 🧩 A composite action bundles multiple steps in one action, making it reusable.  
- 🐞 To debug GitHub Actions workflows, I start by checking the step-by-step logs in the ‘Actions’ tab. Each step has detailed output, and failed steps are clearly marked. Use echo or printenv to print variable values. Enable debug logging with ACTIONS_STEP_DEBUG=true.  
- ⚡ The difference between actions/checkout@v2 and actions/checkout@v3 lies in improvements in performance, security, and new features.  
- 🌐 GitHub Actions provides native GitHub integration, reusable workflows, strong community support, and flexible automation.  
- 🔒 GitHub masks secrets automatically in logs. If a secret is printed, it is replaced with ***.  

---

# 🐳 What is the purpose of job.container in GitHub Actions?

The job.container keyword in GitHub Actions allows you to run a job inside a Docker container, instead of the default GitHub-hosted runner environment. It helps standardize the runtime environment for builds and tests.

### 🧾 Example:

jobs:  
  test:  
    runs-on: ubuntu-latest  
    container:  
      image: node:18  
    steps:  
      - uses: actions/checkout@v3  
      - run: node -v   # runs inside the node:18 container  

---

# 🔄 What is concurrency in GitHub Actions?

concurrency is a GitHub Actions feature used to ensure that only one job or workflow using the same concurrency group runs at a time. It helps avoid duplicate runs, cancel in-progress jobs, and save resources.

## 📌 Real Use Case:

If you push code multiple times quickly to the same branch (main), only the latest run will proceed—previous ones are canceled. With cancel-in-progress: true, we could automatically cancel outdated runs and keep CI/CD pipelines efficient.

## 🧾 Example:

concurrency:  
  group: production-${{ github.ref }}  
  cancel-in-progress: true  
