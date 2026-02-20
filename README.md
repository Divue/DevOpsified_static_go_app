# DevOpsified Go Web Application (EKS + GitOps)

A simple Go web app deployed using a full cloud-native DevOps workflow: CI/CD, containerization, GitOps, Helm, and Kubernetes on AWS EKS.  
The focus is not the app itself, but the deployment architecture and automation around it.

This repository shows the complete path from a code push to a running application in a Kubernetes cluster with zero manual deployment steps.

---

## Architecture

Full diagram:  
`docs/architecture.md`

**Flow**

Developer push → GitHub → CI builds & tests → Docker image built → pushed to registry → Helm tag updated → ArgoCD detects change → deploys to EKS → Ingress exposes app → Load balancer serves users

---

## Stack

- Go (net/http)
- Docker (multi-stage + distroless)
- Kubernetes
- Helm
- AWS EKS
- GitHub Actions
- ArgoCD
- NGINX Ingress Controller
- DockerHub

---

## What’s Implemented

### Containerization
- Multi-stage Docker build
- Distroless runtime image
- Minimal attack surface
- Small image size
- Exposes port 8080

### Kubernetes
- Helm-based deployment
- Deployment + Service + Ingress
- Parameterized image tags
- AWS Load Balancer integration
- Runs on EKS

### CI Pipeline
Runs on every push to `main`:

- Build Go binary
- Run tests
- Run lint
- Build Docker image
- Push image to DockerHub
- Update Helm image tag automatically
- Commit updated tag back to repo

**CI run**

![CI](docs/screenshots/01_ci_success.png)

---

### GitOps Deployment
- ArgoCD watches the repository
- Helm tag update triggers sync
- Cluster state reconciled automatically
- No manual kubectl deploys

**ArgoCD sync**

![ArgoCD](docs/screenshots/02_argocd_sync.png)

---

### Cluster State

Pods running:

![Pods](docs/screenshots/03_k8s_pods_running.png)

EKS nodes:

![Nodes](docs/screenshots/04_eks_nodes.png)

---

### Application Access

- Ingress configured
- AWS load balancer provisioned
- App reachable externally

![App](docs/screenshots/05_app_ui.png)

---

## End-to-End Flow

1. Push code
2. CI builds and tests
3. Docker image built
4. Image pushed to registry
5. Helm values updated
6. Commit pushed back
7. ArgoCD detects change
8. ArgoCD syncs to cluster
9. Kubernetes rolls update
10. Ingress routes traffic
11. App live

Fully automated GitOps loop.

---

## Run Locally

```bash
go run main.go

http://localhost:8080/courses
