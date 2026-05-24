# 🛡️ What is Trivy?
 * ⚡ Trivy is a simple and `powerful security scanner` used to `find vulnerabilities` and `security issues` in your software projects.
 * 🎯 It is very popular in `DevOps`, `CI/CD pipelines`, `Docker`, and `Kubernetes environments`.

✅ GitHub Actions syntax:
```yaml
security-check:
  runs-on: self-hosted
  needs: sonarqube
  steps:
    - uses: actions/checkout@v4

    - run: trivy fs --exit-code 1 --severity HIGH,CRITICAL .
```

## 🔍 Trivy can scan multiple targets:

| 🔍 **Scan Type**          | 🎯 **What It Checks**                | 🧠 **Detailed Explanation**                                               | 💡 **Examples**              | 🚀 **Common Tools**  |
| ------------------------- | ------------------------------------ | ------------------------------------------------------------------------- | ---------------------------- | -------------------- |
| 🖥️ **Filesystem (`fs`)** | Source code & dependencies           | Scans application files, libraries, and packages for vulnerabilities      | React, Node.js, Java, Python | Trivy, Snyk          |
| 📦 **Container Images**   | OS & app packages inside images      | Detects vulnerable packages in Docker/container images                    | NGINX image, Node image      | Trivy,              |
| 🌐 **Repositories**       | Git repositories                     | Scans source repositories for secrets, vulnerabilities, and policy issues | GitHub / GitLab repos        | Gitleaks,          |
| ☸️ **Kubernetes**         | Cluster & manifest misconfigurations | Detects insecure Kubernetes YAML, RBAC, network, and pod settings         | YAML, Helm charts            | Kubescape, Trivy     |
| ⚙️ **Config Files (IaC)** | Infrastructure misconfigurations     | Scans Terraform, Dockerfile, Kubernetes configs for security issues       | Terraform, Dockerfile        | Checkov, Terrascan   |


## 🚨 What Issues Trivy Finds
Trivy detects :
```hcl
🔴 Security vulnerabilities (CVEs)
🟠 Critical & High severity issues
⚠️ Misconfigurations
🔐 Secrets & credentials
📦 Outdated dependencies
🧱 Infrastructure-as-Code risks
```

## ⚙️ Common Trivy Commands
| ⚡ **Command**                           | 🎯 **Purpose**            | 🧠 **What It Scans**                             | 💡 **Example Use Case**                |
| ----------------------------------------- | ------------------------- | ------------------------------------------------ | -------------------------------------- |
| `trivy fs .`                              | 🖥️ Filesystem Scan       | Source code + dependencies in current directory  | Scan Node.js / Python project          |
| `trivy image nginx:latest`                | 📦 Container Image Scan   | OS packages + application libraries inside image | Scan Docker image before push          |
| `trivy repo https://github.com/user/repo` | 🌐 Repository Scan        | Entire Git repository                            | Scan GitHub / GitLab repos             |
| `trivy k8s cluster`                       | ☸️ Kubernetes Scan        | Kubernetes cluster resources & configs           | Detect cluster misconfigurations       |
| `trivy config .`                          | ⚙️ IaC Configuration Scan | Terraform, Dockerfile, Kubernetes YAML           | Detect insecure infrastructure configs |


## 🧩 Why Trivy is Used in CI/CD
 * 🌿 Fast & lightweight
 * 🌿 Easy to integrate
 * 🌿 No separate server needed
 * 🌿 Supports `many languages`
 * 🌿 Free & open source
 * ⚡ Used with :
     * `GitHub Actions`
     * Jenkins and GitLab CI
     * `Docker` and `Kubernetes`

## Trivy 🆚 SonarQube
| 🧩 **Tool** | 🎯 **Type**                     | 🧠 **What It Does**                                         | 💡 **Key Features**                               | ⚡ **Notes**              |
| ----------- | ------------------------------- | ----------------------------------------------------------- | ------------------------------------------------- | ------------------------ |
| Trivy       | 🛡️ All-in-one Security Scanner | Scans vulnerabilities, secrets, IaC, containers, Kubernetes | CVE scanning, secret scanning, SBOM support       | 🚀 Very simple & fast    |
| SonarQube   | 🔍 Code Quality + SAST          | Analyzes source code quality & security issues              | Bugs, code smells, vulnerabilities, quality gates | ⚙️ Requires server setup |

## ✅ One-line Summary
 > Trivy is a security scanner that finds vulnerabilities in your code, containers, and infrastructure — and can stop insecure builds automatically.

## ✅ Option 1: Install Trivy in the workflow (Recommended)
 🎯 Add this step before running trivy fs:
```yaml
- name: Install Trivy
  run: |
    sudo apt-get update
    sudo apt-get install -y wget apt-transport-https gnupg lsb-release
    wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
    echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list
    sudo apt-get update
    sudo apt-get install -y trivy
```

