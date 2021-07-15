# Getting started with kubernetes

Kubenertes mini editions perfect as your first playground[  
K3s: Lightweight Kubernetes](https://k3s.io/) or [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

**etcd:** a key-value store.n Only the API Server is able to communicate with the etcd data store.

**etcdctl:** command line tool for the key-value store.

**kubeadm:** bootstrapping tool

**worker node:** running environment \(container\) for client applications. Requires a **container runtime**

**kubelet:** agent running on each node and communicates with the control plane components from the master node. In order to connect to interchangeable container runtimes, kubelet uses a **shim** application which provides a clear abstraction layer between kubelet and the container runtime. The CRI implements two services: **ImageService** and **RuntimeService**.

**kube-proxy** is the network agent which runs on each node responsible for dynamic updates and maintenance of all networking rules on the node.

**pod**: groups depending worker nodes

**cluster**: groups pods

\*\*\*\*

