# microservice-social-learn-gitops

GitOps repository for Social App on AKS.

## Structure

- `apps/microservice-social-learn`: Helm chart
- `argocd/applications`: ArgoCD Application manifests

## Deploy dev app

```bash
kubectl apply -f argocd/applications/social-app-dev.yaml

## Setup ArgoCD on AKS
```bash
# 1. Cài ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 2. Lấy mật khẩu admin
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d && echo

# 3. Kết nối GitOps repo private (dùng PAT)
kubectl -n argocd create secret generic gitops-repo-secret \
  --from-literal=type=git \
  --from-literal=url=https://github.com/vanlailaptrinh/microservice-social-learn-gitops.git \
  --from-literal=username=vanlailaptrinh \
  --from-literal=password=<GITHUB_PAT>
kubectl -n argocd label secret gitops-repo-secret argocd.argoproj.io/secret-type=repository

# 4. Cho AKS pull được image từ ACR
kubectl create secret docker-registry acr-secret \
  --namespace social-app-dev \
  --docker-server=sociallearn.azurecr.io \
  --docker-username=<ACR_USERNAME> \
  --docker-password=<ACR_PASSWORD>

# 5. Apply Application và theo dõi
kubectl apply -f argocd/applications/social-app-dev.yaml
kubectl -n argocd get application social-app-dev -w

```


