# 🚀 What is on: in GitHub Actions?
 * The `on:` keyword tells GitHub when to run your workflow.
 * Think of it as the trigger for your `CI/CD pipeline`.

### 🔔 GitHub Actions Triggers
#### 👉 push 📤 | PR 🔀 | manual ▶️ | cron ⏰
| 🔔 Trigger                | 🧾 Syntax                                   | 📖 When It Runs                                        | 💡 Use Case                    |
| ------------------------- | ------------------------------------------- | ------------------------------------------------------- | -------------------------------- |
| 📤 **push**               | on:<br>push:<br> branches: [`main`]         | 🚀 When code is pushed directly to `main` branch       | 🚀 Auto deploy after code merge  |
| 🔀 **pull_request**       | on:<br> pull_request:<br> branches: [`main`] | 🔍 When PR is opened/updated/reopened targeting `main` | ✅ Code validation before merge |
| 🖱️ **workflow_dispatch** | on:<br> `workflow_dispatch:`                  | ▶️ Manually triggered via **Run workflow** button       | 🎯 Manual deploy/testing       |
| ⏰ **schedule**          | on:<br> schedule:<br> - `cron: "0 3 * * *"`    | 🌙 Runs at fixed time (cron schedule)                  | 📊 Nightly jobs, backups       |


### 🔁 Multiple Triggers Combined
| 🔔 Trigger Combination | 🧾 Syntax                                                                                          | 📖 Behavior                                                         |
| ---------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| 📤 + 🔀 + 🖱️          | on:<br>  push:<br>   branches: [`main`]<br>  pull_request:<br>   branches: [`main`]<br>  `workflow_dispatch:` | Runs on push to `main`, pull requests to `main`, and manual trigger |


---

# ⚠️ `continue-on-error: true` in GitHub Actions
  * `continue-on-error: true` allows a step or job to continue execution even if the previous `step/job` fails.
  * ❌ Without `continue-on-error: true:` If the workflow stops there and won’t run.
  * ⚠️ Be Careful: Use it only `when failure is acceptable`. It can hide problems if overused.

---

### 🧩 GitHub Actions Concepts
| 🧩 Topic                       | 📖 Explanation                                                                                                                                                  |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 🧩 **Composite Action**        | A composite action bundles multiple steps into a single reusable action. 👉 Helps avoid repeating the same steps across workflows and improves maintainability. |
| 🐞 **Debugging Workflows**     | To debug, check logs in the **Actions tab**. 👉 Each step shows detailed output, and failed steps are clearly highlighted, making issues easier to identify.    |
| ⚡ **checkout@v2 vs v3**        | The difference between `actions/checkout@v2` and `v3` includes improved performance, better security, and support for newer features in v3.                     |
| 🌐 **GitHub Actions Benefits** | Provides native integration with GitHub, reusable workflows, strong community support, and flexible automation for CI/CD pipelines.                             |
| 🔒 **Secrets Handling**        | GitHub automatically masks secrets in logs. 👉 If a secret appears in output, it is replaced with `***` to prevent exposure.                                    |

---

## 🐳 What is the purpose of `job.container` in GitHub Actions?

 * 🚀 `Runs job inside a Docker container`
 * The `job.container` keyword in GitHub Actions allows you to run a `job` inside a Docker container, instead of the default GitHub-hosted runner environment.
 * It helps standardize the runtime environment for `builds` and `tests`.

### 🧾 Example:
```
jobs:  
  test:  
    runs-on: ubuntu-latest  
    container:  
      image: node:18  
    steps:  
      - uses: actions/checkout@v3  
      - run: node -v                          # runs inside the node:18 container  
```

---

# 🔄 What is concurrency in GitHub Actions?

 * 🚀 Controls `how many workflows` run at the same time
 * It helps avoid `duplicate runs`, `cancel in-progress jobs`, and `save resources`.
 * 📌 Real Use Case:
     * If you push code `multiple times quickly` to the same branch (main), only the `latest run` will proceed—previous ones are canceled.
     * With `cancel-in-progress: true`, we could automatically cancel outdated runs and keep CI/CD pipelines `efficient`, especially for production pipelines.

## 🧾 Example:
```
concurrency:  
  group: production-${{ github.ref }}  
  cancel-in-progress: true  
```

## 🏁 Final Summary

 * ✨ on: → Trigger workflow
 * ✨ Secrets → Secure data
 * ✨ continue-on-error → Flexibility
 * ✨ concurrency → Control execution
