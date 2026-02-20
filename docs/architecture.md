# Architecture

```mermaid
flowchart TD

%% Developer
A[Developer] --> B[GitHub Repository]

%% CI
subgraph CI Pipeline
B --> C[Build & Test]
C --> D[Lint]
D --> E[Build Docker Image]
E --> F[Push Image to Registry]
F --> G[Update Helm values.yaml]
G --> H[Commit Tag Update]
end

%% GitOps
subgraph GitOps (ArgoCD)
H --> I[Watch Repo]
I --> J[Sync Application]
end

%% Cluster
subgraph AWS EKS Cluster
J --> K[Helm Chart]
K --> L[Deployment]
L --> M[Service]
M --> N[Ingress]
N --> O[AWS Load Balancer]
end

%% User
O --> P[User Browser]
