# 🧩 When Would You Use Composite Actions?

I would use composite actions when:  
- 🔁 The same logic is repeated in multiple workflows.  
- 🧱 I want to create a custom reusable action that combines multiple tools.  
- ✨ I want to simplify workflows and make them cleaner and more modular.  

---

# ⚡ How to Cache Dependencies in GitHub Actions ?

Caching is used to speed up GitHub workflows by saving files like package dependencies so they don’t have to be reinstalled every time a workflow runs.

📦 Use actions/cache to store and reuse dependencies (like npm modules, Python packages).

## ❓ Why:  
- 🚀 Speeds up build time  
- 📉 Avoids re-downloading on every run  

---

## 🐍 Python Example (pip)

- name: Cache pip  
  uses: actions/cache@v3  
  with:  
    path: ~/.cache/pip  
    key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}  
    restore-keys:  
      ${{ runner.os }}-pip-  

---

# 🔗 What is jobs.<job_id>.needs in GitHub Actions?

In GitHub Actions, jobs.<job_id>.needs is used to control the order of job execution by creating dependencies between jobs.

## ⚙️ What it Does:
It tells GitHub Actions: “This job should only run after the specified job(s) complete successfully.”

---

## 📄 Real-Time Example:

Suppose you're deploying only after successful build and test:

jobs:

  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build completed"

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Tests passed"

  deploy:
    needs: [build, test]
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deployed to production"

---

## 📌 In this case:

- ⛔ test won’t run unless build passes → needs: build  
- ✅ deploy won’t run unless both build and test succeed → needs: [build, test]  

---

## 💡 Why It’s Useful:

- 🧩 Helps organize multi-step CI/CD pipelines  
- 🚫 Prevents unnecessary job execution if dependencies fail  
- ⚡ Enables parallel or conditional workflows  

---

# ⚖️ What is the difference between run, uses, and with?

🟢 run   – Run shell commands in runner  
🔵 uses  – Use a prebuilt GitHub Action  
🟣 with  – Provide inputs to the action  

## 📄 Example:  
- run: echo "Hello"  
- uses: actions/checkout@v3  
- with: python-version: 3.9  

---

# 🚀 Real-World CI/CD Interview Questions

## 🎯 You need to deploy a Docker app to production only if tests pass. How will you design the GitHub Actions workflow?

Use multiple jobs with needs: so that the build-and-push and deploy jobs run only if the test job is successful.

---

## 🔐 How would you avoid hardcoding secrets in your GitHub workflow?

Use GitHub Secrets stored under repository settings, and reference them with secrets.<SECRET_NAME>.

📌 For example:  
${{ secrets.DOCKER_PASSWORD }}

---

## ⏱️ Your builds are taking too long. What would you do?

- 📦 Use caching for dependencies (actions/cache)  
- 🔄 Avoid duplicate steps  
- ⚡ Run jobs in parallel  
- 🪶 Use lightweight runners if possible  

---

## 🌍 How would you manage deployments across different environments (dev, staging, prod)?

Use environment-specific jobs or matrices, with manual approval for production.

---

## ☁️ You want to deploy to AWS securely from GitHub Actions. How?

Use OIDC (OpenID Connect) with IAM roles (IRSA) to give temporary credentials to GitHub instead of storing long-term AWS credentials.

---

## 🔑 How does GitHub Actions authenticate Terraform to AWS?

By using OIDC with the aws-actions/configure-aws-credentials action.

---

## 🧹 Why use terraform fmt and validate in CI?

To ensure consistent formatting and catch syntax/config errors early.

---

## 📦 What is uses in GitHub Actions?

It refers to a pre-built action or reusable component.

---

## 🔒 How do you handle secrets safely in a GitHub Actions pipeline?

- 🔐 Store them in GitHub Secrets  
- 🚫 Never hardcode credentials in YAML  
- 🔑 Use secrets.* syntax  

💡 Tip: You can also use OIDC + AWS IAM roles for better security.  
