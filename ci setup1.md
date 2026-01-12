# SonarQube
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
