<h1 align="center" style="border-bottom: none">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
    <br>
    CKA Lesson 2: Creating Kubernetes Cluster with kubeadm
</h1>
<h3 align="center" style="border-bottom: none">
    Notes by: <a href="https://github.com/mx-ulises" target="_blank">Ulises Martinez</a>
</h3>
<hr />

<p align="center">
    Auto-generated by the <a href="https://github.com/WhitneyLampkin/devscriber" target="_blank">DevScriber</a> command-line tool.
</p>

<div align="center">

![GitHub language count](https://img.shields.io/github/languages/count/mx-ulises/certification-prep-cka-ckad?label=Languages)
![GitHub contributors](https://img.shields.io/github/contributors/mx-ulises/certification-prep-cka-ckad?label=Contributors&color=yellow)
![GitHub repo size](https://img.shields.io/github/repo-size/mx-ulises/certification-prep-cka-ckad?label=Repo%20Size&color=teal)
![GitHub repo file count (file type)](https://img.shields.io/github/directory-file-count/mx-ulises/certification-prep-cka-ckad?label=Files&color=purple)

</div>

## Introduction

This lesson covers how to create clusters with `kubeadm`. We will review the cluster node requirements and network requirements, what happens when we initialize the cluster, and how to install the cluster using `kubeadm init`. Additionally, we'll discuss adding nodes to the cluster and configuring the Kubernetes client.

## Node and Network Requirements

### ⭐ Node Requirements

To install a Kubernetes cluster using `kubeadm`, we need at least two nodes with the following requirements:

- A recent version of Ubuntu or CentOS
- 2 GiB RAM or more
- 2 CPUs or more on the control-plane node
- Network connectivity between the nodes

We also need to install the following:
- A container runtime
- The Kubernetes tools

The container runtime allows you to run containers in the operating system. Some runtimes include containerd, CRI-O, Docker Engine, etc. In the CKA exam, it is not required to install a container runtime as it will be already installed.

The Kubernetes tools include the following:
- `kubeadm`: Used to install and manage a Kubernetes cluster
- `kubelet`: The core Kubernetes service that starts all Pods
- `kubectl`: The interface that allows you to run and manage applications in Kubernetes

You don't need to install the Kubernetes tools in the CKA exam.

### ⭐ Network Requirements

Different types of network communication are used in Kubernetes:
- Node communication: Handled by the physical network
- External-to-Service communication: Managed by Kubernetes Service resources
- Pod-to-Service communication: Managed by Kubernetes Services
- Pod-to-Pod communication: Managed by the network plugin
- Container-to-Container: Managed within the containers of a Pod

To create the software-defined Pod network, a network add-on is needed. This is provided by the Kubernetes ecosystem and is not included in Vanilla Kubernetes. Kubernetes provides a Container Network Interface (CNI), which is a generic interface that allows different plugins to be used. Depending on the plugin you use, different features are included, such as network policies, IPv6 support, or Role-Based Access Control (RBAC).

The most common network add-ons are:

- **Calico**: The most common one that supports all relevant features
- **Flannel**: Generic and used in the past, doesn't support Network Policy
- **Multus**: This plugin can work with multiple plugins and is the default in OpenShift
- **Weave**: Another common add-on that does support common features

## Cluster Initialization and `kubeadm` Utilization

### ⭐ Initialization Phases

When you run `kubeadm init`, multiple phases are executed:

1. **Preflight**: Ensure that all conditions are met, and core container images are downloaded.
1. **Certificates**: Generate a self-signed Kubernetes CA, and create related certificates for `apiserver`, `etcd`, and `proxy`.
1. **kubeconfig**: Generate configuration files for core Kubernetes services.
1. **kubelet-start**: Start the `kubelet`.
1. **control-plane**: Create and start static `Pod` manifests for `apiserver`, control manager, and `scheduler`.
1. **etcd**: Create and start a static `Pod` manifest for `etcd`.
1. **upload-config**: Create `ConfigMaps` for `ClusterConfiguration` and `kubelet` component config.
1. **upload-certs**: Upload all certificates to `/etc/kubernetes/pki`.
1. **mark-control-plane**: Mark the node as a control plane.
1. **bootstrap-token**: Generate the token that can be used to join other nodes.
1. **kubelet-finalize**: Finalize the kubelet settings.
1. **add-on**: Install `coredns` and `kube-proxy` add-ons.

### ⭐ Steps to Install Cluster

1. Install the container runtime.
1. Install the Kubernetes tools.
1. Install the cluster: `sudo kubeadm init`.
1. Set up the client (commands provided in step 3):
   - `mkdir ~/.kube`
   - `sudo cp -i /etc/kubernetes/admin.conf ~/.kube/config`
   - `sudo chown $(id -u):$(id -g) ~/.kube/config`
1. Install the network add-on:
   - `kubectl create -f <network-add-on-manifest>`
1. Join other nodes (commands provided in step 3):
   - `sudo kubeadm join <join-token>`

### ⭐ kubeadm Usage

While using `kubeadm init`, different arguments can be used to further define the cluster:

- `--apiserver-advertise-address`: The IP address on which the API server is listening.
- `--config`: Contains a configuration file for additional configurations.
- `--dry-run`: Performs a dry-run before actually installing for real.
- `--pod-network-cidr`: Sets the CIDR used for the Pod network.
- `--service-cidr`: Sets the service CIDR to something other than `10.96.0.0/12`.
- `reset`: It will make a best effort to revert changes made to this host by `kubeadm init` or `kubeadm join`.

In most scenarios, running `kubeadm init` should be enough.

### ⭐ Add Nodes

When a Kubernetes cluster is initialized, a join token is generated. Use this join token to add other hosts using `sudo kubeadm join` from the host you want to join.

If the token is lost or expired, you can create a new one:

```bash
sudo kubeadm token create --print-join-command
```

### ⭐ Configure the Kubernetes Client

Another important part is configuring the client. You can copy the `/etc/kubernetes/admin.conf` file to `~/.kube/config` to provide admin access to the Kubernetes cluster. Alternatively, you can use `export KUBECONFIG=/etc/kubernetes/admin.conf`.

For more advanced user configuration, users must be created and provided with authorizations using RBAC.

The context groups client access parameters using a convenient name. By selecting a context, a specific group of parameters can be accessed. Each context has the following parameters:

- Cluster: The cluster you want to connect to. The cluster is defined by its endpoint and the certificate and its keys. You can use the following command to define a cluster:

  ```bash
  kubectl config --kubeconfig=~/.kube/config set-cluster <cluster-name> --server=<server-address> --certificate-authority=<cert>
  ```

- Namespace: The namespace is the default namespace defined in this context.
- User: The user is the user account, defined by its X.509 certificates or other. You can define a user as follows:

  ```bash
  kubectl config --kubeconfig=~/.kube/config set-credentials <user-name> --client-certificate=<cert> --client-key=<key>
  ```

To view the context, use `kubectl config view`, and to use a specific context, use `kubectl use-context`. To create a new context, use:

```bash
kubectl config set-context <context-name> --cluster=<cluster-name> --namespace=<namespace> --user=<user-name>
```

<p align="center" style="border-bottom: none; margin-top: 50px;">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
</p>