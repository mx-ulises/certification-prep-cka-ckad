# Spec static analisys with Kubesec

To run kubesec to analize a Kubernetes spec file you can use this command using docker:

```
# docker run -i kubesec/kubesec:latest scan /dev/stdin < <kubernetes_spec_file>
docker run -i kubesec/kubesec:latest scan /dev/stdin < pod-spec.yaml
```
