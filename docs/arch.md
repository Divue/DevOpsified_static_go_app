# System Architecture

```mermaid
flowchart TD

A[Developer Push to GitHub] --> B[GitHub Actions CI]

B --> C[Build Go Binary]
B --> D[Run Tests]
B --> E[Run Lint]
B --> F[Build Docker Image]

F --> G[Push Image to Docker Hub]

G --> H[Update Helm values.yaml with new image tag]

H --> I[Git Commit from CI]

I --> J[ArgoCD GitOps Controller]

J --> K[Sync Kubernetes Cluster]

K --> L[Helm Chart Deployment]

L --> M[Kubernetes Deployment]
L --> N[Service]
L --> O[Ingress]

O --> P[AWS Load Balancer]

P --> Q[User Browser]

subgraph AWS EKS Cluster
M
N
O
end

subgraph CI Pipeline
B
C
D
E
F
G
H
I
end

subgraph GitOps
J
K
end


**Flow**

Developer → GitHub  
→ CI builds image  
→ pushes to registry  
→ updates Helm tag  
→ ArgoCD detects change  
→ deploys to EKS  
→ Ingress exposes app  

That is a full GitOps loop.
