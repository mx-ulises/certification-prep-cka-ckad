# Cluster Node Upgrades

## Master Nodes

Links: [Upgrading kubeadm clusters](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

Kubernetes clusters can be upgraded from one to another minor versions, but not skipping (1.23 to 1.25). In documentation it can be find as "Upgrading kubeadm clusters". The steps of the proccess are:

 1. Upgrade `kubeadm`
 1. Check available versions: `kubeadm upgrade plan`
 1. Upgrade control plane node: `kubeadm upgrade apply <version>`
 1. Drain control node: `kubectl drain controlnode --ignore-daemonsets`
 1. Upgrade and restart kubelet and kubectl
 1. Bring back the control node: `kubectl uncordon controlnode`
 1. Proceed with the rest of the nodes (same steps)

## Worker Nodes

Links: [Upgrading Linux nodes](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/upgrading-linux-nodes/)

 1. Upgrade `kubeadm`
 1. Check available versions: `kubeadm upgrade plan`
 1. Upgrade worker node: `kubeadm upgrade node`
 1. Drain control node: `kubectl drain controlnode --ignore-daemonsets`
 1. Upgrade and restart kubelet and kubectl
 1. Bring back the control node: `kubectl uncordon controlnode`
 1. Proceed with the rest of the nodes (same steps)