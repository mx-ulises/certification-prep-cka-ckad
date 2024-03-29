<h1 align="center" style="border-bottom: none">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
    <br>
    Mesh Week Session 2: Istio Traffic Management
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

In this session of [Mesh Week (Session 2): Istio Traffic management](https://www.youtube.com/live/Q-l1z3ejc8Q?si=2vHAM1lvbcVXvDHC) it is covered the following points:

  * Controlling network traffic flows within a service mesh
  * Configuring sidecar injection
  * Usage of the `Gateway` resource to configure ingress and egress traffic
  * Understanding how to use `ServiceEntry` resource for adding entries to the internal service registry
  * Define traffic policies using `DestinationRule`
  * Configure traffic mirroring capabilities

## Sidecar Injection

When a service is onboarded on the mesh, Istio recognize it and it augment the manifest of the service to add the Istio sidecar container. This can happen by labeling a `Namespace`, so all `Pod` created in that `Namespace` is injected with the sidecar containers. [Documentation](https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/).

`kubectl label ns <namespace_name> istio-inject=enabled`

To manually add the sidecar container to a compatible manifest (`Pod`, `Deployment`, etc...) with `istioctl kube-inject` and `kubectl apply`:

`kubectl apply -f <(istioctl kube-inject -f <resource.yaml>)`

To configure sidecar injection behaviour and override global default configuration, there is a API resource `Sidecar` in the [documentation](https://istio.io/latest/docs/reference/config/networking/sidecar/).

## Routing

The [`Gateway`](https://istio.io/latest/docs/reference/config/networking/gateway/) is the entry point of the traffic: It affects traffic it is incomming in the Mesh. In configures what traffic it is going to let enter based on the hostname, ports, protocols, tls configuration, etc...

The [`VirtualService`](https://istio.io/latest/docs/reference/config/networking/virtual-service/) will check host matching and will set subsets of how to distribute the traffic to different destinations,

The [`DestinationRule`](https://istio.io/latest/docs/reference/config/networking/destination-rule/) will matches subsets to workloads based on a workload selector and versions to find services and define load balancing policies and TLS configuration within that workload.

[Ingress `Gateway`](https://istio.io/latest/docs/tasks/traffic-management/ingress/ingress-control/) provide examples on how to configure Ingress to existing workload.

[Traffic Shifting](https://istio.io/latest/docs/tasks/traffic-management/traffic-shifting/) provide examples on how to configure `VirtualService` to weight how much traffic go to different payloads.

[Egress `Gateway`](https://istio.io/latest/docs/tasks/traffic-management/egress/egress-gateway/)  provide examples to configure the traffic that go outside the cluster.

## Traffic Mirror

You can send exactly the same traffic to different subsets by modifying a `VirtualService` with the mirror configurations. Details provided in the [Traffic Management task for Mirroring documentation](https://istio.io/latest/docs/tasks/traffic-management/mirroring/).

## Adding external resources to the Cluster

The `ServiceEntry` helps to define an object outside of the cluster or the mesh into the mesh. [Accessubg External Services documentation](https://istio.io/latest/docs/tasks/traffic-management/egress/egress-control/) provides steps on how to configure that.



<p align="center" style="border-bottom: none; margin-top: 50px;">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
</p>
