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

In this lesson we will learn how to use storage in Kubernetes. We will review what storage options exist in Kubernetes. We will review storage through `Pod` volumes, `PersistentVolumes` (PV) and `PersistentVolumeClaim` (PVCs). Then we will review Storage provisioners and `StorageClass` to prosivion storage on demand. Finally we will use `ConfigMap` and `Secrets` as storage.

## Storage Options

In Kubernetes, `Pod` can create storage volumes that can be used by the containers inside the `Pod`, but cannot be used outside it. To decouple from the `Pod`, we use `PersistentVolume` (PV). PVs are site specific, and they can be used by `Pod` using `PersistentVolumeClaim` (PVC) that can be mounted and configured as a Pod volume. PVC will check if there are PV available and it is going to be bounded to the PVC. Aditionally, we have `StorageClass` that provision storage if no PV is available and a PVC is requesting storage. A `StorageClass` is an API object that represents a provisioner of storage and help to provision storage on demand.

### ⭐ Pod Volumes

This is the simples way to use storage. Pod volumes are part of the `Pod` specification and have storage reference hard coded in the `Pod` manifest. It doesn't allow flexible storage allocation. Pod Volumes can be used for any storage type, as well as `Secrets` or `ConfigMaps` can be accessed by mounting them as volumes.

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-volume
  name: pod-volume
spec:
  containers:
  - image: nginx
    name: pod-volume
    resources: {}
    # Start: Mount Volume
    volumeMounts:
    - mountPath: /data
      name: test
    # End: Mount Volume
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  # Start: Volume Definition
  volumes:
  - name: test
    emptyDir: {}
  # End: Volume Definition
status: {}
```

### ⭐ Configuring Persistent Volumes and Persistent Volume Claims

`PersistentVolume` (PV) are an API resoruce that represents specific storage. PV can be created manually, or automatically using `StorageClass` and storage provisioners. Pods don't connect to PVs directly, but indirectly using `PersistentVolumeClaim` (PVC).

```
# Start: Persistent Volume
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-demo
  labels:
    type: local
spec:
  storageClassName: demo # Storage class is important to be identified by PVC
  hostPath:  # The storage type is a hostPath in path "/mydata"
    path: "/mydata"
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
# End: Persistent Volume
```

PVC helps `Pod` to abt type of storage that is provided at a specific site. It is required that both PV and PVC has the same storage class, so they can be bound together.

```
# Start: Persistent Volume Claim
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-demo
spec:
  storageClassName: demo # It is important that storage class is same as in PV
  volumeName: pvc-demo
  resources:
    requests:
      storage: 1Gi
  accessModes:
    - ReadWriteOnce
# End: Persistent Volume Claim
```

To set up a `Pod` to use a PVC we need to use the fields `persistentVolumeClaim` and `claimName` in the `volume` section:

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-pvc-volume
  name: pod-pvc-volume
spec:
  containers:
  - image: nginx
    name: pod-pvc-volume
    resources: {}
    # Start: Mount Volume
    volumeMounts:
    - mountPath: /data
      name: pv-storage
    # End: Mount Volume
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  # Start: Volume Definition
  volumes:
  - name: pv-storage
    persistentVolumeClaim:
      claimName: pvc-demo
  # End: Volume Definition
status: {}
```

### ⭐ Storage Class

`StorageClass` is an API resource that allows storage to be automatically provisioned. It can be used as a property that connects PVC and PV without using an actual `StorageClass` resource.  Multiple `StorageClass` resources can co-exist in the same clustoer to provide access to different types of storage. For automatic working selection of `StorageClass`, it should be set as the default:

```
kubectl patch storageclass gold -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

To enable automatic provisioning, `StorageClass` needs a backing storage provisioner. In the PV and PVC definiton, a `storageClass` property can be set to connect to a specific `StorageClass`. If the `storageClass` property is not set, then the PVC will get storage from the detault `StorageClass`. If no default `StorageClass` is set, the PVC will get stuck in pending status.

The stirage orivusuiber wirjs wutg `StorageClass` to automatically provide storage. It runs as a pod in the cluster, provided with the access control configured trought `Roles`, `RolesBinding` and `ServiceAccounts`. Once this is operational, there is no need to create manually PV of this `StorageClass`.

To create a storage provisioner, it needs access permisions to the API. All resources are usually created trhought Helm.

### ⭐ Config Maps and Secrets

A `ConfigMap` is an API resource used to store site-specific data. A `Secret` is a base64 encoded `ConfigMap`. It stores environment variables, startup parameters or config files. When a configuration file is used in a `ConfigMap` or `Secret`, it is mounted as a volume to provide access to its content. These files should have a limit of 1MB.

To create a `ConfigMap` from a file, use:

```
kubectl create cm <configMapName> --from-file=<filePath>
```

In the `Pod`, we can load the `ConfigMap` as a volume in the following way:

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: sample-cm
  name: sample-cm
spec:
  containers:
  - image: nginx
    name: sample-cm
    resources: {}
    # Start: Mount Volume
    volumeMounts:
    - mountPath: /data
      name: cm-volume
    # End: Mount Volume
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  # Start: Volume Definition from CM
  volumes:
  - name: cm-volume
    configMap:
      name: cm-test
  # End: Volume Definition from CM
status: {}
```

<p align="center" style="border-bottom: none; margin-top: 50px;">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
</p>
