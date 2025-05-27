# Demo of External Secret Operator and ArgoCD

# Table of Contents
- [Demo of External Secret Operator and ArgoCD](#demo-of-external-secret-operator-and-argocd)
- [Table of Contents](#table-of-contents)
- [Structure of demo-app](#structure-of-demo-app)
- [Technical Environnement](#technical-environnement)
    - [System Requirements](#system-requirements)
  - [Stack Deployment](#stack-deployment)
  - [Install External Secrets Operator (ESO)](#install-external-secrets-operator-eso)
  - [Install ArgoCD](#install-argocd)
  - [Create Service Account](#create-service-account)


# Structure of demo-app
```bash
demo-app/
├── demo-app-deployment.yaml
├── demo-app-service.yaml
├── demo-eso-store-role.yaml
├── demo-external-secret.yaml
├── demo-secret-store.yaml
└── demo-secret.yaml
```
# Technical Environnement
The table below shows the tools and solutions used in the project:

| Item    | Technology/ Tool | Version |
| -------- | ------- | ------- |
| VCS           | GitHub | N/A|
| Container Deployment | MiniKube |  1.34.0 |

### System Requirements
* Recommended system requirements: 1GB RAM, x86 or arm64 CPU
* Minimum system requirements: 512MB RAM + swap

## Stack Deployment
The application stack can be run using:
1. Docker
2. Using Kubernetes (Minikube)

## Install External Secrets Operator (ESO)

Install ESO in cluster via Helm:

```bash
helm repo add external-secrets https://charts.external-secrets.io
helm repo update
helm install external-secrets external-secrets/external-secrets -n external-secrets --create-namespace
```

## Install ArgoCD

Install via Helm:

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

Create the namespace:

```bash
kubectl create namespace argocd
```
Install ArgoCD:

```bash
helm install argocd argo/argo-cd -n argocd
```

To access the ArgoCD web UI locally:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```


-> Username: admin

-> Password: 
Retrieve with:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

## Create Service Account

```bash
kubectl create serviceaccount demo-store -n ns-demo
```