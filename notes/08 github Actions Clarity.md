## ⚡ What is `strategy.fail-fast: true` in GitHub Actions?
 * fail-fast is a setting inside a job’s strategy that determines what happens when one of the matrix job combinations `fails`.
 * 🎯 Purpose
    * In matrix builds, multiple combinations (e.g., `different OS` or `Node versions`) run in parallel.

| Setting               | Result                                                     |
| --------------------- | ----------------------------------------------------------- |
| ✅ `fail-fast: true`   |cancels all other matrix jobs as soon as one fails.         |
| 🔄 `fail-fast: false` | even if one job fails, the others will continue running.   |


## 🧾 Syntax Example:
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: true
      matrix:
        node: [14, 16, 18]

    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: npm test
```

---

## 🚨 How Do You Handle Errors in GitHub Actions?

 * In GitHub Actions, I handle errors using several methods. ❌ By Default: workflow stops on failure, but I use `continue-on-error` when failure is acceptable.
 * ⚠️ I also control flow using `if: failure()` or `if: success()` to handle post-failure actions like `notifications`.
 * 💡 example :
     * In a security scan job, I set `continue-on-error: true` for the `Trivy scan` step.
     * This allows the workflow to continue while still capturing `vulnerabilities in the logs`, which I `review` later.
     * 🧠 Why? `🛡️ Capture vulnerabilities 🚀 Don’t block deployment unnecessarily`

---

## 🚀 How can you `optimize GitHub Actions for performance`?

 * To optimize GitHub Actions for performance, the goal is to `reduce build time`, `save costs`, and `improve reliability`.
 * To optimize GitHub Actions, I use
     * 📦 Caching dependencies
     * 🔀 Parallel execution with `matrix strategy`
     * 🎯 limit workflows to specific branches or events
     * 🖥️ I also use `self-hosted runners` for `faster builds` especially when working within a private network like `VPC`.
     * 🔁 Reusing workflows and skipping `unnecessary steps` also significantly improves `performance`.

---

## 📂 Folder-Based Trigger Example

 * 👉 Run workflow only when specific folder changes (`/backend`).
 * 🚀 How will you handle this?
      * Use the `paths filter` to limit triggering the workflow:
```yaml
on:
  push:
    paths:
      - 'backend/**'
```
This workflow will only run if there are changes inside the `backend/ folder`.


## 🔁 How can you reuse workflows in GitHub Actions?

  * 🚀 Use `workflow_call` to reuse workflows
  * GitHub Actions allow you to create reusable workflows that can be called from other workflows using the workflow_call event.
  * This is useful for DRY `(Don't Repeat Yourself)` principles across multiple repositories or branches.

## 📄 Difference Between Workflow File and Job File in GitHub Actions

  * In GitHub Actions, there is no separate concept called a `"job file.`
  * A workflow file, typically named `.github/workflows/your_workflow.yml`, defines the entire workflow, including `all the jobs` and their configurations.
  * A job is a part of that file and defines a `set of steps` to run in one runner. There are no separate job files — jobs live `inside workflow files`.

| Concept     | Meaning            |
| ----------- | ------------------ |
| 📄 Workflow | Full pipeline file |
| 🧩 Job      | Part of workflow   |

---

## 🧪 How can you use GitHub Actions to automate code quality checks and linting?

 * Using GitHub Actions, I integrated `ESLint` for `linting` and `SonarQube` for `static code analysis` into our CI pipeline.
 * It ensured that every pull request passed `lint checks`, helping us maintain `consistent coding style` and `catch errors`, bugs early in the development cycle.
 * 🧹 `ESLint (JavaScript)` linting means `analyzing your code for errors`, `bugs`, and bad practices before you run it.
 * 🔍 SonarQube supports a wide range of programming languages including `Java`, `JavaScript`, `Python`, `C`, `C++`, `Go`, `PHP`, etc.
 * 🐞 It helps detect `bugs, code smells, security vulnerabilities`, and enforces coding standards early in the development lifecycle...


## 🔄 Explain the difference between "workflow_run" and "workflow_call"

## 🔗 workflow_run
  - 📌 Used for: Triggering a workflow after `another one finishes`.  
  - 💡 Example use case: Run `integration tests after a build workflow completes`.  
  - ⚙️ How it works: `One workflow finishes → Triggers another workflow`.  

## 🔁 workflow_call
  - 📌 Used for: Create reusable workflows, which can be `called/invoked` by other workflows.  
  - 💡 Example use case: Reuse the `same deployment` or `testing logic` across `multiple workflows`.  
  - ⚙️ How it works: One workflow calls a `reusable workflow`. Helps you follow `DRY` (Don't Repeat Yourself) principle.  

### ⚙️ GitHub Actions: workflow_run vs workflow_call
| 🧩 Feature       | 🔁 `workflow_run`                                 | 🧩 `workflow_call`                  |
| ---------------- | -------------------------------------------------- | ----------------------------------- |
| 🎯 Purpose       | 🚀 Trigger another workflow after one finishes    | ♻️ Reuse a workflow like a function |
| 🔄 Flow          | 👉 Sequential (`Workflow A ➝ Workflow B`)        | 🔁 Modular (call anytime)           |
| 🧠 How It Works  | 👂 Listens to completion via `workflow_run` event | 📞 Called using `workflow_call`     |
| ⚙️ Configuration | `on: workflow_run: workflows: ["Build"]`          | `on: workflow_call:`                |
| 📦 Use Case      | 🚀 Post-build: deploy, notify, cleanup            | 🧪 Reusable build/test workflows    |
| 🔒 Control       | ⚠️ Limited inputs/outputs                         | ✅ Supports inputs, outputs, secrets |
| ♻️ Reusability   | ❌ Not reusable                                   | ✅ Highly reusable                   |
| 👥 Best For      | 🔗 Pipeline chaining (CI ➝ CD)                   | 🧱 DRY principle (reuse logic)      |

---

## 🏁 Final Summary
 * ✨ fail-fast → Stop or continue matrix
 * ✨ Errors → Control flow
 * ✨ Optimization → Speed & cost
 * ✨ Reuse → DRY pipelines
 * ✨ workflow_run vs call → Trigger vs reuse
