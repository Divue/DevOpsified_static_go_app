# System Architecture

```mermaid
flowchart LR

%% ---------- DEV ----------
A[Developer] --> B[GitHub Repository]

%% ---------- CI ----------
subgraph CI Pipeline (GitHub Actions)
B --> C[Build & Test]
C --> D[Run Linter]
D --> E[Build Docker Image]
E --> F[Push Image to DockerHub]
F --> G[Update Helm values.yaml Tag]
G --> H[Commit Back to Repo]
end

%% ---------- GITOPS ----------
subgraph GitOps
H --> I[ArgoCD Watches Repo]
I --> J[Sync Application]
end

%% ---------- K8S ----------
subgraph AWS EKS Cluster
J --> K[Helm Chart]
K --> L[Deployment]
K --> M[Service]
K --> N[Ingress]
N --> O[AWS Load Balancer]
end

%% ---------- USER ----------
O --> P[User Browser]
