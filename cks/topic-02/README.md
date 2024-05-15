# Commands to create TLS Secret

Create Cert and Key

```
openssl genrsa -out key.pem 2048

openssl req -new -key key.pem -out csr.pem

openssl x509 -req -days 365 -in csr.pem -signkey key.pem -out cert.pem
```

Create Secret

```
kubectl create secret tls  -n ingress nginx-app-v1  --cert=pki/cert.pem --key=pki/key.pem
```

Links: [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
