# Create Pod with read-only filesystem

Links:
  - [Configure a Security Context for a Pod or Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)
  - [Volume](https://kubernetes.io/docs/concepts/storage/volumes/)

You can set the whole filesystem of your contaieners in a `Pod` using `readOnlyRootFilesystem: true` in the security context.

If you need write some of the directories, use mount volumes so they are writable.

```
apiVersion: v1
kind: Pod
...
spec:
  containers:
  - image: httpd
...
    securityContext:
      readOnlyRootFilesystem: true # Filesystem cannot be writen
    volumeMounts:
        - name: httpd-logs
          mountPath: /usr/local/apache2/logs # We mount a volume in the directories that we need to write
...
status: {}
```
