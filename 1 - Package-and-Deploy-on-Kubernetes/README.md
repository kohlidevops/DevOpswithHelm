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

**To list the helm deployment**

```
helm list -A
```

![image](https://github.com/user-attachments/assets/d8dabf9d-3c76-4182-a7be-a27f0e41e079)

**To check the helm deployment status which is deployed in database namespace**

```
helm status my-mariadb -n database
```

**To upgarde the helm deployment with same mariadb version**

```
helm repo update
//do some changes in MariaDb_CustomValues.yaml like
//password: "Test12345"
//username: "custom_usr1"
helm upgrade -n database --values /root/MariaDb_CustomValues.yaml my-mariadb bitnami/mariadb --version 11.4.0
kubectl get pods -n database
helm list -A
```

We could see that chart revision has been changed

![image](https://github.com/user-attachments/assets/178c9111-96d7-4cf0-978c-5d6bc5b71b99)

**To upgarde the helm deployment with different mariadb version**

```
helm upgrade -n database --values /root/MariaDb_CustomValues.yaml my-mariadb bitnami/mariadb --version 11.3.5
kubectl get pods -n database
helm list -A
```

![image](https://github.com/user-attachments/assets/d5bfce64-60e4-44df-ac25-5b67bd59a19e)

We could see that Mariadb version and Revision has been changed.

## How Helm maintain the Release records?

To list the secrets of default namespace and check how the release has been tracked.

Then delete the mariadb deployment which one is available in default namespace and again check the history of secrets

```
kubectl get secrets
helm list -A
helm uninstall my-mariadb
kubectl get secrets
```

![image](https://github.com/user-attachments/assets/9928a79a-4041-499a-8eef-083ed78fdf97)

The history is gone.

To delete the mariabdb deployment which one is available in database namespace and again check the history of secrets and it should be available.

```
kubectl get secrets
helm list -A
helm uninstall --keep-history my-mariadb -n database
kubectl get secrets -n database
```

## How to validate the resource before helm deployment?

With the help of dry-run command you can validate the resources before helm deployment. Usually Helm will validate the yaml before sending to k8 API server. However, this command will help us to understand the deployment.

```
helm install -n database --values /root/MariaDb_CustomValues.yaml my-mariadb bitnami/mariadb --version 11.4.0 --dry-run
```

You could see that status should be Pending, Not deployed. As well this dry-run command will generate the yaml file (But we cant use and deploy this yaml) and it shown in output.

![image](https://github.com/user-attachments/assets/f0926e21-28d5-4976-8236-3bd4b8c6bd26)

## How to generate the K8's deployable YAML using Helm?

You can go with template command, and use the following command, to run:

```
helm template -n database --values /root/MariaDb_CustomValues.yaml my-mariadb bitnami/mariadb --version 11.4.0
```

![image](https://github.com/user-attachments/assets/05c84f9b-1d14-4a80-a891-bdeaaf397d4d)

## How to get the details of Helm deployment Releases?

**To install the mariadb with version-11.3.4**

```
helm install -n database --values /root/MariaDb_CustomValues.yaml my-mariadb1 bitnami/mariadb --version 11.3.4
helm list -A
kubectl get pods -n database
kubectl get secrets -n database
```

![image](https://github.com/user-attachments/assets/daeb687a-0238-43d7-a90d-932259cea6a4)

**To upgarde the mariadb properties using Helm**

Just update the password in MariaDb_CustomValues.yaml

```
helm upgrade -n database --values /root/MariaDb_CustomValues.yaml my-mariadb1 bitnami/mariadb --version 11.3.4
helm list -A
kubectl get pods -n database
kubectl get secrets -n database
```

![image](https://github.com/user-attachments/assets/fb177049-e7dc-49d6-828a-f754d1777500)

The revision has been changed now 1 to 2.

To upgrade the mariadb version-11.3.5 using Helm

```
helm upgrade -n database --values /root/MariaDb_CustomValues.yaml my-mariadb1 bitnami/mariadb --version 11.3.5
helm list -A
kubectl get pods -n database
kubectl get secrets -n database
```

Now also revision has been changed in secrets and helm list

![image](https://github.com/user-attachments/assets/8f1bcb37-ec2a-4f88-9e90-6ce1dea0b3d8)

**To view the secrets as encoded format**

```
kubectl get secrets -n database sh.helm.release.v1.my-mariadb1.v3 -o yaml
```

![image](https://github.com/user-attachments/assets/df811966-f269-4712-af3a-dfa275edb2b3)

## How to get the details of deployed deployment?

**To list the helm and get the actual release notes of the deployment**

```
helm list -A
helm get notes my-mariadb1 -n database
```

**To get the values of the deployment**

```
helm list -A
helm get values my-mariadb1 -n database
```

![image](https://github.com/user-attachments/assets/9cd5be18-18a3-4ce5-94fc-f059dc6f1d45)

**To get the values of the particular revision**

```
helm list -A
helm get values my-mariadb1 -n database --revision 1
```

![image](https://github.com/user-attachments/assets/0334e662-db34-43c2-b26a-d1669967cc5c)

**To get the all values(manifest) of particular revision**

```
helm list -A
helm get manifest my-mariadb1 -n database --revision 2
```

## How to Rollback Application using the Helm?

**To list the helm and list the history of my deployment**

```
helm lits -A
helm history my-mariadb1 -n database
```

![image](https://github.com/user-attachments/assets/048abfb4-f776-4e43-9208-0d3869453c65)

**To rollback the application**

```
helm list -A
helm rollback my-mariadb1 1 -n database
helm list -A
helm history my-mariadb1 -n database
helm get values my-mariadb1 -n database --revision 4
```

![image](https://github.com/user-attachments/assets/99721023-ca0a-4656-8ce7-f1943edd9bcd)

To uninstall the deployment and keep the history

```
helm list -A
helm uninstall -n database my-mariadb1 --keep-history
helm list -A
kubectl get secrets -n database
helm history my-mariadb1 -n database
```

![image](https://github.com/user-attachments/assets/7cf2b290-9096-4759-bb5d-7c401dc2a7aa)

**To re-install the application using revision number**

```
helm list -A
helm rollback my-mariadb1 3 -n database
helm list -A
helm history my-mariadb1 -n database
helm get values my-mariadb1 -n database --revision 5
```

![image](https://github.com/user-attachments/assets/0867f640-e0c1-4cc3-9282-0a3791b7acf2)

## Wait Helm Deployment for successful Installation

**To install the mysql with wait command**

Default helm wait timeout is 5m, However you can override this values using timeout command

```
helm install my-mysql bitnami/mysql --version 9.4.4 --wait --timeout 20m
kubectl get pods
helm list -A
```

**To upgrade the mysql version**

```
helm list -A
helm upgrade my-mysql bitnami/mysql --version 9.4.2 --wait --timeout 20m
helm list -A
```

![image](https://github.com/user-attachments/assets/9318086c-b40b-4ccd-9816-f3f118230c72)

![image](https://github.com/user-attachments/assets/ef034287-9e9c-4439-96f3-7fc145398ab0)

**To rollback the mysql which is previously successful state**

```
helm list -A
helm upgrade my-mysql bitnami/mysql --version 9.4.2 --set image.pullPolicy="latchu" --atomic
helm history my-mysql
```

![image](https://github.com/user-attachments/assets/1f7759fe-5037-4e7f-ad65-4517dcbdc64e)

