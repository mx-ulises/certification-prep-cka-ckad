# Create Pods with sidecar and Security context

Links: [Configure a Security Context for a Pod or Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

## Gain Root Capabilities

Containers inside a Pod can gain root capabilities using security context, so these can be added granularly.

You can cehck capabilities by container using the following command:

```
kubectl exec <pod> -c <container> -it -- cat /proc/1/status | grep Cap
```
## Change Container User and Group

To change the user and or group, we can modify `pod` spec to do that. By default, the `pod` is using `root` as user and group. To test this properly, is recommended to use `busybox` image running `sh -c "sleep 3600"` command. We can verify user id and groups with this command:

```
kubectl exec <pod> -it -- id
# default: uid=0(root) gid=0(root) groups=0(root)
# changed: uid=1000 gid=3000 groups=2000,3000
```
