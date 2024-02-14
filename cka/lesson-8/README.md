<h1 align="center" style="border-bottom: none">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
    <br>
    CKA Lesson 4: Managing Storage
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

In this lesson we will lear how to influence the scheduling process. First we will review what is the scheduling proccess, then we check how to set Node preferences on scheduling a `Pod` with Affinity and Anti-Affinity. We will also review taints and tolerations and how to manage quotas (resource limits and requests).

## Scheduling Process

Kube-scheduler takes care of finding a node to schedule new Pods. We can set requirements on the nodes such as:
 - Resource requirements
 - Afinity and Anti-Afinity
 - Taints and tolerations

The scheduler first finds feasible nodes and then scores them; it then picks the node with the highest score. Once the node is found, scheduler notifies the API server. All this automatic. Finally, the Kubelet will instruct the CRI to fetch the image of the required container and create the containers. 

## Node prefences

### ⭐ Node Selector

The `nodeSelector` field in the `pod.spec` specifies a key-value pair that match a label which tis set on nodes that are elegible to run the `Pod`. You can label a `Pod` with this command:

```
kubectl label node <nodeName> <key>=<value>
```

Use in the `nodeSelector: <key>=<value>` to match the `Pod` on that specific node. 

### ⭐ Node Name

You can also use `nodeName` fiel in the `pod.spec` so the `Pod` will always run in that specific node. This is not recommended, because if the node is not available, the `Pod` will never run.

## Affinity and Anti-Affinity

Affinity and Anti-Affinity are advanced scheduler rules. There are two types of affinities:

 - Node Affinity: Contrain a node that can receive a Pod by matching labels of these nodes. A `Pod` that has a node affinity label only will be scheduled to nodes with matching label.
 - Inter-Pod Affinity: Constrain a nodes to recieve Pods by matching labels of existing Pods already runnin on that node. A Pod that has Pod affinity label only will be scheduled to nodes running with the matchin label.

Anti-Affinity only exists in Inter-Pod affinity.

To use Affinity, two different statements can be used:

 - `requiredDuringSchedulingIgnoredDuringExecution`: This requires the node to meet the constraint that is defined.
 - `preferredDuringSchedulingIngnoredDuringExecution`: It will be ignored if it cannot be fulfilled.

At the momment, affinity only applies at scheduling and cannot be used to change where Pods are already running.

Affinity rules go beyon labels. `matchExpression` is used to define a label key, an operator as well as optionally one or more label values.

### ⭐ Topology Key

A `topologyKey` refers to a label that exists on nodes, and tipically has a format  containing a slash. Using `topologyKeys` allows the `Pod` only to be assigned to host that matches the key. We can use zones where the korloads are implemented. If no matching `topologyKey` is found on the hsot, the specified key will be ignored in the affinity.

## Taints and Tolerations

Taints are applied to a node to mark that the node should not accept any Pod that doesn't tolerate the taint. Tolerations are applied to `Pods` and allow (but do not require) them to be scheduled on nodes with matching Taints - so they are an exception to taints applied.

Where Affinities are used on `Pod` to attract them to specific nodes, Taints allow the node to repel some `Pod`. Tains and Tolerations are used to ensure `Pod` are not scheduled on innapropiate nodes, and thus make sure that dedicated nodes can be configured for dedicated tasks.

There are three types of Taint:
 - `NoSchedule`: No new Pods can be scheduled in the node
 - `PreferNoSchedule`: Does not schedule new `Pod`, unless there is no other option
 - `NoExecute`: Migrate all `Pod` awat of the node

If the `Pod` has a toleration, it would ignore the Taint.

There are some ways to set taints:
 - Control plane nodes automatically get taints taht won't schedule user `Pod`
 - When `kubectl drain` and `kubectl cordon` are used, a taint is applied on the target node
 - Taints can be set automatically by the cluster when a critical condition arise, such as running out of disk space. Some automatically created taints are:
   - memory-pressure
   - disk-pressure
   - pod-pressure
   - unschedulable
   - network-unavailable
 - Administrator can use kubectl to set taints:

```
# Add a taint
kubectl taint nodes <nodename> <key>=<value>:<taintType>

# Remove a taint
kubectl taint nodes <nodename> <key>=<value>:<taintType>-
```

A toleration allow a `Pod` to run on a node with a specific taint, a toleration can be used. These tolerations are used by core Kubernetes `Pod` on the control plane nodes. When creating tains and tolerations, a key and a value are defined to allow for more specific access.

While deffining the toleration, the `Pod` needs a key, operator and value. Some commonds operators are `Equal` and `Exists`. 

## Limits and Quotas

### ⭐ Quotas

The `Quota` is an API object that limits the total resources available in a `Namespace`. If a `Namespace` is configured with `Quota`, applications in that `Namespace` must be configured with resource settings in `Pod` in `pod.spec.containers.resources`. The goal is to define maximum resources that can be consumed within a `Namespace` by all the applications.

To create `Quota` in a namespace use the following commands:

```
kubectl create quota <quotaName> --hard pods=<podLimit>,cpu=<cpuLimit>,memory=<memoryLimit> namespace <namespaceName>
```

To set resource `requests` and `limits` in a resource (`Pod`, `Deployment`, etc...) use:

```
kubectl set resources <resourceTypeName> <resourceName> --requests cpu=<cpuLimit>,memory=<memoryLimit> --limits cpu=<cpuLimit>,memory=<memoryLimit> -n <namespaceName>
```
### ⭐ Limit Range

The `LimitRange` is and API object that limits resource usage per container or `Pod` in a `Namespace`. The goal is to set a default restriction for each application in a `Namespace`. It has three options:

 - `type`: specifies wheter it applies to `Pod` or container
 - `defaultRequest`: the default resources the application will request
 - `default`: the maximum resoures that the application can use

To create limit range, the best is to use an example from documentation such as [Limit Range](https://kubernetes.io/docs/concepts/policy/limit-range/):

```
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
  - default: # this section defines default limits
      cpu: 500m
    defaultRequest: # this section defines default requests
      cpu: 500m
    max: # max and min define the limit range
      cpu: "1"
    min:
      cpu: 100m
    type: Pod
```

If you change the `requests` to some number above the `default` limit, then you will have some errors, and you will need to fix your `Pod` specification to override the `default` limit.

<p align="center" style="border-bottom: none; margin-top: 50px;">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
</p>