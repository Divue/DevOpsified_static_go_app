# System Architecture

This diagram shows the full CI/CD + GitOps deployment flow from developer commit to production traffic on AWS EKS.

```mermaid
flowchart LR

%% --- DEV ---
A[Developer] -->|git push| B[GitHub Repository]

%% --- CI PIPELINE ---
subgraph CI_Pipeline_GitHub_Actions
B --> C[Build Go Binary]
C --> D[Run Tests]
D --> E[Run Lint]
E --> F[Build Docker Image]
F --> G[Push Image to DockerHub]
G --> H[Update Helm values.yaml Tag]
H --> I[Commit Back to Repo]
end

%% --- GITOPS ---
subgraph GitOps_ArgoCD
I --> J[ArgoCD Watches Repository]
J --> K[Detects Helm Change]
K --> L[Sync Application]
end

%% --- KUBERNETES ---
subgraph AWS_EKS_Cluster
L --> M[Helm Chart]
M --> N[Kubernetes Deployment]
N --> O[Service ClusterIP]
O --> P[Ingress NGINX]
P --> Q[AWS Load Balancer]
end

%% --- USER ---
Q --> R[User Browser]
