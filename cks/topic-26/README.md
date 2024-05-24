# Restrict container's resource access with Apparmor

Link: [Restrict a Container's Access to Resources with AppArmor](https://kubernetes.io/docs/tutorials/security/apparmor/)

Apparmor is a module installed in linux that help to restrict specific operations to the apps by creating profiles. The profiles can be passed to containers to prevent them to run undesired operations.

To list all the profile available, use following command in worker nodes:

```
sudo cat /sys/kernel/security/apparmor/profiles | sort
```

To force a `Pod` to run with some specific Apparmor profile, you can add the following security context in the spec:

```
apiVersion: v1
kind: Pod
...
spec:
  securityContext:
    appArmorProfile:
      type: Localhost
      localhostProfile: k8s-apparmor-example-deny-write
...
```
