# Disable/Enable and test anonimous access to API server

Links: [Kubelet authentication/authorization](https://kubernetes.io/docs/reference/access-authn-authz/kubelet-authn-authz/)

To access API server you can use

```
# Curl API Server
curl https://127.0.0.1:49208 -k
```

To Modify API server to :

```
# Open manifest of API server from controller node
vim /etc/kubernetes/manifests/kube-apiserver.yaml

# Add flag --anonymous-auth=false
```

If anonymous access is enabled, you will get this message:

```
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}
```

If anonymous access is disabled, you will get this message:

```
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}
```
