# Lab 1

## Exercise

### Installing KOPS

KOPS has a dependency on you having the AWSCLI installed and configured and kubectl.

### Install awscli

### Setup a S3 bucket

1: Setup an S3 Bucket

I used the aws cli to create an S3 Bucket:

aws s3 mb s3://my-kops-configs

**Note:** you can use the bucket to store more than one cluster state

## Exercises

- Lab 1: [Installing kops](/kubernetes-201/labs/00-install-kops.md)
- Lab 2: [Deploy a cluster](/kubernetes-201/labs/01-deploy-cluster.md)
- Lab 3: [Addons](/kubernetes-201/labs/02-addons.md)
- Lab 4: [Deploy a service](/kubernetes-201/labs/03-deploy-service.md)
- Lab 5: [Upgrade a cluster](/kubernetes-201/labs/04-upgrading.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
