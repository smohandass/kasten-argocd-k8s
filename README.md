# kasten-argocd-k8s
Argo CD is an open-source continuous delivery (CD) tool to automate the deplyment and management of applications on kuberenetes clusters. This project demonstrates on how to use argoCD to install Kasten's K10 and automate K10 actions on kuberentes clusters.

## Table of Contents

1. Prerequisites
2. Installing Kasten using ArgoCD application on AKS cluster

### Prerequisites
Prior to applying the examples defined in this project, ArgoCD need to be setup on the cluster. 

### Install Argo CD using the following commands

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Installing argocd cli (optional)

`
brew install argocd
`

### Accessing argo cd dashboard

Kubectl port-forwarding can also be used to connect to the API server without exposing the service.

`
kubectl port-forward svc/argocd-server -n argocd 8080:443
`

The API server can then be accessed using https://localhost:8080

### Get the Argo CD admin user password to login

To get the password, run
`
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
`

Login to the UI as admin user and the password retrieved from the above command.

## Installing Kasten

Installing Kasten on AKS using Active Directory Authentication : [aks/kasten-helm-install/README.md](https://github.com/smohandass/kasten-argocd-k8s/edit/main/aks/kasten-helm-install/README.md)
