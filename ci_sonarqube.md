# SonarQube
🌿 SonarQube Job in GitHub Actions Reactjs
```yaml
 sonarqube:
    runs-on: self-hosted
    needs: build

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: SonarQube Scan
        uses: sonarsource/sonarqube-scan-action@v2
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
        with:
          args: |
            -Dsonar.projectKey=vite-react-app
            -Dsonar.projectName=Vite_React_App
            -Dsonar.sources=src
            -Dsonar.exclusions=**/node_modules/**,**/dist/**
            -Dsonar.sourceEncoding=UTF-8
```

🔍 The SonarQube Quality Gate Step
```yaml
- name: SonarQube Quality Gate 
  uses: sonarsource/sonarqube-quality-gate-action@v1.1.0
  env:
    SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
    SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
  timeout-minutes: 5
```

### What is a Quality Gate?
 * In SonarQube, a Quality Gate is a set of rules like:
   * ❌ No new bugs
   * ❌ No new vulnerabilities
   * ✅ Code coverage ≥ 80%
   * ❌ No new code smells

> If your code fails any rule → Quality Gate = FAILED.
 * ⏱️ Wait up to 5 minutes for Quality Gate result ***If not ready in 5 minutes → ❌ job fails.***

### Think of it like this
 * 📌 SonarQube Scan = `Doctor tests` && `Quality Gate` = Doctor decision (`Fit` / `Not Fit`)


## 🖥️ Install SonarQube using Docker (BEST ✅)
### 🔹 Step 1: Install Docker (Ubuntu)
```hcl
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
```

📌 Verify:
```hcl
docker --version
```

### 🔹 Step 2: Increase Linux Memory Limit (MANDATORY)
⚡ SonarQube will NOT start without this.
```hcl
sudo sysctl -w vm.max_map_count=262144
```
⚡ Make it permanent:
```hcl
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```
🌿 Verify:
```hcl
sysctl vm.max_map_count
```

## 🔹 Step 3: Create SonarQube Directories
```hcl
mkdir -p ~/sonarqube/{data,logs,extensions}
```

## 🔹 Step 4: Run SonarQube Container
```hcl
docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  -v ~/sonarqube/data:/opt/sonarqube/data \
  -v ~/sonarqube/logs:/opt/sonarqube/logs \
  -v ~/sonarqube/extensions:/opt/sonarqube/extensions \
  sonarqube:lts
```

🌿 Check status:
```
docker ps
docker logs sonarqube
```

## 🌐 Access SonarQube UI
* Wait 1–2 minutes ⏳ (SonarQube is heavy — it does NOT start instantly)
* 🌿 Open browser:
```hcl
http://localhost:9000/
```
📌 Default login:
```hcl
Username: admin
Password: admin
```

# Add GitHub Secrets
### Find ubuntu2 IP address On ubuntu2, run:
```hcl
ip a
```
👉 Your IP = 10.0.2.15 (example)

### Create SONAR_TOKEN
* 🔐 Steps
  * Open SonarQube in Browser & `Login as Admin`
  * Click Profile (`top-right`)
  * My Account → Security
  * 📌 Generate Token :
      * Name: github-actions
      * Type: use 🔑 User Token (`specific SonarQube user account`) OR 🌍 Global Analysis Token (for `system-level automation`)
      * Expires in : No expiration (`recommended`)
   * 📋 Copy the token (⚠️ `shown only once`)


### Go to: ***GitHub Repo → Settings → Secrets → Actions*
| Secret Name      | Value                                                                  |
| ---------------- | ---------------------------------------------------------------------- |
| SONAR_HOST_URL | `http://http://10.0.2.15:9000`                                         |
| SONAR_TOKEN    | `squ_e478aeace8dc7c9785d6940aed55c9232e1f34f8  <copied_token>`          |


## (Optional but recommended) Verify from host Linux ubuntu2:
```hcl
curl http://10.0.2.15:9000/api/system/status
```
📌 Expected: {"status":`"UP"`}

****************Job run in GitHub Actions****************


# Access SonarQube from Browser
 * You already have SonarQube running correctly inside Docker 👍
 * The only thing left is how to access it from your browser when your `Ubuntu VM uses VirtualBox NAT`.

## ✅ Add Port Forwarding in VirtualBox
```hcl
1. Power OFF your Ubuntu VM
2. Open VirtualBox
3. Select your Ubuntu VM → Settings
4. Go to Network
5. Adapter 1 → Attached to: NAT
6. Click Advanced
7. ✅Click Port Forwarding

8. 📌Add a rule:
 🌿 Name	sonarqube
 🌿 Protocol	TCP
 🌿 Host IP	(leave empty)
 🌿 Host Port	9000
 🌿 Guest IP	(leave empty)
 🌿 Guest Port	9000
9. ✅ Click OK
```
📌 Start the VM

## ~~~~~~~~~~~~~~~~~~~~ 🌍 ~~~~~~~~~~~~~~~~~~~

