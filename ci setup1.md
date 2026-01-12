# SonarQube
7️⃣ SonarQube Job in GitHub Actions
```
sonarqube:
  runs-on: self-hosted
  needs: build

  steps:
    - uses: actions/checkout@v4

    - name: SonarQube Scan
      run: |
        sonar-scanner \
        -Dsonar.projectKey=my-react-project \
        -Dsonar.sources=src \
        -Dsonar.host.url=${{ secrets.SONAR_HOST_URL }} \
        -Dsonar.login=${{ secrets.SONAR_TOKEN }} \
        -Dsonar.qualitygate.wait=true
```


## 1️⃣ Install SonarQube Server (Docker – Recommended && Docker alredy installed)
▶️ Run SonarQube using Docker
```
docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  sonarqube:lts
```

## 2️⃣ Create SonarQube Project
➕ Steps in SonarQube UI
> Login to SonarQube : Click Projects → Create Project → Choose Manual
```
📌 Project Key: my-react-project
📌 Display Name: React CI Project
```
> 📌 Project Key must match: -Dsonar.projectKey=my-react-project

## 3️⃣ Generate SonarQube Token (VERY IMPORTANT)
🔐 Steps
```
Click Profile (top-right)
My Account → Security
Generate Token:
          Name: github-actions
          Type: Global Analysis Token
📋 Copy the token (shown only once)
```
## 4️⃣ Add GitHub Secrets
Go to: ***GitHub Repo → Settings → Secrets → Actions*
| Secret Name      | Value                     |
| ---------------- | ------------------------- |
| `SONAR_HOST_URL` | `http://host.docker.internal:9000` |
| `SONAR_TOKEN`    | `<copied_token>`          |

## 5️⃣ Install Sonar Scanner on Self-Hosted Runner
🚨 Mandatory – GitHub runner does NOT include sonar-scanner
### ▶️ Install Sonar Scanner
```
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
sudo unzip sonar-scanner-cli-5.0.1.3006-linux.zip
sudo mv sonar-scanner-5.0.1.3006-linux sonar-scanner
```
## ▶️ Add to PATH
```
echo 'export PATH=$PATH:/opt/sonar-scanner/bin' | sudo tee -a /etc/profile
source /etc/profile
```
✅ Verify
```
sonar-scanner --version
```
## 6️⃣ Required Linux Kernel Settings (IMPORTANT)
SonarQube needs this setting:
```
sudo sysctl -w vm.max_map_count=262144
```
Make it permanent:
```
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
```



# Access SonarQube from Browser
You already have SonarQube running correctly inside Docker 👍
> The only thing left is how to access it from your browser when your Ubuntu VM uses VirtualBox NAT.

## ✅ STEP 1: Add Port Forwarding in VirtualBox
```
📌Power OFF your Ubuntu VM
Open VirtualBox
Select your Ubuntu VM → Settings
Go to Network
Adapter 1 → Attached to: NAT
Click Advanced
✅Click Port Forwarding

📌Add a rule:
Name	sonarqube
Protocol	TCP
Host IP	(leave empty)
Host Port	9000
Guest IP	(leave empty)
Guest Port	9000
✅ Click OK
```
Start the VM


## ✅ STEP 2: Fix vm.max_map_count (MANDATORY) && Verify:
```
sudo sysctl -w vm.max_map_count=262144
sysctl vm.max_map_count
```
## 🔄 STEP 3: Restart SonarQube Container
```
docker restart sonarqube
```

Wait 1–2 minutes ⏳
> (SonarQube is heavy — it does NOT start instantly)

## 🧪 STEP 4: Test Again (VERY IMPORTANT)

Inside Ubuntu VM:
```
curl http://localhost:9000
```
✅ If you see HTML output, SonarQube is now working.

## 🌍 STEP 5: Access from Windows (NAT Mode)
After this works inside VM, open browser on Windows host:
```
http://localhost:9000
```
(Default login: admin / admin)

✅ Quick Diagnostic Commands for sonarqube
```
docker ps
docker logs sonarqube
ss -tulnp | grep 9000
sysctl vm.max_map_count
```
## ~~~~~~~~~~~~~~~~~~~~ 🌍 ~~~~~~~~~~~~~~~~~~~
