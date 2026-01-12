# 🛡️ What is Trivy?
Trivy is a simple and powerful security scanner used to find vulnerabilities and security issues in your software projects.
> It is very popular in DevOps, CI/CD pipelines, Docker, and Kubernetes environments.

✅ GitHub Actions syntax:
```
security-check:
  runs-on: self-hosted
  needs: sonarqube
  steps:
    - uses: actions/checkout@v4

    - run: trivy fs --exit-code 1 --severity HIGH,CRITICAL .
```

## 🔍 Trivy can scan multiple targets:
| Scan Type            | What it checks                  | Example                      |
| -------------------- | ------------------------------- | ---------------------------- |
| **Filesystem (fs)**  | Source code & dependencies      | React, Node.js, Java, Python |
| **Container images** | OS & app packages inside images | Docker images                |
| **Repositories**     | GitHub / GitLab repos           | Public & private repos       |
| **Kubernetes**       | Cluster misconfigurations       | YAML, Helm charts            |
| **Config files**     | IaC misconfigurations           | Terraform, Dockerfile        |

## 🚨 What Issues Trivy Finds
Trivy detects:
```
🔴 Security vulnerabilities (CVEs)
🟠 Critical & High severity issues
⚠️ Misconfigurations
🔐 Secrets & credentials
📦 Outdated dependencies
🧱 Infrastructure-as-Code risks
```
## ⚙️ Common Trivy Commands
| Command                                   | Purpose           |
| ----------------------------------------- | ----------------- |
| `trivy fs .`                              | Scan source code  |
| `trivy image nginx:latest`                | Scan Docker image |
| `trivy repo https://github.com/user/repo` | Scan repo         |
| `trivy k8s cluster`                       | Scan Kubernetes   |
| `trivy config .`                          | Scan IaC configs  |

## 🧩 Why Trivy is Used in CI/CD
```
✔ Fast & lightweight
✔ Easy to integrate
✔ No separate server needed
✔ Supports many languages
✔ Free & open source

Used with:

GitHub Actions
Jenkins
GitLab CI
Docker
Kubernetes
```
## Trivy 🆚 SonarQube
| Tool      | Type                | Notes              |
| --------- | ------------------- | ------------------ |
| Trivy     | All-in-one scanner  | Very simple & fast |
| SonarQube | Code quality + SAST (Static Application Security Testing) | Needs server   |

✅ One-line Summary
> Trivy is a security scanner that finds vulnerabilities in your code, containers, and infrastructure — and can stop insecure builds automatically.

## ✅ Option 1: Install Trivy in the workflow (Recommended)
Add this step before running trivy fs:
```
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
```
- name: Trivy filesystem scan
  run: trivy fs --exit-code 1 --severity HIGH,CRITICAL .
```
What this does:
```
1. Scans your entire project
2. Finds HIGH & CRITICAL vulnerabilities
3. Fails the pipeline if any are found
```

## ✅ Option 2: Install Trivy once on the runner machine (Permanent)
> If you don’t want to install Trivy every run, install it once on ubuntu2.
### Run on the runner server:
```
sudo apt-get update
sudo apt-get install -y wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install -y trivy
```
### Verify:
```
trivy --version
```








