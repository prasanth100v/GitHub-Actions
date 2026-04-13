# 🎯 GitHub Actions Terminologies : Key Concepts Explained

## 🔁 1. Workflow
 * 🎯 A workflow is an automation pipeline (CI/CD) defined in a `.yml` file inside `.github/workflows/.` directory
 * That runs based on triggers like `push` or `pull requests`, It tells GitHub what to do and when.

### 🧾 Example:
```
name: Build and Deploy  
on: push  
```
### 🎯 Use Case:
  * 🚀 Automatically test your code every time someone pushes code to the repo.
  * ⚡ A trigger that starts a workflow (e.g., `push`, `pull_request`, `Manual release`, `workflow_dispatch`).

---

## ⚙️ Trigger (on): Defines when the workflow runs.

### 📌 Common triggers:  
| 🧩 Trigger               | 💡 Description                                               |
| ------------------------ | ------------------------------------------------------------- |
| 📤 `push`                | 🚀 Runs when code is pushed to a branch                      |
| 🔀 `pull_request`        | 🔍 Runs on PR creation or updates                            |
| ⏰ `schedule`            | 🕒 Runs automatically using cron schedule  (Run every night) |
| 🖱️ `workflow_dispatch`   | ▶️ Manual trigger from GitHub UI                             |

### 🧾 Example:
```
on:  
  push:  
    branches:              # 🎯 Run CI when code is pushed to the main branch.
      - main  
```

---

## 🧩 2. Job

 * A job is a `group of steps` that run on the same machine (`runner`).
 * You can run multiple jobs in `parallel` or `sequentially`.

### 🧾 Example:
```
jobs:  
  build:  
    runs-on: ubuntu-latest  
```
### 🎯 Use Case:
One job for build, another for test, and another for deploy. For example:  
| 🧩 Job    | 💡 Purpose                             |
| --------- | -------------------------------------- |
| 🏗️ Build | 🔨 Compiles code / creates artifacts   |
| 🧪 Test   | 🧬 Runs test cases (unit, integration) |
| 🚀 Deploy | 🌐 Pushes application to server/cloud  |
 

---

## 🔹 3. Step

* A step is a `single task` within a job.
* Steps are run one after another.

### 🧾 Example:
```
steps:  
  - name: Checkout Code  
    uses: actions/checkout@v3  

  - name: Run Tests  
    run: npm test  
```
### 🎯 Use Case:
  Check out your repo, install packages, and run tests `step-by-step`.

---

## 🖥️ 5. Runner

 * A runner is a virtual machine where your job runs.
 * GitHub provides free hosted runners (`Ubuntu`, `Windows`, `macOS`).
 * You can also use your own `self-hosted runner`.
 * A runner is the `execution environment` where jobs run. It can be `GitHub-hosted` or `self-hosted`.

| 🧩 Type           | 💡 Description                              |
| ------------------ | ------------------------------------------- |
| ☁️ Hosted         | 🌐 GitHub-managed runners (no setup needed) |
| 🖥️ Self-hosted    | 🛠️ Runs on your own server/infrastructure   |

### 🧾 Example:
```
runs-on: ubuntu-latest      # 🎯 Use `ubuntu-latest` to run Linux shell scripts.
```

---

## 🧱 6. Action
  * An action is a `reusable component` that performs a specific task, such as `checking out code` or `setting up an environment` (e.g., actions/checkout@v4).
  * Can be created by` GitHub`, the `community`, or `yourself`.
  * Used with `uses:` keyword.

### 🧾 Example:
```
uses: actions/checkout@v3           # 🔄 This action checks out your GitHub repo code.  

uses: actions/setup-node@v3         # ⚙️ installs Node.js. 
with:  
  node-version: "18"  
```

### 🎯 Use Case:
  * 📥 `actions/checkout`    →  clones your repo.
  * ⚙️ `actions/setup-node`  →  installs Node.js.
  * 💡 You can even write `custom actions` to `reuse` across multiple projects.

---

## 🏁 Final Summary

 * ✨ Workflow → Full pipeline
 * ✨ Trigger → Start event
 * ✨ Job → Group of steps
 * ✨ Step → Single task
 * ✨ Runner → Execution machine
 * ✨ Action → Reusable logic


