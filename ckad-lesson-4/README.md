<h1 align="center" style="border-bottom: none">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
    <br>
    CKAD Lesson 4: Creating a Lab Environment
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

In this lesson, we will set up a lab environment. We will review some deployment options. Since this course is focused on Minikube, this chapter also concentrates on how to install it and ensure it is working. Afterward, we will go over how to install Kubectl and enable Kubectl completion.

## Kubernetes Deployment Options

Kubernetes can be deployed in different ways:

 - Run it in public clouds (GCP, AWS, Azure, etc.) as a managed service (GKE, EKS, AKS, etc.) or configure your own cluster on virtual machines.
 - On-premises in a data center where you own the servers.
 - Use compact Kubernetes distributions for testing, such as Minikube or Kind (Kubernetes in Docker).

Usually, in public clouds and on-premises environments, you have multiple servers/VMs running your Kubernetes controllers and worker nodes. In public clouds, the nodes are managed.

For compact Kubernetes distributions, you have a single node that serves as both the controller and worker node.

## Minikube

Minikube is an all-in-one Kubernetes solution where both controller and worker functionality are typically offered on a single node, but it can also be configured on multiple nodes.

It is recommended that you use a virtual machine so that it doesn't impact your host operating system.

### ⭐ Install in Ubuntu
 1. Download the Minikube setup script from [Sander van Vugt's ckad's Github repository](https://github.com/sandervanvugt/ckad/tree/master):
 ```
 curl -LO https://raw.githubusercontent.com/sandervanvugt/ckad/master/minikube-docker-setup.sh
 ```
 2. Run the script. This script will install Minikube and Kubectl as well. Note that you might be asked to provide sudo's password.
 ```
 ./minikube-docker-setup.sh
 ```

### ⭐ Install in Windows and Mac

Follow instructions in [the minikube's official documentation](https://minikube.sigs.k8s.io/docs/start/).


### ⭐ Verify your Minikube setup

Run the following command to start a Minikube cluster:

```
minikube start --vm-driver=docker
```

Run the following command to check the status of your Minikube setup:

```
minikube status

# Output:

# minikube
# type: Control Plane
# host: Running
# kubelet: Running
# apiserver: Running
# kubeconfig: Configured
```

This is a list of other useful minikube commands:

| Minikube Command | Description |
|-|-|
| `minikube status` | Displays the current status of the Minikube cluster. |
| `minikube start` | Starts the Minikube virtual machine and Kubernetes cluster. |
| `minikube stop` | Stops the running Minikube virtual machine and Kubernetes cluster. |
| `minikube ssh` | Logs into the Minikube virtual machine via SSH. |
| `minikube dashboard` | Opens the Kubernetes dashboard in a web browser if it's installed. |
| `minikube delete` | Deletes the Minikube cluster and its associated resources. |
| `minikube ip` | Retrieves the IP address of the Minikube cluster. |
| `minikube status` | Displays the current status of the Minikube cluster. |
| `minikube version` | Shows the installed Minikube version. |

## Install Kubectl and completion

### ⭐ Install in Kubectl

Depending in your operating system, follow instructions to install Kubectl:

 - ***Ubuntu***: Kubectl will be already installed with the minikube setup script above.
 - ***Windows***: Follow instructions in [the official's Kubernetes' kubectl documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/).
 - ***Mac***: Follow instructions in [the official's Kubernetes' kubectl documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/).

### ⭐ Install in completion

Kubectl completion helps to autocomplete commands for Kubectl. Run the following command to get instructions on how to install completion for Kubectl.

```
kubectl completion -h
```

<p align="center" style="border-bottom: none; margin-top: 50px;">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
</p>