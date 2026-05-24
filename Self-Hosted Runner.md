# 🔧 What is a Self-Hosted Runner?
 * ⚡ A self-hosted runner is a Linux machine (`VM` / `server` / `laptop`) that executes GitHub Actions workflows instead of `GitHub’s cloud runners`.

## 1️⃣ 🪜 Step-by-Step Setup
🌿 Go to Your GitHub Repository
```hcl
Repo → Settings → Actions → Runners → New self-hosted runner
```

* 🚀 GitHub will show commands specific to your repo (`token is unique`).
```hcl
mkdir actions-runner
cd actions-runner
curl -o actions-runner-linux-x64.tar.gz -L \
https://github.com/actions/runner/releases/download/v2.312.0/actions-runner-linux-x64-2.312.0.tar.gz
tar xzf actions-runner-linux-x64.tar.gz
```

## 2️⃣ Configure Runner ***⚠️ Copy token from GitHub page*
```
----------------------------------------------------
```

🌿 Start Runner (`Foreground`)
```hcl
./run.sh
```

## 🔄 Run Runner as a Service (`Recommended`)
```hcl
sudo ./svc.sh install
sudo ./svc.sh start
sudo ./svc.sh status
```

## 🧪 Verify Runner
 * 🚀 Go to :
    * Repo → Settings → Actions → Runners
    * You should see: ● `ubuntu-runner` (🌿Idle)

# Optional
## 🚀 Check & enable Docker service
```hcl
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```

## 🚀 start minikube 
```hcl
minikube start
```

## 🚀 start sonarqube
```hcl
docker start sonarqube
```
