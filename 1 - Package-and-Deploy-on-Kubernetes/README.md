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

