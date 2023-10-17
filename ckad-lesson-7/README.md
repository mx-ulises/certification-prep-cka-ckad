<h1 align="center" style="border-bottom: none">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
    <br>
    CKAD Lesson 7: Managing Deployments
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

In this lesson, we will review Deployments and DaemonSets to scale from Pods. We will also explore topics such as updates, rollbacks, scalability, and autoscaling. Additionally, we will cover how to manage labels, selectors, and annotations.

## Deployments

Deployments are the standard way to run applications rather than using naked Pods. They offer the following benefits:

- The scalability of Deployments allows us to run multiple instances (Pods) of the same application to handle workload spikes and recover from Pod failures.
- Updates and update strategies to ensure zero downtime during updates.

Deployments protect Pods by automatically restarting them when they fail.

Deployments include configurations such as labels to identify the Pods that belong to the Deployment. Other Deployment configurations comprise the number of Pod replicas and their status, the specifications of the Pods in the Deployment, and events related to scaling.

If you delete a Pod managed by a Deployment, Kubernetes will restore the Pod to maintain the specified number of replicas. This is different from naked Pods, as if you delete a naked Pod, it won't come back. To change the number of Pods in a Deployment, you need to modify the Deployment's specification.

Deployments manage Pods via ReplicaSets, with the ReplicaSet responsible for handling scalability. It's important not to use the ReplicaSet directly since it's a managed resource of the Deployment.

Deployments enable zero-downtime application updates, particularly when you need to change container image versions or set new parameters for the deployment. When an update is applied, a new ReplicaSet with updated properties is created. After a successful update, the old ReplicaSet is no longer used and can be deleted, depending on your `deployment.spec.revisionHistoryLimit` configuration (the default is 10). The `updateStrategy` property dictates how updates are handled, with options for rolling updates or replacement.

<p align="center" style="border-bottom: none; margin-top: 50px;">
    <a href="https://github.com/mx-ulises/certification-prep-cka-ckad" target="_blank">
        <img alt="" src="https://github.com/mx-ulises/certification-prep-cka-ckad/blob/main/assets/notes-logo.png?raw=true" style="border-radius: 50%; height: 100px;">
    </a>
</p>