## Installing Kasten using argocd on AKS cluster

In this example, we will install kasten's K10 helm chart using ArgoCD on a Openshift cluster with token authentication.

### Pre-requisites 
If you haven't setup the Argo CD on the kubernetes cluster, follow the instructions [here](https://github.com/smohandass/kasten-argocd-k8s/blob/main/README.md)

Review this [blog](https://www.kasten.io/kubernetes/resources/blog/posts/using-azure-ad-with-kasten-k10-for-authentication-and-authorization) for the setup that is needed on Azure active directory for Kasten authentication to work.

Fork the repo to your github account and update the values.yaml file according to your install

### Create Argo CD application

Create an ArgoCD application using the following yaml to install Kasten K10 chart. Update the repoURL to match your Github Repo.

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: k10
  namespace: argocd
  finalizers:
    - resources-finalizer.argocd.argoproj.io
spec:
  destination:
    namespace: kasten-io
    server: 'https://kubernetes.default.svc'
  source:
    path: openshift/kasten-install-token-auth
    repoURL: https://github.com/smohandass/kasten-argocd-k8s.git
    targetRevision: main
    helm:
      valueFiles:
        - values.yaml 
  project: default
  syncPolicy:
    managedNamespaceMetadata:
      labels:
        argocd.argoproj.io/managed-by: argocd
    automated: 
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

Verify the argoCD is successfully created and status shows synced. It might take a few minutes for the operator and deployment to be created and status to shows Synced.

```
kubectl get app -n argocd
NAME         SYNC STATUS   HEALTH STATUS
kasten-app   Synced        Healthy
```

Verify that all k10 pods are successfully running 

```
kubectl get pods -n kasten-io
```
