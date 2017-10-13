# Lab 4

## Exercises

This set of exercises will build on the kubernetes-101 lab, but we will have the opportunity to play with some more advanced features that the cloud providers give us such as External LoadBalancers and Elastic Block Storage and due to the fact we have more than one server in our cluster as opposed to the minikube examples.

### 1. Deployment

Lets get started with a deployment, in this example we are going to deploy a sample application that allows users to vote and see the current state of the results. There are several moving parts to this. A front end and a redis cluster that stores results in memory. Theres no persitant data in this at the moment. There is a service associated with the redis deployment to allow the slaves to find the master service using service discovery (dns in this case)

```bash
kubectl apply -f voting-deployment.yaml
```

### 2. Service Load Balancer

We played with services in kubernetes-101 and used a NodePort to gain access to a pod. Now we have a production cluster in AWS we can take advantage of the ```cloud-controller-manager``` and use it to provision an ELB for us. The external ELB will then be configured to connect to our instances (masters and nodes) on a NodePort, inside the k8s the ClusterIP then is aware for the pod Enpoints and traffic is sent to the correct containers over the k8s network. The diagram below is a conceptual overview of this.

![Load Balancer](/kubernetes-201/labs/img/aws-elb-k8s.png "Fig. 1")
(Figure 1: AWS and K8s ELB integration)

### 3. Scale the deployment

### 4. Logs and exec

### 5. Persistant Disks



### 6. Cordon / Uncordon

### 7. Trigger an autoscale event

- Lab 1: [Installing kops](/kubernetes-201/labs/00-install-kops.md)
- Lab 2: [Deploy a cluster](/kubernetes-201/labs/01-deploy-cluster.md)
- Lab 3: [Addons](/kubernetes-201/labs/02-addons.md)
- Lab 4: [Deploy a service](/kubernetes-201/labs/03-deploy-service.md)
- Lab 5: [Upgrade a cluster](/kubernetes-201/labs/04-upgrading.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
