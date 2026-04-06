# 🚀 1. What is GitHub Actions ?

GitHub Actions is a powerful CI/CD (Continuous Integration/Continuous Deployment) platform integrated into GitHub that automates workflows to build, test, and deploy code directly from the repository. It can be triggered by events like push, pull requests, or scheduled times.

☁️ GitHub Actions is a Software as a Service (SaaS).  
📁 Workflows are defined in YAML files (.yml) inside the .github/workflows/ directory.

---

# ⚙️ 2. What is the difference between a job and step in GitHub Actions?

🧩 Job  – A collection of steps that run on the same runner  
🔹 Step – An individual task inside a job (like a command or action)

### 📌 Example:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Build project
        run: npm run build

---

# 🔔 3. How do you trigger a GitHub Actions workflow manually and automatically?

⚡ Automatically : on: push, on: pull_request, on: schedule  
🎯 Manually     : on: workflow_dispatch  

---

# 🖥️ 4. What are runners? What’s the difference between hosted and self-hosted runners?

A runner is the environment where your job runs.  
☁️ GitHub provides hosted runners (Ubuntu, Windows, macOS).  
🧑‍💻 You can also create your own self-hosted runner (e.g., EC2).

---

# 🔐 5. How do you pass secrets in GitHub Actions?

Use the GitHub Secrets feature.  
⚙️ Define secrets under Repo → Settings → Secrets  
🔑 Access them with ${{ secrets.SECRET_NAME }}

### 📌 Example:

- run: aws s3 ls  
  env:
    AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}  
    AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}  

---

# 📦 6. What is actions/checkout@v3 and why is it required?

This is a standard action to clone your repo into the runner.  
⚠️ Without it, your workflow won’t have access to your project files.

### 📌 Example:
- uses: actions/checkout@v3  

---

# 🧪 7. What is matrix strategy in GitHub Actions?

The matrix lets you run jobs in parallel across different environments (e.g., multiple Node.js versions, OS, etc.).  
⚡ Matrix strategy = test across multiple environments easily.  
⏱️ Saves time by running jobs in parallel. Helps catch environment-specific bugs.

## ❓ Why Use Matrix?

To test your code in multiple environments automatically and faster.  

📌 Example Use Case: You have a Node.js project, and you want to test it on:  
- Node.js versions: 14, 16, 18  
- OS: ubuntu-latest, windows-latest  

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

💡 "Matrix builds allow you to run jobs in parallel with different combinations (e.g., OS + versions)"

---

# 🔁 What is a reusable workflow in GitHub Actions?

A reusable workflow is a GitHub Actions file that can be called from another workflow — like a function you can reuse instead of writing the same steps again and again.

📌 Real-Time Example:  
Step 1: Create reusable workflow in .github/workflows/reusable.yml  
Step 2: Call it from another service’s workflow  

jobs:
  eal-reusable:
    uses: ./.github/workflows/reusable.yml
    with:
      service_name: cart

## 🎤 In Interview, say like this:

“We used a reusable workflow in GitHub Actions to avoid repeating the same CI/CD steps across all services. We created one reusable.yml that handled Docker build, ECR push, and EKS deploy. Then, each service like auth or cart called that workflow using workflow_call, passing the service name as input. This made our pipeline DRY, easy to maintain, and standardized.”

---

# ⚖️ 10. What is the difference between run and uses in steps?

🟢 run   Executes a shell command  
🔵 uses  Runs a GitHub Action  

### 📌 Example:
- run: echo "Hello"  
- uses: actions/checkout@v3  
