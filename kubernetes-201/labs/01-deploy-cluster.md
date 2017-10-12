# Lab 2

# Exercise build a cluster

Run KOPS create

The follow command will create all the files required for the deployment. It wont actually create the cluster unless you add --yes to the end. However we want to tweak the install further so we wont add this flag.

```bash
kops create cluster \
--admin-access 172.31.0.0/12 \
--api-loadbalancer-type internal \
--cloud aws \
--networking calico \
--topology private \
--zones=${ZONES} \
--encrypt-etcd-storage \
--kubernetes-version ${VERSION} \
--dns private ${FQDN} \
--dns-zone ${TLD} \
--node-size m4.large \
--node-count=3 \
--node-volume-size 100 \
--master-zones=${MASTER-ZONES} \
--master-size m4.large \
--master-volume-size 100 \
--vpc=${VPC_ID} \
--state={$S3_BUCKET}
```

These are the values you will need to edit:

zones: This defines where your nodes get deployed to in their ASG
example:
**--zones=eu-central-1a,eu-central-1b,eu-central-1c**

kubeneretes-version: Self explanatory
example:
**--kubernetes-version 1.7.5**

dns: This specifies the FQDN of your cluster and also specifies that the DNS zone is Private
example:
**--dns private kops.contain.io**

dns-zone: the TLD of the zone
example:
**--dns-zone contain.io**

master-zones: These are you AWS AZ's if you specify three you'll get a master in each AZ for HA.
example:
**--master-zones=eu-central-1a,eu-central-1b,eu-central-1c**

vpc: Set this to your target VPC ID
example:
**--vpc=vpc-e5fFGHfrjh**

state: This is the S3 bucket you just created
example:
**--state=s3://my-kops-configs**

Once this command runs it will place files in S3 that are then used by the S3 command in the future to make further edits. You can of course tweak other values to customise the deployment.

Now run:

```bash
kops edit cluster
```

Then to finalise changes run:

```bash
kops update cluster --yes
```

## Exercises

- Lab 1: [Installing kops](/kubernetes-201/labs/00-install-kops.md)
- Lab 2: [Deploy a cluster](/kubernetes-201/labs/01-deploy-cluster.md)
- Lab 3: [Addons](/kubernetes-201/labs/02-addons.md)
- Lab 4: [Deploy a service](/kubernetes-201/labs/03-deploy-service.md)
- Lab 5: [Upgrade a cluster](/kubernetes-201/labs/04-upgrading.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
