# 🎯 Visual Diagram: Multi-Environment Deployment in GitHub Actions
## 💬 Real-Time Example Answer for Interviews:
 In my last project, we used GitHub Actions to deploy a microservices-based app to three EKS clusters: dev, staging, and prod.

### 🌿 We handled deployments based on the branch name:
   * 🌱 develop branch  →  dev cluster
   * 🧪 staging branch  →  staging cluster
   * 🚀 main branch     →  production cluster  

🔐 Each environment had separate Kubernetes contexts and credentials, securely managed using GitHub Environments and encrypted secrets.

## 🔄 Visual Flow
```yaml
Developer Push
      ↓
GitHub Actions Trigger 🔔
      ↓
Build Docker Image 🐳
      ↓
Push to ECR 📦
      ↓
Deploy Based on Branch 🎯
      ↓
EKS Cluster (dev / staging / prod)
```

 * 📌 For example, we used: `if: github.ref == 'refs/heads/staging'`  to conditionally deploy only to `staging`.
 * ⚙️ The pipeline included `build`, `security scan` (Trivy), `push to ECR`, and then used `kubectl` with environment-specific `kubeconfig` files to apply manifests.
 * 🛑 We also used manual approval for production using GitHub’s `environment: production` with `protection rules`.
 * ✅ This setup gave us `full control`, `traceability`, and `CI/CD automation` across all environments.

---

## ✨ Bonus If You Want to Add More:

“I also structure the workflow to use if: conditions to prevent accidental deployments, and I use environment-specific secrets to securely manage AWS credentials or kubeconfig files. This gives clear separation and control over each environment.”

---
```yaml
name: Deploy to EKS Based on Branch  

on:  
  push:  
    branches: [ develop, staging, main ]  

env:  
  ECR_REGISTRY: 123456789012.dkr.ecr.us-east-1.amazonaws.com  
  IMAGE_NAME: my-app  

jobs:  
  build:  
    runs-on: ubuntu-latest  

    steps:  
      - name: Checkout code  
        uses: actions/checkout@v3  

      - name: Configure AWS credentials  
        uses: aws-actions/configure-aws-credentials@v3  
        with:  
          role-to-assume: arn:aws:iam::123456789012:role/GitHubActionsRole  
          aws-region: us-east-1  

      - name: Login to Amazon ECR  
        run: aws ecr get-login-password | docker login --username AWS --password-stdin ${{ env.ECR_REGISTRY }}  

      # 🐳 Build Docker image  
      - name: Build Docker image  
        run: |  
          docker build -t $IMAGE_NAME:${{ github.sha }} .  
          docker tag $IMAGE_NAME:${{ github.sha }} $ECR_REGISTRY/$IMAGE_NAME:${{ github.sha }}  

      - name: Push to ECR  
        run: docker push $ECR_REGISTRY/$IMAGE_NAME:${{ github.sha }}  

  deploy:  
    needs: build  
    runs-on: ubuntu-latest  

    environment:  
      name: ${{ github.ref_name }}  # dev, staging, main  

    steps:  
      - name: Configure AWS credentials  
        uses: aws-actions/configure-aws-credentials@v3  
        with:  
          role-to-assume: arn:aws:iam::123456789012:role/GitHubActionsRole  
          aws-region: us-east-1  

      - name: Set up kubectl  
        uses: azure/setup-kubectl@v3  
        with:  
          version: latest  

      - name: Configure kubeconfig  
        run: aws eks update-kubeconfig --name my-cluster-${{ github.ref_name }}  

      - name: Deploy to EKS  
        run: |  
          kubectl set image deployment/my-app-deployment my-app-container=$ECR_REGISTRY/$IMAGE_NAME:${{ github.sha }}  
          kubectl rollout status deployment/my-app-deployment  
```
