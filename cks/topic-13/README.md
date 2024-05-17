# Create Pods with sidecar and Security context

Links: [Configure a Security Context for a Pod or Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

Containers inside a Pod can gain root capabilities using security context, so these can be added granularly.

You can cehck capabilities by container using the following command:

```
kubectl exec <pod> -c <container> -it -- cat /proc/1/status | grep Cap
```
