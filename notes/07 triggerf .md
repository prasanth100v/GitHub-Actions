# 🚀 What is on: in GitHub Actions?
 * The `on:` keyword tells GitHub when to run your workflow.
 * Think of it as the trigger for your `CI/CD pipeline`.

### 🔔 GitHub Actions Triggers
#### 👉 push 📤 | PR 🔀 | manual ▶️ | cron ⏰
| 🔔 Trigger                | 🧾 Syntax                            | 📖 When It Runs                                        | 💡 Use Case                    |
| ------------------------- | ------------------------------------ | ------------------------------------------------------- | -------------------------------- |
| 📤 **push**               | `on:<br>push: branches:<br>  [main]`         | 🚀 When code is pushed directly to `main` branch       | 🚀 Auto deploy after code merge  |
| 🔀 **pull_request**       | `on: pull_request: branches: [main]` | 🔍 When PR is opened/updated/reopened targeting `main` | ✅ Code validation before merge |
| 🖱️ **workflow_dispatch** | `on: workflow_dispatch:`             | ▶️ Manually triggered via **Run workflow** button       | 🎯 Manual deploy/testing       |
| ⏰ **schedule**            | `on: schedule: - cron: "0 3 * * *"`  | 🌙 Runs at fixed time (cron schedule)                  | 📊 Nightly jobs, backups       |

| 🔔 Trigger Combination | 🧾 Syntax                                                                            | 📖 Behavior                                               |
| ---------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------- |
| 📤 + 🔀 + 🖱️          | `yaml on: push: branches: [main] pull_request: branches: [main] workflow_dispatch: ` | Runs on push to `main`, PRs to `main`, and manual trigger |


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
