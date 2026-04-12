# 🎯 GitHub Actions Terminologies : Key Concepts Explained

📂 Workflows are defined in YAML files (.yml) inside the .github/workflows/ directory.

---

## 🔁 1. Workflow

A workflow is an automation pipeline defined in a .yml file inside .github/workflows/.  
It tells GitHub what to do and when.

### 🧾 Example:
name: Build and Deploy  
on: push  

### 🎯 Use Case:
Automatically test your code every time someone pushes code to the repo.  
⚡ A trigger that starts a workflow (e.g., push, pull_request, release, workflow_dispatch).

---

## ⚙️ Trigger (on): Defines when the workflow runs.

### 📌 Common triggers:  
- 📤 push: Runs when you push code  
- 🔀 pull_request: Runs on PR creation/update  
- ⏰ schedule: Runs on a cron schedule  
- 🖱️ workflow_dispatch: Manual trigger  

---

## 🧩 2. Job

A job is a group of steps that run on the same machine (runner).  
You can run jobs in parallel or sequentially.

### 🧾 Example:
jobs:  
  build:  
    runs-on: ubuntu-latest  

### 🎯 Use Case:
One job for build, another for test, and another for deploy. For example:  
- 🏗️ build job → compiles code  
- 🧪 test job → runs test cases  
- 🚀 deploy job → pushes to a server  

---

## 🔹 3. Step

A step is a single task within a job.  
Steps are run one after another.

### 🧾 Example:
steps:  
  - name: Checkout Code  
    uses: actions/checkout@v3  

  - name: Run Tests  
    run: npm test  

### 🎯 Use Case:
Check out your repo, install packages, and run tests step-by-step.

---

## 🖥️ 5. Runner

A runner is a virtual machine where your job runs.  
GitHub provides free hosted runners (Ubuntu, Windows, macOS).  
You can also use your own self-hosted runner.

### 🧾 Example:
runs-on: ubuntu-latest  

### 🎯 Use Case:
Use ubuntu-latest to run Linux shell scripts.  
Use self-hosted to deploy to a private cloud server.

---

## 🧱 5. Action

A reusable unit of code that performs a task (e.g., actions/checkout@v4).  
Can be created by GitHub, the community, or yourself.  
Used with uses: keyword.

### 🧾 Example:

(1)  
uses: actions/checkout@v3  
🔄 This action checks out your GitHub repo code.  

(2)  
uses: actions/setup-node@v3  
with:  
  node-version: "18"  

### 🎯 Use Case:
- 📥 actions/checkout → clones your repo.  
- ⚙️ actions/setup-node → installs Node.js.  

💡 You can even write custom actions to reuse across multiple projects.

---

## 🔁 6. Trigger (on)

Defines when the workflow runs.

### 🧾 Example:
on:  
  push:  
    branches:  
      - main  

### 🎯 Use Case:
Run CI when code is pushed to the main branch.

### 📌 Other triggers:  
- 🔀 pull_request: On PR creation  
- ⏰ schedule: Run every night (cron)  
- 🖱️ workflow_dispatch: Manually triggered  
