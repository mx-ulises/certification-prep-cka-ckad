# Scan Images Vulnerability

Link: [Aquasec's Trivy](https://hub.docker.com/r/aquasec/trivy/)

To scan vulnerabilities with an image:

```
# docker run aquasec/trivy image <image_name>
docker run aquasec/trivy image python:3.4-alpine
```
