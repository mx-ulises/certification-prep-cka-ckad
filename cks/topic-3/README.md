# Deploy and Secure Kubernetes Dashboard

Link: [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

To deploy the Dashboard UI use following `helm` commands:

```
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```

A bearer token is required:

```
kubectl -n dashboard-role create token dashboard-sa
```

New service accounts might not have the access you need, you can get access by assigning RBAC roles (or cluster roles):

```
kubectl create clusterrolebinding dashboard-rb --clusterrole=view --serviceaccount=dashboard-role:dashboard-sa
```


