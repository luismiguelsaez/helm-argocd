Description
===========

Create ArgoCD server for testing purposes

### Install ArgoCD chart
```
helm install argocd argo/argo-cd --create-namespace  -f values.yml --version 2.11.0
```

### Generate admin password
```
export ARGO_PWD=admin
`htpasswd -nbBC 10 "" $ARGO_PWD | tr -d ':\n' | sed 's/$2y/$2a/'`
```

### Check secret password
```
kgsec argocd-secret -o jsonpath="{.data.admin\.password}" | base64 -d
kgsec argocd-secret -o jsonpath="{.data.admin\.passwordMtime}" | base64 -d
kubectl port-forward svc/argocd-server 8080:80
```

### Create application from helm repository
```
keti argocd-server-5db44cd58f-nsk6z -- argocd login 127.0.0.1:8080 --username admin --password admin --insecure
keti argocd-server-5db44cd58f-nsk6z -- argocd app create nginx --repo https://github.com/luismiguelsaez/helm-charts --revision HEAD --path nginx --dest-namespace default --dest-server https://kubernetes.default.svc --server 127.0.0.1:8080 --insecure --sync-policy none
```
