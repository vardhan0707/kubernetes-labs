## Exercises

### Service Load Balancer

We played with services in kubernetes-101 and used a NodePort to gain access to a pod. Now we have a production cluster in AWS we can take advantage of the ```cloud-controller-manager``` and use it to provision an ELB for us. The external ELB will then be configured to connect to our instances (masters and nodes) on a NodePort, inside the k8s the ClusterIP then is aware for the pod Enpoints and traffic is sent to the correct containers over the k8s network. The diagram below is a conceptual overview of this.

![Load Balancer](/kubernetes-201/labs/img/aws-elb-k8s.png "Fig. 1")
(Figure 1: AWS and K8s ELB integration)




- Lab 1: [Installing kops](/kubernetes-201/labs/00-install-kops.md)
- Lab 2: [Deploy a cluster](/kubernetes-201/labs/01-deploy-cluster.md)
- Lab 3: [Addons](/kubernetes-201/labs/02-addons.md)
- Lab 4: [Deploy a service](/kubernetes-201/labs/03-deploy-service.md)
- Lab 5: [Upgrade a cluster](/kubernetes-201/labs/04-upgrading.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
