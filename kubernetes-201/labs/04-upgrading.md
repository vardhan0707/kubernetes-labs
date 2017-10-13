# Lab 5

## Exercises

kops allows you to easily upgrade the version of kubernetes and the underlaying AMI (ec2 instance). 

### 1. Upgrading Kubernetes



Manual updates:

kops has a new feature that drains nodes and validates their status before upgrading the next node. This is a very useful feature and ensure stability in your cluster whilst you are upgrading. To enable this feature run the following:

```bash
export KOPS_FEATURE_FLAGS="+DrainAndValidateRollingUpdate"
kops edit cluster --name=${CLUSTER_NAME} --state=${S3_Bucket}
```
Find and set the KubernetesVersion to the target version (e.g. v1.7.8)

```bash
kops update cluster --name=${CLUSTER_NAME} --state=${S3_Bucket}  # to preview changes
kops update cluster --name=${CLUSTER_NAME} --state=${S3_Bucket} --yes
kops rolling-update cluster --name=${CLUSTER_NAME} --state=${S3_Bucket} --yes
```

### 2. Upgrading the Ec2 Instances/AMI

As we rolled the cluster with the latest version of the kops debian image its pretty hard to do this update, however we can swap out debian and use coreOS instead. We'll first need to get the AMI-id of the latest coreOS image. This handy bash function can be used to get the ID for any region, add it to your .bash_profile / .bashrc and restart your terminal session:

```bash
coreos() {
    if [ "$1" == "" ]; then
        echo "please specify a regions (eg: eu-west-1)"
    else
        aws ec2 describe-images --region=$1 --owner=595879546273 --filters "Name=virtualization-type,Values=hvm" "Name=name,Values=CoreOS-stable*" --query 'sort_by(Images,&CreationDate)[-1].{id:ImageId}'
    fi
}
```

Now when you run the following command you will get the latest AMI-id back that you can use with kops:

```bash
coreos eu-west-1
```

Enable the validate and drain feature for rolling updates:

```bash
export KOPS_FEATURE_FLAGS="+DrainAndValidateRollingUpdate"
```

List your instanceGroups as we'll need to upgrade them all:

```bash
kops validate cluster --name=${CLUSTER_NAME} --state=${S3_Bucket}
```

Now update each instance group you have, for example:

```bash
kops edit ig master-eu-west-1a --name=${CLUSTER_NAME} --state=${S3_Bucket}
kops edit ig master-eu-west-1b --name=${CLUSTER_NAME} --state=${S3_Bucket}
kops edit ig master-eu-west-1c --name=${CLUSTER_NAME} --state=${S3_Bucket}
kops edit ig nodes --name=${CLUSTER_NAME} --state=${S3_Bucket}
```

change the AMI id in each of the above, for the one you generated with the ```coreos``` command.

Now commit the changes to the cluster:

```bash
kops update cluster --name=${CLUSTER_NAME} --state=${S3_Bucket}  # to preview changes
kops update cluster --name=${CLUSTER_NAME} --state=${S3_Bucket} --yes
kops rolling-update cluster --name=${CLUSTER_NAME} --state=${S3_Bucket} --yes
```

This will start the upgrade process and once finsihed you will see the nodes are running coreOS and not debian as before. you can see this by looking at the output of:

```bash
kubectl get no -o wide
```

- Lab 1: [Installing kops](/kubernetes-201/labs/00-install-kops.md)
- Lab 2: [Deploy a cluster](/kubernetes-201/labs/01-deploy-cluster.md)
- Lab 3: [Addons](/kubernetes-201/labs/02-addons.md)
- Lab 4: [Deploy a service](/kubernetes-201/labs/03-deploy-service.md)
- Lab 5: [Upgrade a cluster](/kubernetes-201/labs/04-upgrading.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
