# 101 Introduction

Kubernetes 101 is designed to give you a quick insight into the components and simple operations of a k8s cluster. We'll also cover installing minikube and the kubectl cli on your local machine for testing and running the lab exercises locally to give you experiance with kubectl (the kubernetes command line interface)

## Cluster Components

### Overview

![Kubernetes Components](img/components.png "fig.1")

#### Masters

Masters run four (three before 1.6) main components. In production, you should run multiple master servers and a loadbalancer across them. Its normal to run at least 3 masters, this allows the ETCD component to achieve quorum and have election to pick a master node.

##### kube-api-server

kube-apiserver exposes the Kubernetes API. It is the front-end for the Kubernetes control plane and its where the ```kubectl``` command line connects into. In a HA setup this is the service you connect the loadbalancer to.

##### kube-controller-manager

kube-controller-manager runs controllers, which are the background threads that handle routine tasks in the cluster. Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.

These controllers include:

- Node Controller: Responsible for noticing and responding when nodes go down.
- Replication Controller: Responsible for maintaining the correct number of pods for every replication controller object in the system.
- Endpoints Controller: Populates the Endpoints object (that is, joins Services & Pods).
- Service Account & Token Controllers: Create default accounts and API access tokens for new namespaces.

##### cloud-controller-manager (new after 1.6)

cloud-controller-manager runs controllers that interact with the underlying cloud providers. The cloud-controller-manager binary is an alpha feature introduced in Kubernetes release 1.6.

cloud-controller-manager runs cloud-provider-specific controller loops only.

cloud-controller-manager allows cloud vendors code and the Kubernetes core to evolve independent of each other. In prior releases, the core Kubernetes code was dependent upon cloud-provider-specific code for functionality. In future releases, code specific to cloud vendors should be maintained by the cloud vendor themselves, and linked to cloud-controller-manager while running Kubernetes.

The following controllers have cloud provider dependencies:

- Node Controller: For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
- Route Controller: For setting up routes in the underlying cloud infrastructure
- Service Controller: For creating, updating and deleting cloud provider load balancers
- Volume Controller: For creating, attaching, and mounting volumes, and interacting with the cloud provider to orchestrate volumes

##### kube-scheduler

kube-scheduler watches newly created pods that have no node assigned, and selects a node for them to run on.

#### etcd

In A HA production setup you need at least 3 etcd servers. It's a key based store thats used to keep state data about whats happening in the cluster, and the k8s network.

#### nodes

##### kubelet

kubelet is the primary node agent. It watches for pods that have been assigned to its node (either by apiserver or via local configuration file) and:

- Mounts the pod’s required volumes.
- Downloads the pod’s secrets.
- Runs the pod’s containers via docker (or, experimentally other OCI run times).
- Periodically executes any requested container liveness probes.
- Reports the status of the pod back to the rest of the system, by creating a mirror pod if necessary.
- Reports the status of the node back to the rest of the system.

##### kube proxy

kube-proxy enables the Kubernetes service abstraction by maintaining network rules on the host and performing connection forwarding.


#### Networking

Kubernetes assumes that pods can communicate with other pods, regardless of which host they land on. We give every pod its own IP address so you do not need to explicitly create links between pods and you almost never need to deal with mapping container ports to host ports. This creates a clean, backwards-compatible model where pods can be treated much like VMs or physical hosts from the perspectives of port allocation, naming, service discovery, load balancing, application configuration, and migration.

##### pods

The pods normally exist on a /16 network within Kubernetes thats not externally routable. Each node will be assigned a /24 network, this allocation is stored in etcd and is used to help the other nodes route the traffic to the correct controller.

##### services

Services also have an internal network to Kubernetes. When you assign a service as type ```ClusterIP``` they are assigned an IP from a separate /24 network to the pod network. When you wish to expose the service to external users you use the type ```NodePort``` this uses kube-proxy and iptables to allow ingress to the Kubernetes network. If you assign a service as type ```LoadBalancer``` this is actually built up of a NodePort and ClusterIP and exists on every node, traffic to the pods is then forwarded over the network overlay.

##### CNI

By default Kubernetes uses KubeNet as its network model. However its possible to swap this out to a Container Network Interface (CNI) compatible alternative. A few options are:

- Project Calico
- Weave
- flanneld

and many more. Each option has its pro's and con's so take a good look a the one that suits you.

### Typical Deployment Diagram using KOPS and AWS

![KOPS Deployment](img/deployment.png "fig. 2")

Temrinology
- namespaces
- pods
- services
- deployments
- daemon and stateful sets

## Tasks

Installing k8s tools
- OSX
- Linux

Install Minikube

Basic tool usage
- create a namespace
- start a pod
- add a service / ingress
- use a deployment
- delete a pods,services,deployments
