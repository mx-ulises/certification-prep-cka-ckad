# Get an use the digest of an image

Image digest is an image identifier in the container registry that is unique per binary image. It helps to use exactly the same binaries when the label is override with a new version (such as latest or other tags if the registry is compromised).

To get the image digest from a running pod use this:

```
# kubectl get pod <pod_name> -o yaml | grep imageID
kubectl get pod pod-with-image-digest -o yaml | grep imageID
```

When creating a new pod, you can specify the images of your containers to be an specific digest, so it will pick the same binary always:

```
apiVersion: v1
kind: Pod
...
spec:
  containers:
  - name: pod-with-image-digest
    #image: nginx # This is a tag based image
    image: nginx@sha256:a484819eb60211f5299034ac80f6a681b06f89e65866ce91f356ed7c72af059c # This is a digest based image
...
```
