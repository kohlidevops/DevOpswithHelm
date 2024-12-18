# HELM : K8s Packaging Manager

➢ WithOut HELM-

  ○ Teams rely on Kubernetes YAML files to configure Kubernetes workload.

  ○ YAML files specify everything needed for deploying containers.

  ○ Set up a new Kubernetes workload, you need to create a YAML file for that workload.

  ○ All YAML files, you creating for Kubernetes are Static. They don’t receive parameters dynamically.

➢ Consistency is the major issue with Hand Crafted YAMLs or Deployments.

➢ Kubernetes Can’t maintain the revision History.

## What is Helm?

➢ HELM is a Package Manager running atop Kubernetes.

➢ HELM Package Manager simplified Micro services management on K8s.

![image](https://github.com/user-attachments/assets/8064fd64-c5b7-4f32-bdfa-984eb551c2dc)

![image](https://github.com/user-attachments/assets/dd39e81a-3709-4c36-b3c7-fa1d1ed0d128)

If you want to install nginx ingress, then you can execute the following command, to run:

```
helm install my-nginx nginx-stable/nginx-ingress --namespace=webapp
```

## After Helm

➢ With HELM-

  ○ Instead of writing separate YAML files for each application manually, user can simply create a Helm chart and let Helm deploy the application to the cluster for you.

  ○ Helm chart can be customized when deploying it on different Kubernetes clusters.

![image](https://github.com/user-attachments/assets/c09dc1c0-7b98-4a1d-b853-edeebe6c274c)

➢ Benefits of HELM-

  ○ Greatly improved productivity using Single Click Deployment.
  
  ○ Reduced complexity of deployments

  ○ More reproducible deployments and results
  
  ○ Ability to leverage Kubernetes with a single CLI command
  
  ○ Easier rolling back to previous versions of an app

## Helm Charts and Repos

➢ HELM Charts and Repos-

  ○ Helm uses a packaging format called charts.

  ○ Chart is a collection of files that describe a related set of Kubernetes resources.

  ○ User can perform HELM CLI commands on HELM Charts

You can refer

https://helm.sh/

https://bitnami.com/stacks/helm

## Types of Installation

1. Local Installation
2. Cloud Installation using MiniKube
3. Cloud Installation using K8's HA Environment
4. K8's Managed Service Setup (EKS, AKS)

## Install Kubernetes using Minikube

You can refer the following link, to run:

https://github.com/kohlidevops/DevOpsWithKubernetes/blob/main/1%20-%20Minikube/Install-minikube.md

## Install Helm

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm version
```

You can refer the following link:

https://helm.sh/docs/intro/install/

## Work with Repos

Chart - It is a Helm package and it contains all of the resource necessary to run an application, tool or service inside of the k8 cluster.

Repository - It is the place where charts can be collected and shared.

Release - It is an instance of chart running in k8 cluster. One chart can often be installed many times into the same cluster and each time it is installed, a new release is created.

## Commands

1. To list the helm repo

```
helm repo list
```

2. To add the repo

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add brigade https://brigadecore.github.io/charts
```

3. To remove the heml repo

```
helm repo remove brigade
```

4. To search repo

```
helm search repo mysql
helm search repo nginx
```

![image](https://github.com/user-attachments/assets/763a0beb-b9ba-43fe-98fa-520b4415c931)

5. To execute services using Helm

You can use this link to refer

https://artifacthub.io/

```
helm install my-redis bitnami/redis --version 17.3.9

To get your password run:

    export REDIS_PASSWORD=$(kubectl get secret --namespace default my-redis -o jsonpath="{.data.redis-password}" | base64 -d)

To connect to your Redis&reg; server:

1. Run a Redis&reg; pod that you can use as a client:

   #kubectl run --namespace default redis-client --restart='Never'  --env REDIS_PASSWORD=$REDIS_PASSWORD  --image docker.io/bitnami/redis:7.0.5-debian-11-r15 --command -- sleep infinity

   Use the following command to attach to the pod:

   #kubectl exec --tty -i redis-client \
   --namespace default -- bash

2. Connect using the Redis&reg; CLI:
   #REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h my-redis-master
  >SET mykeys "Hello Latchu"
  >Get mykeys
  >exit
  
   #REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h my-redis-replicas
  >Get mykeys

(optional)
To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace default svc/my-redis-master 6379:6379 &
    REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h 127.0.0.1 -p 6379
```

6. To list the helm

```
helm list
```

![image](https://github.com/user-attachments/assets/dc7eca8c-a704-4baf-8122-cd5cec76127b)

7. To check the redis

```
kubectl get pods
```

![image](https://github.com/user-attachments/assets/8cf84795-93eb-49f7-867e-9fe9131b014c)

There are multiple pods are running - If you deploy this using manifest yaml, then you need more yaml files to run this. But with Helm, we can achieve this using single command.

8. To install the redis in different namespace

If you list the helm, it will show the redis which is created in default namespace. If you try to create same repo name with different redis version, then you cant. But it is possible in different namespace.

```
helm list
helm install my-redis bitnami/redis --version 17.3.11
kubectl create namespace redis
helm install -n redis my-redis bitnami/redis --version 17.3.11
helm list
helm list -n redis
```

![image](https://github.com/user-attachments/assets/592a230f-988e-4cde-97c5-55fd6c3a5e8b)

![image](https://github.com/user-attachments/assets/91e037eb-48c8-42de-ba54-a384cb4d5894)

9. To check the status of helm deployment

```
helm status my-redis -n redis
```

![image](https://github.com/user-attachments/assets/f4c7f7b3-a1ba-4abd-94e8-8c75c1df7e42)

10. To delete the helm deployment

```
helm delete my-redis
helm delete my-redis -n redis
```

## How to provide custom values to Helm chart

You can refer this link to get the link to helm deploy maria db

https://artifacthub.io/

To deploy a maria db usimg helm

```
helm install my-mariadb bitnami/mariadb --version 11.4.0
helm list
kubectl get pods
helm status my-mariadb

Administrator credentials:

  Username: root
  Password : $(kubectl get secret --namespace default my-mariadb -o jsonpath="{.data.mariadb-root-password}" | base64 -d)

To execute in the minikube cluster and keep the password in some place

#kubectl get secret --namespace default my-mariadb -o jsonpath="{.data.mariadb-root-password}" | base64 -d
ymRm9Orw9B

To connect to your database:

  1. Run a pod that you can use as a client:

      #kubectl run my-mariadb-client --rm --tty -i --restart='Never' --image  docker.io/bitnami/mariadb:10.6.11-debian-11-r0 --namespace default --command -- bash

  2. To connect to primary service (read/write):

      mysql -h my-mariadb.default.svc.cluster.local -uroot -p my_database
      >(provide the password)
      >exit

```

To create a namespace

```
kubectl create namespace database
```

To create a MariaDB_CustomValues

```
nano MariaDb_CustomValues.yaml

https://github.com/kohlidevops/DevOpswithHelm/blob/main/1%20-%20Package-and-Deploy-on-Kubernetes/MariaDb_CustomValues.yaml

helm install -n database --values /root/MariaDb_CustomValues.yaml my-mariadb bitnami/mariadb --version 11.4.0
kubectl get pods -n database
helm status my-mariadb -n database

Administrator credentials:

To connect to your database:

  1. Run a pod that you can use as a client:

      #kubectl run my-mariadb-client --rm --tty -i --restart='Never' --image  docker.io/bitnami/mariadb:10.6.11-debian-11-r0 --namespace database --command -- bash

  2. To connect to primary service (read/write):

      #mysql -h my-mariadb.database.svc.cluster.local -uroot -p latchu-helm
      >(providerootpassword - RootPass1234)
      >exit
      #mysql -h my-mariadb.database.svc.cluster.local -ucustom_usr -p latchu-helm
      >(provideuserpassword - Test1234)
      >exit
```

![image](https://github.com/user-attachments/assets/d2bafbd5-cc26-47fb-8df0-484ae24769d8)

## How to upgarde the services using Helm?





