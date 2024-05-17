# Encrypt ETCD

Link: [Encrypting Confidential Data at Rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)

Run these steps in the master node

 1. Create a base64 encryption key:

    ```
    head -c 32 /dev/urandom | base64
    ```

 1. Create the `EncryptionConfiguration` spec and save it as `/etc/kubernetes/enc/enc.yaml`.

 1. Update API server spec (`/etc/kubernetes/manifests/kube-apiserver.yaml`) with the following:

    1. Add the parameter: `--encryption-provider-config=/etc/kubernetes/enc/enc.yaml`
    1. Create a hostpath volume for `/etc/kubernetes/enc`
    1. Mount the new hostpath volume in `/etc/kubernetes/enc`

 1. Recreate all secrets `kubectl get secrets --all-namespaces -o json | kubectl replace -f -`

To verify (after and before) that ECTD es encrypted, you can query a secret:

```
ETCDCTL_API=3 etcdctl \
   --cacert=/etc/kubernetes/pki/etcd/ca.crt   \
   --cert=/etc/kubernetes/pki/etcd/server.crt \
   --key=/etc/kubernetes/pki/etcd/server.key  \
   get /registry/secrets/default/secret1
```
