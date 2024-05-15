# Commands to communicate to different pods/services

Links: [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

```

# v1 to v2 (should be allowed with network policy)
kubectl exec nginx-app-v1  -n network-policy-1 -- curl nginx-app-v2.network-policy-1 2>/dev/null

# v2 to v1 (should not be allowed with network policy)
kubectl exec nginx-app-v2  -n network-policy-1 -- curl nginx-app-v1.network-policy-1 2>/dev/null

# v1 to v3 (cross-namespace, should be allowed with network policy)
kubectl exec nginx-app-v1  -n network-policy-1 -- curl nginx-app-v3.network-policy-2 2>/dev/null

```