## 🔎 Important Notes (Read this)
### ❓ Why checkout again in sonarqube?
 * Each job runs in a fresh workspace, so you must: - uses : `actions/checkout@v4`

### ❓ Do I need build artifacts for SonarQube?
 * ❌ `No`. SonarQube analyzes source code, not `dist/.`

## ✅ Quick Diagnostic Commands for sonarqube
```hcl
docker ps
docker logs sonarqube
ss -tulnp | grep 9000
sysctl vm.max_map_count
```

## 🧰 Common Commands
```hcl
docker start sonarqube
docker stop sonarqube
docker restart sonarqube
docker logs -f sonarqube
```

---

## 🔍 SonarQube & GitHub Actions — Rapid Fire Interview Q&A

| 🔢 Q#   | ❓ Question                                           | 💡 Answer                                                                                  |
| ------- | ------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| 🔹 Q1   | What is SonarQube?                                     | 👉 SonarQube — Static code analysis tool for `code quality` and `security.  `                  |
| 🔹 Q2   | Main purpose of SonarQube?                             | 👉 Detect `bugs`, `vulnerabilities`, and `code smells`.                                          |
| 🔹 Q3   | What does SonarQube analyze?                           |  Source code <br> Security issues <br> Code quality <br> Coverage <br> Duplications      |
| 🔹 Q4   | What is static code analysis?                          | 👉 Analyzing code` without executing it.`                                                    |
| 🔹 Q5   | Why is SonarQube important in DevOps?                  | 👉 Improves code quality before deployment.                                                |
| 🔹 Q6   | Which CI/CD platforms integrate with SonarQube?        | 👉 GitHub Actions <br> Jenkins <br> GitLab CI <br> Azure DevOps                            |
| ⚙️ Q7   | Purpose of SonarQube job in GitHub Actions?            | 👉 Automated code quality scanning.                                                        |
| ⚙️ Q8   | GitHub Action used for SonarQube scan?                 | 👉 `sonarsource/sonarqube-scan-action@v2`                                                  |
| ⚙️ Q9   | Why use actions/checkout@v4 again in SonarQube job?    | 👉 Each GitHub Actions job runs in a `fresh workspace`.                                      |
| ⚙️ Q10  | What does runs-on: self-hosted mean?                   | 👉 Workflow runs on self-hosted runner.                                                    |
| ⚙️ Q11  | Why use self-hosted runner for SonarQube?              | 👉 Better access to internal/private SonarQube server.                                     |
| ⚙️ Q12  | Purpose of needs: build?                               | 👉 SonarQube job waits until build job completes.                                          |
| 🔍 Q13  | What is sonar.projectKey?                              | 👉 Unique project identifier in SonarQube.                                                 |
| 🔍 Q14  | What is sonar.projectName?                             | 👉 `Display name of project. `                                                               |
| 🔍 Q15  | Purpose of sonar.sources=src?                          | 👉 Defines `source code directory. `                                                         |
| 🔍 Q16  | Why exclude node_modules and dist?                     | 👉 Avoid scanning generated/dependency files.                                              |
| 🔍 Q17  | Purpose of sonar.sourceEncoding=UTF-8?                 | 👉 Defines file encoding.                                                                  |
| 🚦 Q18  | What is a Quality Gate?                                | 👉 Set of `code quality rules.   `                                                           |
| 🚦 Q19  | Examples of Quality Gate checks?                       |  ❌ No new bugs <br> ❌ No vulnerabilities <br> ✅ Coverage threshold <br> ❌ No code smells |
| 🚦 Q20  | What happens if Quality Gate fails?                    | 👉 Pipeline/job fails.                                                                     |
| 🚦 Q21  | GitHub Action for Quality Gate?                        | 👉 `sonarsource/sonarqube-quality-gate-action@v1.1.0`                                      |
| 🚦 Q22  | Why use Quality Gates?                                 | 👉 `Prevent poor-quality code deployment.  `                                                 |
| 🚦 Q23  | Real-world analogy for Quality Gate?                   | 👉 Scan = medical test <br> Quality Gate = doctor decision                                 |
| 🚦 Q24  | What does timeout-minutes: 5 mean?                     | 👉 Wait up to 5 minutes for Quality Gate result.                                           |
| 🚦 Q25  | What happens if result isn't ready in 5 minutes?       | 👉 Job fails.                                                                              |
| 🔐 Q26  | What is SONAR_TOKEN?                                   | 👉 `Authentication token for SonarQube. `                                                    |
| 🔐 Q27  | What is SONAR_HOST_URL?                                | 👉` SonarQube server URL.`                                                                   |
| 🔐 Q28  | Where are GitHub secrets stored?                       | 👉` Repo → Settings → Secrets → Actions.  `                                                  |
| 🔐 Q29  | Why use GitHub Secrets?                                | 👉 Securely store sensitive values.                                                        |
| 🔐 Q30  | Should tokens be hardcoded in workflow?                | 👉 Never.                                                                                  |
| 🐳 Q31  | Recommended way to install SonarQube?                  | 👉 Docker.                                                                                 |
| 🐳 Q32  | Command to install Docker in Ubuntu?                   | 👉 `sudo apt install -y docker.io`                                                         |
| 🐳 Q33  | Command to start Docker service?                       | 👉 `sudo systemctl start docker`                                                           |
| 🐳 Q34  | Command to enable Docker at boot?                      | 👉 `sudo systemctl enable docker`                                                          |
| 🐳 Q35  | Command to verify Docker installation?                 | 👉 `docker --version`                                                                      |
| 🖥️ Q36 | Important Linux setting for SonarQube?                 | 👉 `vm.max_map_count`                                                                      |
| 🖥️ Q37 | Required value for SonarQube?                          | 👉 `262144`                                                                                |
| 🖥️ Q38 | Command to update vm.max_map_count?                    | 👉 `sudo sysctl -w vm.max_map_count=262144`                                                |
| 🖥️ Q39 | Why is vm.max_map_count required?                      | 👉 Elasticsearch inside SonarQube needs it.                                                |
| 🖥️ Q40 | Command to make sysctl setting permanent?              | 👉 `echo "vm.max_map_count=262144" \| sudo tee -a /etc/sysctl.conf`                        |
| 🖥️ Q41 | Command to reload sysctl config?                       | 👉 `sudo sysctl -p`                                                                        |
| 🖥️ Q42 | Command to verify vm.max_map_count?                    | 👉 `sysctl vm.max_map_count`                                                               |
| 📁 Q43  | Command to create SonarQube directories?               | 👉 `mkdir -p ~/sonarqube/{data,logs,extensions}`                                           |
| 📁 Q44  | Default SonarQube container port?                      | 👉 `9000`                                                                                  |
| 📁 Q45  | Command to run SonarQube container?                    | 👉 `docker run -d --name sonarqube -p 9000:9000 sonarqube:lts`                             |
| 📁 Q46  | Why mount volumes in SonarQube container?              | 👉 Persist data/logs/plugins.                                                              |
| 📁 Q47  | Command to check running containers?                   | 👉 `docker ps`                                                                             |
| 📁 Q48  | Command to check SonarQube logs?                       | 👉 `docker logs sonarqube`                                                                 |
| 📁 Q49  | Command to stream logs continuously?                   | 👉 `docker logs -f sonarqube`                                                              |
| 🌐 Q50  | Default SonarQube URL?                                 | 👉 `http://localhost:9000`                                                                 |
| 🌐 Q51  | Default SonarQube username/password?                   | 👉 `admin / admin`                                                                         |
| 🌐 Q52  | Why wait before accessing SonarQube?                   | 👉 Startup takes `1–2 minutes. `                                                             |
| 🔑 Q53  | Where to generate SonarQube token?                     | 👉 Profile → My Account → Security.                                                        |
| 🔑 Q54  | Common token type for GitHub Actions?                  | 👉 User Token.                                                                             |
| 🔑 Q55  | Why is token shown only once?                          | 👉 Security reason.                                                                        |
| 🌍 Q56  | Why can't host browser access SonarQube VM directly?   | 👉 VirtualBox NAT isolation.                                                               |
| 🌍 Q57  | Solution to access SonarQube from host?                | 👉 Port forwarding.                                                                        |
| 🌍 Q58  | Host port for SonarQube in VirtualBox?                 | 👉 `9000`                                                                                  |
| 🌍 Q59  | Guest port for SonarQube?                              | 👉 `9000`                                                                                  |
| 🩺 Q60  | Command to check listening port 9000?                  | 👉 `ss -tulnp \| grep 9000`                                                                |
| 🩺 Q61  | Command to verify SonarQube API status?                | 👉 `curl http://10.0.2.15:9000/api/system/status`                                          |
| 🩺 Q62  | Expected healthy API response?                         | 👉 `{"status":"UP"}`                                                                       |
| 🐳 Q63  | Command to start SonarQube container?                  | 👉 `docker start sonarqube`                                                                |
| 🐳 Q64  | Command to stop SonarQube container?                   | 👉 `docker stop sonarqube`                                                                 |
| 🐳 Q65  | Command to restart SonarQube container?                | 👉 `docker restart sonarqube`                                                              |
| 🚨 Q66  | SonarQube job fails immediately. First thing to check? | 👉 `docker logs sonarqube`                                                                 |
| 🚨 Q67  | SonarQube container exits immediately. Common reason?  | 👉 `vm.max_map_count` not configured.                                                      |
| 🚨 Q68  | Why integrate SonarQube in CI pipeline?                | 👉 Shift-left quality and security checks.                                                 |
| 🚨 Q69  | Why fail pipeline on Quality Gate failure?             | 👉 Prevent bad-quality code reaching production.                                           |
| 🚨 Q70  | Why doesn't SonarQube need build artifacts?            | 👉 `It analyzes source code`, not `dist output`.                                               |

