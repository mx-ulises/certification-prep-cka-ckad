<h1 align="center" style="border-bottom: none">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
    <br>
    Istio Topic 4: Traffic Shifting
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

These notes cover the topic on how we can do traffic shifting on different versions/subsets of services part of the Istio's service registry using `DestinationRule` and `VirtualService`. This allow routing traffic to different versions of the application for rollouts or A/B testing. Relevant documentations includes the following:

 * [Documentation](https://istio.io/latest/docs/) > [Tasks](https://istio.io/latest/docs/tasks/) > [Traffic Management](https://istio.io/latest/docs/tasks/traffic-management/) > [Traffic Shifting](https://istio.io/latest/docs/tasks/traffic-management/traffic-shifting/)
 * [Documentation](https://istio.io/latest/docs/) > [Reference](https://istio.io/latest/docs/reference/) > [Configuration](https://istio.io/latest/docs/reference/config/) > [Traffic Management](https://istio.io/latest/docs/reference/config/networking/) > [Destination Rule](https://istio.io/latest/docs/reference/config/networking/destination-rule/)
 * [Documentation](https://istio.io/latest/docs/) > [Reference](https://istio.io/latest/docs/reference/) > [Configuration](https://istio.io/latest/docs/reference/config/) > [Traffic Management](https://istio.io/latest/docs/reference/config/networking/) > [Virtual Service](https://istio.io/latest/docs/reference/config/networking/virtual-service/)


## Deploy Pre-Requisites

For this task, we will deploy a `Deployment` (`nginx-app-v1`) that has the `app` label (`nginx-app`) and the `version` label (`v1` and `v2`). When we hit the port `80` on these deployments, this will display a message with the `Pod` name (that includes the version as well):

```
Welcome to: nginx-app-v1-55cbcdd84-hvzph
```

Additionally, we create a `Service` with the selector `app=nginx-app`, meaning that the endpoints are all `Pod` in the deployment.

All these resources are created in a the `traffic-shifting` namespace that is created as well by this spec.

Apply the file [nginx-app.yaml](nginx-app.yaml) to deploy all the resources:

```
kubectl apply -f nginx-app.yaml
```

You should see following resources:

```
kubectl get all -n traffic-shifting

NAME                                READY   STATUS    RESTARTS   AGE
pod/nginx-app-v1-55cbcdd84-ccjf2    2/2     Running   0          17m
pod/nginx-app-v2-5b557788b7-h9wv7   2/2     Running   0          17m

NAME                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/nginx-app   ClusterIP   10.96.242.217   <none>        80/TCP    17m

NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-app-v1   1/1     1            1           17m
deployment.apps/nginx-app-v2   1/1     1            1           17m

NAME                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-app-v1-55cbcdd84    1         1         1       17m
replicaset.apps/nginx-app-v2-5b557788b7   1         1         1       17m
```

## Accessing the Service

You can access your service from another Pod. For this purpose, we create an auxiliar sleep nginx Pod:

```
kubectl run sleep --image=nginx -n traffic-shifting -- sleep 3600
```

And then you can run curl on it:

```
kubectl exec sleep -c sleep -n traffic-shifting -- curl nginx-app 2> /dev/null
# Welcome to: nginx-app-v1-55cbcdd84-ccjf2
```

## Defining `DestinationRule` with subsets

A `DestinationRule` is a resource that help to configure policies once traffic arrived to the service. Different configurations can be added here such as Load Balancing policies. In this case we are adding a rule to define `subsets` in the destination based on the labels of the `Pod` selected by the service.

In the file [nginx-app-dr.yaml](nginx-app-dr) we are indicating that the `nginx-app` service we previously created will have two subsets:
 * `v1` that will select the `Pod` with label `version=v1`
 * `v2` that will select the `Pod` with label `version=v2`

```
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: nginx-app-dr
  namespace: traffic-shifting
spec:
  host: nginx-app
  subsets:
  - name: v1
    labels:
      version: v1
  - name: v2
    labels:
      version: v2
```

As we apply this policy, we won't have any effect yet in the routing as we are just defining names for groups of `Pod` that the `Service` can recognize, but these `subsets` can be used later by the `VirtualService` to route the requests.

```
kubectl apply -f nginx-app-dr.yaml
```

## Defining `VirtualService` to inject Delay

Traffic shifting configurations happens here. We create following `VirtualService` that will default all traffic comming to `nginx-app` to the `Pod` in subsets `v1` or `v2` of service `nginx-app`, and we will weight `v1` with 90% of the traffic and `v2` with 10%. Note that the weight in all the destinations should always add to 100. This definiton is in file [nginx-app-vs-traffic-shifting.yaml](nginx-app-vs-traffic-shifting.yaml):

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: nginx-app-vs
  namespace: traffic-shifting
spec:
  hosts:
  - nginx-app
  http:
  - route:
    - destination:
        host: nginx-app
        subset: v1
      weight: 90
    - destination:
        host: nginx-app
        subset: v2
      weight: 10
```

We can apply and test our delay:

```
# Deploy the VirtualService with fault injection
kubectl apply -f nginx-app-vs-traffic-shifting.yaml

# Test connecting to the service several times:
kubectl exec sleep -c sleep -n traffic-shifting -- curl nginx-app 2> /dev/null

# Welcome to: nginx-app-v1-55cbcdd84-ccjf2    ---> We connect to v1 90% of the time
# Welcome to: nginx-app-v2-5b557788b7-h9wv7   ---> We connect to v2 10% of the time
```


<p align="center" style="border-bottom: none; margin-top: 50px;">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
</p>