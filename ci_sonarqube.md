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


Go to: ***GitHub Repo → Settings → Secrets → Actions*
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
