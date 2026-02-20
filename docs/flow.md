# Deployment Flow

1. Developer pushes code
2. GitHub Actions runs CI
3. Docker image built and pushed
4. CI updates Helm chart tag
5. Commit pushed back to repo
6. ArgoCD detects change
7. ArgoCD syncs to EKS
8. Kubernetes rolls deployment
9. Ingress exposes service
10. AWS Load Balancer serves traffic
