

## Exercises

Deployments of extra services

### Deploying services to the cluster

The cluster should have some standard services deployed onto it. These include:

- Monitoring via heapster
- The Dashboard
- Helm
- Autoscaler

#### Heapster:

```bash
kubectl create -f https://raw.githubusercontent.com/kubernetes/kops/master/addons/monitoring-standalone/v1.6.0.yaml
```

#### DashBoard:

```bash
kubectl create -f https://raw.githubusercontent.com/kubernetes/kops/master/addons/kubernetes-dashboard/v1.6.3.yaml
```

To get your dashboard password run:

```bash
kubectl config view --minify
```

#### Helm:
Install helm on your system and run:

```bash
helm init
```

This deploys tiller on your cluster and you can check its running by issuing the command:

```bash
kubectl get po -n kube-system
```

#### Autoscaler:
Run the following script and change the following MIN_NODES, MAX_NODES and GROUP_NAME:

```
CLOUD_PROVIDER=aws
IMAGE=gcr.io/google_containers/cluster-autoscaler:v0.6.0
MIN_NODES=3
MAX_NODES=15
AWS_REGION=eu-central-1
GROUP_NAME="nodes.k8s.example.com"
SSL_CERT_PATH="/etc/ssl/certs/ca-certificates.crt" # (/etc/ssl/certs for gce)

addon=cluster-autoscaler.yml
wget -O ${addon} https://raw.githubusercontent.com/kubernetes/kops/master/addons/cluster-autoscaler/v1.6.0.yaml

sed -i -e "s@{{CLOUD_PROVIDER}}@${CLOUD_PROVIDER}@g" "${addon}"
sed -i -e "s@{{IMAGE}}@${IMAGE}@g" "${addon}"
sed -i -e "s@{{MIN_NODES}}@${MIN_NODES}@g" "${addon}"
sed -i -e "s@{{MAX_NODES}}@${MAX_NODES}@g" "${addon}"
sed -i -e "s@{{GROUP_NAME}}@${GROUP_NAME}@g" "${addon}"
sed -i -e "s@{{AWS_REGION}}@${AWS_REGION}@g" "${addon}"
sed -i -e "s@{{SSL_CERT_PATH}}@${SSL_CERT_PATH}@g" "${addon}"

kubectl apply -f ${addon}
```

The make the script executable and then run it:

```bash
chmod +x cluster-autoscaler.sh
./cluster-autoscaler.sh
```

## Exercises

- Lab 1: [Installing kops](/kubernetes-201/labs/00-install-kops.md)
- Lab 2: [Deploy a cluster](/kubernetes-201/labs/01-deploy-cluster.md)
- Lab 3: [Addons](/kubernetes-201/labs/02-addons.md)
- Lab 4: [Deploy a service](/kubernetes-201/labs/03-deploy-service.md)
- Lab 5: [Upgrade a cluster](/kubernetes-201/labs/04-upgrading.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
