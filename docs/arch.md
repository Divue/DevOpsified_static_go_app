# System Architecture

```mermaid
flowchart TD

A[Developer Push] --> B[GitHub Repo]

B --> C[GitHub Actions CI]

C --> D[Build Go Binary]
C --> E[Run Tests]
C --> F[Run Lint]
C --> G[Build Docker Image]

G --> H[Push Image to DockerHub]

H --> I[Update Helm values.yaml with new image tag]

I --> J[ArgoCD detects change]

J --> K[Sync to EKS Cluster]

K --> L[Kubernetes Deployment]
L --> M[Service]
M --> N[Ingress]

N --> O[AWS Load Balancer]

O --> P[User Browser]
