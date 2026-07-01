# microservice-social-learn-gitops

GitOps repository for Social App on AKS.

## Structure

- `apps/microservice-social-learn`: Helm chart
- `argocd/applications`: ArgoCD Application manifests

## Deploy dev app

```bash
kubectl apply -f argocd/applications/social-app-dev.yaml