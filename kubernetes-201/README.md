# 201 Introduction

## Overview

In this lab we are going to productionise a k8s cluster using the KOPS deployment tool. KOPS will provision a k8s cluster for us and set up the PKI used to allow nodes to communcate with the API. It'll also set up out local ```kubectl```. Most guides use KOPS in its default mode in this guide we'll dig a little deeper and deploy our cluster into a set of existing AWS infrastructure. The dafult behavious or KOPS is to create a new VPC,, subenets, security groups, NAT Gateways and other resources required. However in this lab we are going to use existing an VPC that has subnets configured and NAT Gateways deployed.

### KOPS

kops helps you create, destroy, upgrade and maintain production-grade, highly available, Kubernetes clusters from the command line. AWS (Amazon Web Services) is currently officially supported, with GCE and VMware vSphere in alpha and other platforms planned.

#### Features

- Automates the provisioning of Kubernetes clusters in (AWS)
- Deploys Highly Available (HA) Kubernetes Masters
- Supports upgrading from kube-up
- Built on a state-sync model for dry-runs and automatic idempotency
- Ability to generate configuration files for AWS CloudFormation and Terraform Terraform configuration
- Supports custom Kubernetes add-ons
- Command line autocompletion
- Manifest Based API Configuration
- Community supported!

[Offical page here](https://github.com/kubernetes/kops)

### Reference Architecture

![AWS KOPS](./img/aws-kops.png "Figure. 4")
(Figure 4: AWS reference deployment Architecture)

## Exercises

- Lab 1: [Installing kops](/kubernetes-201/labs/00-install-kops.md)
- Lab 2: [Deploy a cluster](/kubernetes-201/labs/01-deploy-cluster.md)
- Lab 3: [Addons](/kubernetes-201/labs/02-addons.md)
- Lab 4: [Deploy a service](/kubernetes-201/labs/03-deploy-service.md)
- Lab 5: [Upgrade a cluster](/kubernetes-201/labs/04-upgrading.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
