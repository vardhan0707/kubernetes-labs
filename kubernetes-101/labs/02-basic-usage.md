# Lab 3

## Basic tool usage

The aim of this lab is to get familiar with the basics. By the end you'll be able to create, find, modify and delete resources within your cluster.

## Exercises  

### Create/Use a namespace

Namespaces are a great way of separating logical workloads, so its a good starting point to learn how to handle them.

Start by listing your namespaces:

```
kubectl get ns
```

Now create your own namespace:

```
kubectl create ns <NAME>
```

You can validate that its been created by listing the namespaces again. You can now use the namespace when using other kubectl commands. For example the following command should return _"No resources found."_:

```
kubectl get pods -n <NAME>
```

But if you run the command against the kube-system namespace you'll see lots of pods returned:

```
kubectl get pods -n kube-system
```

**NOTE:** if you install kubectx you'll also have a command called kubens, this will allow you to change and set the default namespace that kubectl uses easily.

### Start a pod

Now we have our own workspace we can start services in this and quickly find the resources.

- add a service / ingress
- use a deployment
- delete a pods,services,deployments

## Exercises

- Lab 1: [Installing k8s tools](/kubernetes-101/labs/00-tools.md)
- Lab 2: [Install Minikube](/kubernetes-101/labs/01-minikube.md)
- Lab 3: [Basic tool usage](/kubernetes-101/labs/02-basic-usage.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
