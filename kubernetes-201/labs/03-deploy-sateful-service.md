# Lab 4b

## Exercises

This set of exercises will build on the kubernetes-101 lab and your knowledge of ```kubectl```, but we will have the opportunity to play with some more advanced features that the cloud providers give us such as External LoadBalancers and Elastic Block Storage. The work load will also be distributed around your cluster due the fact we have multiple nodes.

## Deploy a Stateful application

In this lab we are going to create a WordPress deployment with persistent storage. This is because we want to keep stateful information such as the data for MySQL and any files you upload to the CMS.

### Deploying MySQL

In this deployment we are going to create a service to allow the webserver to connect to MySQL using service discovery and a persistent volume for storing the contents of the database. Lets get going by creating a password for MySQL:

```bash
kubectl create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD
```

This secret is going to be consumed by your deployment to set the password for mysql meaning you dont't have to store passwords in your yaml directly.

Now lets create the deployment file named `mysql-deployment.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  ports:
    - port: 3306
  selector:
    app: wordpress
    tier: mysql
  clusterIP: None
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
  labels:
    app: wordpress
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  selector:
    matchLabels:
      app: wordpress
      tier: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: wordpress
        tier: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-pass
              key: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
```
After running:

```bash
kubectl create -f mysql-deployment.yaml
```

you should be able to run:

```bash
kubectl get po,svc,pv,pvc
```

This will show you something like the following:

```bash
NAME                                  READY     STATUS              RESTARTS   AGE
po/wordpress-mysql-2917821887-b0l6w   0/1       ContainerCreating   0          23s

NAME                  TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
svc/wordpress-mysql   ClusterIP   None         <none>        3306/TCP   23s

NAME                                          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM                     STORAGECLASS   REASON    AGE
pv/pvc-92325b42-b4ba-11e7-9f3f-0aa38f659e70   20Gi       RWO            Delete           Bound     ric-test/mysql-pv-claim   gp2                      22s

NAME                 STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc/mysql-pv-claim   Bound     pvc-92325b42-b4ba-11e7-9f3f-0aa38f659e70   20Gi       RWO            gp2            23s
```



## Exercises

- Lab 1: [Installing kops](/kubernetes-201/labs/00-install-kops.md)
- Lab 2: [Deploy a cluster](/kubernetes-201/labs/01-deploy-cluster.md)
- Lab 3: [Addons](/kubernetes-201/labs/02-addons.md)
- Lab 4: [Deploy a Stateless Application](/kubernetes-201/labs/03-deploy-service.md) | [Deploy a Stateful Application](/kubernetes-201/labs/03-deploy-stateful-service.md)
- Lab 5: [Upgrade a cluster](/kubernetes-201/labs/04-upgrading.md)

##### Labs : [kubernetes-101](/kubernetes-101/) | [kubernetes-201](/kubernetes-201/) | [kubernetes-301](/kubernetes-301/)
