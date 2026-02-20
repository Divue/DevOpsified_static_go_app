# Architecture

```mermaid
flowchart TD

A[Developer Push] --> B[GitHub Repo]

subgraph CI
B --> C[Build]
C --> D[Test]
D --> E[Lint]
E --> F[Build Docker Image]
F --> G[Push Image]
G --> H[Update Helm Tag]
H --> I[Commit Tag]
end

subgraph GitOps
I --> J[ArgoCD Watch]
J --> K[ArgoCD Sync]
end

subgraph EKS
K --> L[Helm Chart]
L --> M[Deployment]
M --> N[Service]
N --> O[Ingress]
O --> P[LoadBalancer]
end

P --> Q[User]
