---
layout: default
title: K3s 
parent: Kubernetes 
nav_order: 2
---
# K3s
### Why K3s?
I have chosen K3s for the majority of my Kubernetes needs. Not only do I see it getting documented the most frequently, but it's absurdly easy to set up in HA. It's also suitable as a base for either manually mangaged K8s or for underneath Rancher.
### Installation
In this example we're assuming 3 nodes with the purpose of being a manually managed Kubernetes cluster.
On your first node (doesn't necessarily have to be your "master" node, I just choose the one with the lowest hostname index), run the following command.
```shell
curl -sfL https://get.k3s.io | K3S_TOKEN=$YOUR_SECRET sh -s - server --cluster-init
```
On the second and third nodes, run the following command (with the same $YOUR_SECRET as the first command we ran).
```shell
curl -sfL https://get.k3s.io | K3S_TOKEN=$YOUR_Secret sh -s - server --server https://<ip or hostname of server1>:6443
```
If you need to install a specific version of K3s (ie, for Rancher), you can add in the flag: ```INSTALL_K3S_VERSION=$K3S_VERSION```
Check the Rancher version matrix to see what type of Kubernetes is necessary. At the time of writing, Rancher v2.7.1 requires < v1.25. An example would be.
```shell
curl -sfL https://get.k3s.io | K3S_TOKEN=$YOUR_Secret INSTALL_K3S_VERSION=v1.24.10+k3s1 sh -s - server --server https://<ip or hostname of server1>:6443
```

You can retrieve the KubeConfig from ```/etc/rancher/k3s/k3s.yaml``` and test your cluster with ```kubectl get nodes```.
