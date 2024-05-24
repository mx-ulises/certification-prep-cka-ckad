# # Restrict container's syscalls with seccomp

Links: [Restrict a Container's Syscalls with seccomp](https://kubernetes.io/docs/tutorials/security/seccomp/)

You can restrict specific specific system calls (syscalls) with Seccomp. You need to follow these steps in the kubelet of the nodes:

 1. Create a directory `/var/lib/kubelet/seccomp/profiles` in the nodes.
 1. Move your profile files there.
 1. Create a pod that loads one of the profiles by using a security context:

```
apiVersion: v1
kind: Pod
...
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
...
```

