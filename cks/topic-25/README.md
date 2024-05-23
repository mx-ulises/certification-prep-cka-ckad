# Enable and use Kubernetes Audit log

Links: [Auditing](https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/)

To enable Audit Logs, you need to follow these steps:

 1. Create an audit policy file: [`audit-policy.yaml`](audit-policy.yaml) and store it in `/etc/kubernetes/audit/audit-policy.yaml` in master node:
    ```
    apiVersion: audit.k8s.io/v1 # This is required.
    kind: Policy
    rules:
    # Log configmap and secret changes in all other namespaces at the Metadata level.
    - level: Metadata
        resources:
        - group: "" # core API group
        resources: ["secrets", "configmaps"]
    ```
 1. Update [`kube-apiserver.yaml`](kube-apiserver.yaml) to use audit logs flags:
    ```
    - --audit-policy-file=/etc/kubernetes/audit/audit-policy.yaml
    - --audit-log-path=/var/log/kubernetes/audit/logs/audit.log
    - --audit-log-maxage=1
    - --audit-log-maxbackup=2
    - --audit-log-maxsize=10
    ```
 1. Also add the volume mounts for audit policy and audit logs:
    ```
    volumeMounts:
    - mountPath: /etc/kubernetes/audit/audit-policy.yaml # Audit log
      name: audit # Audit log
      readOnly: true # Audit log
    - mountPath: /var/log/kubernetes/audit/logs/ # Audit log
      name: audit-log # Audit log
      readOnly: false # Audit log
    ```
 1. And configure the `hostPath` volumes:
    ```
    volumes:
    - name: audit # Audit log
      hostPath: # Audit log
        path: /etc/kubernetes/audit/audit-policy.yaml # Audit log
        type: File # Audit log
    - name: audit-log # Audit log
      hostPath: # Audit log
        path: /var/log/kubernetes/audit/logs/ # Audit log
        type: DirectoryOrCreate # Audit log
    ```
 1. After you make the updates, the `kube-apiserver` pod will restart and emiting the logs.

To access the logs, you can go to `/var/log/kubernetes/audit/logs/` in master node and review what is printed there.

```
tail -f /var/log/kubernetes/audit/logs/audit.log
```