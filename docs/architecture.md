# System Architecture

```mermaid
flowchart TD

%% ======================
%% DEVELOPER
%% ======================
A[Developer Push] --> B[GitHub Repository]

%% ======================
%% CI PIPELINE
%% ======================
subgraph CI_GitHub_Actions
B --> C[Build Binary]
C --> D[Run Tests]
D --> E[Run Lint]
E --> F[Build Docker Image]
F --> G[Push Image to DockerHub]
G --> H[Update Helm values.yaml]
H --> I[Commit Back to Repo]
end

%% ======================
%% GITOPS
%% ======================
subgraph GitOps_ArgoCD
I --> J[ArgoCD Watches Repo]
J --> K[Detects Change]
K --> L[Sync to Cluster]
end

%% ======================
%% EKS CLUSTER
%% ======================
subgraph AWS_EKS
L --> M[Helm Chart]
M --> N[Deployment]
N --> O[Service]
O --> P[Ingress]
P --> Q[AWS LoadBalancer]
end

%% ======================
%% USER
%% ======================
Q --> R[User Browser]
