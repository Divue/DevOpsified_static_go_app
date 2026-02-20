# System Architecture

```mermaid
flowchart TD

A[Developer Push] --> B[GitHub Repo]

subgraph CI
B --> C[Build & Test]
C --> D[Lint]
D --> E[Build Docker Image]
E --> F[Push Image to DockerHub]
F --> G[Update Helm Tag]
G --> H[Commit Back to Repo]
end

subgraph GitOps
H --> I[ArgoCD Watches Repo]
I --> J[Sync Application]
end

subgraph EKS Cluster
J --> K[Helm Chart]
K --> L[Deployment]
L --> M[Service]
M --> N[Ingress]
N --> O[AWS LoadBalancer]
end

O --> P[User Browser]
