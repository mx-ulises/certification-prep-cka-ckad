# Verify Binary Sha:

Links: [Kubernetes github releases](https://github.com/kubernetes/kubernetes/releases/tag/)

  1. Get the Server version:
       ```
       kubectl version | grep Server
       # Server Version: v1.30.0
       ```
  1. Go to the github repository kubernetes, in the release section of the version you are looking at: [Version 1.30.0](https://github.com/kubernetes/kubernetes/releases/tag/v1.30.0).
  1. Go to CHANGELOG and download the amd version of the binaries (you can use `wget`).
  1. Get the sha of the kube-apiserver binary that you downloaded:
       ```
       sha512sum kubernetes/server/bin/kube-apiserver
       shasum -a 512 kubernetes/server/bin/kube-apiserver
       ```
  1. Now, in master node, get the binaries from the container:
       ```
       # Use docker/crictl: 1. Get conatiner id and then copy to the directory
       crictl ps | grep kube-apiserver
       crictl cp <container_id>:/ container

       # Use kubectl
       kubectl -n kube-system cp kube-apiserver-minikube:/ container
       ```
  1. Once you have the content of the container, check the sha:
       ```
       sha512sum container/usr/local/bin/kube-apiserver
       shasum -a 512 container/usr/local/bin/kube-apiserver
       ```
  1. Compare the two sha are the same

If you need to verify other binaries, you can use the command `find` and `grep` to get the paths to the files.
