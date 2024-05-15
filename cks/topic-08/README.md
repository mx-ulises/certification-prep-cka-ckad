# Create account without mounted Service Account

Links: [Configure Service Accounts for Pods](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

 - Service Account secret automounting in a `Pod` can be disabled with flag in `Pod` spec: `automountServiceAccountToken: false`
 - Service Account secret automounting in a `Pod` can be disabled with flag in `ServiceAccount` spec: `automountServiceAccountToken: false`

We can describe the `Pod` and check for secret mounted for the service account with `kubectl describe pod | less`.
