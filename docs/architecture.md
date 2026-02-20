# Architecture

```mermaid
flowchart TD
A[Developer Push] --> B[GitHub Repo]
B --> C[CI Build and Test]
C --> D[Build Docker Image]
D --> E[Push Image to DockerHub]
E --> F[Update Helm Tag]
F --> G[ArgoCD Detects Change]
G --> H[Deploy to EKS]
H --> I[Ingress]
I --> J[AWS Load Balancer]
J --> K[User Browser]
