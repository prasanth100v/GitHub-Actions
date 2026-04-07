## 🚀 What is GitHub Actions ?
  
  * GitHub Actions is a powerful `CI/CD` (Continuous Integration/Continuous Deployment) platform integrated into GitHub.
  * That automates workflows to `build`, `test`, and `deploy` code directly from the repository.
  * It can be triggered by events like `manual`, `push`, `pull requests`, or `scheduled times`.
  * ☁️ GitHub Actions is a `Software as a Service` (SaaS) `no server management`.
  * 📁 Workflows are defined in `YAML files` (.yml) inside the `.github/workflows/*.yml` directory.

---

## ⚙️ What is the difference between a job and step in GitHub Actions?
| 🧩 Concept | 💡 Description                                           |
| ---------- | --------------------------------------------------------- |
| 🧩 Job     | 🖥️ Collection of steps that run on the same runner       |
| 🔹 Step    | ⚙️ Individual task inside a job (like a command/action)  |


### 📌 Example:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Build project
        run: npm run build
```

---

## 🔔 How do you trigger a GitHub Actions workflow manually and automatically?
### ⚙️ GitHub Actions Triggers
| 🧩 Trigger Type | 📌 Method                  | 💡 Description                |
| --------------- | -------------------------- | ----------------------------- |
| ⚡ Automatic     | 🔄 `on: push`              | 📤 Runs when code is pushed   |
|                 | 🔀 `on: pull_request`      | 🔍 Runs on PR creation/update |
|                 | ⏰ `on: schedule`           | 🕒 Runs on cron schedule      |
| 🎯 Manual       | ▶️ `on: workflow_dispatch` | 👤 Trigger manually from UI   |

---

## 🖥️ What are runners? What’s the difference between hosted and self-hosted runners?

 * Runners are the environments where jobs execute.
 * ☁️ GitHub provides
     * hosted runners (`Ubuntu`, `Windows`, `macOS`).
     * 🧑‍💻 You can also create your own `self-hosted runner` (e.g., `EC2`).

---

## 🔐 How do you pass secrets in GitHub Actions?

 * I use `GitHub Secrets` to securely store credentials and access them in workflows
 * ⚙️ Define secrets under `Repo → Settings → Secrets`
 * 🔑 Access them with `${{ secrets.SECRET_NAME }}`

### 📌 Example:
```yaml
- run: aws s3 ls  
  env:
    AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}  
    AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}  
```

---

## 📦 What is `actions/checkout@v3` and why is it required?

 * This is a standard action to `clone your repo` into the runner.
 * ⚠️ Without it, your workflow `won’t have access` to your project files.
 * 📌 Example: `uses: actions/checkout@v3 ` 

---

## 🧪 What is matrix strategy in GitHub Actions?

 * Run jobs in parallel across different environments (e.g., `multiple Node.js versions`, `OS`, etc.).
 * ⚡ `Matrix strategy` = test across multiple environments easily.
 * ⏱️ Saves time by running jobs in `parallel`. Helps catch `environment-specific bugs`.

### Why Use Matrix❓

 * To test your code in multiple environments automatically and faster.
 * 📌 Example Use Case: You have a Node.js project, and you want to test it on:
     * Node.js versions: 14, 16, 18
     * OS: ubuntu-latest, windows-latest  
```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        node: [14, 16, 18]
        os: [ubuntu-latest, windows-latest]

    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node }}
```
💡 "Matrix builds allow you to run jobs in parallel with different combinations (e.g.,` OS + versions`)"

---

## 🔁 What is a reusable workflow in GitHub Actions?

 * A reusable workflow is a GitHub Actions file that can be called from `another workflow`
 * Like a function you can `reuse` instead of writing the same steps again and again.
 * `Reusable Workflow = Function in programming`

### 📌 Real-Time Example:  
   * Step 1: Create reusable workflow in `.github/workflows/reusable.yml`
   * Step 2: Call it from `another service’s workflow ` 

```yaml
jobs:
  eal-reusable:
    uses: ./.github/workflows/reusable.yml
    with:
      service_name: cart
```

### 🎤 In Interview, say like this:
  * We used a reusable workflow in GitHub Actions to avoid repeating the same `CI/CD steps` across all services.
  * We created one `reusable.ym`l that handled `Docker build`, `ECR push`, and `EKS deploy`.
  * Then, each service like `auth` or `cart` called that workflow using `workflow_call`, passing the service name as `input`.
  * This made our `pipeline DRY`, easy to `maintain`, and `standardized`.

---

## ⚖️ What is the difference between run and uses in steps?
| 🧩 Keyword | 💡 Purpose                                         |
| ---------- | --------------------------------------------------- |
| 🟢 `run`   | 🖥️ Execute shell commands                          |
| 🔵 `uses`  | 📦 Use pre-built GitHub Actions (reusable actions) |

### 📌 Example:
```yaml
- run: echo "Hello"  
- uses: actions/checkout@v3  
```
