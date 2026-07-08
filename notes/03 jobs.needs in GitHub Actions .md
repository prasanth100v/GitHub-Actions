# 🔗 What is jobs.<job_id>.needs in GitHub Actions?
## 🚀 What is it?
 * 👉 Controls execution order of jobs
 * In GitHub Actions, `jobs.<job_id>.needs` is used to control the `order of job execution` by creating `dependencies` between jobs.
 * It tells GitHub Actions : `“This job should only run after the specified job(s) complete successfully.”`

## 📄 Real-Time Example:
 * I use `needs` to define dependencies between jobs, ensuring that steps like `deployment` only run after successful `build and test stages` .
```yaml
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
```

## 📌 In this case:
   * ⛔ test won’t run unless `build passes` → needs: build
   * ✅ deploy won’t run unless `both build and test` succeed → needs: [build, test]  

## 💡 Why It’s Useful:
   * 🧩 Helps organize multi-step `CI/CD` pipelines
   * 🚫 Prevents unnecessary `job execution` if dependencies fail
   * ⚡ Enables `parallel` or `conditional workflows` 

---

# ⚖️ What is the difference between `run, uses, and with`?

| 🧩 Keyword | 💡 Purpose                               |
| ---------- | ----------------------------------------- |
| 🟢 `run`   | 🖥️ Run `shell` commands in the runner    |
| 🔵 `uses`  | 📦 Use a `prebuilt` GitHub Action        |
| 🟣 `with`  | ⚙️ Provide `inputs/config` to the action |

## 📄 Example:  
```yaml
- run: echo "Hello"  
- uses: actions/checkout@v3  
- with: python-version: 3.9  
```

---

## ⏱️ Your builds are taking too long. What would you do?
 * 📦 Use caching for dependencies (`actions/cache`)
 * 🔄 Avoid duplicate steps
 * ⚡ Run jobs in `parallel`
 * 🪶 Use `lightweight` runners if possible  

### 🌍 How would you manage deployments across different environments (dev, staging, prod)?
   * Use `environment-specific jobs` or `matrices`, with `manual approval` for production.

### ☁️ You want to deploy to AWS securely from GitHub Actions. How?
   * Use OIDC (`OpenID Connect`) with IAM roles (`IRSA`) to give temporary credentials to GitHub instead of storing `long-term` AWS credentials.

### 🔑 How does GitHub Actions authenticate Terraform to AWS?
   * By using OIDC with the `aws-actions/configure-aws-credentials` action. (`OIDC + IAM Role`)

### 🧹 Why use terraform fmt and validate in CI?
   * ✅ To Ensure `consistent` formatting
   * 🛠️ Catch `syntax errors` early

### 📦 What is `uses` in GitHub Actions?
   * It refers to a `pre-built action` or `reusable` component.

## 🔒 How do you handle secrets safely in a GitHub Actions pipeline?
   * 🔐 I securely manage secrets using `GitHub Secrets` or `OIDC-based` authentication `🔑 Use secrets.* syntax ` 
   * 🚫 Never hardcode credentials in YAML and ensuring least privilege access.
   * Use `OIDC + AWS IAM roles` for better security.  

---

## 🚀 GitHub Actions: Composite Action vs Reusable Workflow
  * 🧱 A Composite Action is used to reuse a group of steps within a job.
  * 🔄 A Reusable Workflow is used to reuse an entire workflow, including one or more jobs, runners, matrices, environments, and secrets.
| 🧩 **Feature**                    | 🧱 **Composite Action**                           | 🔄 **Reusable Workflow**            |
| --------------------------------- | ------------------------------------------------- | ----------------------------------- |
| 🎯 **Purpose**                    | Reuse a set of **steps**                          | Reuse an entire **workflow or job** |
| 📂 **File Location**              | `.github/actions/<action-name>/action.yml`        | `.github/workflows/<workflow>.yml`  |
| 📞 **Called Using**               | `uses:` inside a **step**                         | `uses:` inside a **job**            |
| 🏗️ **Can Contain Jobs?**         | ❌ No                                              | ✅ Yes                               |
| 🖥️ **Can Define `runs-on`?**     | ❌ No (inherits caller's runner)                   | ✅ Yes                               |
| 📊 **Supports Matrix Strategy?**  | ❌ No                                              | ✅ Yes                               |
| 🔁 **Can Call Other Workflows?**  | ❌ No                                              | ✅ Yes (where supported)             |
| 🎯 **Best Use Case**              | Reuse common steps (login, build, setup, install) | Reuse complete CI/CD pipelines      |

