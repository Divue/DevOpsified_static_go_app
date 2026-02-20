Developer
   ↓ push
GitHub Repository
   ↓
GitHub Actions (CI)
   - Build
   - Test
   - Lint
   - Build Docker image
   - Push to DockerHub
   - Update Helm tag
   ↓
Helm values.yaml updated
   ↓
ArgoCD watches repo
   ↓
ArgoCD syncs to EKS
   ↓
Kubernetes Deployment
   ↓
Service
   ↓
Ingress
   ↓
AWS Load Balancer
   ↓
User Browser


**Flow**

Developer → GitHub 
→ CI builds image 
→ pushes to registry 
→ updates Helm tag
→ ArgoCD detects change 
→ deploys to EKS 
→ Ingress exposes app 

That is a full GitOps loop.
