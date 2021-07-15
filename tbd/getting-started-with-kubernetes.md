# Getting started with kubernetes

## Kubernetes Home Training System

Kubenetes mini editions perfect as your first playground[  
K3s: Lightweight Kubernetes](https://k3s.io/) or [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

Install Virtualbox from [https://www.virtualbox.org/](https://www.virtualbox.org/)

Install minikube [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

```text
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```

Review installation has been successfully. minikube start, minikube status or minikube stop.

```text
$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

```text
$ minikube stop
✋  Stopping node "minikube"  ...
🛑  1 nodes stopped.
```

Install kubectl [https://kubernetes.io/docs/tasks/tools/](https://kubernetes.io/docs/tasks/tools/) I use homebrew 

```text
$ brew install kubectl 
```

**kubectl** allows us to manage local Kubernetes clusters like the minikube cluster, or remote clusters deployed in the cloud. It is generally installed before installing and starting minikube, but it can also be installed after the cluster bootstrapping step.

To access ths cluster **kubectl** needs the master node endpoint and appropriate credentials to be able to securely interact with the API server running on the master node. While starting Minikube, the startup process creates, by default, a configuration file, **config**, inside the **.kube** directory \(often referred to as the [**kubeconfig**](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/)\), which resides in the user's **home** directory. The configuration file has all the connection details required by **kubectl**. By default, the **kubectl** binary parses this file to find the master node's connection endpoint, along with credentials. Multiple **kubeconfig** files can be configured with a single **kubectl** client. To look at the connection details, we can either display the content of the **~/.kube/config** file \(on Linux\) or run the following command: 

```text
$ kubectl config view
```

### Minikube dashboard

Minikube embarks a web UI. Start it by typing

```text
 $ minikube dashboard
```

This command starts the UI and automatically opens a browser window to it.

We can also proxy it to the machine where kubectl is running with the following command:

```text
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

##  Minikube addons

list the available addons with

```text
$ minikube addons list
|-----------------------------|----------|--------------|-----------------------|
|         ADDON NAME          | PROFILE  |    STATUS    |      MAINTAINER       |
|-----------------------------|----------|--------------|-----------------------|
| ambassador                  | minikube | disabled     | unknown (third-party) |
| auto-pause                  | minikube | disabled     | google                |
| csi-hostpath-driver         | minikube | disabled     | kubernetes            |
| dashboard                   | minikube | enabled ✅   | kubernetes            |
| default-storageclass        | minikube | enabled ✅   | kubernetes            |
| efk                         | minikube | disabled     | unknown (third-party) |
| freshpod                    | minikube | disabled     | google                |
| gcp-auth                    | minikube | disabled     | google                |
| gvisor                      | minikube | disabled     | google                |
| helm-tiller                 | minikube | disabled     | unknown (third-party) |
| ingress                     | minikube | disabled     | unknown (third-party) |
| ingress-dns                 | minikube | disabled     | unknown (third-party) |
| istio                       | minikube | disabled     | unknown (third-party) |
| istio-provisioner           | minikube | disabled     | unknown (third-party) |
| kubevirt                    | minikube | disabled     | unknown (third-party) |
| logviewer                   | minikube | disabled     | google                |
| metallb                     | minikube | disabled     | unknown (third-party) |
| metrics-server              | minikube | disabled     | kubernetes            |
| nvidia-driver-installer     | minikube | disabled     | google                |
| nvidia-gpu-device-plugin    | minikube | disabled     | unknown (third-party) |
| olm                         | minikube | disabled     | unknown (third-party) |
| pod-security-policy         | minikube | disabled     | unknown (third-party) |
| registry                    | minikube | disabled     | google                |
| registry-aliases            | minikube | disabled     | unknown (third-party) |
| registry-creds              | minikube | disabled     | unknown (third-party) |
| storage-provisioner         | minikube | enabled ✅   | kubernetes            |
| storage-provisioner-gluster | minikube | disabled     | unknown (third-party) |
| volumesnapshots             | minikube | disabled     | kubernetes            |
|-----------------------------|----------|--------------|-----------------------|
```



## Kubernetes ecosystem

**etcd:** a key-value store.n Only the API Server is able to communicate with the etcd data store.

**etcdctl:** command line tool for the key-value store.

**kubeadm:** bootstrapping tool

**worker node:** running environment \(container\) for client applications. Requires a **container runtime**

**kubelet:** agent running on each node and communicates with the control plane components from the master node. In order to connect to interchangeable container runtimes, kubelet uses a **shim** application which provides a clear abstraction layer between kubelet and the container runtime. The CRI implements two services: **ImageService** and **RuntimeService**.

**kube-proxy** is the network agent which runs on each node responsible for dynamic updates and maintenance of all networking rules on the node.

**pod**: groups depending worker nodes

**cluster**: groups pods

Kubernetes can be installed using different cluster configurations. Installation types are:

* * * **All-in-One Single-Node Installation** In this setup, all the master and worker components are installed and running on a single-node. While it is useful for learning, development, and testing, it should not be used in production. Minikube is an installation tool originally aimed at single-node cluster installations.
    * **Single-Master and Multi-Worker Installation** In this setup, we have a single-master node running a stacked etcd instance. Multiple worker nodes can be managed by the master node.
    * **Single-Master with Single-Node etcd, and Multi-Worker Installation** In this setup, we have a single-master node with an external etcd instance. Multiple worker nodes can be managed by the master node.
    * **Multi-Master and Multi-Worker Installation** In this setup, we have multiple master nodes configured for High-Availability \(HA\), with each master node running a stacked etcd instance. The etcd instances are also configured in an HA etcd cluster and, multiple worker nodes can be managed by the HA masters.
    * **Multi-Master with Multi-Node etcd, and Multi-Worker Installation** In this setup, we have multiple master nodes configured in HA mode, with each master node paired with an external etcd instance. The external etcd instances are also configured in an HA etcd cluster, and multiple worker nodes can be managed by the HA masters. This is the most advanced cluster configuration recommended for production environments. 

As the Kubernetes cluster's complexity grows, so does its hardware and resources requirements. While we can deploy Kubernetes on a single host for learning, development, and possibly testing purposes, the community recommends multi-host environments that support High-Availability control plane setups and multiple worker nodes for client workload. 

\*\*\*\*

