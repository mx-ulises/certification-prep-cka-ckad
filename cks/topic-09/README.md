# Enable Admission Control Plugins

Links: [Admission Controllers Reference](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)

Admission Control Plugins help to intercept requests and refuse them depending on what plugins we have, so it will make it more secure. The Plugins are added as part of the api server `Pod` parameters, and you can modify them by modifying the API server manifest in the master nodes (`/etc/kubernetes/manifests/kube-apiserver.yaml`):

```
--enable-admission-plugins=...
```

One common Plugin is `NodeRestriction` as this prevent nodes to be modified by API calls comming from outside the node itself.