Then your scan step will work:
```yaml
- name: Trivy filesystem scan
  run: trivy fs --exit-code 1 --severity HIGH,CRITICAL .
```

### What this does:
   1. ⚡ Scans your `entire project`
   2. ⚡ Finds `HIGH & CRITICAL vulnerabilities`
   3. ⚡ Fails the pipeline if any are found

## ✅ Option 2: Install Trivy once on the runner machine (`Permanent`)
> If you don’t want to install Trivy every run, `install it once on ubuntu2`.

### Run on the runner server:
```hcl
sudo apt-get update
sudo apt-get install -y wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install -y trivy
```

### Verify:
```hcl
trivy --version
```

---

## 🛡️ Trivy — Rapid Fire DevOps Interview Q&A
| 🔢 Q#   | ❓ Question                                           | 💡 Answer                                                                                                                                     |
| ------- | ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 🔹 Q1   | What is Trivy?                                       | 👉 Trivy — A security scanner for code, containers, Kubernetes, and infrastructure.                                                           |
| 🔹 Q2   | Main purpose of Trivy?                               | 👉 Detect vulnerabilities and security issues.                                                                                                |
| 🔹 Q3   | Who created Trivy?                                   | 👉 Aqua Security                                                                                                                              |
| 🔹 Q4   | Why is Trivy popular in DevOps?                      | 👉 ✅ Fast <br> ✅ Lightweight <br> ✅ Easy CI/CD integration <br> ✅ Free & open source                                                          |
| 🔹 Q5   | One-line interview definition of Trivy?              | 👉 Trivy is a security scanner that detects vulnerabilities in code, containers, and infrastructure.                                          |
| 🎯 Q6   | What can Trivy scan?                                 | 👉 Filesystem <br> Containers <br> Repositories <br> Kubernetes <br> IaC configs                                                              |
| 🎯 Q7   | What does trivy fs scan?                             | 👉 Source code and dependencies.                                                                                                              |
| 🎯 Q8   | What does trivy image scan?                          | 👉 Container images.                                                                                                                          |
| 🎯 Q9   | What does trivy repo scan?                           | 👉 Entire Git repositories.                                                                                                                   |
| 🎯 Q10  | What does trivy k8s scan?                            | 👉 Kubernetes clusters and resources.                                                                                                         |
| 🎯 Q11  | What does trivy config scan?                         | 👉 Infrastructure-as-Code configurations.                                                                                                     |
| 🔐 Q12  | What are CVEs?                                       | 👉 Common Vulnerabilities and Exposures.                                                                                                      |
| 🔐 Q13  | What issues can Trivy detect?                        | 👉 CVEs <br> Secrets <br> Misconfigurations <br> Outdated packages <br> IaC risks                                                             |
| 🔐 Q14  | What severity levels does Trivy support?             | 👉 LOW <br> MEDIUM <br> HIGH <br> CRITICAL                                                                                                    |
| 🔐 Q15  | Why are HIGH and CRITICAL vulnerabilities important? | 👉 They represent major security risks.                                                                                                       |
| 🔐 Q16  | What are misconfigurations?                          | 👉 Insecure infrastructure or application settings.                                                                                           |
| 🔐 Q17  | Can Trivy detect secrets?                            | 👉 Yes.                                                                                                                                       |
| 🔐 Q18  | Example of secrets detected by Trivy?                | 👉 API keys <br> Passwords <br> Tokens <br> AWS credentials                                                                                   |
| 🛠️ Q19 | Command to scan current project filesystem?          | 👉 `trivy fs .`                                                                                                                               |
| 🛠️ Q20 | Command to scan Docker image?                        | 👉 `trivy image nginx:latest`                                                                                                                 |
| 🛠️ Q21 | Command to scan Git repository?                      | 👉 `trivy repo https://github.com/user/repo`                                                                                                  |
| 🛠️ Q22 | Command to scan Kubernetes cluster?                  | 👉 `trivy k8s cluster`                                                                                                                        |
| 🛠️ Q23 | Command to scan IaC configs?                         | 👉 `trivy config .`                                                                                                                           |
| 🛠️ Q24 | Command to check Trivy version?                      | 👉 `trivy --version`                                                                                                                          |
| ⚙️ Q25  | Why integrate Trivy into GitHub Actions?             | 👉 Automated security scanning in CI/CD.                                                                                                      |
| ⚙️ Q26  | Example Trivy GitHub Actions command?                | 👉 `trivy fs --exit-code 1 --severity HIGH,CRITICAL .`                                                                                        |
| ⚙️ Q27  | Purpose of --exit-code 1?                            | 👉 Fail pipeline if vulnerabilities are found.                                                                                                |
| ⚙️ Q28  | Purpose of --severity HIGH,CRITICAL?                 | 👉 Scan only high-risk vulnerabilities.                                                                                                       |
| ⚙️ Q29  | What happens if Trivy finds critical issues?         | 👉 Pipeline/job fails.                                                                                                                        |
| ⚙️ Q30  | Why fail pipelines on vulnerabilities?               | 👉 Prevent insecure deployments.                                                                                                              |
| 🚀 Q31  | Why is Trivy used in CI/CD?                          | 👉 Shift-left security scanning.                                                                                                              |
| 🚀 Q32  | What is shift-left security?                         | 👉 Detecting security issues early in development.                                                                                            |
| 🚀 Q33  | Which CI/CD tools commonly use Trivy?                | 👉 GitHub Actions <br> Jenkins <br> GitLab CI <br> Azure DevOps                                                                               |
| 🚀 Q34  | Why is Trivy good for pipelines?                     | 👉 Fast and lightweight.                                                                                                                      |
| 🚀 Q35  | Does Trivy require separate server setup?            | 👉 No.                                                                                                                                        |
| 🐳 Q36  | Why scan container images?                           | 👉 Detect vulnerable OS/app packages.                                                                                                         |
| 🐳 Q37  | Example image scan?                                  | 👉 `trivy image node:18`                                                                                                                      |
| 🐳 Q38  | When should images be scanned?                       | 👉 Before pushing/deploying.                                                                                                                  |
| 🐳 Q39  | Why is container scanning important?                 | 👉 Containers may contain vulnerable packages.                                                                                                |
| ☸️ Q40  | Why scan Kubernetes manifests?                       | 👉 Detect insecure configurations.                                                                                                            |
| ☸️ Q41  | Example Kubernetes issues detected by Trivy?         | 👉 Privileged containers <br> Open RBAC permissions <br> Insecure networking                                                                  |
| ☸️ Q42  | Why scan Terraform files?                            | 👉 Prevent insecure cloud infrastructure.                                                                                                     |
| ☸️ Q43  | What is IaC scanning?                                | 👉 Security analysis of infrastructure code.                                                                                                  |
| 🔍 Q44  | Difference between Trivy and SonarQube?              | 👉 Trivy focuses on vulnerabilities/security; SonarQube focuses on code quality and SAST.                                                     |
| 🔍 Q45  | Which tool scans containers?                         | 👉 Trivy.                                                                                                                                     |
| 🔍 Q46  | Which tool uses Quality Gates?                       | 👉 SonarQube                                                                                                                                  |
| 🔍 Q47  | Which tool detects code smells?                      | 👉 SonarQube.                                                                                                                                 |
| 🔍 Q48  | Which tool scans IaC configs?                        | 👉 Trivy.                                                                                                                                     |
| 🔍 Q49  | Which tool is simpler to set up?                     | 👉 Trivy.                                                                                                                                     |
| 📦 Q50  | Recommended way to install Trivy in CI?              | 👉 Install dynamically in workflow.                                                                                                           |
| 📦 Q51  | Alternative installation method?                     | 👉 Install permanently on runner.                                                                                                             |
| 📦 Q52  | Command to install Trivy package repo key?           | 👉 `wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key \| sudo apt-key add -`                                                |
| 📦 Q53  | Command to install Trivy permanently?                | 👉 `sudo apt-get install -y trivy`                                                                                                            |
| 📦 Q54  | Why install Trivy permanently on runner?             | 👉 Faster workflows.                                                                                                                          |
| 🚨 Q55  | Pipeline failed after Trivy scan. Why?               | 👉 HIGH/CRITICAL vulnerabilities detected.                                                                                                    |
| 🚨 Q56  | Why scan dependencies in CI/CD?                      | 👉 Prevent vulnerable libraries in production.                                                                                                |
| 🚨 Q57  | Why combine SonarQube + Trivy?                       | 👉 Code quality + security coverage.                                                                                                          |
| 🚨 Q58  | Real DevOps security workflow?                       | 👉 Code push <br> ↓ <br> SonarQube scan <br> ↓ <br> Trivy scan <br> ↓ <br> Build image <br> ↓ <br> Deploy                                     |
| 🚨 Q59  | Why is Trivy called “all-in-one” scanner?            | 👉 It scans code, containers, Kubernetes, secrets, and IaC.                                                                                   |
| 🚨 Q60  | Best practice before deployment?                     | 👉 Run Trivy security scan.                                                                                                                   |
| 🏆 Q61  | Final Trivy interview answer?                        | 👉 “I use Trivy in CI/CD pipelines to scan source code, containers, Kubernetes manifests, and IaC for vulnerabilities and misconfigurations.” |
| 🏆 Q62  | Final DevOps security workflow answer?               | 👉 “We integrate SonarQube for code quality and Trivy for security scanning before deployment.”                                               |
