# Argocd Kubernetes GitOps

Setup and testing instructions for deploying ArgoCD on Kubernetes and managing applications with GitOps.

## Prerequisites

- kubectl installed and configured
- Helm installed
- A Kubernetes cluster
- A GitHub account

## Install ArgoCD on your Cluster
```
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
kubectl create namespace argocd
helm install argocd argo/argo-cd --namespace argocd --version 7.7.0
```

## Access ArgoCD UI

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

## Retrieve Credentials

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## Testing

Use the following commands to test the Grade Submission API. **Windows users should use Git Bash**.

```bash
curl -X POST http://localhost:<port>/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Harry", "subject": "Defense Against Dark Arts", "score": 95}'

curl -X POST http://localhost:<port>/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Ron", "subject": "Charms", "score": 82}'

curl -X POST http://localhost:<port>/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Hermione", "subject": "Potions", "score": 98}'
```

Verify the grades were added:
```bash
curl http://localhost:<port>/grades
```
