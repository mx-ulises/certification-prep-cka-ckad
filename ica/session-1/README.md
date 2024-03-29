<h1 align="center" style="border-bottom: none">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
    <br>
    Mesh Week Session 1: Istio installation, upgrade & configuration
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

In this session of [Mesh Week (Session 1): Istio installation, upgrade & configuration](https://www.youtube.com/live/w_8Gg_jsAbU?si=AvxzBh-Bi4_NXq8B) it is cover the main points of how to install, upgrade and configure Istio, including using the CLI to install a basic Istio cluster, how to customize the installation with the istioOperator API as well as using the overlay to manage Istio settings.

## What is Istio?

Istio is a service mesh. A Service Mesh is an infrastructure layer that allow you to add to your application transparently security, observability, routing and traffic manager features. Istio inject a Proxy container in your pod than intercept all the inbound and outbound traffic.

There are two main components:

 * ***Data Plane***: Include all the Proxy containers in the pods
 * ***Control Plane***: The services that are resposible to recieve configuration and push it into the Data Plane Proxies.

 ## Configuration Profiles

During the Istio installation process, a IstioOperator API Custom Resource Definitions (CRD) is installed in your cluster. There are different configuration profiles that can be used:

 1. `default`: Production deplouments (istiod & ingress)
 1. `demo`: Demostration purposes (istiod & ingress & egress)
 1. `minimal`: Only control plane (allows configuring igresses separately)
 1. `remote`: For configuring remote clusters
 1. `empty`: Deploy nothing (used as base profile for customization)
 1. `preview`: Contains experimental features (not recommended for production)
 1. `ambient`: Ambient mesh profile (istiod, CNI, ztunnel)

All these profiles are included in YAML specs available in Istio's github repository.

To check the details of your current Istio configuration you can use following command:

```
istioctl profile dump
```

To check specific details, you can use the JSON path to the document as a parameter:

```
istioctl profile dump --config-path components.pilot
```

You can compare profiles with `diff`

```
istioctl profile diff default demo | less
```

To generate a manifest you can use:

```
istioctl manifest generate
```

You can compare manifests with `diff`

```
istioctl manifest diff manifest1.yaml manifest2.yaml
```

More details on the different profiles in the Official documentation for [Installation Configuration Profiles](https://istio.io/latest/docs/setup/additional-setup/config-profiles/)

## Using Istio CLI (istioctl)

The document [Install with Istioctl](https://istio.io/latest/docs/setup/install/istioctl/) describe how to install Istio with the command line. The command `istioctl` fully support the Istio Operator API.

Prerequisites:
 - [Download the appropiate Istio release](https://istio.io/latest/docs/setup/getting-started/#download)
 - [Meet specific platform requirements](https://istio.io/latest/docs/setup/platform-setup/)

### ⭐ Install

Steps:
 1. Install Istio using `istioctl` with default or custom configurations:
    ```
    # Will use the default Istio profile
    istioctl install

    # You can configure settings at installation time:
    istioctl install --set meshConfig.accessLogFile=/dev/stdout

    # Install with a different profile
    istioctl install --set profile=demo
    ```
 1. Check what is installed
    ```
    kubectl -n istio-system get deploy
    ```
 1. To get details of the istio operator you have installed you can use this command:
    ```
    kubectl -n istio-system get IstioOperator installed-state -o yaml | less
    ```
 1. You can verify the installation of istio with `verify-install` command:
    ```
    # Basic isntallation
    istioctl verify-install

    # Use a manifest to verify the installation
    istioctl verify-install -f manifest.yaml
    ```

**Note**: The `istioctl install -f <spec>` file can be used to update an existing Istio configuration with new settings provided in the `<spec>` file. You can generate the `<spec>` file of your current configuration using `istioctl profile dump`.

### ⭐ Unistall

To unistall Istio use:
```
istioctl unistall --purge
```

## UsingHelm Charts

When using Helm, you will be using same profiles as with Istio CLI. The document [Install with Helm](https://istio.io/latest/docs/setup/install/helm/) describe how to install Istio with Helm.

### ⭐ Install

Steps:
 1. Configure Helm repositories
    ```
    helm repo add istio https://istio-release.storage.googleapis.com/charts
    helm repo update
    ```
 1. Install using Helm charts:
    ```
    # Example:
    #   helm install <release> <chart> --namespace <namespace> --create-namespace

    helm install istio-base istio/base -n istio-system
    ```
    You can use  `--set <parameter>=<value>` and `--values <file>` to configure specific settings
 1. Deploy control plane (`istiod`)
    ```
    helm install istiod istio/istiod -n istio-system --wait
    ```
 1. (Optional) Install Istio Ingress
    ```
    kubectl create namespace istio-ingress
    helm install istio-ingress istio/gateway -n istio-ingress --wait
    ```
### ⭐ Update configuration

To update the configuration, you can use the previous steps ussing either `--set <parameter>=<value>` or `--values <file>` to change the configuration.

### ⭐ Uninstall

To uninstall Istio from your cluster, follow these steps:
 1. Get the installed charts in `istio-system` namespace:
    ```
    helm ls -n istio-system
    ```
 1. If installed, delete Istio gateway:
    ```
    helm delete istio-ingress -n istio-ingress
    kubectl delete namespace istio-ingress
    ```
 1. Unistall `istiod`, `istio-base` and namespace
    ```
    helm delete istiod -n istio-system
    helm delete istio-base -n istio-system
    kubectl delete namespace istio-system
    ```

<p align="center" style="border-bottom: none; margin-top: 50px;">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
</p>
