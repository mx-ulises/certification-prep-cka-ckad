# Delete shell from Pod with Startup Probe

Link: [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

When creating a Pod, is a good practice to delete the shell/bash access to it. We can do it from the image but it is also posible to do it in `Pod` definition using `startupProbe`:

```
apiVersion: v1
kind: Pod
...
spec:
  containers:
  - image: httpd
    startupProbe: # This will run at the begining of each container with shell
      exec:
        command: # This command delete the Bash from the pof
        - rm
        - /bin/bash # You need to know the path of the bash
      initialDelaySeconds: 5
      periodSeconds: 5
...
```
