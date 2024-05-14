# Authenticate an User

Links: [Certificate Signing Requests](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#normal-user)

1. Create Cert and Key
    ```
    # Create Key
    openssl genrsa -out key.pem 2048

    # Create CSR
    openssl req -new -key key.pem -out csr.pem
    ```

1. Get enconded string of the CSR file, it will be used in the CSR YAML:
    ```
    cat csr.pem | base64 | tr -d "\n"
    ```

1. Create and apply the Certificate Signing Request YAML:
    ```
    kubectl apply -f ulisesmx-csr.yaml
    ```

1. Get and approve CSR Request:
    ```
    # Check status of CSR Requests
    kubectl get csr

    # Approve CSR Request
    kubectl certificate approve ulisesmx
    ```

1. Export the certificate to a file
    ```
    kubectl get csr ulisesmx -o jsonpath='{.status.certificate}'| base64 -d > ulisesmx.crt
    ```
1. Create a new user and a new context
    ```
    # Create new credentials for new context
    kubectl config set-credentials ulisesmx --client-key=key.pem --client-certificate=ulisesmx.crt --embed-certs

    # Create a new context on the cluster
    kubectl config set-context ulisesmx --user=ulisesmx --cluster=minikube

    # Use the new context
    kubectl config use-context ulisesmx
    ```
